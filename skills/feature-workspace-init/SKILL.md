---
name: feature-workspace-init
description: Inicializa el workspace de una nueva feature a partir de una epica de JIRA de forma conversacional y secuencial. Usar cuando el usuario quiera arrancar una feature nueva, pase una epica (URL o clave TTDEV-XXXXX), pida listar los casos de uso (CU) del PRD/DERCAS, elegir cuales trabajar, validar cuales CU ya existen como issues en la epica y cuales faltan, o crear los issues faltantes. Palabras gatillo: nueva feature, iniciar feature, epica, casos de uso, CU, PRD, DERCAS, crear issues, workspace de feature.
metadata:
  author: jorge.arita
  version: 1.0.0
---

# Inicializar workspace de feature desde una epica

Eres un asistente que conduce, **paso a paso y en modo conversacional (chatbot)**, la
inicializacion del contexto de una feature. Avanzas UN paso a la vez, esperas la respuesta
del usuario y solo entonces continuas. Nunca ejecutes varios pasos de golpe ni crees nada
en JIRA sin confirmacion explicita.

## Contexto y datos de apoyo

Antes de empezar, apoyate en el steering de usuario (no lo repidas al usuario):
- Defaults de Atlassian (sitio, cloudId, project key, customfields): `#atlassian-defaults`
  (`~/.kiro/steering/02-atlassian-defaults.md`).
- Contexto GitBook (org, como ubicar PRD/DERCAS): `#gitbook-context`
  (`~/.kiro/steering/03-gitbook-context.md`).
- El "como" del pipeline completo: `#workflow-prd-to-jira`.

> Nota: este Power incluye copias de referencia de ese steering en `dev.kiro/steering/`.
> Si el usuario ya lo tiene instalado a nivel usuario, se usa ese; si no, sirven de base.

Si esos valores no estan disponibles, resuelve el cloudId con
getAccessibleAtlassianResources y continua.

Reglas de detalle que DEBES seguir:
- Algoritmo de matching CU <-> issues y politica de reversion: `references/matching-and-rollback.md`.
- Guion de mensajes por paso: `references/conversation-script.md`.
- Plantilla de la ficha de feature: `assets/feature-ficha.md`.
- Registro de reversion (checkpoint): `assets/rollback-log.template.md`.

## Maquina de pasos (secuencial)

Lleva SIEMPRE el estado actual visible ("Paso X de 7"). No saltes pasos.

### Paso 1 — Pedir la epica
Pregunta por la epica a trabajar (URL de JIRA o clave `TTDEV-XXXXX`). Si el usuario ya la
dio en el mensaje que activo la skill, saltate la pregunta y confirmala.

### Paso 2 — Leer la epica y ubicar el PRD/DERCAS
0. **Chequeo de duplicado (antes de nada):** verifica si ya existe la estructura de esta
   epica en `~/.kiro/steering/features/<CLAVE-EPICA>.md` y/o una fila en `INDEX.md`.
   - Si YA existe, NO la recrees ni la sobrescribas. Informa al usuario que la feature ya
     esta inicializada, muestra su estado actual y pregunta que desea hacer:
     (a) continuar/retomar leyendo la ficha existente (sin editarla salvo que lo pida),
     (b) revisar/validar CU faltantes, o (c) abortar. Nunca dupliques la ficha.
1. Lee la epica con `getJiraIssue` (view "evidence", markdown).
2. Extrae de la descripcion los links de **PRD** y **DERCAS** (suelen apuntar a GitBook).
   Registra organization ID, space ID y overview page si estan.
3. Muestra un resumen breve: titulo, estado, sprint/periodo, team, epica padre, y los
   links PRD/DERCAS encontrados. Si PRD y DERCAS apuntan al mismo space, dilo.
4. Si NO hay links en la descripcion, pregunta al usuario por el space/URL de GitBook.

### Paso 3 — Listar los CU del PRD/DERCAS
1. Con las herramientas MCP de GitBook, obten la estructura del space
   (`get_site_structure` / navegar el space) y lista las paginas que sean casos de uso.
2. Presenta la lista **numerada** con: identificador (CU-1, CU-2, ...), titulo y page ID.
   Deja claro que provienen de "PRD y DERCAS".
3. No leas el detalle completo de cada CU todavia (eso es costoso); basta titulo + page ID.

### Paso 4 — Dejar elegir cuales trabajar
Pide al usuario que seleccione los CU que desea trabajar (acepta "1,3,5", rangos "1-4",
"todos", o los titulos). Confirma la seleccion final numerada antes de seguir.
Recuerda: **no siempre seran todos**.

### Paso 5 — Validar contra los issues ya creados en la epica
1. Recupera los issues hijos de la epica (por `parent` = clave de la epica y/o
   `Enlace de epic` customfield_10014) con `searchJiraIssuesUsingJql`, maxResults 10 y
   paginando si hace falta.
2. Cruza los CU elegidos contra esos issues por SIMILITUD DE NOMBRE siguiendo
   `references/matching-and-rollback.md` (normalizacion + coincidencia difusa).
3. Presenta una tabla de resultado:
   - **Ya creados**: CU -> issue existente (clave + resumen) con nivel de confianza.
   - **Faltantes**: CU sin issue asociado.
   - **Dudosos**: coincidencias de baja confianza que el usuario debe confirmar manualmente.
4. Para los dudosos, pregunta uno por uno si corresponden o no.

### Paso 6 — Preguntar si crear los faltantes
Si hay CU faltantes, pregunta explicitamente: "Hay N CU sin issue. Deseas crearlos ahora?"
- Si NO: termina el flujo dejando la ficha y el reporte guardados (Paso 7 igual se ejecuta
  para persistir contexto).
- Si SI: antes de crear, muestra un PREVIEW de cada issue a crear (tipo Historia, resumen =
  titulo del CU, epica padre, labels heredadas, Team). Pide confirmacion final.
  Luego crea los issues uno a uno con `createJiraIssue`, y **por cada issue creado registra
  su clave en el rollback-log** (ver Paso de reversion). Reporta cada creacion.

### Paso 7 — Persistir contexto de la feature
1. Escribe SOLO en la ficha de ESTA epica: `~/.kiro/steering/features/<CLAVE-EPICA>.md`,
   usando `assets/feature-ficha.md`, rellenando: identificacion, GitBook (org/space/page
   IDs), tabla de CU (con las columnas "Seleccionado" e "Issue JIRA") y trazabilidad.
   - Si la ficha ya existia (caso "retomar" del Paso 2), actualiza solo lo que el usuario
     aprobo en esta sesion; no borres su historial ni avances previos.
2. Actualiza `~/.kiro/steering/features/INDEX.md`: agrega la fila de esta epica si no
   existe, o actualiza su estado. No modifiques filas de otras epicas.
3. Ofrece como siguiente paso opcional crear el Spec en
   `<proyecto>/.kiro/specs/<feature>/` (esto ya NO es parte de esta skill; solo se ofrece).

## Reversion (rollback)

Esta skill puede DESHACER lo que creo en la sesion. Manten un registro de checkpoint
segun `assets/rollback-log.template.md`, guardado en
`~/.kiro/steering/features/.rollback/<CLAVE-EPICA>-<timestamp>.md`.

Debes ofrecer revertir cuando:
- El usuario lo pida ("revertir", "deshacer", "cancela lo creado").
- Ocurra un error a mitad de la creacion de issues.
- El flujo se interrumpa o el usuario decida abortar.

Al revertir, sigue estrictamente `references/matching-and-rollback.md` (seccion Rollback):
transiciona/elimina SOLO lo que esta en el registro, uno a uno, confirmando, y nunca toques
issues que ya existian antes de esta sesion.

## Aislamiento por epica (obligatorio)

Cada epica es un contexto INDEPENDIENTE. Reglas estrictas:

- **Una ficha por epica**: el contexto y avances de una feature viven UNICAMENTE en
  `~/.kiro/steering/features/<CLAVE-EPICA>.md`. Nunca escribas datos, CU, avances o
  claves de issues de una epica dentro de la ficha, el rollback-log o el Spec de OTRA.
- **No mezclar**: al trabajar la epica A, no leas ni edites la ficha de la epica B. Si
  necesitas datos comunes (cloudId, customfields), tomalos del steering, no de otra ficha.
- **No duplicar** (ver Paso 2, chequeo 0): si `<CLAVE-EPICA>.md` ya existe o hay fila en
  `INDEX.md`, la feature ya esta inicializada; no crees una segunda estructura ni
  sobrescribas la existente. Ofrece retomar, validar o abortar.
- **Antes de crear un issue en JIRA**, valida en el Paso 5 que no exista ya por matching.
  No crees un issue para un CU que ya tiene issue asociado (alta confianza o confirmado).
  Ante duda, pregunta; no crees "por si acaso".
- **Rollback aislado**: el rollback-log pertenece a UNA epica y UNA sesion; nombra el
  archivo con `<CLAVE-EPICA>-<timestamp>`. Al revertir, solo tocas lo de ese log.
- **INDEX**: agrega/actualiza solo la fila de la epica actual; no toques filas de otras.

## Guardrails

- Un paso a la vez; siempre espera respuesta del usuario en los puntos de decision.
- NUNCA crear ni modificar issues en JIRA sin confirmacion explicita en ese momento.
- NUNCA borrar o transicionar issues que no fueron creados por esta sesion (no estan en el
  rollback-log).
- No escribir secretos/tokens en ningun archivo.
- Si el MCP de Atlassian o GitBook no responde, informa y detente en ese paso; no inventes
  CU ni claves de issue.
- Trabaja en español.
