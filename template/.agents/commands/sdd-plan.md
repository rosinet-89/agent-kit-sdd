---
description: SDD · Genera 02_analisis_impacto.md y 03_plan.md de una spec aprobada
argument-hint: <NNNN_NombreFeature>
---
Fase `plan` de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`. NO escribas código.

Si `01_spec.md` no dice `Estado: aprobada` o tiene dudas abiertas, para y avísame.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner**, pasándole la spec y mis decisiones:
   - `02_analisis_impacto.md`: módulos, tablas y archivos con ruta; datos reales con consultas de
     solo lectura (fecha y origen); flujo actual; reglas conocidas contra código; riesgos;
     preguntas abiertas.
   - `03_plan.md`: capas y patrón de la arquitectura del proyecto, archivos y RF que cubren,
     `sql/` con rollback si hay cambios de datos, seguridad, decisiones con su alternativa
     descartada y fases F0..Fn con **estimado en horas**. Si hay `sql/`, el planner sigue
     `.agents/skills/migracion-bd/SKILL.md`.
   - Si alguna decisión tiene alternativas reales y efecto duradero, el planner la marca y yo
     decido si se registra como ADR (`.agents/skills/adr/SKILL.md`). No se crea sin mi visto bueno.
3. Muéstrame un resumen de ambos. Si es solo análisis, cambia `01_spec.md` a
   `Estado: analisis` y termina aquí.
4. Registra la fila `plan` en `05_avance.md` y el siguiente paso: `/sdd-tasks $1`.
