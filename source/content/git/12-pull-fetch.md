---
id: 11-pull-fetch
aliases:
  - pull
  - fetch
tags: []
---

# Pull y fetch
El comando git pull se utiliza para ***actualizar tu rama local con los cambios de la rama remota*** correspondiente. El comando internamente realiza dos comandos:

1. `git fetch`: Este comando descarga las actualizaciones desde el repositorio remoto pero no las aplica automáticamente a tu rama local. Simplemente las guarda para que puedas revisar los cambios.
2. `git merge`: Después de hacer el `fetch`, `git merge` fusiona los cambios descargados de la rama remota con tu rama local actual.

Por ejemplo, estando en la rama `main` y se ejecuta `git pull origin main`, Git va a traer los últimos cambios de la rama `main` del repositorio remoto de nombre `origin` y los va a fusionar en tu rama local `main`.

```bash
$ git pull origin main
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 923 bytes | 10.00 KiB/s, done.
From https://github.com/pinnguino/repotesthub
   09518fb..130d07b  main       -> origin/main
Updating 09518fb..130d07b
Fast-forward
 test.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

Aquí se ve que luego de ejecutar el `pull` se nos descargan los cambios (`fetch`) del repositorio remoto y se integra (`merge`) en nuestra rama local.

> [!NOTE]
> Con repositorios clonados o repositorios locales migrados correctamente a repositorios remotos, con escribir `git pull` o `git push` para subir los cambios es suficiente.
> Más adelante veremos cómo [migrar repositorios locales](./13-migrar-repo-local.md#Migrar-un-repositorio-local).

---

# Git fetch
Cuando hacemos un pull, como dijimos, ejecuta los comandos fetch y merge para integrar los cambios en nuestra rama local. Es más rápido para nosotros, pero perdemos control en el proceso, generando conflictos de fusión que necesitamos resolver.
La alternativa más segura cuando trabajamos en un repositorio compartido, es descargarnos los cambios en el repositorio remoto, revisarlos, y luego integrarlos a nuestro repositorio local.
Esto es posible con el comando `git fetch`, que sirve para <u>actualizar tu repositorio local con los cambios realizados</u> en el repositorio remoto, pero sin modificar tu área de trabajo ni la rama en la que te encuentras. Es decir, git fetch descarga los nuevos datos (como commits, ramas o tags) desde el servidor remoto a tu repositorio local, pero no fusiona ni cambia tu código actual.

```bash
$ git fetch
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 997 bytes | 8.00 KiB/s, done.
From https://github.com/pinnguino/repotesthub
   3a5c455..030efb6  main       -> origin/main
```

Vemos que si se encuentran cambios en el repositorio remoto respecto a nuestro repositorio local, o dicho de otra manera, si el repositorio remoto está adelantado en commits, descargará los cambios. Luego, si queremos visualizar los cambios, debemos movernos a la rama remota.

```bash
$ git switch --detach origin/main
HEAD is now at 030efb6 Create new.txt
```

Aquí nos movemos a la rama remota usando el argumento `--detach`. Luego aquí podemos ver `log` de commits, ver las diferencias con `diff`, y si nos gustan los cambios, podemos integrarlos a la rama que estamos trabajando con `git pull`. 

> [!IMPORTANT]
> Usar el argumento `--detach` nos dejará en el estado detached HEAD. Ver más [aquí](./09-reflog.md#Detached-HEAD).

```bash
$ git switch main
Previous HEAD position was d4eb402 Create asd.txt
Switched to branch 'main'
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
$ git pull
Updating 3a5c455..030efb6.
Fast-forward
 asd.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 new.txt
```

[Siguiente: Migrando un repositorio local](13-migrar-repo-local.md)
