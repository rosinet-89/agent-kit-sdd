# 003 — Modelo por rol y permisos del frontmatter de los agentes

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-10-25 |
| Origen | Recorte de alcance en `003_OptimizarArnes` (duda 1, decisión del usuario, 2026-10-25) |
| Módulo | Agentes del kit (`coordinator`, `planner`, `implementer`, `reviewer`) |
| Prioridad | Media |
| Atendida por | `004_ModeloYPermisosAgentes` (cierre 2026-10-25 16:13) |

## Descripción
Dos asuntos de los agentes que `003_OptimizarArnes` dejó fuera:
1. **Modelo por rol:** ningún agente define `model:`; todos heredan el de la sesión. El coordinator (enruta y lleva tiempos) y el implementer (ejecuta un paso ya definido) podrían usar un modelo más barato, y el planner y el reviewer conservar el fuerte.
2. **Permisos del frontmatter:** los agentes declaran `mode:` y `permissions:` (formato de otra herramienta). No se ha verificado cómo los trata Claude Code; solo `tools` está garantizado. Hallazgos de la revisión: el reviewer tiene `Write` aunque no debe modificar código; el planner podría escribir fuera de su carpeta si el campo se ignora; el implementer tiene `shell` permitido sin impedir `git commit`, `push` ni `fetch` (que `AGENTS.md` prohíbe); el reviewer pide confirmación para cada comando de lint.

## Por qué quedó pendiente
Decisión del usuario: sin verificar el comportamiento real de la herramienta sería un cambio a ciegas, y mezclarlo con la reducción de lecturas dificulta saber qué causó una regresión.

## Impacto si no se atiende
- Sin modelo por rol, el costo por feature no baja por esa vía.
- Si `permissions:` se ignora, las restricciones (reviewer sin escritura, implementer sin commit) se cumplen solo porque las instrucciones lo dicen, sin garantía.

## Alcance (archivos, tablas)
`template/.agents/agents/*.md` (frontmatter), `.agents/commands/sdd-init.md` (sugerir reglas `allow` para los comandos de lint del proyecto), y su manifiesto y replica local.

## Cómo se atendería (propuesta)
1. Verificar en la versión de Claude Code en uso qué campos del frontmatter se respetan (`tools`, `model`, `permissionMode`, `permissions`, `mode`).
2. Según el resultado: añadir `model:` por rol; quitar `Write` al reviewer o acotarlo con un hook; denegar `git commit/push/fetch` al implementer; documentar qué campos son de otra herramienta.
3. Abrirlo como spec propia por el flujo `sdd`, con la medición del costo antes y después.

## Cómo se atendió (al resolver: qué se hizo y dónde)
`004_ModeloYPermisosAgentes`, cierre 2026-10-25 16:13. Verificación contra la documentación de subagentes de Claude Code (2026-10-25, decisión 2 del avance): `mode` y `permissions` no son campos soportados; `tools` sí; `omitClaudeMd` evita cargar `CLAUDE.md` y los `AGENTS.md` de proyecto; un `deny` de settings vale para toda la sesión. Los cuatro agentes de `template/.agents/agents/` declaran `model: inherit` (el alias `sonnet` no se sustituyó: no hubo corrida válida). `omitClaudeMd: true` en planner, reviewer y coordinator; el implementer no tiene la clave. Se quitaron `mode` y `permissions`. El reviewer quedó con `tools: Read, Glob, Grep, Bash`. La tabla de restricciones está en `template/.agents/skills/sdd/restricciones-de-rol.md` (ocho filas; filas 2 y 3 solo instrucción; el `deny` de Git es texto y no se copió a `.claude/`). Sin hooks. El paso 5 de `template/.agents/commands/sdd-validate.md` deja la evidencia formal en la sesión. «Qué lee cada rol» quedó en los dos `AGENTS.md`. Ítem en `CHANGELOG.md` dentro de 0.5.0. Manifiesto `0.5.0` regenerado (42 filas, E20 en 0). `/sdd-init` no sugiere `allow` ni `deny`: sigue fuera de este cierre, propuesto como deuda nueva.

## Historial
- 2026-10-25 — Alta.
- 2026-10-25 — Decisión del usuario: esperar a que `003_OptimizarArnes` esté implementada antes de atenderla (mismos archivos de agentes, medición limpia y un solo manifiesto). Al retomarla se abre como spec propia por el flujo `sdd`; el primer paso es verificar qué campos del frontmatter respeta Claude Code.
- 2026-10-25 — Se retoma tras implementar `003_OptimizarArnes`. Paso 1 hecho: verificación contra la documentación de Claude Code (ver `004_ModeloYPermisosAgentes/05_avance.md`, decisión 2). En atención por la spec `004`.
- 2026-10-25 16:13 — Resuelta por el cierre de `004_ModeloYPermisosAgentes`.
