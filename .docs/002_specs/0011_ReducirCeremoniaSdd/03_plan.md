# 03 — Plan: Reducir la ceremonia del flujo SDD
Spec: 01_spec.md (aprobada 2026-11-03 22:14)
Plan: aprobado por el usuario 2026-11-03 22:22 (con las decisiones 9 a 15 de `05_avance.md`; el ADR 0023 «Ceremonia proporcional» se crea al cierre).

## 1. Ubicación y capas
Solo Markdown (ADR 004), sin código ni `sql/`. Capas del kit: skill `sdd` (fuente de las reglas), comandos y agentes (enlazan, no copian), `AGENTS.md` (grupo B), docs de publicación de la raíz. Lo distribuible se edita en `template/`; la réplica `.agents/` de la raíz se refresca con `cp` y se comprueba con `diff -rq`.

Principio 3: las reglas nuevas se escriben una vez en la pieza nueva `template/.agents/skills/sdd/topes-y-modos.md`: tope de 3 pasadas, tope de 3 marcadores con prioridad y desempate, bloqueo, lista de 8 condiciones, umbrales, promoción y revisión cruzada. Los comandos la enlazan en una frase.

## 2. Archivos que se crean o modifican
Todas las rutas `template/` tienen réplica en `.agents/` (no cuentan como archivos aparte). Bytes de hoy en `02_analisis_impacto.md` § 2.3.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/sdd/topes-y-modos.md` (nuevo) | Skill, A | Fuente única: definición de pasada, tope 3, nota «pasada n de 3», marcador `[ACLARAR: …]` (regla, prioridad alcance > seguridad > experiencia > detalle, desempate, bloqueo), 8 condiciones y umbrales, spec corta, promoción, revisión cruzada | 1-9, 11-16 |
| `template/.agents/skills/sdd/SKILL.md` | Skill, A | Línea 97 sin copiar el marcador (remite a `plantillas.md` y a la pieza nueva); fila de enlace a la pieza; `01_spec.md` y `06_flujo.md` según modo | 7-14, 19 |
| `template/.agents/skills/sdd/plantillas.md` | Skill, A | Línea 25: `- [ACLARAR: <duda>]` (forma única del marcador); `01_spec.md` corta; `04_tasks.md` con plan dentro; fila «Modo» en PUNTO DE RETOMO de `05_avance.md` | 10, 13, 19 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Skill, A | Remite a la fila «Modo» y a «pasada n de 3» sin repetir su regla; si la tabla de PUNTO DE RETOMO es la definición, ahí va la fila | 1, 5, 13 |
| `template/.agents/commands/sdd-clarify.md` | Comando, A | Cuenta la pasada, anota «pasada n de 3», aplica el cierre de tope (RF-6), una sola pasada en ligero | 1, 2, 3, 6, 11 |
| `template/.agents/commands/sdd-spec.md` | Comando, A | Rellena la lista de 8 condiciones con sí/no y evidencia, propone modo, una confirmación, crea los documentos del modo | 2, 3, 7-10, 13 |
| `template/.agents/commands/sdd-plan.md` | Comando, A | Paso 1: para si hay marcadores sin resolver y dice cuáles; anota la decisión al resolver | 4, 5 |
| `template/.agents/commands/sdd-tasks.md` | Comando, A | En ligero: `04_tasks.md` con plan dentro y bloqueo por marcadores (hallazgo 8) | 4, 10, 11 |
| `template/.agents/commands/sdd-change.md` | Comando, A | Promoción a completo (RF-12): qué se conserva y qué se completa | 12 |
| `template/.agents/commands/sdd-status.md` | Comando, A | Lee el modo desde PUNTO DE RETOMO y no espera `02`, `03`, `06` en ligero ni en specs antiguas | 13, 14 |
| `template/.agents/commands/sdd-cierre.md` | Comando, A | `06_flujo.md` aprobado solo si el modo es completo | 14 |
| `template/.agents/agents/reviewer.md` | Agente, A | Modo «revisión cruzada», solo lectura, sin comando; una sola pasada si el modo es ligero | 1, 16 |
| `template/.agents/agents/coordinator.md` | Agente, A | Al cerrar cada paso en ligero, revisa las condiciones salvo la 7 y promueve; fila de ruteo de la revisión cruzada | 12, 16 |
| `template/AGENTS.md` (línea 104) | Global, B | Seis documentos en modo completo; en ligero, `01`, `04`, `05` | 10, 18 |
| `AGENTS.md` raíz (línea 105) | Mantenimiento versionado | La misma frase, conservando lo propio del clon | 10 |
| `template/.docs/002_specs/README.md` (línea 17) y `.docs/002_specs/README.md` raíz (línea 17) | Doc, C / mantenimiento | «Toda carpeta nace en formato SDD» admite el modo ligero; la fila 0011 la actualiza `/sdd-cierre` | 14 |
| `VersionAgentKitSdd` | Publicación | `0.7.0` | 18 |
| `ChangelogAgentKitSdd.md` | Publicación | Entrada 0.7.0 y nota para proyectos instalados | 18 |
| `manifiestos/0.7.0.md` (nuevo) | Publicación | 44 filas con huellas `kit_huella` | 18 |

No cambian: `planner.md`, `implementer.md`, `sdd.md` (enruta por la tabla del coordinator), `sdd-implement.md`, `sdd-flujo.md`, `restricciones-de-rol.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`.

## 3. Base de datos
No aplica. Sin `sql/`.

## 4. Seguridad
Sin datos, credenciales ni secretos. Las consultas de datos no aplican (`AGENTS.md` › Comandos). Los Guardrails se conservan: el modo de revisión cruzada es de solo lectura y no amplía las herramientas del reviewer (`Read, Glob, Grep, Bash`, ya sin `Write`). Las puertas de aprobación de spec y tareas se mantienen en ligero (RF-11). Se aplica `seguridad-basica` en modo «solo secretos» antes del commit.

## 5. Liberación / piloto
Versión 0.7.0 del kit. Los proyectos instalados reciben el grupo A con `ACTUALIZAR.md` y la pieza nueva `topes-y-modos.md` como archivo nuevo. Hasta que incorporan a mano la línea de `AGENTS.md`, sigue operando el flujo completo. Detalle de qué ve cada grupo en § 6.12.

## 6. Decisiones
| # | Decisión | Alternativa descartada | Por qué |
|---|---|---|---|
| 6.1 | Reglas nuevas en pieza propia `topes-y-modos.md`, hermana de `avance-y-tiempos.md` y `restricciones-de-rol.md` | Añadirlas a `SKILL.md` (9 501 B) | Cada comando que cargue la skill leería ~3,5 KB más aunque no los use; la pieza se enlaza solo donde se aplica. Sigue el patrón de 003 y 004 |
| 6.2 | Umbrales de 3 archivos y 1 hora sin cambio | Subir a 5 u 8 archivos | La tabla de calibración (02 § 3.1) no tiene specs con menos de 9 archivos; subir el umbral no cambia ninguna decisión histórica y relaja la puerta sin evidencia. Revisar tras 3 ligeros reales |
| 6.3 | **Hallazgo 5, spec corta**: conserva Contexto y objetivo (hasta 5 líneas), Requisitos funcionales (EARS, hasta 8 RF), Casos límite (solo los propios), Criterios de finalización y Dudas abiertas. Omite Usuarios y roles, Historias, Definiciones, Requisitos no funcionales y Fuera de alcance, salvo que una condición lo exija. Límite: 5 000 bytes y 8 RF. Si lo excede, se promueve a completo | Dejar «corta» sin medida | Sin medida, RF-10 no es verificable (hallazgo 5). 5 000 B es menos que la spec más pequeña de la historia (002: 6 748 B) |
| 6.4 | **Hallazgo 6, caso de referencia**: dos mediciones separadas. M1 (pasadas, medida): 0010 con 7 pasadas, contra el tope de 3 y el dato real de 0011 con 2; bytes de lectura de pasada = spec + constitución + `sdd-clarify.md` + `reviewer.md`, con los tamaños nuevos tras las ediciones. M2 (documentos, hipotética): la spec completa más pequeña con plan (006, 34 031 B) frente al conjunto ligero con sus límites (spec ≤ 5 000 B, `04` con plan ≤ 8 000 B, `05` real). Criterio de selección: 0010 por ser la de más pasadas; 006 por ser la más chica, el caso más favorable al ligero | Un solo caso sumando pasadas y documentos | Mezclar bytes de documentos con pasadas oculta cuál cambio ahorra. Límite declarado: lecturas contadas, no respuestas ni tokens; M2 es una cota, se confirma con el primer ligero real |
| 6.5 | **Hallazgo 8, bloqueo en ligero**: `/sdd-tasks` aplica el bloqueo por marcadores sin resolver (paso 1, antes de delegar) cuando `Modo` es ligero. En completo lo aplica `/sdd-plan` y `/sdd-tasks` no lo repite | Bloquear en `/sdd-implement` | Es la primera puerta posterior a la spec en ligero; implement sería tarde |
| 6.6 | **Hallazgo 9, pasada al promover**: la pasada ya hecha cuenta como pasada 1 del completo; el contador sigue en «pasada n de 3» | Reiniciar a 0 | Es una revisión del reviewer sobre la misma spec; reiniciar permitiría 4 pasadas en total y reabriría la espiral que se quiere evitar |
| 6.7 | **Hallazgo 10, modo al promover**: la sesión principal corrige la fila `Modo` de PUNTO DE RETOMO a `completo (promovido de ligero, AAAA-MM-DD HH:MM, condición N)`, deja la lista de condiciones original en Decisiones y anota la hora. Mientras `02` y `03` no existen, `/sdd-status` y los demás comandos toman el modo registrado, no la lista de documentos: la fase es «plan pendiente» y el siguiente paso `/sdd-plan`. `04` y `01` se conservan y se concilian | Reescribir `05_avance.md` desde cero | Se conserva el historial y no hay estado intermedio ambiguo |
| 6.8 | **Hallazgo 11, publicación**: `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `manifiestos/<versión>.md` no cuentan en la condición 1 (son archivos de publicación que manda la constitución). Los cuenta como tarea del `04_tasks.md` (RF-15) | Contarlos como archivos tocados | Con ellos, ningún cambio distribuible llegaría a 3 archivos y se vaciaría la decisión 3 de 05_avance. Si otra spec en curso también hace bump, la condición 2 aplica y va a completo |
| 6.9 | **Hallazgo 12, fuente única del marcador**: la forma `[ACLARAR: <duda>]` se define solo en `plantillas.md:25`; la regla (cuándo, tope, prioridad, bloqueo) solo en `topes-y-modos.md`; `SKILL.md:97` pasa a «dudas abiertas (marcadores)» y enlaza ambas sin copiar el contenido; `sdd-clarify`, `sdd-spec` y `sdd-plan` enlazan la pieza. Prueba: `command grep -rn 'ACLARAR' template` solo muestra la forma en `plantillas.md` y la regla en `topes-y-modos.md` más los enlaces | Repetir la regla en cada comando | Principio 3. Un tope repetido en cinco archivos se desfasa |
| 6.10 | **Nota de `AGENTS.md`**: la línea 104 de `template/AGENTS.md` y la 105 de `AGENTS.md` pasan a «`01_spec.md`, `04_tasks.md` y `05_avance.md` siempre; `02_analisis_impacto.md`, `03_plan.md` y `06_flujo.md` en modo completo; `sql/` si hay cambios de datos», con enlace a `topes-y-modos.md` y sin copiar las condiciones | Dejar seis documentos y que la skill lo matice | Contradicción entre `AGENTS.md` y la skill; `AGENTS.md` es la fuente de verdad (ADR 003) |
| 6.11 | **RF-16 entra en 0.7.0**: estimado 15 min (ver § 7, F4) < 30. El procedimiento vive en la pieza nueva, el reviewer lo enlaza y el coordinator lo enruta | Comando `/sdd-analyze` (fuera de alcance de la spec) | Spec, RF-16: sin comando nuevo |
| 6.12 | **Qué ve cada proyecto instalado** (según `ACTUALIZAR.md` › Grupos por ruta): grupo A (13 piezas): los KIT-VIEJO se actualizan y los PERSONALIZADOS se preguntan; `topes-y-modos.md` es ruta nueva y se agrega. Grupo B (`AGENTS.md`): no se pisa; el changelog dice qué línea incorporar a mano, en dos frases: (1) la línea de los documentos por feature, (2) nada más en `AGENTS.md`. Grupo C (`.docs/002_specs/README.md`): no se pisa; la línea 17 es opcional. Mientras no se incorpore la línea, un proyecto con comandos nuevos y `AGENTS.md` viejo exige seis documentos en la lectura de `AGENTS.md` pero los comandos permiten ligero: el conflicto lo resuelve el changelog («incorpora la línea antes de usar el modo ligero») | Cambiar `AGENTS.md` por la actualización | ADR 003 y `ACTUALIZAR.md`: el grupo B nunca se sobrescribe |

ADR candidatos (no se crean; decide el usuario). Último ADR: 0022.
- **Candidato 1, recomendado: modo ligero con 8 condiciones y promoción obligatoria.** Alternativas reales: ligero a criterio libre; excluir lo distribuible (descartada por el usuario, decisión 3). Efecto duradero: cambia la ceremonia de todo el kit y los proyectos.
- **Candidato 2, recomendado: tope de 3 pasadas y marcador único `[ACLARAR: …]` con prioridad.** Alternativas reales: sin tope; dos marcadores; pasada extra (decisiones 2, 4 y 5 de 05_avance). Podría ir en el mismo ADR que el 1: «Ceremonia proporcional».
- Sin ADR: pieza `topes-y-modos.md` (consecuencia del ADR 009), umbrales provisionales (se registran en la pieza) y revisión cruzada (sin comando, spec fuera de alcance).

## 7. Fases y estimado
Estimado en minutos de ejecución con agentes; las ediciones Markdown reales suelen tardar un quinto (0008, 0010). La espera del usuario va aparte y no se estima. Cada paso `Fn.m` es verificable por comando.

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Preparación: `git status`, `diff -rq template/.agents .agents`, `command grep` del marcador y de líneas 104/105; confirmar que no hay otra spec en curso | — | 5 |
| F1 | Escribir `topes-y-modos.md` completo (fuente única) y editar `SKILL.md` (línea 97, fila de enlace, `01`/`06` según modo). Verificación: `file -i`, sin BOM, `name`, rutas citadas existen | 1-9, 11-15, 19 | 20 |
| F2 | `plantillas.md`: marcador, spec corta (límite 5 000 B, 8 RF), `04` con plan dentro (≤ 8 000 B), fila «Modo»; `avance-y-tiempos.md` | 5, 10, 13, 19 | 15 |
| F3 | Siete comandos: `sdd-clarify`, `sdd-spec`, `sdd-plan`, `sdd-tasks`, `sdd-change`, `sdd-status`, `sdd-cierre`; cada uno con una frase que enlaza la pieza | 1-14 | 28 |
| F4 | RF-16 y RF-12 en agentes: `reviewer.md` (modo cruzado, una pasada en ligero), `coordinator.md` (promoción al cerrar paso, fila de ruteo). Estimación de RF-16 sola: procedimiento en la pieza 4 min + `reviewer.md` 4 + fila del coordinator 3 + verificación 4 = 15 min | 12, 16 | 15 |
| F5 | `AGENTS.md` (línea 104 y 105) y los dos `README` de specs (línea 17) | 10, 14 | 10 |
| F6 | Réplica: `cp` de cada archivo editado de `template/.agents/` a `.agents/`; `diff -rq template/.agents .agents` | 18, criterio 5 | 5 |
| F7 | Verificación y medición (§ 8): chequeos por archivo, `command grep` de fuente única, ensayo de instalación en `mktemp -d`, M1 y M2 de RF-17 en `05_avance.md`, comprobación de que las 3 reglas de lectura de 003 no crecieron | todos, 17 | 30 |
| F8 | Revisión: `/sdd-validate` por el reviewer (3 pasadas máximo). Sin escritura | todos | 15 |
| F9 (última, tras la última edición de `template/`) | `VersionAgentKitSdd` a `0.7.0`; entrada de `ChangelogAgentKitSdd.md` con la nota para proyectos instalados; `manifiestos/0.7.0.md` con 44 filas calculadas con `kit_huella` (`ACTUALIZAR.md`, líneas 125-134) y comparación con 0.6.0 (solo cambian las piezas editadas más una fila nueva); si cualquier archivo de `template/` cambia después, se regenera el manifiesto entero | 18 | 20 |

Total: 163 min estimados; real esperado ≈ 35 min. Aparte, sin estimar: aprobación de plan y tareas, decisión de ADR, visto bueno de la deuda de RF-16 si no entrara, y el cierre `/sdd-cierre` (más ADR si se aprueban).

Regla para RF-16: estimado 15 min < 30, entra. Si al planear las tareas el estimado superara 30 min, F4 conserva solo RF-12 y RF-16 pasa a deuda con visto bueno.

## 8. Criterios de aceptación
Comandos de `AGENTS.md` › Comandos. Cada uno con salida esperada.
1. Por archivo tocado de `template/` y de la raíz: `file -i <archivo>` indica `utf-8`; `command grep -l $'^\xEF\xBB\xBF' <archivo>` vacío; `name` del frontmatter igual a su carpeta; rutas citadas existentes (no aplica a `manifiestos/`); en `template/` sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
2. `diff -rq template/.agents .agents` solo muestra `napkin.md` y `skills/caveman`.
3. Fuente única: `command grep -rn 'NECESITA ACLARACI' template .agents AGENTS.md` da 0; `command grep -rn 'ACLARAR' template` muestra solo la forma en `plantillas.md`, la regla en `topes-y-modos.md` y enlaces, sin copiar el tope ni la prioridad en otra pieza; el número «3 pasadas» aparece en `topes-y-modos.md` y no en otro archivo (salvo la nota «pasada n de 3» que cita la pieza).
4. Ensayo: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia todo sin pisar nada y con 44 archivos; nunca en un proyecto real. Se repite con un destino que ya tenga un `AGENTS.md` propio y se comprueba que no cambia.
5. RF-17: `05_avance.md` lleva M1 y M2 con los bytes de `wc -c`, el número de pasadas y de documentos por separado y el límite de la métrica.
6. Lecturas de 003: `sdd-status.md` sigue leyendo solo `Estado`, lista de documentos y PUNTO DE RETOMO, Pasos, Pruebas y Tiempos (el modo vive en PUNTO DE RETOMO, sin lectura nueva); `implementer.md` y `sdd-implement.md` sin cambios (`git diff --stat` los muestra fuera).
7. `git diff --stat` no muestra cambios en `.docs/002_specs/001_*` a `0010_*` (criterio 8 de la spec).
8. Publicación (F9, al final): `cat VersionAgentKitSdd` da `0.7.0`; el changelog trae la entrada y la nota de `AGENTS.md`; `manifiestos/0.7.0.md` tiene 44 filas y cada huella coincide al recalcular con `kit_huella` sobre `template/` (0 diferencias), sin ediciones posteriores de `template/`.
9. RF-16: el reviewer en modo cruzado tiene `tools` sin `Write` ni `Edit`, no hay comando nuevo (`ls template/.agents/commands | wc -l` igual a hoy, 15).
10. Cierre: `/sdd-cierre` deja la fila 0011 del índice al día y propone ADR según § 6.
