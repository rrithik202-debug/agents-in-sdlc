---
slug: es-es/real-world-development/cloud
title: "Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

El **[agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asignas trabajo en GitHub y el agente en la nube se encarga de él en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios mientras tú puedes dedicarte a otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá el agente en la nube y, después, le asignarás una incidencia de GitHub para que la implemente. Revisarás y utilizarás agentes personalizados para orientar su enfoque, supervisarás y dirigirás las sesiones desde el panel de agentes y, por último, revisarás sus solicitudes de incorporación de cambios e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigna incidencias al agente en la nube de Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y utiliza agentes personalizados |
| [4. Gestionar agentes][ex4] | Supervisión | Supervisa y dirige las sesiones de agente |
| [5. Iterar][ex5] | Revisión | Revisa las solicitudes de incorporación de cambios (PR), itera sobre el trabajo de Copilot y elige los próximos pasos |

## Requisitos previos

Antes de empezar este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] El agente en la nube de Copilot habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes cuya condición de estudiante se haya verificado pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, del Protocolo de contexto de modelo (MCP), de revisión de código y de Copilot CLI que se usan en este taller, por lo que permite completar todos los entornos.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> El agente en la nube está disponible en los planes **Copilot Student, Pro, Pro+, Business y Enterprise**, pero no en Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este entorno no incluye el Protocolo de contexto de modelo (MCP). Para aprender a usar servidores MCP con Copilot, consulta el [entorno de CLI][cli-harness] o el [entorno de VS Code][vscode-harness].

## Empezar

Para iniciar el taller, abre la guía de requisitos previos y prepara el entorno.

**[Empieza con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
