---
title: "Lección 5: Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código no es lo único que hay que hacer. Hemos podido comprobar manualmente que el código funciona y hemos utilizado archivos de instrucciones para asegurarnos de que cumple nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción adecuada. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos auxiliares que un agente carga cuando los necesita. En esencia, son una carpeta con un archivo Markdown `SKILL.md`. El archivo contiene metadatos de frontmatter con el nombre y la descripción de la habilidad, una descripción general de lo que hace y pautas sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Vamos a explorar la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a determinar cuándo debe utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Observa que todavía no existe una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Utiliza este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ejecutó, si se completó correctamente y qué resultados comunicaron realmente las herramientas. Actualiza la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Una vez actualizado el archivo, comprueba el resultado. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Utiliza exactamente el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

Este cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

- Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:
- [Especificación de Agent Skills][agent-skills-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas en navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[agent-skills-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
