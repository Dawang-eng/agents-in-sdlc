---
title: "Ejercicio 2: Añadir funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y la ejecución de tareas como las pruebas. Normalmente, el flujo de trabajo de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otro cambio... y así sucesivamente.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para ofrecer experiencias distintas. El modo de agente trabaja en el IDE y permite obtener comentarios e interactuar con rapidez. El agente en la nube funciona como un compañero de programación que trabaja de forma asíncrona y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generadas por el modo de agente de Copilot antes de combinarlos en la base de código.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. En el ejercicio anterior ya has añadido una función auxiliar para obtener editoriales. Ahora completarás el trabajo pendiente de la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en el frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro pequeños y acotados, mediante las API estándar del DOM.

### Iniciar el sitio web

Para facilitar el inicio del sitio, se ha incluido la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Para iniciarlo en GitHub Codespace:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre una ventana de terminal nueva en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama nueva para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador con la dirección `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace a la URL localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de tareas pendientes con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funciones. Empecemos por revisar la lista de tareas pendientes. Al crear el proyecto a partir de la plantilla, se generaron automáticamente varias incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos en el terminal e invoca herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Durante su desarrollo utilizamos Claude Sonnet 4.5 y lo hemos incluido en las instrucciones para intentar que las sugerencias sean coherentes y ofrecer una experiencia adecuada. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es posible que las sugerencias sean distintas de las que aparecen en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre la lista de tareas pendientes con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Fíjate en que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedirle al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones, que incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, incluidos:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar a Copilot ejemplos y plantillas.

## Implementar la función de filtrado

Para completar el filtrado habrá que realizar, como mínimo, tres cambios distintos en la aplicación:

- añadir o ajustar el resto de la lógica de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y asegurarse de que se superen antes de combinar los cambios en la base de código. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la función.

1. Puedes continuar en la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que siga seleccionado **Claude Sonnet 4.5** como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada de la lista de tareas pendientes, utilizando este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función: los helpers de la capa de datos y la interfaz de usuario, además de las pruebas. Después, empieza a modificar los archivos y a ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y de la base de código, puede tardar varios minutos. Durante el proceso, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no tienes que seleccionar esos botones mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva función.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generadas por el modo de agente de Copilot antes de combinarlos en la base de código.

A continuación, [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que implemente la paginación en el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si los usuarios podrán elegir el tamaño de página o si este será fijo;
- al escribir el prompt, proporciona a Copilot las directrices necesarias para implementar la función como quieres;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
