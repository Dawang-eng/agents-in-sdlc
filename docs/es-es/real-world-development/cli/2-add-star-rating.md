---
title: "Lección 2: Añadir valoraciones por estrellas, una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador de Codespaces y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y has probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero todavía no se muestra en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación específica de Copilot en una rama de funcionalidad;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para comprobarlo en un navegador de Codespaces;
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya se muestra en la página de detalles del juego. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: es un cambio pequeño e independiente, perfecto para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de la CLI utiliza el repositorio y la rama que estén activos en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas te permiten volver a la misma charla más adelante, mientras que los archivos y la rama siguen formando parte del estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas que utiliza para explorar y editar archivos, y los cambios que puedes revisar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Empecemos una conversación nueva para implementar la función.

1. Vuelve a tu Codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa el siguiente prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una función con Copilot CLI.

## Revisar el diff

Todos los cambios generados por IA deben revisarse antes de combinarlos, por pequeños que sean. Revisemos los cambios directamente en Copilot CLI.

1. Introduce `/diff` y revisa todos los archivos modificados.
2. Comprueba que cada tarjeta de juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` tiene el valor `null`.
3. Comprueba que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando hayas terminado la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot ofrece resultados probabilísticos, no deterministas. El código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por sentado que funciona. Pidamos a Copilot que inicie el sitio para que podamos examinar la interfaz de usuario (UI) actualizada en el navegador abierto mediante Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Comprueba que las tarjetas de juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora crearás una solicitud de incorporación de cambios (PR) para integrarla en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará su URL.
3. Para abrir la PR, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has entregado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación específica de Copilot en una rama de funcionalidad;
- has pedido al agente que haga un pequeño cambio en las tarjetas de juegos;
- has revisado el cambio con `/diff`;
- has ejecutado la aplicación y comprobado la valoración por estrellas en un navegador de Codespaces;
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [empezarás por la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para implementar una función más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
