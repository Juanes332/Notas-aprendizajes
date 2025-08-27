---
id: 2025061001
tags:
  - coding
  - Python
type: permanent
author: Juan Hernandez
---

## 📗 Programación Orientada a Objetos (POO): idea general

POO modela **entidades** como **objetos** con **estado** (atributos) y **comportamiento** (métodos). En Python, _todo_ es objeto. Los pilares: **abstracción, encapsulamiento, herencia y polimorfismo**. La **cohesión** y el **acoplamiento** guían el buen diseño.


Elementos en python - Como identificar objetos

```python
a = 4
b = 64
c = 9
d = "Hi"

def area(x):
	return x ** 2

area(3)

"""
a -> identifiers
4 -> Object

objects:
4, 64, 9, "Hi", function definition, 2, 3

identifiers:
a, b, c, d, area, x

operators:
** 

delimeters:
=, "", (), :, []

keywords:
def, return

comments: # 

blank lines

white space

identation
"""
```

Un programa muestra un mensaje donde se le dice al usuario las coordenadas de un rectángulo. El usuario debe ingresar las coordenadas de un punto dentro del rectángulo

objetos: Mensaje, coordenadas, rectángulo, punto

---

## Pasos para crear un programa profesional

1. Escribir los objetos
2. Escribir una clase por cada objeto
3. Escribir métodos para cada clase
4. Llamar las clases y sus métodos

### 📗 Creando una clase

```python
class Libro:
    """Entidad del dominio: un libro a la venta."""
    def __init__(self, titulo: str, autor: str, precio: float, isbn: str):
        self.titulo = titulo
        self.autor = autor
        self.precio = precio
        self.isbn = isbn

    def aplicar_descuento(self, porcentaje: float) -> None:
        """Reduce el precio en 'porcentaje' (0-100)."""
        if not (0 <= porcentaje <= 100):
            raise ValueError("Porcentaje inválido")
        self.precio *= (1 - porcentaje/100)



    def info(self) -> str:
        return f"{self.titulo} — {self.autor} (${self.precio:.2f})"

# Uso
l = Libro("Clean Code", "Robert C. Martin", 100.0, "9780132350884")
l.aplicar_descuento(10)
print(l.info())  # Clean Code — Robert C. Martin ($90.00)
```

### Que es Self?
En Python, `self` ==es una referencia a la instancia actual de una clase==. Se utiliza para acceder a los atributos y métodos de la instancia dentro de los métodos de la clase. Es una convención en Python, y aunque se puede usar otro nombre, se recomienda seguir esta convención para la legibilidad del código. Esta palabra se usa como estándar para crear el constructor, pude ser cualquiera pero vamos a ver a muchos desarrolladores usando self (ya hace parte del PEP 8)

---

### 📗 Encapsulamiento

Controla cómo se accede al estado interno. En Python no hay _private_ estricto, pero se usan convenciones y _name mangling_.

```python
class Inventario:
    def __init__(self):
        self._articulos = {}        # convención: "protegido"
        self.__bloqueado = False    # "privado" (name-mangled)

    def agregar(self, libro: Libro, unidades: int = 1):
        if self.__bloqueado:
            raise RuntimeError("Inventario bloqueado")
        self._articulos[libro.isbn] = self._articulos.get(libro.isbn, 0) + unidades

    def bloquear(self):   self.__bloqueado = True
    def desbloquear(self): self.__bloqueado = False

    def stock(self, isbn: str) -> int:
        return self._articulos.get(isbn, 0)

```

---

### 📙 Tipos de métodos (instancia, clase y estático)

```python
import datetime as _dt

class Pedido:
    IVA = 0.19  # atributo de clase

    def __init__(self, usuario: str):
        self.usuario = usuario
        self.items: list[tuple[Libro, int]] = []
        self.creado_en = _dt.datetime.now()

    # Método de instancia (usa self)
    def agregar_item(self, libro: Libro, cantidad: int = 1):
        self.items.append((libro, cantidad))

    # Método de clase (usa cls y accede a estado/constantes de clase)
    @classmethod
    def con_item_unico(cls, usuario: str, libro: Libro, cantidad: int = 1) -> "Pedido":
        p = cls(usuario)
        p.agregar_item(libro, cantidad)
        return p

    # Método estático (utilidad que no usa self ni cls)
    @staticmethod
    def calcular_subtotal(items: list[tuple[Libro, int]]) -> float:
        return sum(libro.precio * cant for libro, cant in items)

    def total(self) -> float:
        subtotal = Pedido.calcular_subtotal(self.items)
        return subtotal * (1 + Pedido.IVA)

```

---

### 📙 Decorador `@property`

Encapsula acceso como si fuera atributo, con validaciones perezosas.

```python
class Usuario:
    def __init__(self, email: str):
        self.email = email
        self._puntos = 0

    @property
    def puntos(self) -> int:
        return self._puntos

    @puntos.setter
    def puntos(self, valor: int):
        if valor < 0:
            raise ValueError("Los puntos no pueden ser negativos")
        self._puntos = valor

    @property
    def es_vip(self) -> bool:
        # propiedad solo-lectura
        return self._puntos >= 1000

u = Usuario("ana@example.com")
u.puntos = 1200
print(u.es_vip)  # True
```

---

### 📙 Herencia

Reutiliza y especializa comportamiento.

```python
class ProductoDigital(Libro):
    def __init__(self, titulo, autor, precio, isbn, tamanio_mb: float):
        super().__init__(titulo, autor, precio, isbn)
        self.tamanio_mb = tamanio_mb

    def info(self) -> str:  # sobreescritura (override)
        base = super().info()
        return base + f" [eBook {self.tamanio_mb:.1f} MB]"

```

Herencia múltiple (con mixins, usarla con moderación):

```python
class DescargableMixin:
    def url_descarga(self) -> str:
        return f"https://tienda/descargas/{self.isbn}"

class eLibro(ProductoDigital, DescargableMixin):
    pass

```

---

### 📙 Polimorfismo

Objetos de distintas clases comparten interfaz (métodos comunes) y pueden usarse indistintamente.

```python
class MedioPago:
    def cobrar(self, monto: float) -> str:
        raise NotImplementedError

class Tarjeta(MedioPago):
    def cobrar(self, monto: float) -> str:
        return f"Cobro con tarjeta aprobado por ${monto:.2f}"

class PayPal(MedioPago):
    def cobrar(self, monto: float) -> str:
        return f"Pago PayPal completado por ${monto:.2f}"

def procesar_pago(medio: MedioPago, monto: float) -> str:
    # polimórfico: no importa la subclase concreta
    return medio.cobrar(monto)

print(procesar_pago(Tarjeta(), 150))
print(procesar_pago(PayPal(), 150))

```

---

### 📗 Abstracción

Oculta detalles y expone solo lo esencial (interfaz). Ejemplo: un **Repositorio** de libros que esconde si guarda en memoria, archivo, o BD.

```python
from abc import ABC, abstractmethod

class RepoLibros(ABC):
    @abstractmethod
    def obtener(self, isbn: str) -> Libro | None: ...
    @abstractmethod
    def guardar(self, libro: Libro) -> None: ...

class RepoMemoria(RepoLibros):
    def __init__(self): self._datos = {}
    def obtener(self, isbn): return self._datos.get(isbn)
    def guardar(self, libro): self._datos[libro.isbn] = libro

```

---

### 📗 Encapsulamiento (con `property`)

```python
class Carrito:
    def __init__(self):
        self._items: list[tuple[Libro, int]] = []

    def agregar(self, libro: Libro, cantidad: int = 1):
        self._items.append((libro, cantidad))

    @property
    def items(self) -> tuple[tuple[Libro, int], ...]:
        # Entrego una vista inmutable para evitar que me rompan el estado
        return tuple(self._items)

    @property
    def total(self) -> float:
        return sum(l.precio*c for l, c in self._items)

```

---

### 📙 Métodos dunder o mágicos (protocolo de Python)

Permiten integrar tu clase a los “protocolos” del lenguaje.

```python
class ColeccionLibros:
    def __init__(self, libros: list[Libro] | None = None):
        self._libros = libros or []

    def agregar(self, libro: Libro): self._libros.append(libro)

    # Representaciones
    def __repr__(self):  # para depurar (debe ser no-ambigua)
        return f"ColeccionLibros({self._libros!r})"
    def __str__(self):   # para usuarios
        return f"Colección con {len(self)} libros"

    # Contenedor
    def __len__(self): return len(self._libros)
    def __iter__(self): return iter(self._libros)  # soporta for ... in
    def __contains__(self, libro: Libro): return libro in self._libros
    def __getitem__(self, idx): return self._libros[idx]  # indexado/slicing

    # Comparaciones (define orden por título)
    def __lt__(self, other: "ColeccionLibros"):  # opcional
        return len(self) < len(other)

```

### 📕 Sobreescribiendo métodos mágicos (ordenación, igualdad, hashing, suma, context manager)

```python
from functools import total_ordering

@total_ordering
class SKU:
    """Identificador de inventario: comparable, hasheable y usable como clave en dict/set."""
    def __init__(self, codigo: str):
        self.codigo = codigo

    def __eq__(self, other): 
        return isinstance(other, SKU) and self.codigo == other.codigo

    def __hash__(self): 
        return hash(self.codigo)

    def __lt__(self, other):
        if not isinstance(other, SKU): return NotImplemented
        return self.codigo < other.codigo

class Dinero:
    """Suma/resta con validación de moneda."""
    def __init__(self, monto: float, moneda: str = "USD"):
        self.monto = float(monto)
        self.moneda = moneda

    def __add__(self, other):
        if not isinstance(other, Dinero) or other.moneda != self.moneda:
            raise TypeError("Moneda incompatible")
        return Dinero(self.monto + other.monto, self.moneda)

    def __repr__(self): return f"Dinero({self.monto:.2f}, '{self.moneda}')"

class ArchivoRegistro:
    """Context manager con __enter__/__exit__."""
    def __init__(self, ruta: str): self.ruta = ruta
    def __enter__(self):
        self._f = open(self.ruta, "a", encoding="utf-8")
        return self._f
    def __exit__(self, tipo, valor, tb):
        self._f.close()
        # si hubo excepción, no la suprimimos (False/None)
        return False

# Uso:
s1, s2 = SKU("A-10"), SKU("A-2")
print(sorted({s1, s2}))                      # usa __lt__/__hash__
print(Dinero(10) + Dinero(5))                # Dinero(15.00, 'USD')
with ArchivoRegistro("ventas.log") as f:     # __enter__/__exit__
    f.write("venta 001 OK\n")

```

---

### 📕 Interfaces y Abstract Base Class (ABC)

En Python, el “duck typing” permite interfaces implícitas, pero `abc` te da contratos explícitos.

```python
from abc import ABC, abstractmethod

class Notificador(ABC):
    @abstractmethod
    def enviar(self, destino: str, mensaje: str) -> None: ...

class NotificadorEmail(Notificador):
    def enviar(self, destino: str, mensaje: str) -> None:
        print(f"Email a {destino}: {mensaje}")

class NotificadorSMS(Notificador):
    def enviar(self, destino: str, mensaje: str) -> None:
        print(f"SMS a {destino}: {mensaje}")

def avisar(notificador: Notificador, usuario: Usuario, texto: str):
    notificador.enviar(usuario.email, texto)

avisar(NotificadorEmail(), Usuario("ana@ex.com"), "Tu pedido fue enviado")

```

---
### 📗 Abstracción (ejemplo aplicado con _casos de uso_)

Separar **lógica de aplicación** de **infraestructura**:

```python
class ServicioCheckout:
    """Caso de uso: confirmación de compra."""
    def __init__(self, repo: RepoLibros, notificador: Notificador, medio_pago: MedioPago):
        self.repo = repo
        self.notificador = notificador
        self.medio_pago = medio_pago

    def ejecutar(self, isbn: str, email: str) -> str:
        libro = self.repo.obtener(isbn)
        if not libro:
            raise LookupError("Libro no encontrado")
        mensaje = self.medio_pago.cobrar(libro.precio)
        self.notificador.enviar(email, f"Compra de '{libro.titulo}' confirmada")
        return mensaje

```

---

### 📗 Acoplamiento (bajo vs alto) y cómo reducirlo

- **Alto acoplamiento**: clases dependen de concreciones específicas.
    
- **Bajo acoplamiento**: dependen de **abstracciones** (interfaces/protocolos) e **inyectas** dependencias.
    

**MAL (alto acoplamiento):**
```python
class ServicioMalo:
    def ejecutar(self, isbn: str):
        repo = RepoMemoria()                 # crea concreción adentro
        noti = NotificadorEmail()            # fija un canal
        libro = repo.obtener(isbn)
        noti.enviar("admin@ex.com", libro.titulo)

```

**BIEN (bajo acoplamiento, inversión de dependencias):**
```python
class ServicioBueno:
    def __init__(self, repo: RepoLibros, notificador: Notificador):
        self.repo = repo
        self.notificador = notificador

    def ejecutar(self, isbn: str):
        libro = self.repo.obtener(isbn)
        if libro:
            self.notificador.enviar("admin@ex.com", libro.titulo)

```

---

### 📗 Cohesión

Grado en que los métodos de una clase **persiguen un único propósito**.

**Baja cohesión (hace de todo):**
```python
class Utilidades:
    def leer_archivo(self): ...
    def parsear_json(self): ...
    def enviar_email(self): ...
    def calcular_impuestos(self): ...

```

**Alta cohesión (clases con responsabilidad clara):**
```python
class CalculadoraImpuestos:
    def calcular(self, monto: float, tasa: float) -> float:
        return monto * tasa

class EnviadorEmail:
    def enviar(self, a: str, asunto: str, cuerpo: str): ...

```

Beneficios: código más testable, mantenible y reutilizable.

--- 

### Ejemplo completo de integración

```python
from abc import ABC, abstractmethod
from functools import total_ordering
from typing import Iterable

# --- Dominio: Libros
class Libro:
    def __init__(self, titulo: str, autor: str, precio: float, isbn: str):
        self.titulo = titulo
        self.autor = autor
        self._precio = float(precio)
        self.isbn = isbn

    @property
    def precio(self) -> float: return self._precio

    @precio.setter
    def precio(self, valor: float):
        if valor < 0: raise ValueError("Precio inválido")
        self._precio = float(valor)

    def info(self) -> str:
        return f"{self.titulo} — {self.autor} (${self.precio:.2f})"

class ProductoDigital(Libro):
    def __init__(self, titulo, autor, precio, isbn, tamanio_mb: float):
        super().__init__(titulo, autor, precio, isbn)
        self.tamanio_mb = tamanio_mb

    def info(self) -> str:
        return super().info() + f" [eBook {self.tamanio_mb:.1f} MB]"

# --- Colección con protocolos especiales
class ColeccionLibros:
    def __init__(self, libros: Iterable[Libro] = ()):
        self._libros = list(libros)

    def agregar(self, libro: Libro): self._libros.append(libro)
    def __len__(self): return len(self._libros)
    def __iter__(self): return iter(self._libros)
    def __getitem__(self, i): return self._libros[i]
    def __repr__(self): return f"ColeccionLibros({[l.isbn for l in self._libros]!r})"

# --- Identificador comparable/hasheable
@total_ordering
class SKU:
    def __init__(self, codigo: str): self.codigo = codigo
    def __repr__(self): return f"SKU('{self.codigo}')"
    def __eq__(self, o): return isinstance(o, SKU) and self.codigo == o.codigo
    def __lt__(self, o): 
        if not isinstance(o, SKU): return NotImplemented
        return self.codigo < o.codigo
    def __hash__(self): return hash(self.codigo)

# --- Repositorios (Abstracción)
class RepoLibros(ABC):
    @abstractmethod
    def obtener(self, isbn: str) -> Libro | None: ...
    @abstractmethod
    def guardar(self, libro: Libro) -> None: ...

class RepoMemoria(RepoLibros):
    def __init__(self): self._datos: dict[str, Libro] = {}
    def obtener(self, isbn): return self._datos.get(isbn)
    def guardar(self, libro): self._datos[libro.isbn] = libro

# --- Notificación (ABCs + polimorfismo)
class Notificador(ABC):
    @abstractmethod
    def enviar(self, destino: str, mensaje: str) -> None: ...

class NotificadorEmail(Notificador):
    def enviar(self, destino: str, mensaje: str) -> None:
        print(f"[EMAIL] a {destino}: {mensaje}")

class NotificadorNull(Notificador):  # útil en tests
    def enviar(self, destino: str, mensaje: str) -> None:
        pass

# --- Pagos (polimorfismo)
class MedioPago(ABC):
    @abstractmethod
    def cobrar(self, monto: float) -> str: ...

class Tarjeta(MedioPago):
    def cobrar(self, monto: float) -> str:
        return f"Cobro con tarjeta: ${monto:.2f}"

class PayPal(MedioPago):
    def cobrar(self, monto: float) -> str:
        return f"Cobro con PayPal: ${monto:.2f}"

# --- Caso de uso (bajo acoplamiento, alta cohesión)
class ServicioCheckout:
    def __init__(self, repo: RepoLibros, notificador: Notificador, medio_pago: MedioPago):
        self.repo = repo
        self.notificador = notificador
        self.medio_pago = medio_pago

    def ejecutar(self, isbn: str, email: str) -> str:
        libro = self.repo.obtener(isbn)
        if not libro: raise LookupError("Libro no encontrado")
        resultado_pago = self.medio_pago.cobrar(libro.precio)
        self.notificador.enviar(email, f"Compra confirmada de '{libro.titulo}'")
        return resultado_pago

# --- Ejecución de ejemplo
if __name__ == "__main__":
    repo = RepoMemoria()
    clean = Libro("Clean Code", "Robert C. Martin", 55.0, "9780132350884")
    ebook = ProductoDigital("Fluent Python", "Luciano Ramalho", 40.0, "9781492056355", 12.8)
    repo.guardar(clean); repo.guardar(ebook)

    col = ColeccionLibros([clean, ebook])
    print(col)                 # __repr__
    for l in col: print(l.info())

    skus = {SKU("A-10"), SKU("A-02"), SKU("B-01")}
    print(sorted(skus))        # usa orden total

    servicio = ServicioCheckout(repo, NotificadorEmail(), Tarjeta())
    print(servicio.ejecutar("9780132350884", "ana@ex.com"))

```


### Consejos de diseño rápidos relacionados con el PEP 8

- Implementa `__repr__` para depurar, y `__str__` solo si necesitas una representación “bonita”.
- Si vas a ordenar o comparar, usa `functools.total_ordering` y define al menos `__eq__` y un método de orden (`__lt__`).
- Expón propiedades con `@property` cuando necesites validación sin cambiar el interfaz.
- Prefiere **composición** sobre herencia si solo quieres “usar” otra clase sin especializar su comportamiento.
- Inyecta dependencias (recíbelas por el constructor) para pruebas más simples y menor acoplamiento.
