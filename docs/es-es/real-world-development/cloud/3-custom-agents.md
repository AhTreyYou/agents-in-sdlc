---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definir agentes en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos.

Cada perfil tiene frontmatter YAML con al menos los campos `name` y `description`, seguido de instrucciones que definen el comportamiento y los conocimientos del agente. Así, los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique las directrices [WCAG][wcag], uno de seguridad que siga prácticas seguras de programación o uno de pruebas que utilice patrones coherentes.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen en algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar tareas. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las directrices y podría utilizar habilidades como una que gestione solicitudes de incorporación de cambios o ejecute pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar lo que funciona en tu entorno y tus situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunas personas usuarias indican que el tema oscuro resulta difícil de leer porque hay poco contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, utilizarás un agente personalizado para implementar esta función cuanto antes. En este ejercicio aprenderás cómo se definen los agentes personalizados y cómo se les asigna una tarea.

## Revisar el agente personalizado de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revísalo para saber cómo orientará a Copilot.

1. Vuelve a tu codespace y abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que incluyen:
   - las responsabilidades principales al generar código para un sitio accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código para HTML, CSS y JavaScript;
   - errores frecuentes que conviene evitar.

## Crear y asignar una incidencia

Mission control es el espacio central para trabajar con agentes. Puedes asignar tareas al agente en la nube, supervisarlas y proporcionar indicaciones adicionales. Vamos a asignarle una tarea para crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. En **Description**, escribe:

   ```plaintext
   Necesitamos un modo de alto contraste para el sitio. Debe haber un control que permita activarlo y desactivarlo, y la selección debe guardarse en el almacenamiento local del navegador.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Captura de la asignación del agente en la nube, con los campos de agente personalizado y accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguiente paso

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados para tareas y áreas concretas. Te permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En esta lección has aprendido cómo se definen los agentes personalizados y cómo se asigna una tarea a uno. Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para utilizar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para utilizar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
