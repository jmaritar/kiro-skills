# Empaquetado y como compartir esto (Kiro Power vs Claude Skills)

Guia en español para entender que es este Power, como se relaciona con un repositorio de
"Claude skills/plugins" y como se instala/comparte con el equipo.

---

## 1. ¿Es lo mismo que un repo de Claude Skills?

**La idea es la misma; el formato NO.**

Un repo tipo `claude-skills` (Claude Code) es un *marketplace de plugins* con su propio
formato de manifiestos (`.claude-plugin/marketplace.json`, `plugin.json` de Claude). Kiro
reutiliza el concepto de **skill** (el `SKILL.md` es casi identico), pero se empaqueta como
un **Power** que sigue el estandar abierto Agent Plugins. Equivalencias:

| En un repo Claude Code | Equivalente en Kiro | En este Power |
|---|---|---|
| `skills/<x>/SKILL.md` | **Skill** (mismo `SKILL.md`) | `skills/<x>/SKILL.md` |
| `skills/<x>/references/`, `assets/`, `scripts/` | Igual | idem |
| `commands/*.md` (slash commands) | **skills** (`/`) o **steering** (`#`) | `skills/` + `dev.kiro/steering/` |
| `agents/*.md` | **Agents** | (no incluidos aun) |
| `.claude-plugin/plugin.json` + `marketplace.json` | **`plugin.json`** (Agent Plugins) | `plugin.json` en la raiz |
| servidores MCP | **`mcp.json`** (Agent Plugins) | (no incluido; sin secretos) |
| `/plugin install` | Powers panel → Import power | ver seccion 4 |

**Conclusion:** tu experiencia con skills se traslada casi 1:1. Lo unico distinto es la
capa de empaquetado: en Claude es `marketplace.json`; en Kiro es `plugin.json` (Power).

---

## 2. Estructura de este Power

```
kiro-skills/
├── plugin.json                 # manifiesto Agent Plugins (name, version, keywords...)
├── README.md                   # que es, como instalar, lista de comandos
├── skills/
│   ├── comandos/               # /comandos  (indice)
│   └── feature-workspace-init/ # /feature-workspace-init
└── dev.kiro/
    └── steering/               # steering (#...) + plantillas, copia base
        ├── 00-workflow-prd-to-jira.md
        ├── 01-estimations.md
        ├── 02-atlassian-defaults.md
        ├── 03-gitbook-context.md
        ├── _spec-template.md
        ├── features/INDEX.md
        └── templates/
```

- `plugin.json` es lo unico obligatorio; identifica el Power y sus `keywords` de activacion.
- Las skills y el steering se agrupan por carpetas (no se "declaran" dentro de plugin.json).
- No se incluye `mcp.json`: la conexion a Atlassian/GitBook la configura cada usuario en su
  `~/.kiro/settings/mcp.json` (los secretos NO viajan en el Power).

---

## 3. Reglas de seguridad al compartir (IMPORTANTE)

- **NO** publicar tokens ni credenciales. Van en `~/.kiro/settings/mcp.json`, fuera del repo.
- cloudId y project key NO son secretos, pero puedes dejarlos como placeholders `<< ... >>`
  en la version compartida (asi cada equipo pone lo suyo).
- **No** compartir fichas de features en curso ni rollback-logs: son contexto interno.
  Este Power solo trae `features/INDEX.md` vacio y el `_TEMPLATE`.
- Revisa que ningun `SKILL.md`/steering tenga datos de clientes o rutas internas sensibles.

---

## 4. Como se instala / comparte (Kiro IDE)

La activacion es dinamica por `keywords`: Kiro carga el Power cuando la tarea coincide.

Instalar desde el panel de Powers (icono de Powers en Kiro):
- **Desde carpeta local (para probar):** Add Custom Power → Import power from a folder →
  seleccionar la carpeta del Power → Install.
- **Desde GitHub (para compartir):** Add Custom Power → Import power from GitHub →
  `https://github.com/jmaritar/kiro-skills` → Install. El repo debe tener `plugin.json`
  valido en la raiz.

Actualizar: Powers panel → el power → Check for updates → Install updates.

Publicar: `git add plugin.json README.md skills/ dev.kiro/`, commit y push al repo publico.
Cualquier compañero lo instala con "Import power from GitHub".
