# kiro-skills — Power de PDC para Kiro

Power (plugin) para [Kiro](https://kiro.dev) que empaqueta el flujo de trabajo de PDC:
**PRD/DERCAS (GitBook) → Spec de Kiro → issues en JIRA**, con skills conversacionales,
steering de contexto y plantillas reutilizables. Todo en español.

Sigue el estandar abierto [Agent Plugins](https://agent-plugins.org/) (`plugin.json`).

## ¿Qué incluye?

### Skills (se activan con `/`)

| Comando | Que hace |
|---|---|
| `/comandos` | Indice directo, en español, de todos los comandos disponibles. |
| `/feature-workspace-init` | Inicializa el contexto de una feature desde una epica de JIRA (lee epica, ubica PRD/DERCAS en GitBook, lista casos de uso, valida issues existentes y ofrece crear los faltantes, con confirmacion y rollback). |

### Steering (se invoca con `#`)

| Comando | Aporta |
|---|---|
| `#workflow-prd-to-jira` | El pipeline completo PRD/DERCAS → Spec → JIRA. |
| `#estimations` | Story points Fibonacci + equivalencia en tiempo. |
| `#atlassian-defaults` | Defaults de JIRA (sitio, cloudId, project key, tipos, customfields). |
| `#gitbook-context` | Organizacion y spaces de PRD/DERCAS en GitBook. |
| `#spec-template` | Molde de un Spec (requirements/design/tasks). |
| `#features-index` | Indice central de features trabajadas. |

### Plantillas (se copian al proyecto)

- `dev.kiro/steering/templates/specs/SPEC-GUIDE.md` — como instanciar un Spec.
- `dev.kiro/steering/templates/hooks/*.json` — hooks de JIRA y verificacion Angular.

## Estructura

```
kiro-skills/
├── plugin.json                 # manifiesto Agent Plugins
├── README.md
├── skills/
│   ├── comandos/
│   └── feature-workspace-init/
└── dev.kiro/
    └── steering/               # steering (#...) + plantillas
```

## Instalacion (Kiro IDE)

Abre el panel de **Powers** en Kiro (icono de Powers) → **Add Custom Power**:

- **Probar en local:** *Import power from a folder* → selecciona esta carpeta → Install.
- **Compartir / instalar desde GitHub:** *Import power from GitHub* →
  `https://github.com/jmaritar/kiro-skills` → Install.

La activacion es dinamica: Kiro carga el Power cuando tu tarea coincide con sus
`keywords` (feature, epica, jira, gitbook, prd, casos de uso, comandos, ...).

Para actualizar: Powers panel → el power → *Check for updates* → *Install updates*.

## Configuracion previa (una vez por equipo)

1. **MCP** en tu `~/.kiro/settings/mcp.json`: servidores `atlassian` y `gitbook`
   (con OAuth o token). Los secretos NO viven en este Power.
2. **Rellenar placeholders** `<< ... >>` en el steering con tus valores reales:
   - `02-atlassian-defaults.md`: sitio, cloudId, project key, customfields.
   - `03-gitbook-context.md`: organizacion y organization ID.

## Seguridad

- Este Power NO contiene tokens ni credenciales. Van en `~/.kiro/settings/mcp.json`.
- No incluye fichas de features en curso ni rollback-logs (contexto interno).
- cloudId, project key y customfields vienen como placeholders para que cada equipo
  ponga los suyos.

## Relacion con Claude Skills

Este repo es el equivalente en Kiro de un repo `claude-skills`. El `SKILL.md` es casi
identico entre ambos; lo que cambia es el empaquetado: en Claude es `marketplace.json`,
en Kiro es `plugin.json` (Power). Ver `skills/comandos/references/empaquetado.md`.

## Licencia

MIT. Ver `LICENSE`.
