# 02 — Análisis de impacto: Reducir la ceremonia del flujo SDD

Datos del 2026-11-03, solo lectura. Origen de cada dato entre corchetes. Los bytes salen de `wc -c` o `stat -c %s`; `grep` se corrió como `command grep` porque `grep` es un shim.

## 1. Resumen del requerimiento
Tres cambios al proceso: tope de 3 pasadas de clarify y de 3 marcadores `[ACLARAR: …]` que bloquean el plan (RF-1 a RF-6, RF-19); modo ligero con 8 condiciones, promoción a completo y documentos ausentes tolerados (RF-7 a RF-15); y, si el plan lo estima en menos de 30 min, revisión cruzada como modo del reviewer (RF-16). Cierra con medición en bytes (RF-17) y versión 0.7.0 (RF-18). Solo Markdown (ADR 004).

### 1.1 Antecedentes
- `003_OptimizarArnes` (Implementado 2026-10-25) [`.docs/002_specs/README.md`]. Ya hizo: lecturas mínimas por paso del implementer y de `/sdd-status`, cada comando declara «Qué lee», el contrato de avance y tiempos vive en `avance-y-tiempos.md`, y se midió en bytes con un caso congelado. No hizo: tope de pasadas de clarify ni modos de ceremonia. Esta spec no repite sus lecturas; las preserva (RNF).
- `0010_FalsoPositivoInitPendiente`: 7 pasadas de clarify; 24 min de implementación contra 115 estimados [`05_avance.md` de 0010]. Es el caso de referencia del escenario «antes».
- `0008`: 31 min contra 150 [`05_avance.md` de 0008].
- `005_ConvivenciaNumeracion` y `0009`: fijaron que la numeración y los enlaces viven en una sola fuente; este plan sigue ese patrón (pieza propia en `skills/sdd/`, los comandos la enlazan).
- Búsqueda del marcador anterior [`command grep -rn 'NECESITA ACLARACI' template AGENTS.md .agents`, 2026-11-03]: 4 coincidencias, 2 archivos y sus 2 réplicas: `skills/sdd/plantillas.md:25` y `skills/sdd/SKILL.md:97`. Ningún comando, agente ni `AGENTS.md` lo cita.

### 1.2 Deuda técnica relacionada [`.docs/003_deuda/README.md`, 2026-11-03]
- Deuda 0011 (`/sdd-constitucion` fuera de la tabla de fases y del ruteo): abierta. Toca las mismas piezas que este plan (tabla de `SKILL.md` y `coordinator.md`). No la resuelve; solo hay que cuidar que las ediciones no se pisen si se atiende después.
- Deudas 0009 y 0010: sin relación.
- Por abrir, sin visto bueno: la observación 8 de `05_avance.md` (`/sdd-spec` y `/sdd-clarify` no sustituyeron `$1`). No se crea aquí.
- Si RF-16 no entra, se registra como deuda nueva (criterio 9).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
Sin menú. Se tocan comandos `/sdd-clarify`, `/sdd-spec`, `/sdd-plan`, `/sdd-tasks`, `/sdd-change`, `/sdd-status`, `/sdd-cierre` y, para RF-16, el reviewer sin comando nuevo.

### 2.2 Tablas
No aplica (sin BD, sin `sql/`).

### 2.3 Archivos de código (ruta, capa, bytes hoy, RF)
Bytes actuales [`wc -c`, 2026-11-03]. Cada archivo de `.agents/` de `template/` tiene su réplica en `.agents/` de la raíz (idéntica por `diff -rq`, salvo `napkin.md` y `skills/caveman`) [`diff -rq template/.agents .agents`].

| Archivo (bajo `template/` salvo indicación) | Capa / grupo | Bytes | RF |
|---|---|---|---|
| `.agents/skills/sdd/topes-y-modos.md` (nuevo) | Skill, A | — (≈3 500 previstos) | 1-9, 11-15, 16 |
| `.agents/skills/sdd/SKILL.md` | Skill, A | 9 501 | 7-14, 16, 19 (línea 97) |
| `.agents/skills/sdd/plantillas.md` | Skill, A | 3 970 | 10, 13, 19 (línea 25) |
| `.agents/skills/sdd/avance-y-tiempos.md` | Skill, A | 6 270 | 1, 5, 13 |
| `.agents/commands/sdd-clarify.md` | Comando, A | 1 496 | 1, 2, 3, 6, 11 |
| `.agents/commands/sdd-spec.md` | Comando, A | 2 177 | 2, 3, 7, 8, 9, 10, 13 |
| `.agents/commands/sdd-plan.md` | Comando, A | 1 466 | 4, 5 |
| `.agents/commands/sdd-tasks.md` | Comando, A | 1 113 | 4, 10, 11 |
| `.agents/commands/sdd-change.md` | Comando, A | 1 320 | 12 |
| `.agents/commands/sdd-status.md` | Comando, A | 920 | 13, 14 |
| `.agents/commands/sdd-cierre.md` | Comando, A | 2 189 | 14 (línea 16 exige `06_flujo.md` aprobado) |
| `.agents/agents/reviewer.md` | Agente, A | 2 385 | 1, 16 |
| `.agents/agents/coordinator.md` | Agente, A | 4 948 | 12, 16 |
| `AGENTS.md` (línea 104) | Global, B | 11 608 | 10, 18 |
| `.docs/002_specs/README.md` (línea 17) | Doc, C | 7 210 | 14 |
| Raíz: `AGENTS.md` (línea 105) | Mantenimiento versionado | 13 063 | 10 |
| Raíz: `.docs/002_specs/README.md` (línea 17 y fila 0011 al cerrar) | Mantenimiento versionado | 8 365 | 14 |
| Raíz: `VersionAgentKitSdd` | Publicación | 5 | 18 |
| Raíz: `ChangelogAgentKitSdd.md` | Publicación | 9 511 | 18 |
| Raíz: `manifiestos/0.7.0.md` (nuevo) | Publicación | — (0.6.0: 4 526) | 18 |

Total archivos distintos de `template/.agents/`: 13 (1 nuevo, 12 modificados) más 2 `AGENTS.md`, 2 README de specs y 3 de publicación. No se tocan: `restricciones-de-rol.md`, `planner.md`, `implementer.md`, `sdd.md`, `sdd-implement.md`, `sdd-flujo.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, `.agents/README.md`.

Por qué `sdd.md` y `planner.md` no cambian: `sdd.md` enruta por la tabla del coordinator; `planner.md` remite al comando, y `sdd-spec.md` es quien le pasa la lista de condiciones.

Esta spec cumple lo opuesto a las 8 condiciones (más de 3 archivos, toca `AGENTS.md`, comportamiento nuevo): va en modo completo.

### 2.4 Código legado o sin uso
- Las specs `001` a `0010` no se tocan (criterio 8); sus carpetas de 6, 5 o 2 documentos siguen válidas (RF-14).
- `template/.docs/002_specs/README.md:17` («Toda carpeta nace en formato SDD: `01` … `06`») contradice el modo ligero; se corrige en el kit y en la raíz.

## 3. Datos reales
### 3.1 Línea base de umbrales: tabla de calibración (specs 001 a 0010)
Origen: tabla «Archivos que se crean o modifican» y «Fases y estimado» de cada `03_plan.md`, y «Plan contra real» de cada `05_avance.md` [2026-11-03]. Archivos = filas del plan sin contar réplicas, `05_avance.md` ni ADR, contando los archivos que una fila enumera; cifra aproximada (≈). Real = minutos de implementación (sin spec, clarify, plan, tasks, que incluyen esperas del usuario).

| Spec | Archivos ≈ | Estimado | Real impl. | Real / est. | ¿Ligero? y condiciones que falla |
|---|---|---|---|---|---|
| 001_ActualizarKit | 12 | 870 min (14,5 h) | 55 min | 6 % | No: 1, 3, 4, 6, 7, 8 |
| 002_Kit050 | sin plan | sin estimado | retroactivo | — | No evaluable: solo tiene `01` y `05`; ante la duda, completo |
| 003_OptimizarArnes | 27 | 240 min (4 h) | 37 min | 15 % | No: 1, 3, 6, 7, 8 |
| 004_ModeloYPermisosAgentes | 11 | 205 min | 26 min | 13 % | No: 1, 3, 6, 7, 8 |
| 005_ConvivenciaNumeracion | 24 | 89 min | 34 min | 38 % | No: 1, 3, 4, 6, 7, 8 |
| 006_RutaSiNoDelega | 13 | 135 min (2,25 h) | 6 min | 4 % | No: 1, 6, 7, 8 |
| 007_DeudasAbiertas | 15 | 85 min | 84 min | 99 % | No: 1, 3, 7, 8 |
| 0008_GuardrailsBdSoloLectura | 13 | 150 min | ~31 min | 21 % | No: 1, 3, 4, 6, 7, 8 |
| 0009_EnlacesTrasPull | 17 | 846 min (14,1 h) | ~18 min | 2 % | No: 1, 3, 4, 6, 7, 8 |
| 0010_FalsoPositivoInitPendiente | 9 (+2 de ADR) | 115 min | 24 min | 21 % | No: 1, 3, 4, 7, 8 |

Lecturas:
- Ninguna spec de la historia habría sido ligera (0 de 10; la 002 no se puede evaluar). El mínimo de archivos es 9; el mínimo estimado es 85 min. Fallan siempre las condiciones 1, 7 y 8.
- El real fue de 2 % a 99 % del estimado y, salvo la 007, estuvo por debajo del 40 %; la mediana de las nueve con dato ronda el 15 %.
- Conclusión sobre 3 archivos / 1 hora: los datos no los contradicen ni los respaldan, porque el historial solo contiene cambios más grandes que el umbral. Un umbral de 5 o de 8 archivos tampoco cambiaría ninguna decisión (el mínimo es 9; las specs de 9 a 11 archivos fallan también 3, 4, 7 y 8). Propuesta: no cambiar los umbrales ahora (no hay propuesta para `/sdd-change`). Sí conviene registrar en cada `05_avance.md` ligero el resultado (archivos, estimado, real) y revisar los umbrales tras 3 usos reales. Observación para la condición 7: el sesgo de sobreestimación (~5 veces) hace que «1 hora estimada» equivalga a unos 12 min reales; el planner debe estimar en minutos por paso de edición mínima (1 a 3 min), como indican los «Plan contra real» de 0008 y 0010.

### 3.2 Caso de referencia: bytes del escenario «antes»
Origen: `wc -c` y `stat -c %s` de las carpetas de `0010`, `0011` y de las piezas que cada pasada lee [2026-11-03].

Bytes de documentos de 0010 (completo, sin `06_flujo.md`, que 0010 no tiene): `01` 17 257 + `02` 14 071 + `03` 18 364 + `04` 14 780 + `05` 21 644 = 86 116 B. Solo `02`+`03`: 32 435 B.

Costo de lectura por pasada de clarify (lo que `/sdd-clarify` manda leer): `01_spec.md` + constitución (1 133) + `sdd-clarify.md` (1 496) + `reviewer.md` (2 385). Con la spec final de 0010 (17 257) como cota alta: 22 271 B por pasada.

| Escenario | Pasadas | Bytes de lectura de pasadas |
|---|---|---|
| Antes: 0010 real | 7 | 155 897 |
| Después: tope de RF-1 | ≤ 3 | ≤ 66 813 (−57 %, es 3/7) |
| Dato: 0011 real | 2 | 44 232 (spec 17 102) |

Límites ya visibles: se cuentan lecturas, no las respuestas del reviewer ni los ajustes del planner; la spec crece entre pasadas, así que la cota es alta; faltan las secciones de `AGENTS.md` que el reviewer también lee; no se miden tokens. Bytes de documentos y número de pasadas se informan por separado (hallazgo 6).

### 3.3 Calibración del ahorro por modo ligero
Ningún spec histórico es elegible, así que el ahorro por documentos no se puede medir en un caso real de la historia. Referencia aproximada: la spec completa más pequeña con plan, `006`: `01` 10 249 + `02` 6 756 + `03` 5 091 + `04` 4 596 + `05` 7 339 = 34 031 B. Un ligero omitiría `02` y `03` (11 847 B) pero sumaría un plan dentro de `04`. Es un escenario hipotético, no una medición; se declara como tal y se confirma con el primer cambio ligero real.

### 3.4 Estado de la réplica y del manifiesto (línea base de F9)
- `template/` tiene 43 archivos; `manifiestos/0.6.0.md` tiene 43 filas; las 43 huellas coinciden con `kit_huella` de `ACTUALIZAR.md` (líneas 125-134) recalculadas hoy, sin diferencias. 0.7.0 tendrá 44 (suma `topes-y-modos.md`).
- `VersionAgentKitSdd` = `0.6.0`.
- `diff -rq template/.agents .agents`: solo `napkin.md` distinto y `skills/caveman` solo en la raíz.
- Árbol de trabajo: carpeta 0011 sin seguimiento y `.docs/002_specs/README.md` modificado; sin otra spec en curso (condición 2 no aplica).

## 4. Flujo actual
`/sdd-spec` → `/sdd-clarify` (sin tope) → `/sdd-plan` (para si hay «dudas abiertas») → `/sdd-tasks` → `/sdd-implement` → `/sdd-validate` → `/sdd-flujo` → `/sdd-cierre` (exige `06_flujo.md` aprobado). La skill `sdd` y `AGENTS.md:104/105` declaran los seis documentos. Cualquier cambio, del tamaño que sea, produce `01` a `06`.

## 5. Reglas de negocio: conocidas vs código
- `sdd-plan.md` paso 1: «tiene dudas abiertas» (sin definir cuáles). Pasa a marcadores `[ACLARAR: …]` sin resolver.
- `sdd-cierre.md:16` exige `06_flujo.md` aprobado: en ligero no existe. Se corrige.
- `sdd-status.md` ya deduce la fase por los documentos presentes si falta `05_avance.md`; con el modo dentro de PUNTO DE RETOMO (que ya lee) no hay lectura nueva.
- `sdd-change.md` hoy señala qué partes de `02`, `03` y `04` cambian; pasa a incluir la promoción (RF-12).
- Constitución (`.docs/001_constitucion/constitution.md`): principio 3 (cada regla en una fuente) condiciona dónde se escriben tope, condiciones y marcador; principio 6 exige `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` en cada versión.

## 6. Riesgos y casos borde
1. El modo ligero puede no usarse nunca con los umbrales fijos (ver 3.1). Se mitiga registrando usos y revisando tras 3.
2. Dos fuentes del marcador (`plantillas.md:25` y `SKILL.md:97`): hallazgo 12; se resuelve en el plan.
3. `AGENTS.md` es grupo B: el proyecto instalado no recibe el cambio de la línea 104 sin acción manual; hasta entonces opera el flujo completo (edge de la spec).
4. Edición simultánea con la deuda 0011 en `SKILL.md` y `coordinator.md`: sin otra spec en curso hoy.
5. El manifiesto debe generarse tras la última edición de `template/` o quedará desfasado (E20).
6. Que `04_tasks.md` con plan dentro crezca y encarezca la lectura del implementer (003). Se mitiga con límite de longitud y lectura por identificador.
7. Transición del marcador: esta spec se rige por `[NECESITA ACLARACIÓN]` hasta publicar 0.7.0 (RF-19).

## 7. Preguntas abiertas
Ninguna que bloquee el plan. Dudas de decisión del usuario van en `03_plan.md` § 6 y en el resumen de la respuesta del planner.
