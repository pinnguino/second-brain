---
id: primeros-pasos
aliases: []
tags: []
---
# Tu primer repositorio

## ¿Qué es un repositorio?
Un repositorio es un espacio (generalmente un directorio) donde se almacenan archivos, y *gestionar las distintas versiones* a lo largo del tiempo de forma eficiente y cómoda para las personas involucradas en dicho repositorio. 
Git nos proporciona una manera de gestionar estos repositorios localmente. Estos repositorios pueden alojarse en la nube, mediante <u>GitHub</u> o de manera local en nuestra computadora o servidor.

## Creando un repositorio
Para crear nuestro repositorio debemos posicionarnos mediante interfaz de línea de comandos al directorio en donde queremos que se aloje nuestro repositorio. En este caso nuestro directorio se llamará `repo-test`.
Luego ejecutamos el comando `git init`. Este comando creará un directorio llamado `.git/` que tendrá todos los archivos necesarios para implementar el repositorio.
Al finalizar la creación del repositorio, si tenemos un prompt que nos indique información de git, nos dirá que estamos en la rama `master`. Esto lo veremos en detalle cuando veamos el concepto de <u>rama o branch</u>.

## Entornos de git
Cuando usamos git, tenemos 3 entornos importantes en donde manipularemos nuestros archivos.

- **Área de trabajo (workspace):** Es el directorio en donde trabajaremos con nuestros archivos a gestionar.
- **Área de preparación (staging):** Es el entorno donde se "preparan" los archivos modificados para ser subidos al repositorio.
- **Repositorio:** Es en donde se localizan todos los archivos subidos desde el área de *staging*.

>   Al tener un archivo en nuestra área de trabajo y lo modificamos, el archivo se dice que está *modificado*.
>   Y cuando está subido al área de preparación, se dice que un archivo *staged*.

## Tu primer commit
En nuestro nuevo repositorio (vacío) crearemos un archivo de texto en donde le agregaremos unas palabras.
```text
-- archivo.txt --
Hola, mi nombre es Jaimito.
Estoy aprendiendo git!
```
Al crear este archivo en el repositorio, se nos creará un nuevo archivo ***untracked***. Esto significa que hay un archivo nuevo en el repositorio que no está siendo preparado para subirse. Entonces, lo que haremos será subirlo al repositorio. Pero antes, necesitamos subirlo al área de staging.
Para subir un archivo al área de staging, usaremos el comando `git add <file1> <file2> ... <fileN>`.

>   Todos los eventos que transcurran en el repositorio pueden consultarse mediante el comando `git status`.
>   Nos dirá la rama en la que estamos posicionados, los archivos *untracked*, los archivos *modified*, los que están en el area de *staging*, entre otras cosas.
>   Existe una versión más compacta, se puede usar con `git status -s` o bien `git status --short`

Luego de consultar el estado del repositorio después de agregar `archivo.txt` a staging, la salida será similar a la siguiente:
```bash
$ git status
On branch master

No commits yet

Changes to be commited:
    (use "git rm --cached <file>" to unstage)
    new file:   archivo.txt
```

Como vemos, aquí nos dice que, como habíamos hecho, `archivo.txt` está listo para subirse al repositorio.

> [!note] 
> Si un archivo(s) está en el área de staging y queremos sacarlo(s) de ahí, podemos usar el comando `git restore --staged <file1> <file2> ...` para quitarlo(s).

A modo didáctico, si quisieramos quitar el archivo de staging tendremos la salida:
```bash
$ git remove --staged archivo.txt
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        archivo.txt
```

Como se ve en la salida, `archivo.txt` pasaría a ser un archivo sin seguimiento (*untracked*). <u>Todos los archivos que NO estén en staging no se subirán al repositorio</u>.

Una vez que tenemos todos los archivos preparados, los subiremos al repositorio, lo que se le dice "hacer un *commit*". Para hacer esto lo podemos hacer de dos maneras.

### Commit (Forma 1)

```bash
$ git commit -m "Agregando el primer archivo al repositorio!"
[master (root-commit) 6f2da48] Agregando el primer archivo al repositorio!
 1 file changed, 2 insertions(+)
 create mode 100644 archivo.txt
```

Aquí distinguimos información importante:
- La rama en donde se hizo el commit (`master`).
- El ID del commit (`6f2da48`). Este ID es único e irrepetible para cada commit.
- Los archivos modificados. Podemos ver cuántos archivos se modificaron, así como también las líneas nuevas agregadas. `1 file changed, 2 insertions(+)`
- Los nombres de los archivos modificados. `create mode 100644 archivo.txt`

### Commit (Forma 2)
Para la segunda forma, basta con introducir el comando `git commit`.
Apenas introducimos el comando, se nos abre nuestro editor de texto configurado anteriormente para introducir el mensaje asociado al commit, donde podemos escribir *mensajes más extensos y detallados* para los commits. Este método es recomendado cuando los cambios realizados son complejos o difíciles de explicar en pocas palabras.

> [!tip]
> Otro comando popular es `git commit -a`. La `-a` representa todo (**all**). Esta opción prepara automáticamente todos los archivos para realizarles commit. Nos permite saltarnos el paso de agregar los archivos a staging.
> Si se agregan nuevos archivos, la opción `-a` no los preparará.
> <u>Solo se confirmarán los archivos que el repositorio de Git tenga conocimiento</u>.

[Siguiente: Manipulación de archivos](03-manipulacion-de-archivos.md)
