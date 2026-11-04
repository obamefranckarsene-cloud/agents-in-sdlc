---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

En este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, creara una sección de juegos relacionados y añadiera funciones de accesibilidad con controles de alto contraste y modo claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisaremos las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

El agente trabaja de forma asíncrona y sin supervisión directa, por lo que existen medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama de trabajo.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) creada por el agente: la documentación del código. Utilizarás la interfaz habitual de PR en GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que Copilot está bloqueado por un firewall. Es lo esperado: el acceso del agente a recursos externos, incluidos los servidores MCP, está limitado de forma predeterminada. Puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios que proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado todo, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían pasar para la aplicación Astro. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

El trabajo con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, como harías con cualquier miembro del equipo. Copilot leerá los comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay una única petición correcta. Por ejemplo, puedes pedirle que:

- añada al principio de cada archivo de código un encabezado con una descripción breve.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**. Utiliza una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás un commit nuevo en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, convierte la PR de borrador en una PR lista para revisión y combínala.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos se ejecuten desde la sección de comprobaciones de la PR y que pasen las comprobaciones del proyecto.
9. **Opcional:** cambia a la rama en tu Codespace para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir iterando y pedir cambios; por ejemplo, ajustar la cantidad de juegos relacionados o añadir encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade una solicitud en **Conversation** para que Copilot la vea e inicie una sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código.
5. Presta atención a:
   - los controles de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación por teclado.
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos se ejecuten y pasen las comprobaciones del proyecto.
10. **Opcional:** cambia a la rama en tu Codespace y prueba manualmente las funciones de accesibilidad. Ejecuta los comandos siguientes y sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que conservan las preferencias después de volver a cargar la página.

El agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones según las prácticas recomendadas. Si detectas problemas de accesibilidad o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que realice cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta especialmente útil para delegar elementos reales del backlog y ocuparte de otra tarea. Para practicar, registra otras incidencias en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles del juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para implementar modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios**, mencionado a `@copilot` para solicitar cambios y aprobado flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades, un agente QA personalizado, MCP de Playwright, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
