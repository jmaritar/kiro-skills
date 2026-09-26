---
inclusion: manual
name: features-index
description: Indice ordenado de las features en las que estamos trabajando, con su epica de JIRA, su documentacion en GitBook (PRD/DERCAS) y su estado.
---

# Indice de Features

Registro central de las features trabajadas. Cada una tiene una ficha detallada
en `features/<clave>.md`. Este indice es el mapa rapido.

| Feature | Epica JIRA | Sprint | GitBook Space | Estado | Ficha |
|---------|------------|--------|---------------|--------|-------|
| _(sin features registradas)_ | | | | | |

## Estados posibles

- **En contexto**: leyendo/consolidando PRD y DERCAS.
- **Spec en curso**: generando requirements/design/tasks.
- **Aprobado**: tasks.md con visto bueno.
- **En JIRA**: issues creados.
- **En ejecucion**: trabajando tareas.
- **Cerrado**: feature completada.

## Como agregar una feature

1. Leer la epica de JIRA -> extraer links a GitBook.
2. Crear ficha `features/<CLAVE-EPICA>.md` a partir de `features/_TEMPLATE.md`.
3. Agregar una fila a esta tabla.
