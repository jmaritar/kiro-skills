---
inclusion: manual
name: workflow-prd-to-jira
description: Flujo de trabajo reutilizable para transformar PRD y DERCAS de GitBook en specs de Kiro, estimarlos con story points Fibonacci y crear/actualizar tareas en JIRA.
---

# Flujo de trabajo: PRD/DERCAS -> Spec -> JIRA

> ESQUELETO / PLANTILLA. Sustituir los valores entre `<< >>` por los reales.
> Este archivo define el "cómo" trabajamos, es transversal a todos los proyectos.

## Objetivo

Mantener contexto de la feature en curso y llevarla de forma controlada desde la
documentacion (PRD, DERCAS y HTML de UI/UX en GitBook) hasta tareas ejecutables en JIRA.

## Fuentes de conocimiento

- PRD: documento de requerimiento de producto (GitBook).
- DERCAS: documento con los casos de uso.
- HTML de UI/UX: maqueta que refleja lo indicado por PRD y DERCAS.
- Foco principal: FRONTEND. Considerar tambien servicios y estructuracion
  necesarios para cumplir todos los casos de uso.

## Punto de entrada: la epica de JIRA

El origen del contexto normalmente es una EPICA de JIRA. Su descripcion contiene
los links al PRD y a los DERCAS en GitBook.

1. **Leer la epica (MCP `atlassian`)**
   - `getJiraIssue` con la clave (ej. TTDEV-26218) y `view: "evidence"`.
   - Extraer de la descripcion los enlaces a GitBook (PRD / DERCAS / UI-UX).
   - Registrar la feature en el indice `features/INDEX.md`.

## Patron de navegacion de links (SIEMPRE)

Ante cualquier link de GitBook (o de JIRA que apunte a GitBook), seguir SIEMPRE
este metodo, aunque la estructura varie ligeramente:

1. De la URL de GitBook extraer los IDs:
   `/o/<ORG_ID>/s/<SPACE_ID>/...`  -> organizationId y spaceId.
2. `getSpaceById(spaceId)` para obtener titulo y `revision`.
3. `getRevisionById(revisionId, spaceId)` para listar el arbol de paginas.
4. Identificar las paginas de casos de uso (prefijo CU-/UC-) y su jerarquia
   (p.ej. agrupadas bajo un Sprint). Guardar id, titulo y path de cada CU.
5. Leer el contenido con `getPage` (markdown) por cada CU necesario.
6. Estos CU son la FUENTE DE CONTEXTO de la feature; no re-descubrir si ya se
   guardaron en el registro de la feature.

> Nota: space != site. Para spaces usar getSpaceById/getRevisionById, NO
> get_site_structure (ese es solo para sitios publicados).

## Pipeline (pasos)

1. **Leer contexto desde GitBook (MCP `gitbook`)**
   - Aplicar el "Patron de navegacion de links" de arriba.
   - Extraer cada caso de uso con su identificador y criterios de aceptacion.
   - Incorporar el HTML/imagenes de UI/UX como referencia de la interfaz esperada.

2. **Consolidar el contexto de la feature**
   - Resumir alcance, actores, casos de uso y reglas de negocio.
   - Identificar impacto en front, servicios y estructura de datos.

3. **Crear un Spec de Kiro por feature** (en el workspace del proyecto, NO aqui)
   - `requirements.md`: requisitos en formato EARS por caso de uso.
   - `design.md`: diseno tecnico (componentes Angular, servicios, contratos).
   - `tasks.md`: lista de tareas con estimaciones (ver estimations steering).

4. **Estimar** cada tarea: tiempo + story points Fibonacci.
   - Ver la guia en `01-estimations.md`.

5. **Revision y visto bueno**
   - El usuario revisa `tasks.md`. NO crear nada en JIRA sin aprobacion explicita.

6. **Crear tareas en JIRA (MCP `atlassian`)**
   - Un issue por caso de uso o por tarea, segun granularidad acordada.
   - Aplicar defaults de `02-atlassian-defaults.md`.

7. **Ejecucion controlada**
   - Trabajar tarea por tarea. Al iniciar: pasar el issue a "In Progress".
   - Al completar: actualizar estado y agregar comentario con el resultado.

## Reglas

- Nunca crear ni modificar issues de JIRA sin visto bueno explicito del usuario.
- Tratar los tokens y credenciales como secretos. No imprimirlos en respuestas.
- Confirmar antes de acciones de alto impacto (crear en lote, transiciones masivas).
