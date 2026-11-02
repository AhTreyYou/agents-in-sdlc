---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir modificar varios archivos y ejecutar tareas como las pruebas. Normalmente hay que localizar los archivos, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificar el código. Copilot en modo agente puede explorar el proyecto, realizar cambios coordinados, ejecutar tareas y corregir los errores que detecte.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios y revisión. El agente en la nube trabaja de forma asíncrona como otra persona del equipo, e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- dejar que el modo agente explore el proyecto, identifique archivos pertinentes y coordine los cambios;
- implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de incorporarlos al código.

## Situación

El catálogo de Tailspin Toys está creciendo y quieres que quienes visitan el sitio puedan filtrar por categoría y por editorial. En el ejercicio anterior ya creaste una función auxiliar para obtener editoriales; ahora completarás con Copilot en modo agente el resto de los cambios en la capa de datos y la interfaz, además de las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad actual.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz de cliente. Las funciones auxiliares reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante pequeños elementos `<script>` de Astro con las API estándar del DOM.

### Iniciar el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en una ventana del navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor web que se ejecuta en el codespace.

### Explorar el sitio web

El sitio web ofrece estas funciones principales:

- **Página de inicio**: muestra juegos de mesa con título, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de tareas con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará qué seleccionar.

La implementación inicial funciona, pero queremos añadir capacidades. La plantilla ya ha creado automáticamente una lista de incidencias; pide a Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el menú de agentes de Chat. Este agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos disponibles, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> El equipo autor de este taller no recomienda un modelo concreto. Durante su elaboración se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes, pero los resultados pueden variar porque los modelos de lenguaje son probabilísticos. Es normal que las sugerencias no coincidan exactamente con las indicadas en el taller.

> [!NOTE]
> Por su naturaleza probabilística, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre la lista de tareas:

   ```plaintext
   Muéstrame la lista de tareas pendientes del repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para las personas usuarias.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot ha priorizado los elementos según lo que considera más útil para quienes utilizan el sitio.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales, que incluyen:
   - una descripción de la arquitectura;
   - principios para diseñar componentes y crear código verificable y accesible;
   - enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar ejemplos y plantillas, y dividir instrucciones complejas en partes más manejables.

## Implementar el filtrado

Para completar el filtrado hay que hacer al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben superarse antes de combinar los cambios. Copilot en modo agente puede encargarse de estas tareas.

1. Continúa la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia relacionada de GitHub. Ya existe una función auxiliar de editoriales del ejercicio anterior; consérvala y ajústala si es necesario mientras completas el resto de los cambios en la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución; no hace falta volver a iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos de la capa de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Es posible que aparezcan los botones **Keep** y **Undo** en el editor; cuando termine, podrás aceptar o deshacer todos los cambios. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas podrían fallar durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguiente paso

Has utilizado GitHub Copilot en modo agente para añadir funciones al sitio de Tailspin Toys. Has aprendido que puede explorar el proyecto, identificar archivos pertinentes, coordinar cambios en la interfaz y la capa de datos, y generar pruebas que debes revisar antes de combinar el código.

A continuación, [probarás la funcionalidad con Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza lo aprendido para pedir a Copilot que implemente esta función. Sigue las prácticas recomendadas y los archivos de instrucciones existentes, decide si quien utiliza el sitio podrá seleccionar el tamaño de página o si será fijo y proporciona a Copilot el contexto necesario. Es posible que tengas que iterar y pedir cambios; es parte habitual del trabajo con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
