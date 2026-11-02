---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definir agentes en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Así, los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices [WCAG][wcag], uno de seguridad que siga prácticas seguras de programación o uno de pruebas que utilice patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con al menos los campos `name` y `description`, seguido de una instrucción Markdown que define el comportamiento, los conocimientos y las directrices del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen en algunos aspectos: ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar tareas. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad y podría utilizar habilidades como una que gestione solicitudes de incorporación de cambios o ejecute pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que se adapten al entorno y a las situaciones de tu equipo.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunas personas usuarias indican que el tema oscuro resulta difícil de leer porque hay poco contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, utilizarás un agente personalizado para implementar esta función cuanto antes. En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- lo utilizarás en Copilot Chat para implementar el modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revísalo para saber cómo orientará a Copilot.

Vuelve a tu codespace, abre un terminal y crea una rama nueva desde `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que incluyen:
   - las responsabilidades principales al generar código para un sitio accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código para HTML, CSS y JavaScript;
   - errores frecuentes que conviene evitar.

## Utilizar el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para que la sesión utilice sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará qué seleccionar.

1. Selecciona **Agent** en el menú de agentes de Chat, si aún no está seleccionado.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú de agentes de la parte inferior de la vista Chat. Muestra el agente activo, que por defecto es **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía al agente este mensaje:

   ```plaintext
   Añade al sitio un modo de alto contraste. Debe haber un control que permita activarlo y desactivarlo, y la selección debe persistir entre recargas de la página mediante el almacenamiento local del navegador.
   ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para revisarla.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot hará cambios reales en el repositorio y puede editar componentes Astro, CSS y pruebas relacionadas.

En el ejercicio siguiente revisarás y dirigirás el trabajo mientras se realiza.

## Resumen y siguiente paso

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados para tareas y áreas concretas. Te permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En este ejercicio aprendiste cómo se definen los agentes personalizados y cómo se utilizan en el modo agente de Copilot Chat. A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
