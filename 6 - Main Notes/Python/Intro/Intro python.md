
2026-01-02 21:23

status: #baby

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

## References

https://www.geeksforgeeks.org/python/python-data-types/


