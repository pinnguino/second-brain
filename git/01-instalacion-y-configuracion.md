---
id: instalacion-y-configuración
aliases: []
tags: []
---
# Instalación

A continuación se detallan los pasos para instalar git:
- Descargar git desde internet mediante un instalador (Opción más fácil para Windows) o desde tu gestor de paquetes preferido.
Luego de instalar git, podremos utilizar la herramienta desde nuestro terminal favorito.

---

# Configuración

Es necesario como configuración inicial configurar nuestro alias y correo electrónico, de manera que git pueda atribuir los cambios que realicemos en nuestros repositorios.
El alias a usar en nuestros repositorios, tiene 3 tipos de alcance:
- **Local**: Tiene alcance sólo en un repositorio. Para usar este alcance, deberemos estar adentro de un repositorio.
- **Global**: Tiene alcance a todos los repositorios alojados en un usuario.
- **Sistema**: Tiene alcance en toda la computadora (múltiples usuarios).

> [!note]
> El alcance más común para usar inicialmente es el alcance global. Si se necesita algo más específico para un repositorio se podrá configurar más adelante.

### Alias

```bash
$ git config --global user.name "Jaimito"
```
Aquí para todos nuestros repositorios en nuestro usuario, Nuestro alias será *Jaimito*.

> [!warning]
> Si tenemos configuraciones en distintos alcances, siempre git le dará prioridad a la configuración de alcance más pequeño.

### Dirección de correo electrónico

```bash
$ git config --global user.email "jaimito@domain.com"
```
Aquí para todos nuestros repositorios en nuestro usuario, nuestro correo será jaimito@domain.com

> [!tip]
> Con el comando `git config --list` podremos ver el estado de nuestra configuración de git.

### Editor

Cuando hagamos un cambio y lo queramos subir al repositorio, deberemos escribir cuáles son esos cambios.
El editor nos servirá para escribir esos cambios, o resolver ciertos conflictos, por lo que deberemos configurarlo. El editor puede ser cualquier editor de texto. El recomendado es [Visual Studio Code](https://code.visualstudio.com).

```bash
$ git config --global core.editor "code --wait"
```
El parámetro `--wait` es usado para que la ventana que invoca a Visual Studio Code espere a que el programa se cierre para continuar con la ejecución.

### Coloreado de sintaxis

```bash
$ git config --global color.ui true
```
De esta manera, las salidas de los comandos de git se verán más coloridas, mejorando la legibilidad.

### Auto CRLF

Esta configuración es necesaria ya que en windows, para hacer el salto de línea y volver al principio, se usan 2 caracteres: \n y \r.
En sistemas basados en UNIX sólo se usa \n, por lo que para evitar errores de compatibilidad al trabajar con un repositorio con varios sistemas operativos.

```bash
# En windows
$ git config --global core.autocrlf true

# En sistemas basados en UNIX
$ git config --global core.autocrlf input
```

[Siguiente: Primeros Pasos](02-primeros-pasos.md)
