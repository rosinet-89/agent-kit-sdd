---
description: SDD · Cambio de requisito - primero la spec, luego el plan y el código
argument-hint: <NNNN_NombreFeature> <cambio solicitado>
---
Cambio de requisitos de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`.
Cambio: el resto de `$ARGUMENTS` después de `$1`. NO toques código.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner** la actualización de `01_spec.md`: RF nuevo o
   modificado en EARS, sus casos límite y lo que queda fuera de alcance.
3. Indica qué partes de `02_analisis_impacto.md`, `03_plan.md` y `04_tasks.md` habría que
   cambiar después, y si cambia el estimado.
4. Muéstrame el diff de la spec y espera mi aprobación. Anota la decisión con su hora en
   `05_avance.md` y registra la fila `change` de tiempos.
