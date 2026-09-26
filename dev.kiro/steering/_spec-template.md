---
inclusion: manual
name: spec-template
description: Plantilla de referencia de la estructura de un Spec de feature (requirements, design, tasks). No es un spec real; se usa como molde al crear cada feature en el workspace del proyecto.
---

# Plantilla de Spec de feature (referencia)

> Esto es solo un MOLDE de referencia a nivel usuario.
> Los specs REALES se crean en el workspace del proyecto: `.kiro/specs/<feature>/`.
> Se incluye aqui para que el esqueleto quede completo y reutilizable.

## Estructura de carpeta de un spec

```
.kiro/specs/<nombre-feature>/
  requirements.md
  design.md
  tasks.md
```

---

## requirements.md (molde)

```markdown
# Requisitos: << Nombre de la feature >>

## Contexto (desde PRD/DERCAS en GitBook)
- Space/paginas de origen: << enlaces o rutas >>
- Resumen del alcance: << ... >>

## Casos de uso
### UC-01 << titulo >>
- Actor: << ... >>
- Precondiciones: << ... >>
- Flujo principal: << ... >>

## Requisitos (EARS)
### Requisito 1
**User Story:** Como << rol >>, quiero << accion >>, para << beneficio >>.
#### Criterios de aceptacion
1. CUANDO << evento >> EL SISTEMA DEBERA << respuesta >>.
2. SI << condicion >> ENTONCES EL SISTEMA DEBERA << respuesta >>.
```

---

## design.md (molde)

```markdown
# Diseno tecnico: << Nombre de la feature >>

## Frontend (Angular 16)
- Componentes nuevos/modificados: << ... >>
- Modulos y rutas: << ... >>
- Estado / servicios de UI: << ... >>
- Referencia de UI/UX (HTML de GitBook): << ... >>

## Servicios y estructuracion
- Servicios Angular (HttpClient) requeridos: << ... >>
- Contratos / modelos de datos: << ... >>
- Endpoints backend involucrados: << ... >>

## Consideraciones
- Manejo de errores, validaciones, accesibilidad, i18n.
```

---

## tasks.md (molde)

```markdown
# Tareas: << Nombre de la feature >>

- [ ] 1. << tarea >>  (Caso de uso: UC-01) — SP: 3 — Est: << tiempo >>
  - Detalle: << ... >>
  - JIRA: << clave del issue una vez creado >>

- [ ] 2. << tarea >>  (Caso de uso: UC-02) — SP: 5 — Est: << tiempo >>
```
