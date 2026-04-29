---
id: 02-instalacion
aliases: []
tags: []
---

# Instalación de Docker
Instalar Docker tiene ciertas consideraciones según el sistema en el que te encuentras a la hora de usarlo.
Veamos las diferencias.

## Docker Engine vs. Docker Desktop

| Característica | Docker Engine | Docker Desktop |
| :--- | :--- | :--- |
| **Naturaleza** | Nativo de Linux. Es el motor central sin interfaz gráfica. | Aplicación de escritorio con interfaz gráfica (GUI). |
| **Componentes** | El daemon (`dockerd`), la CLI y los runtimes. | Incluye Docker Engine, Kubernetes, Dashboard y herramientas de gestión. |
| **Funcionamiento** | Corre directamente sobre el Kernel de Linux. | Corre dentro de una máquina virtual (incluso en Linux). |
| **Consumo de recursos** | Muy bajo y eficiente. | Mayor (debido a la virtualización y la GUI). |

### ¿Cuál elegir según el entorno?

* **Servidores Linux:** **Docker Engine** es la única opción lógica. Máximo rendimiento y estabilidad.
* **Windows / macOS:** **Docker Desktop** es el estándar, ya que facilita la configuración de la virtualización necesaria en estos sistemas.
* **Escritorio Linux (Desarrollo):** Aquí se puede elegir. **Docker Engine** si se prefiere ligereza y terminal, o **Docker Desktop** si se busca una interfaz visual para gestionar contenedores y un clúster de Kubernetes integrado con un solo clic.

---

# Consideraciones para Docker Engine en Linux
En este caso opto por instalar Docker Engine.
Hay una consideración especial a tener en cuenta si instalas Docker Engine en la computadora que usas para otros propósitos que no sea un servidor.
Al instalar **Docker Engine** en Linux, éste se configura por defecto para arrancar como un servicio del sistema (`systemd`) apenas enciendes la computadora.
Esto hace que cuando no se esté utilizando, Docker consumirá recursos incluso cuando no estés gestionando estos contenedores de forma activa.

Para evitar esto, a continuación se muestra cómo configurar el entorno para que Docker consuma recursos cuando lo decidas.

### 1. Evitar que Docker inicie con el sistema
Lo primero es decirle a Linux que no inicie Docker automáticamente al arrancar.
Para hacer esto, simplemente se ejecutan estos dos comandos en la terminal:

```bash
# Deshabilita el inicio automático del servicio de Docker
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```
*Nota: Esto no borra Docker, solo impide que se "despierte" solo al encender el portátil.*

---

### 2. El "Interruptor" Manual
Ahora que Docker está "dormido", se pueden usar estos comandos cuando decidas que es hora de practicar:

**Para encender Docker:**
```bash
sudo systemctl start docker
```

**Para apagarlo completamente (y liberar RAM):**
```bash
sudo systemctl stop docker
sudo systemctl stop containerd
```

---

### 3. Automatización: Crear tus propios comandos (Recomendado)
Estar escribiendo comandos largos de `systemctl` es tedioso. Puedes crear "alias" en tu archivo de configuración de la shell (probablemente `.bashrc` o `.zshrc`).

1. Abre el archivo con tu editor
2. Ve al final del archivo y pega esto:

```bash
# Alias para gestionar Docker bajo demanda
alias docker-on='sudo systemctl start docker && echo "Docker encendido 🚀"'
alias docker-off='sudo systemctl stop docker containerd && echo "Docker apagado 😴"'
alias docker-status='systemctl status docker'
```
3. Guarda y refresca tu terminal con `source ~/.bashrc`.

**A partir de ahora, solo tendrás que escribir `docker-on` para empezar a practicar y `docker-off` cuando vuelvas a tu trabajo habitual.**

---

### 4. ¿Qué recursos se liberan realmente?
Cuando ejecutas `docker-off`, estarás deteniendo dos procesos principales:

1. **`dockerd`**: El demonio que gestiona la API y los contenedores.
2. **`containerd`**: El motor de ejecución de bajo nivel.

Además, al detener estos servicios, te aseguras de que cualquier contenedor que hayas dejado con la bandera `--restart always` o `--restart unless-stopped` también se detenga y no consuma ciclos de CPU en segundo plano mientras trabajas.

**Dato extra:** Si después de una sesión de práctica quieres limpiar no solo los procesos, sino también el espacio en disco de contenedores basura que quedaron por ahí, puedes usar:
`docker system prune` (Esto borra contenedores detenidos y redes que no se usen).

---

# Instalación de Docker Engine en Linux

> En la página oficial de Docker están las instrucciones de instalación para todos los sistemas soportados oficialmente.
> → [Instrucciones de la página oficial aquí](https://docs.docker.com/engine/install). ←

Esta guía adapta las instrucciones oficiales de Docker para **Ubuntu** específicamente para **Linux Mint** y otras distribuciones derivadas. El punto clave es que estas distribuciones no son compatibles oficialmente, por lo que debemos "forzar" al sistema a usar los paquetes de la base Ubuntu correspondiente.

## 1. Requisitos de Sistema y Arquitectura
Docker Engine es compatible con arquitecturas **x86_64 (amd64)**, **armhf**, **arm64**, **s390x** y **ppc64le**. Asegúrate de usar una versión de Mint basada en una de estas versiones de Ubuntu:
* **Ubuntu 24.04 (Noble)** -> Linux Mint 22.x
* **Ubuntu 22.04 (Jammy)** -> Linux Mint 21.x

## 2. Limpieza de Conflictos
Es obligatorio desinstalar paquetes extraoficiales que puedan entrar en conflicto con la versión oficial de Docker.

```bash
sudo apt remove docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc
```

## 3. Configuración del Repositorio (Método APT)
Este es el método recomendado para recibir actualizaciones automáticas.

### A. Preparar claves GPG
Instalamos las herramientas necesarias y añadimos la clave oficial para validar los paquetes.
```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### B. Registrar el Repositorio (Ajuste para Mint)
En las instrucciones oficiales, se usa `$(. /etc/os-release && echo "$VERSION_CODENAME")`, pero en Mint esto devolvería `wilma` o `virginia`, nombres que el servidor de Docker no reconoce. 

Usamos `UBUNTU_CODENAME` para que el comando funcione en cualquier derivada basada en Ubuntu:
```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

## 4. Instalación de Paquetes
Instalamos el motor, la interfaz de línea de comandos (CLI) y los plugins necesarios.
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 5. Verificación
Docker suele iniciarse automáticamente tras la instalación. Para confirmar que todo está correcto:
```bash
sudo docker run hello-world
```
Este comando descarga una imagen de prueba, la ejecuta en un contenedor y muestra un mensaje de éxito.

---

## 💡 Notas Post-Instalación

### Gestión sin Root (Opcional)
Si recibes errores de permisos al ejecutar comandos sin `sudo`, es porque el tu usuario personal no tiene acceso al daemon `dockerd`.
Para solucionar esto se puede trabajar como usuario `root` (no recomendable para entornos de producción), o agregar el usuario con el que trabajarás al grupo de docker.
Para añadir tu usuario al grupo `docker`:
```bash
# Si quieres crear un usuario nuevo:
sudo adduser dockerdev

# Agregarlo al grupo docker:
sudo usermod -aG docker $USER
```
*Debes cerrar sesión y volver a entrar para que este cambio surta efecto.*

### ⚠️ IMPORTANTE ⚠️

**El "Poder Oculto" del nuevo usuario**: Es importante que entiender que, aunque `devuser` no esté en el grupo `sudo`, ahora <u>técnicamente es un administrador encubierto</u>.

Por ejemplo, ese usuario podría ejecutar:
```bash
docker run -v /:/host_root -it ubuntu bash
```

Ese comando arranca un contenedor que "monta" todo tu disco duro real dentro del contenedor. Una vez dentro, el usuario puede ver y borrar tus fotos, cambiar contraseñas de otros usuarios o leer archivos privados de `/root`, todo esto sin conocer la contraseña de `root` ni estar en **sudoers**.

### La solución: Modo *rootless*
El modo Rootless es la respuesta de Docker a los problemas de seguridad que comentamos antes. Básicamente, permite ejecutar el demonio de Docker y los contenedores sin privilegios de root.
En este modo, incluso si alguien logra "escapar" del contenedor, solo tendrá acceso a los archivos del usuario que lanzó el proceso, no a todo el sistema operativo.
Puedes buscar más información en el sitio oficial de Docker [aquí](https://docs.docker.com/engine/security/rootless).

### Limitaciones de Firewall
Ten en cuenta que si usas **ufw**, Docker suele saltarse las reglas de firewall al exponer puertos de contenedores. Asegúrate de configurar tus reglas en la cadena `DOCKER-USER` si necesitas filtrado estricto.

### Desinstalación Total
Si necesitas limpiar la instalación por completo:
1. Purga los paquetes: `sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`.
2. Borra datos de contenedores/imágenes: `sudo rm -rf /var/lib/docker` y `/var/lib/containerd`.
3. Elimina el repositorio y la clave: `sudo rm /etc/apt/sources.list.d/docker.sources` y `/etc/apt/keyrings/docker.asc`.

[Siguiente: Contenedores](./03-contenedores.md)
