---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

En este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le has pedido a Copilot que documente el código, cree una función de juegos relacionados sobre la que pueda iterar el equipo de diseño e implemente funciones de accesibilidad, incluidos modos de contraste alto y claro. Vamos a revisar los cambios de código y, si es necesario, dar instrucciones a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre queremos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisemos las propuestas de GitHub Copilot para añadir documentación, crear la función de juegos relacionados e implementar las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que todo siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para realizar su trabajo.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que ha generado el agente en la nube de GitHub Copilot: la que añade documentación al código. Para revisarla, utilizaremos la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al revisar la PR, puede que aparezca una advertencia que indica que el firewall bloquea a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes hacer una pausa o repasar lo que has aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios concretos pueden variar.

   Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían pasar para la aplicación Astro. Pueden tardar unos minutos en completarse.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR, igual que harías con otros miembros del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto que debas utilizar. Puedes pedirle, por ejemplo:

- que añada encabezados de comentario al principio de cada archivo de código con una breve descripción de su contenido;
- que añada comentarios de documentación TSDoc a los archivos TypeScript y Astro;
- que cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Puedes usar una de las ideas anteriores o pedirle otro cambio de documentación que quieras ver en la base de código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una nueva sesión para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud de incorporación de cambios.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar las modificaciones.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Volvamos a la PR que Copilot ha generado para resolver la incidencia sobre los juegos relacionados en la página de detalles de cada juego.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían pasar para la aplicación Astro. Pueden tardar unos minutos en completarse.
9. **Opcional:** Puedes cambiar a esta rama en tu codespace para probar manualmente la función de juegos relacionados. Abre el terminal del codespace y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube de Copilot y pedirle que haga cambios. Por ejemplo, podrías pedirle que ajuste cuántos juegos relacionados se muestran o que añada encabezados de comentario y comentarios de documentación TSDoc. Recuerda que esta tarea se asignó **antes** de que actualizaras las instrucciones personalizadas. Como en el ejercicio anterior, puedes pedir estos cambios añadiendo un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de contraste alto que asignaste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar entre modos;
   - el uso del almacenamiento local para guardar las preferencias del usuario;
   - los cambios de CSS o de estilo para los modos de contraste alto y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían pasar para la aplicación Astro. Pueden tardar unos minutos en completarse.
10. **Opcional:** Puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Abre el terminal del codespace y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba en el navegador los modos de contraste alto y claro. Comprueba que funcionan como esperas y que la configuración se conserva al volver a cargar la página.

Fíjate en cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema de accesibilidad o una posible mejora, puedes mencionarlo en un comentario a **@copilot** para pedirle que haga cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando puedes asignarle tareas reales de la lista de pendientes y ocuparte de otra cosa mientras trabaja. Para practicarlo, crea algunas incidencias más en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para registrar el interés de los patrocinadores en la página de detalles del juego;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas del repositorio** para comprobar cómo afectan al trabajo del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de contraste alto y claro.
- **Has utilizado la página de agentes de Copilot como Mission Control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a **@copilot** para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, MCP de Playwright, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

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
