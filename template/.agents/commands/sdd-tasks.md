---
description: SDD · Divide 03_plan.md en fases y pasos verificables (04_tasks.md)
argument-hint: <NNN_NombreFeature>
---
Fase `tasks` de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner** `04_tasks.md` a partir de `01_spec.md` y `03_plan.md`,
   con la plantilla de la skill:
   - Las fases F0..Fn del plan, con su estimado en horas.
   - Pasos `Fn.m` pequeños, en orden de dependencia, cada uno con sus RF.
   - Checklist "Hecho cuando" por paso: los comandos de verificación de `AGENTS.md` › Comandos,
     evidencia de datos si toca datos, verificación manual de UI si aplica.
3. Muéstrame fases, pasos y estimados. PARA hasta que yo apruebe plan y tareas; anota la
   aprobación con su hora.
4. Registra la fila `tasks` en `05_avance.md` y el siguiente paso:
   `/sdd-implement $1 F0.1` (o el primer paso).
