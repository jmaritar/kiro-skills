# Catalogo detallado de comandos

Referencia ampliada del indice (`SKILL.md`). Todo en español. Mantener sincronizado
cuando se agregue, renombre o elimine una skill o un steering.

---

## 1. Skills (se activan con `/<nombre>`)

Las skills viven en `skills/<nombre>/SKILL.md` dentro de este Power (y quedan como
`~/.kiro/skills/<nombre>/` cuando se instala/copia). Se activan escribiendo `/<nombre>`.

### `/feature-workspace-init`
- **Archivo**: `skills/feature-workspace-init/SKILL.md`
- **Que hace**: conduce paso a paso (modo conversacional, 7 pasos) la inicializacion del
  contexto de una feature a partir de una epica de JIRA.
- **Entradas**: clave de epica (`TTDEV-XXXXX`) o URL de JIRA.
- **Salidas**: ficha `~/.kiro/steering/features/<CLAVE>.md`, fila en `INDEX.md`, y
  opcionalmente issues creados en JIRA (con confirmacion) y un rollback-log.
- **Apoyos que consume**: `#atlassian-defaults`, `#gitbook-context`, `#workflow-prd-to-jira`.
- **Referencias internas**: `references/matching-and-rollback.md`, `references/conversation-script.md`,
  `assets/feature-ficha.md`, `assets/rollback-log.template.md`.
- **Guardrails**: nunca crea/transiciona issues sin confirmacion; puede revertir lo creado
  en la sesion; aislamiento por epica; trabaja en español.

### `/comandos`
- **Archivo**: `skills/comandos/SKILL.md`
- **Que hace**: muestra el indice de todos los comandos disponibles.
- **Referencias internas**: `references/catalogo.md` (este archivo), `references/empaquetado.md`.

---

## 2. Steering (se invoca con `#<nombre>`)

En este Power el steering vive en `steering/`. Al instalarse queda como steering
de usuario invocable con `#<name>`. El `name` del front-matter es el que se usa, no el
nombre de archivo.

| `#name` | Archivo | Aporta |
|---|---|---|
| `#workflow-prd-to-jira` | `00-workflow-prd-to-jira.md` | Pipeline PRD/DERCAS → Spec → JIRA (el "como"). |
| `#estimations` | `01-estimations.md` | Story points Fibonacci + equivalencia en tiempo. |
| `#atlassian-defaults` | `02-atlassian-defaults.md` | Sitio, cloudId, project key TTDEV, tipos de issue, customfields. |
| `#gitbook-context` | `03-gitbook-context.md` | Organizacion y spaces de PRD/DERCAS en GitBook. |
| `#spec-template` | `_spec-template.md` | Molde de un Spec (requirements/design/tasks). |
| `#features-index` | `features/INDEX.md` | Indice central de features. |

### Datos clave incluidos en el steering (referencia rapida)
- **Atlassian**: sitio `https://grupopdc.atlassian.net`, cloudId
  `<< configurar en 02-atlassian-defaults.md >>`, project key `TTDEV`. Tipos: Epic (10042),
  Historia (10041), Subtarea (10045). Customfields: Story Points `customfield_10028`,
  Sprint `customfield_10020`, Enlace de epic `customfield_10014`, Team `customfield_10001`.
- **GitBook**: organizacion PDC, Organization ID `QgLHpttBkiSAaJjDUc7E`.

> Las fichas de feature se generan como `#feature-<clave>` en el `~/.kiro/` del usuario
> (no viajan en este Power). La lista viva esta en `#features-index`.

---

## 3. Plantillas (no se invocan; se copian al proyecto)

Viven en `steering/templates/`. No tienen `name`, no entran con `#`.

| Plantilla | Destino en el proyecto | Uso |
|---|---|---|
| `templates/specs/SPEC-GUIDE.md` | `<proyecto>/.kiro/specs/<feature>/` | Guia para instanciar un Spec. |
| `templates/hooks/jira-start-on-task-begin.json` | `<proyecto>/.kiro/hooks/` | Recordatorio: pasar issue a "In Progress" al iniciar tarea. |
| `templates/hooks/jira-sync-on-task-complete.json` | `<proyecto>/.kiro/hooks/` | Recordatorio: actualizar/transicionar issue al completar. |
| `templates/hooks/angular-build-check-on-save.json` | `<proyecto>/.kiro/hooks/` | Revisar #Problems tras guardar .ts/.html. |

---

## 4. Mantenimiento de este catalogo

Cuando cambie la configuracion, actualizar en el MISMO commit:
1. `skills/comandos/SKILL.md` (tabla resumen).
2. Este archivo (`references/catalogo.md`).
3. Si aplica, el `README.md` del Power.

Checklist de "un comando nuevo bien documentado":
- [ ] Tiene `name` (steering) o carpeta `skills/<nombre>/SKILL.md` (skill).
- [ ] Aparece en la tabla del `SKILL.md` de `comandos`.
- [ ] Tiene fila en este catalogo con archivo, que hace, entradas/salidas.
- [ ] Si es skill, su `description` incluye palabras gatillo ("Usar cuando...").
