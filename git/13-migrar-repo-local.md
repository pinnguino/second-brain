---
id: 12-migrar-repo-local
aliases:
  - Migrando un repositorio local
tags: []
---
# Migrar un repositorio local
Así como vimos cómo clonar un repositorio de GitHub y trabajar con él de manera local en nuestra computadora, también podemos [[02-primeros-pasos#Creando un repositorio|crear un repositorio local]] con git, y luego migrarlo a un repositorio remoto.

> [!NOTE]
> [GitHub](https://github.com) es sólo un sitio en donde podemos alojar repositorios remotos de git.
> También se puede usar [GitLab](https://about.gitlab.com), entre otros servicios de hosting de repositorios.

Para migrar un repositorio, luego de crear un repositorio local o partiendo de un repositorio ya existente, lo configuramos mediante `git remote`.
El comando `git remote` se utiliza en Git para **gestionar las conexiones con repositorios remotos**. Permite <u>ver, agregar, modificar o eliminar repositorios remotos</u> asociados con tu repositorio local.

---

Para configurar un repositorio local y migrarlo a un repositorio remoto, seguimos los siguientes pasos:

### Paso 1: Crear un repositorio en GitHub
En primer lugar, debemos crear un repositorio en GitHub para poder asociarlo con un repositorio local. Para evitar errores, el repositorio debe ser creado sin un archivo `README` o un archivo `.gitignore`. Estos archivos pueden ser incluidos luego de que el repositorio haya sido subido a GitHub.
Para más información, vea [Creando un repositorio](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository).

### Paso 2: Agregar el repositorio remoto
Posicionados en la carpeta del repositorio, ejecutamos el comando:

```bash
$ git remote add origin REMOTE-URL
```

Este comando asigna a *nuestro repositorio local un repositorio remoto existente*, y en el ámbito local (la carpeta del repositorio local) el repositorio remoto será referenciado como `origin`. Este nombre es una convención comúnmente usada, se recomienda usarla.
En `REMOTE-URL` se hace referencia a la URL del repositorio remoto en donde estará alojado el repositorio.
Esta URL tiene la forma <u>https://github.com/username/repo</u>.

Para verificar que la URL del repositorio remoto ha sido correctamente configurada, ejecutamos el siguiente comando:

```bash
$ git remote -v
origin  https://github.com/pinnguino/repo-test (fetch)
origin  https://github.com/pinnguino/repo-test (push)
```

En este caso, el repositorio local descargará (*fetch*) y subirá (*push*) la información a la URL asignada.

### Paso 3: Subir el repositorio
Luego de configurar el repositorio remoto, sólo queda subir los archivos y trabajar en él.

```bash
$ git push
fatal: The current branch main has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin main
```

Con el repositorio en este estado, cada vez que queramos *pushear* o *pullear* los cambios al repositorio remoto, debemos ejecutar `git push origin main` o bien `git pull origin main` según querramos descargar o subir cambios, en este caso, en la rama `main`. Este comando deberemos introducirlo siempre de manera completa, a menos que configuremos que querramos subir o descargar la información siempre desde el mismo repositorio remoto, en este caso, `origin`.
Para esto, ejecutamos el siguiente comando la primera vez que vayamos a subir información al repositorio:

```bash
$ git push -u origin main
```

De esta manera, sólo será necesario usar `git push` o `git pull` de ahora en adelante.

> [!NOTE]
> Si el repositorio tiene más de una rama, deberemos *pushear* todas las ramas para que aparezcan en el repositorio remoto.

[Siguiente: Forks y contribuciones](14-forks.md)
