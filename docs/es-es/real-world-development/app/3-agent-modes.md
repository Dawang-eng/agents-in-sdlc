---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña función al proyecto, pero los cambios más amplios requieren un proceso más sólido. Por suerte, la aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo dirigido por agentes: partirás de una incidencia para implementar una función, comprobarás que el código es válido y que la función se comporta como se espera y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de la función. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente desde una incidencia de GitHub;
- definirás los requisitos en modo **Plan**;
- implementarás la nueva función con el modo **Autopilot**;
- revisarás el código;
- validarás la función manualmente en un lienzo del navegador.

Mientras continúas con esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación mediante MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia de la lista de tareas pendientes describe la función, pero hay que acordar detalles, como la combinación de categorías, antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los principios fundamentales; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia con los detalles del trabajo que hay que realizar.
2. Crear un plan de lo que se debe desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva función.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código una vez revisado y superado el proceso de integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos son variantes del flujo anterior.

Al mantener este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu rama local `main` está actualizada.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, en la navegación superior del repositorio, selecciona **Issues**. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente creada a partir de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia esté asociada a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal, pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Repite la operación y las comprobaciones que fallaron; no reinstales npm solo porque el agente no lo encuentre.

## Planificar la función de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya conoce la solicitud de la función. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a los datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide que se modifique el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprobar Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador de modo muestra **Autopilot**.

Copilot comenzará a trabajar en la implementación. Verás cómo avanza por el proceso, sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado tú.

## Revisar y verificar la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona.

1. Abre **Changes** y examina la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios sigan las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este worktree, no al de una lección anterior.

## Explorar la nueva función

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en un lienzo del navegador.

1. Utiliza el prompt siguiente para pedir a Copilot que inicie la aplicación y abra la página en el lienzo del navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y comprueba que los resultados coincidan con ella.
4. Combina filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba que se combinan según lo acordado.
5. Prueba una combinación sin juegos coincidentes y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión y cierre el lienzo del navegador:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantener esta sesión para las siguientes lecciones

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, su worktree y su rama para las lecciones 4 a 8. En las siguientes aplicarás instrucciones personalizadas al código de filtrado, añadirás la habilidad y las mejoras de QA y crearás y combinarás la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor de desarrollo y retomarla en la siguiente lección.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para desarrollar y revisar una función. En esta lección:

- has iniciado una sesión de agente desde una incidencia de GitHub;
- has definido los requisitos en modo **Plan**;
- has implementado la nueva función con el modo **Autopilot**;
- has revisado el código;
- has validado la función manualmente en un lienzo del navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
