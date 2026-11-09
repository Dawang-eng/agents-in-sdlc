---
title: "Lección 1: Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y busca la incidencia de filtrado creada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm;
- te autenticarás con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación breve;
- buscarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a utilizar agentes de IA para abordar una lista de tareas pendientes cada vez mayor. Copilot CLI lleva esta capacidad al terminal, donde ya trabajan muchos desarrolladores. En esta lección lo instalarás, te autenticarás y lo dejarás listo para utilizarlo durante el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. GitHub Codespaces ya incluye Node.js, así que utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y que cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o una posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba que se haya instalado:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autenticarse con GitHub

La primera vez que se inicia, Copilot CLI te pide que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones para autenticarte con un código de dispositivo y autorizar Copilot CLI.
3. Copilot CLI muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponda a tu repositorio de Tailspin Toys y selecciona **Yes, and remember this folder for future sessions** para indicar que confías en él.

> [!NOTE]
> En un Codespace, puede que ya te hayas autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está listo.

## Familiarizarte con la herramienta

Los comandos que escribes en el prompt habitual del shell se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, los mensajes en lenguaje natural se envían al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funcione:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y fíjate en cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y comprueba que esté disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que busque la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio breve.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama activa en el terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Normalmente, Copilot CLI te pide autorización antes de utilizar herramientas que no tengan permisos establecidos. En este taller, lo volverás a iniciar con `--yolo`, un atajo aprobado por el usuario que elimina esos avisos dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita los permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y no lo configures nunca como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que utilizarás en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para comprobar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la función.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- has instalado GitHub Copilot CLI con npm;
- te has autenticado con tu cuenta de GitHub;
- has indicado que confías en el repositorio del taller y has probado una conversación breve;
- has buscado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [empezarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
