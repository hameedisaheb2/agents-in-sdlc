---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Hay varias formas de compartirlo con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- generarás la primera parte de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volverás a enviar el mismo prompt y observarás cómo el código generado incorpora el nuevo estándar;
- confirmarás los cambios de las instrucciones y de la parte de filtrado en `main` para que Copilot utilice las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la *tendencia* de los cambios en su comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario (UI) debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios de documentación de TypeScript (TSDoc).
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Los archivos de instrucciones te ayudarán a proporcionar a Copilot la información adecuada para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar a Copilot para que genere sugerencias y fragmentos de código más relevantes. Puedes especificar tus convenciones de programación, las bibliotecas preferidas e incluso los tipos de comentarios que quieres incluir. También puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y aportar contexto a nivel de tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes de chat o de la interfaz de línea de comandos (CLI), como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivo concretos. Permiten definir directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` para generar código.
>
> Copilot también [admite archivos de instrucciones basados en otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

En este taller no cabe una explicación completa sobre cómo crear archivos de instrucciones. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como qué se está desarrollando, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes de Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural y con claridad. Incluye ejemplos de cómo debería ser el código y de qué se debe evitar.

No hay una única forma correcta de crear archivos de instrucciones, igual que tampoco hay una única forma de usar la IA. La experimentación te ayudará a encontrar lo que funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que encuentres archivos para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando lo indicará cuando corresponda.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a revisar los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que define un glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, los que coincidan con `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentario de los archivos.

## Crea una rama para los cambios

Para seguir algunas buenas prácticas, crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

Para ver el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe estar el helper.
3. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

5. Copilot explora el proyecto y aplica actualizaciones de código, normalmente en el archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios de documentación TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo sigue siendo observar la mejora de *consistencia* después de actualizar las instrucciones.

## Añade un nuevo estándar del repositorio

Como se ha explicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade a las normas de programación del repositorio un estándar que mejore las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero todavía no incluye una regla de documentación en el código. Por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas Markdown justo después de los estándares existentes para indicar a Copilot que incluya encabezados de comentario y comentarios de documentación TSDoc:

   ```markdown
   - Todas las funciones exportadas deben tener un comentario TSDoc que describa su finalidad, sus parámetros y el valor que devuelven.
   - Antes de las importaciones o de cualquier código, añade al archivo un bloque de comentarios que explique su finalidad.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para directrices generales o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a enviar el prompt y observa el cambio

Ahora que has actualizado el estándar del repositorio, vuelve a enviar el mismo prompt para comparar el resultado.

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar la conversación.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma Tailspin Toys Crowd Funding.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editoriales.
    */
   ```

Acabas de orientar a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real que utilizarás como base en el siguiente ejercicio.

## Confirma las instrucciones y sube la rama

Los archivos de instrucciones se administran con el mismo sistema de control de versiones que cualquier otro recurso del repositorio. Confirma los cambios y sube la rama.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que están presentes las actualizaciones de instrucciones y del helper ejecutando:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya cambiado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otras actualizaciones relacionadas (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina un pull request

Con la rama ya subida, crea un pull request (PR) y relaciónalo con la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una PR para cerrarla y, después, combinarla mediante las herramientas de GitHub ya conectadas al proyecto.

> [!NOTE]
> En este ejercicio, Copilot actuará por primera vez en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones para permitirlo. En un ejercicio posterior conocerás el funcionamiento de esta conexión, el Model Context Protocol (MCP).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y asegúrate de que **Agent** esté seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. La descripción debe incluir los cambios en las instrucciones y la nueva base del helper de editoriales:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando Copilot te pida **Allow** para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la PR, pide a Copilot que la combine en `main` y que actualice la rama local con el código más reciente:

   ```
   Combina la PR en main. Después, vuelve a main en el repositorio local y descarga el código más reciente para dejarlo actualizado.
   ```

7. Cuando Copilot te pida **Allow** para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado un pull request con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *actuales*;
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una nueva funcionalidad al código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
