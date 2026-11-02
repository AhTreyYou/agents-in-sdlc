---
title: "Lección 1: Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso es instalar la herramienta; por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm;
- te autenticarás con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación rápida;
- localizarás la incidencia de filtrado mediante el servidor GitHub MCP integrado.

## Situación

Tu equipo está empezando a utilizar agentes de IA para abordar una lista de trabajo pendiente cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde ya trabajan muchas personas desarrolladoras. En esta lección instalarás y autenticarás la herramienta para poder utilizarla durante el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   La versión debe ser 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autenticarse con GitHub

La primera vez que se inicia Copilot CLI, te pide que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación con un código de dispositivo y autoriza Copilot CLI.
3. Copilot CLI mostrará este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde a tu repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace, puede que ya te hayas autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está listo.

## Familiarizarse con Copilot CLI

Los comandos que escribes en el terminal normal se ejecutan directamente en el Codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la pantalla de ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor GitHub MCP integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Utiliza GitHub MCP para buscar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Indícame su URL y un breve resumen. No hagas cambios.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Utilizar el acceso directo del taller

Normalmente, Copilot CLI pregunta antes de utilizar herramientas fuera de los permisos establecidos. Para este taller, lo iniciarás con `--yolo`, un acceso directo aprobado por el usuario que elimina esas solicitudes de aprobación dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` activa todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Utilízalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo configures como opción predeterminada para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

La opción `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que utilizarás más adelante para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo, pero los recursos de GitHub a los que te autenticas son reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de funcionalidad.

## Resumen y siguiente paso

Has instalado GitHub Copilot CLI con npm, te has autenticado, has confiado en el repositorio del taller, has probado una conversación y has localizado la incidencia de filtrado mediante GitHub MCP.

A continuación, [empezarás con un cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
