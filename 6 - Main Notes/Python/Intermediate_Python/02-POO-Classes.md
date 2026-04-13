
2026-04-12 18:09

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 


# Classes

## 1. Introducción

Una **clase** es un plano o plantilla que define la estructura y el comportamiento de objetos. Es el núcleo de la Programación Orientada a Objetos (POO).

### Conceptos clave

- **Clase**: Define un tipo de objeto (como un blueprint)
- **Objeto/Instancia**: Una implementación específica de una clase
- **Atributos**: Datos que pertenecen a la clase u objeto
- **Métodos**: Funciones que pertenecen a la clase u objeto

---

## 2. Qué es una Clase

### 2.1 Estructura básica

```python
class NombreClase:
    """Documentación de la clase."""
    
    # Atributos de clase
    atributo_clase = "valor"
    
    def __init__(self, parametros):
        """Constructor - se llama al crear un objeto."""
        # Atributos de instancia
        self.atributo_instancia = parametros
    
    def metodo(self):
        """Método de instancia."""
        return "comportamiento"
```

### 2.2 Ejemplo: La clase Persona

```python
class Persona:
    """Representa una persona."""
    
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    def saludar(self):
        return f"Hola, soy {self.nombre}"

# Crear un objeto (instancia)
persona1 = Persona("Ana", 25)
print(persona1.nombre)   # Ana
print(persona1.edad)     # 25
print(persona1.saludar())  # Hola, soy Ana
```

### 2.3 Anatomía de una clase

```
┌─────────────────────────────────────┐
│            class Persona            │
├─────────────────────────────────────┤
│  Atributos de CLASE                 │
│  (compartidos por todas las         │
│   instancias)                       │
├─────────────────────────────────────┤
│  Atributos de INSTANCIA             │
│  (únicos para cada objeto)          │
│  - nombre                           │
│  - edad                             │
├─────────────────────────────────────┤
│  MÉTODOS                            │
│  - saludar()                        │
│  - cumpleaños()                     │
└─────────────────────────────────────┘
```

---

## 3. Métodos

### 3.1 Tipos de métodos

| Tipo | Primer parámetro | Descripción |
|------|------------------|-------------|
| Instancia | `self` | Opera sobre instancia específica |
| Clase | `cls` | Opera sobre la clase misma |
| Estático | Ninguno | Función normal sin acceso especial |

### 3.2 Métodos de instancia

```python
class Coche:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
        self.encendido = False
    
    # Método de instancia - recibe self
    def encender(self):
        self.encendido = True
        return f"{self.marca} {self.modelo} encendido"
    
    def conducir(self):
        if self.encendido:
            return f"Conduciendo {self.marca}"
        return "Primero enciende el coche"
```

### 3.3 Métodos de clase

[+] Útiles para constructores alternativos o modificar estado de clase:

```python
class Persona:
    especie = "Humano"  # Atributo de clase
    
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    # Método de clase - recibe cls
    @classmethod
    def desde_string(cls, datos):
        """Crea Persona desde string 'nombre-edad'."""
        nombre, edad = datos.split("-")
        return cls(nombre, int(edad))
    
    # Método de clase - crear persona con edad por defecto
    @classmethod
    def anonimo(cls):
        return cls("Sin nombre", 0)

# Uso de métodos de clase
p1 = Persona("Ana", 25)
p2 = Persona.desde_string("Luis-30")
p3 = Persona.anonimo()

print(p2.nombre)  # Luis
print(p3.nombre)  # Sin nombre
```

### 3.4 Métodos estáticos

[!] No necesitan acceso a la clase ni instancia:

```python
class Calculadora:
    @staticmethod
    def sumar(a, b):
        """Suma sin necesidad de instancia."""
        return a + b
    
    @staticmethod
    def es_par(numero):
        return numero % 2 == 0

# Uso sin crear instancia
print(Calculadora.sumar(2, 3))  # 5
print(Calculadora.es_par(4))    # True
```

### 3.5 Métodos pueden retornar cualquier cosa

```python
class Operacion:
    def __init__(self, valor):
        self.valor = valor
    
    # Retornar número
    def duplicar(self):
        return self.valor * 2
    
    # Retornar string
    def describir(self):
        return f"El valor es {self.valor}"
    
    # Retornar booleano
    def es_positivo(self):
        return self.valor > 0
    
    # Retornar lista
    def divisores(self):
        return [i for i in range(1, self.valor + 1) if self.valor % i == 0]
    
    # Retornar diccionario
    def informacion(self):
        return {
            "valor": self.valor,
            "duplicado": self.duplicar(),
            "positivo": self.es_positivo()
        }
    
    # Retornar otra instancia
    def inverso(self):
        return Operacion(-self.valor)
    
    # Retornar None (comportamiento)
    def incrementar(self):
        self.valor += 1

op = Operacion(10)
print(op.duplicar())      # 20
print(op.describir())     # El valor es 10
print(op.es_positivo())   # True
print(op.divisores())     # [1, 2, 5, 10]
print(op.informacion())   # {'valor': 10, 'duplicado': 20, 'positivo': True}
print(op.inverso().valor) # -10
```

### 3.6 Métodos vs Funciones

| Aspecto | Método | Función |
|---------|--------|---------|
| Definición | Dentro de una clase | Fuera de una clase |
| Parámetro especial | `self` o `cls` | No tiene |
| Acceso | Puede acceder a atributos de clase/instancia | Solo recibe parámetros |
| Llamada | `objeto.metodo()` | `funcion(param)` |

```python
# FUNCIÓN
def area_rectangulo(ancho, alto):
    return ancho * alto

# MÉTODO
class Rectangulo:
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def area(self):
        return self.ancho * self.alto

# Uso
print(area_rectangulo(5, 3))  # 15 - Función

rect = Rectangulo(5, 3)
print(rect.area())            # 15 - Método
```

[!] Los métodos tienen acceso al estado del objeto, las funciones no.

---

## 4. Constructores

### 4.1 El método __init__

El constructor se llama automáticamente al crear un objeto:

```python
class Libro:
    def __init__(self, titulo, autor, paginas=0):
        self.titulo = titulo
        self.autor = autor
        self.paginas = paginas
        self.disponible = True

libro = Libro("Cien años de soledad", "Gabriel Garcia Marquez", 417)
print(libro.titulo)  # Cien años de soledad
```

### 4.2 Constructor con validación

```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial=0):
        if saldo_inicial < 0:
            raise ValueError("El saldo no puede ser negativo")
        
        self.titular = titular
        self.__saldo = saldo_inicial
    
    @property
    def saldo(self):
        return self.__saldo

# Uso
cuenta = CuentaBancaria("Ana", 100)
print(cuenta.saldo)  # 100

# Esto genera error:
# CuentaBancaria("Luis", -50)  # ValueError
```

### 4.3 Constructor con valores por defecto

```python
class Empleado:
    def __init__(
        self,
        nombre,
        departamento="General",
        salario=0,
        activo=True
    ):
        self.nombre = nombre
        self.departamento = departamento
        self.salario = salario
        self.activo = activo

# Todas estas formas son válidas
emp1 = Empleado("Ana")
emp2 = Empleado("Luis", "IT")
emp3 = Empleado("Maria", "Ventas", 30000)
emp4 = Empleado("Pedro", activo=False, salario=25000)
```

### 4.4 Constructor secundario (__new__)

[!] Raramente necesario, pero útil para casos especiales:

```python
class Singleton:
    _instancia = None
    
    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
        return cls._instancia

# Siempre retorna la misma instancia
a = Singleton()
b = Singleton()
print(a is b)  # True
```

---

## 5. Variables de Clase vs Instancia

### 5.1 Variables de instancia

[+] Unicas para cada objeto:

```python
class Persona:
    def __init__(self, nombre):
        self.nombre = nombre  # Variable de instancia

ana = Persona("Ana")
luis = Persona("Luis")

print(ana.nombre)   # Ana
print(luis.nombre)  # Luis
ana.nombre = "Ana Maria"
print(ana.nombre)   # Ana Maria
print(luis.nombre)  # Luis (sin cambios)
```

### 5.2 Variables de clase

[+] Compartidas por todas las instancias:

```python
class Persona:
    especie = "Humano"  # Variable de clase
    
    def __init__(self, nombre):
        self.nombre = nombre  # Variable de instancia

ana = Persona("Ana")
luis = Persona("Luis")

print(ana.especie)   # Humano
print(luis.especie)   # Humano
Persona.especie = "Ser pensante"
print(ana.especie)   # Ser pensante
print(luis.especie)   # Ser pensante
```

### 5.3 Diagrama de memoria

```
┌─────────────────────────────────────────┐
│              class Persona              │
│  especie = "Humano"  (compartido)       │
├─────────────────────────────────────────┤
│                                         │
│  ┌─────────────┐    ┌─────────────┐     │
│  │  ana        │    │  luis       │     │
│  │  nombre=    │    │  nombre=    │     │
│  │   "Ana"     │    │   "Luis"    │     │
│  └─────────────┘    └─────────────┘     │
│                                         │
│  Ambos acceden a Persona.especie        │
└─────────────────────────────────────────┘
```

### 5.4 Cuando usar cada una

```python
class Coche:
    # [+] Variables de clase para constantes
    CANTIDAD_RUEDAS = 4
    COLOR_POR_DEFECTO = "blanco"
    
    def __init__(self, marca, modelo, color=None):
        # [+] Variables de instancia para datos únicos
        self.marca = marca
        self.modelo = modelo
        self.color = color or self.COLOR_POR_DEFECTO
        self.kilometraje = 0
    
    def conducir(self, km):
        self.kilometraje += km

# Uso
coche1 = Coche("Toyota", "Corolla", "rojo")
coche2 = Coche("Honda", "Civic")

print(coche1.CANTIDAD_RUEDAS)  # 4
print(coche2.CANTIDAD_RUEDAS)  # 4
print(coche1.color)  # rojo
print(coche2.color)  # blanco (por defecto)
```

### 5.5 Cuidado con variables mutables

[!] No uses listas o diccionarios como variables de clase:

```python
# MAL - Lista compartida
class Lista:
    valores = []  # Compartida por todas las instancias

a = Lista()
b = Lista()
a.valores.append(1)
print(b.valores)  # [1] - ¡También se modificó!
```

```python
# BIEN - Lista en __init__
class Lista:
    def __init__(self):
        self.valores = []  # Cada instancia tiene la suya

a = Lista()
b = Lista()
a.valores.append(1)
print(b.valores)  # [] - Correcto
```

---

## 6. Múltiples Objetos

### 6.1 Crear múltiples instancias

```python
class Estudiante:
    def __init__(self, nombre, nota):
        self.nombre = nombre
        self.nota = nota
    
    def aprobo(self):
        return self.nota >= 6

# Crear múltiples objetos
estudiantes = []
for datos in [("Ana", 8), ("Luis", 5), ("Maria", 7), ("Pedro", 9)]:
    estudiantes.append(Estudiante(datos[0], datos[1]))

# Procesar todos
for est in estudiantes:
    estado = "Aprobó" if est.aprobo() else "Reprobó"
    print(f"{est.nombre}: {estado}")
```

### 6.2 Colecciones de objetos

```python
class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio
    
    def descuento(self, porcentaje):
        return self.precio * (1 - porcentaje / 100)

class Carrito:
    def __init__(self):
        self.productos = []
    
    def agregar(self, producto, cantidad=1):
        self.productos.append({"producto": producto, "cantidad": cantidad})
    
    def total(self):
        return sum(
            p["producto"].precio * p["cantidad"]
            for p in self.productos
        )

# Uso
p1 = Producto("Manzana", 1.50)
p2 = Producto("Pan", 2.00)
p3 = Producto("Leche", 1.20)

carrito = Carrito()
carrito.agregar(p1, 5)
carrito.agregar(p2, 2)
carrito.agregar(p3)

print(f"Total: ${carrito.total():.2f}")  # Total: $13.60
```

### 6.3 Objetos que referencian otros objetos

```python
class Direccion:
    def __init__(self, calle, ciudad, pais):
        self.calle = calle
        self.ciudad = ciudad
        self.pais = pais
    
    def __str__(self):
        return f"{self.calle}, {self.ciudad}, {self.pais}"

class Persona:
    def __init__(self, nombre, direccion):
        self.nombre = nombre
        self.direccion = direccion

# Crear objetos relacionados
dir1 = Direccion("Calle Mayor 1", "Madrid", "España")
persona = Persona("Ana", dir1)

print(persona.nombre)           # Ana
print(persona.direccion)        # <Direccion object>
print(persona.direccion.ciudad) # Madrid
print(persona.direccion)       # Calle Mayor 1, Madrid, España
```

---

## 7. Ejercicios Prácticos

### 7.1 Sistema de bibliothèque

```python
class Libro:
    # Variables de clase
    FORMATOS_DISPONIBLES = ["digital", "fisico", "audiolibro"]
    
    def __init__(self, titulo, autor, isbn, formato="fisico"):
        self.titulo = titulo
        self.autor = autor
        self.isbn = isbn
        if formato not in self.FORMATOS_DISPONIBLES:
            raise ValueError(f"Formato debe ser uno de {self.FORMATOS_DISPONIBLES}")
        self.formato = formato
        self.prestado = False
    
    def __str__(self):
        return f"{self.titulo} por {self.autor}"
    
    def prestar(self):
        if self.prestado:
            return False
        self.prestado = True
        return True
    
    def devolver(self):
        self.prestado = False
    
    def esta_disponible(self):
        return not self.prestado

class Biblioteca:
    def __init__(self, nombre):
        self.nombre = nombre
        self.libros = []
    
    def agregar_libro(self, libro):
        self.libros.append(libro)
    
    def buscar_por_titulo(self, titulo):
        return [l for l in self.libros if titulo.lower() in l.titulo.lower()]
    
    def libros_disponibles(self):
        return [l for l in self.libros if l.esta_disponible()]
    
    def prestar_libro(self, isbn):
        for libro in self.libros:
            if libro.isbn == isbn:
                return libro.prestar()
        return None

# Uso
bib = Biblioteca("Municipal")
bib.agregar_libro(Libro("1984", "George Orwell", "978-0451524935"))
bib.agregar_libro(Libro("Don Quixote", "Cervantes", "978-0060935467"))

disponibles = bib.libros_disponibles()
print(f"Tenemos {len(disponibles)} libros disponibles")
for libro in disponibles:
    print(f"  - {libro}")
```

### 7.2 Gestor de tareas

```python
class Tarea:
    PRIORIDADES = ["baja", "media", "alta", "urgente"]
    
    def __init__(self, titulo, prioridad="media"):
        self.titulo = titulo
        if prioridad not in self.PRIORIDADES:
            raise ValueError(f"Prioridad debe ser: {self.PRIORIDADES}")
        self.prioridad = prioridad
        self.completada = False
    
    def completar(self):
        self.completada = True
    
    def __str__(self):
        estado = "[x]" if self.completada else "[ ]"
        return f"{estado} {self.titulo} ({self.prioridad})"

class GestorTareas:
    def __init__(self):
        self.tareas = []
    
    def agregar(self, titulo, prioridad="media"):
        tarea = Tarea(titulo, prioridad)
        self.tareas.append(tarea)
        return tarea
    
    def pendientes(self):
        return [t for t in self.tareas if not t.completada]
    
    def completar(self, titulo):
        for tarea in self.tareas:
            if tarea.titulo == titulo:
                tarea.completar()
                return True
        return False
    
    def por_prioridad(self):
        prioridad_orden = {p: i for i, p in enumerate(Tarea.PRIORIDADES)}
        return sorted(
            self.tareas,
            key=lambda t: prioridad_orden[t.prioridad],
            reverse=True
        )
    
    def estadisticas(self):
        total = len(self.tareas)
        completadas = sum(1 for t in self.tareas if t.completada)
        pendientes = total - completadas
        return {
            "total": total,
            "completadas": completadas,
            "pendientes": pendientes,
            "porcentaje": (completadas / total * 100) if total else 0
        }

# Uso
gestor = GestorTareas()
gestor.agregar("Comprar groceries", "alta")
gestor.agregar("Llamar al médico", "urgente")
gestor.agregar("Leer libro", "baja")
gestor.completar("Comprar groceries")

print("Todas las tareas:")
for tarea in gestor.por_prioridad():
    print(f"  {tarea}")

print("\nEstadísticas:")
stats = gestor.estadisticas()
print(f"  Total: {stats['total']}")
print(f"  Completadas: {stats['completadas']}")
print(f"  Pendientes: {stats['pendientes']}")
print(f"  Progreso: {stats['porcentaje']:.1f}%")
```

---

## Resumen

```python
class MiClase:
    # Variable de clase - compartida
    atributo_clase = "valor"
    
    def __init__(self, param):
        # Variable de instancia - única
        self.atributo_instancia = param
    
    # Método de instancia
    def metodo_instancia(self):
        return self.atributo_instancia
    
    # Método de clase
    @classmethod
    def metodo_clase(cls):
        return cls.atributo_clase
    
    # Método estático
    @staticmethod
    def metodo_estatico():
        return "sin acceso a self/cls"
```

- **Variable de instancia**: Unica por objeto
- **Variable de clase**: Compartida por todas las instancias
- **Método de instancia**: Recibe `self`, accede a todo
- **Método de clase**: Recibe `cls`, opera sobre la clase
- **Método estático**: No recibe nada especial




