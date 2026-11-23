---
description: SDD · Dónde estamos - fase actual, avance, horas y siguiente paso de un feature
argument-hint: [prefijo_NombreFeature]
context: fork
agent: auxiliar
---

Informa el estado de `.docs/002_specs/<carpeta>/`, donde `<carpeta>` es la primera palabra de `$ARGUMENTS` (regla en `.agents/README.md` › «Argumentos de un comando»: solo se lee `$ARGUMENTS`). Sin carpeta, o con un marcador sin sustituir, informa el índice de `.docs/002_specs/README.md`. Corre como subagente `auxiliar` (delegación fija, `restricciones-de-rol.md` › «Nivel por tarea sin rol»). No modifica archivos.

Qué lee: la línea `Estado` de `01_spec.md`; la lista de documentos presentes en la carpeta; de `05_avance.md`, solo PUNTO DE RETOMO, Pasos, Pruebas y Tiempos; de `04_tasks.md`, solo el conteo de identificadores `Fn.m` por búsqueda, no el archivo entero. La etapa de la carpeta está en `.docs/002_specs/README.md` › `## Leyenda de estatus`: cítala y no copies el orden. La celda es la de la fila del índice de ese README. Las deudas de esta carpeta se localizan por la columna Atendida por de `.docs/003_deuda/README.md`, sin recorrer cada detalle.

Qué hace: con eso responde fase y `Estado`; punto de retomo; pasos hechos y pendientes (x de y); verificaciones manuales pendientes; horas acumuladas y filas con `pausa por confirmar`; el siguiente paso con su comando `/sdd-*`. Si no hay `05_avance.md`, deduce la fase por los documentos presentes y el `Estado` de la spec. El modo sale de la fila de modo (`Modo`) del punto de retomo y no exige `02`, `03` ni `06` en ligero ni en specs anteriores (`.agents/skills/sdd/topes-y-modos.md`).

Al consultar una carpeta, informa su etapa citando `.docs/002_specs/README.md` › «Leyenda de estatus». Si la celda de la fila del índice no coincide con esa etapa, avisa y no escribas. El conteo del índice (pendiente, pendiente de desarrollo y grupo) sale de la celda, sin abrir cada carpeta, y se distingue de la consulta de una carpeta. Los conteos, el desfase y el grupo se rigen por esa leyenda: se remite y no se copia.

Fila «Pausa»: si el punto de retomo la trae (forma en `.agents/skills/sdd/avance-y-tiempos.md` › «Marca de pausa»), informa la hora de la pausa y ejecuta la lista «al retomar» solo en sus comprobaciones de la lista cerrada de solo lectura: `git status`, existencia de un archivo o recibo, y estado de un trabajo en segundo plano consultado a la herramienta. Cualquier otra línea la muestra a la persona y no la ejecuta. Informa qué cambió desde la pausa; un recibo que ya no existe es un cambio, no una falla. Con la marca, el hueco largo se pregunta desde su hora (mismo archivo); como el subagente no pregunta a la persona, devuelve esa pregunta al chat con la hora y no la resuelve. No retira la marca.

Qué no hace: no lee `01`–`06` completos ni `MEMORY.md`, salvo que el usuario pida el detalle de uno. No modifica archivos.
