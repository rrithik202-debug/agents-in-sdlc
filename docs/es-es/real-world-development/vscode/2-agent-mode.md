---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Explora cómo usar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el entorno de desarrollo integrado (IDE) y permite obtener comentarios e interactuar rápidamente. El agente en la nube trabaja de forma asíncrona como un compañero del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios en varios archivos.
- el modo de agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario (UI) y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de incorporarlos al código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. En el ejercicio anterior ya creaste un asistente para editoriales. Ahora completarás con el modo de agente de Copilot el resto del trabajo en la capa de datos, la interfaz de usuario y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo de software. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter. No hay una API de backend independiente ni un framework de interfaz de usuario en el cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Inicia el sitio web

Para facilitar el inicio del sitio, el proyecto incluye una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Ejecútala en tu Codespace de GitHub siguiendo estos pasos:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un Codespace, seleccionar el enlace `localhost` en el terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los pasos siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que necesites.

La versión inicial del sitio funciona, pero queremos ampliarla con nuevas funciones. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo en particular. Al preparar el taller, usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que las sugerencias difieran de los ejemplos del taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes (LLM), Copilot podría usar un comando del Protocolo de contexto de modelo (MCP) distinto, pero debería poder completar la tarea.

3. Envía a Copilot este mensaje para preguntarle por el backlog de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado las incidencias según cuáles cree que serán más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivo, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios independientes en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. El modo de agente de Copilot puede realizar estas tareas. Añade la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con este mensaje:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: los asistentes de la capa de datos, la interfaz y las pruebas. Después, empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, aparecerán las opciones **Keep** o **Undo** para todos los cambios. No es necesario seleccionarlas mientras el trabajo está en curso.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se detenga varias veces e incluso que algunas pruebas fallen. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código que genere Copilot o cualquier otra herramienta de inteligencia artificial.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funcionalidades al sitio Tailspin Toys. Has aprendido cómo:

- el modo de agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de incorporarlos al código base.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo que has aprendido para pedir a Copilot que actualice el sitio. Al redactar el mensaje, ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el mensaje las directrices necesarias para que Copilot implemente lo que necesitas.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
