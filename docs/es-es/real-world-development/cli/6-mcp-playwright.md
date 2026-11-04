---
title: "Lección 6: Validar la funcionalidad con MCP de Playwright"
description: "Inspecciona o configura MCP de Playwright en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, escribir código implica algo más que programar. Tenemos que trabajar con datos y servicios externos, y permitir que Copilot utilice automatizaciones adicionales. Para ello sirven los servidores MCP: permiten que Copilot amplíe las herramientas y los servicios integrados en la CLI.

En esta lección:

- entenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero validar cambios en la interfaz requiere interactuar con ella. Quieres que Copilot pueda utilizar el sitio web como lo haría una persona, para automatizar más tareas y aumentar la confianza en que los cambios funcionan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente hacia las herramientas y los servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizarlos, revisa su código fuente, comprueba quién los publica y considera sus implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si MCP de Playwright ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente el funcionamiento. Ahora utiliza el servidor MCP de Playwright disponible en Copilot para hacer lo mismo.

1. Pide a Copilot que valide la funcionalidad con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es imprescindible indicar explícitamente a Copilot que utilice el servidor MCP: normalmente lo detectará. Sin embargo, señalarle la herramienta ayuda a obtener resultados más coherentes y ahorra tokens.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como se esperaba.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si todavía no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente QA personalizado][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
