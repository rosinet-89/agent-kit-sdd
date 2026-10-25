---
description: SDD · Genera 02_analisis_impacto.md y 03_plan.md de una spec aprobada
argument-hint: <prefijo_NombreFeature>
---

Fase `plan`. Carpeta: `.docs/002_specs/$1/`.

Qué lee: `01_spec.md` y las decisiones de la sesión. La forma de los dos documentos está en `.agents/skills/sdd/plantillas.md`, secciones `## 02_analisis_impacto.md` y `## 03_plan.md`. Si hay `sql/`, `.agents/skills/migracion-bd/SKILL.md`. Un ADR solo si se pide: `.agents/skills/adr/SKILL.md`.

Qué hace:

1. Si `01_spec.md` no dice `Estado: aprobada` o tiene dudas abiertas, para y avisa.
2. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
3. Delega en el subagente **planner**, con la spec y las decisiones. Los documentos siguen esas secciones de `plantillas.md`. Los datos reales salen de consultas de solo lectura, con fecha y origen. Si una decisión tiene alternativas reales y efecto duradero, el planner la marca; se decide si se registra como ADR. No se crea sin visto bueno.
4. Muestra un resumen de ambos. Si es solo análisis, cambia `01_spec.md` a `Estado: analisis` y termina aquí.
5. La sesión principal registra la fila `plan` según `.agents/skills/sdd/avance-y-tiempos.md`. Siguiente paso: `/sdd-tasks $1`.

Qué no hace: no escribe código. No repite el contenido de `plantillas.md`. No copia el formato de la fila de tiempos ni el arranque de sesión. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
