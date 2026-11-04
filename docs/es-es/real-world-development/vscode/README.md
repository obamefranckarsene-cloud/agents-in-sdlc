---
slug: es-es/real-world-development/vscode
title: "VS Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** en VS Code lleva GitHub Copilot al editor de código que ya utilizas. Al trabajar en Visual Studio Code y GitHub Codespaces, dirigirás Copilot Chat en modo de agente, conectarás herramientas externas mediante MCP y utilizarás agentes personalizados, todo ello sin salir del IDE, donde Copilot puede consultar tus archivos, el terminal y los problemas.

Empezarás por preparar el entorno y añadir instrucciones personalizadas para comprobar cómo las sigue Copilot. Después, utilizarás el modo de agente para crear una funcionalidad de filtrado que abarque la interfaz, la capa de datos y las pruebas. A continuación, conectarás el servidor MCP de Playwright y dejarás que Copilot controle un navegador para probar la funcionalidad antes de abrir una solicitud de incorporación de cambios. Por último, revisarás y utilizarás un agente personalizado para trabajar en la accesibilidad y, después, supervisarás, guiarás e iterarás sobre los cambios de Copilot, todo ello sin salir del editor.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade y verifica instrucciones personalizadas en VS Code |
| [2. Modo agente][ex2] | Generación de código | Crea una funcionalidad de filtrado con el modo agente |
| [3. MCP con Playwright][ex3] | Herramientas externas | Prueba la funcionalidad en un navegador con el servidor MCP de Playwright |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revisa y utiliza agentes personalizados |
| [5. Gestión de agentes][ex5] | Supervisión | Supervisa y guía las sesiones de agente |
| [6. Iteración][ex6] | Revisión | Revisa localmente el trabajo de Copilot y elige los siguientes pasos |
| [Opcional: Incorporar Foundry][foundry-toolkit] | Agentes hospedados | Prepara un modelo, crea y despliega un agente hospedado e intégralo en el sitio en tres módulos con VS Code y Foundry Toolkit |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Acceso a GitHub Codespaces

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funcionalidades de agente, MCP, revisión de código y Copilot CLI que utiliza este taller.

[callout-student-plan-education]: https://github.com/education/students
## Empezar

**[Empieza por el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
