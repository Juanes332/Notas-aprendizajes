
2026-04-12 14:57

status: #Adult 

tags: [[python]] [[Coding]] 


# Colecciones y estructuras de datos en python

## Listas

## 1. Introducción

Las listas son la estructura de datos más versátil y utilizada en Python. Son:
- **Ordenadas**: Los elementos mantienen un orden definido
- **Mutables**: Se pueden modificar después de creadas
- **Dinámicas**: Pueden crecer o reducirse según sea necesario
- **Indexadas**: Cada elemento tiene una posición numérica

```python
# Mi primera lista
frutas = ["manzana", "pera", "uva"]
print(frutas)  # ['manzana', 'pera', 'uva']
print(type(frutas))  # <class 'list'>
```

---

## 2. Creación de Listas

### 2.1 Creación básica

```python
# Lista vacía
vacia = []
vacia_constructor = list()

# Con elementos
numeros = [1, 2, 3, 4, 5]
nombres = ["Ana", "Luis", "María"]

# Usando el constructor list()
from_string = list("hola")      # ['h', 'o', 'l', 'a']
from_range = list(range(1, 6))  # [1, 2, 3, 4, 5]

# Lista con elementos repetidos
zeros = [0] * 5                 # [0, 0, 0, 0, 0]
palos = ["♠"] * 4               # ['♠', '♠', '♠', '♠']
```

### 2.2 Creación con list comprehensions

```python
cuadrados = [x**2 for x in range(1, 6)]  # [1, 4, 9, 16, 25]
pares = [x for x in range(0, 11, 2)]     # [0, 2, 4, 6, 8, 10]
```

---

## 3. Almacenamiento Heterogéneo

Las listas pueden contener elementos de diferentes tipos:

```python
mixto = [42, "hola", 3.14, True, None, [1, 2]]
print(mixto)  # [42, 'hola', 3.14, True, None, [1, 2]]

# Cada elemento puede ser de un tipo diferente
empleado = [
    "Juan Pérez",      # str
    35,                # int
    45000.50,          # float
    True,              # bool
    ["Python", "SQL"], # list
    {"departamento": "IT"}  # dict
]
```

### 3.1 Verificar tipos de elementos

```python
datos = [1, "texto", 3.14, True, []]

for elemento in datos:
    print(f"{elemento!r} -> {type(elemento).__name__}")

# Salida:
# 1 -> int
# 'texto' -> str
# 3.14 -> float
# True -> bool
# [] -> list
```

---

## 4. Indexación y Corte

### 4.1 Indexación

```python
animales = ["gato", "perro", "pájaro", "pez", "conejo"]

# Índices positivos (de izquierda a derecha)
print(animales[0])   # 'gato'
print(animales[2])   # 'pájaro'
print(animales[4])   # 'conejo'

# Índices negativos (de derecha a izquierda)
print(animales[-1])  # 'conejo' (último)
print(animales[-2])  # 'pez'
print(animales[-5])  # 'gato' (primero)

# Indexación inválida genera error
# print(animales[10])   # IndexError
# print(animales[-10])  # IndexError
```

### 4.2 Slicing (Corte)

```python
numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# sintaxis: lista[inicio:fin:paso]
print(numeros[2:7])     # [2, 3, 4, 5, 6] - del índice 2 al 6
print(numeros[:5])      # [0, 1, 2, 3, 4] - desde el inicio
print(numeros[5:])      # [5, 6, 7, 8, 9] - hasta el final
print(numeros[::2])     # [0, 2, 4, 6, 8] - pasos de 2
print(numeros[::-1])    # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0] - inverso
print(numeros[5:2:-1])  # [5, 4, 3] - corte inverso
```

### 4.3 Asignación por índice

```python
colores = ["rojo", "verde", "azul"]
colores[1] = "amarillo"
print(colores)  # ['rojo', 'amarillo', 'azul']

colores[-1] = "morado"
print(colores)  # ['rojo', 'amarillo', 'morado']
```

---

## 5. Listas Anidadas

### 5.1 Matrices (listas de listas)

```python
# Matriz 3x3
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Acceder a elementos
print(matriz[0])       # [1, 2, 3]
print(matriz[1][1])    # 5 (fila 1, columna 1)
print(matriz[2][0])    # 7

# Modificar elementos
matriz[1][1] = 10
print(matriz)  # [[1, 2, 3], [4, 10, 6], [7, 8, 9]]
```

### 5.2 Recorrer matrices

```python
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Por filas
for fila in matriz:
    print(fila)

# Elemento por elemento
for fila in matriz:
    for elemento in fila:
        print(elemento, end=" ")
    print()

# Con enumerate
for i, fila in enumerate(matriz):
    for j, elemento in enumerate(fila):
        print(f"matriz[{i}][{j}] = {elemento}")
```

### 5.3 Ejemplo: Sudoku simplificado

```python
sudoku = [
    [5, 3, 0, 0, 7, 0, 0, 0, 0],
    [6, 0, 0, 1, 9, 5, 0, 0, 0],
    [0, 9, 8, 0, 0, 0, 0, 6, 0],
    [8, 0, 0, 0, 6, 0, 0, 0, 3],
    [4, 0, 0, 8, 0, 3, 0, 0, 1],
    [7, 0, 0, 0, 2, 0, 0, 0, 6],
    [0, 6, 0, 0, 0, 0, 2, 8, 0],
    [0, 0, 0, 4, 1, 9, 0, 0, 5],
    [0, 0, 0, 0, 8, 0, 0, 7, 9]
]

# Obtener submatriz 3x3
def obtener_submatriz(matriz, fila_inicio, col_inicio):
    submatriz = []
    for i in range(fila_inicio, fila_inicio + 3):
        submatriz.append(matriz[i][col_inicio:col_inicio + 3])
    return submatriz

bloque = obtener_submatriz(sudoku, 0, 0)
print(bloque)  # [[5, 3, 0], [6, 0, 0], [0, 9, 8]]
```

---

## 6. Operaciones Fundamentales

### 6.1 Concatenación

```python
a = [1, 2, 3]
b = [4, 5, 6]

# Usando +
c = a + b
print(c)  # [1, 2, 3, 4, 5, 6]

# Usando extend()
a.extend(b)
print(a)  # [1, 2, 3, 4, 5, 6]

# Usando +=
a = [1, 2, 3]
a += [4, 5, 6]
print(a)  # [1, 2, 3, 4, 5, 6]
```

### 6.2 Repetición

```python
base = [1, 2]
repetida = base * 3
print(repetida)  # [1, 2, 1, 2, 1, 2]

# Patrón útil: inicializar matriz
matriz_2x3 = [[0] * 3 for _ in range(2)]
print(matriz_2x3)  # [[0, 0, 0], [0, 0, 0]]
```

### 6.3 Longitud

```python
numeros = [1, 2, 3, 4, 5]
print(len(numeros))       # 5
print(len("hola"))        # 4 (funciona con cualquier secuencia)
print(len([[1,2], [3,4]])) # 2
```

### 6.4 Membresía (in / not in)

```python
frutas = ["manzana", "pera", "uva"]

print("pera" in frutas)      # True
print("kiwi" in frutas)      # False
print("kiwi" not in frutas)  # True
```

### 6.5 Comparación

```python
[1, 2, 3] == [1, 2, 3]   # True
[1, 2, 3] == [1, 2, 4]   # False
[1, 2, 3] < [1, 2, 4]    # True (comparación lexicográfica)
[1, 2, 3] < [1, 2, 3, 0] # True (menor longitud es menor si es prefijo)
```

### 6.6 Conversión a lista

```python
# Desde string
texto = "python"
lista_texto = list(texto)
print(lista_texto)  # ['p', 'y', 't', 'h', 'o', 'n']

# Desde tupla
tupla = (1, 2, 3)
lista_tupla = list(tupla)
print(lista_tupla)  # [1, 2, 3]

# Desde diccionario (solo claves)
dicc = {'a': 1, 'b': 2}
lista_claves = list(dicc)
print(lista_claves)  # ['a', 'b']

# Desde set
conjunto = {1, 2, 3}
lista_conj = list(conjunto)
print(lista_conj)  # [1, 2, 3] (orden no garantizado)
```

---

## 7. Métodos de Listas

### 7.1 Añadir Elementos

#### append() - Añade al final

```python
frutas = ["manzana", "pera"]
frutas.append("uva")
print(frutas)  # ['manzana', 'pera', 'uva']

# Añadir cualquier elemento
numeros = [1, 2]
numeros.append([3, 4])
print(numeros)  # [1, 2, [3, 4]] - ¡Añade la lista como un solo elemento!
```

#### extend() - Extiende con múltiples elementos

```python
frutas = ["manzana", "pera"]
frutas.extend(["uva", "kiwi", "香蕉"])
print(frutas)  # ['manzana', 'pera', 'uva', 'kiwi', '香蕉']

# Funciona con cualquier iterable
numeros = [1, 2]
numeros.extend(range(3, 6))
print(numeros)  # [1, 2, 3, 4, 5]

numeros.extend("abc")
print(numeros)  # [1, 2, 3, 4, 5, 'a', 'b', 'c']
```

#### insert() - Inserta en posición específica

```python
frutas = ["manzana", "uva"]
frutas.insert(1, "pera")  # Insertar en índice 1
print(frutas)  # ['manzana', 'pera', 'uva']

frutas.insert(0, "kiwi")  # Insertar al inicio
print(frutas)  # ['kiwi', 'manzana', 'pera', 'uva']

frutas.insert(100, "coco")  # Índice mayor que longitud = al final
print(frutas)  # ['kiwi', 'manzana', 'pera', 'uva', 'coco']

frutas.insert(-1, "mango")  # Inserta antes del último
print(frutas)  # ['kiwi', 'manzana', 'pera', 'uva', 'mango', 'coco']
```

### 7.2 Eliminar Elementos

#### remove() - Elimina por valor

```python
frutas = ["manzana", "pera", "uva", "pera"]
frutas.remove("pera")  # Elimina la primera ocurrencia
print(frutas)  # ['manzana', 'uva', 'pera']

# Genera error si no existe
# frutas.remove("kiwi")  # ValueError: 'kiwi' not in list
```

#### pop() - Elimina por índice (y lo devuelve)

```python
frutas = ["manzana", "pera", "uva"]

elemento = frutas.pop()  # Por defecto elimina el último
print(elemento)  # 'uva'
print(frutas)    # ['manzana', 'pera']

elemento = frutas.pop(0)  # Eliminar primero
print(elemento)  # 'manzana'
print(frutas)    # ['pera']

# pop() en lista vacía genera error
# frutas.pop()  # IndexError: pop from empty list
```

#### del - Elimina por índice o slice

```python
frutas = ["manzana", "pera", "uva", "kiwi"]

del frutas[1]  # Eliminar por índice
print(frutas)  # ['manzana', 'uva', 'kiwi']

del frutas[0:2]  # Eliminar slice
print(frutas)  # ['kiwi']

# Eliminar toda la lista
# del frutas
# print(frutas)  # NameError: name 'frutas' is not defined
```

#### clear() - Vacía la lista

```python
frutas = ["manzana", "pera", "uva"]
frutas.clear()
print(frutas)  # []
print(len(frutas))  # 0
```

### 7.3 Ordenamiento

#### sort() - Ordena in-place (modifica la lista)

```python
numeros = [3, 1, 4, 1, 5, 9, 2, 6]
numeros.sort()
print(numeros)  # [1, 1, 2, 3, 4, 5, 6, 9]

# Orden inverso
numeros.sort(reverse=True)
print(numeros)  # [9, 6, 5, 4, 3, 2, 1, 1]

# Con clave personalizada
palabras = ["banana", "apple", "cherry", "date"]
palabras.sort(key=len)  # Por longitud
print(palabras)  # ['apple', 'date', 'banana', 'cherry']

palabras.sort(key=lambda x: x[-1])  # Por última letra
print(palabras)  # ['banana', 'apple', 'cherry', 'date']
```

#### sorted() - Retorna una nueva lista ordenada

```python
numeros = [3, 1, 4, 1, 5]
ordenada = sorted(numeros)
print(ordenada)   # [1, 1, 3, 4, 5]
print(numeros)    # [3, 1, 4, 1, 5] - Original sin cambios

# Con reverse
ordenada_desc = sorted(numeros, reverse=True)
print(ordenada_desc)  # [5, 4, 3, 1, 1]
```

### 7.4 Inversión

#### reverse() - Invierte in-place

```python
numeros = [1, 2, 3, 4, 5]
numeros.reverse()
print(numeros)  # [5, 4, 3, 2, 1]
```

#### slicing [::-1] - Crea copia invertida

```python
numeros = [1, 2, 3, 4, 5]
invertida = numeros[::-1]
print(invertida)  # [5, 4, 3, 2, 1]
print(numeros)    # [1, 2, 3, 4, 5] - Original sin cambios
```

### 7.5 Búsqueda

#### index() - Retorna el índice de un elemento

```python
frutas = ["manzana", "pera", "uva", "pera"]

print(frutas.index("pera"))     # 1 (primera ocurrencia)
print(frutas.index("pera", 2))  # 3 (busca desde índice 2)
print(frutas.index("manzana"))  # 0

# Genera error si no existe
# frutas.index("kiwi")  # ValueError: 'kiwi' is not in list
```

#### count() - Cuenta ocurrencias

```python
numeros = [1, 2, 2, 3, 2, 4, 2, 5]

print(numeros.count(2))   # 4
print(numeros.count(1))   # 1
print(numeros.count(0))   # 0
```

### 7.6 Copiado

#### copy() - Copia superficial

```python
original = [1, 2, [3, 4]]
copia = original.copy()

copia[0] = 10  # No afecta al original
print(original)  # [1, 2, [3, 4]]
print(copia)     # [10, 2, [3, 4]]

copia[2][0] = 30  # ¡SÍ afecta al original! (copia superficial)
print(original)  # [1, 2, [30, 4]]
print(copia)     # [10, 2, [30, 4]]
```

#### deepcopy() - Copia profunda

```python
import copy

original = [1, 2, [3, 4]]
copia = copy.deepcopy(original)

copia[2][0] = 30
print(original)  # [1, 2, [3, 4]] - No se afecta
print(copia)     # [1, 2, [30, 4]]
```

### 7.7 Otros Métodos

#### copy vs list() vs [:]

```python
original = [1, 2, 3]

copia1 = original.copy()
copia2 = list(original)
copia3 = original[:]

print(copia1 == copia2 == copia3 == original)  # True
```

---

## 8. Comprensión de Listas (List Comprehensions)

### 8.1 Sintaxis básica

```python
# Forma tradicional
cuadrados = []
for x in range(10):
    cuadrados.append(x ** 2)
print(cuadrados)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Con comprensión de listas
cuadrados = [x ** 2 for x in range(10)]
print(cuadrados)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### 8.2 Con condición (filtrado)

```python
# Solo pares
pares = [x for x in range(20) if x % 2 == 0]
print(pares)  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Cuadrados de números pares
cuadrados_pares = [x**2 for x in range(10) if x % 2 == 0]
print(cuadrados_pares)  # [0, 4, 16, 36, 64]

# Filtrar strings por longitud
palabras = ["sol", "luna", "estrella", "cielo", "mar"]
largas = [p for p in palabras if len(p) > 4]
print(largas)  # ['estrella', 'cielo']
```

### 8.3 Con transformación y filtrado

```python
# Transformar y filtrar
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

resultado = [x * 2 for x in numeros if x > 5]
print(resultado)  # [12, 14, 16, 18, 20]

# Múltiples condiciones
resultado = [x for x in range(20) if x % 2 == 0 if x % 3 == 0]
print(resultado)  # [0, 6, 12, 18]
```

### 8.4 Con if-else (transformación condicional)

```python
numeros = [1, 2, 3, 4, 5]

# Clasificar números
clasificacion = ["par" if x % 2 == 0 else "impar" for x in numeros]
print(clasificacion)  # ['impar', 'par', 'impar', 'par', 'impar']

# Multiplicar por 2 si es par, por 3 si es impar
transformado = [x * 2 if x % 2 == 0 else x * 3 for x in numeros]
print(transformado)  # [3, 4, 9, 8, 15]
```

### 8.5 Listas anidadas

```python
# Matriz 3x3 de productos
matriz = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print(matriz)
# [[1, 2, 3],
#  [2, 4, 6],
#  [3, 6, 9]]

# Aplanar matriz
plana = [elem for fila in matriz for elem in fila]
print(plana)  # [1, 2, 3, 2, 4, 6, 3, 6, 9]
```

### 8.6 Ejemplos prácticos

```python
# Extraer extensiones de archivos
archivos = ["documento.txt", "imagen.png", "datos.csv", "script.py"]
extensiones = [arch.split(".")[-1] for arch in archivos]
print(extensiones)  # ['txt', 'png', 'csv', 'py']

# Limpiar y normalizar texto
textos = ["  hola  ", "ADIOS", "  PyThOn  "]
limpios = [t.strip().lower() for t in textos]
print(limpios)  # ['hola', 'adios', 'python']

# Crear diccionario desde listas
claves = ["nombre", "edad", "ciudad"]
valores = ["Ana", 25, "Madrid"]
diccionario = {k: v for k, v in zip(claves, valores)}
print(diccionario)  # {'nombre': 'Ana', 'edad': 25, 'ciudad': 'Madrid'}

# Generar coordenadas
coordenadas = [(x, y) for x in range(3) for y in range(3)]
print(coordenadas)  # [(0,0), (0,1), (0,2), (1,0), (1,1), (1,2), (2,0), (2,1), (2,2)]
```

---

## 9. Manipulación Avanzada

### 9.1 Desempaquetado (Unpacking)

```python
# Desempaquetado básico
a, b, c = [1, 2, 3]
print(a, b, c)  # 1 2 3

# Desempaquetado con asterisco
primero, *medio, ultimo = [1, 2, 3, 4, 5]
print(primero)  # 1
print(medio)    # [2, 3, 4]
print(ultimo)   # 5

primero, *resto = [1, 2, 3, 4, 5]
print(resto)  # [2, 3, 4, 5]

*primeros, ultimo = [1, 2, 3, 4, 5]
print(primeros)  # [1, 2, 3, 4]

# Desempaquetado anidado
x, y, [z, w] = [1, 2, [3, 4]]
print(x, y, z, w)  # 1 2 3 4
```

### 9.2 enumerate() - Con índice y valor

```python
frutas = ["manzana", "pera", "uva"]

# enumerate() retorna tuplas (índice, valor)
for indice, fruta in enumerate(frutas):
    print(f"{indice}: {fruta}")

# Empezar desde índice diferente
for indice, fruta in enumerate(frutas, start=1):
    print(f"{indice}: {fruta}")

# Convertir a diccionario
diccionario = dict(enumerate(frutas))
print(diccionario)  # {0: 'manzana', 1: 'pera', 2: 'uva'}
```

### 9.3 zip() - Combinar listas

```python
nombres = ["Ana", "Luis", "María"]
edades = [25, 30, 35]

# Combinar listas
combinado = list(zip(nombres, edades))
print(combinado)  # [('Ana', 25), ('Luis', 30), ('María', 35)]

# Iterar paralelamente
for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")

# Desempaquetar
nuevos_nombres, nuevas_edades = zip(*combinado)
print(nuevos_nombres)  # ('Ana', 'Luis', 'María')
print(nuevas_edades)   # (25, 30, 35)
```

### 9.4 map() - Aplicar función a cada elemento

```python
numeros = [1, 2, 3, 4, 5]

# Sin map
duplicados = []
for n in numeros:
    duplicados.append(n * 2)
print(duplicados)  # [2, 4, 6, 8, 10]

# Con map
duplicados = list(map(lambda x: x * 2, numeros))
print(duplicados)  # [2, 4, 6, 8, 10]

# Con comprensión (más Pythonico)
duplicados = [x * 2 for x in numeros]
print(duplicados)  # [2, 4, 6, 8, 10]
```

### 9.5 filter() - Filtrar elementos

```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Sin filter
pares = []
for n in numeros:
    if n % 2 == 0:
        pares.append(n)
print(pares)  # [2, 4, 6, 8, 10]

# Con filter
pares = list(filter(lambda x: x % 2 == 0, numeros))
print(pares)  # [2, 4, 6, 8, 10]

# Con comprensión (más Pythonico)
pares = [x for x in numeros if x % 2 == 0]
print(pares)  # [2, 4, 6, 8, 10]
```

### 9.6 reduce() - Reducir a un solo valor

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5]

# Sumar todos
suma = reduce(lambda a, b: a + b, numeros)
print(suma)  # 15

# Con valor inicial
suma = reduce(lambda a, b: a + b, numeros, 10)
print(suma)  # 25

# Producto
producto = reduce(lambda a, b: a * b, numeros)
print(producto)  # 120

# Máximo
maximo = reduce(lambda a, b: a if a > b else b, numeros)
print(maximo)  # 5
```

### 9.7 all() y any()

```python
numeros = [2, 4, 6, 8, 10]

# all() - ¿Todos son verdaderos/condición?
print(all(x % 2 == 0 for x in numeros))  # True
print(all(x > 0 for x in numeros))       # True

# any() - ¿Alguno es verdadero/condición?
print(any(x > 5 for x in numeros))  # True
print(any(x > 10 for x in numeros)) # False
```

### 9.8 split() y join()

```python
# split() - String a lista
frase = "Hola mundo Python"
palabras = frase.split()
print(palabras)  # ['Hola', 'mundo', 'Python']

csv = "ana,25,madrid"
campos = csv.split(",")
print(campos)  # ['ana', '25', 'madrid']

# splitlines() - Por líneas
texto = "línea1\nlínea2\nlínea3"
lineas = texto.splitlines()
print(lineas)  # ['línea1', 'línea2', 'línea3']

# join() - Lista a string
palabras = ["Hola", "mundo"]
frase = " ".join(palabras)
print(frase)  # Hola mundo

frase = "-".join(palabras)
print(frase)  # Hola-mundo

numeros = ["1", "2", "3"]
print("".join(numeros))  # 123
```

---

## 10. Buenas Prácticas

### 10.1 Evitar mutaciones durante iteración

```python
# [x] MAL - Modificar lista mientras se itera
numeros = [1, 2, 3, 4, 5]
for n in numeros:
    if n % 2 == 0:
        numeros.remove(n)
print(numeros)  # [1, 3, 5] - Puede dar resultados inesperados

# [+] BIEN - Iterar sobre copia
numeros = [1, 2, 3, 4, 5]
for n in numeros[:]:  # Copia de la lista
    if n % 2 == 0:
        numeros.remove(n)
print(numeros)  # [1, 3, 5]

# [+] BIEN - Usar list comprehension
numeros = [1, 2, 3, 4, 5]
numeros = [n for n in numeros if n % 2 != 0]
print(numeros)  # [1, 3, 5]
```

### 10.2 Preferencia por comprensión de listas

```python
# [x] MENOS Pythonico
cuadrados = []
for x in range(10):
    cuadrados.append(x ** 2)

# [+] MÁS Pythonico
cuadrados = [x ** 2 for x in range(10)]
```

### 10.3 Evitar listas excesivamente profundas

```python
# [x] Cuidado con el anidamiento excesivo
profunda = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]

# [+] Mejor usar clases o namedtuple para datos complejos
from collections import namedtuple

Punto = namedtuple('Punto', ['x', 'y'])
puntos = [Punto(1, 2), Punto(3, 4)]
```

### 10.4 Usar generadores para listas grandes

```python
# [x] Crear lista enorme en memoria
# suma = sum([x**2 for x in range(100000000)])

# [+] Usar generador (lazy evaluation)
suma = sum(x**2 for x in range(100000000))
```

### 10.5 Documentar el propósito de la lista

```python
# [x] Poco claro
datos = [1, 2, 3, 4, 5]

# [+] Claro
# Temperaturas en Celsius de los últimos 5 días
temperaturas_celsius = [22.5, 23.1, 21.8, 24.0, 22.9]
```

---

## 11. Listas vs Otras Colecciones

### 11.1 Comparación de características

| Característica | Lista | Tupla | Set | Dict     |
| -------------- | ----- | ----- | --- | -------- |
| Ordenado       | ✅     | ✅     | ❌   | ✅ (3.7+) |
| Indexable      | ✅     | ✅     | ❌   | ✅ (keys) |
| Mutable        | ✅     | ❌     | ✅   | ✅        |
| Duplicados     | ✅     | ✅     | ❌   | ❌ (keys) |
| Heterogéneo    | ✅     | ✅     | ✅   | ✅        |

### 11.2 Cuándo usar cada una

```python
# LISTA - Secuencias ordenadas, mutables
tareas = ["comprar", "cocinar", "limpiar"]
tareas.append("ordenar")

# TUPLA - Datos inmutables, más eficiente en memoria
coordenadas = (40.7128, -74.0060)  # Latitud, longitud
colores_rgb = (255, 128, 0)

# SET - Colecciones únicas, operaciones de conjuntos
etiquetas = {"python", "javascript", "rust"}
etiquetas.add("python")  # No hace nada, ya existe

# DICT - Pares clave-valor
usuario = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}
```

### 11.3 Conversiones entre tipos

```python
# Lista a Tupla
lista = [1, 2, 3]
tupla = tuple(lista)

# Tupla a Lista  
tupla = (1, 2, 3)
lista = list(tupla)

# Lista a Set (elimina duplicados)
lista = [1, 2, 2, 3, 3, 3]
unico = set(lista)
print(unico)  # {1, 2, 3}

# Set a Lista
conjunto = {1, 2, 3}
lista = list(conjunto)

# Lista de tuplas a Dict
parejas = [("a", 1), ("b", 2)]
diccionario = dict(parejas)
print(diccionario)  # {'a': 1, 'b': 2}
```

### 11.4 Operaciones de conjuntos con listas

```python
# Convertir a set para operaciones de conjuntos
l1 = [1, 2, 3, 4]
l2 = [3, 4, 5, 6]

# Intersección
interseccion = list(set(l1) & set(l2))
print(interseccion)  # [3, 4]

# Unión
union = list(set(l1) | set(l2))
print(union)  # [1, 2, 3, 4, 5, 6]

# Diferencia
diferencia = list(set(l1) - set(l2))
print(diferencia)  # [1, 2]

# Diferencia simétrica
simetrica = list(set(l1) ^ set(l2))
print(simetrica)  # [1, 2, 5, 6]
```

---

## 12. Ejercicios Prácticos

### 12.1 Ejercicio: Gestor de tareas

```python
class GestorTareas:
    def __init__(self):
        self.tareas = []
    
    def agregar(self, tarea, prioridad=0):
        self.tareas.append({"tarea": tarea, "completada": False, "prioridad": prioridad})
    
    def completar(self, indice):
        self.tareas[indice]["completada"] = True
    
    def pendientes(self):
        return [t for t in self.tareas if not t["completada"]]
    
    def ordenar_por_prioridad(self):
        self.tareas.sort(key=lambda x: x["prioridad"], reverse=True)
    
    def __str__(self):
        resultado = []
        for i, t in enumerate(self.tareas):
            estado = "✓" if t["completada"] else "○"
            resultado.append(f"{i+1}. [{estado}] {t['tarea']} (Prioridad: {t['prioridad']})")
        return "\n".join(resultado)

# Uso
gestor = GestorTareas()
gestor.agregar("Comprar groceries", prioridad=2)
gestor.agregar("Llamar al médico", prioridad=3)
gestor.agregar("Responder emails", prioridad=1)
gestor.agregar("Pasear al perro", prioridad=2)

gestor.completar(0)
gestor.ordenar_por_prioridad()
print(gestor)
```

### 12.2 Ejercicio: Analizador de texto

```python
def analizar_texto(texto):
    palabras = texto.lower().split()
    
    # Frecuencia de palabras
    frecuencia = {}
    for palabra in palabras:
        frecuencia[palabra] = frecuencia.get(palabra, 0) + 1
    
    # Palabras más comunes
    ordenadas = sorted(frecuencia.items(), key=lambda x: x[1], reverse=True)
    
    return {
        "total_palabras": len(palabras),
        "palabras_unicas": len(frecuencia),
        "mas_comunes": ordenadas[:5],
        "frecuencia": frecuencia
    }

texto = "Python es un lenguaje de programación Python moderno y versátil. Python es fácil de aprender."
resultado = analizar_texto(texto)
print(f"Palabras totales: {resultado['total_palabras']}")
print(f"Palabras únicas: {resultado['palabras_unicas']}")
print(f"Más comunes: {resultado['mas_comunes']}")
```

### 12.3 Ejercicio: Matriz y operaciones

```python
def crear_matriz(filas, columnas, valor=0):
    return [[valor] * columnas for _ in range(filas)]

def transponer(matriz):
    return [list(fila) for fila in zip(*matriz)]

def sumar_matrices(a, b):
    return [[a[i][j] + b[i][j] for j in range(len(a[0]))] 
            for i in range(len(a))]

def imprimir_matriz(matriz):
    for fila in matriz:
        print(" ".join(f"{x:4}" for x in fila))

# Uso
A = [[1, 2], [3, 4]]
B = [[5, 6], [7, 8]]

print("Matriz A:")
imprimir_matriz(A)
print("\nTranspuesta de A:")
imprimir_matriz(transponer(A))
print("\nA + B:")
imprimir_matriz(sumar_matrices(A, B))
```

### 12.4 Ejercicio: Filtro y transformación pipeline

```python
datos = [
    {"nombre": "Ana", "edad": 25, "ciudad": "Madrid", "salario": 30000},
    {"nombre": "Luis", "edad": 35, "ciudad": "Barcelona", "salario": 45000},
    {"nombre": "María", "edad": 28, "ciudad": "Madrid", "salario": 35000},
    {"nombre": "Pedro", "edad": 42, "ciudad": "Valencia", "salario": 55000},
    {"nombre": "Laura", "edad": 31, "ciudad": "Barcelona", "salario": 40000},
]

# Filtrar por ciudad
def filtrar(lista, **criterios):
    return [
        item for item in lista
        if all(item.get(k) == v for k, v in criterios.items())
    ]

# Ordenar por campo
def ordenar(lista, campo, reverse=False):
    return sorted(lista, key=lambda x: x.get(campo), reverse=reverse)

# Seleccionar campos
def proyectar(lista, *campos):
    return [{k: v for k, v in item.items() if k in campos} for item in lista]

# Pipeline
resultado = (
    datos
    | (lambda d: filtrar(d, ciudad="Madrid"))
    | (lambda d: ordenar(d, "salario", reverse=True))
    | (lambda d: proyectar(d, "nombre", "salario"))
)

for r in resultado:
    print(r)
# {'nombre': 'María', 'salario': 35000}
```

---

## Resumen Rápido

```python
# CREAR
lista = [1, 2, 3]
lista = list("abc")           # ['a', 'b', 'c']
lista = list(range(5))        # [0, 1, 2, 3, 4]
lista = [x**2 for x in range(5)]  # [0, 1, 4, 9, 16]

# AÑADIR
lista.append(4)               # Al final
lista.insert(0, 99)           # En índice
lista.extend([5, 6])          # Concatenar

# ELIMINAR
lista.remove(3)              # Por valor
lista.pop()                  # Por índice (devuelve)
del lista[0]                 # Por índice
lista.clear()                # Vaciar

# ORDENAR
lista.sort()                 # In-place
sorted(lista)               # Nueva lista
lista.reverse()              # In-place
lista[::-1]                  # Nueva lista invertida

# BUSCAR
lista.index(3)              # Índice
lista.count(3)               # Ocurrencias
3 in lista                   # Membresía

# ITERAR
for i, v in enumerate(lista): pass
for a, b in zip(lista1, lista2): pass
```

---

## Tuplas

## 1. Introducción

Las tuplas son estructuras de datos fundamentales en Python que representan secuencias **ordenadas e inmutables** de elementos. Mientras que las listas son como cajas mágicas que pueden crecer y cambiar, las tuplas son como cajas selladas: una vez creadas, no puedes modificar su contenido.

### Diferencia clave con listas

```python
# Lista - Mutable
lista = [1, 2, 3]
lista[0] = 10
print(lista)  # [10, 2, 3] - Funciona

# Tupla - Inmutable
tupla = (1, 2, 3)
tupla[0] = 10  # TypeError: 'tuple' object does not support item assignment
```

### ¿Por qué usar tuplas?

- [+] **Rendimiento**: Las tuplas son más rápidas que las listas
- [+] **Seguridad**: Protegen datos que no deben modificarse
- [+] **Memoria**: Ocupan menos espacio que las listas
- [+] **Hashables**: Pueden usarse como claves de diccionarios
- [+] **Retorno de funciones**: Permiten retornar múltiples valores

---

## 2. Creación de Tuplas

### 2.1 Creación básica

```python
# Tupla vacía
tupla_vacia = ()
tupla_vacia = tuple()

print(tupla_vacia)  # ()
print(type(tupla_vacia))  # <class 'tuple'>

# Tupla con elementos
coordenadas = (40.7128, -74.0060)
colores_rgb = (255, 128, 0)
print(coordenadas)  # (40.7128, -74.0060)
```

### 2.2 El caso del paréntesis único

```python
# Con coma, siempre es tupla
tupla_uno = (42,)
print(tupla_uno)      # (42,)
print(type(tupla_uno))  # <class 'tuple'>

# Sin coma, es solo el número
no_tupla = (42)
print(no_tupla)        # 42
print(type(no_tupla))  # <class 'int'>

# La coma es la que define la tupla
tupla_multiple = 1, 2, 3
print(tupla_multiple)  # (1, 2, 3)
```

### 2.3 Usando el constructor tuple()

```python
# Desde una lista
lista = [1, 2, 3]
tupla = tuple(lista)
print(tupla)  # (1, 2, 3)

# Desde un string
tupla = tuple("abc")
print(tupla)  # ('a', 'b', 'c')

# Desde un rango
tupla = tuple(range(5))
print(tupla)  # (0, 1, 2, 3, 4)

# Desde otro iterable
tupla = tuple({1, 2, 3})  # Sets no mantienen orden
print(tupla)  # (1, 2, 3) o en otro orden
```

### 2.4 Tuplas con diferentes tipos

```python
# Heterogénea
mixto = (42, "hola", 3.14, True, None)
print(mixto)  # (42, 'hola', 3.14, True, None)

# Tupla de tuplas (anidada)
matriz = ((1, 2), (3, 4), (5, 6))
print(matriz)  # ((1, 2), (3, 4), (5, 6))
print(matriz[1][0])  # 3
```

---

## 3. Características Fundamentales

### 3.1 Inmutabilidad

[!] La principal característica de las tuplas es su inmutabilidad. Una vez creada, no puedes:

- Agregar elementos
- Eliminar elementos
- Modificar elementos existentes
- Cambiar el orden de los elementos

```python
tupla = (1, 2, 3)

# No puedes hacer esto:
# tupla.append(4)      # AttributeError
# tupla[0] = 10        # TypeError
# tupla.remove(2)      # AttributeError
# del tupla[0]         # TypeError
```

[+] Sin embargo, si un elemento es mutable (como una lista), puedes modificar ese elemento interno:

```python
tupla = (1, [2, 3], 4)
print(tupla)  # (1, [2, 3], 4)

# El elemento mutable interno SÍ se puede modificar
tupla[1].append(5)
print(tupla)  # (1, [2, 3, 5], 4) - La tupla cambió internamente
```

### 3.2 Orden y duplicados

```python
# Mantienen el orden
tupla = (3, 1, 4, 1, 5, 9, 2, 6)
print(tupla)  # (3, 1, 4, 1, 5, 9, 2, 6) - El orden se mantiene

# Permiten duplicados
tupla = (1, 1, 1, 2, 2, 3)
print(tupla)  # (1, 1, 1, 2, 2, 3)
```

### 3.3 Hashabilidad

[+] Las tuplas son hashables (si todos sus elementos lo son), lo que permite usarlas como claves de diccionarios o elementos de sets.

```python
# Como clave de diccionario
ubicaciones = {
    ("Madrid", "España"): "Capital",
    ("Paris", "Francia"): "Capital",
    ("Roma", "Italia"): "Capital"
}
print(ubicaciones[("Madrid", "España")])  # Capital

# Como elemento de set
conjunto = {(1, 2), (3, 4), (5, 6)}
print(conjunto)  # {(1, 2), (3, 4), (5, 6)}

# [!] Tuplas con elementos mutables no son hashables
# tupla = (1, [2, 3])  # TypeError: unhashable type: 'list'
```

---

## 4. Indexación y Corte

### 4.1 Indexación

```python
tupla = ("a", "b", "c", "d", "e")

# Índices positivos
print(tupla[0])   # 'a'
print(tupla[2])   # 'c'
print(tupla[4])   # 'e'

# Índices negativos
print(tupla[-1])  # 'e' (último)
print(tupla[-3])  # 'c'
print(tupla[-5])  # 'a' (primero)

# IndexError si está fuera de rango
# print(tupla[10])  # IndexError: tuple index out of range
```

### 4.2 Slicing (Corte)

```python
numeros = (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)

# sintaxis: tupla[inicio:fin:paso]
print(numeros[2:7])     # (2, 3, 4, 5, 6)
print(numeros[:5])      # (0, 1, 2, 3, 4)
print(numeros[5:])      # (5, 6, 7, 8, 9)
print(numeros[::2])     # (0, 2, 4, 6, 8)
print(numeros[::-1])    # (9, 8, 7, 6, 5, 4, 3, 2, 1, 0)
print(numeros[7:2:-1])  # (7, 6, 5, 4, 3)
```

### 4.3 Operaciones de corte

[!] El corte siempre retorna una **nueva** tupla:

```python
original = (1, 2, 3, 4, 5)
porcion = original[1:4]
print(porcion)  # (2, 3, 4)
print(original) # (1, 2, 3, 4, 5) - Original intacta
```

---

## 5. Desempaquetado

### 5.1 Desempaquetado básico

```python
# Desempaquetado automático
coordenadas = (40.7128, -74.0060)
latitud, longitud = coordenadas
print(latitud)   # 40.7128
print(longitud)  # -74.0060

# Con más variables que elementos
# a, b, c = (1, 2)  # ValueError: not enough values to unpack

# Con menos variables
# a, b = (1, 2, 3)  # ValueError: too many values to unpack
```

### 5.2 Desempaquetado con asterisco (*)

```python
numeros = (1, 2, 3, 4, 5)

# Capturar primero y último
primero, *medio, ultimo = numeros
print(primero)  # 1
print(medio)    # [2, 3, 4] - Lista
print(ultimo)   # 5

# Capturar inicio
primero, segundo, *resto = numeros
print(primero)   # 1
print(segundo)   # 2
print(resto)     # [3, 4, 5]

# Capturar final
*inicio, penultimo, ultimo = numeros
print(inicio)      # [1, 2, 3]
print(penultimo)   # 4
print(ultimo)      # 5
```

### 5.3 Desempaquetado anidado

```python
# Tupla de tuplas
punto = ((10, 20), (30, 40))
(x1, y1), (x2, y2) = punto
print(x1, y1)  # 10 20
print(x2, y2)  # 30 40

# Mezclado
dato = (1, (2, 3), 4)
a, (b, c), d = dato
print(a, b, c, d)  # 1 2 3 4
```

### 5.4 Desempaquetado extendido (Python 3+)

```python
# Múltiples variables
a, b, c = (1, 2, 3)

# Intercambio de valores
a, b = b, a
print(a, b)  # 2 1

# Desempaquetado con _ (ignorar)
punto = (10, 20, 30)
x, _, z = punto
print(x, z)  # 10 30

# Desempaquetado de string
primer, *resto = "hola"
print(primer)  # 'h'
print(resto)   # ['o', 'l', 'a']
```

### 5.5 Uso práctico del desempaquetado

```python
# Retornar múltiples valores desde función
def dividir(dividendo, divisor):
    cociente = dividendo // divisor
    resto = dividendo % divisor
    return cociente, resto

cociente, resto = dividir(17, 5)
print(f"Cociente: {cociente}, Resto: {resto}")  # Cociente: 3, Resto: 2

# Iterar con enumerate
tupla = ("a", "b", "c")
for i, valor in enumerate(tupla):
    print(f"{i}: {valor}")

# Iterar con zip
nombres = ("Ana", "Luis", "Maria")
edades = (25, 30, 35)
for nombre, edad in zip(nombres, edades):
    print(f"{nombre}: {edad}")
```

---

## 6. Inmutabilidad y sus Consecuencias

### 6.1 ¿Por qué la inmutabilidad?

[+] **Ventajas de usar tuplas**:

```python
# 1. Seguridad de datos
# Los datos no pueden ser modificados accidentalmente
CONFIG = ("DEBUG", "INFO", "WARNING", "ERROR")
# CONFIG[0] = "TRACE"  # TypeError - Protegido

# 2. Como claves de diccionario
cache = {}
cache[(1, 2)] = "resultado_1"
cache[(3, 4)] = "resultado_2"

# 3. Integridad en estructuras de datos
# Sets de tuplas para relaciones únicas
relaciones = {("Ana", "Luis"), ("Maria", "Pedro")}
```

### 6.2 Simular "mutación" de tuplas

[x] No puedes modificar una tupla, pero puedes crear una nueva:

```python
original = (1, 2, 3)
print(f"Original: {original}")

# Simular append (crear nueva tupla)
nueva = original + (4,)
print(f"Con append: {nueva}")  # (1, 2, 3, 4)

# Simular insert
def tupla_insert(tupla, indice, valor):
    return tupla[:indice] + (valor,) + tupla[indice:]

resultado = tupla_insert((1, 2, 3), 1, 10)
print(f"Con insert: {resultado}")  # (1, 10, 2, 3)

# Simular remove
def tupla_remove(tupla, valor):
    return tuple(x for x in tupla if x != valor)

resultado = tupla_remove((1, 2, 3, 2), 2)
print(f"Con remove: {resultado}")  # (1, 3)

# Simular cambio de elemento
def tupla_replace(tupla, indice, valor):
    return tupla[:indice] + (valor,) + tupla[indice+1:]

resultado = tupla_replace((1, 2, 3), 1, 99)
print(f"Con replace: {resultado}")  # (1, 99, 3)
```

### 6.3 Tuplas vs Literales

```python
# [] = lista literal, () = tupla literal
# Pero hay excepciones:

# Esto es una tupla vacía
vacia = ()
print(type(vacia))  # <class 'tuple'>

# Esto NO es tupla (es un entero)
no_tupla = (42)
print(type(no_tupla))  # <class 'int'>

# Esto SÍ es tupla
si_tupla = (42,)
print(type(si_tupla))  # <class 'tuple'>

# En retorno de función, la coma es importante
def retorna_tupla():
    return 1, 2  # Retorna tupla, no requiere paréntesis
```

---

## 7. Métodos de Tuplas

### 7.1 Métodos disponibles

[!] Las tuplas tienen solo dos métodos propios debido a su inmutabilidad:

```python
tupla = (1, 2, 2, 3, 2, 4, 2)

# count() - Cuenta ocurrencias
print(tupla.count(2))   # 4
print(tupla.count(5))   # 0

# index() - Retorna el índice de la primera ocurrencia
print(tupla.index(2))     # 1
print(tupla.index(2, 3))  # 4 (busca desde índice 3)
print(tupla.index(2, 0, 3))  # 1 (busca en rango 0-2)
# print(tupla.index(5))  # ValueError: tuple.index(x): x not in tuple
```

### 7.2 Métodos heredados

Las tuplas heredan métodos de tuple que también funcionan con iterables:

```python
tupla = (3, 1, 4, 1, 5, 9, 2, 6)

# len()
print(len(tupla))  # 8

# in / not in
print(4 in tupla)       # True
print(7 not in tupla)   # True

# max() y min()
print(max(tupla))  # 9
print(min(tupla))  # 1

# sum()
print(sum(tupla))  # 31

# sorted() - Retorna lista
print(sorted(tupla))  # [1, 1, 2, 3, 4, 5, 6, 9]
print(tupla)          # (3, 1, 4, 1, 5, 9, 2, 6) - Sin cambios
```

---

## 8. Operaciones con Tuplas

### 8.1 Concatenación

```python
tupla1 = (1, 2, 3)
tupla2 = (4, 5, 6)

# Concatenar con +
resultado = tupla1 + tupla2
print(resultado)  # (1, 2, 3, 4, 5, 6)

# Concatenar con += (crea nueva tupla)
tupla1 += tupla2
print(tupla1)  # (1, 2, 3, 4, 5, 6)
```

### 8.2 Repetición

```python
tupla = (1, 2, 3)

# Repetir con *
resultado = tupla * 3
print(resultado)  # (1, 2, 3, 1, 2, 3, 1, 2, 3)

# Útil para inicializar
zeros = (0,) * 10
print(zeros)  # (0, 0, 0, 0, 0, 0, 0, 0, 0, 0)
```

### 8.3 Comparación

```python
# Comparación lexicográfica
print((1, 2, 3) == (1, 2, 3))   # True
print((1, 2, 3) == (1, 2, 4))   # False
print((1, 2, 3) < (1, 2, 4))    # True
print((1, 2, 3) < (1, 2, 3, 0))  # True

# Compara elemento por elemento
print((1, 2, 10) > (1, 2, 5))  # True
print((1, 5, 3) > (1, 2, 10))   # True (5 > 2)
```

### 8.4 Membresía

```python
tupla = (1, 2, 3, 4, 5)

print(3 in tupla)       # True
print(10 not in tupla)   # True
print("3" in tupla)     # False (tipo diferente)
```

---

## 9. Named Tuples

### 9.1 Creación básica

```python
from collections import namedtuple

# Definir un named tuple
Punto = namedtuple('Punto', ['x', 'y'])

# Crear instancia
p = Punto(10, 20)
print(p)            # Punto(x=10, y=20)
print(p.x, p.y)    # 10 20
print(p[0], p[1])  # 10 20 (también indexable)
```

### 9.2 Con tipos y valores por defecto

```python
from collections import namedtuple
from typing import NamedTuple

# Definición moderna (Python 3.6+)
class Producto(NamedTuple):
    nombre: str
    precio: float
    cantidad: int = 0

# Crear productos
p1 = Producto("Manzana", 1.50)
p2 = Producto("Naranja", 2.00, 10)

print(p1)  # Producto(nombre='Manzana', precio=1.50, cantidad=0)
print(p2)  # Producto(nombre='Naranja', precio=2.00, cantidad=10)
```

### 9.3 Métodos de Named Tuples

```python
from collections import namedtuple

Persona = namedtuple('Persona', ['nombre', 'edad', 'ciudad'])

# Crear desde diccionario
datos = {'nombre': 'Ana', 'edad': 25, 'ciudad': 'Madrid'}
p1 = Persona(**datos)
print(p1)  # Persona(nombre='Ana', edad=25, ciudad='Madrid')

# _make() - Crear desde iterable
datos = ("Luis", 30, "Barcelona")
p2 = Persona._make(datos)
print(p2)  # Persona(nombre='Luis', edad=30, ciudad='Barcelona')

# _asdict() - Convertir a diccionario
print(p2._asdict())  # {'nombre': 'Luis', 'edad': 30, 'ciudad': 'Barcelona'}

# _replace() - Crear copia con cambios
p3 = p2._replace(edad=31)
print(p3)  # Persona(nombre='Luis', edad=31, ciudad='Barcelona')
print(p2)  # Persona(nombre='Luis', edad=30, ciudad='Barcelona') - Original intacta
```

### 9.4 Campos calculados con _fields

```python
from collections import namedtuple

# Ver campos
Color = namedtuple('Color', ['rojo', 'verde', 'azul'])
print(Color._fields)  # ('rojo', 'verde', 'azul')

# Crear desde strings
rojo_str = Color._make(['255', '0', '0'])
print(rojo_str)  # Color(rojo='255', verde='0', azul='0')

# Herencia con campos adicionales
ColorAlpha = Color._make(['255', '0', '0', '128'])
print(ColorAlpha)  # ('255', '0', '0', '128')
```

### 9.5 Ejemplo: Registro de estudiantes

```python
from collections import namedtuple

Estudiante = namedtuple('Estudiante', ['nombre', 'apellido', 'carrera', 'nota'])

estudiantes = [
    Estudiante('Maria', 'Garcia', 'Ingenieria', 8.5),
    Estudiante('Carlos', 'Rodriguez', 'Medicina', 9.2),
    Estudiante('Ana', 'Martinez', 'Derecho', 7.8),
    Estudiante('Luis', 'Lopez', 'Ingenieria', 8.1),
]

# Promedio de notas
promedio = sum(e.nota for e in estudiantes) / len(estudiantes)
print(f"Promedio general: {promedio:.2f}")

# Mejores estudiantes
print("Mejores estudiantes:")
for e in sorted(estudiantes, key=lambda x: x.nota, reverse=True)[:3]:
    print(f"  {e.nombre} {e.apellido}: {e.nota}")

# Agrupar por carrera
from collections import defaultdict
por_carrera = defaultdict(list)
for e in estudiantes:
    por_carrera[e.carrera].append(e)

print("\nPor carrera:")
for carrera, grupo in por_carrera.items():
    print(f"  {carrera}: {len(grupo)} estudiantes")
```

---

## 10. Uso Práctico

### 10.1 Retornar múltiples valores

```python
def min_max(numeros):
    return min(numeros), max(numeros)

resultado = min_max([3, 1, 4, 1, 5, 9, 2, 6])
print(resultado)  # (1, 9)

minimo, maximo = min_max([3, 1, 4, 1, 5])
print(f"Min: {minimo}, Max: {maximo}")
```

### 10.2 Intercambio de valores

```python
# Sin variable temporal
a, b = 10, 20
a, b = b, a
print(a, b)  # 20 10
```

### 10.3 Claves de diccionarios

```python
# Cache con claves complejas
cache = {}

clave1 = ("usuario", "sesion_123")
clave2 = ("usuario", "sesion_456")

cache[clave1] = {"nombre": "Ana", "rol": "admin"}
cache[clave2] = {"nombre": "Luis", "rol": "user"}

print(cache[clave1])  # {'nombre': 'Ana', 'rol': 'admin'}
```

### 10.4 Datos de configuración

```python
# Configuración inmutable
CONFIGuracion = (
    ("DEBUG", False),
    ("MAX_CONNECTIONS", 100),
    ("TIMEOUT", 30),
    ("ALLOWED_HOSTS", ["localhost", "127.0.0.1"]),
)

for clave, valor in CONFIGuracion:
    print(f"{clave}: {valor}")
```

### 10.5 Empaquetar valores relacionados

```python
# En lugar de listas separadas
#BAD: nombres = ["Ana", "Luis"]
#BAD: edades = [25, 30]

#GOOD: Usar tuplas
empleados = [
    ("Ana", 25, "Madrid", 30000),
    ("Luis", 30, "Barcelona", 40000),
    ("Maria", 28, "Valencia", 35000),
]

for nombre, edad, ciudad, salario in empleados:
    print(f"{nombre}, {edad} años, {ciudad}, {salario}€")
```

### 10.6 Pattern matching (Python 3.10+)

```python
def procesar_respuesta(respuesta):
    match respuesta:
        case (200, mensaje):
            return f"OK: {mensaje}"
        case (404, url):
            return f"No encontrado: {url}"
        case (500, _, error_id):
            return f"Error interno: {error_id}"
        case _:
            return "Respuesta desconocida"

print(procesar_respuesta((200, "Exito")))      # OK: Exito
print(procesar_respuesta((404, "/pagina")))   # No encontrado: /pagina
print(procesar_respuesta((500, "msg", "ERR")))  # Error interno: ERR
```

---

## 11. Tuplas vs Listas

### 11.1 Tabla comparativa

| Característica | Tupla | Lista |
|----------------|-------|-------|
| Sintaxis | `(1, 2, 3)` | `[1, 2, 3]` |
| Mutabilidad | Inmutable | Mutable |
| Métodos | 2 (`count`, `index`) | ~11 (`append`, `insert`, etc.) |
| Rendimiento | Mas rapida | Mas lenta |
| Memoria | Menor | Mayor |
| Como clave dict | SI | NO |
| En set | SI | NO |
| Hashable | SI (si elementos lo son) | NO |
| Seguro contra escritura | SI | NO |

### 11.2 Cuándo usar cada una

```python
# USAR TUPLAS cuando:
# [+] Datos que no deben cambiar (configuracion, constantes)
COLORES = ("rojo", "verde", "azul")
DIAS_SEMANA = ("lunes", "martes", "miercoles", "jueves", "viernes", "sabado", "domingo")

# [+] Claves de diccionario
coordenadas = {(0, 0): "origen", (1, 0): "este"}

# [+] Retornar multiples valores
def dividir(a, b):
    return a // b, a % b

# [+] Mejor rendimiento en iteraciones
# for _ in range(1000000): ...  # Tuplas mas rapidas

# USAR LISTAS cuando:
# [+] Necesitas agregar/eliminar elementos
tareas = []
tareas.append("comprar")
tareas.remove("comprar")

# [+] Necesitas ordenar
numeros = [3, 1, 4, 1, 5]
numeros.sort()

# [+] Necesitas modificar elementos
colores = ["rojo", "verde"]
colores[0] = "azul"

# [+] Colecciones de longitud variable unknown
#BAD: dias_mes = (31, 28, 31, 30, ...)  # Fijos
#GOOD: notas = [7.5, 8.0, 6.5]  # Variables
```

### 11.3 Conversiones

```python
# Tupla a Lista
tupla = (1, 2, 3)
lista = list(tupla)
print(lista)  # [1, 2, 3]

# Lista a Tupla
lista = [1, 2, 3]
tupla = tuple(lista)
print(tupla)  # (1, 2, 3)

# Set a Tupla (elimina orden y duplicados)
conjunto = {1, 2, 2, 3}
tupla = tuple(conjunto)
print(tupla)  # (1, 2, 3)
```

---

## 12. Ejercicios Prácticos

### 12.1 Ejercicio: Sistema de coordenadas

```python
from collections import namedtuple
import math

Punto = namedtuple('Punto', ['x', 'y'])

def distancia(p1, p2):
    dx = p2.x - p1.x
    dy = p2.y - p1.y
    return math.sqrt(dx**2 + dy**2)

def punto_medio(p1, p2):
    return Punto(
        (p1.x + p2.x) / 2,
        (p1.y + p2.y) / 2
    )

# Uso
a = Punto(0, 0)
b = Punto(3, 4)

print(f"Distancia: {distancia(a, b):.2f}")
print(f"Punto medio: {punto_medio(a, b)}")
```

### 12.2 Ejercicio: Registro CSV como tuplas

```python
from collections import namedtuple

Registro = namedtuple('Registro', ['fecha', 'producto', 'cantidad', 'precio'])

# Simular datos CSV
datos_csv = [
    "2024-01-01,Manzana,10,1.50",
    "2024-01-02,Naranja,5,2.00",
    "2024-01-03,Manzana,8,1.50",
]

registros = []
for linea in datos_csv:
    campos = linea.split(",")
    registros.append(Registro(*campos))

# Calcular totales
total_manzana = sum(
    float(r.precio) * int(r.cantidad)
    for r in registros
    if r.producto == "Manzana"
)
print(f"Total manzanas: {total_manzana:.2f}")
```

### 12.3 Ejercicio: Cache con tuplas como claves

```python
def cache_decorator(func):
    cache = {}
    
    def wrapper(*args):
        if args in cache:
            return f"Cache: {cache[args]}"
        resultado = func(*args)
        cache[args] = resultado
        return f"Nuevo: {resultado}"
    
    wrapper.cache = cache
    return wrapper

@cache_decorator
def suma(a, b):
    return a + b

@cache_decorator
def multiplica(a, b):
    return a * b

# Las tuplas permiten usar argumentos como claves
print(suma(2, 3))    # Nuevo: 5
print(suma(2, 3))    # Cache: 5
print(suma(4, 5))    # Nuevo: 9
print(multiplica(2, 3))  # Nuevo: 6
```

### 12.4 Ejercicio: Tabla de multiples valores

```python
from collections import namedtuple

Empleado = namedtuple('Empleado', ['id', 'nombre', 'departamento', 'salario'])

empleados = [
    Empleado(1, 'Ana', 'IT', 45000),
    Empleado(2, 'Luis', 'Ventas', 40000),
    Empleado(3, 'Maria', 'IT', 48000),
    Empleado(4, 'Carlos', 'RRHH', 42000),
]

# Buscar por departamento
def buscar_por_departamento(emps, depto):
    return tuple(e for e in emps if e.departamento == depto)

# Encontrar maximo salario
def mayor_salario(emps):
    return max(emps, key=lambda e: e.salario)

# Agrupar por departamento
from itertools import groupby
def agrupar_por_departamento(emps):
    emps_ordenados = sorted(emps, key=lambda e: e.departamento)
    return {
        depto: list(grupo) 
        for depto, grupo in groupby(emps_ordenados, key=lambda e: e.departamento)
    }

# Uso
print("IT:", buscar_por_departamento(empleados, 'IT'))
print("Mayor salario:", mayor_salario(empleados))
print("Grupos:", agrupar_por_departamento(empleados))
```

---

## Resumen Rápido

```python
# CREAR
tupla = ()                  # Vacia
tupla = (1,)                # Un elemento (con coma)
tupla = (1, 2, 3)          # Multiple elementos
tupla = 1, 2, 3            # Sin parentesis
tupla = tuple([1, 2, 3])    # Desde iterable
tupla = tuple("abc")        # Desde string

# METODOS
tupla.count(3)              # Contar ocurrencias
tupla.index(3)              # Indice de elemento

# OPERACIONES
tupla + tupla               # Concatenar
tupla * n                   # Repetir
tupla[0]                    # Indexar
tupla[1:3]                  # Cortar
len(tupla)                  # Longitud
3 in tupla                  # Membresia
min(tupla), max(tupla)      # Minimo/Maximo

# DESEMPAQUETAR
a, b, c = (1, 2, 3)
primero, *resto = tupla
```

## Consejos Finales

- [+] Usa tuplas para datos **fijos** y listas para datos **variables**
- [+] Usa namedtuples para **legibilidad** y **documentacion automatica**
- [+] Las tuplas son **mas eficientes** en memoria y velocidad
- [!] Recuerda: **la coma define la tupla**, no los paréntesis
- [x] No intentes modificar tuplas: usa **desempaquetado** o crea **nuevas**


---

## Conjuntos

## 1. Introducción

Los conjuntos (sets) son estructuras de datos fundamentales en Python que almacenan colecciones **no ordenadas** de elementos **únicos**. Son la herramienta perfecta cuando necesitas eliminar duplicados, realizar operaciones de conjuntos o verificar membresía de manera eficiente.

### ¿Qué es un conjunto?

Imagina una bolsa donde solo puedes meter elementos únicos. No importa el orden en que los metas ni cuántas veces intentes meter el mismo, solo quedará uno.

```python
# Lista con duplicados
numeros = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
print(f"Lista: {numeros}")  # [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]

# Conjunto (elimina duplicados automáticamente)
conjunto = set(numeros)
print(f"Conjunto: {conjunto}")  # {1, 2, 3, 4}
```

### ¿Cuándo usar conjuntos?

- [+] Eliminar duplicados de una colección
- [+] Verificar si un elemento existe (membresía)
- [+] Realizar operaciones matemáticas de conjuntos (unión, intersección, diferencia)
- [+] Comparar características entre grupos
- [x] NO usar si necesitas mantener orden
- [x] NO usar si necesitas elementos duplicados

---

## 2. Creación de Conjuntos

### 2.1 Creación básica

```python
# Conjunto vacío
conjunto_vacio = set()  # No usar {}, eso crea un dict
print(conjunto_vacio)  # set()

# Verificar tipo
print(type({}))    # <class 'dict'>
print(type(set())) # <class 'set'>

# Conjunto con elementos
colores = {"rojo", "verde", "azul"}
print(colores)  # {'rojo', 'verde', 'azul'}

# Desde una lista
numeros = set([1, 2, 3, 4, 5])
print(numeros)  # {1, 2, 3, 4, 5}
```

### 2.2 Constructor set()

```python
# Desde una lista (elimina duplicados)
lista = [1, 2, 2, 3, 3, 3, 4, 4, 5]
print(set(lista))  # {1, 2, 3, 4, 5}

# Desde un string (caracteres únicos)
texto = "hola mundo"
print(set(texto))  # {'h', 'o', 'l', ' ', 'm', 'u', 'n', 'd', 'a'}

# Desde una tupla
tupla = (1, 2, 3, 2, 1)
print(set(tupla))  # {1, 2, 3}

# Desde un rango
print(set(range(10)))  # {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

# Desde diccionario (solo claves)
dicc = {'a': 1, 'b': 2, 'c': 3}
print(set(dicc))  # {'a', 'b', 'c'}
```

### 2.3 Eliminación de duplicados

```python
# Problema: lista con muchos duplicados
emails = [
    "ana@ejemplo.com",
    "luis@ejemplo.com",
    "ana@ejemplo.com",  # duplicado
    "maria@ejemplo.com",
    "luis@ejemplo.com",  # duplicado
    "pedro@ejemplo.com"
]

# Solución con set
emails_unicos = set(emails)
print(f"Total emails: {len(emails)}")       # 6
print(f"Emails únicos: {len(emails_unicos)}")  # 4
print(emails_unicos)
# {'ana@ejemplo.com', 'luis@ejemplo.com', 'maria@ejemplo.com', 'pedro@ejemplo.com'}
```

### 2.4 Conjuntos con diferentes tipos

[!] Los elementos deben ser hashables (inmutables):

```python
# Elementos válidos
conjunto_valido = {1, "hola", 3.14, True, (1, 2)}
print(conjunto_valido)

# Elementos inválidos (no hashables)
# conjunto_invalido = {1, [2, 3]}  # TypeError: unhashable type: 'list'
# conjunto_invalido = {1, {2, 3}}  # TypeError: unhashable type: 'set'
```

---

## 3. Características Fundamentales

### 3.1 Sin orden

[!] Los conjuntos NO mantienen orden:

```python
numeros = {5, 3, 1, 4, 2}
print(numeros)  # Puede mostrar {1, 2, 3, 4, 5} o cualquier orden

# Si necesitas orden, usa sorted()
print(sorted(numeros))  # [1, 2, 3, 4, 5]

# No puedes acceder por índice
# numeros[0]  # TypeError: 'set' object is not subscriptable
```

### 3.2 Elementos únicos

```python
conjunto = {1, 2, 2, 3, 3, 3, 4}
print(conjunto)  # {1, 2, 3, 4}

# Intentar agregar duplicado no tiene efecto
conjunto.add(2)
print(conjunto)  # {1, 2, 3, 4}
```

### 3.3 No son subscriptables

[x] No puedes acceder a elementos por índice:

```python
colores = {"rojo", "verde", "azul"}

# Esto NO funciona:
# print(colores[0])  # TypeError

# Esto SÍ funciona:
for color in colores:
    print(color)

# Verificar existencia:
print("rojo" in colores)  # True
print("amarillo" in colores)  # False
```

### 3.4 Heterogeneidad

```python
# Pueden contener diferentes tipos (si son hashables)
mixto = {1, "texto", 3.14, True, (1, 2), None}
print(mixto)  # {1, 'texto', 3.14, True, None, (1, 2)}
```

---

## 4. Operaciones de Conjuntos

### 4.1 Unión (|)

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Usando operador |
resultado = a | b
print(resultado)  # {1, 2, 3, 4, 5, 6}

# Usando método
resultado = a.union(b)
print(resultado)  # {1, 2, 3, 4, 5, 6}

# Unión múltiple
c = {5, 6, 7, 8}
resultado = a | b | c
print(resultado)  # {1, 2, 3, 4, 5, 6, 7, 8}
```

### 4.2 Intersección (&)

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Usando operador &
resultado = a & b
print(resultado)  # {3, 4}

# Usando método
resultado = a.intersection(b)
print(resultado)  # {3, 4}

# Intersección múltiple
c = {3, 4, 9}
resultado = a & b & c
print(resultado)  # {3, 4}
```

### 4.3 Diferencia (-)

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Elementos en a pero no en b
resultado = a - b
print(resultado)  # {1, 2}

# Elementos en b pero no en a
resultado = b - a
print(resultado)  # {5, 6}

# Usando método
resultado = a.difference(b)
print(resultado)  # {1, 2}
```

### 4.4 Diferencia simétrica (^)

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Elementos en a o b pero NO en ambos
resultado = a ^ b
print(resultado)  # {1, 2, 5, 6}

# Usando método
resultado = a.symmetric_difference(b)
print(resultado)  # {1, 2, 5, 6}
```

### 4.5 Subconjunto (<=)

```python
a = {1, 2}
b = {1, 2, 3, 4}

# a es subconjunto de b
print(a <= b)      # True
print(a.issubset(b))  # True

# b NO es subconjunto de a
print(b <= a)      # False

# Subconjunto propio (menor que, no igual)
print(a < b)       # True (a tiene menos elementos que b)
print(a < a)       # False (no es propio)
print(a <= a)       # True (es igual a sí mismo)
```

### 4.6 Superconjunto (>=)

```python
a = {1, 2, 3, 4}
b = {1, 2}

# a es superconjunto de b
print(a >= b)        # True
print(a.issuperset(b))  # True

# b NO es superconjunto de a
print(b >= a)        # False

# Superconjunto propio (mayor que)
print(a > b)         # True
print(a > a)         # False
```

### 4.7 Conjuntos disjuntos

```python
a = {1, 2, 3}
b = {4, 5, 6}
c = {3, 4, 5}

# Conjuntos sin elementos en común
print(a.isdisjoint(b))  # True (no comparten elementos)
print(a.isdisjoint(c))  # False (comparten el 3)
```

### 4.8 Diagrama de Venn de operaciones

```
    A           B
  {1,2}     {3,4,5}
    
    UNION (|):     {1,2,3,4,5}
    INTERSECCION (&): {}
    DIFERENCIA (-):  {1,2}
    DIF SIMETRICA (^): {1,2,3,4,5}
```

---

## 5. Métodos de Conjuntos

### 5.1 Añadir elementos

#### add() - Añade un elemento

```python
colores = {"rojo", "verde"}

colores.add("azul")
print(colores)  # {'rojo', 'verde', 'azul'}

# Añadir elemento ya existente (no hace nada)
colores.add("rojo")
print(colores)  # {'rojo', 'verde', 'azul'}

# Añadir tupla (es hashable)
colores.add(("amarillo", "naranja"))
print(colores)  # {'rojo', 'verde', 'azul', ('amarillo', 'naranja')}
```

#### update() - Añade múltiples elementos

```python
colores = {"rojo", "verde"}

# Desde otra colección
colores.update(["azul", "amarillo", "rojo"])
print(colores)  # {'rojo', 'verde', 'azul', 'amarillo'}

# Desde tupla
colores.update(("morado", "gris"))
print(colores)  # {'rojo', 'verde', 'azul', 'amarillo', 'morado', 'gris'}

# Desde string (cada carácter)
colores.update("negro")
print(colores)  # {'r', 'o', 'v', 'e', 'z', ...}
```

### 5.2 Eliminar elementos

#### remove() - Elimina, error si no existe

```python
colores = {"rojo", "verde", "azul"}

colores.remove("verde")
print(colores)  # {'rojo', 'azul'}

# Eliminar elemento que no existe
# colores.remove("amarillo")  # KeyError: 'amarillo'
```

#### discard() - Elimina, NO error si no existe

```python
colores = {"rojo", "verde", "azul"}

colores.discard("verde")
print(colores)  # {'rojo', 'azul'}

# Eliminar elemento que NO existe (no da error)
colores.discard("amarillo")
print(colores)  # {'rojo', 'azul'}
```

[!] Recomendación: Usa `discard()` si no estás seguro de que el elemento exista.

#### pop() - Elimina y retorna un elemento arbitrario

```python
colores = {"rojo", "verde", "azul"}

elemento = colores.pop()
print(f"Eliminado: {elemento}")  # Uno de los tres
print(f"Restante: {colores}")

# Pop en conjunto vacío
# conjunto.pop()  # KeyError: 'pop from an empty set'
```

#### clear() - Elimina todos los elementos

```python
colores = {"rojo", "verde", "azul"}
colores.clear()
print(colores)  # set()
```

### 5.3 Métodos de comparación

```python
a = {1, 2, 3}
b = {1, 2}
c = {4, 5}

# issubset()
print(a.issubset({1, 2, 3, 4}))  # True
print(b.issubset(a))              # True

# issuperset()
print(a.issuperset(b))   # True
print(a.issuperset(c))   # False

# isdisjoint()
print(a.isdisjoint(c))   # True
print(a.isdisjoint(b))   # False
```

---

## 6. Mutabilidad y Frozenset

### 6.1 Conjuntos mutables vs inmutables

```python
# Conjunto mutable (set)
conjunto = {1, 2, 3}
conjunto.add(4)
print(conjunto)  # {1, 2, 3, 4}

# [!] No puede ser clave de diccionario ni elemento de otro conjunto
# conjunto_anidado = {{1, 2}, {3, 4}}  # TypeError
# dicc = {{1, 2}: "valor"}  # TypeError
```

### 6.2 Frozenset - Versión inmutable

[+] Los frozensets son como conjuntos pero **inmutables**:

```python
# Crear frozenset
fs = frozenset([1, 2, 3, 2, 1])
print(fs)  # frozenset({1, 2, 3})

# Pueden ser claves de diccionarios
dicc = {fs: "valor"}
print(dicc)  # {frozenset({1, 2, 3}): 'valor'}

# Pueden ser elementos de otros conjuntos
conjunto_de_conjuntos = {frozenset([1, 2]), frozenset([3, 4])}
print(conjunto_de_conjuntos)
# {frozenset({1, 2}), frozenset({3, 4})}
```

### 6.3 Operaciones en frozenset

```python
a = frozenset([1, 2, 3])
b = frozenset([3, 4, 5])

# Unión
print(a | b)  # frozenset({1, 2, 3, 4, 5})

# Intersección
print(a & b)  # frozenset({3})

# Diferencia
print(a - b)  # frozenset({1, 2})

# Diferencia simétrica
print(a ^ b)  # frozenset({1, 2, 4, 5})

# [!] No puedes modificar un frozenset
# a.add(4)  # AttributeError: 'frozenset' object has no attribute 'add'
```

### 6.4 Cuándo usar frozenset

```python
# Registro de студентов que tomaron cada curso
cursos = {
    "Matematicas": frozenset(["Ana", "Luis", "Maria"]),
    "Fisica": frozenset(["Luis", "Pedro", "Juan"]),
    "Quimica": frozenset(["Ana", "Maria", "Sofia"])
}

# Encontrar estudiantes en Matematicas y Fisica
en_ambos = cursos["Matematicas"] & cursos["Fisica"]
print(en_ambos)  # frozenset({'Luis'})

# Estudiantes únicos por curso (no se pueden modificar accidentalmente)
for curso, estudiantes in cursos.items():
    print(f"{curso}: {len(estudiantes)} estudiantes")
```

---

## 7. Iteración y Comprensión

### 7.1 Iteración básica

```python
colores = {"rojo", "verde", "azul", "amarillo"}

# Iterar sobre elementos
for color in colores:
    print(color)

# Iterar ordenadamente
for color in sorted(colores):
    print(color)  # azul, amarillo, rojo, verde

# Con enumerate
for i, color in enumerate(colores):
    print(f"{i}: {color}")
```

### 7.2 Comprensión de conjuntos

```python
# Números pares del 0 al 9
pares = {x for x in range(10) if x % 2 == 0}
print(pares)  # {0, 2, 4, 6, 8}

# Cuadrados únicos
cuadrados = {x**2 for x in range(-5, 6)}
print(cuadrados)  # {0, 1, 4, 9, 16, 25}

# Caracteres únicos de un string
texto = "abracadabra"
unicos = {c for c in texto}
print(unicos)  # {'a', 'b', 'r', 'c', 'd'}

# Con transformación
nombres = ["Ana", "Luis", "Maria", "Pedro"]
iniciales = {nombre[0] for nombre in nombres}
print(iniciales)  # {'A', 'L', 'M', 'P'}
```

### 7.3 Conversiones con comprensión

```python
# De lista a conjunto (eliminar duplicados)
numeros = [1, 2, 2, 3, 3, 3, 4, 4, 5]
unicos = {x for x in numeros}
print(unicos)  # {1, 2, 3, 4, 5}

# De conjunto a lista
conjunto = {1, 2, 3}
lista = [x for x in conjunto]
print(lista)  # [1, 2, 3]

# Ordenar desde conjunto
ordenado = sorted(conjunto, reverse=True)
print(ordenado)  # [3, 2, 1]
```

### 7.4 Operaciones con múltiples conjuntos

```python
conjuntos = [
    {1, 2, 3, 4},
    {3, 4, 5, 6},
    {5, 6, 7, 8}
]

# Unión de todos
union_total = set()
for c in conjuntos:
    union_total |= c
print(union_total)  # {1, 2, 3, 4, 5, 6, 7, 8}

# Intersección de todos
interseccion_total = conjuntos[0]
for c in conjuntos[1:]:
    interseccion_total &= c
print(interseccion_total)  # set() (ningún elemento en común)
```

---

## 8. Uso Práctico

### 8.1 Eliminar duplicados mientras mantienes orden

```python
# Problema: quieres únicos pero mantener el orden de primera aparición
def eliminar_duplicados_mantener_orden(lista):
    visto = set()
    resultado = []
    for item in lista:
        if item not in visto:
            resultado.append(item)
            visto.add(item)
    return resultado

# Uso
datos = [1, 2, 2, 3, 1, 4, 3, 5, 2, 1]
print(eliminar_duplicados_mantener_orden(datos))  # [1, 2, 3, 4, 5]

nombres = ["Ana", "Luis", "Ana", "Maria", "Luis", "Pedro"]
print(eliminar_duplicados_mantener_orden(nombres))  # ['Ana', 'Luis', 'Maria', 'Pedro']
```

### 8.2 Encontrar diferencias entre grupos

```python
# Estudiantes que aprobaron cada examen
examen_1 = {"Ana", "Luis", "Maria", "Pedro", "Juan"}
examen_2 = {"Maria", "Pedro", "Juan", "Sofia", "Carlos"}

# Aprobaron ambos
ambos = examen_1 & examen_2
print(f"Aprobaron ambos: {ambos}")

# Solo examen_1
solo_1 = examen_1 - examen_2
print(f"Solo examen 1: {solo_1}")

# Solo examen_2
solo_2 = examen_2 - examen_1
print(f"Solo examen 2: {solo_2}")

# Aprobaron al menos uno
al_menos_uno = examen_1 | examen_2
print(f"Al menos uno: {al_menos_uno}")

# Aprobaron exactamente uno
exactamente_uno = examen_1 ^ examen_2
print(f"Exactamente uno: {exactamente_uno}")
```

### 8.3 Validación de entrada única

```python
def agregar_tag(tags_existentes, nuevo_tag):
    if nuevo_tag in tags_existentes:
        return tags_existentes, False
    tags_existentes.add(nuevo_tag)
    return tags_existentes, True

# Uso
tags = {"python", "javascript", "java"}
tags, agregado = agregar_tag(tags, "python")
print(f"Agregado: {agregado}, Tags: {tags}")  # False, sin cambios

tags, agregado = agregar_tag(tags, "rust")
print(f"Agregado: {agregado}, Tags: {tags}")  # True, rust agregado
```

### 8.4 Contar elementos únicos

```python
# Problema: contar visitantes únicos
visitas = [
    ("2024-01-01", "Ana"),
    ("2024-01-01", "Luis"),
    ("2024-01-01", "Ana"),      # Repetido
    ("2024-01-02", "Maria"),
    ("2024-01-02", "Ana"),      # Repetido
    ("2024-01-02", "Pedro"),
]

# Contar visitantes únicos por día
visitas_por_dia = {}
for fecha, nombre in visitas:
    if fecha not in visitas_por_dia:
        visitas_por_dia[fecha] = set()
    visitas_por_dia[fecha].add(nombre)

for fecha, visitantes in visitas_por_dia.items():
    print(f"{fecha}: {len(visitantes)} visitantes únicos")
# 2024-01-01: 2 visitantes únicos
# 2024-01-02: 3 visitantes únicos
```

### 8.5 Verificación de permisos

```python
# Definir permisos por rol
PERMISOS = {
    "admin": {"leer", "escribir", "eliminar", "configurar"},
    "editor": {"leer", "escribir"},
    "lector": {"leer"},
    "invitado": set()
}

def tiene_permiso(rol, permiso):
    return permiso in PERMISOS.get(rol, set())

# Uso
print(tiene_permiso("admin", "eliminar"))   # True
print(tiene_permiso("editor", "eliminar"))  # False
print(tiene_permiso("lector", "leer"))       # True

# Verificar múltiples permisos
def tiene_todos_los_permisos(rol, permisos_necesarios):
    permisos_rol = PERMISOS.get(rol, set())
    return permisos_necesarios.issubset(permisos_rol)

# El admin puede hacer cualquier cosa
print(tiene_todos_los_permisos("admin", {"leer", "escribir"}))  # True
print(tiene_todos_los_permisos("lector", {"leer", "escribir"}))  # False
```

### 8.6 Análisis de texto

```python
def analizar_textos(texto1, texto2):
    palabras1 = set(texto1.lower().split())
    palabras2 = set(texto2.lower().split())
    
    return {
        "palabras_unicas_1": len(palabras1),
        "palabras_unicas_2": len(palabras2),
        "palabras_comunes": palabras1 & palabras2,
        "solo_en_texto1": palabras1 - palabras2,
        "solo_en_texto2": palabras2 - palabras1,
        "palabras_totales": len(palabras1 | palabras2),
        "similitud": len(palabras1 & palabras2) / len(palabras1 | palabras2) * 100
    }

texto1 = "El veloz murcielago hindu comia feliz cardillo y kiwi"
texto2 = "El murcielago feliz comia frutas y verdura"

resultado = analizar_textos(texto1, texto2)
print(f"Palabras únicas texto 1: {resultado['palabras_unicas_1']}")
print(f"Similitud: {resultado['similitud']:.1f}%")
print(f"Comunes: {resultado['palabras_comunes']}")
```

---

## 9. Conjuntos vs Listas

### 9.1 Tabla comparativa

| Característica | Conjunto | Lista |
|----------------|----------|-------|
| Orden | No mantiene orden | Mantiene orden |
| Duplicados | Elimina automáticamente | Permite |
| Membresía (in) | O(1) promedio | O(n) |
| Acceso por índice | No | Sí |
| Mutable | Sí | Sí |
| Elementos hashables | Sí (para elementos) | No |
| Memoria | Más eficiente para únicos | Más memoria |
| Operaciones de conjunto | Nativas | No disponibles |

### 9.2 Comparación de rendimiento

```python
import time

n = 10000

# Crear lista con duplicados vs conjunto único
lista_grande = list(range(n)) + list(range(n // 2))
inicio = time.time()
unicos_lista = list(set(lista_grande))
tiempo_lista = time.time() - inicio

# Buscar elemento
inicio = time.time()
for _ in range(1000):
    n // 2 in set(range(n))
tiempo_set = time.time() - inicio

inicio = time.time()
for _ in range(1000):
    n // 2 in list(range(n))
tiempo_lista = time.time() - inicio

print(f"Tiempo búsqueda en set: {tiempo_set:.6f}s")
print(f"Tiempo búsqueda en lista: {tiempo_lista:.6f}s")
```

### 9.3 Cuándo usar cada uno

```python
# USAR CONJUNTOS cuando:
# [+] Necesitas eliminar duplicados
ids = [101, 102, 101, 103, 102]
unicos = set(ids)
print(unicos)  # {101, 102, 103}

# [+] Necesitas verificar pertenencia frecuentemente
categorias_permitidas = {"A", "B", "C"}
categoria_usuario = "B"
print(categoria_usuario in categorias_permitidas)  # True

# [+] Necesitas operaciones de conjuntos
clientes_2023 = {"Ana", "Luis", "Maria"}
clientes_2024 = {"Luis", "Maria", "Pedro"}
nuevos_clientes = clientes_2024 - clientes_2023
print(nuevos_clientes)  # {'Pedro'}

# USAR LISTAS cuando:
# [+] Necesitas mantener orden
prioridades = ["baja", "media", "alta"]
print(prioridades[0])  # "baja"

# [+] Necesitas elementos duplicados
numeros_ganadores = [1, 1, 1]  # Múltiples ganadores
print(numeros_ganadores.count(1))  # 3

# [+] Necesitas acceder por índice
elementos = ["primero", "segundo", "tercero"]
print(elementos[1])  # "segundo"

# [+] Necesitas modificar/ordenar
elementos = [3, 1, 2]
elementos.append(4)
elementos.sort()
```

### 9.4 Conversiones comunes

```python
# Lista a Conjunto
lista = [1, 2, 2, 3, 3, 3]
conjunto = set(lista)
print(conjunto)  # {1, 2, 3}

# Conjunto a Lista
conjunto = {1, 2, 3}
lista = list(conjunto)
print(lista)  # [1, 2, 3] (orden arbitrario)

# Lista a Conjunto a Lista (eliminar duplicados, orden arbitrario)
numeros = [1, 2, 2, 3, 1, 4, 3]
unicos = list(set(numeros))
print(unicos)  # [1, 2, 3, 4]

# Eliminar duplicados manteniendo orden
def unicos_ordenados(lista):
    return list(dict.fromkeys(lista))

numeros = [1, 2, 2, 3, 1, 4, 3]
print(unicos_ordenados(numeros))  # [1, 2, 3, 4]
```

---

## 10. Ejercicios Prácticos

### 10.1 Ejercicio: Sistema de etiquetas

```python
class SistemaEtiquetas:
    def __init__(self):
        self.etiquetas = set()
    
    def agregar(self, etiqueta):
        if etiqueta in self.etiquetas:
            return False
        self.etiquetas.add(etiqueta)
        return True
    
    def quitar(self, etiqueta):
        self.etiquetas.discard(etiqueta)
    
    def buscar_similares(self, otras_etiquetas):
        return self.etiquetas & otras_etiquetas
    
    def buscar_exclusivas(self, otras_etiquetas):
        return self.etiquetas - otras_etiquetas
    
    def совпадения(self, otras_etiquetas):
        return self.etiquetas.isdisjoint(otras_etiquetas)
    
    def __str__(self):
        return f"Etiquetas({len(self.etiquetas)}): {self.etiquetas}"

# Uso
blog = SistemaEtiquetas()
blog.agregar("python")
blog.agregar("programacion")
blog.agregar("tutorial")
print(blog)

# Etiquetas relacionadas
relacionadas = {"python", "django", "web"}
print(f"Similares: {blog.buscar_similares(relacionadas)}")  # {'python'}
print(f"Exclusivas: {blog.buscar_exclusivas(relacionadas)}")  # {'programacion', 'tutorial'}
```

### 10.2 Ejercicio: Detección de plagio simple

```python
def calcular_similitud(texto1, texto2):
    palabras1 = set(texto1.lower().split())
    palabras2 = set(texto2.lower().split())
    
    interseccion = palabras1 & palabras2
    union = palabras1 | palabras2
    
    if not union:
        return 0.0
    
    indice_similitud = len(interseccion) / len(union)
    return indice_similitud * 100

textos = [
    "La programacion en Python es divertida y poderosa",
    "Python es un lenguaje de programacion divertido",
    "Java es otro lenguaje de programacion popular"
]

for i in range(len(textos)):
    for j in range(i + 1, len(textos)):
        similitud = calcular_similitud(textos[i], textos[j])
        print(f"Texto {i+1} vs {j+1}: {similitud:.1f}%")
```

### 10.3 Ejercicio: Gestor de palabras prohibidas

```python
class FiltroPalabras:
    def __init__(self, prohibidas=None):
        self.prohibidas = set(prohibidas) if prohibidas else set()
        self.detectadas = set()
    
    def agregar_prohibida(self, palabra):
        self.prohibidas.add(palabra.lower())
    
    def contiene_prohibida(self, texto):
        palabras = set(texto.lower().split())
        coincidencias = palabras & self.prohibidas
        self.detectadas.update(coincidencias)
        return len(coincidencias) > 0
    
    def filtrar(self, texto):
        palabras = texto.split()
        resultado = [
            "***" if p.lower() in self.prohibidas else p
            for p in palabras
        ]
        return " ".join(resultado)
    
    def reporte(self):
        return {
            "total_prohibidas": len(self.prohibidas),
            "detectadas_historico": len(self.detectadas),
            "palabras_detectadas": self.detectadas
        }

# Uso
filtro = FiltroPalabras(["spam", "oferta", "gratis"])
print(filtro.contiene_prohibida("Esto es spam"))  # True
print(filtro.filtrar("Gana dinero gratis hoy"))  # Gana dinero *** hoy
print(filtro.reporte())
```

### 10.4 Ejercicio: Agenda de contactos con grupos

```python
class AgendaContactos:
    def __init__(self):
        self.contactos = {}
    
    def agregar_contacto(self, nombre, telefono, grupos=None):
        grupos = grupos or set()
        self.contactos[nombre] = {
            "telefono": telefono,
            "grupos": set(grupos)
        }
    
    def agregar_a_grupo(self, nombre, grupo):
        if nombre in self.contactos:
            self.contactos[nombre]["grupos"].add(grupo)
    
    def contactos_en_grupo(self, grupo):
        return {
            nombre for nombre, datos in self.contactos.items()
            if grupo in datos["grupos"]
        }
    
    def contactos_en_todos_los_grupos(self, grupos):
        grupos_set = set(grupos)
        return {
            nombre for nombre, datos in self.contactos.items()
            if grupos_set.issubset(datos["grupos"])
        }
    
    def grupos_en_comun(self, nombre1, nombre2):
        if nombre1 not in self.contactos or nombre2 not in self.contactos:
            return set()
        return (
            self.contactos[nombre1]["grupos"] &
            self.contactos[nombre2]["grupos"]
        )

# Uso
agenda = AgendaContactos()
agenda.agregar_contacto("Ana", "123", ["trabajo", "deportes"])
agenda.agregar_contacto("Luis", "456", ["trabajo", "musica"])
agenda.agregar_contacto("Maria", "789", ["familia", "deportes"])

print("En trabajo:", agenda.contactos_en_grupo("trabajo"))
print("Grupos en común Ana-Luis:", agenda.grupos_en_comun("Ana", "Luis"))
```

---

## Resumen Rápido

```python
# CREAR
conjunto = set()                   # Vacío
conjunto = {1, 2, 3}              # Con elementos
conjunto = set([1, 2, 2, 3])      # Desde lista
conjunto = frozenset([1, 2, 3])   # Inmutable

# AÑADIR
conjunto.add(4)                    # Un elemento
conjunto.update([5, 6])            # Múltiples elementos

# ELIMINAR
conjunto.remove(3)                # Error si no existe
conjunto.discard(3)               # Sin error
conjunto.pop()                     # Elimina arbitrario
conjunto.clear()                   # Vaciar

# OPERACIONES
a | b                              # Unión
a & b                              # Intersección
a - b                              # Diferencia
a ^ b                              # Diferencia simétrica
a <= b                             # Subconjunto
a >= b                             # Superconjunto
a.isdisjoint(b)                    # ¿Disjuntos?

# ITERAR
for x in conjunto: pass
{x for x in range(10)}            # Comprensión
```

---

## Consejos Finales

- [+] Usa `set()` para crear conjuntos, no `{}` (eso es un dict)
- [+] Usa `discard()` en lugar de `remove()` si no estás seguro del contenido
- [+] Usa frozenset cuando necesites inmutabilidad o como clave de dict
- [!] Recuerda: los conjuntos NO mantienen orden
- [!] Los elementos deben ser hashables (inmutables)
- [x] No intentes acceder a elementos por índice
- [+] Los conjuntos son ideales para eliminar duplicados eficientemente

---

## Diccionarios

# 1. Introducción

Los diccionarios son estructuras de datos fundamentales en Python que almacenan pares **clave-valor**. A diferencia de las listas que usan índices numéricos, los diccionarios te permiten acceder a los valores usando **claves** que pueden ser de casi cualquier tipo inmutable.

### Analogía del mundo real

Imagina un diccionario real (como el de la RAE):
- Cada **palabra** (clave) tiene una **definición** (valor)
- Buscas la palabra y obtienes su significado
- Cada palabra es única, pero puede tener múltiples definiciones

```python
# Diccionario español
diccionario = {
    "python": "Lenguaje de programación de alto nivel",
    "algoritmo": "Conjunto de pasos para resolver un problema",
    "variable": "Espacio de memoria para almacenar datos"
}

print(diccionario["python"])
# Lenguaje de programación de alto nivel
```

### ¿Cuándo usar diccionarios?

- [+] Almacenar datos relacionados por nombre/categoría
- [+] Búsquedas rápidas por clave (O(1) promedio)
- [+] Cuando necesitas associating valores con claves únicas
- [+] Para contar ocurrencias de elementos
- [+] Para representar estructuras JSON
- [x] No usar si necesitas mantener orden de inserción (usar OrderedDict o lista)
- [x] No usar si las claves no son hashables

---

## 2. Creación de Diccionarios

### 2.1 Creación básica con llaves {}

```python
# Diccionario vacío
vacio = {}
vacio_explicito = dict()

print(vacio)   # {}
print(type(vacio))  # <class 'dict'>

# Con elementos
persona = {
    "nombre": "Ana",
    "edad": 25,
    "ciudad": "Madrid"
}
print(persona)  # {'nombre': 'Ana', 'edad': 25, 'ciudad': 'Madrid'}
```

### 2.2 Usando dict()

```python
# Desde pares clave-valor
colores = dict([("rojo", "#FF0000"), ("verde", "#00FF00"), ("azul", "#0000FF")])
print(colores)  # {'rojo': '#FF0000', 'verde': '#00FF00', 'azul': '#0000FF'}

# Usando keyword arguments
persona = dict(nombre="Luis", edad=30, ciudad="Barcelona")
print(persona)  # {'nombre': 'Luis', 'edad': 30, 'ciudad': 'Barcelona'}

# Desde dos listas con zip
claves = ["nombre", "edad", "profesion"]
valores = ["Maria", 28, "Ingeniera"]
persona = dict(zip(claves, valores))
print(persona)  # {'nombre': 'Maria', 'edad': 28, 'profesion': 'Ingeniera'}
```

### 2.3 Claves válidas e inválidas

[!] Las claves deben ser **hashables** (generalmente inmutables):

```python
# Claves válidas
validas = {
    "nombre": "Ana",           # str
    1: "uno",                 # int
    (1, 2): "coordenada",     # tuple (hashable)
    3.14: "pi",               # float
    True: "verdadero",        # bool
    None: "nada"              # None
}
print(validas)  # {'nombre': 'Ana', 1: 'uno', (1, 2): 'coordenada', ...}

# Claves INVÁLIDAS (listas y diccionarios son mutables)
# invalido = {[1, 2]: "lista"}   # TypeError: unhashable type: 'list'
# invalido = {{"a": 1}: "dict"}  # TypeError: unhashable type: 'dict'
```

### 2.4 Valores de cualquier tipo

```python
# Los valores pueden ser de cualquier tipo
mixto = {
    "entero": 42,
    "flotante": 3.14,
    "texto": "hola",
    "lista": [1, 2, 3],
    "tupla": (1, 2, 3),
    "diccionario": {"a": 1},
    "funcion": print,
    "nada": None,
    "booleano": True
}
print(mixto)
```

---

## 3. Acceso y Modificación

### 3.1 Acceso por clave

```python
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}

# Acceso directo
print(persona["nombre"])   # Ana
print(persona["edad"])     # 25

# [!] Clave inexistente genera KeyError
# print(persona["pais"])  # KeyError: 'pais'
```

### 3.2 Acceso seguro con get()

```python
persona = {"nombre": "Ana", "edad": 25}

# get() retorna None si no existe
print(persona.get("nombre"))     # Ana
print(persona.get("pais"))       # None

# get() con valor por defecto
print(persona.get("pais", "España"))  # España
print(persona.get("edad", 0))          # 25
```

[!] Recomendación: Usa siempre `get()` cuando no estés seguro de que la clave exista.

### 3.3 Agregar elementos

```python
persona = {"nombre": "Ana"}

# Agregar nueva clave-valor
persona["edad"] = 25
print(persona)  # {'nombre': 'Ana', 'edad': 25}

# Agregar múltiples con update()
persona.update({"ciudad": "Madrid", "profesion": "Ingeniera"})
print(persona)  # {'nombre': 'Ana', 'edad': 25, 'ciudad': 'Madrid', 'profesion': 'Ingeniera'}

# Sobrescribir valor existente
persona["edad"] = 26
print(persona)  # {'nombre': 'Ana', 'edad': 26, ...}
```

### 3.4 Eliminar elementos

```python
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}

# del - Eliminar por clave
del persona["edad"]
print(persona)  # {'nombre': 'Ana', 'ciudad': 'Madrid'}

# pop() - Eliminar y retornar valor
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}
valor = persona.pop("edad")
print(f"Eliminado: {valor}")  # 25
print(persona)  # {'nombre': 'Ana', 'ciudad': 'Madrid'}

# pop() con valor por defecto
valor = persona.pop("pais", "No existe")
print(valor)  # No existe

# popitem() - Eliminar último elemento (Python 3.7+)
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}
clave, valor = persona.popitem()
print(f"Eliminado: {clave}={valor}")  # ciudad=Madrid
print(persona)  # {'nombre': 'Ana', 'edad': 25}

# clear() - Vaciar diccionario
persona.clear()
print(persona)  # {}
```

### 3.5 Modificación condicional

```python
# Agregar solo si no existe
if "email" not in datos:
    datos["email"] = "default@ejemplo.com"

# O más limpio con setdefault()
datos = {"nombre": "Ana"}
datos.setdefault("email", "default@ejemplo.com")
print(datos)  # {'nombre': 'Ana', 'email': 'default@ejemplo.com'}

# Si ya existe, no modifica
datos.setdefault("email", "otro@ejemplo.com")
print(datos)  # {'nombre': 'Ana', 'email': 'default@ejemplo.com'} - Sin cambios
```

---

## 4. Métodos de Diccionarios

### 4.1 Métodos de vista

```python
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}

# keys() - Ver todas las claves
print(persona.keys())    # dict_keys(['nombre', 'edad', 'ciudad'])

# values() - Ver todos los valores
print(persona.values())  # dict_values(['Ana', 25, 'Madrid'])

# items() - Ver pares clave-valor
print(persona.items())
# dict_items([('nombre', 'Ana'), ('edad', 25), ('ciudad', 'Madrid')])
```

[!] Los objetos de vista reflejan cambios en el diccionario:

```python
persona = {"nombre": "Ana", "edad": 25}

claves = persona.keys()
print(list(claves))  # ['nombre', 'edad']

persona["ciudad"] = "Madrid"
print(list(claves))  # ['nombre', 'edad', 'ciudad'] - Actualizado automáticamente
```

### 4.2 Métodos de copia

```python
# copy() - Copia superficial
original = {"nombre": "Ana", "datos": [1, 2, 3]}
copia = original.copy()

copia["nombre"] = "Luis"
print(original)  # {'nombre': 'Ana', ...} - No cambia
print(copia)    # {'nombre': 'Luis', ...}

# [!] Copia superficial: objetos mutables internos se comparten
copia["datos"].append(4)
print(original["datos"])  # [1, 2, 3, 4] - ¡También cambió!
print(copia["datos"])    # [1, 2, 3, 4]
```

### 4.3 Copia profunda

```python
import copy

original = {"nombre": "Ana", "datos": [1, 2, 3]}
copia = copy.deepcopy(original)

copia["datos"].append(4)
print(original["datos"])  # [1, 2, 3] - No cambia
print(copia["datos"])    # [1, 2, 3, 4]
```

### 4.4 Métodos de actualización

```python
a = {"x": 1, "y": 2}
b = {"y": 3, "z": 4}

# update() - Combinar diccionarios
a.update(b)
print(a)  # {'x': 1, 'y': 3, 'z': 4} - Sobrescribe valores existentes

# | (Python 3.9+) - Unión
a = {"x": 1, "y": 2}
b = {"y": 3, "z": 4}
resultado = a | b
print(resultado)  # {'x': 1, 'y': 3, 'z': 4}

# |= (Python 3.9+) - Unión in-place
a |= b
print(a)  # {'x': 1, 'y': 3, 'z': 4}
```

### 4.5 Otros métodos útiles

```python
datos = {"a": 1, "b": 2, "c": 3}

# len() - Número de elementos
print(len(datos))  # 3

# in / not in - Verificar clave
print("a" in datos)      # True
print("x" not in datos)  # True

# setdefault() - Obtener o crear
valor = datos.setdefault("d", 4)
print(valor)    # 4
print(datos)    # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# fromkeys() - Crear diccionario con valores por defecto
claves = ["a", "b", "c"]
valores_default = dict.fromkeys(claves, 0)
print(valores_default)  # {'a': 0, 'b': 0, 'c': 0}

# Solo un valor por defecto (el mismo objeto)
valores_default = dict.fromkeys(claves, [])
valores_default["a"].append(1)
print(valores_default)  # {'a': [1], 'b': [1], 'c': [1]} - ¡Todos comparten la misma lista!
```

---

## 5. Iteración

### 5.1 Iterar sobre claves

```python
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}

# Forma directa
for clave in persona:
    print(clave)

# Usando keys()
for clave in persona.keys():
    print(clave)
```

### 5.2 Iterar sobre valores

```python
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}

# Usando values()
for valor in persona.values():
    print(valor)

# Contar valores únicos
valores = [1, 2, 2, 3, 3, 3]
conteo = {}
for v in valores:
    conteo[v] = conteo.get(v, 0) + 1
print(conteo)  # {1: 1, 2: 2, 3: 3}
```

### 5.3 Iterar sobre pares clave-valor

```python
persona = {"nombre": "Ana", "edad": 25, "ciudad": "Madrid"}

# Usando items()
for clave, valor in persona.items():
    print(f"{clave}: {valor}")

# Con enumerate
for i, (clave, valor) in enumerate(persona.items()):
    print(f"{i}. {clave}: {valor}")
```

### 5.4 Iteración ordenada

```python
persona = {"ciudad": "Madrid", "nombre": "Ana", "edad": 25}

# Ordenar por clave
for clave in sorted(persona.keys()):
    print(f"{clave}: {persona[clave]}")

# Ordenar por valor
for clave, valor in sorted(persona.items(), key=lambda x: x[1]):
    print(f"{clave}: {valor}")

# Orden inverso
for clave in sorted(persona.keys(), reverse=True):
    print(f"{clave}: {persona[clave]}")
```

---

## 6. Comprensión de Diccionarios

### 6.1 Sintaxis básica

```python
# Tradicional
cuadrados = {}
for x in range(5):
    cuadrados[x] = x ** 2
print(cuadrados)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Con comprensión
cuadrados = {x: x ** 2 for x in range(5)}
print(cuadrados)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### 6.2 Con condición (filtrado)

```python
# Solo números pares
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares = {x: x ** 2 for x in numeros if x % 2 == 0}
print(pares)  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}

# Filtrar diccionario existente
datos = {"a": 1, "b": 2, "c": 3, "d": 4}
filtrado = {k: v for k, v in datos.items() if v > 2}
print(filtrado)  # {'c': 3, 'd': 4}
```

### 6.3 Con transformación

```python
# Invertir clave-valor
original = {"a": 1, "b": 2, "c": 3}
invertido = {v: k for k, v in original.items()}
print(invertido)  # {1: 'a', 2: 'b', 3: 'c'}

#[!] Cuidado con valores duplicados
original = {"a": 1, "b": 1, "c": 2}
invertido = {v: k for k, v in original.items()}
print(invertido)  # {1: 'b', 2: 'c'} - 'a' se pierde

# Transformar valores
precios = {"manzana": 1.5, "pera": 2.0, "uva": 3.0}
con_iva = {k: round(v * 1.21, 2) for k, v in precios.items()}
print(con_iva)  # {'manzana': 1.82, 'pera': 2.42, 'uva': 3.63}
```

### 6.4 Ejemplos prácticos

```python
# Contar frecuencia de palabras
texto = "hola mundo hola python hola mundo mundo"
frecuencia = {palabra: texto.split().count(palabra) for palabra in set(texto.split())}
print(frecuencia)  # {'hola': 3, 'mundo': 3, 'python': 1}

# Mejor forma de contar
from collections import Counter
frecuencia = Counter(texto.split())
print(frecuencia)  # Counter({'hola': 3, 'mundo': 3, 'python': 1})

# Crear diccionario desde listas
nombres = ["Ana", "Luis", "Maria"]
edades = [25, 30, 28]
personas = {nombre: edad for nombre, edad in zip(nombres, edades)}
print(personas)  # {'Ana': 25, 'Luis': 30, 'Maria': 28}

# Clasificar por longitud
palabras = ["sol", "luna", "estrella", "cielo", "mar"]
por_longitud = {}
for palabra in palabras:
    longitud = len(palabra)
    if longitud not in por_longitud:
        por_longitud[longitud] = []
    por_longitud[longitud].append(palabra)
print(por_longitud)  # {3: ['sol', 'mar'], 4: ['luna', 'cielo'], 8: ['estrella']}
```

---

## 7. Diccionarios Anidados

### 7.1 Diccionarios dentro de diccionarios

```python
# Estructura de empresa
empresa = {
    "departamento": "IT",
    "empleados": {
        "Ana": {"edad": 25, "cargo": "Developer"},
        "Luis": {"edad": 30, "cargo": "Lead"}
    }
}

# Acceder a elementos anidados
print(empresa["empleados"]["Ana"]["cargo"])  # Developer

# Modificar elementos anidados
empresa["empleados"]["Ana"]["edad"] = 26
print(empresa["empleados"]["Ana"])  # {'edad': 26, 'cargo': 'Developer'}

# Agregar nuevo empleado
empresa["empleados"]["Maria"] = {"edad": 28, "cargo": "Designer"}
print(empresa["empleados"])
```

### 7.2 Listas dentro de diccionarios

```python
# Estudiantes y sus cursos
cursos = {
    "Ana": ["Matematicas", "Fisica", "Quimica"],
    "Luis": ["Historia", "Geografia"],
    "Maria": ["Matematicas", "Historia"]
}

# Agregar curso a estudiante
cursos["Luis"].append("Matematicas")
print(cursos["Luis"])  # ['Historia', 'Geografia', 'Matematicas']

# Encontrar estudiantes en un curso
def estudiantes_en_curso(curso):
    return [nombre for nombre, cursos_est in cursos.items() if curso in cursos_est]

print(estudiantes_en_curso("Matematicas"))  # ['Ana', 'Luis', 'Maria']
```

### 7.3 Recorrer diccionarios anidados

```python
empresa = {
    "IT": {"Ana": 25, "Luis": 30},
    "RRHH": {"Maria": 28, "Pedro": 35}
}

# Recorrer departamento y empleados
for departamento, empleados in empresa.items():
    print(f"\n{departamento}:")
    for nombre, edad in empleados.items():
        print(f"  {nombre}: {edad}")
```

---

## 8. Defaultdict y Orden

### 8.1 defaultdict

```python
from collections import defaultdict

# El problema con diccionarios normales
# conteo = {}
# for palabra in ["hola", "hola", "mundo"]:
#     conteo[palabra] += 1  # KeyError: 'hola'

# Solución con defaultdict
conteo = defaultdict(int)
for palabra in ["hola", "hola", "mundo"]:
    conteo[palabra] += 1
print(conteo)  # defaultdict(<class 'int'>, {'hola': 2, 'mundo': 1})

# Con list
agrupacion = defaultdict(list)
for nombre in ["Ana", "Luis", "Ana", "Maria", "Luis"]:
    agrupacion[nombre[0]].append(nombre)
print(agrupacion)
# defaultdict(<class 'list'>, {'A': ['Ana', 'Ana'], 'L': ['Luis', 'Luis'], 'M': ['Maria']})

# Con set
conjuntos = defaultdict(set)
for palabra in ["hola", "holo", "casa", "cosa"]:
    conjuntos[palabra[0]].add(palabra)
print(conjuntos)
# defaultdict(<class 'set'>, {'h': {'hola', 'holo'}, 'c': {'casa', 'cosa'}})
```

### 8.2 Tipos de defaultdict

```python
from collections import defaultdict

# int - Para contar
conteo = defaultdict(int)
palabras = ["gato", "perro", "gato", "raton", "gato", "perro"]
for p in palabras:
    conteo[p] += 1
print(dict(conteo))  # {'gato': 3, 'perro': 2, 'raton': 1}

# list - Para agrupar
grupos = defaultdict(list)
for nombre, edad in [("Ana", 25), ("Luis", 30), ("Maria", 25)]:
    grupos[edad].append(nombre)
print(dict(grupos))  # {25: ['Ana', 'Maria'], 30: ['Luis']}

# dict - Para diccionarios anidados
anidado = defaultdict(lambda: defaultdict(list))
anidado["depto"]["empleados"].append("Ana")
print(dict(anidado))
# {'depto': defaultdict(<class 'list'>, {'empleados': ['Ana']})}

# set - Para conjuntos
valores_unicos = defaultdict(set)
valores_unicos["pares"].add(2)
valores_unicos["pares"].add(4)
valores_unicos["impares"].add(1)
print(dict(valores_unicos))  # {'pares': {2, 4}, 'impares': {1}}
```

### 8.3 Orden de inserción (Python 3.7+)

[!] Desde Python 3.7, los diccionarios mantienen el **orden de inserción**:

```python
# Orden de inserción garantizado
datos = {}
datos["z"] = 1
datos["a"] = 2
datos["m"] = 3
print(list(datos.keys()))  # ['z', 'a', 'm'] - Mantiene orden de inserción

# En Python 3.6 y anteriores, el orden NO estaba garantizado
# Para compatibilidad, usa OrderedDict
from collections import OrderedDict
```

### 8.4 OrderedDict (legacy)

```python
from collections import OrderedDict

# Similar a dict pero con orden garantizado (pre-Python 3.7)
ordenado = OrderedDict()
ordenado["z"] = 1
ordenado["a"] = 2
ordenado["m"] = 3
print(list(ordenado.keys()))  # ['z', 'a', 'm']

# Mover elemento al final
ordenado.move_to_end("z")
print(list(ordenado.keys()))  # ['a', 'm', 'z']

# Mover al inicio
ordenado.move_to_end("a", last=False)
print(list(ordenado.keys()))  # ['a', 'm', 'z']

# popitem() desde el final
clave, valor = ordenado.popitem()
print(clave, valor)  # 'z' 3
```

---

## 9. Uso Práctico

### 9.1 Contar elementos

```python
from collections import Counter

# Con Counter
texto = "mississippi"
conteo = Counter(texto)
print(conteo)  # Counter({'i': 4, 's': 4, 'p': 2})
print(conteo.most_common(3))  # [('i', 4), ('s', 4), ('p', 2)]

# Sin Counter
def contar_elementos(iterable):
    conteo = {}
    for elemento in iterable:
        conteo[elemento] = conteo.get(elemento, 0) + 1
    return conteo

print(contar_elementos("abracadabra"))
# {'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1}
```

### 9.2 Buscar y agrupar

```python
from collections import defaultdict

# Agrupar por atributo
estudiantes = [
    {"nombre": "Ana", "carrera": "Ing", "nota": 8.5},
    {"nombre": "Luis", "carrera": "Der", "nota": 7.0},
    {"nombre": "Maria", "carrera": "Ing", "nota": 9.0},
    {"nombre": "Pedro", "carrera": "Der", "nota": 6.5}
]

# Agrupar por carrera
por_carrera = defaultdict(list)
for est in estudiantes:
    por_carrera[est["carrera"]].append(est["nombre"])

print(por_carrera)  # {'Ing': ['Ana', 'Maria'], 'Der': ['Luis', 'Pedro']}

# Promedio por carrera
promedios = {}
for est in estudiantes:
    carrera = est["carrera"]
    if carrera not in promedios:
        promedios[carrera] = {"suma": 0, "count": 0}
    promedios[carrera]["suma"] += est["nota"]
    promedios[carrera]["count"] += 1

for carrera, datos in promedios.items():
    print(f"{carrera}: {datos['suma']/datos['count']:.2f}")
```

### 9.3 Caché simple

```python
def fibonacci_cache(n, cache={}):
    if n in cache:
        return cache[n]
    if n <= 1:
        return n
    cache[n] = fibonacci_cache(n - 1, cache) + fibonacci_cache(n - 2, cache)
    return cache[n]

# O usando un wrapper
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print([fibonacci(i) for i in range(10)])  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### 9.4 Fusionar diccionarios

```python
# Python 3.9+
a = {"x": 1, "y": 2}
b = {"y": 3, "z": 4}

# Usando |
fusionado = a | b
print(fusionado)  # {'x': 1, 'y': 3, 'z': 4}

# Fusionar múltiples
c = {"w": 5}
resultado = a | b | c
print(resultado)  # {'x': 1, 'y': 3, 'z': 4, 'w': 5}

# Versión tradicional
def fusionar(*diccionarios):
    resultado = {}
    for d in diccionarios:
        resultado.update(d)
    return resultado

print(fusionar(a, b, c))  # {'x': 1, 'y': 3, 'z': 4, 'w': 5}
```

### 9.5 Diccionarios como switch/case

```python
# En lugar de múltiples if-elif
def operacion(tipo, a, b):
    return {
        "suma": lambda: a + b,
        "resta": lambda: a - b,
        "multiplica": lambda: a * b,
        "divide": lambda: a / b if b != 0 else "Error"
    }.get(tipo, lambda: "Operación inválida")()

print(operacion("suma", 10, 5))      # 15
print(operacion("multiplica", 10, 5))  # 50
print(operacion("desconocida", 10, 5))  # Operación inválida
```

### 9.6 Configuración flexible

```python
# Valores por defecto con cadena de reemplazo
defaults = {
    "host": "localhost",
    "puerto": 5432,
    "base_datos": "miapp",
    "debug": False
}

# Sobrescribir con configuración del usuario
user_config = {
    "host": "produccion.com",
    "debug": True
}

# Fusionar
config = {**defaults, **user_config}
print(config)
# {'host': 'produccion.com', 'puerto': 5432, 'base_datos': 'miapp', 'debug': True}
```

---

## 10. Diccionarios vs Otras Estructuras

### 10.1 Tabla comparativa

| Característica | Dict | List | Set | Tuple |
|----------------|------|------|-----|-------|
| Acceso | Por clave | Por índice | No accesible | Por índice |
| Orden | Inserción (3.7+) | Sí | No | Sí |
| Duplicados | Valores sí, claves no | Sí | No | Sí |
| Mutable | Sí | Sí | Sí | No |
| Búsqueda por clave | O(1) | O(n) | - | - |
| Búsqueda por valor | O(n) | O(n) | O(n) | O(n) |
| Memoria | Media-Alta | Baja | Baja | Baja |

### 10.2 Cuándo usar cada estructura

```python
# USAR DICCIONARIO cuando:
# [+] Necesitas buscar por clave
telefonos = {"Ana": "123", "Luis": "456", "Maria": "789"}
print(telefonos["Ana"])  # 123

# [+] Necesitas asociación clave-valor
config = {"debug": True, "timeout": 30}

# [+] Necesitas contar frecuencias
conteo = {"a": 5, "b": 3, "c": 2}

# [+] Necesitas estructura jerárquica
datos = {"usuario": {"nombre": "Ana", "roles": ["admin"]}}

# USAR LISTA cuando:
# [+] Necesitas orden secuencial
tareas = ["comprar", "cocinar", "limpiar"]

# [+] Necesitas duplicados
numeros = [1, 1, 1, 2, 2, 3]

# [+] Necesitas acceso por índice
primero = tareas[0]

# USAR SET cuando:
# [+] Necesitas elementos únicos
etiquetas = {"python", "django", "web"}

# [+] Necesitas operaciones de conjunto
comunes = set1 & set2
```

### 10.3 Conversiones

```python
# De diccionario a lista/tupla/set
datos = {"a": 1, "b": 2, "c": 3}

# Solo claves
print(list(datos.keys()))   # ['a', 'b', 'c']
print(tuple(datos.keys()))  # ('a', 'b', 'c')
print(set(datos.keys()))    # {'a', 'b', 'c'}

# Solo valores
print(list(datos.values()))   # [1, 2, 3]
print(tuple(datos.values()))  # (1, 2, 3)
print(set(datos.values()))   # {1, 2, 3}

# Clave-valor como tuplas
print(list(datos.items()))  # [('a', 1), ('b', 2), ('c', 3)]

# De lista de tuplas a diccionario
pares = [("a", 1), ("b", 2), ("c", 3)]
print(dict(pares))  # {'a': 1, 'b': 2, 'c': 3}
```

---

## 11. Ejercicios Prácticos

### 11.1 Ejercicio: Inventario de productos

```python
class Inventario:
    def __init__(self):
        self.productos = {}
    
    def agregar(self, codigo, nombre, cantidad, precio):
        self.productos[codigo] = {
            "nombre": nombre,
            "cantidad": cantidad,
            "precio": precio
        }
    
    def buscar(self, codigo):
        return self.productos.get(codigo, "No encontrado")
    
    def actualizar_cantidad(self, codigo, cantidad):
        if codigo in self.productos:
            self.productos[codigo]["cantidad"] += cantidad
            return True
        return False
    
    def valor_total(self):
        return sum(
            p["cantidad"] * p["precio"]
            for p in self.productos.values()
        )
    
    def productos_bajos(self, umbral=5):
        return {
            codigo: datos
            for codigo, datos in self.productos.items()
            if datos["cantidad"] < umbral
        }

# Uso
inv = Inventario()
inv.agregar("P001", "Manzana", 100, 0.50)
inv.agregar("P002", "Naranja", 3, 0.75)
inv.agregar("P003", "Platano", 50, 0.30)

print(inv.buscar("P001"))
print(f"Valor total: {inv.valor_total():.2f}")
print(f"Bajos en stock: {inv.productos_bajos()}")
```

### 11.2 Ejercicio: Historial de comandos

```python
class HistorialComandos:
    def __init__(self):
        self.historial = {}
    
    def ejecutar(self, comando):
        if comando not in self.historial:
            self.historial[comando] = {"ejecuciones": 0, "ultima_vez": None}
        self.historial[comando]["ejecuciones"] += 1
        self.historial[comando]["ultima_vez"] = "ahora"
        return f"Ejecutado: {comando}"
    
    def mas_ejecutados(self, n=5):
        return sorted(
            self.historial.items(),
            key=lambda x: x[1]["ejecuciones"],
            reverse=True
        )[:n]
    
    def comandos_sin_usar(self):
        return [
            cmd for cmd, datos in self.historial.items()
            if datos["ejecuciones"] == 0
        ]

# Uso
hist = HistorialComandos()
for cmd in ["ls", "cd", "ls", "pwd", "ls", "ls", "cd", "ls"]:
    hist.ejecutar(cmd)

print("Mas usados:")
for cmd, datos in hist.mas_ejecutados(3):
    print(f"  {cmd}: {datos['ejecuciones']} veces")
```

### 11.3 Ejercicio: Matrix dispersa

```python
class MatrizDispersa:
    def __init__(self, filas, cols):
        self.filas = filas
        self.cols = cols
        self.datos = {}  # {(fila, col): valor}
    
    def asignar(self, fila, col, valor):
        if 0 <= fila < self.filas and 0 <= col < self.cols:
            if valor != 0:
                self.datos[(fila, col)] = valor
            elif (fila, col) in self.datos:
                del self.datos[(fila, col)]
        else:
            raise IndexError("Posicion fuera de rango")
    
    def obtener(self, fila, col):
        return self.datos.get((fila, col), 0)
    
    def __str__(self):
        lineas = []
        for i in range(self.filas):
            fila = [str(self.obtener(i, j)) for j in range(self.cols)]
            lineas.append(" ".join(fila))
        return "\n".join(lineas)
    
    def elementos_no_cero(self):
        return len(self.datos)

# Uso
matriz = MatrizDispersa(5, 5)
matriz.asignar(0, 0, 1)
matriz.asignar(0, 4, 1)
matriz.asignar(4, 0, 1)
matriz.asignar(4, 4, 1)
matriz.asignar(2, 2, 5)

print(matriz)
print(f"Elementos no cero: {matriz.elementos_no_cero()}")
```

### 11.4 Ejercicio: Registry pattern

```python
class Registry:
    def __init__(self):
        self._objetos = {}
    
    def registrar(self, clave, valor):
        if clave in self._objetos:
            raise ValueError(f"Ya existe: {clave}")
        self._objetos[clave] = valor
    
    def obtener(self, clave, default=None):
        return self._objetos.get(clave, default)
    
    def eliminar(self, clave):
        return self._objetos.pop(clave, None)
    
    def listar(self):
        return sorted(self._objetos.keys())
    
    def __contains__(self, clave):
        return clave in self._objetos
    
    def __len__(self):
        return len(self._objetos)


# Uso como registro de plugins
plugins = Registry()

def plugin_saludo(nombre):
    return f"Hola, {nombre}!"

def plugin_despedida(nombre):
    return f"Adios, {nombre}!"

plugins.registrar("saludo", plugin_saludo)
plugins.registrar("despedida", plugin_despedida)

print("plugin_saludo" in plugins)  # True
plugin = plugins.obtener("saludo")
print(plugin("Mundo"))  # Hola, Mundo!
print(plugins.listar())  # ['despedida', 'saludo']
```

---

## Resumen Rápido

```python
# CREAR
vacio = {}
vacio = dict()
datos = {"nombre": "Ana", "edad": 25}
datos = dict(nombre="Ana", edad=25)
datos = dict.fromkeys(["a", "b"], 0)

# ACCEDER
datos["nombre"]             # Genera KeyError si no existe
datos.get("nombre")         # Retorna None si no existe
datos.get("x", "default")   # Con valor por defecto

# MODIFICAR
datos["nombre"] = "Luis"    # Asignar
datos.update({"x": 1})      # Actualizar
datos.setdefault("y", 2)   # Obtener o crear

# ELIMINAR
del datos["nombre"]          # Por clave
datos.pop("nombre")          # Retorna valor eliminado
datos.popitem()              # Elimina ultimo (3.7+)
datos.clear()                # Vaciar

# ITERAR
for clave in datos: pass
for valor in datos.values(): pass
for clave, valor in datos.items(): pass

# COMPRENSION
{k: v for k, v in datos.items() if v > 0}

# METODOS CLAVE
datos.keys()    # Claves
datos.values()  # Valores
datos.items()   # Pares
datos.copy()    # Copia superficial
d1 | d2         # Union (3.9+)
```

---

## Consejos Finales

- [+] Usa `get(clave, default)` en lugar de `dic[clave]` para evitar KeyError
- [+] Usa `defaultdict` cuando necesites agrupar o contar automáticamente
- [+] Usa comprensión de diccionarios para transformaciones eficientes
- [+] Desde Python 3.7+, los diccionarios mantienen orden de inserción
- [!] Cuidado con `dict.fromkeys()`: el valor por defecto se comparte si es mutable
- [!] Para copiar diccionarios anidados, usa `copy.deepcopy()`
- [x] No uses diccionarios cuando necesites acceso secuencial (usa listas)
- [x] Las claves deben ser hashables; los valores pueden ser cualquier cosa

