---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas y orientar una sesión que esté en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad de alternancia, parecía lógico incluirlo. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas que aparecen en los paneles principal e izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar interfaces de programación de aplicaciones (API) para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que Copilot ha realizado, entre ellas:
   - obtener el código del repositorio;
   - preparar el entorno de trabajo de Copilot;
   - configurar servidores del Protocolo de Contexto del Modelo (MCP);
   - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de solicitudes de incorporación de cambios, que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir supervisándola.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya elegido y del estado actual del trabajo.

   ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de preparar el entorno y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios seleccionar el modo de visualización que prefieran.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir tu mensaje.

Como el trabajo puede tardar varios minutos, es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En este ejercicio has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde allí puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir sesiones en curso para ajustar el alcance o dar más indicaciones.

Has explorado estos conceptos:

- explorar el panel de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
