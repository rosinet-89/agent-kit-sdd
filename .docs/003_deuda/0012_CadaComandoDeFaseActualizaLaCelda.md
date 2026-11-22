# 0012 — Ningún comando de fase actualiza la celda del índice en `Planificado`, `En curso` y `En pruebas`

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-11-16 |
| Origen | Recorte de alcance en `0012_EstatusDeSpecYCierre` (queda fuera de la spec; alta propuesta por la sesión y aprobada por la persona en el cierre) |
| Módulo | Comandos de fase (`/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/sdd-validate`, `/sdd-flujo`, `/sdd-change`), índice de planes, E22 |
| Prioridad | Media |
| Atendida por | `0017_PausaOrigenYContexto` (cierre 2026-11-22) |

## Descripción
La leyenda de estatus del índice de planes define ocho estatus, pero hoy solo la apertura (`/sdd-spec`, que escribe `Análisis`) y el cierre (`/sdd-cierre`, que escribe `Implementado`, `Cerrado` o `Descartado`) escriben la celda de la fila. La persona escribe `Diferido`. Nadie escribe `Planificado`, `En curso` ni `En pruebas`: la celda de una carpeta en esas etapas se queda en la palabra anterior hasta el cierre.

## Por qué quedó pendiente
La spec decidió, con la persona, que la respuesta a «qué está pendiente» sale de la celda y que la carpeta manda solo al consultar una carpeta concreta. Hacer que cada comando de fase escriba la celda toca cuatro o cinco comandos más y ampliaba el alcance, así que se propuso como deuda en vez de entrar a la 0012.

## Impacto si no se atiende
La celda de una carpeta en curso puede estar desfasada respecto de su etapa real. El conteo de pendiente y de pendiente de desarrollo no cambia (solo salen del pendiente un `Cerrado` o un `Descartado` bien formados), pero la etapa y el grupo que se resumen sí pueden ser viejos. E22 avisa del desfase en el chequeo de estructura y `/sdd-status` lo avisa al consultar la carpeta; ninguno lo corrige.

## Alcance (archivos, tablas)
`template/.agents/commands/sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md` y `sdd-change.md`; la línea «Quién escribe» de la leyenda en `.docs/002_specs/README.md` y en su plantilla; los puntos 5 y 6 de «Antecedentes» de `AGENTS.md`; E22; las réplicas en `.agents/`; versión, changelog y manifiesto.

## Cómo se atendería (propuesta)
1. Decidir qué comando o momento escribe cada estatus (por ejemplo, `Planificado` al aprobar el plan, `En curso` al empezar el primer paso, `En pruebas` al terminar el último paso).
2. Alternativas: que lo escriba cada comando; que lo escriba la sesión principal al registrar el avance de cada fase; o mantener la lectura bajo demanda y dejar el aviso de E22.
3. Si se elige escribirlo, mantener la regla de una sola fuente: la leyenda dice quién escribe y los comandos remiten.

## Cómo se atendió (al resolver: qué se hizo y dónde)
RF-28 de `0017_PausaOrigenYContexto` (ACLARAR-2): la sesión principal escribe `Planificado`, `En curso` y `En pruebas` al registrar en `05_avance.md` el cierre de `plan`, del primer paso y del último paso. La regla vive solo en la línea «Quién escribe» de la leyenda de los dos `002_specs/README.md`; `sdd-plan.md` y `sdd-implement.md` remiten. E22 sigue avisando de un desfase.

## Historial
- 2026-11-16 — Alta, en el cierre de `0012_EstatusDeSpecYCierre`, con el visto bueno de la persona.
- 2026-11-22 — Pasa a En atención y a Resuelta en el cierre de `0017_PausaOrigenYContexto`, con el visto bueno de la persona.
