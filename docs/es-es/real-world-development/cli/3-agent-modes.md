---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudar a desarrollar lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código, validarás su comportamiento y, finalmente, la combinarás con el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de la funcionalidad, usarás la misma conversación y rama. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivo, pero aquí simplificaremos algunos pasos para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás manualmente la funcionalidad en un navegador reenviado.

Para completar la funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para tomar esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. ¡De hecho, los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Empiezan con una incidencia registrada que describe el trabajo.
2. Planifican lo que hay que desarrollar.
3. Escriben y revisan el código.
4. Ejecutan pruebas para validar el código.
5. Validan manualmente la nueva funcionalidad.
6. Crean una solicitud de incorporación de cambios (PR).
7. Combinan el código después de revisarlo y de que el proceso de integración continua finalice correctamente.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos son variaciones de este modelo.

Seguir este enfoque estándar ayuda a que el código generado por IA cumpla los requisitos y pase por el mismo proceso de evaluación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: tú y el agente trabajáis juntos. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan y revisa el plan; después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de comenzar el trabajo de filtrado, vuelve al Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve al Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de la valoración por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` o un número similar. El carácter `#` permite incluir en el contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de delegarla a Copilot. Para cualquier trabajo complejo, conviene dedicar un momento a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el prompt siguiente:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y determinará el enfoque más adecuado para elaborar el plan.

3. Es posible que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte del plan que no coincida con tus decisiones.

## Aprueba Autopilot

Ya has redactado y revisado el plan; ahora toca implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo primero. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y verifica la implementación

Antes de combinar el código generado, revísalo como cualquier otro cambio. También ejecutarás el sitio para confirmar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la funcionalidad de filtrado en el navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el resultado vacío coinciden con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y pasos siguientes

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot desde una incidencia de GitHub;
- definiste los requisitos en el modo Plan;
- implementaste la funcionalidad con Autopilot;
- revisaste el código;
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo generar código de acuerdo con las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
