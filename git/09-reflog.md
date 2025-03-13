---
id: 08-reflog
aliases:
  - Reflog
tags: []
---

# Reflog
`git reflog` es una herramienta que almacena el historial de todas las referencias a las que apuntó el puntero `HEAD` en nuestro repositorio. Esto nos permite recuperar commits perdidos por accidente.

## Funcionamiento
Internamente, cuando eliminamos un commit con `git reset`, el commit no se elimina, si no que se pierde la referencia del hash del mismo. Esto significa que, el commit sigue en nuestra computadora, pero si queremos recuperarlo, debemos conocer la referencia.
Puede pasar que por accidente eliminamos un commit que no queríamos eliminar, y aún el ID del commit está presente en el historial de comandos de git. Pero si no lo tenemos, podemos acceder al historial de movimientos del puntero con `git reflog`.

```bash
$ git reflog
bc464da (HEAD -> master) HEAD@{0}: checkout: moving from modificar to master
387c633 (modificar) HEAD@{1}: rebase (finish): returning to refs/heads/modificar
387c633 (modificar) HEAD@{2}: rebase (continue): Modificando archivo2, mejorando comentarios en cafeteria y actualizando
bc464da (HEAD -> master) HEAD@{3}: rebase (start): checkout master
a0a034a HEAD@{4}: checkout: moving from master to modificar
bc464da (HEAD -> master) HEAD@{5}: commit: Leves agregados en comentarios y texto en archivo2
```

Aquí se ve sólo unas líneas de la salida de `git reflog`. Notar que se muestran todos los movimientos que se realizaron con el puntero.
Si eliminamos un commit que no queríamos perder, podremos encontrarlo aquí, y recuperarlo con `git reset --hard <commit-ID>`.

### Detached HEAD
Es posible usar `git checkout` con un commit recuperado con `git reflog` para entrar en el estado de "*puntero separado*". Esto nos permite mirar el commit, hacer cambios para confirmarlos en nuestro repositorio, o llevar el commit hacia una nueva rama con `git switch -c <nombre-rama>`.

Al ejecutar el comando en un commit perdido, git nos dirá lo siguiente:
```bash
Note: switching to 'edc783a'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false
```

Aquí podremos echar un vistazo a los archivos, y si queremos quedarnos con los cambios en nuestro repositorio, podemos crear una rama en este estado, y el commit se llevará a esa rama. Luego podremos trabajar sobre esa rama e integrar los cambios en la rama principal si así lo quisiéramos.

[Siguiente: Introducción a GitHub](10-github.md)
