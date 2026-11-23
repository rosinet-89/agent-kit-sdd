# 07 — Casos de prueba: Bitácora por subagente

Destino: `local`. Todos los casos son de solo lectura, a mano (`AGENTS.md` › Comandos: «Herramienta de
apoyo para casos: No aplica»). CP-05 a CP-07 leen transcripts con un comando de un solo uso que solo
toma `timestamp`, `message.id`, `message.model` y `message.usage`, nombres de archivo e ids; no se
guarda, no se versiona y no imprime contenido. Escribe: no. Ningún caso toca una BD.

## Casos
### CP-01 Columnas y tipos de entrada en Formato
- RF: RF-1, RF-4
- Datos de partida: F1 hecha
- Destino: `local`
- Pasos: `awk '/^## Formato/,/^## Registro por tramo/' template/.agents/skills/bitacora-uso/SKILL.md | grep -n -E 'Sesión|Subagente|coordinación'`
- Resultado esperado: el encabezado de la tabla lleva «Sesión» y «Subagente»; el ejemplo trae dos
  entradas delegadas con id y una de coordinación con minutos vacíos
- Herramienta: a mano (`awk`, `grep`)
- Escribe: no

### CP-02 Registro con varias entradas por fila
- RF: RF-2, RF-3, RF-4, RF-13
- Datos de partida: F1 hecha
- Destino: `local`
- Pasos: leer `## Registro por tramo` y `## Actividad` de `template/.agents/skills/bitacora-uso/SKILL.md`
- Resultado esperado: id anotado al recibir el lanzamiento; una entrada delegada por subagente en la
  misma escritura; fila con chat y subagentes = varias entradas; lote = una entrada; una coordinación
  por escritura, salvo entrada en el chat con la misma ventana; ya no dice «una fila de Tiempos, una
  entrada»; actividad con «Sesión» y subagentes del turno incluidos
- Herramienta: a mano
- Escribe: no

### CP-03 Fuente de tokens por herramienta
- RF: RF-12, RNF kit genérico
- Datos de partida: F1 hecha
- Destino: `local`
- Pasos: `grep -rn -E 'CLAUDE_CODE_SESSION_ID|subagents/agent-' template/`
- Resultado esperado: solo aparece en la tabla «Fuente de tokens» de la skill; Cursor, OpenCode y
  otra herramienta en «no disponible» en las columnas nuevas
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-04 Una sola fuente de la regla
- RF: RNF «una regla, una fuente», RF-3
- Datos de partida: F1 y F2 hechas
- Destino: `local`
- Pasos: `grep -rn -E 'Registro por tramo|Completar tokens|coordinación' template/.agents template/AGENTS.md | grep -v 'skills/bitacora-uso/'`
- Resultado esperado: solo remisiones a la skill; `avance-y-tiempos.md` dice «las entradas» en sus dos
  remisiones
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-05 Retroactiva 0022, ola 1
- RF: RF-5, RF-7, RF-8, criterio 2 de la spec
- Datos de partida: transcripts de la sesión `0efc4732…` del proyecto; ids de L1 a L5 de la ola de
  21:03 (`02_analisis_impacto.md` › §3)
- Destino: `local`
- Pasos:
  1. Sumar los cuatro tokens de cada `subagents/agent-<id>.jsonl` de L1 a L5, sin ventana, cada
     `message.id` una vez.
  2. Sumar los de la sesión principal de 21:03 a 21:06 (hora local, ventana de delegación), sin
     subagentes ni los `message.id` que ya cuentan F0.1 (21:02–21:03) y F6.4 (21:06).
  3. Comparar con la regla anterior: 5 × (sesión + subagentes en la ventana).
- Resultado esperado: L1 a L5 entre 45 345 y 140 814, iguales a §3; coordinación 1 033 088; suma de las
  6 ≈ 1,42 M, menor que 15,9 M
- Herramienta: a mano (comando de un solo uso)
- Escribe: no

### CP-06 Dos sesiones en la ventana
- RF: RF-6, RF-9
- Datos de partida: una ventana de la 0022 con mensajes de dos sesiones del proyecto (las dos
  entradas que quedaron «no disponible (sesiones paralelas)»)
- Destino: `local`
- Pasos: sumar, con la regla nueva, solo los mensajes de la sesión que escribió la fila en esa ventana
- Resultado esperado: tokens de esa sesión, sin los de la otra; sin «no disponible (sesiones
  paralelas)»
- Herramienta: a mano (comando de un solo uso)
- Escribe: no

### CP-07 Bitácora vieja con la regla anterior
- RF: RF-9
- Datos de partida: `.agents/local/bitacora/0022_PlanContraRealEnVivo.md` (sin columnas «Sesión» ni
  «Subagente»)
- Destino: `local`
- Pasos: completar sus entradas con la regla de la skill nueva, sin escribir el archivo, y comparar
  con los tokens que ya tiene
- Resultado esperado: mismos tokens y mismas marcas; el archivo no cambia (`git` no lo ve; comparar
  su suma antes y después)
- Herramienta: a mano (comando de un solo uso)
- Escribe: no

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-22 | obtenido | 5 coincidencias en Formato: columnas «Sesión» y «Subagente», tipos de entrada; ejemplo con 2 delegadas (`a-01`, `a-02`) y 1 coordinación con minutos vacíos |
| CP-02 | 2026-11-22 | obtenido | Registro y Actividad con las reglas listadas; «Un lote es una entrada, como es una fila» ya no está (0) |
| CP-03 | 2026-11-22 | obtenido | Solo `template/.agents/skills/bitacora-uso/SKILL.md:96` (tabla «Fuente de tokens»); Cursor, OpenCode y otra en «no disponible» |
| CP-04 | 2026-11-22 | obtenido | Fuera de la skill, el grep no da ninguna línea; `avance-y-tiempos.md` con «las entradas» 2 veces |
| CP-05 | 2026-11-22 | obtenido | L1 79 084, L2 140 814, L3 52 190, L4 67 400, L5 45 345 (= §3). Coordinación: 21:03–21:05 da 1 822 349 (suma 6 = 2 207 182); con la ventana de delegación de la regla (21:03–21:06, Fin de las filas) y la resta de las entradas en el chat F0.1 (21:02–21:03) y F6.4 (21:06), RF-7 da 1 252 499 (suma 6 = 1 636 832). Regla anterior: 3 182 217 × 5 = 15 911 085 |
| CP-06 | 2026-11-22 | obtenido | 21:07–21:09: otra sesión (`dc9870c7…`) con 52 754. Regla nueva: sesión `0efc4732…` sola 1 195 000 (sin subagentes); delegadas auxiliar 755 664 y L7 120 871. Sin «no disponible (sesiones paralelas)» |
| CP-07 | 2026-11-22 | obtenido | Regla anterior recalculada: 8 de 12 entradas iguales; difieren 21:06 (975 035 contra 1 194 446), 21:06–21:07 (2 085 253 contra 2 304 664), 21:10 (2 343 629 contra 2 396 383) y 21:12 (1 249 312 contra 1 374 209). La regla anterior sigue igual en la skill; la diferencia sale de los comandos de un solo uso. 21:07 sigue con dos sesiones. Archivo sin cambio |
| CP-01 | 2026-11-22 21:54 | ok | validate v1, tester claude-haiku-5-5: encabezado con «Sesión» y «Subagente», tipos de entrada y coordinación con minutos vacíos en el ejemplo (reviewer) |
| CP-02 | 2026-11-22 21:54 | ok | validate v1: «Registro por tramo» con id al recibir el lanzamiento, una delegada por subagente, varias entradas por fila, lote = una entrada, una coordinación salvo chat con la misma ventana; las dos frases viejas en 0 (reviewer) |
| CP-03 | 2026-11-22 21:54 | ok | validate v1: solo `SKILL.md:96`, tabla «Fuente de tokens»; Cursor, OpenCode y otra en «no disponible» (reviewer) |
| CP-04 | 2026-11-22 21:54 | ok | validate v1: sin líneas fuera de la skill; «las entradas» 2 (literal) |
| CP-05 | 2026-11-22 21:54 | ok | validate v1: L1 a L5 iguales a §3 (79 084, 140 814, 52 190, 67 400, 45 345); 21:03–21:05 = 1 822 349. Con la regla (ventana 21:03–21:06 menos los mensajes de F0.1 y F6.4, por `message.id`) la coordinación da 1 033 088; suma de las 6 = 1 418 921 < 15 911 085 (regla anterior 3 182 217 × 5). Decisión 18 (reviewer) |
| CP-06 | 2026-11-22 21:54 | ok | validate v1: 21:07–21:09 sesión `0efc4732…` sola 1 426 645 y `dc9870c7…` sola 52 754, cada una por su id; auxiliar 755 664 y L7 120 871 por transcript; sin «no disponible (sesiones paralelas)» (reviewer) |
| CP-07 | 2026-11-22 21:54 | ok | validate v1: con un solo comando, 13 de 13 ventanas iguales a lo guardado en la bitácora de 0022 (cierre en la sesión `c427763f…`, buscada por el reviewer); 21:07–21:09 con dos sesiones = «no disponible (sesiones paralelas)», igual. md5 igual antes y después. Decisión 19 (reviewer) |
