# 07 — Casos de prueba: Bitácora de uso (beta)

Destino: `local`. CP-01 a CP-07 son de solo lectura con `grep` o `ls` (`AGENTS.md` › Comandos:
«Herramienta de apoyo para casos: No aplica»). CP-08 y CP-09 son a mano, en el chat. Escribe: no,
salvo CP-09 (`.agents/local/`, con confirmación de la persona). Ningún caso toca una BD.

## Casos
### CP-01 Skill registrada y con sus secciones
- RF: RF-16
- Pasos: `grep -c -E '^## (Activación|Formato|Registro por tramo|Actividad|Fuente de tokens|Completar tokens|Reporte|Privacidad)$' template/.agents/skills/bitacora-uso/SKILL.md`; `grep -c 'bitacora-uso/SKILL.md' template/AGENTS.md AGENTS.md`
- Resultado esperado: `8`; `1` en cada `AGENTS.md`
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-02 Descripción dentro del tope
- RF: RF-16
- Pasos: contar los caracteres del valor de `description:` del frontmatter de la skill
- Resultado esperado: 1024 o menos; `name: bitacora-uso`
- Herramienta: a mano (`grep`, `wc -m`)
- Escribe: no

### CP-03 Paso de configuración en la lista fija
- RF: RF-1, RF-3
- Pasos: `grep -n '## Bitácora' template/.agents/configuracion.ejemplo.md`; `grep -n 'bitacora' template/.agents/commands/sdd-config.md`
- Resultado esperado: `## Bitácora` dentro de la viñeta de nombres fijos de «Formato del archivo
  local»; «Paso 6. Bitácora de uso (beta)» con sugerido `apagada`; `bitacora` en la lista de `[paso]`
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-04 Remisiones sin copia
- RF: RF-16, RNF
- Pasos: `grep -rn 'bitacora-uso' template/.agents/commands/ template/.agents/skills/sdd/avance-y-tiempos.md template/.agents/configuracion.ejemplo.md`; `grep -rln -E 'message\.id|cache_read_input_tokens|ventana con pausa' template/`
- Resultado esperado: `sdd-config.md`, `sdd-spec.md`, `sdd-cierre.md`, `avance-y-tiempos.md` y
  `configuracion.ejemplo.md` nombran la skill; las reglas de tokens solo aparecen en
  `template/.agents/skills/bitacora-uso/SKILL.md`
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-05 Beta en los cuatro lugares
- RF: RF-17
- Pasos: `grep -n -i 'bitácora.*beta\|beta.*bitácora' README.md INSTALAR.md template/.agents/configuracion.ejemplo.md template/.agents/commands/sdd-config.md`; `sed -n '/^## 0.16.0/,/^## 0.15.0/p' ChangelogAgentKitSdd.md | grep -i beta`
- Resultado esperado: al menos una línea en cada uno de los cuatro (el paso de `/sdd-config` cuenta
  en el comando o en su paso del ejemplo) y en la entrada 0.16.0
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-06 Nombres de herramienta solo en la fuente de tokens
- RF: RNF (kit genérico)
- Pasos: `grep -n -i -E 'cursor|opencode|claude code|jsonl' template/.agents/skills/bitacora-uso/SKILL.md`
- Resultado esperado: todas las líneas dentro de «Fuente de tokens» o «Completar tokens»
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-07 Versión
- RF: RF-17
- Pasos: `cat VersionAgentKitSdd`; `ls manifiestos/0.16.0.md`
- Resultado esperado: `0.16.0`; el manifiesto existe (tras F6.1)
- Herramienta: a mano
- Escribe: no

### CP-08 Completar tokens de un tramo real, sin escribir
- RF: RF-12, RF-13
- Datos de partida: fila `plan` de `0021_BitacoraDeUso/05_avance.md` (2026-11-22 20:14 a 20:17),
  transcripts de este repo
- Pasos: seguir la skill › «Fuente de tokens» y «Completar tokens» para esa ventana; mostrar el
  resultado en el chat sin guardarlo
- Resultado esperado: cuatro cifras (entrada, salida, lectura y escritura de caché) o «no disponible
  (sesiones paralelas)» con las sesiones halladas; el comando solo imprime cifras, ningún texto de
  mensajes
- Herramienta: a mano (chat)
- Escribe: no

### CP-09 Recorrido de punta a punta en una sesión
- RF: RF-1, RF-4, RF-8, RF-10, RF-14
- Datos de partida: kit 0.16.0 en la copia; sin `## Bitácora` en `.agents/local/configuracion.md`
- Pasos:
  1. Pedir «registra la actividad prueba-bitacora»: responde que está apagada y remite a `/sdd-config`.
  2. `/sdd-config bitacora`, elegir `encendida`.
  3. Repetir el paso 1 y hacer una petición corta.
  4. Pedir «reporte de la bitácora de prueba-bitacora».
- Resultado esperado: paso 1 sin archivo nuevo; paso 2 escribe `## Bitácora` con `encendida`; paso 3
  crea `.agents/local/bitacora/actividad-prueba-bitacora.md` con una entrada; paso 4 muestra el
  reporte con totales y los cuatro hallazgos, sin texto de prompts
- Herramienta: a mano (chat)
- Escribe: sí (`.agents/local/`, con confirmación de la persona)

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-22 | obtenido | 8 secciones; 1 fila en cada `AGENTS.md` |
| CP-02 | 2026-11-22 | obtenido | `description` de 587 caracteres; `name: bitacora-uso` |
| CP-03 | 2026-11-22 | obtenido | `## Bitácora` en la lista fija (línea 13); paso 6 con sugerido `apagada`; `bitacora` en `[paso]` |
| CP-04 | 2026-11-22 | obtenido | los 5 archivos nombran la skill; reglas de tokens solo en `bitacora-uso/SKILL.md` |
| CP-05 | 2026-11-22 | obtenido | `README.md:168`, `INSTALAR.md:63`, ejemplo línea 77, `sdd-config.md:13`; 3 líneas «beta» en la entrada 0.16.0 |
| CP-06 | 2026-11-22 | obtenido | líneas 78–80 en «Fuente de tokens» y 89 en «Completar tokens»; ninguna fuera (tras la corrección de la decisión 17) |
| CP-07 | 2026-11-22 | obtenido | `0.16.0`; `manifiestos/0.16.0.md` con 56 filas |
| CP-08 | 2026-11-22 | obtenido | ventana 02:14–02:17:59 UTC; 2 sesiones con mensajes → «no disponible (sesiones paralelas)». Esta sesión: 18 mensajes, entrada 38, salida 19 739, caché leída 1 502 998, caché escrita 91 986; la otra: 2 mensajes. Solo cifras impresas |
| CP-09 | 2026-11-22 | obtenido | 1: apagada sin `## Bitácora`, una línea con `/sdd-config bitacora`, sin archivo; 2: `## Bitácora` encendida en el local; 3: `actividad-prueba-bitacora.md` con 1 entrada (petición = el turno de CP-09); 4: reporte en el chat con totales y 4 hallazgos, tokens completados (1 sesión en la ventana: 8 / 3 296 / 849 189 / 4 735), sin texto de prompts. Observación: el rol «chat» no está en «Modelo por rol» → «sin comparar» |
| CP-01 | 2026-11-22 | ok | validate, vuelta 1: tester (claude-haiku-5-5) obtuvo 8 y 1/1; literal |
| CP-02 | 2026-11-22 | ok | validate, vuelta 1: 587 ≤ 1024 y `name: bitacora-uso`; literal |
| CP-03 | 2026-11-22 | ok | validate, vuelta 1: líneas 13, 77 y 82; `presentación o bitacora`; literal |
| CP-04 | 2026-11-22 | ok | validate, vuelta 1: el tester obtuvo 6 archivos en el primer grep y salida vacía en el segundo; la sesión lo repitió y dio `bitacora-uso/SKILL.md` (`message.id` ×2, «ventana con pausa» ×5). El reviewer lo juzga ok: la salida vacía vino de las comillas al pasarle el comando al tester |
| CP-05 | 2026-11-22 | ok | validate, vuelta 1: `README.md:168`, `INSTALAR.md:63`, ejemplo :77, `sdd-config.md:13`; 3 en 0.16.0 |
| CP-06 | 2026-11-22 | ok | validate, vuelta 1: líneas 80–82 («Fuente de tokens») y 91 («Completar tokens»); juzgado por el reviewer |
| CP-07 | 2026-11-22 | ok | validate, vuelta 1: `0.16.0`; manifiesto con 56 filas |
| CP-08 | 2026-11-22 | ok | validate: la corrida de la sesión sigue la regla de sesiones paralelas; juzgado por el reviewer |
| CP-09 | 2026-11-22 | ok | validate: los 4 pasos dieron lo esperado; juzgado por el reviewer |

## Evidencia formal (opcional)
No aplica.
