---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En lugar de recorrer la aplicación por tu cuenta, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador real para probarla. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es el protocolo de contexto del modelo (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás una PR para el trabajo de filtrado.

## ¿Qué es el protocolo de contexto del modelo (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. El protocolo de contexto del modelo (MCP) permite que Copilot lo haga: es una forma estándar de que un agente se comunique con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el modelo de lenguaje grande (LLM) y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [protocolo de contexto del modelo (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP administra la comunicación entre el agente y las herramientas externas, como las interfaces de programación de aplicaciones (API) existentes o las herramientas locales, por ejemplo, los paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server):** proporciona acceso a varias API para gestionar repositorios de GitHub. Permite al agente realizar acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp):** proporciona funciones de automatización del navegador mediante Playwright. Permite al agente realizar acciones como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar que se descubran y se contribuya a este ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa detenidamente su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Utiliza únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el Codespace.
2. Deberías encontrar un servidor `github` ya configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La entrada `github` forma parte de la plantilla del proyecto. Por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta mediante el protocolo de transferencia de hipertexto (HTTP) y debes autenticarte con GitHub la primera vez que Copilot use una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible. Esto es necesario en un Codespace, que no dispone de un escritorio para mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador necesario ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y autorízalo para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code iniciará el servidor y te pedirá que lo autorices la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de recorrer la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Es una forma rápida de confirmar que la funcionalidad se comporta como esperas sin salir de la conversación.

El servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de lista), igual que lo hace la tecnología de asistencia. Por eso, una comprobación funcional rápida también sirve como una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Usa el servidor MCP de Playwright para abrir un navegador con la aplicación en ejecución en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos y anota cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Quita ese filtro y aplica uno de editorial. Confirma que la lista solo muestra juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados cumplen ambos.

Describe lo que observes en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que encuentre. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ahora que has confirmado que la funcionalidad funciona, abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero publicarás la rama `filtering-vscode`.

1. Ve al panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, en github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que cree la PR. Puede encontrar la incidencia correspondiente y vincularla a la PR.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** esté seleccionado en el menú desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y relaciónala con la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, puede que se te pida autenticarte. Sigue las instrucciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has:

- aprendido qué es el protocolo de contexto del modelo (MCP) y cómo sus servidores amplían las herramientas de Copilot;
- añadido el servidor MCP de Playwright al espacio de trabajo;
- usado el servidor para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- publicado la rama y abierto una PR para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, crearás un [agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
