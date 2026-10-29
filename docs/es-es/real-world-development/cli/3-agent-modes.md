---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. En esta lección, que inicia una serie sobre el desarrollo con agentes, partirás de una incidencia para crear una funcionalidad, comprobar que el código es válido y que la funcionalidad se comporta como esperas, y combinarla correctamente en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían distintas ramas o PR para los diferentes tipos de archivos, pero en este taller se simplifica el proceso para centrarse en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador reenviado.

Mientras sigues trabajando en esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás la validación con el Protocolo de contexto de modelo (MCP), crearás un agente de control de calidad (QA) y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de empezar a programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con inteligencia artificial al flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo necesario.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y el proceso de integración continua se haya completado correctamente.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen un patrón parecido al anterior.

Al seguir este enfoque estándar, garantizas que el código generado con inteligencia artificial cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: colaboras con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta que considera que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de iniciar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de valoración por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

Planificar permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de delegarla a Copilot. Para cualquier trabajo complejo, conviene dedicar tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este mensaje:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no se ajuste a tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, puedes implementarlo con Autopilot, que permite a Copilot iterar hasta que considera que la tarea está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente que aparezca en la versión instalada.
2. Confirma que el indicador situado debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio. Examina el código y ejecuta el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa el resultado de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

Ahora que el código parece correcto, comprueba que se ejecuta. Inicia la aplicación como antes y abre el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y próximos pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot a partir de una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la funcionalidad con el modo Autopilot.
- revisaste el código.
- validaste la funcionalidad manualmente en un navegador reenviado.

A continuación, aprenderás a generar código siguiendo las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
