# ADR 0029: Configuración del flujo por dev, no versionada, con sugerencias en un ejemplo

- **Estado:** Aceptado
- **Fecha:** 2026-11-21
- **Origen:** `0014_ConfiguracionGuiada`

## Contexto
El flujo tiene valores que hoy son reglas fijas: modelos por rol, roles que corren como subagente,
tope y modo de corrección de clarify, entornos de QA y presentación de las respuestas. Varios dependen
de la herramienta de agente y del gusto de cada dev, y en un mismo proyecto conviven devs con
herramientas distintas. Un valor guardado en un archivo del kit deja de ser KIT-VIEJO y no se
actualiza (ADR 007); uno guardado en `AGENTS.md` se carga en cada turno (criterio de la
`0015_ArranqueLigero`).

## Decisión
El kit solo sugiere: los pasos y sus valores sugeridos viajan en `.agents/configuracion.ejemplo.md`,
que cita las fuentes únicas existentes sin copiarlas. Lo que elige cada dev vive en
`.agents/local/configuracion.md` (más `entornos.md` y `auditor.md`), no se versiona y lo escribe el
asistente `/sdd-config`. `PREFERENCIAS.md` solo remite.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| `.agents/configuracion.md` versionado (configuración del equipo) | Los modelos dependen de la herramienta de cada dev; la persona pidió que nada se versione |
| Sección de `AGENTS.md` | Se carga en cada turno; va contra el criterio de la 0015 |
| Editar los archivos del kit (frontmatter, `topes-y-modos.md`) | Dejan de ser KIT-VIEJO y la actualización ya no los toca |
| Todo en `PREFERENCIAS.md` | Texto libre, difícil de editar por un asistente; se carga entero en cada sesión |

## Consecuencias
- Cada dev puede reconfigurar sin tocar el repo; `git status` no cambia por configurar.
- Las reglas compartidas (spec y avance) deben tolerar valores distintos por dev: el tope de clarify
  se cuenta por spec y rige el N de quien corre la pasada.
- Un dev nuevo no tiene configuración hasta correr `/sdd-config`; rigen los sugeridos y el chequeo de
  arranque lo avisa.
- Complementa el ADR 0028: `restricciones-de-rol.md` sigue siendo la fuente de los valores sugeridos
  de Claude Code; «la tabla del proyecto» pasa a la configuración local.
