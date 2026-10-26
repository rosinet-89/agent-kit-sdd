# 008 — Limitaciones de la numeración que dejó la 005

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-10-25 |
| Origen | Decisión de requerimiento. Diferido en `005_ConvivenciaNumeracion` (decisión 28 del avance; alta pedida en la decisión 30) |
| Módulo | Numeración de specs, deudas y ADR (E12 y aviso) |
| Prioridad | Baja |
| Atendida por | `007_DeudasAbiertas` (cierre 2026-10-25) |

## Descripción
La revalidación de la `005` dejó cuatro bordes que no bloquean el cierre. No cambian la regla del ADR 014 ni la fuente del ADR 015.

1. Un prefijo de 5 o más dígitos con ceros de relleno (`00017_`) se trata como inválido por la regla « sin ceros de relleno ».
2. En el clon, una carpeta con mezcla válida (por ejemplo `001_` y `1000_` por desbordamiento) no emite `AVISO`: el `AVISO` es solo de proyecto, y la fila E12 no lo dice con esas palabras.
3. El ancho de carpeta cuando hay un ítem inválido no está definido. El script de la prueba lo ignora.
4. El índice de `template/.docs/` no declara el ancho del clon. Solo lo declara el README local del kit. En el clon, E12 compara contra ese índice.

## Por qué quedó pendiente
La decisión 25 de la `005` corrigió en el momento las ambigüedades 1 a 4 y dejó la 5, más los tres bordes de la decisión 28, como limitación conocida. No bloquean y no entraron en F8.

## Impacto si no se atiende
E12 puede marcar inválido un prefijo con ceros de relleno, callar una mezcla válida en el clon y no fijar el ancho si aparece un ítem inválido. Quien copie solo `template/.docs/` no ve declarado el ancho del clon.

## Alcance (archivos, tablas)
`template/.agents/validar-estructura.md` (fila E12), `template/.docs/002_specs/README.md` › « Numeración única », y el README de ADR y el de deuda del clon. No se renombra nada y no se cambia el grupo C.

## Cómo se atendería (propuesta)
1. Abrirlo como spec propia por el flujo `sdd`.
2. Decidir, con alternativa descartada, qué hace E12 con ceros de relleno, con la mezcla válida en el clon y con el ancho si hay un ítem inválido.
3. Declarar el ancho del clon en el índice que vive en `template/`, o decir en E12 que ese ancho solo lo declara el README del clon.
4. No renumerar ítems existentes.

## Cómo se atendió (al resolver: qué se hizo y dónde)
`template/.docs/002_specs/README.md` › « Numeración única » dice que el prefijo vale su entero, que el relleno no es fallo, que el mismo entero es `FALLA` y no `AVISO`, que `abc_Mal` no cuenta, y que un ítem nuevo del clon se escribe con 4 dígitos sin renumerar los ya creados. La fila E12 dice ese resultado y remite. `AGENTS.md` y los índices de planes y de ADR de este clon remiten y no copian la regla. No se editaron los ADR 014 y 015 ni el grupo C.

## Historial
- 2026-10-25 — Alta, al atender la decisión 30 de `005_ConvivenciaNumeracion`, con el visto bueno del usuario.
- 2026-10-25 — Pasa a En atención: la spec `007_DeudasAbiertas` la incluye junto con las deudas 005, 006 y 007.
- 2026-10-25 — Resuelta en el cierre de `007_DeudasAbiertas`.
