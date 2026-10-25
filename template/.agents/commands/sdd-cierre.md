---
description: SDD · Cierra el feature - plan contra real, deuda, MEMORY y commit
argument-hint: <prefijo_NombreFeature>
---

Fase `cierre`. Carpeta: `.docs/002_specs/$1/`. La ejecuta la sesión principal.

Qué lee: en `05_avance.md`, la tabla de Pasos, Pruebas y PUNTO DE RETOMO, y si `06_flujo.md` está aprobado. No lee `04_tasks.md`. Deuda en `.docs/003_deuda/README.md`. Un ADR, si se propone, con `.agents/skills/adr/SKILL.md`. La fila del índice, en `.docs/002_specs/README.md`. Plan contra real y la fila de tiempos, en `.agents/skills/sdd/avance-y-tiempos.md`.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Comprueba que se puede cerrar. Si algo falla, para y avisa:
   - En la tabla de Pasos de `05_avance.md`, localiza por búsqueda de patrón los pasos cuyo estado no sea `[x]`. Un `[ ]`, `[~]` o `[!]` impide el cierre, salvo que se decida diferirlo y quede registrado, con propuesta de deuda.
   - Ninguna verificación manual pendiente en Pruebas ni en PUNTO DE RETOMO.
   - `06_flujo.md` aprobado.
   - Sin renglones de tiempos con `pausa por confirmar`.
3. Agrega a `05_avance.md` la tabla plan contra real, según el contrato, y una línea de qué ajustar en el siguiente plan si alguna fase se desvió fuerte.
4. Cambia `01_spec.md` a `Estado: implementada`. En todo feature revisa deuda y ADR: pasa a `Resuelta` las deudas que este feature resolvió (con "Atendida por" y "Cómo se atendió") y propón dar de alta lo diferido. No crees deudas sin visto bueno.
5. Propón entradas para `MEMORY.md`. No las escribas sin aprobación. Si una decisión tiene alternativas reales y efecto duradero, propón un ADR en vez de una línea de memoria. Tampoco lo crees sin visto bueno.
6. Registra la fila `cierre` según el contrato y actualiza la fila del feature en `.docs/002_specs/README.md` (estatus, fecha del último avance, pendiente).
7. Si se pide, prepara el commit con `.agents/skills/git-commit/SKILL.md`. Nunca hagas `git commit` sin confirmación explícita.

Qué no hace: no lee las tareas completas. No copia el formato de la fila de tiempos. No crea deuda, memoria ni ADR sin visto bueno. No hace `git commit` sin confirmación explícita.
