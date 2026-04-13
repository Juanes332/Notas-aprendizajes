
2026-04-12 16:14

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 


# Clean Code & DRY Code

---

## 1. Introducción

La calidad del código no solo se mide por si funciona, sino por lo fácil que es leer, mantener y extender. **Clean Code** y **DRY Code** son dos principios fundamentales que todo programador debe dominar.

### ¿Por qué importan?

- [+] Código limpio es más fácil de mantener y debuggear
- [+] Reduce la probabilidad de errores
- [+] Facilita el trabajo en equipo
- [+] Disminuye la deuda técnica

---

## 2. Clean Code

### 2.1 Nombres significativos

[x] Malos nombres que no dicen nada:

```python
# Nombres vaga
x = 5
d = {"n": "Ana", "e": 25}
def fn(a, b):
    return a + b
```

[+] Buenos nombres que describen el propósito:

```python
# Nombres descriptivos
edad_usuario = 5
empleado = {"nombre": "Ana", "edad": 25}
def sumar(a, b):
    return a + b
```

### 2.2 Funciones pequeñas y con un solo propósito

[x] Función que hace muchas cosas:

```python
def procesar_usuario(usuario):
    # Validar
    if not usuario.get("nombre"):
        return False
    if not usuario.get("email"):
        return False
    # Guardar en BD
    conexion = conectar_db()
    cursor = conexion.cursor()
    cursor.execute("INSERT INTO usuarios VALUES (?)", (usuario,))
    conexion.commit()
    # Enviar email
    enviar_email(usuario["email"], "Bienvenido")
    # Actualizar cache
    cache.set(usuario["nombre"], usuario)
    return True
```

[+] Funciones pequeñas con un propósito:

```python
def validar_usuario(usuario):
    return bool(usuario.get("nombre")) and bool(usuario.get("email"))

def guardar_usuario_db(usuario):
    conexion = conectar_db()
    cursor = conexion.cursor()
    cursor.execute("INSERT INTO usuarios VALUES (?)", (usuario,))
    conexion.commit()

def notificar_bienvenida(email):
    enviar_email(email, "Bienvenido")

def procesar_usuario(usuario):
    if not validar_usuario(usuario):
        return False
    guardar_usuario_db(usuario)
    notificar_bienvenida(usuario["email"])
    cache.set(usuario["nombre"], usuario)
    return True
```

### 2.3 Comentarios útiles

[x] Comentarios inútiles o redundantes:

```python
# Incrementar i en 1
i = i + 1

# Obtener la lista de usuarios
usuarios = obtener_todos_los_usuarios()

# Si x es mayor que 0
if x > 0:
    pass
```

[+] Comentarios que explican el "por qué":

```python
# El algoritmo de ordenamiento rápido es mas rapido para listas grandes
# 参考: https://wiki.python.org/timsort
usuarios = sorted(usuarios, key=lambda u: u.nombre)

# Timeout de 5 segundos para dar tiempo a reconexion
# Ver: https://github.com/proyecto/issues/123
socket.settimeout(5.0)
```

### 2.4 Evitar números mágicos

[x] Números sin contexto:

```python
def calcular_precio(cantidad):
    if cantidad > 100:
        return cantidad * 0.8
    elif cantidad > 50:
        return cantidad * 0.9
    elif cantidad > 20:
        return cantidad * 0.95
    return cantidad * 1.0
```

[+] Constantes con nombres claros:

```python
DESCUENTO_10_UNIDADES = 1.0
DESCUENTO_20_UNIDADES = 0.95
DESCUENTO_50_UNIDADES = 0.90
DESCUENTO_100_UNIDADES = 0.80

def calcular_precio(cantidad):
    if cantidad > 100:
        return cantidad * DESCUENTO_100_UNIDADES
    elif cantidad > 50:
        return cantidad * DESCUENTO_50_UNIDADES
    elif cantidad > 20:
        return cantidad * DESCUENTO_20_UNIDADES
    return cantidad * DESCUENTO_10_UNIDADES
```

### 2.5 Formato consistente

[+] Sigue las convenciones de Python (PEP 8):

```python
# Espacios alrededor de operadores
resultado = (a + b) * c

# No espacios dentro de paréntesis
funcion(param1, param2)

# Dos líneas entre funciones
def funcion_uno():
    pass


def funcion_dos():
    pass
```

[!] Usa herramientas como `black` o `ruff` para formateo automático.

---

## 3. DRY Code

### 3.1 ¿Qué es DRY?

**Don't Repeat Yourself** - No te repitas. Cada pieza de conocimiento debe tener una representación única y libre de ambigüedades.

### 3.2 Duplicación de código

[x] Código repetido:

```python
# Calcular área de rectángulo
area_1 = 10 * 20
area_2 = 15 * 25
area_3 = 30 * 40

# Calcular perímetro de rectángulo
perimetro_1 = 2 * 10 + 2 * 20
perimetro_2 = 2 * 15 + 2 * 25
perimetro_3 = 2 * 30 + 2 * 40

# Calcular volumen de prisma rectangular
volumen_1 = 10 * 20 * 5
volumen_2 = 15 * 25 * 8
volumen_3 = 30 * 40 * 12
```

[+] Código DRY:

```python
def area_rectangulo(ancho, alto):
    return ancho * alto

def perimetro_rectangulo(ancho, alto):
    return 2 * ancho + 2 * alto

def volumen_prisma(ancho, alto, profundidad):
    return area_rectangulo(ancho, alto) * profundidad

# Uso
area_1 = area_rectangulo(10, 20)
area_2 = area_rectangulo(15, 25)
area_3 = area_rectangulo(30, 40)

perimetro_1 = perimetro_rectangulo(10, 20)
perimetro_2 = perimetro_rectangulo(15, 25)
perimetro_3 = perimetro_rectangulo(30, 40)

volumen_1 = volumen_prisma(10, 20, 5)
volumen_2 = volumen_prisma(15, 25, 8)
volumen_3 = volumen_prisma(30, 40, 12)
```

### 3.3 Variables en lugar de repetición

[x] Repetir valores:

```python
def crear_usuario(nombre, email):
    return {
        "nombre": nombre,
        "email": email,
        "activo": True,
        "fecha_creacion": "2024-01-01",
        "rol": "usuario"
    }

def crear_admin(nombre, email):
    return {
        "nombre": nombre,
        "email": email,
        "activo": True,
        "fecha_creacion": "2024-01-01",
        "rol": "admin"
    }
```

[+] Usar valores por defecto:

```python
USUARIO_DEFAULT = {
    "activo": True,
    "fecha_creacion": "2024-01-01",
    "rol": "usuario"
}

def crear_usuario(nombre, email, rol="usuario"):
    return {
        **USUARIO_DEFAULT,
        "nombre": nombre,
        "email": email,
        "rol": rol
    }
```

### 3.4 DRY vs WET

| Concepto | Descripción |
|----------|-------------|
| DRY | Don't Repeat Yourself - Cada conocimiento en un solo lugar |
| WET | Write Everything Twice - Código duplicado intencional |

[!] DRY no significa eliminar toda duplicación. A veces la repetición es aceptable:
- Tests duplicados para claridad
- Código simple que no justifica abstracción
- Casos específicos que no comparten lógica

---

## 4. Aplicación en POO

### 4.1 Clases bien nombradas

[x] Nombres vagos:

```python
class Data:
    pass

class Manager:
    pass

class Handler:
    pass
```

[+] Nombres descriptivos:

```python
class DatosPersona:
    pass

class GestorInventario:
    pass

class ManejadorEventos:
    pass
```

### 4.2 Métodos con responsabilidad única

[x] Clase que hace demasiado:

```python
class Usuario:
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email
    
    def guardar(self):
        # Guardar en BD
        pass
    
    def enviar_email(self, mensaje):
        # Enviar email
        pass
    
    def validar(self):
        # Validar datos
        pass
    
    def calcular_estadisticas(self):
        # Calcular estadísticas
        pass
```

[+] Clases con responsabilidad única:

```python
class Usuario:
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email

class UsuarioRepository:
    def guardar(self, usuario):
        pass

class ValidadorUsuario:
    def validar(self, usuario):
        pass

class NotificadorUsuario:
    def enviar(self, usuario, mensaje):
        pass
```

### 4.3 Evitar duplicación en clases

[x] Propiedades duplicadas:

```python
class Circulo:
    def __init__(self, radio):
        self.radio = radio
        self.diametro = radio * 2
    
    def area(self):
        return 3.14159 * self.radio ** 2
    
    def perimetro(self):
        return 2 * 3.14159 * self.radio

class Esfera:
    def __init__(self, radio):
        self.radio = radio
        self.diametro = radio * 2
    
    def volumen(self):
        return (4/3) * 3.14159 * self.radio ** 3
    
    def area_superficial(self):
        return 4 * 3.14159 * self.radio ** 2
```

[+] Compartir lógica:

```python
PI = 3.14159

class FiguraCircular:
    def __init__(self, radio):
        self.radio = radio
    
    @property
    def diametro(self):
        return self.radio * 2
    
    def area(self):
        return PI * self.radio ** 2

class Circulo(FiguraCircular):
    def perimetro(self):
        return 2 * PI * self.radio

class Esfera(FiguraCircular):
    def volumen(self):
        return (4/3) * PI * self.radio ** 3
    
    def area_superficial(self):
        return 4 * PI * self.radio ** 2
```

### 4.4 Constantes de clase

[x] Valores hardcodeados:

```python
class Empleado:
    def __init__(self, nombre, salario_base):
        self.nombre = nombre
        self.salario_base = salario_base
    
    def calcular_bono(self):
        return self.salario_base * 0.10
    
    def calcular_vacaciones(self):
        return 15
```

[+] Constantes de clase:

```python
class Empleado:
    BONO_PORCENTAJE = 0.10
    DIAS_VACACIONES_POR_DEFECTO = 15
    
    def __init__(self, nombre, salario_base):
        self.nombre = nombre
        self.salario_base = salario_base
    
    def calcular_bono(self):
        return self.salario_base * self.BONO_PORCENTAJE
    
    def calcular_vacaciones(self):
        return self.DIAS_VACACIONES_POR_DEFECTO
```

---

## 5. Ejercicios Prácticos

### 5.1 Refactorizar código duplicado

```python
# ANTES: Código duplicado
class CuentaAhorros:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo
    
    def depositar(self, monto):
        if monto > 0:
            self.saldo += monto
            return True
        return False
    
    def retirar(self, monto):
        if monto > 0 and self.saldo >= monto:
            self.saldo -= monto
            return True
        return False
    
    def mostrar_saldo(self):
        print(f"Saldo: {self.saldo}")

class CuentaCorriente:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo
    
    def depositar(self, monto):
        if monto > 0:
            self.saldo += monto
            return True
        return False
    
    def retirar(self, monto):
        if monto > 0 and self.saldo >= monto:
            self.saldo -= monto
            return True
        return False
    
    def mostrar_saldo(self):
        print(f"Saldo: {self.saldo}")

# DESPUÉS: Código DRY con herencia
class CuentaBancaria:
    TASA_INTERES = 0.0
    
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo
    
    def depositar(self, monto):
        if monto > 0:
            self.saldo += monto
            return True
        return False
    
    def retirar(self, monto):
        if monto > 0 and self.saldo >= monto:
            self.saldo -= monto
            return True
        return False
    
    def mostrar_saldo(self):
        print(f"Saldo: {self.saldo}")
    
    def aplicar_interes(self):
        self.saldo += self.saldo * self.TASA_INTERES

class CuentaAhorros(CuentaBancaria):
    TASA_INTERES = 0.02

class CuentaCorriente(CuentaBancaria):
    TASA_INTERES = 0.0
```

### 5.2 Extraer constantes

```python
# ANTES: Números mágicos
class Carrito:
    def __init__(self):
        self.items = []
        self.descuento = 0.15
    
    def agregar(self, producto, cantidad, precio):
        subtotal = cantidad * precio
        if cantidad > 10:
            subtotal *= 0.85
        elif cantidad > 5:
            subtotal *= 0.90
        self.items.append({
            "producto": producto,
            "cantidad": cantidad,
            "precio": precio,
            "subtotal": subtotal
        })
    
    def total(self):
        return sum(item["subtotal"] for item in self.items) * (1 - self.descuento)

# DESPUÉS: Código limpio con constantes
class Carrito:
    DESCUENTO_MAYORISTA = 0.15
    CANTIDAD_DESCUENTO_10 = 10
    CANTIDAD_DESCUENTO_5 = 5
    PORCENTAJE_DESC_10 = 0.85
    PORCENTAJE_DESC_5 = 0.90
    
    def __init__(self):
        self.items = []
        self.descuento = 0.15
    
    def calcular_subtotal(self, cantidad, precio):
        if cantidad > self.CANTIDAD_DESCUENTO_10:
            return cantidad * precio * self.PORCENTAJE_DESC_10
        elif cantidad > self.CANTIDAD_DESCUENTO_5:
            return cantidad * precio * self.PORCENTAJE_DESC_5
        return cantidad * precio
    
    def agregar(self, producto, cantidad, precio):
        subtotal = self.calcular_subtotal(cantidad, precio)
        self.items.append({
            "producto": producto,
            "cantidad": cantidad,
            "precio": precio,
            "subtotal": subtotal
        })
    
    def total(self):
        return sum(item["subtotal"] for item in self.items) * (1 - self.descuento)
```

---

## Resumen

- **Clean Code**: Código legible, con nombres descriptivos y propósito único
- **DRY**: No repetir conocimiento, extraer lógica común
- [+] Aplica ambos principios desde el inicio del proyecto
- [!] DRY no es excusa para crear abstracciones prematuras
- [+] Prioriza la claridad sobre la cleveridad



