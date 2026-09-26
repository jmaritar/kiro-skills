# Como agregar skills a kiro-skills

Este kit es multi-stack. Suma capacidades (Flutter, Angular, otras) sin reorganizar
el arbol. Sigue estas reglas para que todo cargue bien en Kiro.

## 1. Convencion de nombres (Opcion 1: plano por dominio)

- Una skill = una carpeta `skills/<nombre>/SKILL.md`.
- Nombra por dominio con prefijo claro: `feature-...`, `flutter-...`, `angular-...`,
  `worklog-...`. Ej.: `skills/flutter-scaffold/SKILL.md`.
- NO anides skills en subcarpetas por stack (`skills/flutter/scaffold/`): el importador
  descubre skills a un nivel (`skills/<nombre>/SKILL.md`). Manten ese nivel.

## 2. Frontmatter valido (IMPORTANTE — evita el "skill skipped")

El `SKILL.md` empieza con frontmatter YAML entre `---`. Un YAML invalido hace que Kiro
salte la skill ("SKILL.md frontmatter is not valid YAML, so the skill was skipped").

Regla de oro: **el `description` SIEMPRE como bloque `>-`** (o entre comillas). Asi
evitas que `:` (dos puntos + espacio), comillas o parentesis rompan el parser.

Correcto:
```yaml
---
name: flutter-scaffold
description: >-
  Genera el esqueleto de un proyecto Flutter. Usar cuando el usuario pida crear una app
  Flutter, modulos, o estructura base. Palabras gatillo - flutter, app movil, scaffold.
metadata:
  author: jorge.arita
  version: 1.0.0
---
```

Incorrecto (rompe YAML por el ": " dentro del valor plano):
```yaml
description: Crea una app. Palabras gatillo: flutter, app, scaffold
```

Checklist antes de commitear una skill:
- [ ] `name` en kebab-case, unico, igual al nombre de la carpeta.
- [ ] `description` como bloque `>-`, con clausula "Usar cuando..." y palabras gatillo.
- [ ] No usar `:` seguido de espacio dentro de un `description` plano.
- [ ] Referencias/assets bajo `references/` y `assets/` de la propia skill.

## 3. Steering: transversal vs especifico de stack

En `dev.kiro/steering/`:
- **Transversal** (aplica a cualquier proyecto): `#estimations`, `#spec-template`.
- **Especifico de stack o de negocio**: separalo en su propio archivo con `inclusion: manual`
  para que un proyecto de otro stack NO lo herede por defecto. Ej.: si agregas reglas
  de Flutter, crea `10-flutter-conventions.md` con su propio `name`, y no las metas en
  un steering que se cargue siempre.

## 4. Actualiza el indice de comandos

Cada vez que agregues/renombres/elimines una skill o steering, actualiza en el MISMO commit:
1. `skills/comandos/SKILL.md` (tabla, con su categoria).
2. `skills/comandos/references/catalogo.md`.
3. `README.md` (tabla de capacidades y, si aplica, el roadmap).

## 5. Seguridad

- NUNCA commitear tokens/credenciales. Van en `~/.kiro/settings/mcp.json` (fuera del repo).
- Valores de sitio/cliente sensibles: usar placeholders `<< ... >>`.
- No incluir fichas de features en curso ni rollback-logs.

## 6. Probar antes de publicar

1. Powers panel → Add Custom Power → *Import power from a folder* → esta carpeta.
2. Verifica que la skill aparece SIN advertencia y que responde (`/<nombre>`).
3. Recien entonces: commit + push. Los que lo tengan instalado lo actualizan con
   *Check for updates*.

## 7. Versionado

Sube `version` en `plugin.json` al agregar capacidades (semver): `patch` para arreglos,
`minor` para nuevas skills, `major` para cambios que rompan.
