# ADR 0023: Ceremonia proporcional: modo ligero, tope de pasadas y marcador único

- **Estado:** Aceptado
- **Fecha:** 2026-11-03
- **Origen:** `0011_ReducirCeremoniaSdd`

## Contexto
El flujo SDD pedía siempre seis documentos y no limitaba las pasadas de clarify: la spec 0010 llegó a 7 pasadas (159 201 B leídos solo en revisiones, estimación en bytes y no tokens reales). Un cambio chico pagaba la misma ceremonia que uno grande. Había además dos formas de marcar una duda (`[NECESITA ACLARACIÓN]` y supuestos sueltos) sin tope ni prioridad. El kit es solo Markdown sin scripts (ADR 004) y cada regla debe tener una fuente por fase (ADR 009).

## Decisión
La ceremonia es proporcional al cambio, con tres reglas que viven en una sola pieza, `skills/sdd/topes-y-modos.md`, y que los comandos y agentes solo enlazan:
1. **Tope de pasadas de clarify:** 3 por spec en modo completo (1 en ligero); una pasada es cada revisión del reviewer. Al tope, lo pendiente de alcance y seguridad pasa a `[ACLARAR: …]` y decide la persona; el resto pasa a supuestos explícitos.
2. **Marcador único `[ACLARAR: …]`:** hasta 3 por spec, con prioridad alcance > seguridad > experiencia > detalle técnico. Bloquea `/sdd-plan` (en ligero, `/sdd-tasks`).
3. **Modo ligero:** `01_spec.md` corta, `04_tasks.md` con el plan dentro y `05_avance.md`; sin `02`, `03` ni `06`. Solo si se cumplen las 8 condiciones (3 archivos o menos, sin spec compartida, sin reglas globales, sin ADR, sin datos, sin comportamiento nuevo, 1 hora o menos, un solo módulo). Si una se incumple, se promueve a completo de forma obligatoria y la pasada ya hecha cuenta como pasada 1.

Los umbrales de 3 archivos y 1 hora son provisionales y se revisan tras 3 usos reales de modo ligero.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Modo ligero a criterio libre del agente | Sin condiciones comprobables, cada quien lo aplicaría distinto y se usaría para evitar la revisión |
| Excluir del ligero lo que se distribuye | Casi todo el trabajo del kit se distribuye: vaciaría el modo. El bump y el changelog van como tarea de `04_tasks.md` |
| Sin tope de pasadas | Es la espiral de 7 pasadas de 0010 |
| Dos marcadores (`[NECESITA ACLARACIÓN]` y `[ACLARAR]`) | Dos formas que explicar y aplicar sin ganancia |
| Una pasada extra al llegar al tope | Reabre la espiral; el tope debe cerrar con decisión de la persona |
| Ampliar `SKILL.md` en vez de una pieza propia | Todo comando que carga la skill leería unos 3,5 KB de más en cada uso |
| Dos ADR separados (modo ligero y tope de pasadas) | Las reglas comparten pieza, marcador y puerta de promoción; se leen y cambian juntas |

## Consecuencias
Se gana un tope comprobable de revisiones y un camino corto para cambios chicos, con una única fuente que mantener. Ninguna spec 001 a 0010 habría sido elegible para ligero: el ahorro comprobado está en el tope de pasadas (M1, 0010: 159 201 B a 68 229 B, −57 %, en bytes leídos y no en tokens reales); el ahorro del modo ligero es una cota hipotética hasta su primer uso real. Los proyectos instalados con un `AGENTS.md` viejo deben incorporar a mano la línea de documentos por feature antes de usar el modo ligero (nota en el changelog y en `ACTUALIZAR.md`). Cambiar una condición, un umbral o el tope exige editar solo `topes-y-modos.md` y revisar este ADR.
