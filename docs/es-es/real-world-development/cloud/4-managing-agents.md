---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido al agente en la nube de Copilot que se encargue de tres tareas diferentes para mejorar la experiencia de usuario y añadir funciones. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para solicitar una función adicional.

## Escenario

Después de asignar al agente la creación de un modo de contraste alto, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una función para cambiar de modo, parecía lógico incluir esta opción. Vas a dirigir el trabajo del agente para asegurarte de que añada tanto el modo claro como el contraste alto.

## Revisar las tareas del agente en la nube de Copilot

Veamos el estado actual de todas las tareas que hemos asignado al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación de la base de código;
   - generar API para modificar juegos;
   - añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, como:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar los distintos pasos necesarios para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios de la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir la tarea.

## Dirigir el agente en la nube

Ya has visto las tareas activas. Ahora vamos a pedirle a Copilot que añada también la opción del modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que Copilot haya elegido y del estado actual del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Fíjate en que Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como ocurría antes, la tarea tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde Mission Control puedes seguir las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir las tareas en curso para ajustar su alcance o proporcionar instrucciones adicionales.

Has explorado estos conceptos:

- has utilizado Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- has redirigido una sesión en curso para solicitar una función adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
