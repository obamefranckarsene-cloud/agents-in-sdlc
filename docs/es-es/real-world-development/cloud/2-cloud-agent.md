---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dedicar más tiempo a asuntos importantes.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que las personas puedan seguir explorando. No necesitan una implementación pulida, solo algo que permita probar y aceptar la experiencia de usuario. Aunque esta tarea bloquea el trabajo del equipo, hay otras incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente y son buenas candidatas para el agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar el trabajo terminado.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Una opción es [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una PR en borrador para mostrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue en curso o una vez terminada. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de acotar bien el trabajo

GitHub Copilot no hace magia ni evita tener que pensar. Incluso las tareas aparentemente sencillas pueden ser complejas. Por eso, [define cuidadosamente el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones, habrás visto funcionar `.github/copilot-instructions.md`; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente, examina en modo de solo lectura los archivos de instrucciones que ya incluye el repositorio para poder observar después su efecto.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las pautas sobre convenciones de TypeScript y comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, comprueba si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas pautas proceden de los archivos de instrucciones; volveremos a señalarlas al revisar las PR más adelante.

## Configura el entorno de desarrollo del agente de Copilot en la nube

La creación de código suele requerir un entorno específico y la ejecución de scripts para preparar el proyecto. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller y proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para las operaciones.
    permissions:
      # Para clonar el repositorio en los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas aquí, Copilot lo hará automáticamente al finalizar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación utiliza Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub y tiene varios aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena incidencia para delegar. En un ejercicio posterior revisarás el resultado.

1. En una nueva pestaña del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que se acaba de generar; su título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está configurando el entorno mediante el flujo anterior, analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza el estado, planifica y crea código.

La tarea puede tardar varios minutos. Uno de los objetivos principales del agente en la nube es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecharemos esta función asignándole otra tarea y, mientras tanto, nos centraremos en añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite dividir el trabajo: tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Asignemos al agente la sección de juegos relacionados que ha solicitado el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

6. Selecciona **Create** y, a la derecha, **Assign to Copilot**.
7. Selecciona **Assign**.

Poco después verás el emoji 👀 en el primer comentario de la incidencia. Indica que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente funciona de forma parecida a un ingeniero de software, no hace falta supervisarlo continuamente; revisa el resultado cuando termine. A continuación, crearás y utilizarás agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y utilice Copilot para resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. [El agente también puede utilizar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
