# Empaquetado y como compartir esto (Kiro Powers vs Claude Skills)

Guia en español para entender como esta empaquetado esto, como se relaciona con un repo de
"Claude skills/plugins" y como se instala/comparte con el equipo.

---

## 1. ¿Es lo mismo que un repo de Claude Skills?

**La idea es la misma; el formato NO.**

Un repo tipo `claude-skills` (Claude Code) es un *marketplace de plugins*. Este repo
(`powers`) es el equivalente en Kiro: un **marketplace de Powers**, modelado segun el
repo oficial `kirodotdev/powers` (un repo, varios powers, cada uno en su carpeta).

| En un repo Claude Code | Equivalente en Kiro |
|---|---|
| repo marketplace + `marketplace.json` | repo con varias carpetas de power |
| `skills/<x>/SKILL.md` | **Skill** (mismo `SKILL.md`) dentro de `<power>/skills/` |
| `commands/*.md` (slash commands) | **skills** (`/`) o **steering** (`#`) |
| `agents/*.md` | **Agents** (no incluidos aun) |
| manifiesto del plugin | **`POWER.md`** por carpeta de power |
| servidores MCP | **`mcp.json`** por power (aqui no se incluye; sin secretos) |
| `/plugin install` | Powers panel → Import power (ver seccion 4) |

**Conclusion:** tu experiencia con skills se traslada casi 1:1. La capa de empaquetado es un
repo con carpetas de power, cada una con su `POWER.md`.

---

## 2. Estructura del repositorio (marketplace)

```
powers/                          # repo = marketplace de powers
├── README.md                    # indice de powers
├── CONTRIBUTING.md
├── LICENSE
└── vikingo-workflow/            # un power = una carpeta
    ├── POWER.md                 # manifiesto (displayName, author, keywords, steering)
    ├── assets/                  # logo del power
    ├── skills/
    │   ├── comandos/            # /comandos
    │   └── feature-workspace-init/
    └── steering/                # steering (#...) + plantillas
        ├── 00-workflow-prd-to-jira.md
        ├── 01-estimations.md
        ├── 02-atlassian-defaults.md
        ├── 03-gitbook-context.md
        ├── _spec-template.md
        ├── features/INDEX.md
        └── templates/
```

- `POWER.md` identifica el power. Soporta `displayName` (nombre bonito) y `author` (el "by"),
  que es lo que muestra el IDE. Por eso usamos este formato en vez de `plugin.json`.
- Las skills y el steering se agrupan por carpetas dentro del power.
- No se incluye `mcp.json`: la conexion a Atlassian/GitBook la configura cada usuario en su
  `~/.kiro/settings/mcp.json` (los secretos NO viajan en el power).

---

## 3. Reglas de seguridad al compartir (IMPORTANTE)

- **NO** publicar tokens ni credenciales. Van en `~/.kiro/settings/mcp.json`, fuera del repo.
- cloudId y project key NO son secretos, pero puedes dejarlos como placeholders `<< ... >>`.
- **No** compartir fichas de features en curso ni rollback-logs: son contexto interno.
  Solo viaja `features/INDEX.md` vacio y el `_TEMPLATE`.
- Revisa que ningun `SKILL.md`/steering tenga datos de clientes o rutas internas sensibles.

---

## 4. Como se instala / comparte (Kiro IDE)

La activacion es dinamica por `keywords`: Kiro carga el power cuando la tarea coincide.

Instalar desde el panel de Powers:
- **Carpeta local (para probar):** Add Custom Power → Import power from a folder →
  seleccionar la **carpeta del power** (`vikingo-workflow/`, NO la raiz) → Install.
- **GitHub (para compartir):** Add Custom Power → Import power from GitHub →
  `https://github.com/jmaritar/powers/tree/main/vikingo-workflow` → Install.

Actualizar: Powers panel → el power → Check for updates → Install updates.

Publicar: commit y push al repo publico. Cualquier compañero lo instala apuntando a la
carpeta del power.

## 5. Sobre el logo

El IDE aun NO muestra logo para powers personalizados (funcionalidad pendiente de Kiro,
issue kirodotdev/powers#103). El logo (`assets/logo.svg`) se ve en GitHub. Para branding con
logo en el panel, el power debe entrar al registro oficial: https://kiro.dev/powers/submit
