# 0014 — El «Qué lee» del chequeo de arranque no nombra la definición de adaptador por archivo

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-11-21 |
| Origen | Hallazgo al validar `0015_ArranqueLigero` (opcional (a) del reviewer, decisión 8 del avance; alta aprobada por la persona en el cierre) |
| Módulo | `sync-agents.md` › «Chequeo de arranque» y «Estados de enlace» |
| Prioridad | Baja |
| Atendida por | `0017_PausaOrigenYContexto` (cierre 2026-11-22) |

## Descripción
Desde 0.12.0, el chequeo de arranque lee solo lo que indica su renglón «Qué lee»: «Estados de
enlace», «Estado «visto»» y las filas de las herramientas declaradas. En una carpeta de agentes por
archivo (Cursor, Gemini, GitHub y OpenCode, o Claude Code con modelos propios), «Estados de enlace» dice
que `OK` es un enlace o «un adaptador que cumple la definición de «Carpeta de agentes por archivo»»,
y esa subsección no está en el «Qué lee». Antes de 0.12.0 el arranque abría el archivo entero y la
tenía a mano.

## Por qué quedó pendiente
No bloquea ningún RF: los cuatro escenarios de RF-8 (Claude Code y el clon) dan el mismo resultado. El
caso solo aparece con una herramienta por archivo declarada y un adaptador presente pero mal formado.

## Impacto si no se atiende
Riesgo bajo: el arranque podría dar `OK` a un adaptador viejo o mal formado, o leer la subsección por
su cuenta y perder parte del ahorro. `/sdd-estructura` (E3 y E23) sigue detectando el adaptador viejo.

## Alcance (archivos, tablas)
`template/.agents/sync-agents.md` (renglón «Qué lee» de «Chequeo de arranque» o la frase de `OK` de
«Estados de enlace») y su réplica en `.agents/`; versión, changelog y manifiesto si cambia `template/`.

## Cómo se atendería (propuesta)
Decidir qué comprueba el arranque en una carpeta por archivo: solo que cada rol tenga entrada (y dejar
la validez del adaptador a `/sdd-estructura`), o nombrar en el «Qué lee» la definición, solo cuando
haya una herramienta por archivo declarada.

## Cómo se atendió (al resolver: qué se hizo y dónde)
RF-16 de `0017_PausaOrigenYContexto` (opción a): `template/.agents/sync-agents.md` › «Chequeo de arranque» en una carpeta por archivo solo comprueba que cada rol tenga su entrada; la validez del adaptador queda en `/sdd-enlaces` y `/sdd-estructura` (E3, E23). Su «Qué lee» declara esa lectura reducida.

## Historial
- 2026-11-21 — Alta, en el cierre de `0015_ArranqueLigero`, con el visto bueno de la persona.
- 2026-11-22 — Pasa a En atención y a Resuelta en el cierre de `0017_PausaOrigenYContexto`, con el visto bueno de la persona.
