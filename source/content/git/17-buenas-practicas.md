---
id: 17-buenas-practicas
aliases:
  - buenas practicas
  - practicas
tags: []
---

# Buenas prácticas en Git
En esta sección, aprenderemos algunas de las mejores prácticas para trabajar con Git, especialmente cuando colaboramos en proyectos grandes o en equipos distribuidos. Estas recomendaciones ayudarán a mantener un historial de commits limpio, a mejorar la colaboración con otros desarrolladores y a evitar errores comunes.

## Escribir mensajes de commit claros y descriptivos
Uno de los aspectos más importantes de trabajar con Git es escribir buenos mensajes de commit. Un buen mensaje de commit debe ser claro y descriptivo, proporcionando suficiente información sobre los cambios realizados.

### Mal mensaje en un commit

```bash
$ git commit -m "arreglos"
```

Un mensaje de commit como 'arreglos' no ofrece ninguna información útil sobre qué se cambió o por qué.

### Buen mensaje en un commit

```bash
$ git commit -m "Corrige bug en el cálculo de impuestos cuando el total es negativo"
```

Un mensaje de commit como ese describe claramente qué se cambió y por qué es importante.

## Realizar commits pequeños y frecuentes
Hacer commits pequeños y frecuentes es una buena práctica que facilita el seguimiento de los cambios y simplifica la resolución de problemas. Cada commit debe estar enfocado en una sola tarea o funcionalidad, lo que hace que el historial sea más legible y fácil de revisar.

Cuando los commits son demasiado grandes, resulta más difícil identificar qué introdujo un error o un problema. Por otro lado, commits pequeños y específicos permiten que los cambios sean revertidos o ajustados con mayor precisión.

## Crear y usar ramas responsablemente
Unas recomendaciones para usar de buena maneras las ramas:

- **Ramas para nuevas características**: Crea una nueva rama para cada nueva característica, corrección de errores o experimento. Esto permite trabajar de manera aislada sin afectar la rama principal (main o master).
- **Mantener la rama principal limpia**: La rama principal debe reflejar un estado estable del proyecto. Evita hacer cambios directamente en main a menos que estés seguro de que son cambios listos para producción.
- **Eliminar ramas que ya no se usan**: Una vez que una rama ha sido fusionada, puedes eliminarla para mantener el repositorio organizado.
- **Elegir el método más apropiado para fusionar ramas**: Ver [Estrategias de fusión de ramas](07-metodos-merge.md#Estrategias-de-fusión-de-ramas).

## Revisar el código y realizar pull requests
Cuando trabajas en equipo, es esencial realizar revisiones de código antes de fusionar cambios en la rama principal. Antes de fusionar una rama, abre un [pull request](./15-pull-requests.md#Pull-request) para que otros desarrolladores puedan revisar tus cambios y sugerir mejoras o correcciones.

## Proteger la rama principal
Una práctica común en proyectos colaborativos grandes es proteger la rama principal. Esto significa que nadie puede hacer cambios directamente en la rama principal sin pasar primero por un proceso de revisión (usualmente a través de un [pull request](./15-pull-requests.md#Pull-request)).

Para proteger una rama en GitHub, sigue estos pasos:
- Ve a Settings en tu repositorio.
- Selecciona Branches y luego elige la rama que deseas proteger.
- Habilita la protección de la rama y establece reglas como requerir revisiones antes de fusionar o asegurarse de que los tests pasen.

Esto garantiza que solo el código revisado y probado llegue a la rama principal.
Ver más sobre ramas protegidas [aquí](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches).

## Mantener el historial limpio
El comando `git rebase` puede ser muy útil para mantener un historial de commits limpio. Usando <u>rebase interactivo</u>, puedes reordenar, fusionar o eliminar commits innecesarios antes de que sean fusionados en la rama principal.
Para más información, ver [rebase interactivo](./05-modificar-y-deshacer-commits.md#Modificar-commits-anteriores-(Rebase-interactivo)).
