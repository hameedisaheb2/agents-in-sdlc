---
title: "Ejercicio 2 - Añade funcionalidades con Agent Mode de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a cambiarlo. La lista continúa.

Aquí es donde entra en juego Agent Mode de Copilot.

Agent Mode de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo puedes usar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, lo que permite recibir comentarios e interactuar con rapidez. El agente en la nube está diseñado para trabajar de forma asíncrona como un miembro del equipo, interactuando contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- Agent Mode de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios;
- Agent Mode de GitHub Copilot implementa funcionalidades en la interfaz de usuario (UI) y la capa de datos;
- revisar los cambios y las pruebas generados por Agent Mode antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarla por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora completarás con Agent Mode de Copilot el resto del trabajo en la capa de datos, la interfaz de usuario y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM (mapeador objeto-relacional); no hay una API de backend independiente ni un framework de interfaz de usuario en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las API estándar del Modelo de objetos del documento (DOM).

### Inicia el sitio web

Para facilitar el inicio del sitio, se ha preparado una tarea `npm run dev` que ejecuta el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve al Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para preguntarte si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar un enlace localhost desde el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio:** muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego:** al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funcionalidades. Empezaremos revisando el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. Agent puede planificar e implementar cambios en varios archivos de forma autónoma, ejecutar comandos de terminal y usar herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo u otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones. El objetivo es que las sugerencias de código sean relativamente consistentes y ofrezcan una buena experiencia. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando del protocolo de contexto del modelo (MCP) distinto y aun así completar la tarea.

3. Envía el siguiente prompt a Copilot para preguntarle por el backlog de incidencias:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionarás a Copilot como contexto. Utilizarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidades. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad para probarlos y accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, entre ellos:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos complejos en partes más manejables y proporcionar a Copilot ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica de filtrado que falta en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y pasar antes de combinar los cambios en el código base. Agent Mode de Copilot puede realizar estas tareas. Vamos a añadir la funcionalidad.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con este prompt:

   ```plaintext
   Actualiza el sitio para incluir filtros por editorial y categoría, de acuerdo con los requisitos de la incidencia relacionada del backlog de GitHub. Ya existe un helper de editoriales del ejercicio anterior; consérvalo y mejóralo si hace falta mientras completas el trabajo pendiente en la capa de datos, la interfaz de usuario y las pruebas. Asegúrate de que todas las pruebas pasen antes de terminar. El servidor ya está en ejecución, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Deberías ver que encuentra los helpers de la capa de datos, la interfaz de usuario y las pruebas. Después, empieza a modificar los archivos y a ejecutar las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. En función de la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso, quizá aparezcan las opciones **Keep** y **Undo** en el editor. Cuando termine, tendrás una de esas opciones para cada cambio; no hace falta que las selecciones mientras siga trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un diálogo en el panel Copilot Chat que pide confirmar si se pueden ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga varias veces e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar Agent Mode de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. Has aprendido cómo:

- Agent Mode de GitHub Copilot implementa nuevas funcionalidades en la interfaz de usuario y la capa de datos;
- Agent Mode de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios;
- revisar los cambios y las pruebas generados por Agent Mode antes de combinarlos en el código base.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás un pull request.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido en este ejercicio para pedir a Copilot que añada esta funcionalidad al sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación y si permitirás que el usuario elija el tamaño de página o lo fijarás en el código;
- al crear el prompt, proporciona a Copilot las indicaciones necesarias para que implemente la solución que quieres;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y aportar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos Ask, Edit y Agent, y cuándo usar cada uno][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
