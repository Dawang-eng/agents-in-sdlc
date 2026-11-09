---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una función pequeña al proyecto, pero los cambios de mayor alcance requieren un proceso más completo. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo de una organización y ayudarnos a desarrollar las funciones adecuadas de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como esperas y, por último, la combinarás en el proyecto.

> [!NOTE]
> Seguirás usando la misma conversación y la misma rama durante todo el flujo de trabajo de la función. Lo habitual sería utilizar ramas o PR diferentes para los distintos tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la función en el modo Autopilot;
- revisarás el código;
- validarás la función manualmente en un navegador de Codespaces.

Mientras desarrollas esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia de la lista de tareas pendientes describe la función, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Empezar por una incidencia registrada que describa el trabajo.
2. Crear un plan con lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva función.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles exactos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de este enfoque.

Si sigues este proceso estándar, te asegurarás de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: colaboras con el agente. Te propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, a continuación, utiliza Autopilot para implementarlo.

## Empezar por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Fíjate en que el prompt ahora empieza por `#7` (o por un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planificar la función de filtrado

La planificación te permite definir cómo se implementará una función o se realizará una tarea antes de encargársela a Copilot. Siempre es buena idea dedicar tiempo a planificar las tareas complejas. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador debajo del prompt muestre **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo quieres que funcione el filtrado. Responde según tus preferencias. No hay respuestas incorrectas.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no se ajuste a tus decisiones.

## Aprobar Autopilot

Ya has redactado y revisado el plan; ahora toca implementarlo. Dejemos que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Comprueba que el indicador del modo, debajo del prompt, muestre **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y verificar la implementación

Como ocurre con cualquier otro código, debes revisar el código generado antes de combinarlo. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para volver al modo Interactive. Comprueba que el indicador ya no muestre **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que has tomado durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores que encuentre.

## Explorar la nueva función

El código parece correcto, pero ¿funciona? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas en el plan.
4. Comprueba que el reinicio de los filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una función. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la función con el modo Autopilot;
- has revisado el código;
- has validado la función manualmente en un navegador de Codespaces.

A continuación, aprenderás a generar código siguiendo las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Modo Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
