---
title: "Lección 2: Añadir valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación específica de Copilot en una rama de funcionalidad;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para comprobarlo en un navegador reenviado;
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Situación

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, quien lo publica y la descripción. Para empezar, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación de Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que están seleccionados en el terminal, en vez de crear un árbol de trabajo específico. Las conversaciones guardadas te permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus mensajes y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Inicia una conversación para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inicia Copilot con el comando siguiente:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Solicita el cambio con este mensaje:

   ```plaintext
   Muestra la valoración starRating de cada juego sobre 5 en las tarjetas de la página de listado. Si la valoración es null, muestra "No rating yet". Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisar las diferencias

Debes revisar todos los cambios generados por IA antes de combinarlos, aunque sean pequeños. Examínalos en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot ofrece resultados probabilísticos, no deterministas, por lo que el código exacto puede variar. Revisa el comportamiento en vez de esperar una implementación concreta.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz de usuario (UI) en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de las valoraciones con estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abrirás una solicitud de incorporación de cambios (PR) para combinar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones con estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI sube la confirmación existente al crear la PR y muestra su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguiente paso

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación específica de Copilot en una rama de funcionalidad;
- pediste al agente que hiciera un pequeño cambio en las tarjetas de juegos;
- revisaste el cambio con `/diff`;
- ejecutaste la aplicación para comprobar la valoración en un navegador reenviado;
- abriste y combinaste tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
