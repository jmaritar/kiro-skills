---
inclusion: manual
name: estimations
description: Reglas de estimacion con story points en secuencia Fibonacci y su equivalencia en tiempo, para el listado de tareas por caso de uso.
---

# Guia de estimaciones (Story Points Fibonacci)

> ESQUELETO / PLANTILLA. Ajustar la tabla de equivalencia a la realidad del equipo.

## Escala Fibonacci

Usar la secuencia: `1, 2, 3, 5, 8, 13, 21`.

- **1**  Trivial. Cambio muy pequeno y aislado, sin incertidumbre.
- **2**  Sencillo. Bien entendido, poco riesgo.
- **3**  Moderado. Varias partes, algo de coordinacion.
- **5**  Complejo. Multiples componentes o logica no trivial.
- **8**  Muy complejo. Alta incertidumbre o dependencias.
- **13** Demasiado grande. Deberia dividirse en subtareas.
- **21** No estimable como tarea unica. Dividir obligatoriamente.

## Equivalencia orientativa a tiempo

> SUSTITUIR por la equivalencia real del equipo.

| Story Points | Tiempo estimado (<< ajustar >>) |
|--------------|---------------------------------|
| 1            | << ej. 0.5 dia >>               |
| 2            | << ej. 1 dia >>                 |
| 3            | << ej. 1.5 - 2 dias >>          |
| 5            | << ej. 3 - 4 dias >>            |
| 8            | << ej. 1 - 1.5 semanas >>       |
| 13           | << dividir >>                   |
| 21           | << dividir >>                   |

## Criterios para asignar puntos

Considerar: complejidad tecnica, incertidumbre/riesgo, esfuerzo, dependencias,
y cobertura de pruebas necesaria.

## Formato de salida en tasks.md

Cada tarea debe indicar: descripcion, caso de uso asociado, story points y tiempo estimado.

Ejemplo:
- [ ] 1. Crear componente de listado de cobros  (Caso de uso: UC-01) — SP: 3 — Est: 1.5 dias
