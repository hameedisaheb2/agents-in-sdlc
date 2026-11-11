---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de control de calidad (QA) claramente definido.

En esta lección:

- explorarás cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil de QA reutilizable;
- seleccionarás el agente de QA y revisarás sus conclusiones según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las otras funciones ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil especificará las comprobaciones y los límites que debe respetar.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en `.github/agents/qa.agent.md`. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad `quality-checks`, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Pídele que indique cada requisito como `pass`, `fail` o `blocked`, con pruebas que respalden el resultado. Debe preguntar antes de modificar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Revisa el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha incluido el proceso y los límites de QA previstos. Así evitarás que un agente incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona al agente un nombre visible claro.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que QA aporta pruebas que respaldan sus conclusiones, pregunta antes de modificar el código de implementación y no confirma cambios ni abre PR.
5. Si faltan responsabilidades o límites en el perfil generado, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA según la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la misma conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. Aun así, existe y el comando anterior lo activará.

2. Envía este prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una PR?
   ```

3. Deja que el agente de QA realice el trabajo.
4. Cuando termine, lee el informe que te proporcione.

## Resumen y pasos siguientes

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- exploraste cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP;
- creaste y revisaste un perfil reutilizable de control de calidad;
- seleccionaste el agente de QA y revisaste sus conclusiones según la incidencia de filtrado.

La implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación ya están listos para revisarse. A continuación, [los reunirás en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
