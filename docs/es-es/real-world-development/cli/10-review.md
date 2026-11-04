---
title: "Lección 10: Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- añadido las valoraciones por estrellas, revisado el resultado en un navegador con el puerto reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, creado la funcionalidad en modo Autopilot y revisado el resultado en modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y utilizado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado la herramienta para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisado el cambio completo de filtrado y autorizado Agent Merge para la PR.
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Qué has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas**: muestra en las tarjetas de juegos el valor existente de `starRating` o un estado explícito para las valoraciones que faltan.
2. **Filtrado y flujo de calidad**: implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has trabajado en la misma conversación y rama. Hemos agrupado el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, mediante tu propia inspección en el navegador y mediante la exploración de Copilot en el navegador con MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados en el formato actualizado. QA reunió esos resultados con una revisión de los requisitos y de la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir lagunas reales. Es válido que una ejecución de QA no necesite añadir pruebas. Revisa el código y las pruebas antes de autorizar la combinación; si algo cambia, actualiza las pruebas y las pruebas de verificación afectadas.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir un enfoque antes de desarrollar, **Interactive** para mantener el control de los cambios acotados y **Autopilot** para las tareas bien definidas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas que puedes explorar

Has completado el flujo de trabajo principal. Estas funciones de CLI también pueden resultarte útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y recibir otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que tienes en mente desde hace años. Comparte lo que aprendas con el equipo y aprovecha lo que aprendan los demás. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
