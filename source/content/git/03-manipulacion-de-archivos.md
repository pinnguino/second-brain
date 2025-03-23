---
id: manipulacion-de-archivos
aliases: []
tags: []
---
# Eliminar archivos
Luego de subir archivos a un repositorio, también podemos eliminarlo.
Supongamos que en el repositorio tenemos dos archivos: `archivo.txt` y `archivo2.txt`.
Si queremos eliminar `archivo2.txt` del repositorio, debemos eliminarlo de nuestro espacio de trabajo. Si revisamos el estado del repositorio después de eliminar:

```bash
$ rm archivo2.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    archivo2.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Vemos que `archivo2.txt` ha sido eliminado del espacio de trabajo. Si queremos que estos cambios se suban al repositorio, basta con "agregar el archivo eliminado" a staging, para luego hacer un commit.
Luego de agregar el cambio a staging:

```bash
$ git add archivo2.txt
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    archivo2.txt
```

Luego haremos el commit con el mensaje, y los cambios se actualizarán en el repositorio.

---

# Restaurar
Si en nuestra área de trabajo eliminamos o modificamos un archivo y ese cambio no está en staging, podemos recuperarlo con `git restore <file>`.
Este comando restaura el archivo a la <u>última versión registrada en el repositorio</u>. Es decir, Se vuelve al estado del archivo subido en el último commit.

> [!IMPORTANT]
> Si el archivo está en staging, podemos usar `git restore --staged <file>` para quitarlo del area de staging, y luego podremos restaurarlo como se mencionó anteriormente. Puede resultar engorroso si hay muchos archivos en staging.
> Alternativamente, se puede usar `git reset` para quitar archivos de staging. Ver sección [modificar commits](05-modificar-y-deshacer-commits.md#Git-reset).

---

# Renombrar
También podemos renombrar archivos mediante el comando `git mv <file> <file-with-new-name>`. Luego hacemos un commit y el archivo se cambiará de nombre.
Es más recomendable usar esto que cambiándole el nombre manualmente, ya que de esta manera git detectará que el archivo renombrado (con el nombre anterior) se eliminó, y un nuevo archivo (el mismo pero con el nombre actual) se agrega al repositorio, cuando no fué así.

[Siguiente: Cambios en el repositorio](04-diff.md)
