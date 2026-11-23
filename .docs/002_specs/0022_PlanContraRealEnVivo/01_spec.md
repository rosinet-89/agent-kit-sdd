# 01 — Spec: Plan contra real en vivo
Estado: borrador
Ticket: sin ticket

> Borrador de partida: registra la idea para que quede versionada. Los RF, el modo y la entrevista
> se hacen al retomar con `/sdd-spec 0022_PlanContraRealEnVivo`.

## Contexto y objetivo
Hoy el estimado de cada fase vive en `03_plan.md` › Fases (o en `04_tasks.md` como «estimado: N
min») y lo real, en la tabla Tiempos de `05_avance.md`. La tabla «Plan contra real» solo se escribe
en el cierre (`.agents/skills/sdd/avance-y-tiempos.md` › «Plan contra real (al cierre)»). Para saber
a media implementación cómo va el tiempo hay que leer dos archivos y sumar.

Palabras de la persona: «el avance registra los tiempos de tarea vs plan. Al final se corre Plan vs
avance, pero ambos datos ya los tenemos (plan) y vamos llenando los datos de avance. Si en un punto
quiero saber cómo vamos en cuestión de tiempo, tengo que leer el avance y el plan, cuando debería
poder leerlo en el mismo avance.»

Objetivo: abrir solo `05_avance.md` y ver por fase el estimado, el real acumulado y el delta.

## Requisitos funcionales (EARS)
Pendientes de la entrevista.

## Fuera de alcance
- Tokens y costo: los cubre `0021_BitacoraDeUso`.

## Dudas abiertas
Puntos de partida para la entrevista y la clarify (no son marcadores todavía):
- Fuente del estimado: `03_plan.md` (completo) o `04_tasks.md` (ligero). ¿Se copia al avance una
  vez, al aprobar las tareas?
- Cuándo se actualiza: al cerrar cada paso o lote. ¿Tabla viva que el cierre solo completa, o la
  misma tabla del cierre escrita desde `tasks`?
- Dónde: sección «Plan contra real» al final, subirla junto al PUNTO DE RETOMO o una fila «Tiempo»
  en el retomo (estimado total, real, delta).
- `/sdd-status` informa horas: que lea esa tabla en vez de sumar Tiempos.
- Lotes y olas paralelas: mantener la regla de la fila de lote (cuenta en la fase de su primer paso)
  y escribirla en la fuente.
- Documentación SDD (`spec`…`cierre`) sin estimado: renglón aparte, como hoy.
- Desvío fuerte: umbral (p. ej. real > 2× estimado) para avisar en el momento. En 0019 y 0020 lo
  real fue 4 a 5 veces menor que lo estimado: el ajuste de estimados puede ir aquí o aparte.
- Archivos probables: `template/.agents/skills/sdd/avance-y-tiempos.md` (fuente), `plantillas.md`,
  `commands/sdd-tasks.md`, `sdd-implement.md` y coordinator, `sdd-status.md`, `sdd-cierre.md`.
- Antecedentes: buscar en el índice `tiempos`, `avance`, `plan contra real` al retomar.
