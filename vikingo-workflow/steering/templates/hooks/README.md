# Plantillas de Hooks

Los hooks se ejecutan sobre el WORKSPACE activo y Kiro los lee desde
`<proyecto>/.kiro/hooks/`. No existe un hook "global" que corra en todos los
proyectos, por eso estos archivos son PLANTILLAS que se copian al proyecto solo
cuando quieras activarlos.

## Como activar un hook en un proyecto

1. Copia el archivo `.json` deseado a `<proyecto>/.kiro/hooks/`.
2. Para que NO llegue al repositorio, agrega `.kiro/hooks/` (o el archivo puntual)
   al `.gitignore` del proyecto.
3. Ajusta el `matcher` o el `prompt` si hace falta.
4. Kiro tomara el hook en la siguiente sesion / al recargar.

> Tambien puedes crear hooks desde la paleta de comandos: "Open Kiro Hook UI".

## Plantillas incluidas

| Archivo                               | Trigger        | Que hace                                                        |
|---------------------------------------|----------------|-----------------------------------------------------------------|
| `jira-start-on-task-begin.json`       | PreTaskExec    | Recuerda pasar el issue de JIRA a "In Progress" al iniciar tarea |
| `jira-sync-on-task-complete.json`     | PostTaskExec   | Recuerda actualizar/transicionar el issue de JIRA al completar   |
| `angular-build-check-on-save.json`    | PostFileSave   | Revisa #Problems tras guardar archivos .ts/.html                 |

## Notas

- Los hooks de JIRA son de tipo `agent` (inyectan una instruccion), no ejecutan
  comandos ni escriben en JIRA por si mismos; siguen respetando el "visto bueno".
- Evita hooks que lancen procesos de larga duracion (ng serve, watchers).
