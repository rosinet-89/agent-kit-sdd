# Topes y modos del flujo SDD

Fuente única del tope de clarify, de los marcadores `[ACLARAR: …]`, del modo ligero, de la
promoción a completo y de la revisión cruzada. Los comandos y agentes enlazan este archivo. No
copian las reglas.

## Pasadas de clarify

- Una **pasada** es cada revisión del reviewer sobre `01_spec.md`. Los ajustes del planner entre
  revisiones no cuentan. Una pasada incompleta o interrumpida no cuenta; una solo de verificación
  cuenta si la hace el reviewer.
- Tope: 3 pasadas por spec en modo completo; 1 en modo ligero.
- La sesión principal anota «pasada n de 3» en la fila `clarify` de `05_avance.md`.
- Al llegar al tope con hallazgos pendientes no se abre otra pasada automática. Lo pendiente de
  alcance y seguridad pasa a marcadores `[ACLARAR: …]` (hasta 3, con la prioridad de abajo), el
  resto pasa a supuestos explícitos escritos en la spec, y la persona decide si aprueba así.

## Marcadores `[ACLARAR: …]`

- La forma del marcador está en [plantillas.md](plantillas.md), sección `01_spec.md`.
- Es una duda que impide redactar el plan (de alcance, seguridad, experiencia o detalle técnico). Una
  duda que no lo impide no es marcador: es un supuesto explícito.
- Tope: 3 marcadores por spec. Con más candidatas, prioridad alcance > seguridad > experiencia >
  detalle técnico; en empate dentro de la misma categoría gana la que afecta a más RF. El resto
  pasa a supuestos explícitos que la persona puede revertir al aprobar.
- Bloqueo: con un marcador sin resolver no se pasa a `/sdd-plan` (en modo ligero, a `/sdd-tasks`) y
  se dicen cuáles faltan.
- Al resolver uno, se reemplaza por la decisión y la sesión principal la anota con su hora en
  `05_avance.md`.

## Modo ligero

Para cambios chicos. Documentos: `01_spec.md` corta, `04_tasks.md` con el plan dentro y
`05_avance.md` con tiempos. No hay `02_analisis_impacto.md`, `03_plan.md` ni `06_flujo.md`. Se
mantienen las puertas de aprobación de la spec y de las tareas.

El planner rellena las 8 condiciones con sí o no y una línea de evidencia por condición, y propone
el modo. Si se cumplen todas, la persona confirma con una sola pregunta. Si alguna no se cumple o
no se puede comprobar, el modo es completo sin más confirmación que la habitual.

1. Toca 3 archivos o menos, sin contar réplicas ni los archivos de publicación (`VersionAgentKitSdd`,
   `ChangelogAgentKitSdd.md`, `manifiestos/`).
2. Ningún archivo compartido con otra spec en curso.
3. No cambia reglas globales: Guardrails, constitución, numeración ni los contratos entre piezas
   que citan los índices de `AGENTS.md`.
4. Sin decisión con alternativas reales (sin ADR).
5. Sin datos ni esquema (sin `sql/`).
6. Sin requisitos de comportamiento nuevos: no cambia qué hace o decide un comando, agente o
   chequeo (corrección de texto, bug acotado o ajuste de lo ya especificado).
7. Estimado de 1 hora o menos, juzgado solo con el estimado al planear.
8. Un solo módulo: archivos de una sola carpeta de primer nivel de `.agents` (`agents`, `commands`
   o `skills`), más sus réplicas.

Los umbrales de 3 archivos y 1 hora son fijos y provisionales: se revisan tras 3 usos reales. Cada
uso ligero se registra en `05_avance.md`, con la lista de condiciones evaluada en la fila `Modo` de
PUNTO DE RETOMO.

Si el cambio se distribuye (toca `template/` o los docs distribuibles) sigue siendo elegible y
`04_tasks.md` lleva como tarea el bump de `VersionAgentKitSdd` y la entrada del changelog.

**Spec corta:** conserva Contexto y objetivo (hasta 5 líneas), Requisitos funcionales (EARS, hasta
8 RF), Casos límite propios, Criterios de finalización y Dudas abiertas. Omite Usuarios y roles,
Historias, Definiciones, Requisitos no funcionales y Fuera de alcance, salvo que una condición lo
exija. Límite: 5 000 bytes y 8 RF. **`04_tasks.md` ligero:** plan dentro, hasta 8 000 bytes. Si
alguno excede, se promueve a completo.

## Promoción a completo

- El `coordinator`, al cerrar cada paso en ligero, revisa las condiciones salvo la 7. Si una se
  incumple, detiene el avance, dice cuál y promueve con `/sdd-change`. Un clarify que exigiría una
  segunda pasada también promueve.
- Se conservan `01_spec.md` y `04_tasks.md`; se completan `02`, `03` y `06`.
- La pasada de clarify ya hecha cuenta como pasada 1 del completo.
- La sesión principal corrige la fila `Modo` de PUNTO DE RETOMO a `completo (promovido de ligero,
  AAAA-MM-DD HH:MM, condición N)`, deja la lista original en Decisiones y anota la hora. Mientras
  no existan `02` y `03`, la fase es «plan pendiente» y el siguiente paso `/sdd-plan`.
- Carpetas con documentos ausentes por ser ligeras o anteriores a esta regla no son incompletas:
  ningún comando exige crear lo que falta.

## Revisión cruzada

La persona la pide. El reviewer, solo en lectura, cruza `01_spec.md`, `03_plan.md` y `04_tasks.md`
y lista requisitos sin tarea y contradicciones entre los tres. No modifica archivos y no es un
comando aparte.
