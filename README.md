<p align="center">
  <img src="assets/logo.svg" alt="kiro-skills" width="120" height="120" />
</p>

<h1 align="center">kiro-skills</h1>

<p align="center">Kit de trabajo de PDC para <a href="https://kiro.dev">Kiro</a> · multi-stack · en español · by PDC</p>

---

Power (plugin) para Kiro que empaqueta la forma de trabajar de PDC: **skills**,
**steering** de contexto y **plantillas** reutilizables, en español y pensado para
**varios tipos de proyecto** (Angular, Flutter y los que vengan).

No es "solo el flujo PRD→JIRA": ese flujo es la **primera capacidad**. El kit esta
preparado para ir sumando skills de otros stacks sin reorganizar nada.

Sigue el estandar abierto [Agent Plugins](https://agent-plugins.org/) (`plugin.json`).

## Capacidades actuales

### Skills (se activan con `/`)

| Comando | Categoria | Que hace |
|---|---|---|
| `/comandos` | General | Indice directo, en español, de todo lo disponible. Empieza aqui. |
| `/feature-workspace-init` | Feature / JIRA | Inicializa el contexto de una feature desde una epica de JIRA (lee epica, ubica PRD/DERCAS en GitBook, lista casos de uso, valida issues y ofrece crear los faltantes, con confirmacion y rollback). |

### Steering (se invoca con `#`)

| Comando | Categoria | Aporta |
|---|---|---|
| `#workflow-prd-to-jira` | Feature / JIRA | El pipeline PRD/DERCAS → Spec → JIRA. |
| `#estimations` | Transversal | Story points Fibonacci + equivalencia en tiempo. |
| `#atlassian-defaults` | Feature / JIRA | Defaults de JIRA (sitio, cloudId, project key, tipos, customfields). |
| `#gitbook-context` | Feature / JIRA | Organizacion y spaces de PRD/DERCAS en GitBook. |
| `#spec-template` | Transversal | Molde de un Spec (requirements/design/tasks). |
| `#features-index` | Feature / JIRA | Indice central de features trabajadas. |

### Plantillas (se copian al proyecto)

- `dev.kiro/steering/templates/specs/SPEC-GUIDE.md` — como instanciar un Spec.
- `dev.kiro/steering/templates/hooks/*.json` — hooks de JIRA y verificacion Angular.

## Roadmap (proximas capacidades)

- `flutter-*` — skills para proyectos Flutter (scaffolding, patrones, revision).
- `angular-*` — utilidades especificas de Angular mas alla del flujo actual.
- `worklog` / `daily` — bitacora de trabajo y generacion del Daily.

## Estructura

```
kiro-skills/
├── plugin.json                 # manifiesto Agent Plugins
├── README.md · LICENSE · CONTRIBUTING.md · .gitignore
├── assets/
│   └── logo.svg                # logo del kit (ver nota sobre logos abajo)
├── skills/                     # una carpeta por skill (nombrada por dominio)
│   ├── comandos/
│   └── feature-workspace-init/
└── dev.kiro/
    └── steering/               # steering (#...) + plantillas
```

Convencion (Opcion 1, plano por dominio): cada skill es `skills/<nombre>/SKILL.md`,
nombrada por su dominio (`feature-...`, `flutter-...`, `angular-...`). El steering
transversal (estimaciones, spec) se comparte; el especifico de un stack va en su
propio archivo. Ver `CONTRIBUTING.md` para agregar una skill nueva.

## Instalacion (Kiro IDE)

Panel de **Powers** → **Add Custom Power**:

- **Probar en local:** *Import power from a folder* → selecciona esta carpeta → Install.
- **Instalar / compartir desde GitHub:** *Import power from GitHub* →
  `https://github.com/jmaritar/kiro-skills` → Install.

La activacion es dinamica por `keywords`: Kiro carga el kit cuando tu tarea coincide.
Para actualizar: Powers panel → el power → *Check for updates* → *Install updates*.

## Configuracion previa (una vez por dispositivo)

1. **MCP** en tu `~/.kiro/settings/mcp.json`: servidores `atlassian` y `gitbook`
   (OAuth o token). Los secretos NO viven en este Power.
2. **Rellenar placeholders** `<< ... >>` del steering con tus valores reales
   (`02-atlassian-defaults.md`, `03-gitbook-context.md`).

## Nota sobre el logo y el "by"

- El texto **"by PDC"** lo controla el campo `author.name` de `plugin.json`.
- El **logo** de la lista de Powers en el IDE proviene del registro curado de Kiro
  (branding de partners); el schema de `plugin.json` no admite un campo de icono para
  powers personalizados. Por eso incluimos `assets/logo.svg` para el README/GitHub y
  para uso futuro, aunque el IDE aun no lo pinte para powers importados.

## Relacion con Claude Skills

Equivalente en Kiro de un repo `claude-skills`: mismo `SKILL.md`, distinto empaquetado
(`plugin.json` en vez de `marketplace.json`). Detalle en
`skills/comandos/references/empaquetado.md`.

## Licencia

MIT. Ver `LICENSE`.
