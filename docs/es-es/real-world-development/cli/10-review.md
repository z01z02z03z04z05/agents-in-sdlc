---
title: "Lección 10 - Conclusiones y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo para Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado del backlog;
- añadido puntuaciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado la función en modo Autopilot y revisado el resultado en modo Interactive;
- guiado al agente con instrucciones personalizadas, actualizado la habilidad `quality-checks` existente y utilizado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Playwright Model Context Protocol (MCP) y utilizado el navegador para explorar el filtrado;
- creado y seleccionado un agente personalizado de garantía de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- revisado todos los cambios de filtrado y autorizado Agent Merge para esa PR;
- explorado comandos slash para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente al agente en la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en la versión actualizada de `main`:

1. **Puntuaciones con estrellas:** muestra la propiedad `starRating` existente y un estado explícito para los juegos sin puntuación en sus tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y aplícalas a la función, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Combinamos ese trabajo en una sola PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: mediante pruebas automatizadas, examinándolo en el navegador y utilizando Copilot para explorar el sitio con un navegador a través de MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos y mostró los resultados con el nuevo formato. El agente QA combinó estos resultados con una revisión de los requisitos y la cobertura de las pruebas antes de abrir la PR.

Las pruebas añadidas deben corregir carencias reales. Si una ejecución de QA concluye que no hacen falta pruebas nuevas, también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a comprobar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir el enfoque antes de desarrollarlo, **Interactive** para participar en cambios concretos y **Autopilot** para tareas con un alcance bien definido. Puedes elegir un modelo rápido para cambios rutinarios y otro más capaz para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas para explorar

Ya has visto el flujo de trabajo principal. Aquí tienes algunas funciones más de CLI que pueden interesarte:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para asignar una tarea al agente de Copilot en la nube.

## Pasos siguientes

La mejor forma de aprender a utilizar una herramienta es seguir usándola. Utilízala para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que has aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
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
