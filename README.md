# Vikingo Skills — repositorio de Powers de PDC

Coleccion de **Kiro Powers** de PDC, en español. Cada power aporta skills, steering y
plantillas para tareas especificas. Modelado segun el repositorio oficial
[kirodotdev/powers](https://github.com/kirodotdev/powers): un repo = varios powers, cada
uno en su propia carpeta.

Documentacion de Powers: https://kiro.dev/docs/powers/

## Powers disponibles

### vikingo-workflow
**Vikingo Workflow** - Flujo de trabajo de PDC en español: PRD/DERCAS (GitBook) -> Spec de
Kiro -> issues en JIRA. Incluye la skill de inicializacion de features desde una epica
(`/feature-workspace-init`) y un indice de comandos (`/comandos`). Trae steering con defaults
de Atlassian, contexto de GitBook, estimaciones (story points Fibonacci) y plantillas de Spec.

**Skills:** comandos, feature-workspace-init
**MCP Servers:** ninguno incluido (usa tus servidores `atlassian` y `gitbook` de `~/.kiro/settings/mcp.json`)

---

> Proximos powers (roadmap): `vikingo-flutter`, `vikingo-angular`, `vikingo-worklog`.

## Estructura del repositorio

```
powers/                          # repo = marketplace de powers
├── README.md                    # este indice de powers
├── CONTRIBUTING.md              # como agregar un power / una skill
├── LICENSE
└── vikingo-workflow/            # un power = una carpeta
    ├── POWER.md                 # manifiesto (displayName, author, keywords, steering)
    ├── assets/                  # logo del power (casco Vikingo)
    ├── skills/
    │   ├── comandos/
    │   └── feature-workspace-init/
    └── steering/                # steering (#...) + plantillas
```

Cada power usa el formato **`POWER.md`** (soporta `displayName` y `author`, que es lo que el
IDE muestra como nombre y "by"). Ver `CONTRIBUTING.md` para agregar un power nuevo.

## Instalacion (Kiro IDE)

Panel de **Powers** → **Add Custom Power** → **Import power from GitHub**, y apunta a la
**carpeta del power** dentro del repo (no a la raiz). Por ejemplo:

```
https://github.com/jmaritar/powers/tree/main/vikingo-workflow
```

O bien **Import power from a folder** y selecciona `vikingo-workflow/` para probar en local.

La activacion es dinamica por `keywords`: Kiro carga el power cuando tu tarea coincide.

## Nota sobre el logo en el IDE

El casco Vikingo (`vikingo-workflow/assets/logo.svg`) se ve en GitHub, pero **el IDE aun no
muestra logo para powers personalizados** — es una funcionalidad pendiente de Kiro
(issue [kirodotdev/powers#103](https://github.com/kirodotdev/powers/issues/103)). El nombre
"Vikingo Workflow" y el "by Vikingo IA" sí se muestran (vienen de `displayName` y `author`
en `POWER.md`). Para branding con logo, el power debe entrar al registro oficial:
https://kiro.dev/powers/submit

## Licencia

MIT. Ver `LICENSE`.
