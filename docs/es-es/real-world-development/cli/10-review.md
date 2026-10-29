---
title: "Lección 10 - Resumen y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado.
- añadiste las valoraciones por estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste en Autopilot y lo revisaste en Interactive.
- guiaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- revisaste el cambio completo de filtrado y autorizaste Agent Merge para su PR.
- exploraste comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Qué has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en la versión actualizada de `main`:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has usado la misma conversación y rama. Para simplificar el taller, todo ese trabajo se reúne en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automáticas, mediante una inspección propia en el navegador y con la exploración del navegador de Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó un informe con el nuevo formato. QA reunió esos resultados con la revisión de los requisitos y la cobertura antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, el resultado también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las evidencias de verificación afectadas después de hacer cambios.

## Procedimientos recomendados

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para cada tarea. Usa **Plan** para definir un enfoque antes de implementarlo, **Interactive** para colaborar en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y otro más capaz para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más funciones por explorar

Ya has completado el flujo de trabajo principal. Estas funciones adicionales de la CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con una herramienta es seguir usándola: aplícala al código de producción, a proyectos personales o a esa pequeña aplicación que hace tiempo que quieres crear. Comparte lo que aprendas con el equipo y aprovecha lo que te enseñen los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
