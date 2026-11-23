# 05 — Avance: Bitácora de uso
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo` sin flujo (decisión 24). Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): 1 no (skill nueva, `sdd-config.md`, `configuracion.ejemplo.md`, `sdd-spec.md`, `sdd-cierre.md`, `template/AGENTS.md`, `README.md`, `INSTALAR.md`); 2 no (`0022_PlanContraRealEnVivo`, en `Análisis`, probablemente toca `sdd-cierre.md` y `avance-y-tiempos.md`); 3 no (agrega un paso a la lista fija de la configuración, que lee el chequeo de arranque); 4 no (alternativas reales en captura, almacenamiento y reporte; ADR candidato); 5 sí (sin `sql/`); 6 no (comportamiento nuevo en `/sdd-config`, `/sdd-spec` y `/sdd-cierre`); 7 no (más de 1 h); 8 no (`commands` y `skills`) |
| Última sesión | 2026-11-22 20:40 |
| Fase actual | `cierre` (Cerrado; 4 commits locales confirmados por la persona) |
| Último paso cerrado | `validate`: APROBADO en la vuelta 1 (2026-11-22 20:35); 19 de 19 pasos; antes `tasks`: `04_tasks.md` (19 pasos, 4 lotes en ola 1, ≈ 36 min) y `07_casos_prueba.md` (CP-01 a CP-09); aprobados |
| Siguiente paso | Ninguno: spec cerrada. Siguiente spec: `0022_PlanContraRealEnVivo` |
| Validación | Aprobada 2026-11-22 20:35: `VEREDICTO: APROBADO`, vuelta 1 de 2 (decisión 22) |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | `01_spec.md`, `02_analisis_impacto.md`, `03_plan.md`, `04_tasks.md`, `05_avance.md`, `07_casos_prueba.md` (sin commit) |

## Pasos
- [x] F0.1 Línea base
- [x] F1.1, F1.2, F1.3, F1.4 (L1); ajustes de F1.2 (decisión 21) y F1.4 (decisión 23)
- [x] F2.1, F2.2 (L2)
- [x] F3.1, F3.2, F3.3, F3.4 (L3)
- [x] F4.1, F4.2, F4.3 (L4)
- [x] F5.1 Réplica local
- [x] F5.2 `/sdd-enlaces` (sin acción: las 3 herramientas enlazan `skills/` como carpeta)
- [x] F5.3 Ensayo de instalación
- [x] F5.4 Chequeos (CP-01 a CP-09)
- [x] F6.1 Manifiesto 0.16.0 y E20
## Decisiones
1. **[2026-11-22 19:24] Se registran 0021 y 0022 y se empieza por 0021.** La persona: «registra las 2,
   luego iniciamos con la 21». Confirma el orden de la decisión 1 de `0020_RuteoDeModelos`. Insumo:
   `.agents/local/idea-0021-bitacora-de-uso.md`.
2. **[2026-11-22 19:25] Antecedentes.** Sin carpeta que trate la bitácora (grep del índice:
   `bitácora`, `tokens`, `medición`, `reporte`). Coincidencias parciales: `0020_RuteoDeModelos` (la Nota
   de cada fila de Tiempos ya lleva modelo y motivo del lanzamiento; su spec deja la bitácora fuera de
   alcance y la manda aquí) y `0014_ConfiguracionGuiada` (pasos de `/sdd-config` y patrón beta del
   auditor externo). Sin deuda abierta relacionada.
3. **[2026-11-22 19:25] Planner en el chat.** Mismo modelo que la sesión (opus); no está en
   `## Subagentes` de la configuración local (`SKILL.md` › «Dónde corre cada rol»).
4. **[2026-11-22 19:40] Interruptor global apagado por omisión** (persona). Como el auditor externo
   beta: con el interruptor apagado, `/sdd-spec` ofrece la bitácora como opción. Alternativa
   descartada: encendido por omisión. RF-1, RF-5.
5. **[2026-11-22 19:40] Captura por tramo y tokens al reportar** (persona). La sesión anota modelo,
   tiempo y nota con cada fila de Tiempos; los tokens se completan desde los transcripts al cerrar o
   al reportar. Alternativas descartadas: todo al cierre y solo a pedido. RF-8, RF-12.
6. **[2026-11-22 19:40] Reporte como texto para copiar** (persona). Sin archivos versionados, sin
   issue automático y sin red. Alternativas descartadas: archivo local y archivo versionado. RF-14,
   RF-15.
7. **[2026-11-22 19:40] Modo completo** sin confirmación adicional: no se cumplen las condiciones 1,
   2, 3, 4, 6, 7 y 8 (fila Modo).
8. **[2026-11-22 20:11] Autochequeo del planner: 6 correcciones** (no cuenta como pasada). Pregunta
   única en `/sdd-spec` en lugar de una «ronda de decisiones iniciales» que el comando no tiene;
   definición de «spec con bitácora activa»; RF nuevo para actividad con el interruptor apagado;
   hallazgos del reporte como lista cerrada; dos casos límite (actividad abierta y comando `/sdd-*`;
   carpeta de bitácoras inexistente); se quita el supuesto viejo S-3 por repetir un RF.
9. **[2026-11-22 20:12] Pasada 1 de clarify: 7 hallazgos, corregidos por el planner (modo
   automático).** «Tarea» fuera de `sdd` = petición (turno); RF-12 aplica en cada corrida de
   `/sdd-cierre`; RF renumerados 1 a 17 (sin «9b»); caso «apagado a mitad» alineado con la
   definición; RF-4 cubre carpeta existente sin decisión; tokens de caché aparte (supuesto S-3,
   marcado para revisión de alcance); RF-6 permite cambiar la decisión por spec.
10. **[2026-11-22 20:12] Pasada 2 (delta): 1 hallazgo, corregido.** RF-7 contradecía a RF-4; ahora
    aplica a comandos distintos de `/sdd-spec`. Pasada 3 (delta: RF-7 y quienes lo citan): sin
    hallazgos.
11. **[2026-11-22 20:14] Spec aprobada tal cual** (persona), con S-1, S-2 y S-3 (tokens de caché aparte).
    Alternativas descartadas: aprobar sin S-3; no aprobar aún.
12. **[2026-11-22 20:17] Plan: 11 decisiones en `03_plan.md` › 6.** Skill `bitacora-uso` con 8 secciones
    fijas, para que F1 a F4 vayan en una ola; disparo de RF-8 por una viñeta en `avance-y-tiempos.md`;
    tokens por ventana del tramo en UTC, una vez por `message.id`, con subagentes y `CLAUDE_CONFIG_DIR`
    (medido en los transcripts de este repo). Versión 0.16.0. ADR candidato (almacenamiento y captura de
    tokens): pendiente de visto bueno de la persona. Planner en el chat (decisión 3).
13. **[2026-11-22 20:18] ADR 0038 aprobado** (persona): almacenamiento de la bitácora y captura de
    tokens por ventana (decisiones 2 y 5 del plan). Se redacta al cierre con la skill `adr`.
    Alternativas descartadas: sin ADR; decidir al cierre.
14. **[2026-11-22 20:21] Plan y tareas aprobados** (persona: «yes, procede con el implementer»).
    Cascada completa; implementer como subagente (motivo: otro modelo, `sonnet`; además figura en
    `## Subagentes` de la configuración local). `04_tasks.md` lo marca la sesión tras revisar cada
    lote, para que los 4 implementers en paralelo no editen el mismo archivo.
15. **[2026-11-22 20:24] Línea base (F0.1).** `git status --short template/` vacío a las 20:21; las 4
    secciones citadas existen. `/sdd-estructura` (auxiliar, claude-sonnet-5-5, motivo «ruteo»): `NO
    alineada` por E3, E8 y E19, todas de las piezas locales ya aceptadas en `MEMORY.md` (`agents/claudio.md`,
    `skills/caveman`, `skills/reporte-sesiones`); no las causa esta spec. E20 OK contra 0.15.0. Corrió a
    la vez que la ola 1 (la sesión no esperó); la bandera quedó en 0.15.0 y se repite en F5.4.
16. **[2026-11-22 20:24] F3.1 y F3.2 sin renumerar** (sesión, antes de la ola 1): la pregunta va al final
    del paso 3 de `sdd-spec.md` y los tokens en el punto 1 de `sdd-cierre.md`, porque otros archivos
    citan esos números (`template/.docs/002_specs/README.md` cita «punto 16»). Se ajustaron F3.1, F3.2 y
    CP-03 (el título del paso 6 no contiene `## Bitácora`).
17. **[2026-11-22 20:24] Revisión de la ola 1: 4 correcciones de la sesión en `bitacora-uso/SKILL.md`** (menores,
    sin relanzar al implementer): ventana de una entrada de actividad (inicio del turno más minutos) y
    «fecha y hora» = Inicio de la fila; motivo del ejemplo «relanzamiento»; «Reporte» sin nombrar una
    herramienta (comparación por alias, RNF de kit genérico); hallazgo de relanzamiento por su motivo.
    También se reacomodó una línea larga de `configuracion.ejemplo.md`. L3 avisó que no leyó la lectura
    mínima y siguió solo el encargo; su diff cumple F3.
18. **[2026-11-22 20:26] F5 y F6.** Réplica con `cp` (`diff -rq` solo con las diferencias de `MEMORY.md`); F5.2 sin
    acción: `.claude`, `.cursor` y `.opencode` enlazan `skills/` como carpeta y ven `bitacora-uso`; ensayo
    en `mktemp -d`: 56 de 56 archivos idénticos, la segunda copia no pisa y la carpeta se borró;
    `/sdd-estructura skills` (auxiliar, claude-sonnet-5-5): E4 a E7 OK. Aviso: escribió la bandera
    `alineada: si` tras correr solo un bloque. Manifiesto 0.16.0: el algoritmo reproduce las 49 filas sin
    cambio de 0.15.0; E20 OK (56 filas, 56 archivos). CP-01 a CP-08 obtenidos; CP-09 espera a la persona.
19. **[2026-11-22 20:28] CP-09 corrido en la sesión** (persona: «Correrla ahora»). `## Bitácora` encendida en
    `.agents/local/configuracion.md`; `.agents/local/bitacora/actividad-prueba-bitacora.md` con 1 entrada,
    tokens completados y `estado: cerrada`. Observación para validate: una actividad la hace el chat, cuyo
    rol no figura en «Modelo por rol», así que su hallazgo de modelo siempre sale «sin comparar».
20. **[2026-11-22 20:29] Bitácora queda encendida** (persona), como piloto para `0022_PlanContraRealEnVivo`
    (`03_plan.md` › 5). Alternativa descartada: volver a `apagada`.
21. **[2026-11-22 20:32] Ajuste a «Registro por tramo»** (persona: «sí, agrégala y repite el manifiesto»): la
    entrada va en la misma escritura que la fila de Tiempos. Medido en esta sesión: cada llamada aparte
    relee ~213 k de caché; en una spec de ~12 filas eso suma un 25–30 % de lectura de caché, contra un
    2–3 % de salida si se combina. Cambio fuera del plan aprobado, consultado; se repiten la réplica, el
    manifiesto y E20. Desde la decisión 20 rige el interruptor `encendida` para 0021 (sin decisión por spec,
    RF-7): esta fila es la primera con entrada en la bitácora, escrita en la misma edición.
22. **[2026-11-22 20:35] Validate, vuelta 1: `VEREDICTO: APROBADO`** (reviewer en el chat, claude-opus-5-5; tester
    subagente, claude-haiku-5-5, motivo «otro modelo»). RF-1 a RF-17 con evidencia `archivo:línea`; lint
    OK en 12 archivos; seguridad en modo completo (la skill indica leer transcripts locales): sin secretos,
    sin rutas personales en lo distribuible, y el checklist no deja hallazgos. Dos opcionales sin aplicar: (a) rol sin fila
    en «Modelo por rol» (el chat de una actividad) → «sin comparar»; (b) «ventana con pausa se cuenta
    aparte» va en los totales y no como un quinto hallazgo de RF-14. Alcance: `.docs/002_specs/README.md`
    mezcla las filas 0021 y 0022, y la carpeta `0022_PlanContraRealEnVivo/` viene de la decisión 1;
    lo de 0022 lo decide la persona al hacer el commit.
23. **[2026-11-22 20:35] Opcionales (a) y (b) aplicados** (persona: «Aplicar ambos ahora»): en «Reporte», un rol sin
    fila en «Modelo por rol» da «sin comparar», y «ventana con pausa» se suma aparte en los totales, sin
    ser hallazgo. Repetidos la copia a `.agents/`, el manifiesto (solo la fila de la skill) y E20 OK. Alternativas
    descartadas: deuda; descartar. Sin otra vuelta del reviewer: el cambio es el texto exacto que propuso.
24. **[2026-11-22 20:36] Sin flujo** (persona: «no requiere flujo pasa al cierre»). Sin `06_flujo.md`: el
    cambio es Markdown del kit, sin usuario final de una app; el modo queda «completo sin flujo», como en
    `0019_FlujoEficiente` (decisión 15) y `0020_RuteoDeModelos` (decisión 36).
25. **[2026-11-22 20:37] Tabla Pasos llenada en el cierre** (sesión): la cascada marcó `04_tasks.md` y no copió los
    pasos a Pasos de `05_avance.md`, que la comprobación previa exige. Se llena con los 19 pasos, ya
    hechos y revisados.
26. **[2026-11-22 20:37] Tokens completados en el cierre** (punto 1 de `sdd-cierre.md`): F1.2, validate y F1.4, con
    una sesión en cada ventana. Hallazgo: validate (20:33–20:35) y F1.4 (20:35) comparten el minuto 20:35,
    así que los mensajes de ese minuto cuentan en las dos entradas. ADR 0038 escrito y en el índice
    (decisión 13).
27. **[2026-11-22 20:39] Deuda y memoria** (persona). Deuda: ninguna (descartadas como deuda el minuto compartido
    de la decisión 26 y la bandera parcial de `/sdd-estructura` de la decisión 18; quedan como notas).
    `MEMORY.md`: una entrada, «escritura única». Descartadas: «tabla Pasos en cascada» y «estimado por lote».
    Criterios 3 (sin ticket ni aviso), 4 (columna de pendiente vacía) y 5 (sin deuda que bloquee): cumplen.
28. **[2026-11-22 20:40] Criterio 2: commits confirmados** (persona: «procede»). Cuatro commits locales, sin push:
    kit 0.16.0, ADR 0038, memoria, y specs (0021 cerrada y carpeta 0022 registrada). `Cerrado` queda.

## Pruebas
- [2026-11-22 20:35] Casos y corridas: [07_casos_prueba.md](07_casos_prueba.md) (CP-01 a CP-09 `ok`). Sin verificación manual de UI pendiente.
## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 19:24 | 2026-11-22 19:40 | 16 | 0 | registro de 0021 y 0022 + entrevista (3 preguntas) + `01_spec.md`; planner en el chat, claude-opus-5-5 |
| clarify | 2026-11-22 20:10 | 2026-11-22 20:12 | 2 | 0 | pasada 3 de 3; autochequeo + 3 pasadas (7, 1 y 0 hallazgos); planner y reviewer en el chat, claude-opus-5-5 |
| plan | 2026-11-22 20:14 | 2026-11-22 20:17 | 3 | 0 | `02_analisis_impacto.md` + `03_plan.md`; medición de transcripts `.jsonl`; planner en el chat, claude-opus-5-5 |
| tasks | 2026-11-22 20:19 | 2026-11-22 20:21 | 2 | 0 | `04_tasks.md` (19 pasos) + `07_casos_prueba.md` (9 casos) + aprobación; planner en el chat, claude-opus-5-5 |
| F0.1 | 2026-11-22 20:21 | 2026-11-22 20:24 | 3 | 0 | línea base; `/sdd-estructura` delegado (auxiliar, claude-sonnet-5-5, motivo «ruteo») en paralelo con la ola 1 |
| F1.1, F1.2, F1.3, F1.4 | 2026-11-22 20:22 | 2026-11-22 20:24 | 2 | 0 | lote L1, ola 1: skill `bitacora-uso`; implementer subagente, claude-sonnet-5-5, motivo «otro modelo»; 4 correcciones de la sesión |
| F2.1, F2.2 | 2026-11-22 20:22 | 2026-11-22 20:23 | 1 | 0 | lote L2, ola 1: configuración; implementer subagente, claude-sonnet-5-5, motivo «otro modelo» |
| F3.1, F3.2, F3.3, F3.4 | 2026-11-22 20:22 | 2026-11-22 20:24 | 2 | 0 | lote L3, ola 1: remisiones; implementer subagente, claude-sonnet-5-5, motivo «otro modelo» |
| F4.1, F4.2, F4.3 | 2026-11-22 20:22 | 2026-11-22 20:24 | 2 | 0 | lote L4, ola 1: docs y 0.16.0; implementer subagente, claude-sonnet-5-5, motivo «otro modelo» |
| F5.1, F5.2, F5.3, F5.4 | 2026-11-22 20:24 | 2026-11-22 20:28 | 4 | 0 | réplica, enlaces, ensayo, lint y CP-01 a CP-08; sesión, claude-opus-5-5; `/sdd-estructura skills` delegado (auxiliar, claude-sonnet-5-5, «ruteo»); CP-09 corrido a las 20:28 (dentro de la fila) |
| F6.1 | 2026-11-22 20:26 | 2026-11-22 20:26 | 1 | 0 | manifiesto 0.16.0 y E20 OK; sesión, claude-opus-5-5 |
| F1.2 | 2026-11-22 20:31 | 2026-11-22 20:32 | 1 | 0 | ajuste por la decisión 21 + réplica + manifiesto 0.16.0 y E20; sesión, claude-opus-5-5 |
| validate | 2026-11-22 20:33 | 2026-11-22 20:35 | 2 | 0 | vuelta 1: APROBADO; tester subagente (claude-haiku-5-5, «otro modelo») + reviewer en el chat, claude-opus-5-5 |
| F1.4 | 2026-11-22 20:35 | 2026-11-22 20:35 | 1 | 0 | opcionales de validate (decisión 23) + réplica + manifiesto y E20; sesión, claude-opus-5-5 |
| cierre | 2026-11-22 20:36 | 2026-11-22 20:40 | 4 | 0 | sesión principal (claude-opus-5-5); previa OK; chequeo de cierre E20 OK (56 archivos, 56 filas); tokens completados; ADR 0038 |
## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F0 | 3 min | 3 min | 0 min |
| F1 (lote L1 y ajustes de las decisiones 21 y 23) | 15 min | 4 min | -11 min |
| F2 (lote L2) | 6 min | 1 min | -5 min |
| F3 (lote L3) | 8 min | 2 min | -6 min |
| F4 (lote L4) | 5 min | 2 min | -3 min |
| F5 | 8 min | 4 min | -4 min |
| F6 | 5 min | 1 min | -4 min |
| **Total implementación** | **50 min (≈ 36 por ola)** | **17 min** | **-33 min** |
| Documentación SDD (spec, clarify, plan, tasks, validate, cierre) | sin estimado | 26 min | — |

Las filas de lote cuentan en la fase de su primer paso; los lotes paralelos suman cada uno sus minutos.
Ajuste para el siguiente plan: el real fue un 34 % de la suma de estimados. La ola de 4 implementers
tardó 2 min contra 15 estimados para su fase más larga. Estimar unos 2 min por lote y 4 min por fase de
réplica y chequeos.
