---
id: 07-colecciones
aliases: []
tags: []
---

# Colecciones
Una colección representa un **grupo de objetos**. Estos objetos son conocidos como *elementos*. Cuando queremos trabajar con un conjunto de elementos, necesitamos un almacén donde poder guardarlos. En Java, se emplea la interfaz genérica `Collection` para este propósito. Gracias a esta interfaz, podemos almacenar cualquier tipo de objeto y podemos usar una serie de métodos comunes, como pueden ser: añadir, eliminar, obtener el tamaño de la colección, entre otros. Partiendo de la interfaz genérica `Collection` extienden otra serie de interfaces genéricas. Estas subinterfaces aportan distintas funcionalidades sobre la interfaz anterior.
En estas colecciones **el tamaño crece de forma dinámica** a medida que el volumen de los datos varía a través del tiempo, por lo que <u>el manejo del espacio no es obligatorio</u> como con los arreglos estáticos, por ejemplo.

# Tipos de colecciones
Veremos a continuación los tipos de colecciones junto con sus especificaciones y las particularidades de cada una.

## Set
La interfaz `Set` define una colección que ***no puede contener elementos duplicados***. Esta interfaz contiene, únicamente, los métodos heredados de `Collection` añadiendo la restricción de que ***los elementos duplicados están prohibidos***. Es importante destacar que, para comprobar si los elementos son elementos duplicados o no lo son, es necesario que dichos elementos tengan implementada de forma correcta los métodos `equals()` y `hashCode()`. Para comprobar si dos `Set` son iguales, se comprobarán si todos los elementos que los componen son iguales sin importar en el orden que ocupen dichos elementos.

### HashSet
Es la *implementación con mejor rendimiento* de todas, pero <u>no garantiza ningún orden</u> a la hora de realizar iteraciones. Es la implementación más empleada debido a su rendimiento y a que generalmente no nos importa el orden que ocupen los elementos. Esta implementación proporciona tiempos constantes en las operaciones básicas siempre y cuando la función hash disperse de forma correcta los elementos dentro de la tabla.

### TreeSet
Esta implementación **almacena los elementos ordenándolos** en función de sus valores. Es bastante <u>más lento que HashSet</u>. Los elementos almacenados deben implementar la interfaz Comparable. Esta implementación garantiza un rendimiento de `log(N)` en las operaciones básicas, debido a la estructura de árbol empleada para almacenar los elementos.
### LinkedHashSet
Esta implementación almacena los elementos en función del orden de inserción. Es, simplemente, un poco más costosa que `HashSet`.


## List
La interfaz `List` define una sucesión de elementos. A diferencia de la interfaz `Set`, la interfaz `List` sí <u>admite elementos duplicados</u>. A parte de los métodos heredados de `Collection`, añade métodos que permiten mejorar los siguientes puntos:

- **Acceso posicional**: manipula elementos en función de su posición en la lista.
- **Búsqueda de elementos**: busca un elemento concreto de la lista y devuelve su posición.
- **Iteración sobre elementos**: mejora el `Iterator` por defecto.
- **Rango de operación**: permite realizar ciertas operaciones sobre ragos de elementos dentro de la propia lista.

Dentro de la interfaz `List` existen varios tipos de implementaciones realizadas dentro de Java. Vamos a analizar cada una de ellas:

### ArrayList
Esta es la implementación típica. Se basa en un array redimensionable que aumenta su tamaño según crece la colección de elementos. Es la que mejor rendimiento tiene sobre la mayoría de situaciones.

### LinkedList
Esta implementación permite que mejore el rendimiento en ciertas ocasiones. Esta implementación se basa en una lista doblemente enlazada de los elementos, teniendo cada uno de los elementos un puntero al anterior y al siguiente elemento. Posee mejor rendimiento para insertar elementos en posiciones que no son el final, comparándola con el `ArrayList`.

> [!Warning]
> Ninguna de las implementaciones de `List` y `Set` son sincronizadas; es decir, no se garantiza el estado de las colecciones si dos o más hilos acceden de forma concurrente al mismo.

## Map
La interfaz `Map` *asocia claves a valores*. Esta interfaz *no puede contener claves duplicadas* y; cada una de dichas claves *sólo puede tener asociado un valor* como máximo.

Analicemos los tipos de implementación para los `Map`:


### HashMap
Es la implementación con mejor rendimiento de todas pero no garantiza ningún orden a la hora de realizar iteraciones. Esta implementación proporciona tiempos constantes en las operaciones básicas siempre y cuando la función hash disperse de forma correcta los elementos.

### TreeMap
Esta implementación almacena las claves ordenándolas en función de sus valores. Es bastante más lento que HashMap. Las claves almacenadas deben implementar la interfaz Comparable. Esta implementación garantiza siempre un rendimiento de `log(N)` en las operaciones básicas, debido a la estructura de árbol empleada para almacenar los elementos.

### LinkedHashMap
Esta implementación almacena las claves en función del orden de inserción. Simplemente, es un poco más costosa que `HashMap`.

---


```java
public class Alumno implements Comparable<Alumno> { // La clase Alumno implementará el método compareTo()
    private String nombre;
    private int nota;

    // Constructor
    public Alumno(String nombre, float nota) {
        this.nombre = nombre;
        this.nota = nota;
    }
    // Getters
    public String getNombre() {
        return nombre;
    }

    public float getNota() {
        return nota;
    }

    // Setters
    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public void setNota(float nota) {
        this.nota = nota;
    }

    @Override
    public String toString() {
        return nombre + " - " + nota;
    }
    
    @Override
    // compareTo() va a comparar a dos alumnos por nota.
    // De esta manera, las colecciones ordenadas usarán esta comparación por defecto.
    public int compareTo(Alumno a) {
        return Float.compare(nota, a.getNota());
    }
}

// ordenación por defecto (nota).
Set<Alumno> listado = new TreeSet<>();

// ts con ordenamiento sobreescrito (por nombre)
Set<Alumno> ts = new TreeSet<>((a, b) -> a.getNombre().compareTo(b.getNombre())); // Con expresiones lambda. Es posible usar funciones tradicionales.
```

Al querer agregar Alumnos al `TreeSet`, obtendremos una excepción, ya que las colecciones tipo `Tree` mantienen un orden entre sus elementos durante el ciclo de vida de todo el programa, y al crear una colección de objetos definidos por el usuario, debemos decirle a la colección cómo es el ordenamiento por defecto, e incluso sobreescribirlo con otro particular si así lo necesitamos. En síntesis, debemos <u>implementar las interfaces</u> `Comparable` para que las colecciones sean capaz de comparar objetos y ordenarlos según un criterio.

---

### Manejo de repetidos en `HashSet`
Las colecciones de tipo `Set` en este caso omitirán la inserción de un `Alumno` que tenga la misma nota que cualquier otro elemento en la colección. Es importante notar esto ya que la colección ordenará por nota, pero no admite notas repetidas.
Para los `TreeSet`, la búsqueda de repetidos se realiza por el método `compareTo()`. En los `HashSet`, se verifican los hashCode y el método `equals()`.

### `equals()`

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true; // 1.

    if (o == null || getClass() != o.getClass()) return false; // 2.

    Alumno alumno = (Alumno) o; // 3.

    return Float.compare(nota, alumno.nota) == 0 && Objects.equals(nombre, alumno.nombre); // 4.
}
```

Desglocemos cada uno de los puntos para entender esta implementación.
Propósito: Este método se utiliza para determinar si dos objetos son "iguales" desde una perspectiva lógica. La implementación por defecto de `equals()` en `Object` simplemente compara las referencias de memoria, es decir, `obj1 == obj2`. Sin embargo, cuando se trabaja con objetos personalizados, como la clase `Alumno`, a menudo se necesita definir qué significa que dos objetos sean iguales, basándote en las necesidades puntuales que se lleguen a presentar. Le proveemos de semántica al modelo para que las colecciones que usen hashing se comporten como queramos: *¿Qué significa que dos instancias de `Alumno` sean iguales?*

1. Verificación de referencia: Si son el mismo objeto en memoria, son iguales.
2. Verificación de nulidad y tipo de clase: Si `o` es `null`, no puede ser igual a `this` (que no es `null`).
Si `o` no es de la misma clase que `this`, no pueden ser iguales. 
`getClass() != o.getClass()` asegura que sean exactamente la misma clase, no solo una subclase. En algunos casos, se prefiere `instanceof` si se quiere permitir la igualdad entre subclases y superclases).
3. Conversión de tipo (casting): Una vez que sabemos que `o` es un `Alumno`, lo podemos castear de `Object` a `Alumno` para acceder a sus atributos.
4. Comparación de atributos:
- `Float.compare(nota, alumno.nota) == 0`: Compara los valores de punto flotante.
- `Objects.equals(nombre, alumno.nombre)`: Compara los nombres. Es preferible a `nombre.equals(alumno.nombre)` porque `Objects.equals()` maneja correctamente los casos en que uno o ambos nombres sean null, evitando las exepciones de tipo `NullPointerException`.

→ **Principios del contrato `equals()`**:
La especificación de Java para Object.equals() establece un contrato estricto que cualquier implementación debe cumplir para funcionar correctamente con colecciones y otras partes del API. Estos principios son:

- **Reflexivo**: Para cualquier referencia no nula `x`, `x.equals(x)` debe devolver `true`.

- **Simétrico**: Para cualquier referencia no nula `x` e `y`, `x.equals(y)` debe devolver `true` si y sólo si `y.equals(x)` devuelve `true`.

- **Transitivo**: Para cualquier referencia no nula `x`, `y`, y `z`, si `x.equals(y)` devuelve `true` y `y.equals(z)` devuelve `true`, entonces `x.equals(z)` debe devolver `true`.

- **Consistente**: Para cualquier referencia no nula `x` e `y`, múltiples invocaciones de `x.equals(y)` deben devolver consistentemente `true` o consistentemente `false`, siempre que ninguna información usada en las comparaciones del objeto sea modificada.

- Para cualquier referencia no nula `x`, `x.equals(null)` debe devolver `false`.

### `hashCode()`

```java
@Override
public int hashCode() {
    return Objects.hash(nombre, nota);
}
```

**Propósito**: Este método devuelve un valor hash entero para el objeto. El valor hash es un número que representa el objeto y se utiliza principalmente para optimizar el almacenamiento y la recuperación de objetos en estructuras de datos basadas en hashing.
El método `hash()` es la forma moderna y recomendada de implementar `hashCode()`. La clase `java.util.Objects` proporciona este método de conveniencia que genera un código hash combinando los códigos hash de los argumentos proporcionados. Internamente, utiliza una estrategia que combina los hash de cada atributo de una manera que <u>minimiza las colisiones y asegura una buena distribución</u>.

→ **Principios del contrato `hashCode()`**
- Siempre que sea invocado en el mismo objeto más de una vez durante una ejecución, `hashCode()` debe devolver consistentemente el mismo valor entero, siempre que no se haya modificado la información usada en las comparaciones de `equals()` del objeto. Este valor no necesita ser consistente entre diferentes ejecuciones de la misma aplicación.

- Si dos objetos son iguales según el método `equals(Object)`, entonces al invocar el método `hashCode()` en cada uno de los dos objetos debe producir el mismo resultado.

- No es obligatorio que si dos objetos no son iguales según el método `equals(Object)`, al invocar el método `hashCode()` en cada uno de los dos objetos deba producir resultados distintos. Sin embargo, generar resultados distintos para objetos desiguales puede mejorar el rendimiento de las tablas hash.

> [!Tip]
> Es importante que `Objects.hash()` combine los hashes de los mismos atributos que `equals()` utiliza para la comparación.

Veamos la lógica que se utiliza para buscar repetidos en los `HashSet`:
1. **Cálculo del Hash**: Primero, el `HashSet` llama al método `hashCode()` del objeto que se intenta agregar. El valor hash devuelto se utiliza para determinar en qué cubeta o *bucket* de la tabla hash interna del `HashSet` debe buscar o almacenar el objeto.
2. **Verificación de Existencia**: Una vez que el `HashSet` ha identificado el cubo potencial, puede haber varios objetos en ese mismo cubo (debido a colisiones de hash, donde diferentes objetos tienen el mismo valor hash).
Para determinar si el objeto que se intenta agregar ya existe en la colección, el `HashSet` itera a través de los objetos en ese cubo y para cada uno, llama al método `equals()` del objeto que intentas agregar, pasándole como argumento los objetos ya presentes en el cubo.
Si `equals()` devuelve `true` para alguno de los objetos en el cubo, el `HashSet` considera que el objeto ya está presente y no lo agrega. Si devuelve `false` para todos los objetos en el cubo o el cubo está vacío, el objeto se agrega.

---

# Recorrido/Iteración de colecciones

## Usando bucles `for` y `foreach`

```java
Set<Alumno> conjuntoAlumnos = new HashSet<>();
List<Alumno> listadoAlumnos = new ArrayList<>();

for(Alumno a : listadoAlumnos ) {
    ...
}

for(int i = 0; i < listadoAlumnos.size(); i++){
    Alumno a = listadoAlumnos.get(i);
    System.out.println(a.toString());
}
```

Ya que las colecciones de tipo **Set** no tienen indexación, no es posible usar un `for` genérico para iterar sobre ellas, pero sí se puede usar un bucle de tipo `foreach`.

## Usando iteradores 

### Iterator

```java
Set<Alumno> listadoAlumnos = new HashSet<>();

/* Ejemplo de listadoAlumnos: { {Juan, 4.5}, {Pirulo, 2}, {Pepe, 6.7} } */

Iterator<Alumno> it = listadoAlumnos.iterator();

while(it.hasNext()){ // Mientras que haya más elementos para iterar
    Alumno a = it.next(); // Obtengo el siguiente valor de la colección
    System.out.println(a.toString());
}
```

Algunas consideraciones sobre los iteradores para tener en cuenta en este ejemplo:
- Cuando se obtiene un Iterator para una colección, el iterador inicialmente "*apunta*" a una posición antes del primer elemento.
- Las llamadas a `next()` obtienen el elemento siguiente de la colección. En este ejemplo, obtiene el alumno Juan. Después de esta llamada, el iterador se mueve hacia adelante para estar "*entre*" Juan y Pirulo.

### ListIterator para colecciones tipo `List`

```java
LinkedList<Alumno> lista = new LinkedList<>();

ListIterator it = lista.listIterator();

// Hacia adelante
while(it.hasNext()){
    Alumno a = it.next();
    System.out.println(a.toString());
}

// Hacia atrás
while(it.hasPrevious()){
    Alumno a = it.previous();
    System.out.println(a.toString());
}
```

Para las colecciones tipo `List`, se usan los iteradores tipo ListIterator, que son más adecuados para listas, permitiéndonos iterar en dos direcciones sobre una lista, usando un sólo iterador.

## Usando expresiones lambda

```java
Set<Alumno> listadoAlumnos = new HashSet<>();

listadoAlumnos.forEach(alumno -> System.out.println(alumno.toString()));
```

## Usando Stream con `forEach()`

no se amigo pero ta re copado re elegante, preguntar!

```java
Set<Alumno> listadoAlumnos = new HashSet<>();

listadoAlumnos.forEach(System.out::println); // god no?
```

---

# Cosas copadas de colecciones tipo `List`

## Ordenamiento

```java
List<Alumno> listadoAlumnos = new ArrayList<>();

Collections.sort(listadoAlumnos); // 1.
Collections.sort(listadoAlumnos, (a, b) -> a.getNombre().comprareTo(b.getNombre())); // 2.
listadoAlumnos.sort((a, b) -> a.getNombre().comprareTo(b.getNombre())); // 3.
listadoAlumnos.sort(Comparator.comparing((Alumno a) -> a.getNombre())); // 4.
listadoAlumnos.sort(Comparator.comparing((Alumno a) -> a.getNombre()).reversed()); // Sentido inverso que en 4.
```

1. Al usar la interfaz `Collections` para ordenar un `ArrayList` de esta manera, el método ordenará por el método `compareTo()` que debe ser definido en su clase. Es decir, deben implementar la interfaz `Comparable`
2. El método `sort()` está sobrecargado, así que podemos <u>sobreescribir el criterio de ordenamiento</u>, para que ordene por nombre en lugar de ordenar por nota, por ejemplo. Este método no necesita que la clase implemente `Comparable`, ya que le pasamos un `Comparator` como segundo argumento.
3. En Java 8 se introdujo el método `sort()` como un método por defecto de la interfaz `List`. Similar a a forma 2, no requiere implementar `Comparable`. En comparación con el método de `Collections`, este usa un método de ordenamiento optimizado para las colecciones de tipo `List`.
4. Es muy legible y conciso, especialmente cuando se ordena por un solo atributo. También es el punto de partida para construir comparadores más complejos, como comparadores encadenados (por ejemplo, `thenComparing()`).

> [!Note]
> Notar que los métodos en los que se usan expresiones lambda, son expresiones que retornan objetos de tipo `Comparator`.
> Una alternativa a usar expresiones lambda es crear una clase que implemente la interfaz `Comparator`, donde definimos un método `compare()` para definir un método de ordenamiento alternativo al predeterminado.

## Conversión
Es posible convertir un `ArrayList` en un array simple con el método `toArray()`.

```java
List<Alumno> lista = new ArrayList<>();
Alumno listaArray[] = lista.toArray();
```

## Sutilezas en `LinkedList`

Al agregar y obtener elementos de las listas enlazadas, se categorizan en dos grupos:
### Lanzan excepciones si la lista está vacía
- `removeFirst()` (Equivalente a `pop()`)
- `removeLast()`
- `getFirst()`
- `getLast()`
- `get(int index)`

### Retornan `null` si la lista está vacía
- `pollFirst()` → Eliminar al inicio
- `pollLast()` → Eliminar al final
- `peekFirst()` → Obtener el primer elemento
- `peekLast()` → Obtener el último elemento
- `peek(int index)` → Obtener el elemento en `index`

---

> [!Note]
> Los `TreeMap` por defecto ordena sus elementos en función de las claves.
> Las claves deben implementar `Comparable`.

