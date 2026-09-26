---
inclusion: manual
name: gitbook-context
description: Ubicacion del conocimiento en GitBook (organizacion y spaces donde viven el PRD, los DERCAS y las maquetas de UI/UX) para leer el contexto de cada feature.
---

# Contexto de GitBook

> Define donde encontrar el conocimiento de producto y casos de uso.
> SUSTITUIR los `<< ... >>` por los valores reales de tu organizacion.

## Conexion (MCP `gitbook`)

Servidor: `https://mcp.gitbook.com/mcp` (streamable HTTP, OAuth o token).

- **Organizacion**: `<< nombre de la organizacion >>`
- **Organization ID**: `<< tu-organization-id >>`

## Spaces de conocimiento

Los PRD/DERCAS viven por feature en su propio space. Cada feature registra su space
en la ficha correspondiente (`features/<CLAVE>.md`).

| Documento         | Space / ubicacion                          | Notas                                    |
|-------------------|--------------------------------------------|------------------------------------------|
| PRD               | Space `<< space-id de la feature >>`       | Requerimiento de producto + casos de uso |
| DERCAS            | Mismo space o uno distinto                 | La epica indica el space en su descripcion |
| UI/UX (imagenes)  | Adjuntos del mismo space (diagramas BPMN)  | Referencia visual/estructural            |

> Observacion: a veces PRD y DERCAS apuntan al MISMO space; no siempre habra un space
> DERCAS separado. Verificar en la descripcion de la epica.

## Como leer el contexto de una feature

1. Localizar el space y las paginas del PRD/DERCAS de la feature (ver ficha en
   `features/<CLAVE>.md`).
2. Extraer cada caso de uso: identificador, objetivo, actores, flujo, reglas de negocio y
   criterios de aceptacion.
3. Tomar las imagenes/diagramas BPMN como referencia visual/estructural del front.

## Convenciones

- Identificar casos de uso con el prefijo del PRD (`CU-1`, `CU-2`, ...).
- El foco es FRONTEND, pero mapear tambien servicios y estructura de datos requeridos.
- No modificar contenido de GitBook (crear change requests / editar) sin visto bueno.
