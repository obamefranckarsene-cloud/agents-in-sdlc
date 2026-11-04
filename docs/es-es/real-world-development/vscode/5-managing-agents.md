---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente, Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y guiar la conversación con prompts para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- guiarás la sesión en curso para añadir un control de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, dirigido a quienes encuentran demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, ampliarás el trabajo en la sesión existente.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Las modificaciones aparecen en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, la lectura y edición de archivos y los comandos de terminal que solicita.
3. Cuando proponga cambios en un archivo, las diferencias aparecerán en línea:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** situado en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará la opción **Allow**. Autoriza solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al finalizar, mostrará un resumen de los cambios.

## Guía al agente para añadir el modo claro

Ahora que ya está implementado el modo de alto contraste, amplía la misma conversación para añadir un control de modo claro. Al continuar en la misma sesión, el agente conserva el contexto de su trabajo: los componentes que modificó, el enfoque de estilos y el uso del almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará sus cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición a medida que aparezca. Utiliza **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles—, puedes detenerlo con **Stop**, junto al campo de chat, y enviar una aclaración.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de crear un commit, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y las pruebas relacionadas.
3. Abre varios archivos y revisa las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia en almacenamiento local.
4. Cuando estés conforme, añade los cambios al área de preparación y crea un commit desde el panel Source Control. Los publicarás en la [lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado cambios individuales en los archivos.
- has guiado la sesión con prompts de seguimiento.

En la [lección siguiente][next-lesson], publicarás los cambios de accesibilidad en una solicitud de incorporación de cambios y revisarás el trabajo del agente local personalizado.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
