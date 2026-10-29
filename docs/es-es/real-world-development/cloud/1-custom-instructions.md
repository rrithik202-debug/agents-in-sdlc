---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Ejercicio siguiente: Agente en la nube de Copilot →][next-lesson]

El contexto es esencial cuando trabajas con inteligencia artificial generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información previa, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para determinadas rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás un mensaje anterior y otro posterior. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente en la nube.

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

Lee los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Estas se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Encontrarás instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript; por ejemplo, con los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos reutilizables más pequeños e indicar a Copilot dónde encontrar ejemplos. Las rutas que aparecen ahí son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige incluir documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Añade un estándar para todo el repositorio

El siguiente paso es la única edición que harás aquí: añadir una regla para incluir documentación en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube aplicará esta directriz a las incidencias que le asignes más adelante.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás los cambios de forma ordenada.

1. En el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade ahí los estándares de código del repositorio para mejorar las sugerencias.

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

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el próximo ejercicio, asignarás una incidencia a Copilot, que creará una rama a partir de `main`. Por tanto, los cambios de instrucciones deben llegar a `main` para que el agente en la nube pueda usarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá leer estas instrucciones al trabajar en las incidencias que le asignes en el próximo ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar el cambio directamente en `main`. La rama permite practicar un flujo de trabajo más parecido al de un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has añadido un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto de este entorno.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
