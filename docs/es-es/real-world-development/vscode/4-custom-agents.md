---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial especializados en tareas o ámbitos concretos del flujo de trabajo de desarrollo. Al definir agentes en archivos Markdown dentro de la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para ayudarle a realizar determinados tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de las Pautas de Accesibilidad para el Contenido Web (WCAG), uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter en formato YAML (YAML Ain't Markup Language) con, al menos, los campos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a una inteligencia artificial cómo realizar determinadas operaciones. La diferencia más sencilla es que un **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes para realizar el trabajo. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Como parte de su trabajo, podría utilizar, por ejemplo, una habilidad para gestionar el flujo de una solicitud de incorporación de cambios y otra para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de inteligencia artificial, pruébalo e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes indican que a algunos usuarios les cuesta leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, quieres implementar esta mejora cuanto antes. Para ello, utilizarás un agente personalizado.

En este ejercicio aprenderás a:

- revisar un agente personalizado de accesibilidad existente.
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y abre un terminal. Cámbiate a una rama nueva basada en `main` para el trabajo de accesibilidad; así mantendrás separada la PR de filtrado del ejercicio 3.

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript.
   - errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes situado en la parte inferior de la vista Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los pasos siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que necesites.

1. Selecciona **Agent** en el selector de agentes de la vista Chat, si aún no está seleccionado.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes de la parte inferior de la vista de chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este mensaje al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat junto con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso probablemente tardará unos minutos. Copilot está haciendo cambios reales en el repositorio: editará archivos existentes, como componentes de Astro, hojas de estilo CSS y pruebas relacionadas.

En el ejercicio siguiente revisarás y orientarás este trabajo mientras está en curso.

## Resumen y próximos pasos

En este ejercicio has explorado los [agentes personalizados][custom-agents], asistentes de inteligencia artificial especializados en tareas y ámbitos concretos. Los agentes personalizados permiten plasmar los conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot para realizar determinados tipos de trabajo con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Ejercicio siguiente: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
