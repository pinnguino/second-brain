---
id: 01-hola-mundo
aliases: []
tags: []
---

# Tu primer Hola Mundo!

Comencemos haciendo tu primer programa en Java.
Para hacerlo, crea un nuevo proyecto en tu IDE de preferencia, y luego <u>crea una clase</u> nueva, la llamaremos **HolaMundo**.
En Java, el 99% de las cosas son objetos, por lo que haremos nuestra clase y nuestro método principal.

```java
public class HolaMundo {
    ...
}
```

<u>El punto de entrada para un programa de Java es el método `main`</u>, que se define como un método público, estático (de clase), y sin retorno (`void`).

```java
public class HolaMundo {
    public static void main(String[] args){
        System.out.println("Hola mundo!");
    }
}
```

Este código muestra por salida estándar el mensaje puesto como argumento.

Ya has hecho tu primer programa de Java!

[Siguiente: Variables](02-variables.md)
