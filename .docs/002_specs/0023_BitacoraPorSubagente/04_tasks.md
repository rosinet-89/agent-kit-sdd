# 04 — Tareas: Bitácora por subagente
Leyenda: `[x]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Plan: `03_plan.md` (ADR 0040). Estimado en minutos y por ola. Olas: F0; luego F1, F2 y F3 en paralelo
(archivos disjuntos), sugeridos en dos lotes: L1 = F1.1 a F1.3 (un archivo); L2 = F2.1 y F3.1. Luego
F4; luego F5, en serie. Lint de cada paso: `AGENTS.md` › Comandos (UTF-8, sin BOM, rutas citadas
existentes, `template/` sin marcadores `<<…>>` salvo los dos admitidos). Casos: `07_casos_prueba.md`.

## F0 — Línea base (estimado: 2 min)
- [x] **F0.1 Copia y diff de partida.** —
  - Hecho cuando:
    - [x] Copia en `.agents/local/` de `template/.agents/skills/bitacora-uso/SKILL.md`,
      `template/.agents/skills/sdd/avance-y-tiempos.md`, `VersionAgentKitSdd` y
      `ChangelogAgentKitSdd.md`, justo antes de editar
    - [x] `git diff HEAD` de esos cuatro archivos vacío (sin ediciones ajenas previas); hallazgos en una
      decisión de `05_avance.md`

## F1 — Skill `bitacora-uso` (estimado: 12 min)
- [x] **F1.1 Formato, Registro por tramo y Actividad.** RF-1, RF-2, RF-3, RF-4, RF-13
  - Hecho cuando:
    - [x] Formato: columnas «Sesión» y «Subagente» con su marca «no disponible»; tipos de entrada (en
      el chat, delegada, de coordinación) con remisión a las definiciones de la spec, sin copiarla;
      ejemplo con una ola de 2 delegadas + 1 de coordinación (minutos vacíos, nota «coordinación»)
    - [x] Registro por tramo: la sesión anota el id que devuelve cada lanzamiento; una entrada delegada
      por subagente, en la misma escritura que su fila; una fila con chat y subagentes da una entrada
      por cada uno (reemplaza «una fila, una entrada»; un lote sigue siendo una entrada); una sola
      coordinación por escritura con delegadas, salvo que ya lleve una entrada en el chat con la misma
      ventana
    - [x] Actividad: anota «Sesión», «Subagente» vacío; el turno incluye a sus subagentes
- [x] **F1.2 Fuente de tokens y Completar tokens.** RF-5, RF-6, RF-7, RF-8, RF-9, RF-11, RF-12
  - Hecho cuando:
    - [x] Fuente de tokens: columnas «Id de sesión» y «Transcript del subagente»; Claude Code con
      `CLAUDE_CODE_SESSION_ID` y `<sesión>/subagents/agent-<id>.jsonl`; las demás en «no disponible».
      Lo propio de Claude Code solo en esta tabla
    - [x] Completar: delegada = todo su transcript sin ventana (reanudado: partido por ventana); en el
      chat = su sesión en la ventana, sin subagentes ni otras sesiones; coordinación = su sesión en la
      ventana menos lo ya contado por entradas en el chat; cada `message.id` una vez por spec (chat
      sobre coordinación; entre coordinaciones, la primera)
    - [x] Entrada sin columnas nuevas, o en el chat o de coordinación con «Sesión» «no disponible»:
      regla anterior, sin reescribirla. Sin id o sin transcript: «no disponible», nunca se estima
- [x] **F1.3 Reporte y Privacidad.** RF-10, RNF privacidad
  - Hecho cuando:
    - [x] Reporte: suma directa por entrada; coordinación dentro de `chat` y su total aparte; sus
      minutos fuera de los totales; cuenta como tramo en los hallazgos, marcada «coordinación»
    - [x] Privacidad: se leen también nombres de archivo e ids, nunca contenido
    - [x] Lint del archivo

## F2 — Remisión en `avance-y-tiempos.md` (estimado: 2 min)
- [x] **F2.1 Plural en las dos remisiones.** RF-3
  - Hecho cuando:
    - [x] «Tiempos reales» › Bitácora: «agrega las entradas»; «Pasos con tiempos» › Actualización del
      real: «las entradas de la bitácora». Siguen remitiendo a la skill, sin copiar la regla
    - [x] Lint del archivo

## F3 — Versión (estimado: 3 min)
- [x] **F3.1 Changelog y versión 0.18.0.** —
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` en `0.18.0`; entrada 0.18.0 en `ChangelogAgentKitSdd.md` con las
      columnas nuevas, las entradas delegadas y de coordinación y el ADR 0040 (reemplaza al 0038)

## F4 — Pruebas (estimado: 5 min)
- [x] **F4.1 Casos de la regla.** RF-1 a RF-13, RNF fuente única
  - Hecho cuando:
    - [x] CP-01 a CP-04 de `07_casos_prueba.md` con su resultado en «Corridas»
- [x] **F4.2 Pruebas con transcripts.** RF-5 a RF-9
  - Hecho cuando:
    - [x] CP-05 (retroactiva 0022, ola 1), CP-06 (dos sesiones) y CP-07 (bitácora vieja de 0022) con su
      resultado en «Corridas»; comando de lectura de un solo uso, sin guardar ni imprimir contenido

## F5 — Réplica, chequeos y manifiesto (estimado: 6 min)
- [x] **F5.1 Réplica local.** —
  - Hecho cuando:
    - [x] `cp` de los dos archivos tocados de `template/.agents/` a `.agents/`
    - [x] `diff -rq template/.agents .agents` solo con las diferencias admitidas en `MEMORY.md`
- [x] **F5.2 Ensayo de instalación y lint.** criterios
  - Hecho cuando:
    - [x] Comando Test de `AGENTS.md` › Comandos en `mktemp -d`: los archivos tocados presentes; nada
      pisado; carpeta borrada al terminar
    - [x] Lint en cada archivo tocado
    - [x] `/sdd-estructura` (delegación fija con `auxiliar`) sin `FALLA`
- [x] **F5.3 Manifiesto 0.18.0 y E20.** —
  - Hecho cuando:
    - [x] `manifiestos/0.18.0.md` con las huellas de `template/` tras el último cambio (algoritmo de
      `ACTUALIZAR.md`)
    - [x] E20 de `template/.agents/validar-estructura.md` en verde; si falla, se regeneran las filas
      tocadas, se anota la decisión y se vuelve a correr (`AGENTS.md` › Comandos › Chequeos de cierre)
