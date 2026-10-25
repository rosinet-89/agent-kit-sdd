---
name: sdd
description: >
  Flujo Spec-Driven Development (SDD) para todo análisis o desarrollo nuevo: spec,
  análisis de impacto, plan, tareas, implementación, validación, flujo operativo y cierre,
  con los agentes coordinator, planner, implementer y reviewer, en
  `.docs/002_specs/<NNNN>_<Feature>/` (01_spec … 06_flujo + sql/) y registro de tiempos
  reales en `05_avance.md`. Activar cuando el usuario pida: desarrollo nuevo, feature,
  requerimiento, análisis de impacto, spec, plan, tareas, avance, tiempos, flujo operativo,
  o use un comando `/sdd*`. Disparadores: "SDD", "spec", "01_spec", "03_plan", "04_tasks",
  "05_avance", "06_flujo", "002_specs", "001_constitucion", "003_deuda", "punto de retomo".
---

# SDD — Spec-Driven Development

Flujo canónico para **análisis y desarrollos nuevos**: toda carpeta nueva nace con este formato.
No aplica a un fix chico sin carpeta. Si el alcance crece a feature, se abre carpeta con este flujo.

Si `AGENTS.md` contiene `SDD-INIT-PENDIENTE`, el proyecto no está personalizado: corre `/sdd-init` primero.

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
.docs/002_specs/<NNNN>_<NombreFeature>/
  01_spec.md               # QUÉ y POR QUÉ (RF en EARS) y Estado
  02_analisis_impacto.md   # módulos, tablas, archivos, datos reales, riesgos
  03_plan.md               # CÓMO: capas, archivos, sql/, fases con estimado (h)
  04_tasks.md              # fases F0..Fn, pasos Fn.m, checklist "Hecho cuando"
  05_avance.md             # PUNTO DE RETOMO, decisiones con hora, pruebas, TIEMPOS
  06_flujo.md              # flujo operativo de lo implementado (usuario final)
  07_pruebas_qa.md         # opcional: evidencia formal de pruebas
  sql/NN_<tipo>_<desc>.sql # DDL, seeds; 99_rollback_*.sql. EJECUCIÓN MANUAL
  tests/                   # opcional: harness de solo lectura
```

- `NNNN`: 4 dígitos en un proyecto (`0001`) para specs, deudas y ADR. Siguiente libre contando la copia local y la rama remota, sin repetir prefijos (`.docs/002_specs/README.md` › "Numeración única"). No se reutilizan números. En el clon del kit manda el ancho de ese README. Las carpetas de sección de `.docs/` siguen en 3.
- `NombreFeature`: PascalCase, sin espacios. Ej.: `0017_ReporteVentasPorZona`.
- Plantillas de cada archivo: [plantillas.md](plantillas.md).
- Contrato de avance, decisiones y tiempos: [avance-y-tiempos.md](avance-y-tiempos.md).

## Fases, comandos y responsables

Las fases del flujo tienen nombre (`spec`, `clarify`…); `F0..Fn` y `Fn.m` son solo las fases y pasos del plan.

| Fase | Enlace | Rol | Documento |
|---|---|---|---|
| `spec` | `/sdd-spec` | planner | `01_spec.md` |
| `clarify` | `/sdd-clarify` | reviewer | `01_spec.md` |
| `plan` | `/sdd-plan` | planner | `02_analisis_impacto.md`, `03_plan.md` |
| `tasks` | `/sdd-tasks` | planner | `04_tasks.md` |
| Implementación | `/sdd-implement` | implementer | el paso `Fn.m` |
| `validate` | `/sdd-validate` | reviewer | `05_avance.md` |
| `flujo` | `/sdd-flujo` | planner, reviewer | `06_flujo.md` |
| `cierre` | `/sdd-cierre` | sesión principal | `05_avance.md` |
| `change` | `/sdd-change` | planner | `01_spec.md` |
| Estado | `/sdd-status` | sesión principal | — |
| Estructura del kit | `/sdd-estructura` | sesión principal | — |
| Entrada única | `/sdd` | coordinator | según la fase |

`/sdd` es el único comando que usas. El coordinator clasifica la petición con la tabla
"Cómo decides qué hacer" de `.agents/agents/coordinator.md`, elige un comando de la tabla
de arriba y lo ejecuta. Esos comandos también se pueden lanzar a mano.

Si quien aplica esta skill es el chat y no el agente `coordinator`, lanza a ese subagente
con la petición y no ejecuta las fases. El coordinator elige el comando, lo sigue, delega
en `planner`, `implementer` y `reviewer`, y lleva `05_avance.md`. El cierre, el estado y la
estructura siguen en la sesión principal. Si la atención entra por el coordinator, él sigue
esos comandos.

- **Solo análisis** (sin desarrollo): termina en `plan` con `Estado: analisis` en `01_spec.md`.
- Tiempos, pausas y quién escribe `05_avance.md`: [avance-y-tiempos.md](avance-y-tiempos.md). No se redefinen aquí.
- Los subagentes no ven la conversación: en cada llamada se les pasa fase, petición original, decisiones, rutas y el resultado de la fase anterior.

## Contenido mínimo por archivo

- **01_spec.md** — Estado (`borrador | aprobada | analisis | implementada`), contexto y objetivo, usuarios y roles, historias, RF en EARS, RNF, casos límite, fuera de alcance, criterios de finalización, dudas abiertas `[NECESITA ACLARACIÓN]`. Solo el QUÉ y el POR QUÉ: nada de archivos, tablas ni arquitectura.
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

- Un paso a la vez; al terminarlo se marca en `04_tasks.md` y se para.
- Las reglas de edición de archivos (encoding, herramientas permitidas) son las de `AGENTS.md` › Stack y convenciones.
- Datos: los agentes solo ejecutan consultas de solo lectura. DDL/DML se **genera** en `sql/` con su rollback y lo ejecuta el usuario. Cómo se escribe: `.agents/skills/migracion-bd/SKILL.md`.
- Dominio: leer la skill del tema (tabla de `AGENTS.md`) antes de tocar esa parte.

## Anti-patrones

- Codear sin `01_spec.md` aprobada o sin `03_plan.md`/`04_tasks.md` aprobados.
- Crear `05_avance.md` al final o reconstruir los tiempos de memoria.
- Un subagente escribiendo `05_avance.md`.
- Spec con nombres de archivo o tablas; plan sin estimado por fase.
- SQL de escritura ejecutado por un agente, o SQL suelto fuera de `sql/`.
- Cerrar un feature con un RF sin verificar.
