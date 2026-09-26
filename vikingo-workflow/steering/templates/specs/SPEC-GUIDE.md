# Guia de Specs por feature

Un Spec formaliza una feature en tres fases: Requirements -> Design -> Tasks.
Los specs son artefactos DEL PROYECTO y viven en `<proyecto>/.kiro/specs/<feature>/`.
Aqui solo guardamos la guia y el molde reutilizable.

## Estructura de un spec

```
<proyecto>/.kiro/specs/<nombre-feature>/
  requirements.md   # Requisitos por caso de uso (formato EARS)
  design.md         # Diseno tecnico: componentes Angular, servicios, contratos
  tasks.md          # Tareas con story points Fibonacci + estimacion + clave JIRA
```

El molde de contenido de cada archivo esta en `_spec-template.md` (invocable como `#spec-template`).

## Como iniciar un spec para una feature

1. Invoca en el chat el flujo:  `#workflow-prd-to-jira`  (y opcionalmente
   `#gitbook-context`, `#estimations`, `#atlassian-defaults`).
2. Indica la feature / caso de uso a trabajar.
3. Kiro lee el PRD + DERCAS + HTML de UI/UX desde GitBook (MCP) y genera
   `requirements.md`, luego `design.md`, luego `tasks.md`.
4. Revisas y das visto bueno fase por fase.
5. Con el OK en `tasks.md`, se crean los issues en JIRA (MCP) y se registra la
   clave de cada issue junto a su tarea.

## Mantener los specs fuera del repositorio (opcional)

Si NO quieres que los specs se suban al repo, agrega al `.gitignore` del proyecto:

```
.kiro/specs/
.kiro/hooks/
```

> Nota: muchos equipos SI versionan los specs porque documentan la feature.
> Decide segun tu politica. Si los versionas, quita esas lineas del .gitignore.

## Buenas practicas

- Un spec por feature; casos de uso identificados como UC-01, UC-02, ...
- Requisitos en EARS ("CUANDO ... EL SISTEMA DEBERA ...").
- Cada tarea trazable a un caso de uso y a un issue de JIRA.
- Foco frontend, pero documentar servicios y estructura de datos necesarios.
