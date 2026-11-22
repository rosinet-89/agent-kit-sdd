# 0009 — Garantía técnica de que la base de datos solo se lee

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-11-03 |
| Origen | Hallazgo al cerrar `0008_GuardrailsBdSoloLectura` (alta pedida por el usuario en el cierre) |
| Módulo | Guardrails (`AGENTS.md`), `restricciones-de-rol.md` |
| Prioridad | Media |
| Atendida por | `0017_PausaOrigenYContexto` (cierre 2026-11-22) |

## Descripción
La regla 1 de «Guardrails» («la base de datos solo se lee») es una instrucción a los agentes, no una barrera técnica. En `restricciones-de-rol.md` figura como NO garantizada. La recomendación de un usuario de BD de solo lectura está escrita, pero el kit no puede imponerla.

## Por qué quedó pendiente
El kit es solo Markdown, sin hooks ni scripts (ADR 004), y el modelo de permisos por rol está acotado (ADR 011). Imponer el permiso exige configuración del entorno de cada proyecto, que el kit no versiona.

## Impacto si no se atiende
Un agente que ignore o malinterprete la regla puede ejecutar una escritura contra una BD a la que tenga credenciales. La protección depende de la obediencia y de que el proyecto use un usuario de solo lectura.

## Alcance (archivos, tablas)
`template/AGENTS.md` › Guardrails (regla 1), `template/.agents/skills/sdd/restricciones-de-rol.md` (fila 9 y sección de la recomendación), `template/.agents/commands/sdd-init.md` (donde `/sdd-init` pregunta cómo se consultan los datos).

## Cómo se atendería (propuesta)
1. Que `/sdd-init` pregunte por el usuario de BD de solo lectura y deje constancia en `AGENTS.md` › Comandos.
2. Valorar fragmentos `deny` de permisos por herramienta que bloqueen los clientes de BD más comunes (el kit solo propone, no ejecuta).
3. Decidirlo con un ADR si hay alternativas reales.

## Cómo se atendió (al resolver: qué se hizo y dónde)
RF-15 de `0017_PausaOrigenYContexto`: `template/.agents/commands/sdd-init.md` pregunta si existe un usuario de BD de solo lectura y deja constancia en `AGENTS.md` › Comandos (punto 1). El fragmento opcional `permissions.deny` de `template/.agents/skills/sdd/restricciones-de-rol.md` suma `mysql`, `psql`, `sqlcmd`, `sqlplus`, `sqlite3`, `mongosh` y `redis-cli`, y sigue siendo solo sugerencia (punto 2). Sin alternativas reales para el punto 3: no hizo falta ADR.

## Historial
- 2026-11-03 — Alta, en el cierre de `0008_GuardrailsBdSoloLectura`, con el visto bueno del usuario.
- 2026-11-22 — Pasa a En atención y a Resuelta en el cierre de `0017_PausaOrigenYContexto`, con el visto bueno de la persona.
