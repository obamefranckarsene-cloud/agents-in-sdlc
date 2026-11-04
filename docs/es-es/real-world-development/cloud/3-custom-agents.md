---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o áreas concretas del flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos que ayuden a Copilot a realizar determinadas tareas. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que siga las pautas [WCAG][wcag], uno de seguridad que siga prácticas seguras o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` en el proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La distinción más clara es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Podría utilizar una habilidad para gestionar solicitudes de incorporación de cambios o para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones y adáptalo a tu entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen.
- cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. Para resolverlo, el equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, por lo que quieres implementar esta funcionalidad cuanto antes. Para generarla, utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea una incidencia y asígnala

Mission control es el centro desde el que puedes trabajar con agentes: asignar tareas al agente de Copilot en la nube, supervisarlas y redirigirlas o proporcionar más indicaciones. Asignemos a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Cuadro de asignación del agente en la nube con el agente personalizado de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados para tareas y áreas concretas. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignarles una tarea.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y guiar la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para utilizar agentes personalizados][org-custom-agents]
- [Preparar la empresa para utilizar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
