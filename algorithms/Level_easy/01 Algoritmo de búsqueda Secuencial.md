---
id: 2025080501
type: permanent
tags:
  - Algorithms
  - Programming
  - coding
  - español
  - Python
  - JavaScript
author: Juan Hernandez
---
El algoritmo de búsqueda secuencial es una método para encontrar un valor específico dentro de una lista o un Vector. Este proceso compara el valor que queremos con todos los de la lista hasta llegar al final, si lo encuentra retorna el resultado y si no indicamos que no se ha encontrado.

Este algoritmo se usa cuando la lista no está ordenada o no se puede ordenar de manera previa. Es un algoritmo simple e ineficiente, ya que en el peor que de los casos hay que recorrer la lista de todo el vector para saber si el valor objetivo está o no.

Diagrama

![[Pasted image 20250805095955.png]]

### Ejemplos en código

**Python**

```python
def busqueda_secuencial(lista, elemento):
	for i in range(len(lista)):
		if lista[i] == elemento:
			return 1
	return -1
```

Esta función realiza una búsqueda de una lista dada para encontrar un elemento específico
```python
mi_lista = [3, 6, 4, 12, 8, 19, 9]
elemento_buscado = 8

# Definición de la lista
resultado = busqueda_secuencial(mi_lista, elemento_buscado)

if resultado != -1:
	print(f"El elemento {elemento_buscado} se encuentra en la posición {resultado}")
else:
	print(f"El elemento {elemento_buscado} no se encuentra en la lista.")
```

**JavaScript**

```JavaScript
function busquedaSecuencial(arr, elemento){
	for(let i = 0; i < arr.length; i++){
		if(arr[i] === elemento){
			return i
		}
	}
	return -1
}
```

```JavaScript
let miArr = [3, 6, 4, 12, 8, 19, 9]
let elementoBuscado = 8

let resultado = busquedaSecuencial(miArr, elementoBuscado)
console.log(`El elemento ${elementoBuscado} se encuentra en el indice ${resultado}`)
```


