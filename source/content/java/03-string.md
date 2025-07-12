---
id: 03-string
aliases: []
tags: []
---

# El tipo `String`

El tipo `String` almacena cadenas de caraceres con tamaño dinámico.
Hay dos maneras válidas de crear un `String` en nuestro programa:

### De forma literal

```java
String texto = "Mensaje";
```

### Con el operador `new()`

```java
String texto = new String("Mensaje");
```

> [!Note]
> Así como los datos primitivos, el `String` puede ser instanciado de forma literal, debido a su uso masivo. Se recomienda la forma literal, ya que es más cómoda.

---

# Inmutabilidad

> [!Important]
> El objeto `String` tiene la característica de ser inmutable.

### ¿Qué es la inmutabilidad en un objeto?
***Si un objeto no puede cambiar su estado después de su creación, entonces el objeto es inmutable***. La imposibilidad de cambiar el estado significa que las variables miembro en el objeto no se pueden cambiar, incluido el valor del tipo de datos básico no se puede cambiar, la variable del tipo de referencia no puede apuntar a otros objetos y el estado del objeto apuntado por el tipo de referencia no se puede cambiar.
Esto significa que si tenemos una variable `String` que tiene como valor `"Texto"` y luego cambiamos su valor, lo que se hace es cambiar la referencia de la variable hacia un objeto nuevo con el valor de la cadena cambiado.

---

## Concatenar con `+` VS `concat()` VS `StringBuilder`

### Concatenar con `+`

```java
String a = "Mate";
String b = "Amargo";

String c = a + b; // c = "MateAmargo"
```

### Concatenar con el método `concat()`

```java
String a = "Mate";
String b = "Amargo";

String c = a.concat(b); // c = "MateAmargo"
```

### Usando `StringBuilder`

```java
String a = "Mate";
String b = "Amargo";

StringBuilder sb = new StringBuilder(a);
sb.append(b);
String c = sb.toString();
```

> [!Note]
> El objeto `StringBuilder` nos permite, como dice su nombre, **crear un `String`**, agregándole cadenas, y luego generar el `String` al final, de forma eficiente.

En orden de eficiencia, `StringBuilder` no tiene comparación con los demás métodos. Se recomienda usar éste cuando el rendimiento sea algo crítico y se estén concatenando muchas veces, como por ejemplo en bucles largos, o hayan muchas llamadas a la concatenación.

---

# Métodos importantes de la clase `String`

- `length()`: Longitud de la cadena.
- `toUpperCase()/toLowerCase()`: Convierte la cadena a mayúsculas/minúsculas.
- `equals("aString")`: Devuelve `true` si la cadena pasada como parámetro es igual (caracter a caracter) a la cadena que llama el método. 
- `equalsIgnoreCase("astring")`: Compara igualmente las cadenas como en `equals()`, pero sin contar mayúsculas ni minúsculas.
- `compareTo("anotherString")`: Compara lexicográficamente las cadenas. El resultado es un entero negativo si este objeto `String` precede lexicográficamente a la cadena del argumento. El resultado es un entero positivo si este objeto String sigue lexicográficamente a la cadena del argumento. El resultado es `0` si las cadenas son iguales. 
- `charAt(int index)`: Retorna el caracter de la cadena posicionado en la posición `index`.
- `substring(int beginIndex, int endIndex)`: Retorna una subcadena del `String` receptor. Puede llamarse indicando sólamente `beginIndex`, y retornará la subcadena desde `beginIndex` hasta el final.
- `replace(char oldChar, char newChar)`: Reemplaza todas las ocurrencias de `oldChar` por `newChar`.

Entre otros.


