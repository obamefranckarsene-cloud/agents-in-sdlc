---
title: "Ejercicio 2: Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Normalmente hay que localizar los archivos, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente puede trabajar de forma más autónoma en el IDE. Se comporta como un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, lo que permite interactuar y recibir comentarios rápidamente. El agente en la nube funciona de manera asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar archivos y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editor. En el ejercicio anterior añadiste una función auxiliar para editores. Ahora completarás los cambios restantes en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de cambiar nada, explora el sitio para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde su frontmatter; no hay una API independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API DOM estándar.

### Inicia el sitio

Para iniciar el sitio, se proporciona la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Desde tu Codespace:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, seleccionar el enlace localhost del terminal redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

### Explora el sitio

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a sus detalles, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, abre Copilot Chat en el codespace y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

El sitio ya funciona, pero queremos añadir capacidades. La plantilla creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que lo muestre.

1. Selecciona **Agent** en el menú de agentes de Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Se utilizó Claude Sonnet 4.5 para preparar el taller con el objetivo de obtener resultados relativamente coherentes. Los LLM son probabilísticos y las sugerencias pueden variar; es normal.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot podría utilizar otro comando MCP y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que enumere las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según el valor que considere que tienen para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, pruebas y accesibilidad.
   - enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar pautas y dividir conjuntos complejos en partes más manejables con ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios. El modo de agente puede realizar estas tareas.

1. Continúa la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes en la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, modificar archivos y ejecutar pruebas. Durante el proceso podrías ver los botones **Keep** y **Undo** en el editor. Al terminar, cada cambio tendrá una de estas opciones; no hace falta seleccionarlas mientras Copilot trabaja.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot Chat pide confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y fallos en algunas pruebas. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos.
- explorar el proyecto, identificar archivos y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo funcionará la paginación y si el usuario podrá seleccionar el tamaño de página o será fijo.
- proporciona a Copilot en el prompt las pautas necesarias para implementar la solución deseada.
- puede que tengas que iterar, pedir cambios y proporcionar contexto; es normal al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
