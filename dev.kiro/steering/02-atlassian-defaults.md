---
inclusion: manual
name: atlassian-defaults
description: Valores por defecto de Atlassian JIRA (sitio, cloudId, project key, tipos de issue) para reducir llamadas de descubrimiento al crear y actualizar tareas.
---

# Defaults de Atlassian / JIRA

> Valores del sitio de JIRA. Reduce llamadas de descubrimiento
> (getAccessibleAtlassianResources) y evita ambiguedad.
> SUSTITUIR los `<< ... >>` por los valores reales de tu sitio antes de usar.

## Conexion (MCP `atlassian`)

- **Sitio**: `<< https://tu-sitio.atlassian.net >>`
- **cloudId**: `<< tu-cloudId >>`
  (usar este valor directamente; solo llamar a getAccessibleAtlassianResources si falla)
- **Project key de JIRA**: `<< TTDEV >>`
- **maxResults / limit**: usar `10` en TODAS las busquedas JQL.

## Tipos de issue (project << TTDEV >>)

- **Epic**: id `<< 10042 >>` (nombre "Epic")
- **Historia**: id `<< 10041 >>` (equivalente a Story) — tipo por defecto para casos de uso
- **Subtarea**: id `<< 10045 >>`

## Convenciones de creacion de issues

- **Tipo de issue por defecto**: `Historia`
- **Epic contenedor por feature**: usar la epica de la feature (ej. `<< TTDEV-XXXXX >>`).
  El vinculo a la epica se hace por `parent` o por `Enlace de epic` (customfield_10014).
- **Labels por defecto**: heredar los de la epica.
- **Team**: `<< nombre del team >>` (customfield_10001).
- **Asignado por defecto**: sin asignar (o el accountId que indique el usuario).

## Mapeo de campos (customfields)

> Los IDs de customfield son especificos por sitio. Verificar en el tuyo.

- **Story Points**: `<< customfield_10028 >>`
- **Sprint**: `<< customfield_10020 >>`
- **Enlace de epic**: `<< customfield_10014 >>`
- **Team**: `<< customfield_10001 >>`
- **Tipo de Tarea**: `<< customfield_XXXXX >>`
- **Semana Objetivo**: `<< customfield_XXXXX >>`
- **Estimacion de tiempo**: usar Original Estimate del time tracking. Confirmar con el
  equipo si se registra en otro campo.

## Reglas de escritura

- No crear ni transicionar issues sin visto bueno explicito del usuario.
- Al iniciar una tarea: transicionar a `In Progress` (verificar transiciones reales con
  listJiraIssueTransitions; los nombres de estado del sitio pueden estar en español).
- Al completar: transicionar al estado de cierre correspondiente y agregar comentario con
  el resultado.
- Preferir crear issues especificos por nombre; evitar operaciones en lote sin confirmar.

## Seguridad

- NUNCA guardar tokens/credenciales en este archivo. Los secretos van en
  `~/.kiro/settings/mcp.json` (o via OAuth), fuera del repositorio.
