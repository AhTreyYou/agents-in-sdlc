---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información previa, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas para determinadas rutas;
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar para editoriales) con las instrucciones actuales;
- añadirás un estándar general al archivo `.github/copilot-instructions.md`;
- repetirás el mismo mensaje y observarás cómo el código generado sigue el estándar nuevo;
- confirmarás las instrucciones actualizadas y la parte del filtrado en `main` para que Copilot pueda utilizarlas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido, ya que Copilot no es determinista. El objetivo es observar la *tendencia* de los cambios de comportamiento al actualizar las instrucciones, no obtener siempre el mismo código.

## Archivos de instrucciones

### Situación

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su función.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor tu estilo y tus requisitos. Puedes especificar convenciones de código, bibliotecas y los comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y directrices generales.
- `.github/instructions/*.instructions.md`, archivos que proporcionan directrices para determinados lenguajes, tipos de archivo o tareas, como la creación de componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni para sugerir las siguientes ediciones.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones de otros estándares, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

No hay un único modo de crear archivos de instrucciones. Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo —como pruebas unitarias, componentes Astro o la capa de datos— o de tareas.
- Usa lenguaje natural, directrices claras y ejemplos del aspecto que debe y no debe tener el código.

Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la interfaz de usuario][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece la función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno que estés usando lo explicará cuando corresponda.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye el repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, fíjate en las directrices de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo` del principio. Define un patrón glob relativo a la raíz del repositorio; por ejemplo, coincide con los archivos TypeScript de pruebas que siguen el patrón `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones y a archivos existentes. Así se pueden dividir las instrucciones extensas en archivos pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás normas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crear una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará qué seleccionar.

Para ver el efecto de las instrucciones personalizadas, envía primero un mensaje con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. Selecciona **Agent** en el menú de agentes de Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales con su nombre e id. Aplica los cambios al archivo.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto a la función auxiliar como a sus pruebas.
6. Observa que la función propuesta tiene tipos, recibe el cliente `db` como primer argumento y devuelve un array de editoriales tipado. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios aunque no se le hayan pedido. No pasa nada; el objetivo es observar que la actualización de las instrucciones mejora la coherencia.

## Añadir un nuevo estándar al repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto. Añade un estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no incluye normas para documentar el código.
3. Debajo de los estándares existentes, añade estas líneas para pedir comentarios de cabecera y TSDoc:

   ```markdown
   - Todas las funciones exportadas deben incluir un comentario TSDoc que describa su propósito, sus parámetros y el valor que devuelven.
   - Antes de las importaciones o de cualquier otro código, añade un bloque de comentarios que explique la finalidad del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en la raíz (`.github/copilot-instructions.md`) para ofrecer directrices generales o en `*.instructions.md` para lenguajes, tipos de archivo y tareas concretas. El archivo general es el lugar adecuado para estándares de todo el proyecto, como la norma de documentación que acabas de añadir.

## Repetir el mensaje y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Vuelve a abrir `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo mensaje** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales con su nombre e id. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos que representan editoriales.
    */
   ```

Has indicado a Copilot un nuevo estándar del proyecto y lo has aplicado a código real, que servirá de base en el ejercicio siguiente.

## Confirmar las instrucciones y publicar la rama

Los archivos de instrucciones se gestionan con el mismo sistema de control de versiones que los demás archivos del repositorio. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que las instrucciones y la función auxiliar se han actualizado:

   ```bash
   git status
   ```

3. Añade y confirma las instrucciones y todos los archivos que Copilot haya modificado para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del estándar de documentación de la lista de tareas. Copilot puede buscar la incidencia, crear una PR que la cierre y, después, combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones. Más adelante aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia sobre las instrucciones y cree una PR desde la rama actual que describa tanto las instrucciones como la función auxiliar:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base de la función auxiliar de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si estás de acuerdo.
6. Después de crear la PR, pídele que la combine en `main` y que actualice la rama local:

   ```plaintext
   Combina la PR en main. Después, vuelve a main en local y descarga el código más reciente para actualizarlo.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si estás de acuerdo.

¡Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguiente paso

Has explorado cómo Copilot utiliza los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general al archivo `.github/copilot-instructions.md`;
- repetir el mismo mensaje y observar cómo el código propuesto sigue el estándar nuevo;
- confirmar las instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda continuar a partir de ahí.

A continuación, pondrás en práctica estas instrucciones con el [modo agente][next-lesson] para que Copilot añada una funcionalidad en varios archivos del proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
