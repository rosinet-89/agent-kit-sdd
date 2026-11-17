# ADR 0024: La leyenda de estatus vive en el índice

- **Estado:** Aceptado
- **Fecha:** 2026-11-16
- **Origen:** `0012_EstatusDeSpecYCierre`

## Contexto
El índice no tenía estatus terminal. Dos agentes, con las mismas celdas, no coincidían en qué estaba pendiente. RF-2 responde pendiente, pendiente de desarrollo, grupo y cerrado desde la celda, sin abrir cada carpeta. La constitución, principio 3, pide que cada regla viva en una sola fuente. Comandos, skill, plantilla y `AGENTS.md` necesitaban citar esa fuente.

## Decisión
La definición es la sección `## Leyenda de estatus` en el README de specs (`template/.docs/002_specs/README.md`) y la definición bajo `## Estados` en el README de deuda (`template/.docs/003_deuda/README.md`). Comandos, skill, plantilla y `AGENTS.md` remiten y no copian la tabla.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Una pieza nueva al estilo de `topes-y-modos.md` | Obligaría al índice a remitir y dejaría de ser la fuente que el agente lee. RF-2 se responde desde el índice, sin abrir carpetas. |
| Copiar la tabla en cada comando | Choca con el principio 3: cada regla vive en una sola fuente. |

## Consecuencias
Cambiar una regla de estatus se hace en `## Leyenda de estatus`; una de deuda, bajo `## Estados`. Quien aplica pendiente, desfase o una transición cita esa sección y no la vuelve a redactar. La copia de las mismas secciones en los README del clon es la decisión 4 (ADR 0027). Este ADR no reescribe `03_plan.md`: el plan ya nombra 0024.
