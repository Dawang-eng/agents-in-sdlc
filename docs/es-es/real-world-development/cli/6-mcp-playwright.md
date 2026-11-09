---
title: "Lección 6: Validar la funcionalidad con MCP de Playwright"
description: "Inspecciona o configura MCP de Playwright en Copilot CLI y úsalo para explorar la función de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, para desarrollar código hay que hacer más que escribir código. También tenemos que trabajar con datos y servicios externos, e incluso proporcionar a Copilot más automatizaciones. Para eso sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y le proporcionan más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor MCP de Playwright si todavía no está disponible;
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz de usuario hay que interactuar con ella. Queremos que Copilot pueda utilizar el sitio web como lo haría un usuario y automatizar así más cambios con la confianza de que funcionan como esperamos.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos disponibles para el agente.

Estos son algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si MCP de Playwright ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. En **Name**, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Comprueba que el tipo de servidor sea **STDIO**. Pulsa <kbd>Enter</kbd> y, después, <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la función con Playwright

Ya has comprobado manualmente que la función se comporta como esperas. Ahora hagamos lo mismo con el servidor MCP de Playwright disponible en Copilot.

1. Utiliza el siguiente prompt para pedir a Copilot que valide la función con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario pedir explícitamente a Copilot que utilice el servidor MCP; normalmente lo deducirá por sí solo. Pero, como sabes qué herramienta debe usar, puedes indicárselo para obtener resultados más coherentes y ahorrar algunos tokens.

2. Observa los distintos pasos que Copilot realiza en el navegador para comprobar que la función funciona.
3. Lee el informe y comprueba que todo se comporta como esperas.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el servidor MCP de Playwright para explorar tu función en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- has añadido el servidor MCP de Playwright si todavía no estaba disponible;
- has pedido al agente que controle un navegador y explore la función de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
