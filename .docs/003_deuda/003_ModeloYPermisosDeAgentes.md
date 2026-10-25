# 003 — Modelo por rol y permisos del frontmatter de los agentes

| Dato | Valor |
|---|---|
| Estado | Abierta |
| Alta | 2026-10-25 |
| Origen | Recorte de alcance en `003_OptimizarArnes` (duda 1, decisión del usuario, 2026-10-25) |
| Módulo | Agentes del kit (`coordinator`, `planner`, `implementer`, `reviewer`) |
| Prioridad | Media |
| Atendida por | — |

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
—

## Historial
- 2026-10-25 — Alta.
- 2026-10-25 — Decisión del usuario: esperar a que `003_OptimizarArnes` esté implementada antes de atenderla (mismos archivos de agentes, medición limpia y un solo manifiesto). Al retomarla se abre como spec propia por el flujo `sdd`; el primer paso es verificar qué campos del frontmatter respeta Claude Code.
