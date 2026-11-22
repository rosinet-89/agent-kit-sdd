# 07 — Casos de prueba: Arranque ligero de sesión

Todos los casos corren en un temporal (`tmp=$(mktemp -d)`) o sobre el propio clon en solo lectura.
Herramienta de apoyo: «No aplica» (`AGENTS.md` › Comandos). «Antes» = commit `8c9681b`; «después» =
árbol de trabajo tras F3. Los casos del chequeo (CP-04 a CP-08) los corre la sesión siguiendo, al pie
de la letra, el paso 7 de «Cada sesión» de la versión que se prueba, sin leer más de lo que ese paso y
«Chequeo de arranque» piden.

Montaje de un proyecto de prueba (`proy`), «antes» o «después»:
`tmp=$(mktemp -d)`; copiar `template/.` de esa versión (`git archive 8c9681b template | tar -x -C "$tmp"`
para «antes», `cp -R --update=none template/. "$tmp"/` para «después»); `git -C "$tmp" init -q`;
`printf '0.12.0\n' > "$tmp/.agents/.kit-version"` (en «antes», `0.11.0`).

## Casos
### CP-01 Instalación copia sin pisar
- RF: criterios de finalización
- Datos de partida: temporal vacío.
- Destino: `local`
- Pasos:
  1. `cp -R --update=none template/. "$tmp"/`; `diff -rq template "$tmp"`
- Resultado esperado: `cp` sin error; `diff` vacío.
- Herramienta: a mano
- Escribe: no

### CP-02 Secciones que se quedan, intactas
- RF: RF-2, RF-6, RF-12
- Datos de partida: clon tras F2.4.
- Destino: `local`
- Pasos:
  1. Para `AGENTS.md` y `template/AGENTS.md`, y para «Guardrails (todos los agentes, siempre)»,
     «Comandos», «Stack y convenciones de código» y «Reglas de negocio y del sistema»: el chequeo S de
     `04_tasks.md`.
- Resultado esperado: los ocho `diff` vacíos.
- Herramienta: a mano
- Escribe: no

### CP-03 Referencias por nombre resuelven
- RF: RF-4, RF-5, RF-11
- Datos de partida: clon tras F2.5.
- Destino: `local`
- Pasos:
  1. `grep -rnE 'Roles y comandos" de .AGENTS|ver "Formato y numeración"|Los pasos 1 y 2 son la lectura del arranque' template .agents ACTUALIZAR.md INSTALAR.md README.md`
  2. Comprobar que existen los encabezados citados: `grep -n '^## Estados de enlace\|^## Chequeo de arranque\|^## Estado «visto»' template/.agents/sync-agents.md`; `grep -n '^## Roles y comandos\|^## Qué va en' template/.agents/README.md`; `grep -n '^## Numeración única' template/.docs/002_specs/README.md`
  3. E11 de `/sdd-estructura` en el clon.
- Resultado esperado: paso 1 vacío; paso 2 con una línea por encabezado; E11 sin `FALLA`.
- Herramienta: a mano
- Escribe: no

### CP-04 Chequeo en el clon del kit (escenario base)
- RF: RF-7, RF-8
- Datos de partida: el propio clon (sin `.agents/.kit-version`, con `template/`).
- Destino: `local`
- Pasos:
  1. Seguir el paso 7 «antes» y «después».
  2. Anotar qué secciones de `sync-agents.md` se leyeron «después».
- Resultado esperado: silencio en los dos. «Después» solo se leyó «Chequeo de arranque».
- Herramienta: a mano
- Escribe: no

### CP-05 Proyecto sin «visto»
- RF: RF-8
- Datos de partida: `proy` «antes» y «después», sin `.agents/local/kit-visto` ni `.agents/local/configuracion.md`.
- Destino: `local`
- Pasos:
  1. Seguir el paso 7 en cada uno.
- Resultado esperado: el mismo aviso en los dos (variante «Clon nuevo: elige tus herramientas con
  `/sdd-enlaces`», con la sugerencia de `/sdd-config` en la línea 2), de 3 líneas como máximo.
- Herramienta: a mano
- Escribe: no

### CP-06 Proyecto con diferencias
- RF: RF-8
- Datos de partida: `proy` «antes» y «después» con `.agents/local/kit-visto` = `version: 0.10.0`,
  `herramientas: claude` y las líneas `pieza:` de hoy menos `commands/sdd-config.md`; sin `.claude/`;
  `.agents/local/configuracion.md` con `version: ` y la versión del `proy` en la primera línea y todas
  las secciones de la lista de `.agents/configuracion.ejemplo.md`.
- Destino: `local`
- Pasos:
  1. Seguir el paso 7 en cada uno.
- Resultado esperado: un aviso de 3 líneas como máximo en los dos. Línea 1: cambio de versión y 1 pieza
  nueva. Línea 2: faltan 3 enlaces de claude y `/sdd-enlaces`. Línea 3: pasos mínimos. Mismo contenido
  salvo el número de versión.
- Herramienta: a mano
- Escribe: no

### CP-07 Proyecto sin diferencias
- RF: RF-8
- Datos de partida: `proy` «después» con «visto» exacto (versión y piezas de hoy, `herramientas: claude`),
  los tres enlaces de `.claude/` hacia `../.agents/…` y `configuracion.md` al día. Igual con «antes».
- Destino: `local`
- Pasos:
  1. Seguir el paso 7 en cada uno.
- Resultado esperado: silencio en los dos.
- Herramienta: a mano
- Escribe: no

### CP-08 Actualización con `AGENTS.md` viejo
- RF: RF-13
- Datos de partida: `proy` de CP-06 «después» con `AGENTS.md` reemplazado por `git show 8c9681b:template/AGENTS.md`.
- Destino: `local`
- Pasos:
  1. Seguir el paso 7 del `AGENTS.md` viejo («sigue `.agents/sync-agents.md` › «Chequeo de arranque»»).
  2. E8 y E9 de `.agents/validar-estructura.md` nuevo sobre ese `proy`.
- Resultado esperado: el mismo aviso que CP-06 «después»; E8 y E9 sin `FALLA`.
- Herramienta: a mano
- Escribe: no

### CP-09 Paso 4 condicional y lectores de `.agents/README.md`
- RF: RF-10
- Datos de partida: clon tras F2.5.
- Destino: `local`
- Pasos:
  1. `sed -n '/^## Cada sesión/,/^## Antecedentes/p' template/AGENTS.md | grep -n 'agents/README.md'`
  2. `grep -n 'agents/README.md' template/.agents/commands/sdd-init.md template/.agents/sync-agents.md`
- Resultado esperado: paso 1, solo el paso 4 con la condición en una línea; paso 2, `sdd-init.md` y el
  paso 8 de «Qué hace el agente» siguen citándolo.
- Herramienta: a mano
- Escribe: no

### CP-10 Medición del arranque y de `AGENTS.md`
- RF: RF-14
- Datos de partida: clon tras F3.
- Destino: `local`
- Pasos:
  1. `wc -c AGENTS.md template/AGENTS.md .agents/napkin.md MEMORY.md .docs/001_constitucion/constitution.md .agents/skills/formato-respuesta/SKILL.md`
  2. `LC_ALL=C sed -n '/^## Chequeo de arranque/,/^## Estados de enlace/{/^## Estados de enlace/!p}' .agents/sync-agents.md | wc -c`
  3. Sumar napkin, `MEMORY.md`, constitución, `formato-respuesta` y el paso 2 (escenario base).
- Resultado esperado: suma ≤ 15 721; `AGENTS.md` ≤ 12 648; `template/AGENTS.md` ≤ 11 266.
- Herramienta: a mano
- Escribe: no

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-21 | ok | Sesión (F4.1) y `tester` (validate, claude-haiku-5-5): `rc=0`, `diff_rc=0` |
| CP-02 | 2026-11-21 | ok | Sesión y `tester`: los 8 `diff` vacíos |
| CP-03 | 2026-11-21 | ok | Sesión y `tester`: paso 1 vacío; los 6 encabezados existen; E11 igual a la línea base (51 rutas genéricas) |
| CP-04 | 2026-11-21 | ok | Sesión. Antes y después: omitido por el punto 1, silencio; después solo se leyó «Chequeo de arranque». Juicio del reviewer |
| CP-05 | 2026-11-21 | ok | Sesión (`.agents/local/0015/chequeo.py`). Antes y después: todo nuevo y sugerencia de `/sdd-config`. Juicio del reviewer |
| CP-06 | 2026-11-21 | ok | Sesión. Antes: 0.10.0 → 0.11.0; después: 0.10.0 → 0.12.0; en los dos, 1 pieza nueva (`commands/sdd-config.md`) y 3 enlaces de claude `FALTA`. Juicio del reviewer |
| CP-07 | 2026-11-21 | ok | Sesión. Antes y después: silencio. Juicio del reviewer |
| CP-08 | 2026-11-21 | ok | Sesión. `AGENTS.md` de `8c9681b` con `.agents/` nuevo: «Chequeo de arranque» existe una vez; mismo aviso que CP-06 después; E8 y E9 sin `FALLA`. Juicio del reviewer |
| CP-09 | 2026-11-21 | ok | Sesión y `tester`: en «Cada sesión», solo el paso 4 (con su condición); `sdd-init.md:55` y `sync-agents.md:139` siguen citándolo |
| CP-10 | 2026-11-21 | ok | Sesión y `tester`: `AGENTS.md` 12 267, `template/AGENTS.md` 11 061, «Chequeo de arranque» 3 139; suma del escenario base 12 110 ≤ 15 721 |

Detalle de las corridas antes y después (escenarios del chequeo y `/sdd-estructura`):
**Antes (2026-11-21 16:18, `8c9681b`, montaje de `07_casos_prueba.md`):**
- CP-04 clon: chequeo omitido por el punto 1; silencio.
- CP-05 sin «visto»: todo nuevo, sin herramientas declaradas, y falta `configuracion.md` (sugerir `/sdd-config` en la línea 2).
- CP-06 con diferencias: versión 0.10.0 → 0.11.0, 1 pieza nueva (`commands/sdd-config.md`), 3 enlaces de claude `FALTA`; configuración al día. Aviso de 3 líneas.
- CP-07 sin diferencias: silencio.


**Después (2026-11-21 18:11, árbol de trabajo tras F3):**
- CP-01 instalación en `mktemp -d`: `cp` sin error y `diff -rq` vacío.
- CP-02 «Guardrails», «Comandos», «Stack» y «Reglas de negocio» idénticas a `8c9681b` en `AGENTS.md` y `template/AGENTS.md` (8 de 8).
- CP-03 sin referencias viejas; existen «Chequeo de arranque», «Estados de enlace», «Estado «visto»», «Roles y comandos», «Qué va en `AGENTS.md`» y «Numeración única»; E11 igual a la línea base (51 rutas genéricas, ninguna nueva).
- CP-04 clon: omitido por el punto 1; silencio (igual). Solo se leyó «Chequeo de arranque».
- CP-05 sin «visto»: todo nuevo y sugerencia de `/sdd-config` (igual).
- CP-06 con diferencias: versión 0.10.0 → 0.12.0, 1 pieza nueva, 3 enlaces de claude `FALTA` (igual salvo el número de versión).
- CP-07 sin diferencias: silencio (igual).
- CP-08 `AGENTS.md` de 0.11.0 con `.agents/` nuevo: su paso 7 resuelve («Chequeo de arranque» existe una vez) y da el mismo aviso que CP-06; E8 y E9 sin `FALLA`.
- CP-09 en «Cada sesión», `.agents/README.md` solo en el paso 4 con su condición; `sdd-init.md` y el paso 8 de «Qué hace el agente» siguen citándolo.
- CP-10 medición: ver abajo.
- `/sdd-estructura` en el clon: E4–E9, E12–E16 y E19 sin `FALLA`; E7 solo las exentas de siempre; E20 con `manifiestos/0.12.0.md`. En el ensayo de instalación (con `.kit-version`): E4–E9, E15, E16 sin `FALLA` y E21 `OK` (marcador pendiente, cuenta 1). Sin `FALLA` nueva frente a F0.1.

