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

### Creando una clase

```python
# Class point
class Point:
	# constructor
	def __init__(self, x, y):
	self.x = x
	self.y = y

# class instance
point1 = Point(10, 20) 

type(point1)
# __main__.Point -> Custom type created. The __main__ is the name of script file were we are coding.
```

### Que es Self?
En Python, `self` ==es una referencia a la instancia actual de una clase==. Se utiliza para acceder a los atributos y métodos de la instancia dentro de los métodos de la clase. Es una convención en Python, y aunque se puede usar otro nombre, se recomienda seguir esta convención para la legibilidad del código. Esta palabra se usa como estandar para crear el constructor, pude ser cualquiera pero vamos a ver a muchos desarrolladores usando self (ya hace parte del PEP 8)