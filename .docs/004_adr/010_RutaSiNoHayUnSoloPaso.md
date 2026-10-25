# ADR 010: La ruta alterna vive en la skill sdd

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `006_RutaSiNoDelega`

## Contexto
Si quien atiende una fase no puede lanzar en un solo paso al rol que esa fase nombra, el flujo se detenía. El caso del 2026-10-25 fue el coordinator como subagente: no podía abrir al planner y no redacta los documentos de la fase. El principio 3 de la constitución pide una sola fuente. El ADR 009 ya fijó que el procedimiento de cada fase vive en su comando. Esta regla cruza todas las fases que delegan, así que copiarla en cada comando la desfasaría.

## Decisión
La regla de un solo paso y de la otra ruta vive en la skill `sdd`, sección «Si no hay un solo paso». La constancia y la fila de tiempos viven en el contrato `avance-y-tiempos.md`. Cada comando que delega enlaza esa sección y no la copia. Si el usuario pide que la fase se haga en el chat, esa entrada se hace en la sesión.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Copiar la regla en cada comando | Contradice el ADR 009: habría nueve copias y se desfasarían |
| Dejarla solo en el coordinator | El chat no la ve cuando el coordinator no llega a delegar |
| Reemplazar el ADR 009 | El procedimiento de cada fase sigue en su comando. Esta decisión solo ubica la regla transversal |

## Consecuencias
Cambiar la ruta es editar la sección de la skill y, si cambia la constancia, el contrato. Los comandos no se reescriben con el procedimiento. El coordinator, si no puede abrir al rol, suelta la fase en una línea. No hay deuda nueva. El manifiesto de `0.5.0` lo regenera quien cierre último entre las specs que aún lo comparten.
