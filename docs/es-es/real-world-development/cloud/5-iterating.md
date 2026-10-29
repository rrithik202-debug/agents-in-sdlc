---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Revisa los cambios de código y, si hace falta, proporciona comentarios a Copilot para mejorar el resultado.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y el desarrollo y las operaciones (DevOps) no cambian al incorporar inteligencia artificial generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisa las propuestas de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que el entorno siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- De forma predeterminada, el [acceso a recursos externos está limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Protocolo de contexto de modelo (MCP).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la solicitud de incorporación de cambios (PR), puede que aparezca un aviso de que el cortafuegos bloquea GitHub Copilot. Es **esperable**: de forma predeterminada, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores del Protocolo de contexto de modelo (MCP) externos. Si quieres, puedes [personalizar o desactivar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** o que describa con más detalle la documentación añadida.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación de TypeScript (TSDoc) y otra documentación. Los cambios exactos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no tiene por qué ser unidireccional. Puedes mencionar a Copilot en comentarios de la PR, como harías con otros miembros del equipo, o en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son no deterministas, no hay un mensaje único que debas usar. Algunas ideas de cambios que puedes pedir a Copilot:

- añadir encabezados de comentarios breves al principio de cada archivo de código para explicar su función.
- añadir comentarios TSDoc a los archivos TypeScript y Astro.
- crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores u otra sugerencia de documentación que quieras incluir en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, convierte la PR de borrador en lista para revisión y combínala en la rama principal.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora vuelve a la PR que Copilot generó para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page** o que describa con más detalle la funcionalidad.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)
8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente la funcionalidad de juegos relacionados. Abre el terminal del Codespace y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente en la nube para solicitar cambios. Por ejemplo, puedes pedirle que ajuste el número de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade otro comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o que describa con más detalle la funcionalidad.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. El proceso puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de hojas de estilo en cascada (CSS) o de estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas de Accessible Rich Internet Applications (ARIA) y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Confirma que funcionan como esperas y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema de accesibilidad o una posible mejora, puedes mencionar a **@copilot** en un comentario para solicitar cambios, igual que hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otra tarea. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para mecenas en la página de detalles del juego.
- implementar la paginación en la página de lista de juegos.
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estos ejercicios:

- **Inspeccionaste los archivos de instrucciones personalizadas del repositorio** para observar su efecto posterior en el trabajo del agente en la nube.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir controles de alto contraste y modo claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Repaso y próximos pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el entorno de desarrollo integrado (IDE).
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el puente opcional `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
