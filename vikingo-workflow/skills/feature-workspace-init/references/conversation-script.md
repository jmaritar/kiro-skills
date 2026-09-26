# Guion de conversacion (tono chatbot)

Mensajes de referencia por paso para `feature-workspace-init`. Adapta el texto, pero
manten el caracter secuencial y de una pregunta por turno. Siempre en español.

## Inicio
> Vamos a inicializar el contexto de una feature. Lo hare paso a paso.
> **Paso 1 de 7 — Epica.** Cual es la epica que trabajaremos? Puedes pasarme la URL de
> JIRA o la clave (ej. TTDEV-26218).

## Paso 2 — resumen de la epica
> **Paso 2 de 7 — Lei la epica <CLAVE>.**
> - Titulo: ...
> - Estado / periodo / sprint: ...
> - Team / epica padre: ...
> - PRD: <link>  ·  DERCAS: <link>
> (Si PRD y DERCAS apuntan al mismo space, lo indico.)
> Continuo a listar los casos de uso? (si/no)

## Paso 3 — lista de CU
> **Paso 3 de 7 — Casos de uso encontrados en PRD y DERCAS:**
> 1. CU-1 — <titulo>
> 2. CU-2 — <titulo>
> ...
> Estos son los CU que detecte en el PRD/DERCAS.

## Paso 4 — seleccion
> **Paso 4 de 7 — Cuales deseas trabajar?**
> Responde con numeros ("1,3,5"), un rango ("1-4"), "todos", o los titulos.
> (Recuerda: no tienen que ser todos.)
> ... luego:
> Seleccionaste: CU-1, CU-3, CU-5. Confirmas? (si/no)

## Paso 5 — validacion contra la epica
> **Paso 5 de 7 — Valide contra los issues ya creados en la epica.**
> Ya creados (N): ...
> Faltantes (M): ...
> Dudosos (K) — te pregunto uno a uno: ...

## Paso 6 — crear faltantes
> **Paso 6 de 7 — Hay M CU sin issue.** Deseas crearlos ahora en la epica? (si/no)
> ... si "si", muestra el preview:
> Voy a crear estos issues (tipo Historia, colgados de <CLAVE>):
> - "CU-4 Configurar oferta tipo Descuento directo"
> - ...
> Confirmas la creacion? (si/no)
> ... tras crear: "Cree TTDEV-XXXX para CU-4. Registrado para posible reversion."

## Paso 7 — persistir
> **Paso 7 de 7 — Guarde el contexto de la feature** en features/<CLAVE>.md
> (org GitBook, space, CU seleccionados y sus issues, trazabilidad).
> Quieres que preparemos el Spec en el proyecto como siguiente paso? (opcional)

## Reversion (en cualquier momento)
> Detecte un problema / me pediste revertir. Esto es lo que cree en esta sesion y puedo
> deshacer: TTDEV-XXXX, TTDEV-YYYY. Como prefieres? (eliminar / cancelar-y-comentar / no
> revertir)

## Cierre por interrupcion
> Ok, detengo el flujo. No deje nada creado sin registrar. Lo creado hasta ahora es: ...
> Puedes retomar activando de nuevo /feature-workspace-init o pedirme revertir.
