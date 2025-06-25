---
id: 2025061001
type: permanent
tags:
  - C
  - coding
  - Console
  - Linux
  - español
---
---

Introduccion a C

Para correr codigo C debemos tener instalado gcc en nuestro sistema linux (si estas en windows tienes que seguir esta documentacion: https://code.visualstudio.com/docs/languages/cpp .  O correrlo con visual studio)

## Semana 1

### Como compilar?


```bash
gcc program.c -o program.out
./program.out
```


---

**Lecturas**
https://www.cc4e.com/book/chap01.md

### Primeros pasos:

De esta forma realizamos un hola mundo:
```C
#include <stdio.h> /* importamos libreria de C */

int main()
{
	printf("Hello, World\n")
	
	return 0;
}

```

Formas de imprimir
```C
#include <stdio.h>


int main()

{

  printf("Hello world\n");

  printf("Answer %d\n", 16); /* %d es para digitos*/

  printf("Name %s\n", "Sarah"); /* %s es para cadenas de texto*/

  return 0;

}
```


Usando ints y & para asignar valor a variable
```C
#include <stdio.h>

int main()

{

    int usf, euf; /* Inicializar variables*/

    printf("Enter US Floor\n");

    scanf("%d", &usf); /* & se usa para asignar valora la variable*/

    euf = usf - 1;

    printf("EU Floor %d\n", euf);

}
```


Input / output
```C
#include <stdio.h>

int main()

{

    char name[1000]; /* Colocar un limite */

    printf("Enter name\n");

    scanf("%100s", name); /* 100%s char limit*/

    printf("Hello %s\n", name);

}
```

Leyendo lineas
```C
#include <stdio.h>

int main()

{

    char line[1000];

    printf("Enter line\n");
    scanf("%1000[^\n]", line); /* lee todos los caracteres EXCEPTO el salto de línea - Lee hasta 999 caracteres o hasta encontrar \n */

    printf("Line: %s\n", line);

}
```

Leyendo lineas con fget()
```C
#include <stdio.h>

int main()

{
    char line[1000];

    printf("Enter line\n");

    fgets(line, sizeof(line), stdin); /* lee todos los caracteres EXCEPTO el salto de línea - Lee hasta 999 caracteres o hasta encontrar \n  */

    printf("Line: %s\n", line);

}
```

---


## Semana 2

Recursos adicionales:

[Chapter 1 Textbook](https://www.cc4e.com/book/chap01.md)

- [A podcast of the entire book](https://audio.cc4e.com/)
    

- [An audio reading of Chapter 1](https://youtu.be/rPwBgsSnxCw "An audio reading of Chapter 1")
    

- [Endianness Explained With an Egg - Computerphile](https://www.youtube.com/watch?v=NcaiHcBvDR4 "External YouTube Link")
    

- [Arrays vs Linked Lists - Computerphile](https://www.youtube.com/watch?v=DyG9S9nAlUM "External Youtube Link")
    

- [Sample source code](https://www.cc4e.com/code "External web link to Dr. Chuck's personal website")
    

- [Audio version of the book](https://audio.cc4e.com/ "External link to the audio version of the book")


## Arrrays vs Linked Lists

## Arrays (Arreglos)

Los arrays almacenan elementos en posiciones de memoria **consecutivas**. Es como tener una fila de casillas numeradas donde cada elemento tiene una dirección específica.

**Ventajas:**

- **Acceso rápido**: Puedes acceder a cualquier elemento directamente usando su índice en tiempo O(1)
- **Eficiencia de memoria**: Solo almacenan los datos, sin información adicional
- **Cache-friendly**: Como los datos están juntos en memoria, el procesador puede cargarlos más eficientemente

**Desventajas:**

- **Inserción/eliminación costosa**: Para insertar en el medio necesitas mover todos los elementos posteriores O(n)
- **Tamaño fijo**: En muchos lenguajes no puedes cambiar el tamaño después de crearlo
- **Desperdicio de memoria**: Si reservas más espacio del necesario

## Linked Lists (Listas Enlazadas)

Las linked lists almacenan elementos en **nodos** dispersos por la memoria. Cada nodo contiene el dato y un puntero al siguiente nodo.

**Ventajas:**

- **Inserción/eliminación eficiente**: Agregar o quitar elementos al inicio es O(1)
- **Tamaño dinámico**: Puedes crecer o reducir la lista según necesites
- **Flexibilidad**: Fácil reorganización de elementos

**Desventajas:**

- **Acceso secuencial**: Para llegar al elemento 100 tienes que pasar por los 99 anteriores O(n)
- **Memoria extra**: Cada nodo necesita espacio adicional para el puntero
- **Cache-unfriendly**: Los nodos pueden estar dispersos en memoria

## Ejemplo práctico

Imagínate que tienes una playlist de música:

**Array**: Como un CD donde las canciones están en orden fijo. Puedes saltar directamente a la canción 5, pero cambiar el orden requiere regrabar todo el disco.

**Linked List**: Como una cadena de notas donde cada nota dice "ahora toca esta canción, luego ve a aquella nota". Fácil reorganizar, pero para llegar a la canción 5 tienes que seguir la cadena desde el inicio.

## ¿Cuándo usar cada uno?

- **Arrays**: Cuando necesitas acceso frecuente por índice, operaciones matemáticas, o cuando el tamaño es relativamente estable
- **Linked Lists**: Cuando haces muchas inserciones/eliminaciones, el tamaño varía mucho, o no sabes cuántos elementos tendrás

---


## Part 2: From Python to C - The Rosetta Stone Lecture