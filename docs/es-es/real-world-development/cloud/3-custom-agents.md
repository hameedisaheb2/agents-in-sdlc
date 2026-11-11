---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados para determinadas tareas o áreas del flujo de trabajo de desarrollo. Al definirlos en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para ayudarle a realizar ciertos tipos de trabajo de forma más eficaz. Los equipos pueden plasmar sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las pautas de accesibilidad para el contenido web (WCAG), uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter en YAML (YAML Ain't Markup Language) que incluye al menos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente en archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son sus herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes como parte de su trabajo. En este ejercicio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Para ello, podría recurrir a habilidades como una que gestione el flujo de trabajo de una solicitud de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor funcione en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados, explorarás lo siguiente:

- cómo se definen;
- cómo se asigna una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que las personas puedan activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad cuanto antes. Para generarla, utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisarás su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que incluyen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - prácticas recomendadas de accesibilidad;
   - ejemplos de código del lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript;
   - errores y problemas habituales.

## Crea y asigna una incidencia

El panel de agentes es el centro de control donde puedes trabajar con todos los agentes de tu entorno. Desde allí, puedes asignar tareas al agente de Copilot en la nube, supervisarlas y hasta redirigir una sesión en curso o proporcionarle más indicaciones. Empezaremos asignando a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Add high contrast mode to website` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Necesitamos un modo de alto contraste para el sitio. Debe haber un control que el usuario pueda activar para usarlo. La configuración debe guardarse en el almacenamiento local del navegador.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Captura del cuadro de asignación del agente en la nube con el agente personalizado y la opción de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y próximos pasos

En este ejercicio has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para determinadas tareas y áreas. Con ellos, puedes plasmar los conocimientos y estándares de tu equipo en agentes reutilizables que guíen a Copilot para que realice ciertos tipos de trabajo de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo se asigna una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes centrarte en [supervisar y orientar la sesión del agente desde el panel de control][next-lesson].

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparación para usar agentes personalizados en tu organización][org-custom-agents]
- [Preparación para usar agentes personalizados en tu empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
