---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda iterar sobre ella e implemente mejoras de accesibilidad, incluidos modos de alto contraste y claro. Vamos a revisar los cambios de código y, si hace falta, dar comentarios a Copilot para que mejore su trabajo.

### Escenario

Como ya se ha señalado varias veces, la inteligencia artificial (IA) generativa no cambia los fundamentos del diseño de software ni del desarrollo y las operaciones (DevOps). Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las propuestas de GitHub Copilot para crear la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que todo siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisa la documentación generada

Empezaremos con la primera solicitud de incorporación de cambios (PR) que ha generado el agente de GitHub Copilot en la nube: añadir documentación al código. Para revisarla, usaremos la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, quizá veas una advertencia que indica que el cortafuegos ha bloqueado GitHub Copilot. Es algo **esperado**, porque Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos para el agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR cuyo título sea parecido a **Add missing documentation** u otro más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes hacer una pausa o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios exactos pueden variar.

   Al revisar los cambios, busca comentarios de documentación de TypeScript (TSDoc), convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. En la sección de comprobaciones de la PR, deberías ver que los flujos de trabajo se han puesto en cola. Si todo va bien, las comprobaciones del proyecto deberían pasar para la única aplicación Astro. Puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

El trabajo con Copilot en una PR no tiene por qué ser unidireccional. También puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otros miembros del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto que debas escribir. Algunas ideas para pedirle cambios a Copilot:

- añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su contenido;
- añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro;
- crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Utiliza una de las ideas anteriores u otra sugerencia de documentación que quieras añadir al código base.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Fíjate en que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado los cambios, debería aparecer un nuevo commit en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot ha generado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** u otro más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. En la sección de comprobaciones de la PR, deberías ver que los flujos de trabajo se han puesto en cola. Si todo va bien, las comprobaciones del proyecto deberían pasar para la única aplicación Astro. Puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace y probar manualmente la funcionalidad de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot, por ejemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente de Copilot en la nube y solicitar cambios. Por ejemplo, podrías pedirle que modifique cuántos juegos relacionados se muestran o que añada encabezados de comentarios y comentarios de documentación TSDoc (recuerda que la incidencia se asignó **antes** de que actualizaras las instrucciones personalizadas). También puedes hacer estas peticiones añadiendo un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisaremos las mejoras de accesibilidad que se han implementado con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado desde el panel de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** u otro más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz de usuario para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias de usuario;
   - los cambios de hojas de estilo en cascada (CSS) o de estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas Accessible Rich Internet Applications (ARIA), navegación con teclado, etc.);
   - cualquier código JavaScript o TypeScript que gestione el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. En la sección de comprobaciones de la PR, deberías ver que los flujos de trabajo se han puesto en cola. Si todo va bien, las comprobaciones del proyecto deberían pasar para la única aplicación Astro. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las mejoras de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas mejoras siguiendo prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora de accesibilidad, puedes mencionar a **@copilot** en un comentario y solicitar cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando le asignas tareas reales de la lista de pendientes y puedes centrarte en otras cosas. Para adquirir el hábito, crea algunas incidencias más en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar el interés de los patrocinadores en la página de detalles del juego;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y manejo de errores a las funciones de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizados del repositorio** para conocer su efecto en el trabajo posterior del agente en la nube.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para accesibilidad, con el que has añadido modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado e iterado sobre las PR del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y próximos pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de Model Context Protocol (MCP) directamente en el IDE.
- 💻 **[Entorno de interfaz de línea de comandos (CLI)](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de control de calidad (QA) personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crear un agente personalizado para otro ámbito;
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
