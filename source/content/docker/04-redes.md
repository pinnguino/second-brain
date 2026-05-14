---
id: 04-redes
aliases: []
tags: []
---

# Redes en Docker
En Docker, el objeto **Network** es el que permite que los contenedores se comuniquen entre sí de forma segura. El comando `docker network` es el que se utiliza para gestionar estas redes virtuales.

## Conceptos fundamentales
Por defecto, cuando instalamos Docker, se crean 3 redes automáticas:

- **Bridge**: La red predeterminada. Si no se especifica una, el contenedor se conecta aquí.
- **Host**: El contenedor comparte la red directamente con el sistema. Esto implica que no hay aislamiento de red en los contenedores.
- **None**: El contenedor no tiene interfaces de red (aislamiento total).

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
2a15540f199c   bridge    bridge    local
fd3214e45717   host      host      local
dc7bf88faa51   none      null      local
```

¿Qué son los drivers? Cuál es la diferencia?

## Las direcciones IP en la red `bridge`
Al crear contenedores trabajando en la red `bridge`, a todos se les asignará una dirección IP automáticamente, por defecto suele ser `172.17.0.x` con la que yo puedo trabajar.
Es más, con un contenedor ya creado, podemos obtener su dirección IP mediante el comando `docker inspect`:

```bash
$ docker inspect apache | grep IPAddress
                    "IPAddress": "172.17.0.3",
```

Con esa IP, podemos hacerle *ping*, y veremos que podemos llegar sin ningún problema.

```bash
$ ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3) 56(84) bytes of data.
64 bytes from 172.17.0.3: icmp_seq=1 ttl=64 time=0.069 ms
64 bytes from 172.17.0.3: icmp_seq=2 ttl=64 time=0.040 ms
64 bytes from 172.17.0.3: icmp_seq=3 ttl=64 time=0.037 ms
```

---

# El comando `docker network`
Las redes de contenedores se refieren a la capacidad de los contenedores para conectarse y comunicarse entre sí, así como con servicios de red ajenos a Docker.
Los contenedores tienen las funciones de red habilitadas de forma predeterminada y pueden establecer conexiones salientes. Un contenedor no tiene información sobre el tipo de red a la que está conectado, ni sobre si los demás elementos de la red son también contenedores de Docker. Un contenedor solo ve una interfaz de red con una dirección IP, una puerta de enlace, una tabla de enrutamiento, servicios DNS y otros detalles de red.

## Subcomandos esenciales
Al igual que con otros objetos de Docker, `network` tiene una estructura de gestión completa:

- `docker network ls`: Lista todas las redes creadas en el sistema.
- `docker network create <nombre>`: Crea una nueva red. <u>Normalmente, es de tipo bridge</u>.
- `docker network inspect <nombre>`: Muestra qué contenedores están conectados y qué IPs tienen asignadas.
- `docker network connect <red> <contenedor>`: Conecta un contenedor a que ya está corriendo a una red adicional.
- `docker network rm <nombre>`: Borra una red. Sólo se puede borrar una red si no tiene contenedores conectados.
- `docker network prune`: Borra todas las redes que no se estén usando. Esté comando está incluido en [`docker system prune`](./03.6-mantenimiento-sistema.md).

## ¿Por qué crear redes propias?
Cuando creamos una red propia, Docker habilita un **servidor DNS interno**, es decir, un servicio que resuelve automáticamente nombres de contenedores en direcciones IP para facilitar la comunicación entre ellos. Como si fuera una *guía telefónica

* **Sin red propia**: Tendríamos que conectar nuestras APIs usando la IP del contenedor, que cambia cada vez que se reinicia.
* **Con red propia**: Podemos usar el nombre del contenedor como si fuera una dirección web.

---

# Los puertos en Docker
Cuando creas o ejecutas un contenedor con los comandos `docker create` o `docker run`, se puede acceder a todos los puertos de los contenedores de las redes puente desde el host de Docker y desde otros contenedores conectados a la misma red. No se puede acceder a los puertos desde fuera del host ni, con la configuración predeterminada, desde contenedores de otras redes.
Podemos usar la opción `--publish` o `-p` para que un puerto esté disponible desde fuera del host y para los contenedores de otras redes puente.

## Ejemplo de trabajo con puertos
Supongamos que queremos montar un servidor web Apache. Para esto, hacemos un *pull* de la imagen que necesitemos de **httpd** en Docker Hub.
Viendo la [documentación de la imagen](https://hub.docker.com/_/httpd), vemos que el servidor Apache usa el puerto 80 para comunicarse.

**¿Y qué pasa si no tenemos disponible documentación para consultar qué puertos usa una imagen?**
Esto puede ser un problema ya que de no tener claro los puertos que usa una imagen, podemos tener dificultades para arrancar o crear contenedores.
Una posible solución a este problema sería inspeccionar la imagen con el comando `docker image inspect <imagen>` y buscar la propiedad `ExposedPorts`.

Para el caso de la imagen del servidor web Apache podemos obtener esta información de forma simple con el comando:

```bash
$ docker image inspect httpd | grep ExposedPorts -A 2
            "ExposedPorts": {
                "80/tcp": {}
            },
```

Aquí obtenemos los detalles de la imagen y luego mediante un pipe (`|`) le pasamos la salida al comando `grep` para que encuentre la cadena `ExposedPorts` y me devuelve la palabra junto las 2 líneas inmediatas que le siguen. Este comportamiento extra surge porque la salida del comando `inspect` en el JSON, por lo que la información del puerto no está en la misma línea.

Ejecutamos un contenedor del servidor Apache:

```bash
$ docker run --name apache -d -p 9000:80 httpd:trixie
```

Aquí crearemos un contenedor de nombre `apache` en modo *detached* (`-d`) y mapeando el puerto 9000 al 80. Esto quiere decir que los datos que entren por el puerto 9000 de la máquina real, se redirijan al puerto 80 del contenedor. Podemos verificar esto ejecutando `docker ps` para ver el estado de los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE          COMMAND              CREATED         STATUS         PORTS                                     NAMES
1127619485db   httpd:trixie   "httpd-foreground"   3 seconds ago   Up 3 seconds   0.0.0.0:9000->80/tcp, [::]:9000->80/tcp   apache
```

Aquí en la columna `PORTS` se ve que está mapeado el puerto 9000 del host al puerto 80 del contenedor, tanto como para IPv4 (`0.0.0.0`) como para IPv6 (`[::]`).

Alternativamente, con el comando `docker port`:

```bash
$ docker port apache
80/tcp -> 0.0.0.0:9000
80/tcp -> [::]:9000
```

> [!QUESTION] Pregunta
> ¿Se puede tener más de un servidor Apache en una máquina?
> Por supuesto. Debemos crear otro contenedor, con otro nombre, y mapear un puerto host distinto a los demás contenedores de `httpd`.

Para hacer la prueba, podemos acceder a un navegador web y acceder a `localhost:9000`, que se redigirá al puerto 80 del contenedor Apache.
Si todo salió bien, veremos una página en blanco con un texto que dice "It works!".

## Laboratorio: Despliegue de contenido personalizado
Una vez que el servidor Apache está corriendo y accesible, el siguiente paso es sustituir la página por defecto por nuestro propio desarrollo.

### 1. Localización del Document Root
En la imagen oficial de Apache (`httpd`), el directorio raíz donde el servidor busca los archivos para mostrar es: `/usr/local/apache2/htdocs/`

Por defecto, allí reside un archivo `index.html` con la leyenda "It works!".

### 2. Preparación de los archivos en el Host
Supongamos que tenemos una estructura web completa en nuestra máquina local:

```bash
$ ls -l
drwxrwxr-x assets/     # Imágenes y recursos
drwxrwxr-x css/        # Estilos
-rw-rw-r-- index.html  # Página principal personalizada
drwxrwxr-x js/         # Scripts
```

### 3. Transferencia de datos con `docker cp`
Para "inyectar" nuestra web en el contenedor sin necesidad de reconstruir la imagen o usar volúmenes, utilizamos la herramienta de [transferencia de archivos](./03.4-transferencia-datos.md) de Docker:

```bash
# Sintaxis: docker cp [ORIGEN_LOCAL] [NOMBRE_CONTENEDOR]:[DESTINO_CONTENEDOR]
$ docker cp . apache:/usr/local/apache2/htdocs
```

> [!NOTE] Nota
> El uso del punto (`.`) indica que copiaremos **todo** el contenido del directorio actual de nuestro Linux Mint hacia la carpeta `htdocs` del contenedor llamado `apache`.

### 4. Verificación final
Al refrescar el navegador en `localhost:9000`, Apache servirá automáticamente el nuevo `index.html` y sus activos (`css`, `js`, etc.), completando el ciclo de despliegue manual.

## Diferencia entre Puertos y Redes
Es común confundir "mapear puertos" con "redes".
En las **redes** (`--network`), se establece la comunicación **entre contenedores** (puerta interna).
En los **puertos** (`-p 80:80), se configura la comunicación **del mundo exterior (el host) al contenedor** (puerta externa).
