---
title: "Lección 8: Crear y combinar la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste tú mismo la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola sesión y rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- examinarás la PR completa de la función y las pruebas de verificación;
- autorizarás Agent Merge solo después de revisarlo y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios desde la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y reorganiza la rama cuando es necesario. Después, combina la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** tú mismo. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar también requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utilizar Agent Merge para gestionar la PR

Ya tienes todo el código creado y revisado. Permitamos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción en el menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge. El agente:

- creará la solicitud de incorporación de cambios con un título y una descripción;
- si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción;
- reorganizará la rama o gestionará posibles conflictos de combinación con la rama de destino;
- supervisará CI para comprobar que se superen todas las comprobaciones;
- supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizará los cambios necesarios para resolverlos;
- podrá combinar automáticamente la PR cuando todo se haya completado correctamente.

Permitamos que Agent Merge también combine la PR cuando se superen todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el proceso y resuelve cualquier bloqueo que se comunique. Antes de continuar, confirma que el estado de la PR sea **Merged**; tener habilitado el permiso de combinación o un agente en ejecución no confirma que se haya completado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación, las pruebas y la validación del código, y ahora también el proceso de la solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- has examinado la PR completa de la función y las pruebas de verificación;
- has autorizado Agent Merge solo después de revisarlo y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
