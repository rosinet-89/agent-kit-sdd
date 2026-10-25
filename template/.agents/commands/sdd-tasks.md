---
description: SDD · Divide 03_plan.md en fases y pasos verificables (04_tasks.md)
argument-hint: <prefijo_NombreFeature>
---

Fase `tasks`. Carpeta: `.docs/002_specs/$1/`.

Qué lee: `01_spec.md`, `03_plan.md` y `AGENTS.md` › Comandos. La forma de `04_tasks.md` está en `.agents/skills/sdd/plantillas.md`, sección `## 04_tasks.md`.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner** `04_tasks.md` a partir de la spec y del plan, siguiendo la sección `## 04_tasks.md` de `plantillas.md`. No copies esa plantilla en el comando.
3. Muestra fases, pasos y estimados. PARA hasta que se aprueben plan y tareas. Anota la aprobación con su hora.
4. La sesión principal registra la fila `tasks` según `.agents/skills/sdd/avance-y-tiempos.md`. Siguiente paso: `/sdd-implement $1 F0.1` (o el primer paso).

Qué no hace: no escribe código. No repite el contenido de `plantillas.md`. No copia el formato de la fila de tiempos ni el arranque de sesión. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
