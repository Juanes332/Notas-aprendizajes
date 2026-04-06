
2026-01-02 21:23

status: #Adult 

tags: #Python #coding 


# Intro Python 

Python es un lenguaje de programación robusto, de tipado débil y con implementación en varios campos de las ciencias de la computación.

Un programa en python normalmente debe tener esta estructura:

```python
#!/usr/bin/python3 --> shebang

def main():
	"""
	Documentacion de la funcion
	"""
	
	sum: int = 2 + 2
	
if __name__ == "__main__":
	main()
```

### Tipos de datos

```python
#!/usr/bin/python3

def main():
	"""
	Data types
	
	Numeric: - integers - float - complex number
	Mapping type: dict
	Boolean: bool
	Set Type: set - frozenset
	Sequence Type: - String - List - Tuple
	Binary Types: bytes - bytearray - memmoryview
	"""
	
	# Numeric
	sum_int: int = 2 + 2 # int
	rest_float: float = 12.9 - 31.9 # float
	# Calculate impedancy
	z1: complex = 10 + 5j # impedancy in Ohmios
	z2: complex = 5 - 2j
	total_impedancy: complex = z1 + z2
	print(z1.real) # view real part of the complex number
	print(z1.imag) # view imag part of the complex number
	
	
	# Mapping type
	data: dict = {
		"name": "Finn",
		"age": 34
	}
	
	
	# Boolean
	is_alive: bool = True
	is_sleeping: bool = False
	
	
	# Set
	number_set: set = { 1, 2 ,3 ,4 }
	set_array: set = set([1, 2, 3, 4, 5])
	
	# Frozen set
	number_frozenset: frozenset = frozenset([1,2,3,4,5])
	list_frozen: list  = ["papa", "arroz", "pollo"]
	list_frozenset: frozenset = frozenset(list_frozen)
	
	
	# Sequence Type
	name: str = "pedro"
	fruit_list: list = ["Banana", "Sandia", "Uvas"]
	low_level: tuple = ("C", "Rust", "Assembler")
	
	
	# Binary Types
	message_bytes: str = "Welcome to Hell"
	b: bytes = bytes(message_bytes, 'utf-8')
	
	# Bytearray
	ba = bytearray("hola")
	
	# Memory View
	data = b'Python'
	mv = memoryview(data)
	
	
	
if __name__ == "__main__":
	main()
```


### Funciones Clave del Intérprete de Python:

- **Ejecución de Código**: El intérprete ejecuta el código escrito en Python línea por línea, lo que facilita la depuración y permite a los desarrolladores probar fragmentos de código de forma interactiva.  

- **Modo Interactivo**: El intérprete puede usarse en un modo interactivo que permite a los usuarios ejecutar comandos de Python uno a uno y ver los resultados de inmediato, lo cual es excelente para el aprendizaje y la experimentación.  

- **Modo de Script**: Además del modo interactivo, el intérprete puede ejecutar programas completos o scripts que se escriben en archivos con la extensión '**.py**'.  

- **Compilación a Bytecode**: Aunque Python es un lenguaje interpretado, internamente, el intérprete compila el código a bytecode antes de ejecutarlo, lo que mejora el rendimiento.


### Control de flujo

```python
   1 │ #!/usr/bin/python3
   2 │ 
   3 │ # for
   4 │ 
   5 │ for i in range(5):
   6 │     print(i)
   7 │ 
   8 │ animes: list = ["Boku no hero", "Naruto", "One Piece"]
   9 │ 
  10 │ for anime_name in animes:
  11 │     print(anime_name)
  12 │ 
  13 │ # while
  14 │ counter: int = 0  # variable contador
  15 │ 
  16 │ while i < 10:
  17 │     print(i)
  18 │     i = i + 1
  19 │ 
  20 │ characters: list = ["Luffy", "Zoro", "Sanji"]
  21 │ 
  22 │ for i, character in enumerate(characters):
  23 │     """
  24 │     enumerate returns a tuple with an index and value
  25 │     """
  26 │     print(f"Character [{i + 1}]: {character}")
  27 │ 
  28 │ 
  29 │ # Diccionarios con bucles
  30 │ frutas: dict = {
  31 │     "manzana": 1,
  32 │     "peras": 4,
  33 │     "bananas": 7,
  34 │     "sandias": 10,
  35 │ }
  36 │ 
  37 │ for fruta, value in frutas.items():
  38 │     print(f"Hay {value} fruta {fruta}")
  39 │ 
  40 │ 
  41 │ # Bucles anidados
  42 │ my_list: list = [[1, 4, 5], [2, 6, 8], [9, 4, 1]]
  43 │ 
  44 │ for element in my_list:
  45 │     print(f"\nVamos a desglosar la lista {element}\n")
  46 │     for element_in_list in element:
  47 │         print(element_in_list)
  48 │ 
  49 │ 
  50 │ # Listas de comprension (for)
  51 │ odd_list = [1, 3, 5, 7, 9]
  52 │ cuadrado: list = [i ** 2 for i in odd_list]
  53 │ print(cuadrado)
  54 │ 
  55 │ 
  56 │ # Salir antes de tiempo de un bucle -> break
  57 │ for i in range(10):
  58 │     print(i)
  59 │     if i == 5:
  60 │         print("\nsaliendo del bucke...\n\n")
  61 │         break
  62 │ 
  63 │ # continue
  64 │ for i in range(10):
  65 │     if i == 5:
  66 │         continue
  67 │     print(i)
  68 │ 
  69 │ print("\n\n")
  70 │ 
  71 │ # Condicionales
  72 │ 
  73 │ age: int = 22
  74 │ 
  75 │ if age >= 18:
  76 │     print("Eres mayor de edad")
  77 │ else:
  78 │     print("Eres menor de edad")
  79 │ 
  80 │ if 1 * True:
  81 │     print("Esta maddre sirve")
  82 │ 
  83 │ # Operadores ternarios
  84 │ age_nino: int = 20
  85 │ mensaje = "Eres mayor de edad" if age_nino >= 18 else "Eres menor de edad"
  86 │ print(f"ternario:{mensaje}")
  87 │ 
  88 │ # Operadores logicos --> and, or
  89 │ the_logic_list: list = [1, 5, 3, 7, 0, 19, 23]
  90 │ 
  91 │ if 5 in the_logic_list and 23 in the_logic_list:
  92 │     print("Estan los valores en la lista")
  93 │ else:
  94 │     print("NO estan los valores en la lista")
  95 │ 
  96 │ if 2 in the_logic_list or 7 in the_logic_list:
  97 │     print("Esta el valor en la lista")
  98 │ else:
  99 │     print("No estan los valores de la lista")
 100 │ 
 101 │ crazy_level = 5
 102 │ level = 5
 103 │ 
 104 │ if level != crazy_level:
 105 │     print("Aun te falta level")
 106 │ 
 107 │ 
 108 │ # Programa peque que detecta numeros impares
 109 │ 
 110 │ nums: int = [2, 4, 6, 8, 10, 7, 12, 14]
 111 │ pares: bool = True
 112 │ 
 113 │ for num in nums:
 114 │     if num % 2 != 0:
 115 │         pares = False
 116 │         break
 117 │ 
 118 │ if pares:
 119 │     print("\nTodos son pares")
 120 │ else:
 121 │     print("\nAlgun numero es impar")
 122 │ 
 123 │ 
 124 │ print("\n\n")
 125 │ # Fizz buzz forma optima
 126 │ for i in range(1, 101):
 127 │     print("Fizz" * (i % 3 == 0) + "Buzz" * (i % 5 == 0) or i)
```

### Functions

```python
   1 │ #!/usr/bin/python3
   2 │ 
   3 │ # funciones y ambito de variables -> scope
   4 │ 
   5 │ def saludo(user: str) -> str:
   6 │     print(f"Hola {user}")
   7 │     return "Hola {user}"
   8 │ 
   9 │ 
  10 │ saludo("Manolo")
  11 │ 
  12 │ 
  13 │ variable_global = "variable global"
  14 │ 
  15 │ 
  16 │ def main():
  17 │     """
  18 │     Las variables dentro de las variables solo
  19 │     existen dentro de al funcion, fuera no.
  20 │     """
  21 │     variable_local: str = "variable local"
  22 │     print(f"Podemos llamar dentro a de la funcion a {
  23 │           variable_global} y a {variable_local}")
  24 │ 
  25 │ 
  26 │ main()
```


### Lambda functions

```python
   1 │ #!/usr/bin/python3
   2 │ 
   3 │ from functools import reduce
   4 │ 
   5 │ # cuadrado = lamnda x: x ** 2 -> MAL
   6 │ 
   7 │ # suma = lambda x, y: x + y
   8 │ # suma(5, 5)
   9 │ 
  10 │ 
  11 │ numeros_list: list = [1, 2, 3, 4, 5]
  12 │ cuadrados = tuple(map(lambda x: x ** 2, numeros_list))
  13 │ print(cuadrados)
  14 │ 
  15 │ numeros_pares = tuple(filter(lambda x: x % 2 == 0, numeros_list))
  16 │ print(numeros_pares)
  17 │ 
  18 │ numeros_impares = tuple(filter(lambda x: x % 2 != 0, numeros_list))
  19 │ print(numeros_impares)
  20 │ 
  21 │ multiplicar_lista = reduce(lambda x, y: x * y, numeros_list)
  22 │ print(multiplicar_lista)
```


### Operaciones

```python
   1 │ #!/usr/bin/python3
   2 │ 
   3 │ num1: int = 6
   4 │ num2: int = 3
   5 │ 
   6 │ 
   7 │ def suma(num1: int, num2: int) -> int:
   8 │     result: int = num1 + num2
   9 │     return result
  10 │ 
  11 │ 
  12 │ def substract(num1: int, num2: int) -> int:
  13 │     result: int = num1 + num2
  14 │     return result
  15 │ 
  16 │ 
  17 │ def multiplication(num1: int, num2: int) -> int:
  18 │     result: int = num1 * num2
  19 │     return result
  20 │ 
  21 │ 
  22 │ def division(num1: int, num2: int) -> float:
  23 │     result: float = round(num1 / num2, 2)
  24 │     return result
  25 │ 
  26 │ 
  27 │ def exponential(num1: int, num2: int) -> str:
  28 │     result: int = num1 ** num2
  29 │     operation_formating: str = "{:,}".format(result).replace(",", ".")
  30 │     print(type(operation_formating))
  31 │     return operation_formating
  32 │ 
  33 │ 
  34 │ def operator_module(num1: int, num2: int) -> int:
  35 │     result: int = num1 % num2
  36 │     return result
  37 │ 
  38 │ 
  39 │ def strings_operations(text: str) -> str:
  40 │     result: str = text[0:3] * 5
  41 │     print(result)
  42 │ 
  43 │ 
  44 │ def sum_list(list1: list, list2: list) -> list:
  45 │     result: list = list(map(sum, zip(list1, list2)))
  46 │     print(result)
  47 │ 
  48 │ 
  49 │ def main():
  50 │     print(division(4, 2))
  51 │     print(exponential(45, 5))
  52 │     print(operator_module(12, 65))
  53 │     print(strings_operations("python"))
  54 │     print(sum_list([1, 4, 6, 4, 2], [3, 6, 67, 6, 9]))
  55 │ 
  56 │ 
  57 │ if __name__ == "__main__":
  58 │     main()
```


### Manejo de errores

```python
   1 │ #!/usr/bin/python3
   2 │ 
   3 │ from pwn import log
   4 │ 
   5 │ try:
   6 │     num = 5/2
   7 │     print(num)
   8 │ except ZeroDivisionError:
   9 │     log.failure("que haces my man")
  10 │ except TypeError:
  11 │     log.failure("Las operaciones math solo admiten nums")
  12 │ finally:
  13 │     log.info_once("Esto siempre se va a ejecutar")
  14 │ 
  15 │ 
  16 │ x = -5
  17 │ 
  18 │ if x < 0:
  19 │     raise Exception("[-] No se pueden usar numeros negativos")
```


## References

https://www.geeksforgeeks.org/python/python-data-types/


