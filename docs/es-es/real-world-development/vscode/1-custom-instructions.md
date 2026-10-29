---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Ejercicio siguiente: Modo de agente →][next-lesson]

El contexto es esencial cuando trabajas con inteligencia artificial generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información previa, asegúrate de que pueda acceder a ese contexto. Hay varias formas de compartir contexto específico con Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para determinadas rutas.
- generarás la primera parte de datos para el filtrado (un asistente para editoriales) con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a enviar el mismo mensaje y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás en `main` la actualización de instrucciones y la parte de filtrado para que Copilot pueda usar las directrices actualizadas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que definas. Copilot es no determinista. El objetivo es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código que en los ejemplos.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo, entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Puedes especificar convenciones de código, bibliotecas e incluso los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para determinados tipos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes de este repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes de chat o de interfaz de línea de comandos (CLI), como la pila tecnológica, una descripción general de lo que se está creando, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`, archivos que puedes crear para tareas o tipos de archivo concretos. Puedes usarlos para definir directrices de lenguajes específicos, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o nuevas pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot [también admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Procedimientos recomendados para gestionar archivos de instrucciones

Este taller no cubre en detalle cómo crear archivos de instrucciones. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como su descripción, estructura y estándares globales de código.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes de Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma correcta de crear archivos de instrucciones, al igual que no hay una única forma de usar la inteligencia artificial. Descubrirás mediante la experimentación qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los de este proyecto, puede que encuentres archivos para distintos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente indicará cómo hacerlo.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Estas se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Encontrarás instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript; por ejemplo, con los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos reutilizables más pequeños e indicar a Copilot dónde encontrar ejemplos. Las rutas que aparecen ahí son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige incluir documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Crea una rama para los cambios

Crea una rama antes de empezar a modificar los archivos.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

Para observar el efecto de las instrucciones personalizadas, empieza enviando un mensaje con el archivo de instrucciones actual. Más adelante, actualizarás el archivo y volverás a enviar el mismo mensaje para comparar el resultado.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los pasos siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que necesites.

1. Cierra las pestañas del editor que sigan abiertas de ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el asistente.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, a menudo tanto al archivo del asistente como a sus pruebas.
6. Observa que el asistente propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esta estructura procede de las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade ahí los estándares de código del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de código, pero aún no contiene una regla para documentar el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade estas líneas de Markdown justo debajo de los estándares existentes para pedir a Copilot que incluya encabezados de comentarios de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en el ejercicio anterior, puedes crear archivos de instrucciones en la raíz del repositorio (`.github/copilot-instructions.md`) para definir directrices globales, o como archivos `*.instructions.md` para determinados lenguajes, tipos de archivo o tareas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a enviar el mensaje y observa el cambio

Vuelve a generar el asistente con el mismo mensaje para observar cómo influye la nueva directriz.

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo mensaje** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Asistentes de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código real sobre el que se basará el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan mediante el control de código fuente, igual que los demás archivos. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están presentes las actualizaciones de instrucciones y del asistente ejecutando:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para crear la base del asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios auxiliares (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Después de publicar la rama, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre los estándares de documentación. Puedes hacerlo manualmente, pero Copilot puede buscar la incidencia, crear la PR y cerrarla al combinarla.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub al área de trabajo, así que no tienes que configurar nada. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto la actualización de instrucciones como la nueva base del asistente de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si estás de acuerdo.
6. Cuando se cree la PR, pide a Copilot que la combine y que vuelva a la rama `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si estás de acuerdo.

¡Has creado y combinado una solicitud de incorporación de cambios con la ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto incorpora el nuevo estándar.
- confirmar en `main` la actualización de instrucciones y la base del asistente para que el ejercicio siguiente pueda continuar el trabajo.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
