# 07 — Casos de prueba: Ruteo de modelos

Destino: `local`. CP-01 a CP-04 y CP-10 son de solo lectura con `grep` (`AGENTS.md` › Comandos:
«Herramienta de apoyo para casos: No aplica»). CP-05 a CP-09 son a mano en la herramienta. Escribe:
no, salvo CP-07 (stage y commit, con confirmación de la persona).

## Casos
### CP-01 Ningún comando con argumentos por posición
- RF: RF-13
- Pasos: `grep -rn '\$[0-9]' template/.agents/commands/`
- Resultado esperado: salida vacía

### CP-02 Tabla de ruteo en una sola fuente
- RF: RF-1, RNF
- Pasos: `grep -rl 'Nivel por tarea sin rol' template/`
- Resultado esperado: los archivos que la citan solo remiten; el encabezado
  `## Nivel por tarea sin rol` está solo en `template/.agents/skills/sdd/restricciones-de-rol.md`
  (`grep -rl '^## Nivel por tarea sin rol' template/`)

### CP-03 Rol `auxiliar` registrado
- RF: RF-7, RF-11
- Pasos: `grep -l 'auxiliar' template/.agents/skills/sdd/restricciones-de-rol.md template/.agents/README.md template/.agents/sync-agents.md template/.agents/configuracion.ejemplo.md`
- Resultado esperado: los cuatro archivos

### CP-04 Delegación fija declarada
- RF: RF-4
- Pasos: `grep -c -E '^(context: fork|agent: auxiliar)$' template/.agents/commands/sdd-status.md template/.agents/commands/sdd-estructura.md`
- Resultado esperado: `2` en cada archivo

### CP-05 `/sdd-status` con carpeta, delegado
- RF: RF-4, RF-10, RF-13
- Datos de partida: sesión de Claude Code de nivel Alto, enlaces al día (F5.2)
- Pasos: `/sdd-status 0020_RuteoDeModelos`
- Resultado esperado: informa la carpeta `0020_RuteoDeModelos`; la línea «quién responde» nombra
  `auxiliar` y su modelo en Contexto; `/model` sin cambio
- Herramienta: a mano

### CP-06 `/sdd-status` sin argumentos
- RF: RF-14
- Pasos: `/sdd-status`
- Resultado esperado: informa el índice; ninguna palabra ni marcador se toma como carpeta
- Herramienta: a mano

### CP-07 Preparación de commit delegada
- RF: RF-5, RF-10
- Pasos: pedir el commit de un módulo de esta spec
- Resultado esperado: la sesión prepara el stage y propone el mensaje (la delegación en `auxiliar` se
  retiró en F6.3, decisión 33), pide confirmación y hace el `git commit`; sin confirmación no hay
  commit y se informa el stage
- Herramienta: a mano
- Escribe: sí (Git, con confirmación)

### CP-08 Alcance de `/sdd-implement` en Claude Code
- RF: RF-13
- Pasos: leer `template/.agents/commands/sdd-implement.md` con los argumentos
  `0020_RuteoDeModelos F2` aplicando «Argumentos de un comando», sin ejecutar el comando
- Resultado esperado: carpeta `0020_RuteoDeModelos`, alcance `F2`
- Herramienta: a mano

### CP-09 OpenCode y Cursor
- RF: RF-4, RF-9, RF-13, RF-14
- Datos de partida: kit 0.15.0 con enlaces de cada herramienta
- Pasos: en cada herramienta, `/sdd-status 0020_RuteoDeModelos` y `/sdd-status`
- Resultado esperado: el comando existe (no se descarta por su frontmatter); toma bien la carpeta o
  el índice; corre delegado o en el chat (respaldo) sin error
- Herramienta: a mano, persona (decisión 16)

### CP-10 Renglón de ruteo en el archivo siempre cargado
- RF: RF-6
- Pasos: `grep -c 'Nivel por tarea sin rol' template/AGENTS.md AGENTS.md`
- Resultado esperado: `1` en cada archivo

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-22 18:25 | ok | `grep -rn '\$[0-9]' template/.agents/commands/` vacío |
| CP-02 | 2026-11-22 18:25 | ok | `## Nivel por tarea sin rol` solo en `restricciones-de-rol.md`; los otros 8 archivos remiten; filas de la tabla solo ahí |
| CP-03 | 2026-11-22 18:25 | ok | los cuatro archivos |
| CP-04 | 2026-11-22 18:25 | ok | `2` en `sdd-status.md` y `sdd-estructura.md` |
| CP-05 | 2026-11-22 18:25 | bloqueado | carpeta bien tomada; corrió como `general-purpose` (`claude-opus-5-5`), no `auxiliar`: repetir en una sesión nueva (decisión 27) |
| CP-05 | 2026-11-22 18:32 | ok | repetido con `auxiliar` cargado: corrió como `auxiliar` en `claude-sonnet-5-5`, tomó la carpeta y devolvió al chat la pregunta del hueco |
| CP-06 | 2026-11-22 18:25 | ok (RF-14) | informa el índice; `$ARGUMENTS` vacío, nada tomado como carpeta. Aparte: corrió en `claude-opus-5-5` como `general-purpose`, no `auxiliar` (decisión 27) |
| CP-08 | 2026-11-22 18:25 | ok | carpeta `0020_RuteoDeModelos`, alcance `F2` |
| CP-10 | 2026-11-22 18:25 | ok | `1` en `template/AGENTS.md` y en `AGENTS.md` |
| CP-02 | 2026-11-22 18:46 | ok | repetido tras F6.3: el encabezado solo en `restricciones-de-rol.md`; remiten 7 archivos (`git-commit/SKILL.md` ya no la cita) |
| CP-07 | 2026-11-22 18:44 | manual | stage de 25 archivos y mensaje preparados en el chat y en `auxiliar`, sin commit, stage sacado (decisión 32); el commit con confirmación va en el cierre |
| CP-10 | 2026-11-22 18:46 | ok | repetido tras F6.3: `1` en cada archivo |
| CP-01 | 2026-11-22 19:03 | ok | validate: `tester` (claude-haiku-5-5) obtuvo salida vacía; comparación literal |
| CP-02 | 2026-11-22 19:03 | ok | validate: `tester` obtuvo 8 archivos y el encabezado solo en `restricciones-de-rol.md`; el reviewer revisó los 7 restantes: solo remiten, ninguna fila de la tabla fuera de su fuente |
| CP-03 | 2026-11-22 19:03 | ok | validate: `tester` obtuvo los cuatro archivos; literal |
| CP-04 | 2026-11-22 19:03 | ok | validate: `tester` obtuvo `2` en cada archivo; literal |
| CP-10 | 2026-11-22 19:03 | ok | validate: `tester` obtuvo `1` en cada archivo; literal |
| CP-09 | 2026-11-22 19:08 | manual | pendiente: fuera del alcance de 0.15.0; la persona lo corre en la semana (decisión 35) |
| CP-09 | 2026-11-22 19:11 | ok (OpenCode) | la persona, capturas de OpenCode (chat `deepseek-v4.1-flash-free`): `/sdd-status` sin argumentos corrió en `auxiliar` (`opencode/big-pickle`), «el argumento llegó vacío», informó el índice (16 s, 34.8k leídos); `/sdd-status 0020_RuteoDeModelos` corrió en `auxiliar`, tomó la carpeta e informó etapa, pasos, retomo y tiempos (1 min 11 s, 163k leídos). Ambos comandos aparecieron (frontmatter aceptado). Contexto de «quién responde» con rol y modelo (RF-10). Cursor: pendiente |
| CP-07 | 2026-11-22 19:17 | ok | cierre: la sesión preparó stage y mensajes, pidió confirmación y, confirmada, hizo los commits (`b1ba9ac` kit, ADR y el de esta spec); `git add` explícito, secretos sin hallazgos |

## Evidencia formal (opcional)
No aplica.
