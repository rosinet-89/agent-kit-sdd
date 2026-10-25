---
description: SDD · Cambio de requisito - primero la spec, luego el plan y el código
argument-hint: <prefijo_NombreFeature> <cambio solicitado>
---

Cambio de requisitos. Carpeta: `.docs/002_specs/$1/`. Cambio: el resto de `$ARGUMENTS` después de `$1`.

Qué lee: `01_spec.md` y el cambio pedido. Para señalar el efecto, solo las partes de `02_analisis_impacto.md`, `03_plan.md` y `04_tasks.md` que el cambio toca.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner** la actualización de `01_spec.md`: RF nuevo o modificado en EARS, sus casos límite y lo que queda fuera de alcance.
3. Indica qué partes de `02_analisis_impacto.md`, `03_plan.md` y `04_tasks.md` habría que cambiar después, y si cambia el estimado. No las reescribas en este comando.
4. Muestra el contenido que cambió en la spec y espera la aprobación. Anota la decisión con su hora en `05_avance.md`. La sesión principal registra la fila `change` según `.agents/skills/sdd/avance-y-tiempos.md`.

Qué no hace: no toca código. No repite el formato de la fila de tiempos ni la secuencia de otra fase. No aplica el cambio al plan ni a las tareas hasta que la spec esté aprobada. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
