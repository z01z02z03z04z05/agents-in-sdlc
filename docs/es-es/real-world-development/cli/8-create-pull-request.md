---
title: "Lección 8 - Crea y combina la PR de la función"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de garantía de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste tú la solicitud de puntuación con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la función, las actualizaciones de instrucciones, la actualización de la habilidad y el agente QA en varias solicitudes de incorporación de cambios. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola conversación y una sola rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación;
- revisarás el cambio completo de la función y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el resto del trabajo de la PR, sin quitar al desarrollador el control de la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve lo que impide combinar la PR: corrige comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a basar la rama cuando es necesario. Después habilita la combinación automática de GitHub si el repositorio la permite.

Hasta ahora, tú has seleccionado **Merge pull request**. Agent Merge puede asumir esa responsabilidad. Antes de habilitarlo, revisa el trabajo y confirma que está listo.

## Utiliza Agent Merge para gestionar la PR

Con todo el código creado, revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve al Codespace.
2. Abre el cuadro de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, las reglas de protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y, cuando el repositorio lo permita, completa manualmente la combinación que ya hayas revisado.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación;
- has revisado el cambio completo de la función y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge solo después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
