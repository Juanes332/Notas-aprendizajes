
2026-04-12 18:15

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 


# Abstraction

## 1. Introducción

La abstracción es el proceso de **ocultar los detalles complejos** y mostrar solo las características esenciales de un objeto. Es como leer el manual de un carro sin necesidad de entender la ingeniería interna del motor.

### Objetivos de la abstracción

- [+] Simplificar la complejidad
- [+] Mostrar solo lo necesario
- [+] Definir contratos claros
- [+] Permitir implementación flexible

---

## 2. Qué es la Abstracción

### 2.1 Analogía: Conducir un carro

```
┌────────────────────────────────────────────────────┐
│              CONDUCCIÓN DE UN CARRO                │
├────────────────────────────────────────────────────┤
│                                                    │
│  Lo que VE el conductor:                           │
│  ┌─────────────────────────────────────┐           │
│  │  ● Volante                           │          │
│  │  ● Pedales (acelerar/frenar)        │           │
│  │  ● Palanca de cambios                │          │
│  │  ● Indicadores                       │          │
│  └─────────────────────────────────────┘           │
│                                                    │
│  Lo que NO VE (implementación oculta):             │
│  • Inyección de combustible                        │
│  • Transmisión de potencia                         │
│  • Sensores del motor                              │
│  • Sistema de refrigeración                        │
│                                                    │
│  El conductor no necesita saber cómo funciona      │
│  internamente, solo cómo usarlo.                   │
│                                                    │
└────────────────────────────────────────────────────┘
```

### 2.2 Ejemplo: Sistema de pagos

[x] Sin abstracción - detalles expuestos:

```python
class PagoPayPal:
    def __init__(self, email):
        self.email = email
        self.conexion = None
        self.autenticacion = None
    
    def conectar_api(self):
        # Código complejo de conexión
        pass
    
    def verificar_token(self):
        # Verificación de seguridad
        pass
    
    def procesar_pago(self, monto):
        # Lógica de pago PayPal
        pass

class PagoTarjeta:
    def __init__(self, numero, cvv):
        self.numero = numero
        self.cvv = cvv
    
    def validar_tarjeta(self):
        # Validación de tarjeta
        pass
    
    def procesar_pago(self, monto):
        # Lógica de pago con tarjeta
        pass

# El usuario tiene que conocer todos los detalles
pago = PagoPayPal("correo@ejemplo.com")
pago.conectar_api()
pago.verificar_token()
pago.procesar_pago(100)
```

[+] Con abstracción - interface simple:

```python
from abc import ABC, abstractmethod

class ProcesadorPago(ABC):
    """Clase abstracta - define el contrato."""
    
    @abstractmethod
    def verificar(self):
        """Verifica que el método de pago sea válido."""
        pass
    
    @abstractmethod
    def cobrar(self, monto):
        """Cobra el monto especificado."""
        pass
    
    def __str__(self):
        return self.__class__.__name__

class PagoPayPal(ProcesadorPago):
    def __init__(self, email):
        self.email = email
    
    def verificar(self):
        print(f"Verificando cuenta PayPal: {self.email}")
        return True
    
    def cobrar(self, monto):
        print(f"Cobrando ${monto} via PayPal a {self.email}")
        return True

class PagoTarjeta(ProcesadorPago):
    def __init__(self, numero, cvv):
        self.numero = numero
        self.cvv = cvv
    
    def verificar(self):
        print(f"Verificando tarjeta: ****{self.numero[-4:]}")
        return True
    
    def cobrar(self, monto):
        print(f"Cobrando ${monto} a tarjeta")
        return True

# Uso simplificado - el usuario solo conoce la interface
def procesar_pago(pago, monto):
    if pago.verificar():
        return pago.cobrar(monto)
    return False

p1 = PagoPayPal("correo@ejemplo.com")
p2 = PagoTarjeta("1234567890123456", "123")

procesar_pago(p1, 100)
procesar_pago(p2, 50)
```

---

## 3. Abstracción vs Encapsulación

### 3.1 Diferencias fundamentales

| Aspecto | Abstracción | Encapsulación |
|---------|-------------|---------------|
| Objetivo | Ocultar complejidad, mostrar esencia | Proteger datos, restringir acceso |
| Nivel | Diseño y especificación | Implementación interna |
| ¿Qué oculta? | Detalles de implementación | Estado interno |
| ¿Cómo? | Clases abstractas, interfaces | Modificadores de acceso |
| Cuando aplica | Al definir "qué hace" | Al definir "cómo lo hace" |

### 3.2 Analogía comparativa

```
┌────────────────────────────────────────────────────────────────┐
│                    ABSTRACCIÓN vs ENCAPSULACIÓN                │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ABSTRACCIÓN: "QUIÉN SOY"                                      │
│  ──────────────────                                            │
│  Define qué es un objeto y qué puede hacer                     │
│  Sin mostrar cómo lo hace internamente                         │
│                                                                │
│  Ejemplo: Vehículo                                             │
│  • Puede acelerar, frenar, girar                               │
│  • No sé cómo funciona el motor                                │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ENCAPSULACIÓN: "QUÉ HAY DENTRO"                               │
│  ────────────────────────                                      │
│  Protege el estado interno del objeto                          │
│  Controla quién puede modificar los datos                      │
│                                                                │
│  Ejemplo: Cápsula de medicina                                  │
│  • Interfaz: tomar con agua                                    │
│  • Encapsulado: ingredientes internos protected                │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### 3.3 Ambos trabajando juntos

```python
# ABSTRACCIÓN: Define qué puede hacer un Empleado
class EmpleadoBase(ABC):
    @abstractmethod
    def calcular_salario(self):
        pass

# ENCAPSULACIÓN: Protege los datos internos
class Empleado(EmpleadoBase):
    def __init__(self, nombre, salario_base):
        self.__nombre = nombre          # Privado (encapsulación)
        self.__salario_base = salario_base
        self.__bonificaciones = []
    
    @property
    def nombre(self):
        return self.__nombre
    
    def agregar_bonificacion(self, monto):
        if monto > 0:
            self.__bonificaciones.append(monto)
    
    # ABSTRACCIÓN: Define qué hace el método
    def calcular_salario(self):
        total = self.__salario_base + sum(self.__bonificaciones)
        return total

# Uso: El usuario no ve los detalles internos
emp = Empleado("Ana", 50000)
emp.agregar_bonificacion(5000)
print(f"{emp.nombre}: ${emp.calcular_salario()}")
# Output: Ana: $55000
```

---

## 4. Clases Abstractas

### 4.1 Definición con ABC

```python
from abc import ABC, abstractmethod

class FiguraGeometrica(ABC):
    """Clase abstracta para figuras geométricas."""
    
    @abstractmethod
    def area(self):
        """Calcula el área de la figura."""
        pass
    
    @abstractmethod
    def perimetro(self):
        """Calcula el perímetro de la figura."""
        pass
    
    def descripcion(self):
        """Método concreto - todas las figuras lo usan."""
        return f"{self.__class__.__name__} con area={self.area():.2f}"
```

### 4.2 Implementación de subclases

```python
class Rectangulo(FiguraGeometrica):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def area(self):
        return self.ancho * self.alto
    
    def perimetro(self):
        return 2 * (self.ancho + self.alto)

class Circulo(FiguraGeometrica):
    def __init__(self, radio):
        self.radio = radio
    
    def area(self):
        import math
        return math.pi * self.radio ** 2
    
    def perimetro(self):
        import math
        return 2 * math.pi * self.radio

class Triangulo(FiguraGeometrica):
    def __init__(self, base, altura, lado1, lado2):
        self.base = base
        self.altura = altura
        self.lado1 = lado1
        self.lado2 = lado2
    
    def area(self):
        return (self.base * self.altura) / 2
    
    def perimetro(self):
        return self.base + self.lado1 + self.lado2

# Uso
figuras = [
    Rectangulo(5, 3),
    Circulo(4),
    Triangulo(6, 4, 5, 5)
]

for figura in figuras:
    print(figura.descripcion())
    print(f"  Area: {figura.area():.2f}")
    print(f"  Perimetro: {figura.perimetro():.2f}")
```

### 4.3 Métodos abstractos y concretos

```python
from abc import ABC, abstractmethod

class Empleado(ABC):
    def __init__(self, nombre):
        self.nombre = nombre
    
    @abstractmethod
    def calcular_salario(self):
        """Obligatorio implementar en subclases."""
        pass
    
    def trabajar(self):
        """Concreto - implementación por defecto."""
        return f"{self.nombre} está trabajando"

class Desarrollador(Empleado):
    def __init__(self, nombre, lineas_codigo):
        super().__init__(nombre)
        self.lineas_codigo = lineas_codigo
    
    def calcular_salario(self):
        return 3000 + (self.lineas_codigo * 0.05)

class Diseñador(Empleado):
    def __init__(self, nombre, proyectos):
        super().__init__(nombre)
        self.proyectos = proyectos
    
    def calcular_salario(self):
        return 2500 + (self.proyectos * 500)

# Uso
empleados = [
    Desarrollador("Ana", 5000),
    Diseñador("Luis", 10)
]

for emp in empleados:
    print(f"{emp.nombre}: ${emp.calcular_salario():.2f}")
    print(f"  {emp.trabajar()}")
```

### 4.4 Restricciones de clases abstractas

```python
from abc import ABC, abstractmethod

class MiAbstracta(ABC):
    @abstractmethod
    def metodo_abstracto(self):
        pass

# ERROR: No se puede instanciar clase abstracta
# obj = MiAbstracta()  # TypeError

# ERROR: Subclase debe implementar métodos abstractos
# class Incompleta(MiAbstracta):
#     pass
# obj = Incompleta()  # TypeError

# CORRECTO
class Completa(MiAbstracta):
    def metodo_abstracto(self):
        return "Implementado"

obj = Completa()
print(obj.metodo_abstracto())  # Implementado
```

---

## 5. Interfaces

### 5.1 Interfaces como contratos

[!] Python no tiene keyword `interface` como Java, pero podemos simularlo:

```python
from abc import ABC, abstractmethod

# INTERFAZ: Define QUÉ debe hacer
class Persistable(ABC):
    """Interface para objetos que pueden persistirse."""
    
    @abstractmethod
    def guardar(self):
        pass
    
    @abstractmethod
    def cargar(self, id):
        pass
    
    @abstractmethod
    def eliminar(self):
        pass

class Searchable(ABC):
    """Interface para objetos buscables."""
    
    @abstractmethod
    def buscar(self, criterio):
        pass
    
    @abstractmethod
    def listar_todos(self):
        pass
```

### 5.2 Implementación de interfaces

```python
# CLASE que implementa múltiples interfaces
class Usuario(Persistable, Searchable):
    def __init__(self, id=None, nombre="", email=""):
        self.id = id
        self.nombre = nombre
        self.email = email
        self.__guardado = False
    
    # Implementación de Persistable
    def guardar(self):
        print(f"Guardando usuario {self.nombre} en BD")
        self.id = 123  # Simular ID asignado
        self.__guardado = True
        return True
    
    def cargar(self, id):
        print(f"Cargando usuario con ID {id}")
        self.id = id
        self.nombre = "Usuario Cargado"
        self.__guardado = True
        return True
    
    def eliminar(self):
        if self.__guardado:
            print(f"Eliminando usuario {self.id}")
            self.__guardado = False
            return True
        return False
    
    # Implementación de Searchable
    def buscar(self, criterio):
        print(f"Buscando usuarios con criterio: {criterio}")
        return [self] if criterio in self.nombre else []
    
    def listar_todos(self):
        print("Listando todos los usuarios")
        return [self]

# Uso
usuario = Usuario(nombre="Ana", email="ana@ejemplo.com")
usuario.guardar()
print(usuario.buscar("Ana"))
```

### 5.3 Duck typing en Python

[!] Python es dinámico: no necesita interfaces formales si siguen el mismo contrato:

```python
# Dos clases con métodos similares
class Gato:
    def hablar(self):
        return "Miau"

class Perro:
    def hablar(self):
        return "Guau"

class Vaca:
    def hablar(self):
        return "Muuu"

# Funciona con cualquier objeto que tenga hablar()
def hacer_hablar(animal):
    print(animal.hablar())

gato = Gato()
perro = Perro()
vaca = Vaca()

hacer_hablar(gato)   # Miau
hacer_hablar(perro)  # Guau
hacer_hablar(vaca)   # Muuu
```

---

## 6. Cuándo Usar Abstracción

### 6.1 Guía de decisión

```
┌─────────────────────────────────────────────────────┐
│           ¿CUÁNDO USAR ABSTRACCIÓN?                 │
├─────────────────────────────────────────────────────┤
│                                                     │
│  USAR cuando:                                       │
│  [+] Diseñas una API o biblioteca                   │
│  [+] Necesitas definir contratos claros             │
│  [+] Múltiples implementaciones posibles            │
│  [+] Quieres ocultar complejidad                    │
│  [+] Trabajas en equipo (roles definidos)           │
│                                                     │
│  NO USAR cuando:                                    │
│  [x] Clase simple sin comportamiento variado        │
│  [x] No hay múltiples implementaciones              │
│  [x] Over-engineering innecesario                   │
│  [x] Prototyping rápido                             │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 6.2 Ejemplo: Sistema de notificaciones

```python
from abc import ABC, abstractmethod

# INTERFAZ: Notificador
class Notificador(ABC):
    @abstractmethod
    def enviar(self, destinatario, mensaje):
        pass
    
    @abstractmethod
    def esta_disponible(self):
        pass

# IMPLEMENTACIONES concretas
class NotificadorEmail(Notificador):
    def __init__(self):
        self.servidor = "smtp.ejemplo.com"
    
    def enviar(self, destinatario, mensaje):
        if self.esta_disponible():
            print(f"EMAIL a {destinatario}: {mensaje}")
            return True
        return False
    
    def esta_disponible(self):
        return True

class NotificadorSMS(Notificador):
    def __init__(self):
        self.creditos = 10
    
    def enviar(self, destinatario, mensaje):
        if self.esta_disponible() and self.creditos > 0:
            print(f"SMS a {destinatario}: {mensaje}")
            self.creditos -= 1
            return True
        return False
    
    def esta_disponible(self):
        return self.creditos > 0

# USO: El cliente solo conoce la interface
class ServicioAlertas:
    def __init__(self, notificador):
        self.notificador = notificador
    
    def alerta(self, mensaje):
        return self.notificador.enviar("admin@ejemplo.com", mensaje)

# Fácil cambiar implementación
alerta1 = ServicioAlertas(NotificadorEmail())
alerta2 = ServicioAlertas(NotificadorSMS())

alerta1.alerta("Sistema caido")
alerta2.alerta("Temperatura alta")
```

---

## 7. Ejercicios Prácticos

### 7.1 shapes con abstracción completa

```python
from abc import ABC, abstractmethod
import math

class Figura(ABC):
    @abstractmethod
    def calcular_area(self):
        pass
    
    @abstractmethod
    def calcular_perimetro(self):
        pass
    
    def info(self):
        return f"{self.__class__.__name__}: Area={self.calcular_area():.2f}"

class Circulo(Figura):
    def __init__(self, radio):
        self.radio = radio
    
    def calcular_area(self):
        return math.pi * self.radio ** 2
    
    def calcular_perimetro(self):
        return 2 * math.pi * self.radio

class Rectangulo(Figura):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto
    
    def calcular_perimetro(self):
        return 2 * (self.ancho + self.alto)

class Triangulo(Figura):
    def __init__(self, a, b, c):
        self.a = a
        self.b = b
        self.c = c
    
    def calcular_area(self):
        s = (self.a + self.b + self.c) / 2
        return math.sqrt(s * (s-self.a) * (s-self.b) * (s-self.c))
    
    def calcular_perimetro(self):
        return self.a + self.b + self.c

class CalculadoraFiguras:
    def __init__(self):
        self.figuras = []
    
    def agregar(self, figura):
        if isinstance(figura, Figura):
            self.figuras.append(figura)
    
    def area_total(self):
        return sum(f.calcular_area() for f in self.figuras)
    
    def perimetro_total(self):
        return sum(f.calcular_perimetro() for f in self.figuras)

# Uso
calc = CalculadoraFiguras()
calc.agregar(Circulo(5))
calc.agregar(Rectangulo(4, 6))
calc.agregar(Triangulo(3, 4, 5))

print(f"Area total: {calc.area_total():.2f}")
print(f"Perimetro total: {calc.perimetro_total():.2f}")

for f in calc.figuras:
    print(f.info())
```

### 7.2 Plugin system con abstracción

```python
from abc import ABC, abstractmethod

class Plugin(ABC):
    @property
    @abstractmethod
    def nombre(self):
        pass
    
    @abstractmethod
    def ejecutar(self, datos):
        pass

class PluginMayusculas(Plugin):
    @property
    def nombre(self):
        return "Mayusculas"
    
    def ejecutar(self, datos):
        return datos.upper()

class PluginReverse(Plugin):
    @property
    def nombre(self):
        return "Reversa"
    
    def ejecutar(self, datos):
        return datos[::-1]

class PluginContar(Plugin):
    @property
    def nombre(self):
        return "Contador"
    
    def ejecutar(self, datos):
        return {"texto": datos, "longitud": len(datos)}

class Pipeline:
    def __init__(self):
        self.plugins = []
    
    def agregar(self, plugin):
        self.plugins.append(plugin)
    
    def procesar(self, texto):
        resultado = texto
        for plugin in self.plugins:
            print(f"Aplicando: {plugin.nombre}")
            resultado = plugin.ejecutar(resultado)
        return resultado

# Uso
pipeline = Pipeline()
pipeline.agregar(PluginMayusculas())
pipeline.agregar(PluginReverse())
pipeline.agregar(PluginContar())

resultado = pipeline.procesar("hola mundo")
print(f"Resultado: {resultado}")
```

---

## Resumen

```
ABSTRACCIÓN = QUÉ hace (interface)
ENCAPSULACIÓN = QUÉ hay dentro (protección)

┌─────────────────────────────────────────────┐
│           CLASES ABSTRACTAS                 │
├─────────────────────────────────────────────┤
│                                             │
│  from abc import ABC, abstractmethod        │
│                                             │
│  class MiAbstracta(ABC):                    │
│      @abstractmethod                        │
│      def metodo(self):                      │
│          pass                               │
│                                             │
│  • No se pueden instanciar                  │
│  • Subclases DEBEN implementar métodos      │
│  • Pueden tener métodos concretos           │
│                                             │
└─────────────────────────────────────────────┘
```

- [+] Abstracción = ocultar complejidad, mostrar esencia
- [+] Encapsulación = proteger datos internos
- [+] Clases abstractas = definir "qué" sin definir "cómo"
- [!] Interfaces en Python = clases abstractas con solo métodos abstractos
- [!] No sobre-ingenierices - usa abstracción cuando añade valor
