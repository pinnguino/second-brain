---
id: suma-de-cuadrados
aliases: []
tags: []
---

Para factorizar un número $n$ en sus factores primos en c++, podemos escribir un programa que recorra los posibles divisores desde 2 hasta \( \sqrt{n} \) (porque cualquier factor primo mayor que \( \sqrt{n} \) debe multiplicarse por un factor menor que \( \sqrt{n} \)). si encontramos un divisor, lo dividimos sucesivamente hasta que ya no sea divisible, y luego continuamos con el siguiente divisor.

Aquí te dejo un ejemplo de cómo podrías hacerlo en C++:

### Código en C++ para factorizar \( n \) en factores primos:

```cpp
#include <iostream>
#include <cmath>

void factorizar(int n) {
    // Primero, probamos los factores primos más pequeños
    // Verificamos si 2 es un factor primo
    while (n % 2 == 0) {
        std::cout << 2 << " ";
        n /= 2;
    }

    // Ahora probamos los números impares desde 3 hasta sqrt(n)
    for (int i = 3; i <= sqrt(n); i += 2) {
        // Verificamos si i es un divisor
        while (n % i == 0) {
            std::cout << i << " ";
            n /= i;
        }
    }

    // Si n es mayor que 2 en este punto, significa que n es primo
    if (n > 2) {
        std::cout << n << " ";
    }

    std::cout << std::endl;
}

int main() {
    int n;
    std::cout << "Introduce un número entero para factorizar: ";
    std::cin >> n;

    std::cout << "Los factores primos de " << n << " son: ";
    factorizar(n);

    return 0;
}
```

### Explicación del código:

1. **Entrada**: El programa pide al usuario que ingrese un número entero \( n \).
   
2. **División por 2**: Primero, se verifica si 2 es un divisor de \( n \) y se divide repetidamente entre 2 mientras sea posible. Esto se hace en el primer `while (n % 2 == 0)`.

3. **División por números impares**: Después, el programa prueba los números impares desde 3 hasta \( \sqrt{n} \). Si un número \( i \) divide a \( n \), el programa lo imprime y divide \( n \) por \( i \) de forma repetida.

4. **Número primo mayor que \( \sqrt{n} \)**: Si después de dividir por todos los posibles divisores hasta \( \sqrt{n} \), queda un número mayor que 2, significa que ese número es primo, por lo que lo imprime.

### Ejemplo de ejecución:

**Entrada:**
```
Introduce un número entero para factorizar: 60
```

**Salida:**
```
Los factores primos de 60 son: 2 2 3 5
```

Esto se debe a que \( 60 = 2 \times 2 \times 3 \times 5 \).

### Notas:

- Este algoritmo tiene una complejidad de tiempo de \( O(\sqrt{n}) \), ya que solo prueba hasta \( \sqrt{n} \) como posibles divisores.
- Si deseas optimizar más, podrías agregar una verificación para números primos más grandes (por ejemplo, el uso de una tabla de números primos) o aplicar un algoritmo de factorización más avanzado para números muy grandes.

Espero que esto te sea útil para factorizar números primos en C++ 😊.
