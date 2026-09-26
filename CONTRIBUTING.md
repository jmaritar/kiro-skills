# Como contribuir a Vikingo Skills

Este repo es un **marketplace de Powers** (modelo `kirodotdev/powers`): un repo, varios
powers, cada uno en su carpeta. Aqui va como agregar un power nuevo y como sumar skills
dentro de un power.

## 1. Agregar un power nuevo

1. Crea una carpeta en la raiz con el nombre del power en kebab-case, p.ej. `vikingo-flutter/`.
2. Dentro, crea `POWER.md` (manifiesto). Usa este molde:

```markdown
---
name: "vikingo-flutter"
displayName: "Vikingo Flutter"
description: "Que hace el power, en una linea."
keywords: ["flutter", "app movil", "scaffold", "pdc", "vikingo"]
author: "Vikingo IA"
---

# Cuando usar este power
...

# Skills incluidas
- `mi-skill` (`/mi-skill`) — que hace.

# Cuando cargar cada steering
- Escenario -> `./steering/archivo.md`
```

3. Agrega, si aplica: `skills/`, `steering/`, y `mcp.json` (solo si el power necesita MCP).
4. Registra el power en el `README.md` raiz (seccion "Powers disponibles").

> `POWER.md` soporta `displayName` (nombre bonito con espacios) y `author` (el "by"). Por eso
> usamos este formato en vez de `plugin.json` (donde `name` debe ser kebab-case y no hay
> displayName). Ambos formatos son validos en Kiro.

## 2. Agregar una skill dentro de un power

- Una skill = `<'power'>/skills/<nombre>/SKILL.md`. Nombra por dominio: `flutter-...`, `angular-...`.
- NO anides skills en subcarpetas por stack; manten `skills/<nombre>/SKILL.md` a un nivel.

### Frontmatter valido (IMPORTANTE — evita el "skill skipped")

El `SKILL.md` empieza con frontmatter YAML entre `---`. Un YAML invalido hace que Kiro
salte la skill ("SKILL.md frontmatter is not valid YAML, so the skill was skipped").

Regla de oro: **el `description` SIEMPRE como bloque `>-`** (o entre comillas). Asi evitas
que `:` (dos puntos + espacio), comillas o parentesis rompan el parser.

Correcto:
```yaml
---
name: flutter-scaffold
description: >-
  Genera el esqueleto de un proyecto Flutter. Usar cuando el usuario pida crear una app
  Flutter. Palabras gatillo - flutter, app movil, scaffold.
metadata:
  author: jorge.arita
  version: 1.0.0
---
```

Incorrecto (rompe YAML por el ": " dentro del valor plano):
```yaml
description: Crea una app. Palabras gatillo: flutter, app, scaffold
```

## 3. Steering: transversal vs especifico de stack

- **Transversal** (cualquier proyecto): estimaciones, molde de Spec.
- **Especifico de stack/negocio**: en su propio archivo con `inclusion: manual`, para que
  otro stack NO lo herede por defecto.

## 4. Seguridad

- NUNCA commitear tokens/credenciales. Van en `~/.kiro/settings/mcp.json` (fuera del repo).
- Valores de sitio/cliente sensibles: placeholders `<< ... >>`.
- No incluir fichas de features en curso ni rollback-logs.

## 5. Probar antes de publicar

1. Powers panel → Add Custom Power → *Import power from a folder* → la carpeta del power
   (p.ej. `vikingo-workflow/`, NO la raiz del repo).
2. Verifica: nombre (displayName), "by" (author), skills SIN advertencia y que responden.
3. Recien entonces: commit + push. Para instalar desde GitHub se apunta a la carpeta:
   `https://github.com/jmaritar/kiro-skills/tree/main/<power>`.
