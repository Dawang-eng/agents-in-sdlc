---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funciones que llevan tiempo en la lista de tareas pendientes porque nunca ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código;
- cómo asegurarte de que las operaciones se realicen de forma segura;
- por qué es importante definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. Como primer paso, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre otros títulos relacionados, es decir, juegos de la misma categoría, para que los usuarios puedan seguir explorando. De momento no necesita una implementación pulida: solo quiere algo que pueda utilizar para realizar pruebas de aceptación de la experiencia de usuario (UX). Aunque esta tarea bloquea el trabajo, hay otras incidencias con mayor prioridad.

Estas son tareas que pueden quedar relegadas fácilmente y que se pueden asignar al agente en la nube de Copilot. El agente trabaja en ellas de forma asíncrona; mientras tanto, puedes ocuparte de otras tareas y, cuando vuelvas, revisar su trabajo y comprobar que el resultado sea el esperado.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Igual que cuando trabajas con otra persona, puedes hacerlo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una solicitud de incorporación de cambios en borrador para hacer un seguimiento del trabajo, configura el entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras está en curso o una vez completada. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de reflexión. Incluso las tareas que parecen sencillas pueden ser complejas cuando se examinan a fondo, así que [define cuidadosamente el alcance de las tareas del agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque sobre la marcha. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para que después puedas observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace si ya tienes uno iniciado:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, fíjate en la PR resultante: los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario proceden de estos archivos de instrucciones. Volveremos a señalarlo al revisar las PR en una lección posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

Para crear código suele ser necesario configurar un entorno específico y ejecutar algunos scripts de preparación, independientemente de quién vaya a realizar el trabajo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. Este flujo se ha configurado de antemano para facilitar el taller y permitirte explorar esta función. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

Este archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Fíjate en el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente sin tener que esperar a que lo ejecute Copilot.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar sin resolver, lo que ralentiza el trabajo y dificulta mantener la base de código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una incidencia perfecta para asignar al agente en la nube de Copilot. Trabajará en segundo plano para generar la documentación necesaria. En una lección posterior volverás a revisar su trabajo.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un minuto o dos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno (ejecutando el flujo de trabajo que se ha descrito antes), analizando el proyecto y determinando cómo abordar mejor el problema.

13. Revisa la lista y las tareas que realizará.
14. Desplázate hacia abajo por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Fíjate en que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado, y crea código.

La tarea probablemente tardará varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es que pueda trabajar de forma asíncrona para que tú puedas centrarte en otras tareas. Aprovecharemos esa función y, mientras asignamos otra tarea al agente en la nube, nos dedicaremos a añadir funciones a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles del juego

Una de las ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo. Así puedes ocuparte de unas tareas mientras el agente trabaja en otras. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero ese tiempo se podría aprovechar en otras tareas. Asignémosela al agente en la nube de Copilot.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás una serie de 👀 en el primer comentario de la incidencia. Indica que Copilot está trabajando en ella.

![Emoji de ojos que indica que Copilot está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot funciona de forma parecida a un ingeniero de software; no hace falta que lo supervises continuamente, sino que puedes revisar su trabajo cuando termine. Ahora vamos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funciones o migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código;
- cómo asegurarte de que las operaciones se realicen de forma segura;
- por qué es importante definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. [El agente en la nube de Copilot también puede utilizar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
