---
title: "Ejercicio 3: Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y permitirás que Copilot utilice un navegador real. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás una PR para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que administra la comunicación con los servicios externos, como API existentes o herramientas locales de paquetes NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear repositorios, actualizarlos y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador mediante Playwright, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP](https://github.com/mcp) que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizarlos, revisa su código fuente, comprueba quién los publica y considera las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al conceder acceso a datos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla incluye esta entrada y por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalarlo localmente: VS Code se conecta por HTTP y la primera vez que Copilot utiliza una herramienta te autenticas con GitHub.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que utilice el navegador sin mostrar una ventana, necesario en un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya estará instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot utiliza las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y selecciona la opción para confiar en él.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y solicitará que confirmes la confianza cuando Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] permite que Copilot controle un navegador real. En vez de probar la aplicación tú mismo, el agente puede abrir una página, navegar, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Es una forma rápida de verificar una funcionalidad sin salir de la conversación.

El servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Por eso, el agente analiza elementos etiquetados y estructurados —botones, enlaces y elementos de lista— de una forma parecida a las tecnologías de asistencia. Una comprobación funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus observaciones. Compara el resultado con los criterios de aceptación de la incidencia. Si algo no funciona, pide más información o solicita que corrija el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en marcha en `http://localhost:4321`. Si detuviste el servidor de desarrollo, inícialo otra vez antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright quizá tenga que descargar un navegador; si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has comprobado que la funcionalidad funciona. Ahora abrirás una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Añade los cambios al área de preparación seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una PR desde github.com o mediante la interfaz de línea de comandos de GitHub. Como ya estás trabajando con Copilot, pídele que la cree y la vincule a la incidencia correcta.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Asegúrate de que **Agent** esté seleccionado para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que utilice una herramienta de GitHub, puede que tengas que autenticarte.
5. Observa cómo Copilot busca las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace de Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar manualmente la funcionalidad de filtrado antes de publicarla.
- has publicado la rama y abierto una PR para el filtrado.

Has utilizado MCP para *probar* una funcionalidad, pero es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
