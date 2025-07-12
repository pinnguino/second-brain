---
id: 06-clases-wrapper
aliases: []
tags: []
---

# Clases Wrapper
Los wrappers son clases que <u>encapsulan los tipos primitivos</u> y los convierten en objetos. Esto es fundamental para trabajar con APIs y colecciones de Java, que requieren objetos en lugar de datos primitivos. Con la evolución hacia Java 21, son también objeto de optimizaciones en términos de rendimiento y nuevas implementaciones de funcionalidad, como la integración con el pattern matching, por ejemplo, la clase Integer sirve para representar valores enteros, permitiendo no solo la manipulación de datos aritméticos, sino también el uso de métodos útiles como parseInt(), valueOf() o intValue(). Este mecanismo es vital para trabajar con colecciones y APIs que requieren argumentos objeto, facilitando la interacción con librerías y frameworks sin tener que recurrir a soluciones complejas o no tipadas.

## Autoboxing y Unboxing
Cuando hablamos de **autoboxing**, hacemos referencia a cuando instanciamos un objeto de una clase wrapper con un valor literal, el lenguaje se encarga de tomar las literales, que se toman como valores primitivos, y asociarlas con una instancia de una clase envoltorio.
Por otra parte, **unboxing** se refiere al proceso inverso: Convertir un objeto wrapper a su correspondiente valor primitivo.
