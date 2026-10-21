---
title: "Ejercicio 2 - Añade funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a modificarlo... Y la lista continúa.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura existente del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como pruebas y corrige automáticamente los errores que encuentra. Veamos cómo puedes utilizar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente realiza las tareas en el IDE, con ciclos de respuesta e interacción rápidos. El agente en la nube está pensado para trabajar como un compañero de programación asíncrono, que interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot explora el proyecto, identifica archivos pertinentes y coordina los cambios;
- el modo de agente de GitHub Copilot implementa funciones en la interfaz y en la capa de datos;
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos con el código.

## Escenario

A medida que crece la lista de juegos, quieres que las personas puedan filtrar por categoría y editor. En el ejercicio anterior añadiste un helper de editores. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y sus detalles. Es una aplicación Astro única que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade con pequeños elementos `<script>` de Astro con ámbito local y API DOM estándar.

### Inicia el sitio

Para simplificar el proceso, se ha configurado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace así:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con el comando siguiente:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace de localhost en el terminal del codespace, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en funcionamiento, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo especificará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas capacidades. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no están expresando preferencia por un modelo en particular. Al preparar este taller utilizamos Claude Sonnet 4.5 y lo hemos incluido en estas instrucciones para que las sugerencias de código sean relativamente uniformes y la experiencia sea buena. Sin embargo, como los LLM son probabilísticos, es posible que las sugerencias sean distintas de las que se describen. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot podría utilizar un comando MCP diferente y aun así completar la tarea.

3. Pide a Copilot que te muestre las incidencias del backlog con esta indicación:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que lista todas las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot también ha priorizado las tareas según lo que considera más útil para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de iniciar el agente para que genere código, revisa el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidades, que incluyen:
   - una descripción general de la arquitectura;
   - principios para el diseño de componentes, la capacidad de prueba y la accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar y dividir conjuntos de instrucciones grandes en otros más pequeños, además de proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, hay que realizar al menos tres cambios en la aplicación:

- añadir o perfeccionar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para añadir los controles de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios con el código. ¡El modo de agente de Copilot puede realizar estas tareas! Añadamos la funcionalidad.

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente del backlog con esta indicación:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función. Debería encontrar los helpers de la capa de datos, la interfaz y las pruebas. Después empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás las opciones **Keep** o **Undo** para todos los cambios; no es necesario que las selecciones mientras el agente todavía está trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pregunta si quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre la generación del código y las pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa del modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has visto cómo:

- el modo de agente de GitHub Copilot implementa funciones en la interfaz y en la capa de datos;
- el modo de agente de Copilot explora el proyecto, identifica archivos pertinentes y coordina los cambios;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos con el código.

Ahora vamos a [probar la función con el servidor Playwright MCP][next-lesson] y abrir una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio y pide a Copilot que la añada al sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación: por ejemplo, si quieres que cada persona pueda seleccionar el tamaño de página o prefieres fijarlo;
- al redactar la indicación, proporciona a Copilot la orientación necesaria para crear la implementación que buscas;
- quizá tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
