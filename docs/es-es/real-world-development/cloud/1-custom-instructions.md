---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de codificación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los talleres de VS Code y CLI, aquí no compararás prompts antes y después. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios del agente.

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

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información relevante para la mayoría de las solicitudes, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas globales.
- `.github/instructions/*.instructions.md`: archivos que proporcionan pautas para lenguajes o tipos de archivo (como TypeScript o Astro), o tareas (como crear un componente de interfaz o pruebas unitarias).

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni sugerir ediciones posteriores.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones excede el alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén `copilot-instructions.md` centrado en las pautas del proyecto, como su descripción, estructura y estándares globales de codificación.
- Utiliza archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expresa las pautas con claridad e incluye ejemplos de cómo debe y no debe ser el código.

No existe una única forma correcta de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para cada proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería disponer de una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, verás archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo ofrece de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller de cada interfaz lo explicará cuando corresponda.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes en archivos reutilizables más pequeños y señalar ejemplos para Copilot. Las rutas se resuelven desde el archivo de instrucciones, no desde la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios en los archivos.

## Añade un estándar para todo el repositorio

Este es el único cambio que realizarás aquí: añade una regla global para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de archivo. El agente en la nube aplicará la regla cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que apunte la incidencia, pero así podrás confirmar los cambios de forma ordenada.

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Allí se documentan los estándares, pero aún no hay ninguna regla de documentación en el código.
4. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en `.github/copilot-instructions.md` para definir pautas globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas específicos. El archivo del repositorio es el lugar adecuado para estándares globales como la regla de comentarios de documentación.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el ejercicio siguiente, Copilot partirá de `main`, así que los cambios deben llegar a `main` para que pueda utilizarlos.

1. Añade los cambios y crea un commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` hacia `main` y combínala. El agente en la nube ya podrá leer estas instrucciones al trabajar en las incidencias del próximo ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y crear el commit en `main`. La rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has añadido un estándar global al repositorio. Más adelante verás cómo aplica ese estándar a las solicitudes de incorporación de cambios que genera.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
