---
id: 2025080501
tags:
  - Algorithms
  - Programming
  - coding
  - español
  - Python
  - JavaScript
type: permanent
author: Juan Hernandez
---
El algoritmo de búsqueda de subcadena se utiliza para encontrar todas las ocurrencias de una subcadena dentro de una subcadena más grande. La idea principal del algoritmo es buscar las subcadena carácter por caracter en la cadena principal.

Estos son los pasos que realiza el algoritmo:

1. **Inicialización:** Comenzamos definiendo dos variables, una para la longitud de la cadena principal (n) y otra para la longitud de la subcadena que estamos buscando (m).
2. **Iteración:** Recorremos la cadena principal buscando la subcadena. Comenzamos en el primer carácter de la cadena principal y comparamos cada carácter con el primer carácter de la subcadena.
3. **Coincidencia parcial:** Si encontramos un carácter que coincide con el primer carácter de la subcadena, comparamos los siguientes caracteres de la subcadena con los caracteres correspondientes de la cadena principal.
4. **Coincidencia completa:** Si todos los caracteres de la subcadena coinciden con los caracteres correspondientes de la cadena principal, hemos encontrado una coincidencia completa. Registramos la posición de inicio de esta coincidencia en la cadena principal.
5. **Continuación de la búsqueda:** Continuamos buscando más ocurrencias de la subcadena moviéndonos al siguiente carácter en la cadena principal y repitiendo los pasos 3 y 4 hasta que hayamos buscado en toda la cadena principal
6. **Resultado:** Una vez hemos buscando en toda la cadena principal, hemos encontrado todas las ocurrencias de la subcadena.
   Devolvemos las posiciones de inicio de cada ocurrencia encontrada

Este algoritmo es eficiente y puede ser implementado de diversas formas, como utilizando el enfoque de fuerza bruta, enfoque de búsqueda rápida o algoritmo de Knuth-Morris-Pratt (KMP), que es una mejora del método de fuerza bruta. Cada enfoque tiene sus propias ventajas y desventajas en términos de tiempo de ejecución y uso de memoria.

Diagrama
