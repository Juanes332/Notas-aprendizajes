
2026-04-12 18:23

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 


# Polymorphism


## 1. Introducción

El polimorfismo (del griego: "muchas formas") es la capacidad de los objetos de diferentes clases de responder al mismo mensaje (llamada de método) de maneras diferentes. Es uno de los cuatro pilares de la POO junto con encapsulación, herencia y abstracción.

### Tipos de polimorfismo

- [+] **Polimorfismo en tiempo de ejecución** (overriding): Diferentes implementaciones según el tipo de objeto
- [+] **Polimorfismo en tiempo de compilación** (overloading): Diferentes firmas de método
- [+] **Duck typing**: Comportamiento basado en atributos/métodos disponibles

---

## 2. Qué es el Polimorfismo

### 2.1 Analogía: El botón de "encender"

```
┌─────────────────────────────────────────────────────┐
│              INTERFAZ UNIFICADA                     │
│                  (Botón de Encender)                │
├─────────────────────────────────────────────────────┤
│                                                     │
│   El usuario presiona UN botón                      │
│                                                     │
│       ┌──────────┐                                  │
│       │  ON/OFF  │                                  │
│       └──────────┘                                  │
│                                                     │
├─────────────────────────────────────────────────────┤
│                                                     │
│   Televisor ──→ Se enciende con imagen              │
│   Radio ───────→ Se enciende con sonido             │
│   Lampara ─────→ Se enciende con luz                │
│   PC ──────────→ Se enciende con sistema operativo  │
│                                                     │
│   El botón no sabe cómo funciona cada dispositivo,  │
│   solo sabe que cuando lo presionan, se enciende.   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 2.2 Ejemplo: Sistema de pagos

```python
class ProcesadorPago:
    def procesar(self, monto):
        """Método que cada subclase implementa diferente."""
        raise NotImplementedError

class PayPal(ProcesadorPago):
    def procesar(self, monto):
        return f"Procesando ${monto} via PayPal"

class TarjetaCredito(ProcesadorPago):
    def procesar(self, monto):
        return f"Procesando ${monto} via Tarjeta"

class Bitcoin(ProcesadorPago):
    def procesar(self, monto):
        return f"Procesando ${monto} en BTC"

class Efectivo(ProcesadorPago):
    def procesar(self, monto):
        return f"Cobrando ${monto} en efectivo"

# La función no sabe qué tipo de pago es
def realizar_pago(pago, monto):
    return pago.procesar(monto)

# Uso: misma función, diferentes comportamientos
pagos = [
    PayPal(),
    TarjetaCredito(),
    Bitcoin(),
    Efectivo()
]

for pago in pagos:
    print(realizar_pago(pago, 100))
# Procesando $100 via PayPal
# Procesando $100 via Tarjeta
# Procesando $100 en BTC
# Cobrando $100 en efectivo
```

---

## 3. Polimorfismo con Herencia

### 3.1 Override de métodos

```python
class Animal:
    def hacer_sonido(self):
        return "..."

class Perro(Animal):
    def hacer_sonido(self):
        return "Guau!"

class Gato(Animal):
    def hacer_sonido(self):
        return "Miau!"

class Vaca(Animal):
    def hacer_sonido(self):
        return "Muuu!"

class Pato(Animal):
    def hacer_sonido(self):
        return "Cuac!"

# Misma llamada, diferente resultado
animales = [Perro(), Gato(), Vaca(), Pato()]

for animal in animales:
    print(animal.hacer_sonido())

# Guau!
# Miau!
# Muuu!
# Cuac!
```

### 3.2 Clases abstractas con polimorfismo

```python
from abc import ABC, abstractmethod

class Figura(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimetro(self):
        pass

class Rectangulo(Figura):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def area(self):
        return self.ancho * self.alto
    
    def perimetro(self):
        return 2 * (self.ancho + self.alto)

class Circulo(Figura):
    def __init__(self, radio):
        self.radio = radio
    
    def area(self):
        import math
        return math.pi * self.radio ** 2
    
    def perimetro(self):
        import math
        return 2 * math.pi * self.radio

# Función polimórfica
def mostrar_info(figura):
    print(f"Area: {figura.area():.2f}")
    print(f"Perimetro: {figura.perimetro():.2f}")

# Uso
figuras = [Rectangulo(5, 3), Circulo(4), Rectangulo(2, 2)]

for fig in figuras:
    print(f"--- {fig.__class__.__name__} ---")
    mostrar_info(fig)
```

### 3.3 Ejemplo: Empleados y salarios

```python
from abc import ABC, abstractmethod

class Empleado(ABC):
    def __init__(self, nombre):
        self.nombre = nombre
    
    @abstractmethod
    def calcular_salario(self):
        pass

class EmpleadoFijo(Empleado):
    def __init__(self, nombre, salario_mensual):
        super().__init__(nombre)
        self.salario_mensual = salario_mensual
    
    def calcular_salario(self):
        return self.salario_mensual

class EmpleadoPorHoras(Empleado):
    def __init__(self, nombre, horas_trabajadas, tarifa):
        super().__init__(nombre)
        self.horas_trabajadas = horas_trabajadas
        self.tarifa = tarifa
    
    def calcular_salario(self):
        return self.horas_trabajadas * self.tarifa

class EmpleadoPorComision(Empleado):
    def __init__(self, nombre, ventas, porcentaje):
        super().__init__(nombre)
        self.ventas = ventas
        self.porcentaje = porcentaje
    
    def calcular_salario(self):
        return self.ventas * self.porcentaje / 100

# Nómina polimórfica
def procesar_nomina(empleados):
    total = 0
    for emp in empleados:
        salario = emp.calcular_salario()
        print(f"{emp.nombre}: ${salario:,.2f}")
        total += salario
    return total

# Uso
empleados = [
    EmpleadoFijo("Ana", 50000),
    EmpleadoPorHoras("Luis", 160, 25),
    EmpleadoPorComision("Maria", 100000, 5)
]

print("=== NÓMINA ===")
total = procesar_nomina(empleados)
print(f"\nTotal a pagar: ${total:,.2f}")
```

---

## 4. Duck Typing

### 4.1 Concepto

"If it walks like a duck and quacks like a duck, it's a duck"

Python no necesita herencia explícita. Si un objeto tiene los métodos necesarios, funciona.

### 4.2 Ejemplo sin herencia

```python
class Pato:
    def nadar(self):
        return "El pato nada"
    
    def hacer_sonido(self):
        return "Cuac!"

class Persona:
    def nadar(self):
        return "La persona nada con estilo"
    
    def hacer_sonido(self):
        return "..."

class Piedra:
    pass  # No tiene los métodos

# Función que acepta cualquier "cosas que naden"
def cosa_que_nada(obj):
    print(obj.nadar())
    print(obj.hacer_sonido())

pato = Pato()
persona = Persona()
piedra = Piedra()

cosa_que_nada(pato)     # Funciona
cosa_que_nada(persona) # Funciona
# cosa_que_nada(piedra) # Error - no tiene nadar()
```

### 4.3 Protocolos implícitos

```python
# PROTOCOLO: iterable
class ListaEnlazada:
    def __init__(self, datos):
        self.datos = datos
    
    def __iter__(self):
        return iter(self.datos)

class Range:
    def __init__(self, inicio, fin):
        self.inicio = inicio
        self.fin = fin
    
    def __iter__(self):
        i = self.inicio
        while i < self.fin:
            yield i
            i += 1

# Ambos son iterables por tener __iter__
for item in ListaEnlazada([1, 2, 3]):
    print(item)

for item in Range(1, 4):
    print(item)
```

---

## 5. Métodos Especiales

### 5.1 Métodos dunder para polimorfismo

```python
class Numero:
    def __init__(self, valor):
        self.valor = valor
    
    def __str__(self):
        return f"Numero({self.valor})"
    
    def __repr__(self):
        return f"Numero({self.valor})"
    
    def __add__(self, otro):
        if isinstance(otro, Numero):
            return Numero(self.valor + otro.valor)
        return Numero(self.valor + otro)
    
    def __eq__(self, otro):
        if isinstance(otro, Numero):
            return self.valor == otro.valor
        return self.valor == otro
    
    def __lt__(self, otro):
        if isinstance(otro, Numero):
            return self.valor < otro.valor
        return self.valor < otro

# Uso
a = Numero(5)
b = Numero(3)

print(a + b)       # Numero(8)
print(a == b)      # False
print(a < b)       # False
print(str(a))      # Numero(5)
```

### 5.2 Ejemplo: Sistema de secuencias

```python
class Secuencia:
    def __len__(self):
        """Número de elementos."""
        raise NotImplementedError
    
    def __getitem__(self, indice):
        """Obtener elemento."""
        raise NotImplementedError

class Lista(Secuencia):
    def __init__(self, elementos):
        self.elementos = elementos
    
    def __len__(self):
        return len(self.elementos)
    
    def __getitem__(self, indice):
        return self.elementos[indice]

class Tupla(Secuencia):
    def __init__(self, elementos):
        self.elementos = elementos
    
    def __len__(self):
        return len(self.elementos)
    
    def __getitem__(self, indice):
        return self.elementos[indice]

class Rango(Secuencia):
    def __init__(self, inicio, fin):
        self.inicio = inicio
        self.fin = fin
    
    def __len__(self):
        return self.fin - self.inicio
    
    def __getitem__(self, indice):
        if 0 <= indice < len(self):
            return self.inicio + indice
        raise IndexError("Índice fuera de rango")

# Función polimórfica
def procesar_secuencia(seq):
    print(f"Longitud: {len(seq)}")
    if len(seq) > 0:
        print(f"Primero: {seq[0]}")
        print(f"Último: {seq[len(seq) - 1]}")

procesar_secuencia(Lista([1, 2, 3]))
procesar_secuencia(Tupla((4, 5, 6)))
procesar_secuencia(Rango(10, 15))
```

---

## 6. Casos de Uso

### 6.1 Caso: Plugin System

```python
from abc import ABC, abstractmethod

class Plugin(ABC):
    @abstractmethod
    def nombre(self):
        pass
    
    @abstractmethod
    def ejecutar(self, data):
        pass

class PluginMayusculas(Plugin):
    def nombre(self):
        return "MAYUSCULAS"
    
    def ejecutar(self, data):
        return data.upper()

class PluginInverso(Plugin):
    def nombre(self):
        return "INVERSO"
    
    def ejecutar(self, data):
        return data[::-1]

class PluginContarVocales(Plugin):
    def nombre(self):
        return "CONTADOR_VOCALES"
    
    def ejecutar(self, data):
        vocales = "aeiouAEIOU"
        return sum(1 for c in data if c in vocales)

class PluginReversaPalabras(Plugin):
    def nombre(self):
        return "REVERSA_PALABRAS"
    
    def ejecutar(self, data):
        return " ".join(data.split()[::-1])

class ProcesadorTexto:
    def __init__(self):
        self.plugins = []
    
    def agregar_plugin(self, plugin):
        self.plugins.append(plugin)
    
    def procesar(self, texto):
        resultado = texto
        for plugin in self.plugins:
            resultado = plugin.ejecutar(resultado)
            print(f"  [{plugin.nombre()}]: {resultado}")
        return resultado

# Uso
procesador = ProcesadorTexto()
procesador.agregar_plugin(PluginMayusculas())
procesador.agregar_plugin(PluginInverso())
procesador.agregar_plugin(PluginContarVocales())

print("=== Procesando 'Hola Mundo' ===")
resultado = procesador.procesar("Hola Mundo")
print(f"\nResultado final: {resultado}")
```

### 6.2 Caso: Sistema de persistencia

```python
from abc import ABC, abstractmethod

class Persistible(ABC):
    @abstractmethod
    def guardar(self, destino):
        pass
    
    @abstractmethod
    def cargar(self, origen):
        pass

class ListaPersistible(Persistible):
    def __init__(self, datos=None):
        self.datos = datos or []
    
    def guardar(self, destino):
        with open(destino, 'w') as f:
            f.write('\n'.join(str(x) for x in self.datos))
    
    def cargar(self, origen):
        with open(origen, 'r') as f:
            self.datos = [linea.strip() for linea in f]

class DictPersistible(Persistible):
    def __init__(self, datos=None):
        self.datos = datos or {}
    
    def guardar(self, destino):
        import json
        with open(destino, 'w') as f:
            json.dump(self.datos, f, indent=2)
    
    def cargar(self, origen):
        import json
        with open(origen, 'r') as f:
            self.datos = json.load(f)

# Función genérica de backup
def hacer_backup(objeto, archivo):
    if isinstance(objeto, Persistible):
        objeto.guardar(archivo)
        print(f"Backup guardado en {archivo}")
    else:
        print("El objeto no es persistente")

# Uso
mi_lista = ListaPersistible([1, 2, 3, 4, 5])
mi_dict = DictPersistible({"nombre": "Ana", "edad": 25})

hacer_backup(mi_lista, "backup_lista.txt")
hacer_backup(mi_dict, "backup_dict.json")
```

### 6.3 Caso: Validadores

```python
from abc import ABC, abstractmethod

class Validador(ABC):
    @abstractmethod
    def validar(self, valor):
        pass
    
    def __call__(self, valor):
        return self.validar(valor)

class ValidadorNoVacio(Validador):
    def validar(self, valor):
        if valor is None:
            return False, "El valor no puede ser None"
        if isinstance(valor, str) and not valor.strip():
            return False, "El string no puede estar vacío"
        if isinstance(valor, (list, tuple)) and len(valor) == 0:
            return False, "La colección no puede estar vacía"
        return True, "Válido"

class ValidadorNumero(Validador):
    def __init__(self, minimo=None, maximo=None):
        self.minimo = minimo
        self.maximo = maximo
    
    def validar(self, valor):
        if not isinstance(valor, (int, float)):
            return False, "Debe ser un número"
        if self.minimo is not None and valor < self.minimo:
            return False, f"Debe ser >= {self.minimo}"
        if self.maximo is not None and valor > self.maximo:
            return False, f"Debe ser <= {self.maximo}"
        return True, "Válido"

class ValidadorEmail(Validador):
    def validar(self, valor):
        if '@' not in valor:
            return False, "Debe contener @"
        if '.' not in valor.split('@')[-1]:
            return False, "Debe tener dominio después de @"
        return True, "Válido"

class Formulario:
    def __init__(self):
        self.campos = {}
    
    def agregar_campo(self, nombre, validador):
        self.campos[nombre] = validador
    
    def validar(self, datos):
        errores = {}
        for nombre, validador in self.campos.items():
            if nombre in datos:
                valido, mensaje = validador(datos[nombre])
                if not valido:
                    errores[nombre] = mensaje
        return len(errores) == 0, errores

# Uso
form = Formulario()
form.agregar_campo("nombre", ValidadorNoVacio())
form.agregar_campo("edad", ValidadorNumero(minimo=0, maximo=150))
form.agregar_campo("email", ValidadorEmail())

datos_validos = {
    "nombre": "Ana",
    "edad": 25,
    "email": "ana@ejemplo.com"
}

datos_invalidos = {
    "nombre": "",
    "edad": -5,
    "email": "invalid-email"
}

print("Datos válidos:")
valido, errores = form.validar(datos_validos)
print(f"  Válido: {valido}")

print("\nDatos inválidos:")
valido, errores = form.validar(datos_invalidos)
print(f"  Válido: {valido}")
print(f"  Errores: {errores}")
```

---

## 7. Ejercicios Prácticos

### 7.1 Calculadora con operaciones polimórficas

```python
from abc import ABC, abstractmethod

class Operacion(ABC):
    @abstractmethod
    def ejecutar(self, a, b):
        pass
    
    def __repr__(self):
        return self.__class__.__name__

class Suma(Operacion):
    def ejecutar(self, a, b):
        return a + b

class Resta(Operacion):
    def ejecutar(self, a, b):
        return a - b

class Multiplicacion(Operacion):
    def ejecutar(self, a, b):
        return a * b

class Division(Operacion):
    def ejecutar(self, a, b):
        if b == 0:
            raise ZeroDivisionError("No se puede dividir entre cero")
        return a / b

class Potencia(Operacion):
    def ejecutar(self, a, b):
        return a ** b

class Calculadora:
    def __init__(self):
        self.operaciones = {
            "+": Suma(),
            "-": Resta(),
            "*": Multiplicacion(),
            "/": Division(),
            "**": Potencia()
        }
    
    def calcular(self, a, b, operacion):
        op = self.operaciones.get(operacion)
        if op is None:
            raise ValueError(f"Operación '{operacion}' no válida")
        return op.ejecutar(a, b)
    
    def calcular_lista(self, valores, operacion):
        resultado = valores[0]
        for v in valores[1:]:
            resultado = self.calcular(resultado, v, operacion)
        return resultado

# Uso
calc = Calculadora()

print("=== Operaciones básicas ===")
print(f"10 + 5 = {calc.calcular(10, 5, '+')}")
print(f"10 - 5 = {calc.calcular(10, 5, '-')}")
print(f"10 * 5 = {calc.calcular(10, 5, '*')}")
print(f"10 / 5 = {calc.calcular(10, 5, '/')}")

print("\n=== Operaciones avanzadas ===")
print(f"2 ** 8 = {calc.calcular(2, 8, '**')}")

print("\n=== Calcular lista ===")
print(f"1 + 2 + 3 + 4 + 5 = {calc.calcular_lista([1, 2, 3, 4, 5], '+')}")
print(f"2 * 3 * 4 = {calc.calcular_lista([2, 3, 4], '*')}")
```

### 7.2 Renderizado de documentos

```python
from abc import ABC, abstractmethod

class Renderizable(ABC):
    @abstractmethod
    def render(self):
        pass

class Texto(Renderizable):
    def __init__(self, contenido):
        self.contenido = contenido
    
    def render(self):
        return self.contenido

class Negrita(Renderizable):
    def __init__(self, contenido):
        self.contenido = contenido if isinstance(contenido, Renderizable) else Texto(contenido)
    
    def render(self):
        return f"**{self.contenido.render()}**"

class Cursiva(Renderizable):
    def __init__(self, contenido):
        self.contenido = contenido if isinstance(contenido, Renderizable) else Texto(contenido)
    
    def render(self):
        return f"_{self.contenido.render()}_"

class Enlace(Renderizable):
    def __init__(self, texto, url):
        self.texto = texto
        self.url = url
    
    def render(self):
        return f"[{self.texto}]({self.url})"

class Lista(Renderizable):
    def __init__(self, items):
        self.items = items
    
    def render(self):
        lineas = [f"- {item.render()}" for item in self.items]
        return "\n".join(lineas)

# Uso
documento = Negrita(
    Cursiva(
        Enlace("documentación de Python", "https://python.org")
    )
)
print(documento.render())
# **_[documentación de Python](https://python.org)_**

mi_lista = Lista([
    Texto("Primer elemento"),
    Negrita("Segundo elemento"),
    Cursiva("Tercer elemento"),
    Enlace("Enlace", "https://ejemplo.com")
])
print(mi_lista.render())
```

---

## Resumen

```python
# POLIMORFISMO con herencia
class Animal:
    def sonido(self):
        pass

class Perro(Animal):
    def sonido(self):
        return "Guau"

# DUCK TYPING
def procesar(obj):
    return obj.metodo_necesario()

# MÉTODOS ESPECIALES
class MiClase:
    def __str__(self): pass
    def __add__(self, other): pass
    def __eq__(self, other): pass
    def __len__(self): pass
    def __getitem__(self, key): pass
```

- [+] Polimorfismo = misma interfaz, diferente implementación
- [+] Override = sobrescribir método del padre
- [+] Duck typing = si tiene los métodos, funciona
- [+] Clases abstractas = definen contrato
- [!] No abusar - el polimorfismo debe tener sentido




