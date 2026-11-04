---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan pautas para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de codificación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- generarás la primera parte de los datos para el filtrado —una función auxiliar para editores— con las instrucciones actuales.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado sigue el estándar nuevo.
- confirmarás los cambios de instrucciones y la base del filtrado en `main` para que Copilot pueda utilizar la guía actualizada en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener siempre código idéntico.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de código y los requisitos. Permiten orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones de código, bibliotecas y los comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a tipos de archivo y tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información relevante para la mayoría de las solicitudes, como la pila tecnológica, una descripción general del proyecto y las pautas globales.
- `.github/instructions/*.instructions.md`: archivos que proporcionan pautas para lenguajes o tipos de archivo (como TypeScript o Astro), o tareas concretas.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni sugerir ediciones posteriores.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

- Mantén `copilot-instructions.md` centrado en las pautas del proyecto, como su descripción, estructura y estándares globales de codificación.
- Utiliza archivos `*.instructions.md` para tipos de archivo —pruebas unitarias, componentes Astro o capa de datos— y tareas específicas.
- Escribe en lenguaje natural. Expresa las pautas con claridad e incluye ejemplos de cómo debe y no debe ser el código.

No existe una única forma correcta de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería disponer de una colección sólida de archivos de instrucciones. En este proyecto hay archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz lo ofrece de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en este repositorio antes de hacer cambios para comprender su efecto.

1. Abre `.github/copilot-instructions.md`.
2. Fíjate en la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las pautas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos de Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define un patrón relativo a la raíz del repositorio; por ejemplo, coincide con archivos TypeScript como `**/*.test.ts`.
5. Observa las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a ejemplos del proyecto. Las rutas se resuelven desde el archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios.

## Crea una rama para los cambios

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso especificarán qué elegir.

Para ver el efecto de las instrucciones personalizadas, envía primero un prompt con las instrucciones actuales. Más adelante actualizarás el archivo y volverás a enviar el mismo prompt.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente en el archivo auxiliar y sus pruebas.
6. Observa que la función propuesta tiene tipos, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto sigue las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, aplicable a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario en el archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación incluso sin que se lo pidas. No pasa nada; el objetivo es observar la mejora en la coherencia tras actualizar las instrucciones.

## Añade un estándar para todo el repositorio

`.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Actualiza los estándares para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. No incluye una regla de documentación en el código.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Los archivos de instrucciones pueden estar en `.github/copilot-instructions.md` para pautas globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. Los estándares globales del proyecto deben estar en el archivo del repositorio.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el búfer.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo.
3. Envía exactamente el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Comprueba que la función incluye un comentario TSDoc parecido al siguiente:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el control de versiones, igual que los demás archivos del proyecto.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba que los cambios están presentes:

   ```bash
   git status
   ```

3. Añade y confirma las instrucciones y los archivos modificados por Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Abre una solicitud de incorporación de cambios vinculada a la incidencia del estándar de documentación del backlog. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub, así que no hay que configurarlas. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión. Sigue las instrucciones. Aprenderás cómo funciona esta conexión (Model Context Protocol) en un ejercicio posterior.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y asegúrate de que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con las instrucciones y cree una PR desde la rama actual. Indícale que describa tanto las instrucciones como la base de la función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub, revisa el comando y permite solo las acciones adecuadas.
6. Cuando se cree la PR, pídele que la combine y vuelva a `main` local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si se te solicita **Allow** para acciones de GitHub o comandos de terminal, revisa el comando antes de autorizarlo.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y, con Copilot Chat en VS Code:

- has enviado un prompt de generación de código y observado el resultado con las instrucciones *existentes*.
- has añadido un estándar global a `.github/copilot-instructions.md`.
- has vuelto a enviar el prompt y observado cómo el código propuesto sigue el estándar nuevo.
- has confirmado los cambios de instrucciones y la base de la función auxiliar en `main`.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] mientras Copilot añade una funcionalidad.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
