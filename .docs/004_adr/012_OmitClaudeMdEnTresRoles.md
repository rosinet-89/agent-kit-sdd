# ADR 012: omitClaudeMd en planner, reviewer y coordinator

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `004_ModeloYPermisosAgentes`

## Contexto
Sin `omitClaudeMd`, un subagente carga `CLAUDE.md` y los `AGENTS.md` de proyecto. Planner, reviewer y coordinator solo necesitan las secciones que su instrucción nombra. El implementer puede recibir el archivo inyectado. No hay una revisión en la que el paso F1.4 de `001_ActualizarKit` ya esté y F1.5 todavía no, así que la comparación de tres pasos para el implementer no se puede correr. El campo quita todo `CLAUDE.md`, no una sección. Si un proyecto no importa `AGENTS.md`, el ahorro es cero.

## Decisión
`omitClaudeMd: true` en planner, reviewer y coordinator. El implementer no declara la clave. Su neto queda anotado en el avance de `004_ModeloYPermisosAgentes` y el campo sigue ausente.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Ningún rol | Planner, reviewer y coordinator seguirían cargando el archivo entero |
| Los cuatro roles | La condición del implementer no se puede medir: no existe el árbol previo a F1.5 |
| Quitarlo si en otro proyecto el ahorro es cero | El límite ya está escrito en la pieza. El campo se mantiene en los tres roles de este kit |

## Consecuencias
No reemplaza al ADR 003. `AGENTS.md` sigue siendo la fuente; estos tres roles nombran las secciones y no las copian. En un proyecto que no importa `AGENTS.md`, el ahorro de este campo es cero.
