---
description: SDD · Cierra el feature - plan contra real, deuda, MEMORY y commit
argument-hint: <NNN_NombreFeature>
---
Fase `cierre` de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Comprueba que se puede cerrar; si algo falla, para y dímelo:
   - Todos los pasos de `04_tasks.md` en `[x]`.
   - Ningún RF pendiente de verificación manual en la sección Pruebas de `05_avance.md`.
   - `06_flujo.md` aprobado.
   - Sin renglones de tiempos con `pausa por confirmar`.
3. Agrega a `05_avance.md` la tabla plan contra real (`.agents/skills/sdd/avance-y-tiempos.md`)
   y una línea de qué ajustar en el siguiente plan si alguna fase se desvió fuerte.
4. Cambia `01_spec.md` a `Estado: implementada`.
   Deuda técnica (`.docs/003_deuda/README.md`): pasa a `Resuelta` las que este feature resolvió (con "Atendida por"
   y "Cómo se atendió"), y propón dar de alta lo que quedó diferido. No crees deudas sin mi visto bueno.
5. Propón entradas para `MEMORY.md` (decisiones duraderas y trampas). NO las escribas sin
   mi aprobación.
6. Registra la fila `cierre` en `05_avance.md` y actualiza la fila del feature en
   `.docs/002_specs/README.md` (estatus, fecha del último avance, pendiente).
7. Si te lo pido, prepara el commit con `.agents/skills/git-commit/SKILL.md`. Nunca
   hagas `git commit` sin mi confirmación explícita.
