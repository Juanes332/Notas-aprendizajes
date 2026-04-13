
2026-04-12 18:20

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 

## 1. Introducción

La herencia es un mecanismo de POO que permite **crear una nueva clase basada en una existente**. La clase nueva (hija) hereda atributos y métodos de la clase existente (padre), promoviendo la reutilización de código.

### Beneficios de la herencia

- [+] Reutilización de código
- [+] Jerarquía natural de conceptos
- [+] Polimorfismo (permitido por herencia)
- [+] Mantenimiento centralizado

---

## 2. Qué es la Herencia

### 2.1 Analogía: Taxonomía biológica

```
                    Animal (padre)
                        │
           ┌───────────┼───────────┐
           │           │           │
        Mamifero    Ave         Reptil
           │           │           │
      ┌────┴────┐   ┌──┴──┐     ┌──┴──┐
      │         │   │     │     │     │
    Perro    Gato  Canario  Gallo  Lagarto
```

- Perro hereda de Mamifero
- Mamifero hereda de Animal
- Animal define características comunes a todos

### 2.2 Notación

```python
class ClaseHija(ClasePadre):
    """La clase Hija hereda de ClasePadre."""
    pass
```

---

## 3. Cuándo Heredar

### 3.1 Relación "es-un" (is-a)

[+] Usa herencia cuando hay relación "es-un":

```
✔  Perro ES UN Animal → class Perro(Animal)
✔  Circulo ES UNA Figura → class Circulo(Figura)
✔  Gato ES UN Mamifero → class Gato(Mamifero)
```

[x] NO uses herencia para relaciones "tiene-un" (has-a):

```
✘  Coche TIENE UN Motor → class Coche(Motor) 
✘  Persona TIENE UN Coche → class Persona(Coche) 
```

### 3.2 Señales de herencia apropiada

```
┌─────────────────────────────────────────────────┐
│           ¿DEBERÍA HEREDAR?                     │
├─────────────────────────────────────────────────┤
│                                                 │
│  SÍ heredar cuando:                             │
│  [+] La subclase es una versión específica      │
│      de la superclase                           │
│  [+] La subclase debe tener los mismos          │
│      métodos públicos                           │
│  [+] Quieres compartir implementación           │
│                                                 │
│  NO heredar cuando:                             │
│  [x] Solo quieres reutilizar código             │
│      (considera composición)                    │
│  [x] Las clases no tienen relación "es-un"      │
│  [x] La relación es "tiene-un"                  │
│                                                 │
└─────────────────────────────────────────────────┘
```

### 3.3 Composición vs Herencia

```python
# HERENCIA para "tiene-un" (INCORRECTO)
class Motor:
    def encender(self): pass

class Coche(Motor):  # Mal: Coche NO es un Motor
    pass

# COMPOSICIÓN para "tiene-un" (CORRECTO)
class Coche:
    def __init__(self):
        self.motor = Motor()  # Coche TIENE un Motor
    
    def encender(self):
        self.motor.encender()
```

---

## 4. Herencia Simple

### 4.1 Estructura básica

```python
class Animal:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    def hablar(self):
        """Método que será sobrescrito."""
        return "..."

class Perro(Animal):
    def __init__(self, nombre, edad, raza):
        super().__init__(nombre, edad)  # Llama al padre
        self.raza = raza
    
    def hablar(self):
        return "Guau!"

class Gato(Animal):
    def __init__(self, nombre, edad, color):
        super().__init__(nombre, edad)
        self.color = color
    
    def hablar(self):
        return "Miau!"

# Uso
perro = Perro("Rex", 3, "Pastor Alemán")
gato = Gato("Michi", 2, "Naranja")

print(perro.nombre)   # Rex - heredado
print(perro.hablar()) # Guau! - sobrescrito
print(gato.color)     # Naranja - propio
```

### 4.2 Jerarquía de empleado

```python
class Empleado:
    IMPUESTO = 0.15
    
    def __init__(self, nombre, salario):
        self.nombre = nombre
        self.salario = salario
    
    def calcular_salario_neto(self):
        return self.salario * (1 - self.IMPUESTO)
    
    def trabajar(self):
        return f"{self.nombre} está trabajando"

class Gerente(Empleado):
    def __init__(self, nombre, salario, departamento):
        super().__init__(nombre, salario)
        self.departamento = departamento
    
    def trabajar(self):
        return f"{self.nombre} está gestionando {self.departamento}"
    
    def contratar(self, empleado):
        return f"{self.nombre} contrató a {empleado.nombre}"

class Desarrollador(Empleado):
    def __init__(self, nombre, salario, lenguaje):
        super().__init__(nombre, salario)
        self.lenguaje = lenguaje
    
    def trabajar(self):
        return f"{self.nombre} está programando en {self.lenguaje}"

# Uso
gerente = Gerente("Ana", 80000, "IT")
dev = Desarrollador("Luis", 60000, "Python")

print(gerente.trabajar())
print(dev.trabajar())
print(gerente.contratar(dev))
print(f"Salario neto dev: ${dev.calcular_salario_neto():,.2f}")
```

### 4.3 Sobrescritura (override)

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
    
    def descripcion(self):
        return f"{self.marca} {self.modelo}"

class Coche(Vehiculo):
    def __init__(self, marca, modelo, puertas):
        super().__init__(marca, modelo)
        self.puertas = puertas
    
    # Sobrescribir método
    def descripcion(self):
        return f"{self.marca} {self.modelo} ({self.puertas} puertas)"

class Moto(Vehiculo):
    def __init__(self, marca, modelo, cilindrada):
        super().__init__(marca, modelo)
        self.cilindrada = cilindrada
    
    def descripcion(self):
        return f"{self.marca} {self.modelo} ({self.cilindrada}cc)"

coche = Coche("Toyota", "Corolla", 4)
moto = Moto("Honda", "CBR", 600)

print(coche.descripcion())  # Toyota Corolla (4 puertas)
print(moto.descripcion())  # Honda CBR (600cc)
```

---

## 5. Herencia Multinivel

### 5.1 Concepto

```
 ClaseA → ClaseB → ClaseC → ClaseD
```

Cada nivel hereda del anterior.

### 5.2 Ejemplo: Sistema de empleados

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    def presentarse(self):
        return f"Hola, soy {self.nombre}"

class Empleado(Persona):
    def __init__(self, nombre, edad, empleado_id):
        super().__init__(nombre, edad)
        self.empleado_id = empleado_id
        self.departamento = None
    
    def trabajar(self):
        return f"{self.nombre} está trabajando"

class Gerente(Empleado):
    def __init__(self, nombre, edad, empleado_id, area):
        super().__init__(nombre, edad, empleado_id)
        self.area = area
    
    def gestionar(self):
        return f"{self.nombre} gestiona {self.area}"

class Director(Gerente):
    def __init__(self, nombre, edad, empleado_id, area, presupuesto):
        super().__init__(nombre, edad, empleado_id, area)
        self.presupuesto = presupuesto
    
    def tomar_decisiones(self):
        return f"{self.nombre} decide sobre presupuesto de ${self.presupuesto:,.2f}"

# Uso
director = Director("Maria", 45, "D001", "Operaciones", 1000000)

print(director.presentarse())    # De Persona
print(director.trabajar())       # De Empleado
print(director.gestionar())      # De Gerente
print(director.tomar_decisiones())  # Propio
```

### 5.3 MRO - Method Resolution Order

[!] Python usa MRO para resolver qué método ejecutar:

```python
class A:
    def saludar(self):
        return "A"

class B(A):
    def saludar(self):
        return "B"

class C(A):
    def saludar(self):
        return "C"

class D(B, C):
    pass

d = D()
print(d.saludar())  # B (MRO: D → B → C → A)

# Ver MRO
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, 
#  <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```

---

## 6. Herencia Múltiple

### 6.1 Concepto

```python
class ClaseA:
    pass

class ClaseB:
    pass

class ClaseC(ClaseA, ClaseB):  # Hereda de ambas
    pass
```

### 6.2 Ejemplo: Clase Volador y Nadador

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre

class Volador:
    def __init__(self):
        self.altura = 0
    
    def volar(self):
        return f"Volando a {self.altura}m"

class Nadador:
    def __init__(self):
        self.profundidad = 0
    
    def nadar(self):
        return f"Nadando a {self.profundidad}m"

# Herencia múltiple
class Pato(Animal, Volador, Nadador):
    def __init__(self, nombre):
        Animal.__init__(self, nombre)
        Volador.__init__(self)
        Nadador.__init__(self)
    
    def describir(self):
        acciones = []
        if isinstance(self, Volador):
            acciones.append(self.volar())
        if isinstance(self, Nadador):
            acciones.append(self.nadar())
        return f"{self.nombre}: {', '.join(acciones)}"

pato = Pato("Donald")
print(pato.describir())
print(pato.volar())
print(pato.nadar())
```

### 6.3 El problema del diamante

```
        Animal
       /      \
    Volador  Nadador
       \      /
        Pato
```

```python
class Animal:
    def describir(self):
        return "Soy un animal"

class Volador(Animal):
    def describir(self):
        return "Puedo volar"

class Nadador(Animal):
    def describir(self):
        return "Puedo nadar"

class Pato(Volador, Nadador):
    pass

pato = Pato()
print(pato.describir())  # Puedo volar (MRO: Pato → Volador → Nadador → Animal)
```

### 6.4 Mixins

[+] Los mixins son clases diseñadas para ser heredadas por otras:

```python
# Mixin - proporciona funcionalidad extra
class JsonMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)
    
    @classmethod
    def from_json(cls, json_str):
        import json
        data = json.loads(json_str)
        return cls(**data)

class Usuario(JsonMixin):
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email

class Producto(JsonMixin):
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio

# Uso
usuario = Usuario("Ana", "ana@ejemplo.com")
producto = Producto("Laptop", 999.99)

print(usuario.to_json())
print(producto.to_json())
```

---

## 7. Wide vs Deep

### 7.1 Concepto

```
HERENCIA ANCHA (Wide)
═══════════════════════
        Animal
     /    |    \
   Perro  Gato  Ave
   |  |   |  |   |  |
 Rex Lassy Miau Felix Tweety
(Herencia superficial pero muchos niveles)

HERENCIA PROFUNDA (Deep)
════════════════════════════
         SerVivo
            │
         Animal
            │
         Mamifero
            │
          Perro
            │
        PastorAleman
            │
       PerroGuia
(Herencia profunda pero clases específicas)
```

### 7.2 Recomendación: Wide over Deep

[x] Evitar jerarquías muy profundas:

```python
# PROBLEMA: Herencia muy profunda
class SerVivo:
    pass

class Animal(SerVivo):
    pass

class Vertebrado(Animal):
    pass

class Mamifero(Vertebrado):
    pass

class Carnivoro(Mamifero):
    pass

class Canino(Carnivoro):
    pass

class Perro(Canino):
    pass

class Raza(Perro):
    pass

class MiPerro(Raza):  # 7 niveles de herencia
    pass
```

[+] Preferir jerarquías más planas:

```python
# MEJOR: Herencia más plana con composición
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre

class Mamifero(Animal):
    def __init__(self, nombre, tipo_alimentacion):
        super().__init__(nombre)
        self.tipo_alimentacion = tipo_alimentacion

class Ave(Animal):
    def __init__(self, nombre, puede_volar):
        super().__init__(nombre)
        self.puede_volar = puede_volar

# Composición para características específicas
class Caracteristica:
    def __init__(self, nombre, valor):
        self.nombre = nombre
        self.valor = valor

perro = Mamifero("Rex", "carnivoro")
perro.agregar_caracteristica = Caracteristica("raza", "Pastor Alemán")
```

### 7.3 Regla práctica

| Profundidad | Recomendación |
|-------------|---------------|
| 1-2 niveles | Ideal, fácil de mantener |
| 3-4 niveles | Aceptable, documentar bien |
| 5+ niveles | [x] Considerar refactorización |
| 7+ niveles | [x] Probablemente mal diseño |

---

## 8. El Método super()

### 8.1 Propósito

`super()` retorna un objeto proxy que delega llamadas de método a la clase padre.

### 8.2 Uso básico

```python
class Persona:
    def __init__(self, nombre):
        self.nombre = nombre

class Empleado(Persona):
    def __init__(self, nombre, salario):
        super().__init__(nombre)  # Llama a Persona.__init__
        self.salario = salario

emp = Empleado("Ana", 50000)
print(emp.nombre)   # Ana
print(emp.salario)  # 50000
```

### 8.3 super() con argumentos

```python
class Figura:
    def __init__(self, color="negro"):
        self.color = color

class Rectangulo(Figura):
    def __init__(self, ancho, alto, color="negro"):
        super().__init__(color)  # Pasa argumentos al padre
        self.ancho = ancho
        self.alto = alto

class Cuadrado(Rectangulo):
    def __init__(self, lado, color="negro"):
        super().__init__(lado, lado, color)  # Llama a Rectangulo

rect = Rectangulo(5, 3, "rojo")
cuad = Cuadrado(4, "azul")

print(rect.color)   # rojo
print(cuad.color)   # azul
```

### 8.4 super() en métodos

```python
class Empleado:
    def calcular_bono(self):
        return self.salario_base * 0.10

class Gerente(Empleado):
    def calcular_bono(self):
        bono_base = super().calcular_bono()  # Obtiene el del padre
        return bono_base + 1000  # Añade extra

class Director(Gerente):
    def calcular_bono(self):
        bono_gerente = super().calcular_bono()  # Obtiene el del gerente
        return bono_gerente + 2000  # Añade más

class Empleado:
    def __init__(self):
        self.salario_base = 50000

director = Director()
director.salario_base = 100000
print(director.calcular_bono())  # 10000 + 1000 + 2000 = 13000
```

---

## 9. Ejercicios Prácticos

### 9.1 Sistema de figuras geométricas

```python
class Figura:
    def __init__(self, color="negro"):
        self.color = color
    
    def area(self):
        raise NotImplementedError("Subclases deben implementar area()")
    
    def perimetro(self):
        raise NotImplementedError("Subclases deben implementar perimetro()")

class Rectangulo(Figura):
    def __init__(self, ancho, alto, color="negro"):
        super().__init__(color)
        self.ancho = ancho
        self.alto = alto
    
    def area(self):
        return self.ancho * self.alto
    
    def perimetro(self):
        return 2 * (self.ancho + self.alto)

class Circulo(Figura):
    def __init__(self, radio, color="negro"):
        super().__init__(color)
        self.radio = radio
    
    def area(self):
        import math
        return math.pi * self.radio ** 2
    
    def perimetro(self):
        import math
        return 2 * math.pi * self.radio

class Triangulo(Figura):
    def __init__(self, base, altura, lado1, lado2, color="negro"):
        super().__init__(color)
        self.base = base
        self.altura = altura
        self.lado1 = lado1
        self.lado2 = lado2
    
    def area(self):
        return (self.base * self.altura) / 2
    
    def perimetro(self):
        return self.base + self.lado1 + self.lado2

class Cuadrado(Rectangulo):
    def __init__(self, lado, color="negro"):
        super().__init__(lado, lado, color)

# Uso
figuras = [
    Rectangulo(5, 3, "rojo"),
    Circulo(4, "azul"),
    Triangulo(6, 4, 5, 5, "verde"),
    Cuadrado(4, "amarillo")
]

for f in figuras:
    print(f"{f.__class__.__name__} ({f.color}): "
          f"area={f.area():.2f}, perimetro={f.perimetro():.2f}")
```

### 9.2 Sistema de archivos

```python
import datetime

class Entrada:
    def __init__(self, nombre):
        self.nombre = nombre
        self.creado = datetime.datetime.now()
    
    def info(self):
        return f"{self.nombre}"

class Archivo(Entrada):
    def __init__(self, nombre, tamano):
        super().__init__(nombre)
        self.tamano = tamano
    
    def info(self):
        return f"{self.nombre} ({self.tamano} bytes)"

class Carpeta(Entrada):
    def __init__(self, nombre):
        super().__init__(nombre)
        self.contenido = []
    
    def agregar(self, entrada):
        self.contenido.append(entrada)
    
    def info(self):
        total = len(self.contenido)
        archivos = sum(1 for e in self.contenido if isinstance(e, Archivo))
        carpetas = total - archivos
        return f"{self.nombre}/ ({archivos} archivos, {carpetas} carpetas)"

class EnlaceSimbolico(Entrada):
    def __init__(self, nombre, objetivo):
        super().__init__(nombre)
        self.objetivo = objetivo
    
    def info(self):
        return f"{self.nombre} -> {self.objetivo}"

# Uso
raiz = Carpeta("proyecto")
src = Carpeta("src")
doc = Carpeta("docs")

src.agregar(Archivo("main.py", 1024))
src.agregar(Archivo("utils.py", 512))
src.agregar(EnlaceSimbolico("app", "main.py"))

doc.agregar(Archivo("README.md", 2048))

raiz.agregar(src)
raiz.agregar(doc)

def mostrar_arbol(entrada, nivel=0):
    indent = "  " * nivel
    print(f"{indent}{entrada.info()}")
    if isinstance(entrada, Carpeta):
        for item in entrada.contenido:
            mostrar_arbol(item, nivel + 1)

mostrar_arbol(raiz)
```

---

## Resumen

```python
# HERENCIA SIMPLE
class Hija(Padre):
    def __init__(self, param_hija, param_padre):
        super().__init__(param_padre)
        self.atributo_hija = param_hija

# HERENCIA MULTIPLE
class Hija(Padre1, Padre2):
    pass

# HERENCIA MULTINIVEL
class Nieto(Padre):
    pass
# Nieto hereda de Padre, que hereda de Abuelo

# super()
super().__init__()  # Llama al padre
super(ClaseHija, self)  # Forma explícita
```

- [+] Herencia = relación "es-un"
- [+] Composición = relación "tiene-un"
- [!] Evitar herencia profunda (wide > deep)
- [!] Cuidado con herencia múltiple y el diamante
- [+] Usar mixins para funcionalidad reutilizable





