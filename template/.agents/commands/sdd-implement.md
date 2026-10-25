---
description: SDD · Implementa UN paso de 04_tasks.md (uso - /sdd-implement 0017_NombreFeature F1.2)
argument-hint: <NNNN_NombreFeature> <Fn.m>
---
Implementación de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`. Paso: `$2`.

Si `04_tasks.md` no está aprobado, para y avísame.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`). Si desde la última fila de tiempos pasaron más
   de 30 minutos, pregúntame por la pausa antes de sumarla.
2. Delega en el subagente **implementer** SOLO el paso `$2`, siguiendo `03_plan.md`,
   `.docs/001_constitucion/constitution.md`, `AGENTS.md` y la skill `sdd`:
   - Convenciones y reglas de edición de archivos de `AGENTS.md`.
   - SQL solo generado en `sql/` con su rollback; nunca ejecutado.
   - Checklist "Hecho cuando" del paso, con los comandos de `AGENTS.md` › Comandos.
3. Muéstrame el resultado: archivos, resultado de los comandos, `sql/` que debo ejecutar y
   verificaciones manuales de UI pendientes.
4. Registra en `05_avance.md`: fila `$2` de tiempos, estado del paso, decisiones con hora,
   pruebas y PUNTO DE RETOMO (siguiente paso).

Después PARA. No empieces el siguiente paso.
