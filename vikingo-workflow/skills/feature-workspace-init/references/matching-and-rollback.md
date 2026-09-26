# Matching CU <-> issues y politica de reversion

Detalle que la skill `feature-workspace-init` debe seguir en los Pasos 5, 6 y en Rollback.

## 1. Recuperar los issues de la epica

Usa `searchJiraIssuesUsingJql` con el cloudId del sitio (ver `#atlassian-defaults`):

- JQL primario: `parent = <CLAVE-EPICA> ORDER BY created ASC`
- JQL alternativo (por si usan el campo de enlace de epic):
  `"Epic Link" = <CLAVE-EPICA> ORDER BY created ASC`
  (customfield_10014 = "Enlace de epic").
- `maxResults` = 10 y **pagina** con `nextPageToken` hasta `isLast = true`.
- Pide solo campos utiles: `["summary","status","issuetype","parent"]` o view "compact".

Guarda por cada issue: clave, summary, status, issuetype.

## 2. Normalizacion de nombres

Antes de comparar, normaliza tanto el titulo del CU como el summary del issue:

1. Pasar a minusculas.
2. Quitar acentos/diacriticos.
3. Quitar el prefijo/codigo de CU: `cu-1`, `cu 1`, `cu1`, `caso de uso 1`, `uc-01`, etc.
4. Quitar signos de puntuacion y colapsar espacios.
5. Quitar palabras vacias irrelevantes (de, del, la, el, los, las, un, una, y, o, para,
   desde, con, tipo).

Ejemplo: `"CU-4 Configurar oferta tipo Descuento directo"` -> `configurar oferta descuento directo`.

## 3. Puntaje de similitud

Calcula una similitud entre el CU normalizado y cada issue normalizado combinando:

- **Coincidencia de tokens** (Jaccard sobre el conjunto de palabras): peso principal.
- **Subcadena**: si el nombre del CU esta contenido en el summary del issue (o viceversa),
  sube el puntaje.
- **Numero de CU**: si el summary del issue contiene el mismo numero de CU (ej. "CU-4" o
  "caso 4"), sube el puntaje.

Clasifica con estos umbrales (guia, no exactos):

- **Alta confianza** (>= ~0.75, o subcadena exacta, o mismo numero de CU + tokens fuertes):
  se considera **YA CREADO**. Reporta CU -> issue.
- **Media/baja confianza** (~0.4 a 0.75): **DUDOSO**. Preguntar al usuario si corresponde.
- **< ~0.4**: **FALTANTE** (no hay issue asociado).

Un issue solo puede emparejarse con UN CU (el de mayor puntaje). Si dos CU compiten por el
mismo issue, gana el de mayor puntaje; el otro pasa a evaluarse contra el resto.

## 4. Presentacion del resultado (Paso 5)

Muestra tres bloques claros:

```
Ya creados (N):
  CU-1  Vista principal ...        -> TTDEV-XXXX  (alta confianza)
Faltantes (M):
  CU-4  Configurar oferta ...      -> (sin issue)
Dudosos (K) — confirmar:
  CU-6  Configurar combinacion ... ~ TTDEV-YYYY ? (media confianza)
```

Para cada dudoso, pregunta: "El CU-6 corresponde al issue TTDEV-YYYY 'resumen'? (si/no)".
- si -> mover a "Ya creados".
- no -> mover a "Faltantes".

## 5. Creacion de faltantes (Paso 6)

Solo tras confirmacion explicita. Por cada CU faltante:

- `createJiraIssue` con:
  - `projectKey`: del steering (TTDEV).
  - `issueType`: "Historia" (default del proyecto).
  - `summary`: el titulo del CU tal cual (incluyendo el codigo, ej. "CU-4 Configurar
    oferta tipo Descuento directo"). Ajustar si el usuario prefiere otro formato.
  - `parent`: la clave de la epica (para colgarlo de la epica).
  - `additional_fields`: heredar labels de la epica (ej. Integracion_Avon) y Team
    (customfield_10001) si aplica.
- Tras cada creacion exitosa, ANOTA la clave devuelta en el rollback-log inmediatamente.
- Si una creacion falla, detente, informa el error y ofrece revertir lo ya creado.

No asignes Story Points aqui (eso pertenece al Spec/estimacion posterior), salvo que el
usuario lo pida.

## 6. Rollback (reversion)

Fuente de verdad: el rollback-log de la sesion (solo contiene lo creado por esta sesion).

Al revertir:
1. Lee el rollback-log y lista lo que se va a deshacer. Pide confirmacion.
2. Por cada issue creado en esta sesion, en orden inverso a su creacion:
   - Preferencia: **eliminar** el issue recien creado con la operacion destructiva de
     Atlassian (deleteJiraIssue via executeDestructive) SOLO si el usuario confirma el
     borrado y el issue no tiene trabajo/hijos.
   - Alternativa mas segura: transicionar el issue a un estado de cancelado/descartado y
     comentar "Revertido por feature-workspace-init". Ofrecer ambas opciones al usuario.
3. NUNCA tocar issues que no esten en el rollback-log (los que ya existian antes).
4. Marca en el rollback-log cada item como revertido, con timestamp.
5. Al terminar, confirma el estado final: que se elimino/cancelo y que se conservo.

Si el rollback-log no existe (no se creo nada), informa que no hay nada que revertir.
