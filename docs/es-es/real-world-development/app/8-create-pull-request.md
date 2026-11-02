---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- examinarás la PR completa de la funcionalidad y las pruebas de verificación;
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR está combinada.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios desde la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la PR y resuelve los obstáculos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reaplica los cambios sobre la rama de destino cuando es necesario. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se combina la PR.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad, pero su capacidad de editar código y combinar sigue necesitando tu autorización explícita. Revisa sus acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utilizar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora dejarás que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable por sí solo no inicia el proceso.

Se iniciará Agent Merge, que realizará estas tareas:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, incluir una referencia a ella en el cuerpo de la descripción.
- Reaplicar los cambios sobre la rama de destino o resolver posibles conflictos con esa rama.
- Supervisar el proceso de CI y asegurarse de que se superen todas las comprobaciones.
- Supervisar la PR por si otras personas desarrolladoras o la revisión de código de Copilot dejan comentarios, y actualizar el código para resolverlos.
- Combinar automáticamente la PR cuando todo se haya completado correctamente, si le das permiso.

Ahora dejarás que Agent Merge combine la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** está marcada.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Combina la PR cuando se superen todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
   ```

9. Supervisa el progreso y resuelve cualquier bloqueo que se indique. Confirma que el estado de la PR es **Merged** antes de continuar; tener permiso para combinar o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo, como la generación, las pruebas y la validación de código, y ahora también el proceso de solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has examinado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR estaba combinada.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app