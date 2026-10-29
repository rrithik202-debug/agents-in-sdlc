---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición de archivo a medida que ocurren. Puedes revisar cada diferencia en línea, aceptar o rechazar los cambios por separado y orientar la conversación con mensajes de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas en los archivos.
- orientarás al agente durante la sesión para que añada un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha hecho una petición adicional: también quiere un control del modo claro para los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías recibir un mensaje final que resuma los cambios.

## Orienta al agente para que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios que ya hizo y propondrá las adiciones para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, si intenta refactorizar los controles de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviar un mensaje aclaratorio.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes («simplifica las hojas de estilo en cascada (CSS): usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y examina las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [el ejercicio siguiente][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición de archivo, aceptado o rechazado cambios y enviado mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- supervisaste el trabajo de un agente en tiempo real.
- aceptaste y rechazaste ediciones de archivos por separado.
- orientaste una sesión en curso mediante mensajes de seguimiento.

En el [ejercicio siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
