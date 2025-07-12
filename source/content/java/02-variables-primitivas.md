---
id: 02-variables
aliases: []
tags: []
---

# Variables primitivas

Una declaración de variable consta de dos partes:
1. El tipo de dato de la variable
2. El nombre de la variable

En cuanto a los tipos de variables, tenemos dos tipos:
- **Primitivas**: Contienen un sólo valor.
- **Referenciados**: Agrupan un conjunto de variables primitivas, referenciados, objetos, entre otros. Aquí irían los <u>arreglos</u>, <u>clases</u>, <u>interfaces</u>, etc.

---

# Tipos de datos primitivos
Comenzaremos con las primitivas, donde veremos los tipos que existen.

## Primitivos numéricos

### Enteros
- `byte`: Valores enteros pequeños de 1 byte de capacidad con valores desde -128 hasta 127.
- `short`: Valores enteros mas grandes que `byte` pero menores que `int`, con capacidad de representación desde el -32'768 a 32'767.
- `int`: Valores enteros de 4 bytes con valores desde -2³¹ hasta 2³¹ - 1.
- `long`: Valores enteros más grandes con valores desde -2⁶³ hasta 2⁶³ - 1.

> [!Note]
> Todos estos valores pueden ser expresados en otros sistemas numéricos como binario (anteponiendole `0b`), octal (anteponiendole `0`) o hexadecimal (anteponiendole `0x`).

### Decimales
- `float`: Números reales de 32 bits con rango desde −3.40282347×10³⁸ hasta 3.40282347×10³⁸. Las constantes literales con una `f` al final se tomarán como flotantes.
- `double`: Números reales de 64 bits con precisión significativamente mayor, el rango va de −1.7976931348623157×10³⁰⁸ hasta 1.7976931348623157×10³⁰⁸. Las constantes literales que terminan con `d` serán tomadas como decimales de doble precisión.

### Otros
- `boolean`: Valores booleanos `true` y `false`.
- `char`: Caracteres ASCII.

Los primitivos se pueden convertir usando casteo, es decir, a una variable anteponerle el tipo de dato al que queremos convertir entre paréntesis.
```java
// Conversión de primitivos

int n = 10000;
short s = (short)n;
```
