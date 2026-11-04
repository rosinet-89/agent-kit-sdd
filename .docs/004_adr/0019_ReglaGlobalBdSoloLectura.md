# ADR 0019: Regla global de la base de datos solo lectura en AGENTS.md › Guardrails

- **Estado:** Aceptado
- **Fecha:** 2026-11-01
- **Origen:** `0008_GuardrailsBdSoloLectura`

## Contexto
La prohibición de ejecutar SQL que escriba en una base de datos estaba repartida por rol y por skill. Quien trabajaba fuera del flujo `sdd` no la veía, y cada pieza la repetía con matices distintos. Hacía falta una regla visible para todos los agentes, en todo proyecto que use el kit, con sitio para sumar otros guardrails.

## Decisión
La regla vive en una sección global de `AGENTS.md` llamada «Guardrails (todos los agentes, siempre)», con numeración abierta. Todos los roles la leen siempre y las demás piezas la citan con una sola forma de remisión (`AGENTS.md` › Guardrails, regla 1), sin resumirla. Si no se sabe si un comando escribe en una base de datos, no se ejecuta y se entrega a la persona.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| (a) Sección global en `AGENTS.md` | Elegida: una fuente única (ADR 009), visible fuera del flujo `sdd` |
| (b) Repartirla por rol y skill (estado actual) | Los resúmenes divergen con el tiempo y quien trabaja fuera del flujo `sdd` no la ve |
| (c) Un archivo aparte de guardrails | Obliga a cada agente a enlazar otro archivo y a los proyectos existentes a incorporar dos piezas |
| (d) Solo en la constitución de cada proyecto | Depende de que cada proyecto la escriba y la mantenga; el kit no puede garantizar que exista |
| (e) Garantía técnica por permisos | Fuera de alcance: el kit es solo Markdown y no gestiona permisos de ejecución (ADR 004, ADR 011) |

## Consecuencias
Hay una fuente por árbol. Los agentes deben nombrar «Guardrails» en su lectura. Los proyectos existentes incorporan la sección a mano. La regla sigue siendo una instrucción, no una garantía técnica.
