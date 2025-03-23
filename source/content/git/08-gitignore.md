---
id: gitignore
aliases:
  - Ignorar archivos
tags: []
---
# Ignorar archivos
En un repositorio es común que tengamos archivos temporales de ejecución, o configuración de nuestro entorno de trabajo que no tiene por qué ser subido al repositorio. Para resolver esto, git nos tiene cubiertos.

## Git ignore
En la carpeta raíz de nuestro repositorio podremos crear un archivo llamado `.gitignore`, que contendrá todos los archivos que no queremos que sean tomados en cuenta por git.

> [!NOTE]
> Git ignora los archivos presentes en el archivo `.gitignore` que no han sido rastreados anteriormente.
> Por ejemplo: Si tenemos un archivo llamado `file.txt` que ya ha sido *commiteado* al repositorio, y luego configuramos `.gitignore` para que ignore ese archivo, o todos los archivos `.txt`, el archivo no será ignorado.
> Si queremos ignorar efectivamente este archivo debemos eliminarlo del repositorio.

### Funcionamiento
Cuando nosotros agregamos a staging (`add`) o hacemos una confirmación (`commit`) en el repositorio, internamente git consulta el archivo `.gitignore` (si este existe) para ver cuáles son los archivos que se deben ignorar. En caso contrario, los agrega al área de preparación.
La forma de escribir el archivo para que detecte los archivos a ignorar es con ***patrones glob***.

### Patrones glob
Los patrones glob <u>especifican juegos de nombres de archivos o carpetas</u> mediante *comodines*.
Los comodines más usados para los patrones en git son:

1. `*` -> Coincide con cualquier número de caracteres, incluido ninguno.

| Ejemplo | Coincide con | No coincide con |
| --------------- | --------------- | --------------- |
| `Law*` | `Law`, `Laws`, `Lawyer` | `GrokLaw`, `La`, `aw` |
| `*Law*` | `Law`, `GrokLaw`, `Lawyer` | `La`, `aw` |


2. `?` -> Coincide con cualquier caracter individual.

| Ejemplo | Coincide con | No coincide con |
| --------------- | --------------- | --------------- |
| `?at` | `Cat`, `cat`, `Bat` o `bat` | `at` |


3. `[abc]` -> Coincide con un caracter dado entre paréntesis.

| Ejemplo | Coincide con | No coincide con |
| --------------- | --------------- | --------------- |
| `[CB]at` | `Cat`, `Bat` | `cat`, `bat`, `CBat` |


### Ejemplos
- Si queremos **ignorar un archivo particular** llamado `ignorar.txt`, basta con escribir el nombre del archivo con su extensión.
- Si queremos **ignorar todos los archivos con extensión** `.jpg`, sin importar el nombre, con la sentencia `*.jpg`.
- Si necesitamos que **todos los archivos `.txt` se ignoren excepto uno** llamado `no-ignorar.txt`, podemos ignorar todos los archivos con `*.txt`, y luego *des-ignorar* el archivo con `!no-ignorar-txt`.
- Si queremos ignorar un directorio completo llamado `img`, podemos escribir su nombre con un separador -> `img/`.

> [!TIP]
> Los comentarios en los archivos `.gitignore` se les antepone un `#`.

## Git ignore global
Si en nuestros proyectos tenemos siempre archivos de una misma característica que ignoramos, podemos definir un archivo .gitignore de manera global para que todos los repositorios se puedan ignorar estos archivos.
Para hacer esto se crea un archivo de git ignore, por ejemplo `.gitignore_global` y lo guardamos en donde creamos conveniente. Luego, accedemos a la configuración global e establecemos un archivo que contendrá todos los archivos comunes a ignorar en todos los repositorios.

```bash
$ git config --global core.excludesfile ruta/al/.gitignore_global
```

> [!NOTE]
> Si necesitamos una configuración especial para ignorar archivos en un repositorio particular, podemos crear un `.gitignore` en el repositorio y git le dará prioridad.
> Siempre se le da prioridad al ámbito local antes que el global.

[Siguiente: Reflog](09-reflog.md)
