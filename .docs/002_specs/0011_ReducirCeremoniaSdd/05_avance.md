# 05 — Avance: Reducir la ceremonia del flujo SDD
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-11-03 23:04 |
| Fase actual | **Cerrada** (`Estado: implementada`). F0 a F9 hechas; sin commit |
| Cierre (23:04) | ADR 0023 aceptado; versión 0.7.0, changelog y `manifiestos/0.7.0.md` (44 filas, 0 diferencias). Observación 2 de F8 corregida (decisión 18); 1, 3, 4 y 5 diferidas. Altas de deuda y entradas de `MEMORY.md` **propuestas, sin crear** (decisión 18). Árbol con 23 rutas modificadas o nuevas, **sin commit**: los commits los hace Claudio con confirmación explícita |
| Último paso cerrado | F5.3 (22:51): Corlos editó `reviewer.md`, `coordinator.md`, `AGENTS.md` (plantilla y raíz) y los dos README de specs; Claudio revisó diff y lint, todo OK. Antes, F3.7 (22:47): Corlos editó los 7 comandos; Claudio revisó el diff, lint OK, y corrigió `sdd-tasks.md` («`03_plan.md` solo en modo completo»). Antes, F2.3 (22:38). `04_tasks.md` aprobada por el usuario (decisión 16). F0 a F2 hechas por Claudio: `topes-y-modos.md` creada en `template/` y editadas `SKILL.md`, `plantillas.md` y `avance-y-tiempos.md`; lint OK. Sin réplica a `.agents/` todavía (F6) |
| Siguiente paso | F6 (réplicas a `.agents/`) y F7 (verificación y medición) los hace Claudio; F8 (`/sdd-validate`, solo lectura) puede ir a Corlos como subagente en segundo plano; F9 Claudio |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | Ninguno. Solo documentos de esta carpeta (`01` a `05`) y la fila 0011 del índice de specs |
| Traspaso (Claudio → siguiente agente) | Ver el bloque «Traspaso» de abajo |

### Traspaso (leer antes de actuar)
1. **Estado de la spec:** `01_spec.md` aprobada (2026-11-03 22:14, 19 RF, clarify en 2 pasadas de 3). Plan `02` y `03` aprobado (22:22). `04_tasks.md` pendiente de aprobación del usuario. El usuario pidió este punto de retomo porque cambia de agente; no dijo qué decidir sobre las tareas.
2. **Decisiones del usuario ya tomadas:** están en «Decisiones» (2 a 6 sobre la spec; 9 a 15 sobre el plan). Resumen: tope de 3 pasadas (cada revisión del reviewer); a tope, alcance y seguridad pasan a `[ACLARAR]` (hasta 3) y decide el usuario; marcador único `[ACLARAR: …]`; modo ligero con 8 condiciones, umbrales 3 archivos y 1 hora fijos y provisionales (revisar tras 3 usos reales); promoción a completo cuenta la pasada hecha como pasada 1; bloqueo por marcadores en ligero lo aplica `/sdd-tasks`; pieza nueva `skills/sdd/topes-y-modos.md`; ADR único «Ceremonia proporcional» (0023) al cierre; versión 0.7.0 y manifiesto `manifiestos/0.7.0.md` como última fase; revisión cruzada (RF-16) entra en 0.7.0 (15 min).
3. **Aviso importante:** ninguna de las 10 specs históricas (001 a 0010) habría calificado para el modo ligero. El ahorro comprobable está en el tope de pasadas (M1, estimación en bytes leídos: 0010 con 7 pasadas = 155 897 B frente a ≤66 813 B con el tope de 3, −57 %, no son tokens reales). El modo ligero queda como apuesta hasta su primer uso real.
4. **Estado del repo al cerrar esta sesión:** rama `main`, 6 commits locales SIN `push` sobre `origin/main` (`afd19b7`, `50ba537`, `22a643e`, `9432b27`, `2420393`, `37af01d`: las specs 0008, 0009 y 0010 cerradas, versión 0.6.0). Árbol de trabajo: solo esta carpeta `0011_ReducirCeremoniaSdd/` (sin seguimiento) y la fila 0011 del índice `.docs/002_specs/README.md` (modificada, sin commit). Nada más pendiente de commit.
5. **Pendiente que NO es de esta spec y espera decisión del usuario:** (a) `push` de los 6 commits (nunca lo hace un agente; el repo es público); (b) cerrar los issues #1, #2 y #3 de `rosinet-89/agent-kit-sdd` cuando suba los commits; (c) 0009 quedó con el riesgo aceptado de no haberse probado `/sdd-enlaces` en una sesión real; (d) propuesta de deuda sin crear: `/sdd-spec` y `/sdd-clarify` dejaron el argumento sin sustituir (`Reducir` y `$1` literales) en este arnés; sin verificar si es del kit o del entorno (decisión 8).
6. **Reglas de trabajo que se mantuvieron en esta sesión:** ningún commit ni `push` sin confirmación explícita; `git add` explícito; el implementer nunca ejecuta SQL ni escribe `05_avance.md` (lo escribe la sesión principal); `command grep` porque `grep` es un shim; `.agents/` de la raíz está fuera de Git y se comprueba con `diff -rq template/.agents .agents` (solo admite `napkin.md` y `skills/caveman`); los ensayos solo en `mktemp -d`; los estimados Markdown se hacen en minutos y el real ronda un quinto.
7. **Antes de F9:** verificar el número libre del ADR con `ls .docs/004_adr` y `git ls-tree --name-only @{u} .docs/004_adr/` (hoy el local llega a 0022 y el remoto a 0018; esperado 0023). El manifiesto 0.7.0 se genera después de la ÚLTIMA edición de `template/`; si F8 corrige algo, repetir réplicas, `diff -rq` y huellas.
8. **Memoria de Claudio (no versionada):** `reducir-tokens-spec-pendiente` en `~/.claude/projects/-home-cromero-dev-agent-kit-sdd/memory/` anota que esta spec era la pendiente; al cerrarla se debe actualizar o borrar esa nota.

## Pasos
- [x] spec (borrador; 19 RF; sin dudas abiertas)

## Decisiones
1. **[2026-11-03 21:51] Se abre esta spec a partir del análisis de github/spec-kit pedido por el usuario.** Alcance: reducir tokens en el proceso (pasadas de clarify y ceremonia de 6 documentos), no en las lecturas, que ya atendió `003_OptimizarArnes`. Versión objetivo 0.7.0 (0.6.0 ya es el bump de 0008, 0009 y 0010). El nombre de carpeta es `0011_ReducirCeremoniaSdd`; el comando `/sdd-spec` tomó `Reducir` como carpeta por un fallo al separar el argumento, y se corrigió. Alternativas descartadas de spec-kit: scripts `.sh` y `.ps1` (ADR 004), generar archivos por herramienta en vez de enlaces (ADR 009), ISO 8859-1, CLI en Python y las 50 integraciones.

2. **[2026-11-03 22:10] Tope de clarify alcanzado con hallazgos pendientes: opción (a).** Alcance y seguridad pasan a marcadores `[ACLARAR]` (hasta 3, por impacto), el resto a supuestos explícitos, y decide el usuario. Alternativas descartadas: (b) todo a supuestos (puede dejar sin resolver algo de seguridad) y (c) una pasada extra (riesgo de volver a la espiral de 7 pasadas de 0010).
3. **[2026-11-03 22:10] Un cambio que se distribuye no queda excluido del modo ligero.** El bump y el changelog van como tarea de `04_tasks`. Alternativa descartada: excluirlo (casi todo el trabajo del kit se distribuye y vaciaría el modo).
4. **[2026-11-03 22:10] Se unifica el marcador en `[ACLARAR: …]`.** El tope de 3 aplica solo a los que bloquean el plan. Cambia la plantilla de `plantillas.md` y las piezas que la citan. Alternativa descartada: mantener `[NECESITA ACLARACIÓN]` junto al nuevo (dos marcadores que explicar).
5. **[2026-11-03 22:10] Una pasada de clarify es cada revisión del reviewer.** Los ajustes del planner entre revisiones no cuentan. Alternativa descartada: contar el ciclo reviewer + planner.
6. **[2026-11-03 22:10] Sin meta numérica de ahorro en la spec; la revisión cruzada (modo analyze del reviewer) entra en 0.7.0 solo si el plan la estima en menos de 30 min.** Si no, pasa a deuda con visto bueno. El ahorro se mide en el plan, en bytes con caso de referencia, y se declara el límite de la métrica. Alternativas descartadas: meta fija de 30 % y analyze obligatorio, o analyze fuera de 0.7.0.

7. **[2026-11-03 22:14] Clarify cerrado en 2 pasadas de 3 y spec aprobada.** Pasada 1: CAMBIOS NECESARIOS (5 bloqueantes, 7 menores); el usuario aprobó corregir los 5 bloqueantes con las recomendaciones y dejar los 7 menores en «Pendiente para el plan». Pasada 2: APROBABLE, sin hallazgos nuevos bloqueantes. Referencia: 0010 necesitó 7 pasadas. Alternativa descartada: aprobar tras la pasada 1 dejando los bloqueantes al plan (los habría heredado el plan y quedaban sin criterio comprobable el tope y la elegibilidad al modo ligero).
8. **[2026-11-03 22:14] Observación para una deuda (sin crear, falta visto bueno):** `/sdd-spec` y `/sdd-clarify` no sustituyeron el argumento en este arnés (dejaron `Reducir` y `$1` literales). Sin verificar si es del kit o del entorno.

9. **[2026-11-03 22:22] Umbrales del modo ligero: se dejan en 3 archivos y 1 hora.** El historial (001 a 0010) no contradice ni respalda el umbral: ninguna spec habría sido elegible (mínimo 9 archivos y 85 min), y subirlo a 5 u 8 archivos no cambiaría ninguna decisión. Se registra cada uso ligero en `05_avance.md` y se revisan los umbrales tras 3 usos reales. Alternativa descartada: subirlos ahora con `/sdd-change`.
10. **[2026-11-03 22:22] El bloqueo por marcadores `[ACLARAR]` pendientes en modo ligero lo aplica `/sdd-tasks`** (no hay `03_plan.md`). Alternativa descartada: otra fase.
11. **[2026-11-03 22:22] Al promover un cambio ligero a completo, la pasada de clarify ya hecha cuenta como pasada 1.** Al completo le quedan 2. Alternativa descartada: reiniciar a 0 (se paga dos veces el mismo trabajo).
12. **[2026-11-03 22:22] Proyectos instalados con `AGENTS.md` viejo (grupo B): basta la nota del changelog y de `ACTUALIZAR.md`** («incorpora la línea antes de usar el modo ligero»). Alternativa descartada: que el arranque lo detecte y avise (más tokens por sesión, contradice el objetivo).
13. **[2026-11-03 22:22] ADR único «Ceremonia proporcional» (número 0023, el siguiente libre al cierre)** con el modo ligero y la promoción más el tope de 3 pasadas y el marcador único. Se crea al cerrar con la skill `adr`. Alternativas descartadas: dos ADR separados; ninguno.
14. **[2026-11-03 22:22] Pieza nueva `skills/sdd/topes-y-modos.md` con las reglas, en lugar de ampliar `SKILL.md`.** Límites: spec corta de hasta 5 000 B y 8 RF con secciones omitidas; `04_tasks.md` con el plan dentro de hasta 8 000 B. Los comandos la enlazan en una frase. Alternativa descartada: ampliar `SKILL.md` (todos los comandos que cargan la skill leerían ~3,5 KB de más).
15. **[2026-11-03 22:22] Medición: M1 (pasadas) es estimación en bytes leídos, no tokens reales.** 0010 con 7 pasadas = 155 897 B; con el tope de 3, ≤66 813 B (−57 %); 0011 con 2 pasadas = 44 232 B. M2 (documentos) es hipotética hasta el primer cambio ligero real. La revisión cruzada (RF-16) se estima en 15 min y entra en 0.7.0.
16. **[2026-11-03 22:30] `04_tasks.md` aprobada con reparto.** Claudio hace F0 a F2 (la pieza `topes-y-modos.md` define lo demás); Corlos (`cursor-agent`) hace F3 a F5; Claudio revisa diff y hace réplicas, medición, validación, cierre y commits. Corlos no hace commit. Alternativa descartada: delegar también F1 y F2. Observación F0.2: la nota de documentos por feature en `AGENTS.md` no dice «seis documentos» literal; está en la línea 104 de `template/AGENTS.md` y 105 de `AGENTS.md` (sección «Skills de proyecto» › Notas) y se editará en F5.
17. **[2026-11-03 23:01] F8 aprobada; 5 observaciones opcionales del reviewer sin decidir.** (1) `reviewer.md:17`: «una sola pasada en ligero» va dentro del bullet de revisión cruzada; (2) `SKILL.md:98` aún lista usuarios, historias, RNF y fuera de alcance como contenido mínimo de `01_spec.md`; (3) `plantillas.md` repite los límites de 5 000 B, 8 RF y 8 000 B que ya están en `topes-y-modos.md` (lo piden F1.1 y F2.2); (4) la fila 0011 del índice de specs aún dice «falta aprobar `04_tasks`» (la actualiza `/sdd-cierre`); (5) `sdd-flujo.md` y `planner.md` siguen orientados a `03` y `06`, sin tocar por plan. Pendiente: el usuario decide si se corrigen (1), (2) o (3) con Edit mínimo antes de F9 o se difieren.
18. **[2026-11-03 23:04] Cierre.** El usuario aprobó corregir la observación 2 y seguir con F9. Se corrigió `SKILL.md:98` (en ligero la spec es corta) y se replicó. F9 la hicieron tres subagentes en paralelo con archivos disjuntos (ADR, versión y changelog, manifiesto) y Claudio verificó cada resultado. Observaciones 1, 3, 4 y 5 de F8: sin corregir; la 4 se resolvió con este cierre. **Propuestas sin crear, a decisión del usuario:** (a) deuda: `reviewer.md:17` mezcla «una sola pasada en ligero» con el bullet de revisión cruzada; (b) deuda: `sdd-flujo.md` y `planner.md` siguen orientados a `03` y `06` aunque en ligero no existen; (c) deuda: `/sdd-spec` y `/sdd-clarify` dejaron el argumento sin sustituir en este arnés (decisión 8; sin verificar si es del kit o del entorno); (d) deuda: revisar los umbrales de 3 archivos y 1 hora tras 3 usos ligeros reales; (e) `MEMORY.md`: «el modo ligero es una apuesta: ninguna spec 001 a 0010 habría sido elegible; el ahorro comprobado es el tope de pasadas». Descartada la propuesta (f) sobre `autoMode.allow` para `cursor-agent --force`: el usuario dijo que ya se atendió (configuración de sus `settings.json`, 2026-11-03).

## Pruebas
- **F8.1 (23:01), pasada 1 de 3, por Corlos como reviewer, solo lectura, lanzado como subagente en segundo plano sin `--force`:** `VEREDICTO: APROBADO`. RF-1 a RF-17 y RF-19 cubiertos con `archivo:línea`; RF-18 pendiente de F9. Fuente única confirmada (`3 pasadas` solo en `topes-y-modos.md`, la prioridad y las 8 condiciones solo ahí). Lint OK en los 17 archivos. Criterio 8 (specs 001 a 0010) sin cambios. `git status` posterior coincide con el anterior: Corlos no escribió nada. El ensayo de instalación no se repitió en esa pasada (consta en F7.2).
- F0 a F2 (22:38): `file -i` utf-8 y sin BOM en `topes-y-modos.md`, `SKILL.md`, `plantillas.md`, `avance-y-tiempos.md`; `NECESITA` da 0 en `SKILL.md` y `plantillas.md`; «3 pasadas» solo en `topes-y-modos.md`; sin marcadores `<<…>>`; `name: sdd` en `SKILL.md`. `topes-y-modos.md` pesa 5 008 B, `SKILL.md` 9 826 B (+325 B).
- F4 y F5 (22:51): `reviewer.md` conserva `tools: Read, Glob, Grep, Bash`; `ls template/.agents/commands | wc -l` = 15; `implementer.md` y `sdd-implement.md` sin cambios; lint OK en los 6 archivos.
- F6 (22:55): 13 réplicas copiadas; `diff -rq template/.agents .agents` solo `napkin.md` y `skills/caveman`; lint OK en las réplicas.
- F7.1: `NECESITA ACLARACI` = 0 en `template`, `.agents` y `AGENTS.md`; `ACLARAR` en `template` solo en `plantillas.md` (forma), `topes-y-modos.md` (regla) y los enlaces de `sdd-plan.md` y `sdd-tasks.md`; «3 pasadas» solo en `topes-y-modos.md`.
- F7.2: ensayo en `mktemp -d` copia 44 archivos; con un `AGENTS.md` propio en el destino, no se pisa. F7.3 y F7.4: `git diff --stat` vacío para `implementer.md`, `sdd-implement.md` y `.docs/002_specs/00*`. F7.6: la tabla de calibración existe en `02` § 3.1.
- **F7.5 Medición (RF-17).** Métrica: bytes que se leen, medidos con `wc -c` sobre los tamaños nuevos; **no son tokens reales**. Pasadas y documentos se informan por separado.
  - **M1, pasadas:** bytes por pasada de clarify = spec + constitución (1 133) + `sdd-clarify.md` (1 664) + `reviewer.md` (2 689). Con la spec de 0010 (17 257): 22 743 B por pasada. 0010 con 7 pasadas = 159 201 B; con el tope de 3 = 68 229 B (**−57 %**, 4 pasadas menos). 0011 con 2 pasadas = 45 176 B (spec de 17 102 B). El plan estimó 155 897 B con los tamaños de antes de editar `sdd-clarify.md` y `reviewer.md`.
  - **M2, documentos (hipotética):** 006 completa, `01` a `05` = 34 031 B (06 no existe). Conjunto ligero con sus límites: spec ≤ 5 000 B + `04` con plan ≤ 8 000 B + `05` real (7 339 B) = ≤ 20 339 B, es decir **al menos −40 %** (−51 % sin contar `05`). Es una cota: se confirma con el primer ligero real. Ninguna de las specs 001 a 0010 habría sido elegible para ligero (`02` § 3.1).
  - **Límite declarado:** son lecturas contadas, no respuestas ni tokens generados.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-03 21:51 | 2026-11-03 22:11 | — | — | Incluye la espera de las respuestas del usuario a las 5 preguntas; los minutos activos no se midieron por separado |
| clarify | 2026-11-03 22:11 | 2026-11-03 22:14 | — | — | 2 pasadas de 3 (pasada 1: 5 bloqueantes y 7 menores; pasada 2: aprobable). Incluye la espera de tus decisiones; los minutos activos no se midieron por separado |

| plan | 2026-11-03 22:15 | 2026-11-03 22:22 | — | — | Planner (análisis con datos medidos y plan de 10 fases). Incluye la espera de tus decisiones; los minutos activos no se midieron por separado |
| tasks | 2026-11-03 22:22 | 2026-11-03 22:25 | — | — | Planner (40 pasos, 173 min). Sin aprobar: el usuario pidió punto de retomo y cambia de agente |
| F0 | 2026-11-03 22:30 | 2026-11-03 22:32 | 2 | 0 | Claudio; estado de partida, grep, specs previas todas implementadas |
| F1 | 2026-11-03 22:32 | 2026-11-03 22:36 | 4 | 0 | Claudio; `topes-y-modos.md` y `SKILL.md` |
| F2 | 2026-11-03 22:36 | 2026-11-03 22:38 | 2 | 0 | Claudio; `plantillas.md` y `avance-y-tiempos.md`, con lint |
| F3 | 2026-11-03 22:38 | 2026-11-03 22:47 | 9 | 0 | Corlos (`cursor-agent --force`) editó los 7 comandos; Claudio revisó diff y lint y corrigió `sdd-tasks.md`. Incluye dos intentos bloqueados por el clasificador del modo automático y el cambio de modo de permisos (el tiempo de espera del usuario no está descontado: no se midió) |
| F4 y F5 | 2026-11-03 22:47 | 2026-11-03 22:51 | 4 | 0 | Corlos (`cursor-agent --force`) editó 2 agentes, 2 `AGENTS.md` y 2 README; Claudio revisó diff y lint (sin correcciones) |
| F6 y F7 | 2026-11-03 22:51 | 2026-11-03 22:56 | 5 | 0 | Claudio: 13 réplicas, chequeos, ensayo de instalación y medición M1/M2 |
| F8 | 2026-11-03 22:56 | 2026-11-03 23:01 | 5 | 0 | Corlos (reviewer, solo lectura) en subagente de segundo plano; pasada 1 de 3, APROBADO, sin correcciones (F8.2 sin cambios) |
| F9 | 2026-11-03 23:01 | 2026-11-03 23:04 | 3 | 0 | Tres subagentes en paralelo (ADR 0023, versión y changelog, manifiesto); Claudio verificó (44 huellas, 0 diferencias) y cerró la spec. El tiempo de espera del usuario no está descontado |

## Plan contra real
Estimado del plan (`03_plan.md` § 7, 163 min y 173 min en `04_tasks.md`) frente a lo medido. Los tiempos de F0 a F9 son minutos de reloj entre filas, no trabajo medido por separado; las esperas del usuario no se descontaron.

| Fase | Estimado (min) | Real (min) | Delta |
|---|---|---|---|
| F0 | 5 | 2 | −3 |
| F1 | 20 | 4 | −16 |
| F2 | 15 | 2 | −13 |
| F3 | 28 | 9 | −19 |
| F4 y F5 | 25 | 4 | −21 |
| F6 y F7 | 35 | 5 | −30 |
| F8 | 15 | 5 | −10 |
| F9 | 30 (20 en el plan) | 3 | −27 |
| Documentación SDD (`spec` a `tasks`) | sin estimado | sin medir (esperas incluidas) | — |

Ajuste para el siguiente plan: el real de edición Markdown vuelve a ser ~un quinto del estimado (0008, 0010, esta spec); estimar por la espera de decisiones del usuario y por los bloqueos de permisos, no por escribir. Los tiempos de F0 a F2 los reconstruí al cerrar F2 con una sola lectura de `date`: son aproximados.
