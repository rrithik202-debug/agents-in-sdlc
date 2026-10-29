---
title: "Lección 7 - Crear y usar un agente de control de calidad (QA)"
description: "Crea un agente personalizado de control de calidad (QA) que combine los requisitos de la incidencia, la habilidad quality-checks y el servidor Model Context Protocol (MCP) de Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automáticas y el servidor Model Context Protocol (MCP) de Playwright para observar la experiencia de filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de control de calidad (QA) bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automáticas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe la finalidad del agente, sus instrucciones y las herramientas disponibles. En este taller, definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. El servidor Model Context Protocol (MCP) de Playwright proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de los resultados. El perfil no sustituye a las demás personalizaciones ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar el agente.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente, revisa su perfil para confirmar que Copilot ha incluido el flujo de QA y los límites previstos. Así evitarás que un agente incompleto o demasiado permisivo cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero da al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que QA presenta pruebas, pide permiso antes de cambiar el código de implementación y no crea commits ni abre solicitudes de incorporación de cambios.
5. Si al perfil generado le falta alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA con la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la misma conversación de filtrado después de crear el perfil y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará.

2. Usa este mensaje para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que el agente de QA termine.
4. Lee el informe que te proporcione.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En concreto:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil reutilizable de control de calidad.
- seleccionaste el agente de QA y revisaste sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y las pruebas de verificación listos para revisar. A continuación, [los reunirás en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
