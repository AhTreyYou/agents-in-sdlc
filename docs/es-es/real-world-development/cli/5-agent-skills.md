---
title: "Lección 5: Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para asegurarte de que sigue los estándares. ¿Y las pruebas, el lint y el resto de las tareas de integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo debe ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Situación

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. Una habilidad consiste principalmente en una carpeta con un archivo Markdown llamado `SKILL.md`. El archivo incluye frontmatter con el nombre y la descripción de la habilidad, una descripción general de su función y directrices sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas con el equipo como parte del repositorio, o en la carpeta global de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks`, que el equipo de Tailspin Toys ha creado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Comprueba que la habilidad todavía no incluye una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes llamar a las habilidades directamente desde Copilot CLI o pedírselo en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Escribe:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe que aparece al final.

## Personalizar el informe

Mejoraremos el informe para que indique qué se ejecutó, si terminó correctamente y qué resultados comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al terminar, indica los comandos que se han ejecutado y si se han superado, han fallado o se han bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta las comunica. Indica el siguiente paso ante cualquier error o bloqueo y nunca presentes como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguiente paso

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- personalizaste el formato de sus resultados;
- volviste a cargar la habilidad y la ejecutaste.

Este cambio se incluirá con el filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
