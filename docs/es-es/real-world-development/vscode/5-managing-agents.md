---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y ediciones a medida que ocurren. Puedes revisar cada diferencia, aceptar o rechazar los cambios y orientar la conversación con mensajes de seguimiento, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- pedirás al agente que añada un control para el modo claro, además del modo de alto contraste.

## Situación

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita.
3. Cuando proponga una edición:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para deshacerlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Verás un mensaje final que resume los cambios.

## Pedir al agente que añada el modo claro

Una vez añadido el modo de alto contraste, ampliarás la misma conversación con un control de modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo: los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

   ```plaintext
   ¡Buen trabajo! Ahora añade un control para el modo claro junto al del modo de alto contraste. El modo claro debe seguir el mismo patrón: conservar la selección en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el de alto contraste de forma independiente.
   ```

2. El agente continuará desde donde lo dejó, explorará sus cambios anteriores y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nueva edición y selecciona **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si reorganiza los controles de una forma que no te gusta—, deténlo con el botón **Stop** junto al campo de mensaje y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza mensajes de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los archivos modificados

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguiente paso

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección aprendiste a supervisar el trabajo del agente en tiempo real, aceptar o rechazar ediciones concretas y orientar una sesión con mensajes de seguimiento. En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
