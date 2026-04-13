
### Bits y Bytes

1. Estados, bits y bombillas (o LEDs)

Imagina un sistema físico con **3 bombillas**. Cada bombilla puede estar **encendida (1)** o **apagada (0)**.  

Esto significa que cada bombilla representa **1 bit de información**.

Por lo tanto, con 3 bombillas tienes 8 estados:

|Estado decimal|Binario|Bombilla 1|Bombilla 2|Bombilla 3|
|---|---|---|---|---|
|0|000|Apagada|Apagada|Apagada|
|1|001|Apagada|Apagada|Encendida|
|2|010|Apagada|Encendida|Apagada|
|3|011|Apagada|Encendida|Encendida|
|4|100|Encendida|Apagada|Apagada|
|5|101|Encendida|Apagada|Encendida|
|6|110|Encendida|Encendida|Apagada|
|7|111|Encendida|Encendida|Encendida|

### Cuanta matematica discreta cabe en un ser humano :v?

### 1. Cada dedo = 1 bit

Tenemos **5 dedos en una mano**.  
Cada dedo puede estar en uno de dos estados:

- Dedo abajo → 0    
- Dedo arriba → 1


Como empezamos en 0, se puede contar del **0 al 31**.

---

### 2. Asignar pesos binarios a los dedos

Pensamos en la mano como un número binario de 5 bits.  
Cada dedo representa una potencia de 2:

| Dedo    | Peso binario |
| ------- | ------------ |
| Pulgar  | 1            |
| Índice  | 2            |
| Medio   | 4            |
| Anular  | 8            |
| Meñique | 16           |

---

## Binary to decimal

![[Pasted image 20251025233832.png]]

Calcular permisos en linux
![[Pasted image 20251025233947.png]]

Tabla ascii
![[Pasted image 20251025234126.png]]

## RGB: Cómo se representa el color

**RGB color model** (Red, Green, Blue) es un modelo aditivo de color.  
Cada color en pantalla se forma combinando **intensidades de rojo, verde y azul**.
### Componentes

Cada componente (R, G y B) se representa con **8 bits** → valores de **0 a 255**:

- R (Red): 0–255
- G (Green): 0–255
- B (Blue): 0–255

Un color completo es una **tupla** de 3 valores:

Color=(R,G,B)\text{Color} = (R, G, B)Color=(R,G,B)

Ejemplos:

- Rojo puro: (255, 0, 0) → `11111111 00000000 00000000`
- Verde puro: (0, 255, 0)
- Azul puro: (0, 0, 255)
- Blanco: (255, 255, 255)
- Negro: (0, 0, 0)
- Gris medio: (128, 128, 128)

---

### Hexadecimal RGB

También se suele representar el color en **hexadecimal**:  
Cada componente de 8 bits se convierte a **dos dígitos hex** (de 00 a FF).

Por ejemplo:

- Rojo (255, 0, 0) → `#FF0000`
- Verde (0, 255, 0) → `#00FF00`
- Azul (0, 0, 255) → `#0000FF`
- Blanco (255, 255, 255) → `#FFFFFF`
- Negro (0, 0, 0) → `#000000`

Esto se debe a que:

- 1 byte = 8 bits
- 8 bits = 2 dígitos hexadecimales
- 3 bytes (R, G, B) = 6 dígitos hexadecimales

---

### Cómo “funciona” físicamente

Las pantallas (LED, LCD, OLED, etc.) tienen **subpíxeles rojos, verdes y azules**.  
Cuando enciendes cada subpíxel con una cierta intensidad, tu ojo percibe la **mezcla aditiva** de esos tres colores.

Por ejemplo:

- Si enciendes rojo + verde = amarillo.
- Verde + azul = cian.
- Rojo + azul = magenta.
- Los tres a tope = blanco.

---

## 3. Relación entre ASCII y RGB

Aunque parecen mundos distintos, ambos se basan en la **representación binaria**:

- ASCII usa **un byte** para representar un carácter.
- RGB usa **tres bytes** para representar un color.

En memoria, no hay diferencia: son solo números binarios.  
La diferencia está en **cómo se interpretan** esos números.

Ejemplo:
```txt
Letra 'A' → 01000001
Rojo (255,0,0) → 11111111 00000000 00000000
```

- **ASCII**: mapa de 0–127 que asigna números a caracteres (usa 7 bits).
- **RGB**: tripleta de valores de 0–255 (usa 3×8 bits = 24 bits por píxel).
- Ambos sistemas son formas distintas de dar **significado** a secuencias binarias.
- 1 byte = 8 bits → 256 posibles valores → base de todo esto.
