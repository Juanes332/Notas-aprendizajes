---
id: 2025061701
type: permanent
tags:
  - coding
  - español
  - Console
---
### Un tris de historia

JavaScript fue creado por un dude llamado Brendan Eich en 10 days (el men se metio un monton de cafes xd)

Es un lenguaje interpretado, es basado en C y en Java ( aunque no tiene casi nada que ver con java mas que el nombre y algunas aspiraciones ).

JavaScript tiene diferentes versiones que se conocen como EcmaScript, cada nueva version tiene mejoras para el lenguaje. La version que se usa como referencia y que trajo grandes cambios fue EcmaScript6 (ya vamos en ecma 2026 porque pues Yolo :v).

### Lecturas de interes y utilles para aprender js

**Libro de Eloquent js:** https://eloquentjavascript.net/
**El tutorial de Js Moderno (page):** https://es.javascript.info/

---


### Los valores de entorno

Las funciones pueden producir valores, es decir es capaz de retornar un valor el cual podemos manipular y usar.

```js
console.log(Math.Max(2,4)) // Imprime el valor mas alto
console.log(Math.Min(2,4)) // Imprime el valor mas bajo
```


## Strings

```js
let greating = "Hola Juan";

let myName = "Juan";

// Metodos comunes


console.log(greating.toUpperCase());

console.log(greating.toLocaleLowerCase());

console.log(greating.indexOf("Hola"));

console.log(greating.indexOf("Juan"));

console.log(greating.includes("Hola"));

console.log(greating.includes("Juan"));

console.log(greating.includes("Jenhz"));

console.log(greating.slice(0, 8));

console.log(greating.replace("Juan", "Jenhz"));


// Template Literals (plantillas lliterales) 

let message = `Este es

un string

Literal`;

console.log(message);


// Interpolacion

console.log(`Hola ${myName}`);


// Ejercicios

console.log("\n");

console.log("==".repeat(50))

console.log("\n");

  
// 1. Concatena dos cadenas de texto

let chain1 = "Hola Buenas tardes";

let chain2 = "Bienvenid@ a nuestro portal web";

console.log(`${chain1}. ${chain2}`);

  
// 2. Muestra la longitud de una cadena de texto

let lengthChain = greating.length;

console.log(lengthChain);

  
// 3. Muestra el primer y utlimo caracter de un string

let firtsChar = greating.slice(0, 1);

let lastChar = greating.slice(8, 9);

console.log({ firtsChar, lastChar });

  
// 4. Convierte en mayusculas y minusculas un string

let upperChain = greating.toUpperCase();

let lowerChain = greating.toLowerCase();

console.log({ upperChain, lowerChain });


// 5. Crea una cadena de texto de varias lineas

let tooManyLines = `Quiero
ser
una
nube
xddddddddd
ddd
dd
d`

console.log({ tooManyLines });

  
// 6. Interpolael valor de una variable en un string

let killerGreating = `${greating}, hoy vas a dormir mucho...`;

console.log({ killerGreating });

  
// 7. Remplaza todos los espacios en blanco de un string con guiones

let whiteSpacesChain = "Que tal crack, maquina, fiera, titan, idolo, maestro";

console.log(whiteSpacesChain.replaceAll(" ", "-"));

  
// 8. Comprueba si na cadena de texto contiene una palabra concreta

let specWord = whiteSpacesChain;

console.log(specWord.includes("crack"));

  
// 9. Comprueba si dos strings son iguales

let equalString = (whiteSpacesChain == killerGreating);

console.log({ equalString });
 

// 10. comprueba si dos strings tienen la misma longitud

let equalLength = (whiteSpacesChain.length == killerGreating.length);

console.log({ equalLength });
```

