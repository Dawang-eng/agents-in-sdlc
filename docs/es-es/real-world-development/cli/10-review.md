---
title: "Lección 10: Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado ya creada;
- has añadido las valoraciones por estrellas, revisado el resultado en un navegador de Codespaces y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado la función en modo Autopilot y revisado el resultado en modo Interactive;
- has orientado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas obtenidas en el navegador;
- has revisado todo el cambio de filtrado y autorizado Agent Merge para combinar la PR de filtrado;
- has explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones por estrellas:** muestra el valor existente de `starRating` y un estado explícito para los juegos sin valorar en sus tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la función, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Hemos reunido ese trabajo en una sola PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, examinándolo en el navegador y mediante la exploración del navegador que Copilot ha realizado a través de MCP. La habilidad `quality-checks` ha ejecutado las pruebas unitarias, el lint y las comprobaciones de tipos, y ha presentado los resultados en el nuevo formato. QA ha reunido esos resultados con una revisión de los requisitos y de la cobertura de pruebas antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, el resultado también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Puedes reutilizar estas personalizaciones en otras conversaciones y adaptarlas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para pensar en un enfoque antes de empezar a desarrollar, **Interactive** para supervisar de cerca cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para cambios rutinarios y otro más potente para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de manera significativa en el resultado.

## Otras funciones para explorar

Ya has completado el flujo de trabajo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear, pero nunca te habías puesto a hacer. Comparte lo que aprendas con el equipo y aprende también de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
