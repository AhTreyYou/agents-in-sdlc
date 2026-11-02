---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen problemas con la deuda técnica: incidencias de seguridad pendientes, código antiguo que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en la lista de tareas. El agente en la nube de GitHub Copilot puede actualizar código y añadir funciones de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en el trabajo de mayor alcance.

Con el agente en la nube:

- personalizarás el entorno de generación de código;
- aprenderás a asegurarte de que las operaciones se realicen de forma segura;
- conocerás la importancia de definir incidencias con un alcance claro;
- asignarás incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica que quiere abordar. La documentación que dejaron quienes desarrollaron la primera versión del sitio es deficiente: verás que no hay ninguna. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para explorar la experiencia y realizar pruebas de aceptación. No necesitan una implementación pulida todavía. La tarea está bloqueada, pero hay otras incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas con facilidad y son adecuadas para asignarlas al agente en la nube. Mientras Copilot trabaja de forma asíncrona, puedes ocuparte de otra tarea y volver después para revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a una persona desarrolladora. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después, Copilot crea un borrador de solicitud de incorporación de cambios para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes revisar la sesión mientras se ejecuta o una vez completada. Cuando la solución esté lista para revisarse, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no es magia: no puedes saltarte el análisis. Hasta las tareas que parecen sencillas pueden ser complejas, así que sigue las [prácticas recomendadas para acotar tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como a una persona que programa contigo: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

Los ejercicios anteriores ya han presentado las instrucciones personalizadas. Si has completado el ejercicio práctico, ya has visto `.github/copilot-instructions.md` en funcionamiento. Si no, consulta brevemente los archivos incluidos antes de asignar trabajo a Copilot para poder observar su efecto después.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, respeta las convenciones de TypeScript y añade comentarios de cabecera. Estas prácticas proceden de los archivos de instrucciones; las volverás a revisar al analizar las PR más adelante.

## Configurar el entorno de desarrollo del agente

La creación de código suele requerir herramientas y scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot: trabaja como un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. El flujo de trabajo del taller ya está configurado para instalar Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes confirmar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Muchos proyectos tienen documentación desactualizada o carecen de ella. Esa deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevas personas al equipo. Copilot puede ayudar a crear documentación; así que esta es una buena incidencia para delegar. El agente trabajará en segundo plano y revisarás sus cambios en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. En **Description**, escribe:

   ```plaintext
   Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios donde aclaren el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana.
12. Pasados unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno —ejecutando el flujo de trabajo anterior—, analizando el proyecto y definiendo el enfoque.

13. Revisa las tareas que Copilot va a completar.
14. Desplázate por la cronología de la PR y comprueba que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Explora la sesión en directo para ver cómo Copilot examina el código, analiza el estado, planifica el trabajo y crea código.

El proceso puede tardar varios minutos. Una ventaja importante del agente en la nube es que trabaja de forma asíncrona, lo que te permite ocuparte de otras tareas. Aprovecharemos esa función y, mientras asignamos otra incidencia, escribiremos código para añadir una función a la aplicación.

## Añadir una sección de juegos relacionados

El agente en la nube puede trabajar en paralelo mientras tú te ocupas de otras tareas. Aunque añadir una sección de juegos relacionados no sea un trabajo muy largo, puedes aprovechar ese tiempo para otra actividad. Asigna también esta tarea al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. En **Description**, escribe:

   ```markdown
   Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir en `src/lib/` una función auxiliar de acceso a datos que devuelva los demás juegos de la misma categoría que un juego, sin incluir ese juego.
   - Mostrar una sección «Juegos relacionados» en la página de detalles del juego que utilice la función auxiliar.
   - Gestionar el caso en que un juego no tenga juegos relacionados.
   - Crear pruebas unitarias para la nueva función auxiliar.
   - Antes de crear la PR, asegurarse de que se superen todas las pruebas.
   ```

6. Selecciona **Create** para crear la incidencia.
7. A la derecha, selecciona **Assign to Copilot** y, después, **Assign**.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

Al poco tiempo, aparecerá 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que trabaja en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. No es necesario supervisarlo continuamente; revisa el resultado cuando termine. A continuación, crearás y utilizarás agentes personalizados.

## Resumen y siguiente paso

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents], que permite asignar incidencias para que Copilot las resuelva de forma asíncrona. Puede ayudar con deuda técnica, nuevas funciones o migraciones entre frameworks.

En este ejercicio has personalizado el entorno de generación, revisado las operaciones seguras, aprendido a definir incidencias claras y asignado trabajo a Copilot. Mientras el agente trabaja, puedes [crear y utilizar agentes personalizados][next-lesson]. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
