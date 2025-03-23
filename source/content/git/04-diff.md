---
id: diff
aliases:
  - Cambios en el repositorio
tags: []
---
# Git diff
El comando `git diff` nos permite ver las diferencias entre distintos estados del repositorio, área de trabajo y de preparación, y poder compararlas entre sí. Veamos las posibles formas de utilizar esta instrucción.

## Workspace - Commit
Podemos modificar archivos en nuestra área de trabajo, y consultar los cambios sin necesidad de hacer nada más con el comando `git diff`.
Ejecutar ese comando nos permite comparar todos los archivos modificados en el área de trabajo con el último commit.

## Staging - Commmit
Si realizamos una modificación a un archivo con intención de subir dichos cambios al repositorio, y necesitamos analizar los cambios realizados, podemos subir el archivo a staging y comparar todo lo que está en preparación con el último commit con el comando `git diff --staged`.

```bash
$ git add archivo.txt
$ git diff --staged
diff --git a/archivo.txt b/archivo.txt
index 33de878..d968e73 100644
--- a/archivo.txt
+++ b/archivo.txt
@@ -1,2 +1,5 @@
 Hola, mi nombre es Jaimito.
 Estoy aprendiendo git!
+
+Aquí agrego una línea más!
+Y otra más...
```

Aquí se ve que modificamos `archivo.txt` y lo subimos a preparación, y lo comparamos con el último commit, donde se ve que el archivo staged tiene tres líneas más agregadas denotadas con el `+`, las líneas eliminadas se denotan con un `-`.

## Comparar ramas
Podemos ver las diferencias entre dos ramas con el comando `git diff <branch1> <branch2>`.
[Ver ramas aquí](./06-branches.md#Rama-(branch)).

## Comparar un archivo
Así como el comando `diff` a secas nos permite ver TODAS las diferencias, podemos enfocarnos sólo en un archivo con `git diff <file>`.
Con esto podremos ver las diferencias entre los archivos en nuestra área de trabajo y el último commit. Si está en preparación, le agregamos el argumento `--staged`.

## Comparación entre commits
En nuestro repositorio puede que se presente la necesidad de ver los cambios entre dos commits. Para hacer esto debemos conocer el id de los commits que queremos comparar. Antes debemos comprender los IDs de los commits.

---

### Git log
Este comando nos permite ver los commits realizados en el repositorio, ordenados de manera descendente. De cada commit podemos ver:
- El ID del commit.
- El autor del commit
- La fecha y hora del commit.
- El mensaje del commit.

```bash
$ git log
commit f986c00c08b8a0d8385706a34656a341008171f3 (HEAD -> master)
Author: pinnguino <s.gallardogaston@gmail.com>
Date:   Mon Mar 3 14:28:11 2025 -0300

    Modificando una vez mas archivo.txt!

commit 45ccb79e3ae17ee4ea1f0f869cf35ffe60bbf434
Author: pinnguino <s.gallardogaston@gmail.com>
Date:   Mon Mar 3 14:16:08 2025 -0300

    Modificando archivo.txt

commit 63aa0132cf0620bf5ea2b007269ef1d0702f3fc9
Author: pinnguino <s.gallardogaston@gmail.com>
Date:   Sun Mar 2 20:22:05 2025 -0300

    Renombrando santi.txt a archivo2.txt, como estaba originalmente.
```

Aquí se muestran los últimos 3 commits del repositorio de práctica.
Se puede abreviar esta información, con el parámetro `--oneline`, que muestra el ID del commit abreviado, junto con la dirección del puntero, y el mensaje de cada commit.
```bash
$ git log --oneline
f986c00 (HEAD -> master) Modificando una vez mas archivo.txt!
45ccb79 Modificando archivo.txt
63aa013 Renombrando santi.txt a archivo2.txt, como estaba originalmente.
8c129c7 Cambiando el nombre de archivo2.txt a santi.txt
ddbc6b8 Agregando nuevamente el archivo 2.
c2df39b Eliminando archivo2.txt
8054f33 Agregando el segundo archivo: archivo2.txt
6f2da48 Agregando el primer archivo al repositorio!
```

> [!IMPORTANT]
> La longitud del ID del commit se puede modificar a elección del usuario.
> A medida que se trabaja con repositorios más grandes, se debe especificar una longitud de ID más larga, ya que a medida que aumenta el número de commits, aumenta la probabilidad de que alguno de estos IDs abreviados se repita.
> La longitud del ID se puede modificar con el comando `git config --global core.abbrev <length>`. Por defecto, está establecido en 7.

Esta abreviación es la que usaremos para referirnos a los commits para compararlos mediante `git diff`.

---

Veamos cómo se comparan 2 commits.
Con el comando `git diff <id1> <id2>` obtendremos con detalle las diferencias entre ambos commits, los archivos que han sido modificados, y qué modificaciones fueron realizadas.

```bash
$ git diff f986c00 368dd6b
diff --git a/archivo.txt b/archivo.txt
index 6fb20bf..3321f04 100644
--- a/archivo.txt
+++ b/archivo.txt
@@ -3,3 +3,5 @@ Estoy aprendiendo git!

 Aquí agrego una línea más!
 Aquí agrego otra más modificada...
+
+Y aquí, otra más!!
diff --git a/archivo2.txt b/archivo2.txt
index aa331d2..a9485a7 100644
--- a/archivo2.txt
+++ b/archivo2.txt
@@ -2,4 +2,6 @@ Hola, este es el archivo 2.

 Te mando un saludo desde acá!

-:D
+:(
+
+Ahora quito la carita feliz y agrego una carita triste.
```

> [!tip]
> Si primero ponemos un commit anterior, y luego un commit posterior (temporalmente), las líneas con `-` Significarán las líneas que han sido eliminadas en el nuevo commit, y las líneas con `+` Representarán las líneas que han sido agregadas. Invertir el orden de los commits (en términos de fecha) puede generar salidas contraintuitivas.

> [!QUESTION]  ¿Y si sólo quiero ver los nombres de los archivos que se modificaron?
> Con el comando `git diff --name-only <id1> <id2>` podremos ver sólo los nombres.

Hay una alternativa para poder analizar con más claridad las diferencias cuando se modifica una o más líneas.
Con el comando `git diff --word-diff <id1> <id2>` podemos ver las diferencias entre líneas modificadas lado a lado, de una manera más clara en algunos contextos.

```bash
$ git diff --word-diff f986c00 368dd6b
diff --git a/archivo.txt b/archivo.txt
index 6fb20bf..3321f04 100644
--- a/archivo.txt
+++ b/archivo.txt
@@ -3,3 +3,5 @@ Estoy aprendiendo git!

Aquí agrego una línea más!
Aquí agrego otra más modificada...

{+Y aquí, otra más!!+}
diff --git a/archivo2.txt b/archivo2.txt
index aa331d2..a9485a7 100644
--- a/archivo2.txt
+++ b/archivo2.txt
@@ -2,4 +2,6 @@ Hola, este es el archivo 2.

Te mando un saludo desde acá!

[-:D-]{+:(+}

{+Ahora quito la carita feliz y agrego una carita triste.+}
```

### Resumen
La instrucción `diff` es sumamente útil para ver las diferencias entre estados del repositorio, y poder comparar entre commits, y también entre el último commit y el área de staging, y las diferencias podemos verlas en varios formatos. 

[Siguiente: Modificación y eliminación de commits](05-modificar-y-deshacer-commits.md)
