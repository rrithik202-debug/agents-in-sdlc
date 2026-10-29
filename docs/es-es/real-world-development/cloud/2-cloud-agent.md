---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog por falta de tiempo para implementarlas. El agente en la nube de GitHub Copilot puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, los desarrolladores pueden delegar tareas tediosas, acelerar el desarrollo y centrarse en asuntos de mayor importancia.

Con el agente en la nube de Copilot explorarás lo siguiente:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría para que los visitantes puedan seguir explorando. Todavía no necesita una implementación refinada; solo quiere algo que pueda usar para las pruebas de aceptación de la experiencia de usuario (UX). Aunque esta tarea bloquea ese trabajo, hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas con facilidad y son buenas candidatas para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo y confirmar que el resultado es correcto.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una PR en borrador para hacer un seguimiento del trabajo, prepara un entorno y empieza a trabajar en la tarea. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando la solución esté lista para que la revises, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las operaciones que parecen sencillas pueden ser complejas cuando se examinan de cerca. Por eso, [delimita con cuidado las tareas que asignas al agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con inteligencia artificial: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar inteligencia artificial generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para que luego puedas observar su efecto.

Abre estos archivos en la interfaz web de GitHub o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca en la PR resultante comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo para el agente en la nube de Copilot

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración para garantizar que todo esté listo, independientemente de quién realice el trabajo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube usa [GitHub Actions][github-actions] para preparar el entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas y dependencias de desarrollo necesarias. Este flujo de trabajo ya está configurado para facilitar el taller y permitirte explorar esta función. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para realizar operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
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

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo es parecido a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que se ejecuta correctamente sin esperar a que Copilot lo haga.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Es un tipo de deuda técnica que a menudo queda sin resolver, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la generación de documentación, así que esta incidencia es ideal para asignarla al agente en la nube. El agente trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior, volverás para revisar el resultado.

1. Abre una pestaña nueva del navegador y visita el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crearla sin una plantilla.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera uno o dos minutos y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecuta el flujo de trabajo mencionado antes—, analizando el proyecto y definiendo cómo resolver la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve la tarea: explora el código y comprende su estado, se detiene a analizar el problema y decidir el plan adecuado, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es que pueda trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecha esa función asignándole otra tarea y, después, céntrate en añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: puedes ocuparte de unas tareas mientras el agente trabaja en otras. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Asigna el trabajo al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar una plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerá 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software (SWE), así que no hace falta que lo supervises continuamente; puedes revisar el resultado cuando termine. Ahora puedes centrarte en crear y usar agentes personalizados.

## Resumen y próximos pasos

En este ejercicio has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignar incidencias a Copilot para que trabaje en ellas de forma asíncrona. El agente puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores del Protocolo de contexto de modelo (MCP)][cloud-agent-mcp] y acceder a las instrucciones personalizadas que exploraste en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Ejercicio siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
