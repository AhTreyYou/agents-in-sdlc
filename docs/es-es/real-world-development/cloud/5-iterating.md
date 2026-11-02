---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funciones: documentar el código, crear una función de juegos relacionados y añadir mejoras de accesibilidad con controles para los modos de alto contraste y claro. Revisarás los cambios de código y, si es necesario, enviarás comentarios a Copilot para mejorarlos.

### Situación

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisa las sugerencias de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido restricciones para proteger el repositorio:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- De forma predeterminada, el [acceso a recursos externos está limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) del agente en la nube: añadir documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que Copilot está bloqueado por un firewall. Es normal: el acceso del agente a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para mostrar las PR.
3. Abre la PR titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que debería incluir comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera: proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se han añadido a la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR, igual que a otras personas del equipo, o añadir comentarios en línea en el código. Copilot utilizará esos comentarios para iniciar otra sesión y resolverlos. Como los resultados no son deterministas, no hay una instrucción concreta que debas utilizar. Algunas ideas:

- pedir comentarios de cabecera al principio de cada archivo de código;
- pedir comentarios TSDoc para archivos TypeScript y Astro;
- pedir un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario que pida un cambio a la documentación generada y menciona a **@copilot**. Puedes utilizar una de las ideas anteriores o proponer otra.
2. Selecciona **View Session** para observar cómo Copilot inicia otra sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve a la PR que Copilot ha creado para mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada algo parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación y, si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. Confirma que las comprobaciones del proyecto se han añadido y se superan. Puede tardar unos minutos.
8. **Opcional:** cambia a la rama de Copilot en tu Codespace para probar manualmente la función. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama de Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes pedirle cambios en un comentario de la pestaña **Conversation**, como ajustar el número de juegos que se muestran o añadir comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas por el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada algo parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Selecciona **Files changed** y revisa el código generado.
4. Comprueba especialmente:
   - los controles para cambiar de modo;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de CSS o de estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como etiquetas ARIA y navegación por teclado;
   - el código JavaScript/TypeScript que gestiona los cambios de modo.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación y, si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. Confirma que las comprobaciones del proyecto se han añadido y se superan.
8. **Opcional:** cambia a la rama de Copilot en tu Codespace y prueba manualmente las funciones de accesibilidad:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al recargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para que implemente estas funciones según las prácticas recomendadas. Si observas algún problema o mejora, menciona a **@copilot** en un comentario y solicita los cambios.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para delegar elementos reales de la lista de tareas y ocuparte de otra cosa mientras trabaja. Para practicarlo, crea algunas incidencias más y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar interés en una página de detalles del juego;
- implementar paginación en la página de listado de juegos;
- añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube** y observaste cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** para implementar controles de alto contraste y modo claro.
- **Utilizaste la página de agentes de Copilot como mission control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste e iteraste sobre las PR del agente**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Puedes complementar lo aprendido con estos otros recorridos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y una integración opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
