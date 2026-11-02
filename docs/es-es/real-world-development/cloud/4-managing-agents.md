---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia y añadir funciones. Aunque trabaja de forma autónoma y asíncrona, es importante que puedas supervisar su trabajo.

Puedes gestionar las tareas asignadas al agente en la nube con varias herramientas, incluida la [página de agentes][agents-page] de GitHub.com. Desde este espacio central puedes consultar las tareas con solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas y orientar las sesiones que siguen en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para pedir una función adicional.

## Situación

Después de asignar al agente el modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo un control, es lógico incorporar ambas funciones. Orientarás el trabajo del agente para que añada el modo claro además del de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Consulta el estado de las tareas asignadas al agente en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot, por ejemplo:
   - descargar el código del repositorio;
   - preparar el entorno de trabajo;
   - configurar servidores MCP;
   - completar distintos pasos de la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha, donde puedes consultar la PR y los archivos modificados.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto variará según el nombre que haya elegido Copilot y el estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos, hasta que la sesión indique que Copilot ha terminado de preparar el entorno y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber otro control para que cada persona pueda seleccionar el modo de visualización que prefiera.
   ```

   ![Captura de la tarea del agente en la nube con el cuadro de diálogo para dirigir la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora a su trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud después de completar el paso en el que estuviera trabajando cuando enviaste el mensaje.

El proceso tardará varios minutos; puedes aprovechar para repasar lo aprendido hasta ahora.

## Resumen y siguiente paso

Has explorado la página de agentes de Copilot, el espacio central para supervisar y orientar las tareas del agente en la nube. Puedes realizar el seguimiento de tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o proporcionar más indicaciones.

En esta lección has supervisado tareas desde mission control y has redirigido una sesión para solicitar otra función. A continuación, [iterarás sobre las solicitudes de incorporación de cambios de Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
