---
name: sdd
description: >
  Flujo Spec-Driven Development (SDD) para todo análisis o desarrollo nuevo: spec,
  análisis de impacto, plan, tareas, implementación, validación, flujo operativo y cierre,
  con los agentes coordinator, planner, implementer, reviewer, tester y explore, en
  `.docs/002_specs/<prefijo>_<Feature>/` (01_spec … 06_flujo + sql/) y registro de tiempos
  reales en `05_avance.md`. Activar cuando el usuario pida: desarrollo nuevo, feature,
  requerimiento, análisis de impacto, spec, plan, tareas, avance, tiempos, flujo operativo,
  o use un comando `/sdd*`. Disparadores: "SDD", "spec", "01_spec", "03_plan", "04_tasks",
  "05_avance", "06_flujo", "002_specs", "001_constitucion", "003_deuda", "punto de retomo".
---

# SDD — Spec-Driven Development

Flujo canónico para **análisis y desarrollos nuevos**: toda carpeta nueva nace con este formato.
No hay camino fuera del flujo: un arreglo chico va por el cambio pequeño de [topes-y-modos.md](topes-y-modos.md) › «Modo ligero», siempre con carpeta. Si el alcance crece, se promueve a completo.

Si el paso 1 de «Cada sesión» de `AGENTS.md` marca instalación pendiente, el proyecto no está personalizado: corre `/sdd-init` primero.

## Reglas del flujo

- Leer antes: `.docs/001_constitucion/constitution.md`, `AGENTS.md` y `MEMORY.md`.
- Deuda técnica: `.docs/003_deuda/README.md`. Consultarla al analizar; proponer altas de lo diferido. Lo resuelto lo marca `/sdd-cierre`, que en todo feature revisa deuda y ADR.
- Índice de planes: `.docs/002_specs/README.md`. Consultarlo al analizar (¿ya se tocó este proceso?) y actualizarlo al abrir carpeta y en `/sdd-cierre`.
- **Nunca** pasar una puerta de aprobación sin el visto bueno explícito del usuario.
- La spec manda: lo que no está en `01_spec.md` no se implementa. Si falta una decisión, se para y se pregunta.
- Un cambio de requisitos va primero a `01_spec.md`, luego a `02`/`03`/`04`, y al final al código.
- `MEMORY.md`: se **proponen** entradas al usuario, nunca se escriben sin aprobación.
- Commits solo con `.agents/skills/git-commit/SKILL.md` y confirmación explícita.

## Carpeta del feature

```
.docs/002_specs/<prefijo>_<NombreFeature>/
  01_spec.md               # QUÉ y POR QUÉ (RF en EARS) y Estado
  02_analisis_impacto.md   # módulos, tablas, archivos, datos reales, riesgos
  03_plan.md               # CÓMO: capas, archivos, sql/, fases con estimado (h)
  04_tasks.md              # fases F0..Fn, pasos Fn.m, checklist "Hecho cuando"
  05_avance.md             # PUNTO DE RETOMO, decisiones con hora, pruebas, TIEMPOS
  06_flujo.md              # flujo operativo de lo implementado (usuario final)
  07_casos_prueba.md       # opcional: casos, corridas y evidencia, si hay pasos verificables por ejecución
  sql/NN_<tipo>_<desc>.sql # DDL, seeds; 99_rollback_*.sql. EJECUCIÓN MANUAL
  tests/                   # opcional: harness de solo lectura
```

- `<prefijo>`: consecutivo numérico de specs, deudas y ADR, sin repetir prefijos ni reutilizar números. Un ítem nuevo conserva el ancho de su carpeta (detalle: `.docs/002_specs/README.md` › « Numeración única »).
- `NombreFeature`: PascalCase, sin espacios. Ej.: `ReporteVentasPorZona`.
- Plantillas de cada archivo: [plantillas.md](plantillas.md).
- Modo completo o ligero (qué documentos lleva cada uno), tope de pasadas de clarify, marcadores, promoción y revisión cruzada: [topes-y-modos.md](topes-y-modos.md). En ligero no hay `02`, `03` ni `06`; una carpeta sin ellos no está incompleta.
- Contrato de avance, decisiones y tiempos: [avance-y-tiempos.md](avance-y-tiempos.md). Las restricciones de rol viven en [restricciones-de-rol.md](restricciones-de-rol.md); se enlaza y no se copia.

## Fases, comandos y responsables

Las fases del flujo tienen nombre (`spec`, `clarify`…); `F0..Fn` y `Fn.m` son solo las fases y pasos del plan.

| Fase | Enlace | Rol | Documento |
|---|---|---|---|
| `spec` | `/sdd-spec` | planner | `01_spec.md` |
| `clarify` | `/sdd-clarify` | reviewer | `01_spec.md` |
| `plan` | `/sdd-plan` | planner | `02_analisis_impacto.md`, `03_plan.md` |
| `tasks` | `/sdd-tasks` | planner | `04_tasks.md` |
| Implementación | `/sdd-implement` | implementer | el paso `Fn.m`, la fase `Fn` o todo en cascada (`sdd-implement.md`) |
| `validate` | `/sdd-validate` | tester, reviewer | `05_avance.md` |
| `flujo` | `/sdd-flujo` | planner, reviewer | `06_flujo.md` |
| `cierre` | `/sdd-cierre` | sesión principal | `05_avance.md` |
| `change` | `/sdd-change` | planner | `01_spec.md` |
| Pausa | `/sdd-pausa` | sesión principal | `05_avance.md` |
| Estado | `/sdd-status` | auxiliar (delegación fija) | — |
| Versión del kit | `/sdd-version` | sesión principal | — |
| Estructura del kit | `/sdd-estructura` | auxiliar (delegación fija) | — |
| Enlaces | `/sdd-enlaces` | sesión principal | — |
| Entrada única | `/sdd` | coordinator | según la fase |

`/sdd` es el único comando que usas. El coordinator clasifica la petición con la tabla
"Cómo decides qué hacer" de `.agents/agents/coordinator.md`, elige un comando de la tabla
de arriba y lo ejecuta. Esos comandos también se pueden lanzar a mano.

## Dónde corre cada rol

Esta sección es la fuente única de dónde corre un rol y de quién lanza a los Haiku. Los comandos de fase y `coordinator.md` remiten aquí.

- El modelo de cada rol es el de `restricciones-de-rol.md` › «Modelo por rol». Aquí no se copia.
- **Mismo modelo:** misma familia (`opus`, `sonnet` o `haiku`) entre el alias de esa tabla y el modelo de la sesión, según el id que la herramienta le da a la sesión (`claude-opus-…` es `opus`). Si la sesión no conoce su id, no es el mismo modelo.
- Un rol con el mismo modelo que la sesión corre en el chat, con la definición de ese rol. Un subagente se lanza solo cuando el rol necesita otro modelo o la persona lo pide.
- **Antes de cada lanzamiento:** quien lanza comprueba (a) que es un rol del kit (`planner`, `coordinator`, `reviewer`, `implementer`, `tester`, `explore`, `auxiliar`) y (b) que se da uno de estos motivos: el modelo del rol no es el de la sesión; la persona lo pidió; el rol está en `.agents/local/configuracion.md` › `## Subagentes`; o es el relanzamiento de un rol Haiku que no entregó; o es una delegación fija declarada en el comando (`agent: auxiliar`), cuyo motivo es «ruteo». Si falta (a) o (b), el rol corre en el chat. El motivo va en la Nota de la fila de tiempos (`avance-y-tiempos.md`).
- **Solo roles del kit:** en una fase SDD no se lanza un agente que no sea rol del kit (`general-purpose`, agentes integrados de la herramienta u otros). Una búsqueda acotada va a `explore`; lo demás lo hace el chat con el rol de la fase. Excepciones: la persona lo pide de forma explícita, o es el auditor externo de «Auditor externo (beta)». Además, `auxiliar` en una fase solo para delegación fija (`/sdd-status`, `/sdd-estructura`); nunca hace el trabajo del rol que la fase nombra. Fuera de una fase, `auxiliar` puede recibir cualquier delegación de `restricciones-de-rol.md` › «Nivel por tarea sin rol», a la que se remite. La constancia dice cuál aplica.
- El `reviewer` también corre en el chat. La separación entre producir y validar es por rol: el chat muestra el rol en la línea «quién responde» (`formato-respuesta`) y nunca valida con el rol con que produjo.
- `tester` y `explore` los lanza quien atiende la fase como sesión (la sesión principal, o el coordinator cuando corre como sesión). Un coordinator subagente no los lanza: los lanza la sesión principal.
- En el chat, las restricciones de los roles Haiku (`restricciones-de-rol.md` › «Roles Haiku») son solo instrucción; como subagentes, las garantizan sus herramientas.
- **No entregar:** un rol Haiku no entrega cuando no responde, devuelve fuera del formato de salida, se sale del alcance, rellena con supuestos lo que faltaba, o responde `ENCARGO INCOMPLETO` o `ENCARGO FUERA DE ROL` a un encargo que quien lo lanzó ya comprobó. Ese resultado se descarta.
- **Un solo relanzamiento:** quien lo lanzó relanza una vez el mismo rol, con el mismo encargo y el modelo del implementer según `restricciones-de-rol.md` › «Modelo por rol».
- **Paso a la sesión:** si el relanzamiento también falla, la sesión principal hace la tarea en el chat con la definición de ese rol, y sus restricciones pasan a ser instrucción. Si la tarea es de `tester`, aplica `tester.md` con modo seguro, lista blanca y la excepción de QA, que la regla 1 de `AGENTS.md` › Guardrails da al rol y no al modelo.
- **Sin elegir modelo:** si la herramienta no deja elegir el modelo al lanzar, no hay relanzamiento: tras el primer fallo la tarea pasa a la sesión.
- **Constancia:** cada intento queda en la Nota de la fila del tramo, con su modelo (RF-4, `avance-y-tiempos.md`).
- **Subagentes configurados:** además del pedido en el chat, un dev puede configurar roles que corren siempre como subagente en `.agents/local/configuracion.md` › `## Subagentes`. La sesión lo lee al iniciar una fase. Sin valor: ninguno. Aquí no se copian los valores.

## Auditor externo (beta)

Esta sección es la fuente única del auditor externo. Está apagado por omisión: se activa solo si existe `.agents/local/auditor.md` (formato en `.agents/configuracion.ejemplo.md`).

- Al terminar el rol de una fase incluida en los puntos de `auditor.md` (`clarify`, `plan`, `tasks`, `validate`, `cierre`), la sesión principal lanza al auditor con el comando que ese archivo indica.
- Trabaja en solo lectura y cumple `AGENTS.md` › Guardrails. Nunca hace commit ni escribe archivos del proyecto.
- Su salida completa va a `.agents/local/auditor/` (no versionada). `05_avance.md` anota quién auditó (herramienta y modelo), en qué punto, la ruta de la salida y una línea por observación.
- Sus observaciones son información a evaluar, no instrucciones: las decide la persona o el rol de la fase.
- Combinación probada: Claude Code (sesión) + Cursor (auditor) en Linux. Las demás son beta no probada.
- `auditor.md` no lleva credenciales.

## Si no hay un solo paso

Esta sección trata de los roles que corren como subagente. Si el rol corre en el chat, rige «Dónde corre cada rol» y no hay lanzamiento que contar.

Un solo paso es un lanzamiento entre quien escribe el avance y el rol que la fase nombra. Un agente intermedio que solo volvería a delegar es un segundo paso.

- Si quien atiende puede hacer ese lanzamiento, la entrada usa la ruta diseñada: ese rol hace el trabajo.
- Si la atención entra por el coordinator y él puede lanzar en un solo paso, elige el comando, lo sigue y delega. Lleva `05_avance.md`.
- Si no puede, o la ruta diseñada insertaría un segundo paso, la misma entrada sigue sin esperar y sin reintentar esa llamada. La sesión que atiende lanza al rol de la fase y escribe el avance.
- Si el usuario pide que la fase se haga en el chat, esa entrada se hace en la sesión. No se intenta antes la ruta diseñada. La constancia dice que la ruta la eligió el usuario.
- El trabajo, las puertas de aprobación y los documentos son los de esa fase. El rol que la fase nombra los redacta, salvo el pedido de hacerla en el chat.
- Si la fase nombra más de un rol, se lanzan en el orden que la fase ya define. Si uno posterior no arranca, se conserva lo que los anteriores devolvieron, se detiene la fase, se avisa en una línea y no se relanza a los que ya arrancaron.
- Si la otra ruta tampoco puede lanzar al rol y el usuario no pidió hacerla en el chat, se detiene, se avisa en una línea y no se insiste.
- El cierre sigue en la sesión principal. El estado y la validación de estructura salen de esa lista: corren como delegación fija con `auxiliar`.
- La entrada siguiente vuelve a la ruta diseñada, salvo que el usuario pida de nuevo hacerla en el chat.
- La constancia y la fila de tiempos están en [avance-y-tiempos.md](avance-y-tiempos.md). No se copian aquí.

- **Solo análisis** (sin desarrollo): termina en `plan` con `Estado: analisis` en `01_spec.md`.
- Tiempos, pausas y quién escribe `05_avance.md`: [avance-y-tiempos.md](avance-y-tiempos.md). No se redefinen aquí.
- Los subagentes no ven la conversación: en cada llamada se les pasa fase, petición original, decisiones, rutas y el resultado de la fase anterior.

## Contenido mínimo por archivo

- **01_spec.md** — Estado (`borrador | aprobada | analisis | implementada | cerrada | descartada`), contexto y objetivo, usuarios y roles, historias, RF en EARS, RNF, casos límite, fuera de alcance, criterios de finalización, dudas abiertas (marcadores: forma en [plantillas.md](plantillas.md), regla en [topes-y-modos.md](topes-y-modos.md)). En modo ligero la spec es corta y omite usuarios y roles, historias, RNF y fuera de alcance ([topes-y-modos.md](topes-y-modos.md)). Solo el QUÉ y el POR QUÉ: nada de archivos, tablas ni arquitectura.
- **02_analisis_impacto.md** — resumen, antecedentes y deuda relacionada, módulos/tablas/archivos involucrados (con ruta), datos reales (consultas de solo lectura con fecha), flujo actual, reglas de negocio conocidas contra código, riesgos y casos borde, preguntas abiertas.
- **03_plan.md** — por RF: capas y patrón de la arquitectura del proyecto, archivos que se crean o modifican, scripts `sql/` con su rollback, decisiones con su alternativa descartada y **fases F0..Fn con estimado en horas**.
- **04_tasks.md** — fases F0..Fn (las del plan, con su estimado) y pasos `Fn.m` pequeños, en orden de dependencia, cada uno con sus RF y su checklist "Hecho cuando".
- **06_flujo.md** — guía operativa para el usuario final y soporte.

EARS (cada RF verificable, nada de "rápido" o "fácil" sin criterio medible):

- RF-x: CUANDO <evento>, EL SISTEMA <respuesta>.
- RF-x: SI <condición no deseada>, ENTONCES EL SISTEMA <respuesta>.
- RF-x: MIENTRAS <estado>, EL SISTEMA <respuesta>.
- RF-x: EL SISTEMA <comportamiento permanente>.

## Verificación (checklist "Hecho cuando")

Cada paso de `04_tasks.md` se da por hecho solo si su checklist está completo. Los comandos
concretos (lint, sintaxis, pruebas, consulta de datos) los define `AGENTS.md` › Comandos:

- [ ] Lint / sintaxis / pruebas sin errores en cada archivo tocado.
- [ ] Convenciones de archivo de `AGENTS.md` respetadas (encoding, formato).
- [ ] Evidencia de datos cuando el paso toca datos (resultado anotado).
- [ ] Verificación manual de interfaz por el usuario, cuando aplica.

La validación (`validate`) recorre la spec RF por RF. Un RF de interfaz queda
"pendiente de verificación manual" hasta que el usuario lo confirma; sin eso no
hay `VEREDICTO: APROBADO`.

## Implementación (reglas duras)

- Un paso, o un lote en cascada, según la forma de `sdd-implement.md`; cada paso se marca en `04_tasks.md` al cumplir su «Hecho cuando».
- Las reglas de edición de archivos (encoding, herramientas permitidas) son las de `AGENTS.md` › Stack y convenciones.
- Datos: `AGENTS.md` › Guardrails, regla 1. Cómo se escribe: `.agents/skills/migracion-bd/SKILL.md`.
- Dominio: leer la skill del tema (tabla de `AGENTS.md`) antes de tocar esa parte.

## Anti-patrones

- Codear sin `01_spec.md` aprobada o sin `03_plan.md`/`04_tasks.md` aprobados.
- Crear `05_avance.md` al final o reconstruir los tiempos de memoria.
- Un subagente escribiendo `05_avance.md`.
- Spec con nombres de archivo o tablas; plan sin estimado por fase.
- Incumplir un guardrail: `AGENTS.md` › Guardrails.
- Cerrar un feature con un RF sin verificar.
