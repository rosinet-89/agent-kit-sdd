# 004 — Convivencia de la numeración de 3 y 4 dígitos en un proyecto que se actualiza

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-10-25 |
| Origen | Hallazgo al revisar el producto de 0.5.0 (el changelog de 0.5.0 lo declara como no definido) |
| Módulo | Numeración de specs, deudas y ADR (reglas del `template/` y procedimiento de actualización) |
| Prioridad | Media |
| Atendida por | `005_ConvivenciaNumeracion` |

## Descripción
En 0.5.0 la numeración de los ítems de `.docs/002_specs/`, `.docs/003_deuda/` y `.docs/004_adr/` pasa a **4 dígitos** (`0001`) en un proyecto; las carpetas de sección de `.docs/` y el clon del kit siguen en 3. Un proyecto que ya tiene el kit y carpetas en 3 dígitos (`017_...`) recibe la regla nueva al actualizar, y ninguna pieza del kit dice cómo conviven los dos anchos.

## Por qué quedó pendiente
La numeración de 4 dígitos entró en `template/` junto con el procedimiento de actualización, pero no se definió qué hacer con lo que ya existe. El `CHANGELOG.md` de 0.5.0 lo deja dicho ("esa regla de convivencia no está definida todavía"). `003_OptimizarArnes` no lo incluyó en su alcance.

## Impacto si no se atiende
- Un proyecto con `017_` y `0018_` mezclados no tiene criterio para calcular el "siguiente libre" ni para la revisión de prefijos repetidos (chequeo E12), que compara prefijos con anchos distintos.
- `/sdd-spec`, la skill `git-commit` y los índices describen el ancho nuevo sin decir qué pasa con las carpetas anteriores; cada agente podría resolverlo distinto.
- El agente podría proponer renumerar carpetas ya citadas desde otras specs, deudas o ADR.

## Alcance (archivos, tablas)
`template/.docs/002_specs/README.md` (« Numeración única »), `template/.docs/003_deuda/README.md`, `template/.docs/004_adr/README.md`, `template/.agents/commands/sdd-spec.md`, `template/.agents/skills/git-commit/SKILL.md`, `template/.agents/skills/adr/SKILL.md`, `template/.agents/validar-estructura.md` (E12), `ACTUALIZAR.md` (qué hace con carpetas que no siguen el ancho nuevo) y `CHANGELOG.md`.

## Cómo se atendería (propuesta)
1. Decidir la regla: conservar el ancho existente de cada proyecto y no renumerar nunca; o fijar una convención de transición (las existentes siguen en 3, las nuevas usan 4, el "siguiente libre" se calcula por el valor numérico).
2. Reflejarla en una sola fuente (la sección « Numeración única ») y remitir a ella desde los comandos y skills.
3. Ajustar E12 para comparar el valor numérico, no el texto del prefijo.
4. Abrirlo como spec propia por el flujo `sdd`, con prueba en un proyecto simulado con carpetas en 3 dígitos. Cualquier cambio a `template/` regenera el manifiesto de la versión vigente y repite E20.

## Cómo se atendió (al resolver: qué se hizo y dónde)
Spec `005_ConvivenciaNumeracion`, implementada el 2026-10-25. Una sola fuente de la regla en « Numeración única » (`template/.docs/002_specs/README.md`): un ítem nuevo conserva el ancho de su carpeta, se compara por valor numérico, no se renumera nunca, y el desempate de un empate de valor usa el ítem publicado. Las piezas (AGENTS.md, skills, comandos, README de deuda y ADR) remiten a la fuente con `<prefijo>` y una regla mínima de una frase. E12 informa los anchos mezclados como `AVISO` y falla por duplicado por valor, ancho inválido y, en el clon, ancho distinto del declarado. `ACTUALIZAR.md` gana `## Aviso de numeración` (por nombres de carpeta, sin leer el grupo C). Validado por reviewers en `mktemp -d`; manifiesto de 0.5.0 regenerado y E20 42 de 42. Limitaciones conocidas en el avance de la spec (decisión 28).

## Historial
- 2026-10-25 — Alta, por decisión del usuario.
- 2026-10-25 — Se atiende por la spec `005_ConvivenciaNumeracion`, tras comprobar que no choca con la `004_ModeloYPermisosAgentes` en las fases de documentación (implementación en serie).
- 2026-10-25 — Resuelta por la spec `005_ConvivenciaNumeracion`.
