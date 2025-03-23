# Mis notas académicas

Página desplegada: https://pinnguino.github.io/second-brain

## Requisitos
Instalar `npm`
Desde Linux, instala `npm` desde tu gestor de paquetes. Casi seguro que estará en los repositorios.
Desde Windows, descarga el instalador desde la [página de Node.js](https://nodejs.org/en/download).

## Basic setup

Tutorial: https://dev.to/defenderofbasic/host-your-obsidian-notebook-on-github-pages-for-free-8l1.
Para generar el HTML localmente, ejecuta `npx quartz build --serve` en la carpeta `./source/`.

## Páginas raw HTML

Hay una carpeta [raw_html](./source/raw_html) que se copia en la carpeta build. Esto te permite hostear HTML arbitrario fuera de Quartz. Ejemplo: https://defenderofbasic.github.io/obsidian-quartz-template/raw-html-test.html

## Personalización

Quartz está hecho para ser muy personalizable, incluso si no sabés programar. Casi todas la configuración que necesitarás estará en el archivo [quartz.config.ts](./source/quartz.config.ts), o  cambiando el diseño en [quartz.layout.ts](./source/quartz.layout.ts).

https://quartz.jzhao.xyz/configuration
