---
title: "Lección 7: Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y MCP de Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y MCP de Playwright para observar la función de filtrado en un navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de control de calidad (QA) bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil de control de calidad reutilizable;
- seleccionarás el agente de QA y revisarás sus conclusiones sobre la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot que se define mediante un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. MCP de Playwright proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar estas capacidades para evaluar los requisitos y presentar los resultados. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la función, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones que debe realizar y los límites que debe respetar.

1. Vuelve a tu Codespace y comprueba que la conversación de filtrado esté en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisar el perfil

Antes de utilizar el agente nuevo, revisa su perfil para comprobar que Copilot haya recogido el proceso y los límites previstos. Así evitarás que un agente incompleto o demasiado general modifique la función cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y comprueba que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza MCP de Playwright.
4. Comprueba que QA presenta pruebas que respaldan sus conclusiones, pide permiso antes de modificar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si el perfil generado omite alguna de estas responsabilidades o restricciones, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar QA con la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la misma conversación de filtrado después de crear el perfil y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista de agentes disponibles. Aunque no aparezca, el comando anterior lo activará.

2. Usa el siguiente prompt para pedir al agente de QA que revise la función:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe que te proporcione.

## Resumen y siguientes pasos

Has incorporado al flujo de trabajo un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- has creado y revisado un perfil de control de calidad reutilizable;
- has seleccionado el agente de QA y revisado sus conclusiones sobre la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación. A continuación, [reunirás todo en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
