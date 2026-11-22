# ADR 0033: «Antecedentes» y «Deuda técnica» se leen bajo demanda desde la skill `sdd`

- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `0017_PausaOrigenYContexto`

## Contexto
`AGENTS.md` se carga en cada turno. Medido el 2026-11-22: «Antecedentes» pesa 893 bytes y «Deuda
técnica», 697, y solo se usan en análisis o desarrollo nuevo (issue #7). El ADR 0032 las dejó en
`AGENTS.md` porque los roles y comandos las citan por nombre. Restricciones: los roles con
`omitClaudeMd` leen secciones por nombre; un proyecto instalado nunca recibe un `AGENTS.md` nuevo
(Grupo B) ni los README de `.docs/` que ya tiene (Grupo C, ADR 007).

## Decisión
El cuerpo de las dos secciones vive en `.agents/skills/sdd/antecedentes-y-deuda.md`. `AGENTS.md`
conserva los dos encabezados, cada uno con la condición de cuándo se lee y la remisión. Ajusta la lista
de secciones del ADR 0032; su criterio sigue vigente.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Los README de `.docs/002_specs/` y `.docs/003_deuda/` | Son Grupo C: un proyecto que actualiza no los recibe y la remisión quedaría rota |
| Una sección de `skills/sdd/SKILL.md` | Obliga a cargar la skill entera para leer 1,5 KB |
| Dejarlas en `AGENTS.md` | No baja el peso de cada turno (RNF-1 de la 0017) |

## Consecuencias
`AGENTS.md` baja unos 1 400 bytes. Las remisiones por nombre siguen resolviendo porque los encabezados
se quedan. El archivo nuevo es Grupo A y llega con la actualización. Un proyecto con `AGENTS.md` viejo
conserva su texto completo y sigue funcionando. La regla tiene una sola fuente: la skill.
