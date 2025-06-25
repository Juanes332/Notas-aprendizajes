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

Introducción a C

Para correr código C debemos tener instalado gcc en nuestro sistema linux (si estas en windows tienes que seguir esta documentación: https://code.visualstudio.com/docs/languages/cpp .  O correrlo con visual studio)

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


## Part 2: Cositas basicas uwu

**Variables**

```C
#include <stdio.h>
#include <stdbool.h> // for work with booleans

int main()
{
  // variable = A reusable container for a value
  //            Behaves as if it were value it conains

  int age = 24;
  int year = 2025;
  int quantity = 4;

  printf("You are %d years old\n", age);
  printf("This year is %d\n", year);
  printf("They are %d bottles of water xddddd\n", quantity);

  // Floats
  
  float price = 13.1342;
  float game_score = 65.2;
  float celsius = 35.4;

  printf("The price of thi product is %.4f\n", price);
  printf("Your score is %.1f\n", game_score);
  printf("The temperature is %.1f\n", celsius);

  // doubles

  double pi = 3.14158265358979;

  printf("The value of pi is %.15ls", pi); // ls means long floating number


  // char
  
  char grade = 'F';
  char symbol = '!';
  char currnecy = '$';

  printf("Your grade is %c\n", grade);
  printf("Your favorite symbol is %c\n");
  printf("Your currency is %c\n", currency);


  // Los strings en C no existen, existen los arreglos de caracteres
  
  char favorite_game[] = "Persona 5";
  printf("My favorite game is %s\n", favorite_game);


  // booleans -> true = 1, false = 0

  bool is_online = true;

  printf("%d\n", is_online);// 1

  // Basic example with booleans

  if (is_online) {
    printf("You're ONLINE\n");
  }
  else {
    printf("You're offline\n");
  }

  // int = whole numbers (4 bytes in modern systems)
  // float = single-precision decimal numbrer (4 bytes)
  // double = double-precision decimal number (8 bytes)
  // char = single character (1 byte)
  // char[] = array of characters (size varies and is basically the strings of C)
  // bool = true or false (1 byte, requires <stdbool.h>)

  return 0;
}
```

**Format specifier**

```C
#include <stdio.h>

int main()
{

  // Format specifier = Special tokens that begin with a %
, // symbol, followed by a character that specifies the 
  // data type and optional modifiers (width, precision, flags).
  // They control how darta is displayed or interpreted 

  int age = 21;
  float price = 20.99;
  double pi = 3.14158265358979; 
  char currency = '$';
  char name_game[] = "TOTKT";

  printf("%d\n", age);
  printf("%f\n", price);
  printf("%lf\n", pi);
  printf("%c\n", currency);
  printf("%s\n", name_game);

  // width

  int num1 = 5;
  int num2 = 45;
  int num3 = -433;

  printf("%5d\n");
  printf("%0d\n");
  printf("%+d\n");

  // precision

  float price1 = 19.99;
  float price2 = 1.50;
  float price3 = -100.00;

  printf("%.1f\n", price1); // digitos despues del punto flotante
  printf("%.2f\n", price2);
  printf("%.2f\n", price3);

  return 0;
}

```

**Arithmetic Operators**

```C
#include <stdio.h>

int main()
{

  // arithmetic operators = + - * / % ++ --

  int x = 10;
  int y = 3;

  printf("%d\n", x + y);
  printf("%d\n", x - y);
  printf("%d\n", x * y);
  printf("%d\n", x / y);
  printf("%d\n", x % y);
  printf("%d\n", x ++ y);
  printf("%d\n", x -- y);

  // x = x + 2; its the same for x+=2;
  // x-=2;
  // x*=2;
  // x%=2;

  return 0
}

```

**User input**

```C
#include <stdio.h>

int main(){
  
  int age = 0;
  float salary = 0.0f;
  char currency = '\0';
  char full_name[30] = "";

  printf("Enter your age: ");
  scanf("%d", &age);

  printf("Enter your salary: ");
  scanf("%f", &salary);

  printf("Enter your currency: ");
  scanf(" %c", &currency); // el espacio de %c es para realizar a limpieza del buffer

  printf("Enter your full name: ");
  fgets(full_name, sizeof(full_name), stdin);

  printf("%d\n", age);
  printf("%f\n", salary);
  printf("%c\n", currency);
  printf("%s\n", full_name);


  return 0;
}

```




