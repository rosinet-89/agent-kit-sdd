# ADR 003: `AGENTS.md` es la fuente de verdad; `CLAUDE.md` solo lo importa

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; 0.4.0)
- **Origen:** Commit 861bb99

## Contexto
Cada herramienta lee su propio archivo de instrucciones (`CLAUDE.md`, `.cursor`, Copilot…). Mantener varias copias genera desvíos.

## Decisión
`AGENTS.md` concentra las instrucciones. `CLAUDE.md` contiene solo `@AGENTS.md`; Copilot se enlaza a `AGENTS.md`. Agentes, comandos y skills viven en `.agents/` y cada herramienta apunta a esa carpeta con enlaces (`.agents/sync-agents.md`).

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Un archivo de instrucciones por herramienta | Copias que se desfasan |

## Consecuencias
Hay que verificar los enlaces cada sesión (paso 7 de `AGENTS.md`). Los enlaces son por entorno y no se versionan.
