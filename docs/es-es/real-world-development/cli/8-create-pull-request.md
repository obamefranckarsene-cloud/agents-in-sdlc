---
title: "Lección 8: Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú combinaste la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control para autorizar la combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y actualiza la rama cuando es necesario. Después, habilita la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Una vez creado todo el código, revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la combinará cuando todo termine correctamente.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y, cuando la configuración del repositorio lo permita, realiza manualmente una combinación que ya hayas revisado.

## Resumen y siguientes pasos

Has automatizado varias etapas del desarrollo: generación de código, pruebas y validación y, ahora, el proceso de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
