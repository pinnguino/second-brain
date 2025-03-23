---
id: modificar-y-deshacer-commits
aliases:
  - Modificacion y eliminacion de commits
tags: []
---

# Modificación de commits
Podemos modificar commits en nuestro repositorio si tenemos la necesidad, pero hay ciertas consideraciones a tener en cuenta para hacer buenas prácticas de git y evitar conflictos si el repositorio es compartido entre más equipos.

## Modificar el último commit
Una situación que se puede presentar es que el último commit realizado en el repositorio quedó incompleto o erróneo, ya sea porque el mensaje está mal, o se nos ha olvidado agregar archivos nuevos o modificaciones que han quedado pendientes.
Una solución es volver a hacer otro commit con los cambios pendientes, pero es una práctica desprolija. Tenemos una alternativa más sofisticada.
Con el comando `git commit --amend` nos permite *reemplazar el último commit* con lo que hayamos preparado. Es decir:
Si el último commit está mal y queremos arreglarlo, debemos preparar todos los cambios y subirlos a staging como si fuéramos a hacer un commit normalmente. La diferencia está en que ejecutamos el comando mencionado más arriba, y podremos escribir el mensaje del commit.
Luego de terminar el mensaje, veremos que el último commit se reemplazó con el commit que preparamos. Notar que no se modifica el commit, si no que se reemplaza por otro.
Esto se ve fácilmente porque el ID del commit cambia.

## Modificar commits anteriores (Rebase interactivo)
También es posible modificar un commit anterior, pero esto es engorroso, puede provocar conflictos de archivos y no se recomienda. Igualmente se explica aquí si no hay otra alternativa.
Si nosotros queremos modificar, por ejemplo, un commit que está 3 lugares hacia atrás, debemos hacer un ***rebase interactivo***. Esto es posible con el comando `git rebase -i`. Este comando nos sirve para modificar el historial de commits de una rama de una forma controlada.
Por ejemplo, si queremos interactuar con los últimos 3 commits, debemos posicionar el puntero `HEAD` y moverlo 3 lugares hacia atrás de la siguiente manera: `git rebase -i HEAD~3`.
A continuación una representación gráfica de mover el puntero:

```text
-- Antes de mover el puntero --

commit1 -> commit2 -> commit3 -> commit4 -> commit5 
                                             HEAD

-- Después de hacer rebase -i --

commit1 -> commit2 -> commit3
                       HEAD
```

Luego de ejecutar el comando, se nos aparece en nuestro editor de texto los últimos 3 commits con su ID y su mensaje, así como también una palabra `pick`. Esta palabra significa qué acciones queremos realizar sobre los commits.
Acciones disponibles:
- `pick`: Mantiene el commit tal como está.
- `reword`: Permite cambiar el mensaje del commit sin cambiar el contenido
- `fixup`: Similar a squash, pero no te permite editar el mensaje del commit (usa el mensaje del commit anterior).
- `edit`: Detiene el rebase en ese commit para que se pueda modificar (ya sea el contenido y/o el mensaje).
- `squash`: Combina el commit con el anterior. El mensaje de commit será una combinación de ambos.
- `fixup`: Similar a `squash`, pero no te permite editar el mensaje del commit (usa el mensaje del commit anterior).
- `drop`: Elimina el commit completamente.

> [!tip]
> Si durante el proceso de rebase se producen conflictos, Git te pedirá que los resuelvas.
> Después de resolverlos, también se usa `git rebase --continue`.

En este caso, si queremos cambiar un commit, elegimos el commit deseado y le anteponemos la palabra `edit`.
Luego preparamos el commit por el que se va a reemplazar y lo confirmamos con `git commit --amend`.

> [!IMPORTANT]
> Notar que si ejecutamos `git log`, veremos que los últimos commits faltan. Esto es porque desplazamos el puntero 3 lugares hacia atrás, por lo que debemos <u>completar el rebase</u> con `git rebase --continue`. Esto hará que vuelva a unir los commits siguientes con el commit que acabamos de modificar.

### Resumen
`git rebase -i` Puede ser utilizado para:
- **Reordenar commits**: Si deseas cambiar el orden de los commits en tu historial, puedes modificar el orden de las líneas en el archivo del rebase interactivo.
- **Fusionar commits**: Utilizando squash o fixup, puedes combinar varios commits en uno solo para mantener un historial limpio.
- **Editar commits antiguos**: Puedes usar edit para modificar el contenido de un commit, incluso si ya han pasado varias revisiones.
- **Cambiar mensajes de commits**: Con reword, puedes corregir o mejorar los mensajes de commit, lo que es útil si cometiste errores de descripción.
- **Eliminar commits innecesarios**: Con drop, puedes eliminar commits del historial que ya no son necesarios o que son erróneos.

---

# Deshacer commits
Como ya vimos antes, un repositorio gráficamente se ve como un grafo en donde cada nodo representa un commit, `(HEAD)` representa el commit en donde estamos parados, y las bifurcaciones representar ramas o branches.

[[git-graph.png]]

Si se presenta la necesidad de deshacer 1 o más commits, podemos hacerlo con el comando `git reset`. Veamos las distintas formas de usarlo.

## Git reset
El comando `git reset` es utilizado para deshacer cambios y modificar el estado de tu repositorio de varias maneras, dependiendo de cómo lo utilices.
Su función es mover el puntero de la rama actual `(HEAD)` a un commit específico, lo que afecta el área de trabajo, el área de staging, o ambos, dependiendo del modo que usemos.
Uso del comando: `git reset --<mode> <commitID>` o bien `git reset --<mode> HEAD~n`, siendo n un número que representa el número de commits a retroceder. *Si no se especifica el commit, el puntero NO retrocede. Se mantiene apuntando al último commit*.

### Soft
La versión `--soft` tiene como efecto mover el puntero al commit especificado, y los archivos que estaban confirmados pasan a estar en staging, listos para ser confirmados nuevamente.
El uso más común es cuando se necesita deshacer un commit pero mantener los cambios listos para ser confirmados de nuevo en el área de staging.
Los archivos que estén en staging se mantienen, los archivos del último commit se agregarán. Se sobreescribirán los archivos de ser necesario.

Antes de usar `git reset --soft`:

| Área de trabajo | Staging  | Repositorio |
| :-------------: | :------: | :---------: |
|                 | file.txt | A -> B -> C |



Después de usar `git reset --soft`:

| Área de trabajo | Staging  | Repositorio | HEAD |
| :-------------: | :------: | :---------: |:----:|
|  Sin modificar  | file.txt |   A -> B    |   C  |
|                 |    C     |             |      |


### Mixed
La versión `--mixed` mueve el puntero al commit especificado, y limpia todo el contenido del área de staging, pero no toca los archivos en el área de trabajo.
**Uso común**: Deshacer un commit, quitar los archivos del área de staging pero mantener los cambios en tu directorio de trabajo.

Antes de usar `git reset --mixed`:

| Área de trabajo | Staging  | Repositorio | HEAD |
| :-------------: | :------: | :---------: |:----:|
|                 | file.txt | A -> B -> C |   C  |


Despues de usar `git reset --mixed`:

| Área de trabajo | Staging | Repositorio | HEAD |
| :-------------: | :-----: | :---------: |:----:|
|  Sin modificar  | (vacío) |   A -> B    |   B  |


> [!NOTE]
> Si teníamos archivos en staging, después de ejecutar `git reset --mixed` los archivos *se moverán a sin preparar* (unstaged).

### Hard
La opción más destructiva de todas, `--hard` mueve el puntero hacia el commit especificado, el área de staging se vacía, y el área de trabajo vuelve al estado del último commit.
Se usa cuando se desea <u>descartar todos los cambios</u> (incluidos los no confirmados) y hacer que tu repositorio sea exactamente como el commit seleccionado.

Antes de usar `git reset --hard`:

| Área de trabajo | Staging  | Repositorio | HEAD |
| :-------------: | :------: | :---------: |:----:|
|                 | file.txt | A -> B -> C |   C  |


Después de usar `git reset --hard`:

|  Área de trabajo   | Staging | Repositorio | HEAD |
| :----------------: | :-----: | :---------: |:----:|
| Sincronizado con B | (vacío) |   A -> B    |   B  |


[Siguiente: Ramas (Branches)](./06-branches.md)
