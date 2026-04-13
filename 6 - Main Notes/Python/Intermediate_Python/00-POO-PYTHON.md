
2026-04-12 18:25

status: #Adult 

tags: [[python]] [[clean-code]] [[dry]] [[poo]] 


# Programación Orientada a Objetos (POO) - Guía

## Tabla de Contenido

| Módulo              | Descripción                                   | Archivo               |
| ------------------- | --------------------------------------------- | --------------------- |
| 1. Clean Code & DRY | Principios de código limpio y sin repetición  | [[POO-CleanCode-DRY]] |
| 2. Classes          | Clases, métodos, constructores, variables     | [[POO-Classes]]       |
| 3. Encapsulation    | Control de acceso y protección de datos       | [[POO-Encapsulation]] |
| 4. Abstraction      | Ocultar complejidad, definir contratos        | [[POO-Abstraction]]   |
| 5. Inheritance      | Herencia simple, múltiple, multinivel         | [[POO-Inheritance]]   |
| 6. Polymorphism     | Polimorfismo, duck typing, métodos especiales | [[POO-Polymorphism]]  |

---

## Los 4 Pilares de la POO

```
┌────────────────────────────────────────────────────────────────┐
│                    LOS 4 PILARES DE LA POO                     │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│    ABSTRACCIÓN                 ENCAPSULACIÓN                   │
│    ═════════════               ════════════════                │
│    "QUIÉN SOY"                "QUÉ HAY DENTRO"                 │
│    Ocultar detalles            Proteger datos                  │
│    internos                    internos                        │
│                                                                │
│         ┌──────────────────┴──────────────────┐                │
│         │                                     │                │
│         ▼                                     ▼                │
│                                                                │
│    HERENCIA                     POLIMORFISMO                   │
│    ═════════                   ══════════════                  │
│    "CÓMO ME RELACIONO"         "CÓMO ACTÚO"                    │
│    Relación padre-hijo          Una forma,                     │
│    Compartir código             muchos comportamientos         │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## Diagrama de Dependencias

```
POO-CleanCode-DRY
        │
        ├──── Fundamentos para todos
        │
POO-Classes ───────────────────────────────────┐
        │                                      │
        ├──── Base de todo                     │
        │                                      │
        ├──── POO-Encapsulation                │
        │        │                             │
        │        └──── Clases (continuación)   │
        │                                      │
        ├──── POO-Abstraction                  │
        │        │                             │
        │        ├──── Requiere: Classes       │
        │        └──── Clases abstractas       │
        │                                      │
        ├──── POO-Inheritance                  │
        │        │                             │
        │        ├──── Requiere: Classes       │
        │        └──── Herencia                │
        │                                      │
        └──── POO-Polymorphism ────────────────┘
                 │
                 ├──── Requiere: Classes
                 ├──── Requiere: Inheritance  
                 └──── Requiere: Abstraction
```

---

## Resumen Rápido de Conceptos

### Clases
```python
class MiClase:
    # Variable de clase
    atributo = "valor"
    
    def __init__(self, param):
        # Variable de instancia
        self.atributo = param
    
    def metodo(self):        # Instancia
        pass
    
    @classmethod
    def metodo_clase(cls):  # Clase
        pass
    
    @staticmethod
    def metodo_static():     # Estático
        pass
```

### Encapsulación
```python
class Ejemplo:
    def __init__(self):
        self.publico = "libre"        # Público
        self._protegido = "cuidado"   # Protected
        self.__privado = "secreto"    # Private
    
    @property
    def algo(self):
        return self.__privado
    
    @algo.setter
    def algo(self, valor):
        self.__privado = valor
```

### Abstracción
```python
from abc import ABC, abstractmethod

class MiAbstracta(ABC):
    @abstractmethod
    def metodo_abstracto(self):
        pass
    
    def metodo_concreto(self):
        pass  # Ya implementado
```

### Herencia
```python
class Padre:
    pass

class Hijo(Padre):           # Simple
    pass

class Nieto(Hijo):           # Multinivel
    pass

class Multiple(Padre1, Padre2):  # Múltiple
    pass

# super() para llamar al padre
class Ejemplo(Padre):
    def __init__(self, a, b):
        super().__init__()
```

### Polimorfismo
```python
# Override
class Hija(Padre):
    def metodo(self):  # Sobrescribe
        return "nueva implementación"

# Duck typing
def procesar(obj):
    return obj.metodo_necesario()  # No importa la clase

# Métodos especiales
class MiClase:
    def __str__(self): pass
    def __add__(self, other): pass
    def __len__(self): pass
```

---

## Clean Code & DRY

### Clean Code
- [+] Nombres significativos
- [+] Funciones pequeñas con propósito único
- [+] Comentarios útiles (explican "por qué", no "qué")
- [+] Evitar números mágicos
- [+] Formato consistente

### DRY
- [+] No repetir conocimiento
- [+] Extraer lógica común
- [!] DRY no es excusa para abstracción prematura
- [!] A veces WET es aceptable por claridad

---

## Principios SOLID (Referencia)

| Letra | Principio             | Archivo relacionado      |
| ----- | --------------------- | ------------------------ |
| S     | Single Responsibility | [[01-POO-CleanCode-DRY]] |
| O     | Open/Closed           | [[06-POO-Polymorphism]]  |
| L     | Liskov Substitution   | [[06-POO-Polymorphism]]  |
| I     | Interface Segregation | [[04-POO-Abstraction]]   |
| D     | Dependency Inversion  | [[04-POO-Abstraction]]   |

---

## Ejercicios Integrados

Cada archivo contiene ejercicios prácticos con:
- Ejemplos reales y funcionales
- Código completo y ejecutable
- Explicaciones paso a paso
- Casos de uso comunes

---

## Convenciones de Nombres

| Elemento | Convención | Ejemplo |
|----------|------------|---------|
| Clases | PascalCase | `MiClase`, `CalculadoraImpuestos` |
| Funciones | snake_case | `calcular_total`, `obtener_datos` |
| Variables | snake_case | `total_ventas`, `lista_usuarios` |
| Constantes | UPPER_SNAKE | `MAXIMO_INTENTOS`, `TASA_IVA` |
| Métodos | snake_case | `calcular_total()`, `obtener_saldo()` |
| Atributos privados | _atributo / __atributo | `_cache`, `__saldo` |




