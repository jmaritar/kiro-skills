---
name: comandos
description: >-
  Indice directo, en español, de todos los comandos y utilidades del Power Vikingo Skills
  de PDC (skills que se activan con "/", steering que se invoca con "#", plantillas y flujos).
  Usar cuando el usuario pregunte que comandos hay, que puede hacer, como se invoca algo,
  "ayuda", "menu", "que skills tengo", "lista de comandos" o cuando no sepa como arrancar
  una tarea.
metadata:
  author: jorge.arita
  version: 1.2.0
---

# Indice de comandos disponibles

Este es el **kit de trabajo de PDC** para Kiro: multi-stack (Angular, Flutter y mas),
en español. Cuando se active esta skill, responde en **español** y de la forma **mas
directa posible**: muestra el catalogo en tablas agrupadas por categoria, sin rodeos.
Si el usuario pidio algo especifico ("como inicio una feature?"), responde primero con
el comando exacto y luego el resto como referencia.

Para el detalle ampliado de cada comando, consulta `references/catalogo.md`.
Para entender como se empaqueta y comparte esto (Power / plugin), ve `references/empaquetado.md`.

## Como se invoca cada cosa (lo minimo que hay que saber)

- **Skills** → se activan escribiendo `/<nombre>` en el chat. Cargan un flujo completo.
- **Steering manual** → se invoca escribiendo `#<nombre>`. Inyecta contexto/reglas.
- **Plantillas** → NO se invocan; son archivos que se copian al proyecto cuando se usan.

## Skills (se activan con `/`)

### General
| Comando | Que hace |
|---|---|
| `/comandos` | Muestra este indice de comandos. Empieza aqui. |

### Feature / JIRA
| Comando | Que hace |
|---|---|
| `/feature-workspace-init` | Inicia el workspace de una feature desde una epica de JIRA, de forma conversacional: lee la epica, ubica PRD/DERCAS en GitBook, lista los casos de uso (CU), deja elegir cuales trabajar, valida cuales ya existen como issues y ofrece crear los faltantes (con confirmacion y rollback). |

### Otros stacks (Flutter, Angular, ...)
| Comando | Estado |
|---|---|
| `/flutter-*` | Por venir (ver Roadmap). |
| `/angular-*` | Por venir (ver Roadmap). |

> Si el usuario pregunta por un stack que aun no tiene skill, dilo claramente y no
> inventes comandos; ofrece el flujo general o crear la skill (ver CONTRIBUTING.md).

## Steering (se invoca con `#`)

Incluido en este Power bajo `steering/`. Al instalarse, queda invocable con `#`.

| Comando | Que aporta al contexto |
|---|---|
| `#workflow-prd-to-jira` | El "como" del pipeline completo: PRD/DERCAS (GitBook) → Spec de Kiro → estimacion → issues en JIRA. |
| `#estimations` | Reglas de estimacion con story points Fibonacci y su equivalencia en tiempo. |
| `#atlassian-defaults` | Defaults de JIRA: sitio, cloudId, project key (TTDEV), tipos de issue y customfields. |
| `#gitbook-context` | Donde vive el conocimiento en GitBook (organizacion y spaces del PRD/DERCAS). |
| `#spec-template` | Molde de referencia de un Spec (requirements / design / tasks). |
| `#features-index` | Indice central de features trabajadas (epica, sprint, space, estado). |

> Nota: por cada feature nueva que se inicializa se genera una ficha invocable como
> `#feature-<clave-epica>` (queda en el `~/.kiro/` del usuario, no en este Power).
> Revisa `#features-index` para la lista viva.

## Plantillas (se copian al proyecto; no se invocan con `#`)

| Plantilla | Uso |
|---|---|
| `steering/templates/specs/SPEC-GUIDE.md` | Como instanciar un Spec por feature en `<proyecto>/.kiro/specs/<feature>/`. |
| `steering/templates/hooks/*.json` | Hooks de ejemplo (JIRA start/sync, Angular build check) para copiar a `<proyecto>/.kiro/hooks/`. |

## Flujos rapidos (recetas)

- **Arrancar una feature nueva** → `/feature-workspace-init` y pasa la epica (`TTDEV-XXXXX` o URL).
- **Entender el proceso completo** → `#workflow-prd-to-jira`.
- **Estimar tareas** → `#estimations`.
- **Crear un Spec** → sigue `templates/specs/SPEC-GUIDE.md` (usa `#spec-template` como molde).
- **Ver features registradas** → `#features-index`.

## Roadmap (proximas capacidades)

- `flutter-*` — skills para proyectos Flutter (scaffolding, patrones, revision).
- `angular-*` — utilidades especificas de Angular mas alla del flujo actual.
- `worklog` / `daily` — bitacora de trabajo y generacion del Daily.

Para agregar una capacidad nueva, sigue `CONTRIBUTING.md` en la raiz del kit.

## Reglas al responder este indice

- Siempre en español y directo.
- Este es un kit multi-stack: si aun no hay skill para el stack que pide el usuario
  (p.ej. Flutter), dilo y ofrece el flujo general o crear la skill; no inventes comandos.
- Manten este indice sincronizado: si se agrega/renombra una skill o un steering,
  actualiza tambien `references/catalogo.md` y el `README.md`.
