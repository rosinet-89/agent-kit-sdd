# 005 — Verificar el frontmatter de los agentes en Cursor, Gemini, Copilot y Kiro

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-10-25 |
| Origen | Alta manual (pregunta del usuario durante la spec `004_ModeloYPermisosAgentes`) |
| Módulo | Agentes del kit (frontmatter) y `sync-agents.md` |
| Prioridad | Media |
| Atendida por | `007_DeudasAbiertas` (cierre 2026-10-25) |

## Descripción
Los agentes del kit usan campos de frontmatter de Claude Code (`model`, `omitClaudeMd`, `tools`). `template/.agents/skills/sdd/restricciones-de-rol.md` los marca como "no verificado" en Cursor, Gemini y Copilot. Kiro no figura en la tabla de enlaces de `sync-agents.md`. No se sabe si esas herramientas ignoran las claves desconocidas, fallan, o las interpretan con otro sentido.

## Por qué quedó pendiente
La spec 004 se acotó a Claude Code, que es la herramienta con documentación leída y verificada (2026-10-25). Verificar las otras exige leer su documentación o probarlas.

## Impacto si no se atiende
- Un agente podría fallar o comportarse distinto en otra herramienta por una clave que no conoce.
- Sin `tools` ni `omitClaudeMd` efectivos, las restricciones "GARANTIZADA" valen solo en Claude Code; en el resto dependen de instrucciones de texto.
- Quien use Kiro no tiene enlace ni formato documentado.

## Alcance (archivos, tablas)
`template/.agents/skills/sdd/restricciones-de-rol.md` (registro de campos), `template/.agents/agents/*.md` (frontmatter), `.agents/sync-agents.md` y su réplica en `template/` (tabla de enlaces, fila de Kiro), `CHANGELOG.md`.

## Cómo se atendería (propuesta)
1. Leer la documentación de agentes de Cursor, Gemini CLI, Copilot y Kiro; registrar fecha y fuente por herramienta.
2. Por cada campo, anotar si es soportado, ignorado o provoca error.
3. Actualizar el registro de campos y la columna "Garantizada" según el resultado.
4. Si Kiro usa otro formato o carpeta, agregar su fila a la tabla de enlaces o documentar que no aplica.
5. Hacerlo como spec propia por el flujo `sdd`.

## Cómo se atendió (al resolver: qué se hizo y dónde)
Registro del 2026-10-25 en `template/.agents/skills/sdd/restricciones-de-rol.md`. Cursor, Gemini CLI, GitHub Copilot y Kiro tienen URL. `model` está soportado en las cuatro. `omitClaudeMd` queda sin veredicto en las cuatro: la fuente no nombra la clave. `tools` queda sin veredicto en Cursor y soportado en las otras tres. La columna `Garantizada` no extiende la garantía donde el campo no la sostiene. El frontmatter de los cuatro agentes no cambió. Kiro quedó como otro formato en `template/.agents/sync-agents.md` (`.kiro/agents/`), sin fila de enlace.

## Historial
- 2026-10-25 — Alta, por decisión del usuario.
- 2026-10-25 — Pasa a En atención: la spec `007_DeudasAbiertas` la incluye junto con las deudas 006, 007 y 008.
- 2026-10-25 — Resuelta en el cierre de `007_DeudasAbiertas`.
