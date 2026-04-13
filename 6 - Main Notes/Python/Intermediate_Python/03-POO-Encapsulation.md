
2026-04-12 18:12

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 


# Encapsulation

## 1. Introducción

La encapsulación es uno de los pilares de la POO que consiste en **agrupar datos y métodos** que operan sobre esos datos dentro de una misma unidad (clase), y **restringir el acceso** a algunos componentes del objeto.

### Objetivos de la encapsulación

- [+] Controlar el acceso a los datos
- [+] Proteger la integridad del estado interno
- [+] Ocultar detalles de implementación
- [+] Facilitar el mantenimiento del código

---

## 2. Qué es la Encapsulación

### 2.1 Analogía: El cajero automático

```
┌────────────────────────────────────────┐
│            CAJERO AUTOMÁTICO           │
├────────────────────────────────────────┤
│                                        │
│   Interface pública:                   │
│   ┌─────────────────┐                  │
│   │  Ingresar PIN   │                  │
│   │  Seleccionar    │                  │
│   │  Retirar        │                  │
│   └─────────────────┘                  │
│                                        │
│   Implementación oculta:               │
│   • Circuitos internos                 │
│   • Conexiones a banco                 │
│   • Sensores de dinero                 │
│                                        │
└────────────────────────────────────────┘

No necesitas saber cómo funciona internamente,
solo usas los botones.
```

### 2.2 Sin encapsulación (problema)

[x] Acceso directo a datos:

```python
class CuentaBancaria:
    def __init__(self, titular, saldo):
        self.titular = titular
        self.saldo = saldo  # Cualquiera puede modificar

cuenta = CuentaBancaria("Ana", 1000)
cuenta.saldo = -50000  # Esto no debería ser posible
cuenta.saldo = "mil"   # Esto tampoco
```

### 2.3 Con encapsulación (solución)

[+] Controlando el acceso:

```python
class CuentaBancaria:
    def __init__(self, titular, saldo):
        self.__titular = titular      # Privado
        self.__saldo = saldo          # Privado
    
    # Getters y setters controlados
    def obtener_saldo(self):
        return self.__saldo
    
    def depositar(self, monto):
        if monto > 0:
            self.__saldo += monto
            return True
        return False
    
    def retirar(self, monto):
        if monto > 0 and self.__saldo >= monto:
            self.__saldo -= monto
            return True
        return False

cuenta = CuentaBancaria("Ana", 1000)
print(cuenta.obtener_saldo())  # 1000
cuenta.depositar(500)          # Funciona
# cuenta.__saldo = -50000     # No funciona directamente
```

---

## 3. Encapsulación en Python

[!] Importante: Python NO tiene encapsulación verdadera como Java o C++. Usa convenciones de nombres.

### 3.1 ¿Por qué no es seguridad real?

```python
class Ejemplo:
    def __init__(self):
        self.__privado = 42  # Doble guión bajo
    
    def obtener(self):
        return self.__privado

obj = Ejemplo()
print(obj.obtener())        # 42

# Python "manglea" el nombre, pero Still accesible
print(obj._Ejemplo__privado)  # 42 - Todavía funciona
```

[!] Es una convención social, no seguridad real.

### 3.2 Propósito real de la encapsulación

```
┌──────────────────────────────────────────────────┐
│           PROPÓSITO DE LA ENCAPSULACIÓN          │
├──────────────────────────────────────────────────┤
│                                                  │
│  1. PROteger integridad de datos                 │
│     → Validar antes de asignar                   │
│     → Impedir estados inválidos                  │
│                                                  │
│  2. ABstraer implementación                      │
│     → Usuario solo ve interface                  │
│     → Detalles internos ocultos                  │
│                                                  │
│  3. FACILITAR mantenimiento                      │
│     → Cambiar implementación sin afectar usuarios│
│     → Agregar validaciones en un lugar           │
│                                                  │
│  NO ES: Seguridad contra acceso malicioso        │
│                                                  │
└──────────────────────────────────────────────────┘
```

---

## 4. Public, Protected, Private

### 4.1 Convenciones de nombres

| Tipo | Sintaxis | Significado |
|------|----------|-------------|
| Public | `self.atributo` | Acceso libre |
| Protected | `self._atributo` | Uso interno nomas |
| Private | `self.__atributo` | Name mangling |

### 4.2 Atributos públicos

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre  # Público
        self.edad = edad      # Público

# Acceso total
p = Persona("Ana", 25)
print(p.nombre)     # Ana
p.nombre = "Maria"  # Permitido
```

### 4.3 Atributos protegidos (_atributo)

[!] Convención: "no acceder directamente desde fuera":

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
        self._kilometraje = 0  # Protegido - "cuidado"
    
    def conducir(self, km):
        self._kilometraje += km  # Uso interno
    
    def obtener_kilometraje(self):
        return self._kilometraje

class Coche(Vehiculo):
    def verificar_kilometraje(self):
        # Las subclases SÍ pueden acceder
        if self._kilometraje > 100000:
            return "Necesita mantenimiento"
        return "OK"

coche = Coche("Toyota", "Corolla")
print(coche.marca)  # Toyota

# Técnicamente accesible, pero se desalienta
print(coche._kilometraje)  # 0 - Funciona pero no recomendado
```

### 4.4 Atributos privados (__atributo)

```python
class CuentaBancaria:
    def __init__(self, titular, saldo):
        self.titular = titular
        self.__saldo = saldo  # Privado - name mangling
        self.__transacciones = []
    
    def depositar(self, monto):
        if monto > 0:
            self.__saldo += monto
            self.__registrar_transaccion("deposito", monto)
            return True
        return False
    
    def retirar(self, monto):
        if monto > 0 and self.__saldo >= monto:
            self.__saldo -= monto
            self.__registrar_transaccion("retiro", monto)
            return True
        return False
    
    def obtener_saldo(self):
        return self.__saldo
    
    # Método privado auxiliar
    def __registrar_transaccion(self, tipo, monto):
        self.__transacciones.append((tipo, monto))
    
    def obtener_transacciones(self):
        return self.__transacciones.copy()

cuenta = CuentaBancaria("Ana", 1000)

# Acceso público a través de métodos
print(cuenta.obtener_saldo())  # 1000
cuenta.depositar(500)
print(cuenta.obtener_saldo())  # 1500

# Acceso directo a privado (no recomendado)
# print(cuenta.__saldo)  # AttributeError
print(cuenta._CuentaBancaria__saldo)  # 1500 - Still funciona

# Transacciones
print(cuenta.obtener_transacciones())  # [('deposito', 500)]
```

---

## 5. Propiedades y Decoradores

### 5.1 El problema: getters y setters

[x] Estilo Java (no idiomático en Python):

```python
class Persona:
    def __init__(self, nombre):
        self.set_nombre(nombre)
    
    def get_nombre(self):
        return self.__nombre
    
    def set_nombre(self, nombre):
        if not nombre:
            raise ValueError("Nombre no puede estar vacío")
        self.__nombre = nombre

p = Persona("Ana")
print(p.get_nombre())
p.set_nombre("Maria")
```

### 5.2 La solución: @property

[+] Estilo Python idiomático:

```python
class Persona:
    def __init__(self, nombre):
        self.nombre = nombre  # Llama al setter
    
    @property
    def nombre(self):
        """Getter - obtener nombre."""
        return self.__nombre
    
    @nombre.setter
    def nombre(self, valor):
        """Setter - asignar nombre con validación."""
        if not valor:
            raise ValueError("Nombre no puede estar vacío")
        self.__nombre = valor
    
    @nombre.deleter
    def nombre(self):
        """Deleter - eliminar nombre."""
        del self.__nombre

# Uso - parece acceso directo pero usa getter/setter
p = Persona("Ana")
print(p.nombre)     # Ana - usa getter
p.nombre = "Maria"  # Maria - usa setter
print(p.nombre)     # Maria

# del nombre - usa deleter
del p.nombre
# print(p.nombre)  # AttributeError
```

### 5.3 Propiedades computadas

```python
class Rectangulo:
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    @property
    def area(self):
        """Propiedad calculada - solo lectura."""
        return self.ancho * self.alto
    
    @property
    def perimetro(self):
        return 2 * (self.ancho + self.alto)
    
    @property
    def es_cuadrado(self):
        return self.ancho == self.alto

rect = Rectangulo(5, 3)
print(rect.area)       # 15
print(rect.perimetro) # 16
print(rect.es_cuadrado)  # False

rect.ancho = 5
rect.alto = 5
print(rect.es_cuadrado)  # True
```

### 5.4 Validación con propiedades

```python
class Temperatura:
    def __init__(self, celsius=0):
        self.celsius = celsius
    
    @property
    def celsius(self):
        return self.__celsius
    
    @celsius.setter
    def celsius(self, valor):
        if valor < -273.15:
            raise ValueError("Temperatura no puede ser menor a -273.15°C (cero absoluto)")
        self.__celsius = valor
    
    @property
    def fahrenheit(self):
        return self.__celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, valor):
        self.celsius = (valor - 32) * 5/9
    
    def __str__(self):
        return f"{self.celsius:.1f}°C = {self.fahrenheit:.1f}°F"

temp = Temperatura(25)
print(temp)  # 25.0°C = 77.0°F

temp.fahrenheit = 32
print(temp)  # 0.0°C = 32.0°F

# temp.celsius = -300  # ValueError
```

### 5.5 Atributos de solo lectura

```python
class AutoIncrement:
    _contador = 0
    
    def __init__(self, nombre):
        AutoIncrement._contador += 1
        self.__id = AutoIncrement._contador
        self.nombre = nombre
    
    @property
    def id(self):
        """Solo lectura - no setter."""
        return self.__id
    
    @property
    def total_creados(self):
        return AutoIncrement._contador

a1 = AutoIncrement("Primero")
a2 = AutoIncrement("Segundo")
a3 = AutoIncrement("Tercero")

print(a1.id)  # 1
print(a2.id)  # 2
print(a3.id)  # 3
print(a3.total_creados)  # 3

# a1.id = 99  # AttributeError
```

---

## 6. Cuándo Usar Encapsulación

### 6.1 Guía de decisión

```
┌─────────────────────────────────────────────────────┐
│           ¿CUÁNDO ENCAPSULAR?                       │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ENCAPSULAR cuando:                                 │
│  [+] Necesitas validación antes de asignar          │
│  [+] Necesitas ocultar complejidad interna          │
│  [+] El atributo es calculado, no almacenado        │
│  [+] Quieres control total sobre acceso             │
│  [+] La interfaz puede cambiar en el futuro         │
│                                                     │
│  NO ENCAPSULAR cuando:                              │
│  [x] Es un DTO (Data Transfer Object) simple        │
│  [x] Solo almacena datos sin lógica                 │
│  [x] Over-engineering para casos simples            │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 6.2 Ejemplo práctico

```python
# SOBRE-ingeniería
class Punto2D:
    def __init__(self, x, y):
        self.__x = x
        self.__y = y
    
    @property
    def x(self): return self.__x
    @property
    def y(self): return self.__y
    @x.setter
    def x(self, v): self.__x = v
    @y.setter
    def y(self, v): self.__y = v

p = Punto2D(1, 2)  # Simplismo para esto

# SIMPLE
class Punto2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Punto2D(1, 2)
```

```python
# ENCAPSULACIÓN NECESARIA
class CuentaBancaria:
    def __init__(self, titular, saldo):
        self.__titular = titular
        self.__saldo = saldo
        self.__historial = []
    
    @property
    def saldo(self):
        return self.__saldo
    
    def depositar(self, monto):
        if monto <= 0:
            raise ValueError("Monto debe ser positivo")
        self.__saldo += monto
        self.__historial.append(("deposito", monto))
    
    def retirar(self, monto):
        if monto <= 0:
            raise ValueError("Monto debe ser positivo")
        if monto > self.__saldo:
            raise ValueError("Saldo insuficiente")
        self.__saldo -= monto
        self.__historial.append(("retiro", monto))
```

---

## 7. Ejercicios Prácticos

### 7.1 Clase con validación completa

```python
class Empleado:
    def __init__(self, nombre, salario, departamento):
        self.nombre = nombre
        self.salario = salario
        self.departamento = departamento
    
    @property
    def nombre(self):
        return self.__nombre
    
    @nombre.setter
    def nombre(self, valor):
        if not valor or len(valor.strip()) < 2:
            raise ValueError("Nombre debe tener al menos 2 caracteres")
        self.__nombre = valor.strip()
    
    @property
    def salario(self):
        return self.__salario
    
    @salario.setter
    def salario(self, valor):
        if valor < 0:
            raise ValueError("Salario no puede ser negativo")
        self.__salario = valor
    
    @property
    def departamento(self):
        return self.__departamento
    
    @departamento.setter
    def departamento(self, valor):
        deptos_validos = ["IT", "RRHH", "Ventas", "Marketing", "Administracion"]
        if valor not in deptos_validos:
            raise ValueError(f"Departamento debe ser: {deptos_validos}")
        self.__departamento = valor
    
    def __str__(self):
        return f"{self.nombre} - {self.departamento} - ${self.salario:,.2f}"

# Uso
try:
    emp = Empleado("Ana", 50000, "IT")
    print(emp)
    
    emp.salario = 55000
    print(emp)
    
    # emp.salario = -1000  # ValueError
    # emp.departamento = "Otro"  # ValueError
    
except ValueError as e:
    print(f"Error: {e}")
```

### 7.2 Cache con propiedad

```python
class PaginaWeb:
    def __init__(self, url):
        self.url = url
        self.__contenido_cache = None
        self.__ultimo_acceso = None
    
    @property
    def contenido(self):
        if self.__contenido_cache is None:
            print(f"Descargando {self.url}...")
            self.__contenido_cache = self.__descargar()
            self.__ultimo_acceso = "justo ahora"
        return self.__contenido_cache
    
    def __descargar(self):
        # Simulación de descarga
        return f"Contenido de {self.url}"
    
    def invalidar_cache(self):
        self.__contenido_cache = None
    
    @property
    def desde_cache(self):
        return self.__contenido_cache is not None

# Uso
pagina = PaginaWeb("https://ejemplo.com")

print(pagina.contenido)  # Descarga...
print(pagina.contenido)  # Usa cache
print(pagina.desde_cache)  # True

pagina.invalidar_cache()
print(pagina.contenido)  # Descarga de nuevo
```

---

## Resumen

| Modificador | Sintaxis | Acceso | Uso |
|-------------|----------|--------|-----|
| Public | `self.x` | Total | Atributos normales |
| Protected | `self._x` | Dentro de clase y subclases | Atributos internos |
| Private | `self.__x` | Solo dentro de clase | Atributos realmente privados |

```python
class Ejemplo:
    PUBLICO = "accesible"
    _PROTEGIDO = "usar con cuidado"
    __PRIVADO = "muy protegido"
    
    def __init__(self):
        self.publico = "libre"
        self._protegido = "interno"
        self.__privado = "secreto"
    
    @property
    def propiedad(self):
        """Getter con validación."""
        return self.__privado
    
    @propiedad.setter
    def propiedad(self, valor):
        """Setter con validación."""
        self.__privado = valor
```

- [+] Usa `property` para interface controlada
- [+] Usa `_` para atributos internos de clase
- [+] Usa `__` para encapsulación fuerte
- [!] Python no tiene seguridad real - es convención
- [!] No sobre-ingenierices con properties para todo




