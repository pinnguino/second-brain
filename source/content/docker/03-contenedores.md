---
id: 03-contenedores
aliases: []
tags: []
---

# Manejo de contenedores

En esta sección aprenderemos todo lo necesario sobre contenedores. Desde la creación, hasta la gestión, informes, registros, etc.

## El comando `docker`
Al ejecutar el comando `docker` en la terminal sin argumentos, obtenemos una salida de *usage* de Docker.

```bash
Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  bake        Build from a file
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Authenticate to a registry
  logout      Log out from a registry
  search      Search Docker Hub for images
  version     Show the Docker version information
  info        Display system-wide information

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx
  compose*    Docker Compose
  container   Manage containers
  ...
```

>   **Recordar que:**
>   En todo momento, si tenemos dudas sobre algún comando o argumento podemos obtener ayuda con la sintaxis `docker COMANDO --help`.

---

# Tabla de contenido

- [Primeros pasos](./03.1-primeros-pasos.md)
- [Modos de ejecución](./03.2-modos-ejecucion.md)
- [Gestión de procesos](./03.3-gestion-procesos.md)
- [Transferencia de datos](./03.4-transferencia-datos.md)
- [Monitoreo y diagnóstico](./03.5-monitoreo-diagnostico.md)
- [Mantenimiento del sistema](./03.6-mantenimiento-sistema.md)
- [Variables de entorno](./03.7-variables-entorno.md)

[Siguiente: Redes](./04-redes.md)
