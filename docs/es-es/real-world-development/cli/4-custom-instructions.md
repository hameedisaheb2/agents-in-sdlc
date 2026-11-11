---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot tenga esa orientación disponible. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que utilizó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizarás el archivo de instrucciones para que se respeten los estándares de programación;
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su finalidad, sus parámetros y los valores que devuelven mediante TSDoc/JSDoc; si se puede inyectar un argumento `db`, hay que documentarlo.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices actuales de formato y lint.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor tu estilo de programación y tus requisitos. Puedes especificar convenciones, bibliotecas y los tipos de comentarios que quieres incluir. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes del proyecto.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> Los formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explora lo que hay antes de hacer cambios para poder observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Examina la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` de la parte superior, que define un glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Esto permite dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar ejemplos que Copilot puede seguir.

## Actualiza los archivos de instrucciones según las directrices del equipo

Los archivos actuales son un buen comienzo, pero aún falta algo. Modifica el archivo principal `copilot-instructions.md` para que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - Todo el código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Usa las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y retómala para que Copilot cargue las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y las directrices de código. ¿Puedes actualizar el código que generaste para que las cumpla?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En esta lección:

- exploraste cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizaste el archivo de instrucciones para que se respeten los estándares de programación;
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
