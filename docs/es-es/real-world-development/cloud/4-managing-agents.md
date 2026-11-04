---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay muchas herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes consultar todas las tareas que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y guiar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como el agente ya está actualizando los estilos y añadiendo controles de selección, parece lógico incluir esa funcionalidad. Guiarás al agente para que añada ambos modos.

## Revisa las tareas del agente de Copilot en la nube

Veamos el estado de las tareas asignadas al agente.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las listas de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que asignaste, como:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones que realizó Copilot. Pueden incluir:
   - clonar el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Desde ahí puedes consultar la PR y sus archivos modificados para seguir el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá de cómo la haya nombrado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar.
3. En el cuadro **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro para guiar la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot reconoce la petición y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva petición al terminar el paso que estaba ejecutando cuando enviaste el mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar la espera para repasar lo que has aprendido.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y guiar tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso para ajustar el alcance o dar más indicaciones.

Has explorado estos conceptos:

- has utilizado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión activa para pedir una funcionalidad adicional.

Mientras Copilot termina de implementar las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
