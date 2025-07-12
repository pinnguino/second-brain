---
id: 05-flujos-de-control
aliases: []
tags: []
---

# Flujos de control condicionales

### Sentencia `if`
...

### Sentencia `switch`
...

---

# Flujos de control repetitivos

### Ciclo `for`
...

### Ciclo `while` y `do while`
...

### Ciclo `foreach`
Son una variación del ciclo `for` todoterreno, en donde este es más declarativo y fácil de leer.
Su principal uso es con arreglos o colecciones de Java, y nos permiten realizar ciertas acciones con todos y cada uno de los objetos de esa colección. Su uso es recomendado cuando no se necesita tener control sobre los índices cuando el bucle está siendo ejecutado, cuando la colección es del tipo `Set` o similares, o cuando se necesiten realizar acciones sobre todos los objetos de la colección.

```java
int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // Colección de datos

for(int num : arr) { // A cada elemento perteneciente a arr lo abstraemos como num
    num++; // Se incrementa en 1
}

// arr[] = {2, 3, 4, 5, 6, 7, 8, 9, 10, 11}
```

---
# Sentencias de escape

### `break`
...

### `continue`
...


