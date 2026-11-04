---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o áreas concretas del flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas seguras o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` en el proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones.

### Diferencias entre los agentes personalizados y las habilidades

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen aspectos en común: ambos se definen principalmente mediante Markdown e indican a una IA cómo realizar operaciones. La distinción más clara es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Podría utilizar habilidades para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba distintas opciones y adáptalo a tu entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, por lo que implementarás esta funcionalidad cuanto antes con un agente personalizado.

En este ejercicio:

- revisarás el agente personalizado de accesibilidad existente.
- utilizarás el agente en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y, en un terminal, cámbiate a una rama nueva basada en `main` para mantener separado el trabajo de accesibilidad de la PR de filtrado del ejercicio 3:

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
   - las responsabilidades principales al generar código para un sitio accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra en el menú de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Al seleccionar uno, la sesión de chat adopta sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, abre Copilot Chat en tu codespace y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

1. Selecciona **Agent** en el menú de agentes de Chat si todavía no está seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el menú de agentes que aparece en la parte inferior de la vista Chat; de forma predeterminada muestra **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía al agente este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarlas.
6. El indicador **Files changed** mostrará los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot realizará cambios reales en el repositorio y editará componentes Astro, CSS y las pruebas relacionadas.

Revisarás y guiarás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados que codifican la experiencia y los estándares del equipo en agentes reutilizables.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
