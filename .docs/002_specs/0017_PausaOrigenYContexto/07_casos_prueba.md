# 07 — Casos de prueba: Pausa, origen del kit y contexto bajo demanda

Herramienta de apoyo: «No aplica» en `AGENTS.md` › Comandos. Los casos se corren con comandos de
lectura (`wc`, `grep`, `diff`, `file`) o a mano. Ninguno escribe en una base de datos.

## Casos
### CP-01 Tamaño de `AGENTS.md` (RNF-1)
- RF: RNF-1
- Datos de partida: F0.1 (raíz 12 267, `template/` 11 061).
- Destino: `local`
- Pasos:
  1. `LC_ALL=C wc -c AGENTS.md template/AGENTS.md`.
  2. Sumar los bytes de lo que lee el arranque en el escenario base de CP-10 de la 0015.
- Resultado esperado: `template/AGENTS.md` ≤ 9 800; raíz ≤ 11 000; arranque ≤ 12 110.
- Herramienta: a mano
- Escribe: no

### CP-02 Piezas de la regla 1 (RF-12, RNF-7)
- RF: RF-12
- Datos de partida: copia de F0.1 en `.agents/local/0017-base/`.
- Destino: `local`
- Pasos:
  1. Medir la regla 1 nueva en las dos capas y compararlas (`diff` de la sección).
  2. Señalar en el texto nuevo cada pieza: (1) lista cerrada de permitidos; (2) ejemplos de
     prohibidos; (3) precedencia, con lo que no está en la lista (`SET` de sesión, `EXPLAIN ANALYZE`,
     `WITH` con escritura, `SELECT ... INTO`, `FOR UPDATE`/`FOR SHARE`, funciones con efectos, locks)
     y «ante duda, pide»; (4) escritura indirecta; (5) trato de los Comandos, incluida la duda;
     (6) entrega del script con rollback aunque se pida correrlo; (7) quién escribe en `sql/`;
     (8) consulta de datos («No aplica», sin credenciales); (9) la regla es sobre lo que se ejecuta,
     no sobre el código; (10) excepción de QA de `tester` con su modo seguro.
- Resultado esperado: ≤ 1 900 bytes, idéntica en las dos capas, 10 de 10 piezas.
- Herramienta: a mano (revisión independiente en validate)
- Escribe: no

### CP-03 Antecedentes y deuda bajo demanda (RF-11)
- RF: RF-11
- Datos de partida: F1 terminada.
- Destino: `local`
- Pasos:
  1. `grep -rn 'Antecedentes\|Deuda técnica' template/ AGENTS.md` y resolver cada remisión.
  2. Comparar el cuerpo de `antecedentes-y-deuda.md` con la copia de F0.1.
- Resultado esperado: cada remisión resuelve en las dos capas; mismo sentido (6 + 4 puntos).
- Herramienta: a mano
- Escribe: no

### CP-04 Paso 8 sobre índices (RF-17)
- RF: RF-17
- Datos de partida: F1 terminada.
- Destino: `local`
- Pasos:
  1. Leer el paso 8 en las dos capas.
  2. Comprobar con las tablas de índice de `002_specs`, `003_deuda` y `004_adr` que no hay dos filas del
     mismo índice con el mismo entero.
- Resultado esperado: el paso no lista carpetas; dos índices distintos con el mismo entero no son
  repetido.
- Herramienta: a mano
- Escribe: no

### CP-05 Excepción de red y `/sdd-version` (RF-7, RF-8)
- RF: RF-7, RF-8
- Datos de partida: F3 terminada.
- Destino: `ninguno`
- Pasos:
  1. Leer «Git» en las dos capas y `sdd-version.md`.
  2. Comprobar que el procedimiento solo usa `git ls-remote` y `git clone --depth 1` en `mktemp -d`,
     con `GIT_TERMINAL_PROMPT=0`, borrado del temporal y parada si pide credenciales.
- Resultado esperado: única excepción; `push`/`pull`/`fetch`/`sync` siguen prohibidos; en el repo del
  kit no aplica.
- Herramienta: a mano
- Escribe: no

### CP-06 Origen y commits de `ACTUALIZAR.md` (RF-6, RF-9, RF-10)
- RF: RF-6, RF-9, RF-10
- Datos de partida: F3 y F7 terminadas.
- Destino: `local`
- Pasos:
  1. Leer `INSTALAR.md` y `ACTUALIZAR.md` (escritura y comparación de `.kit-origen`; tres grupos).
  2. Leer E25 de `validar-estructura.md`.
- Resultado esperado: `.kit-origen` con `url` y `rama`; tres grupos, vacío no se propone, nunca
  `git add .`/`-A`; E25 avisa y no usa la red.
- Herramienta: a mano
- Escribe: no

### CP-07 Pausa y retomo (RF-1 a RF-5)
- RF: RF-1, RF-2, RF-3, RF-4, RF-5
- Datos de partida: F2 terminada.
- Destino: `local`
- Pasos:
  1. Leer `sdd-pausa.md`, `sdd-status.md`, `avance-y-tiempos.md` y `plantillas.md`.
  2. Recorrer los casos límite de la spec: sin carpeta activa, subagente sin respuesta, dos pausas
     seguidas, recibo que ya no existe.
- Resultado esperado: cada RF y caso límite tiene su frase; `/sdd-status` solo ejecuta la lista
  cerrada y no escribe; la marca vive en `avance-y-tiempos.md` y lo demás remite.
- Herramienta: a mano
- Escribe: no

### CP-08 Cambio pequeño y chequeo de arranque (RF-13, RF-16)
- RF: RF-13, RF-16
- Datos de partida: F1 y F7 terminadas.
- Destino: `local`
- Pasos:
  1. `grep -rn 'fix chico\|fuera de este flujo\|fuera del flujo' template/.agents`.
  2. Leer «Modo ligero» y «Chequeo de arranque».
- Resultado esperado: una sola definición de cambio pequeño; ninguna frase ofrece un camino fuera del
  flujo; el chequeo de carpeta por archivo solo comprueba la entrada.
- Herramienta: a mano
- Escribe: no

### CP-09 Contexto del proyecto (RF-20 a RF-23)
- RF: RF-20, RF-21, RF-22, RF-23
- Datos de partida: F4 y F7 terminadas.
- Destino: `local`
- Pasos:
  1. `diff template/.docs/005_contexto/README.md .docs/005_contexto/README.md`.
  2. Leer la propuesta de `sdd-init.md`, el punto de `sdd-cierre.md`, la línea de `AGENTS.md` y E26.
- Resultado esperado: índice vacío igual en las dos capas; sin visto bueno no se crea ni se actualiza
  nada; E26 avisa por documento sin fila, fila sin documento, sin «Última verificación» o > 180 días.
- Herramienta: a mano
- Escribe: no

### CP-10 Stack y permisos (RF-14, RF-15, RF-24 a RF-26)
- RF: RF-14, RF-15, RF-24, RF-25, RF-26
- Datos de partida: F5 terminada.
- Destino: `local`
- Pasos:
  1. Leer `catalogo-skills.md`, `sdd-init.md` y `restricciones-de-rol.md`.
  2. `grep -n 'mysql\|psql\|sqlcmd\|sqlplus\|sqlite3\|mongosh\|redis-cli' template/.agents/skills/sdd/restricciones-de-rol.md`.
- Resultado esperado: 7 clientes en el `deny`; el catálogo no nombra proyectos ni fuentes externas
  (RNF-4); no abre `.env`; allow solo con comandos de verificación de solo lectura del proyecto.
- Herramienta: a mano
- Escribe: no

### CP-11 Estatus y ruteo (RF-27, RF-28, RF-29)
- RF: RF-27, RF-28, RF-29
- Datos de partida: F6 y F7 terminadas.
- Destino: `local`
- Pasos:
  1. Leer «Quién escribe» en las dos capas y comparar.
  2. Leer E10, E13, la fila de `/sdd-constitucion` del `coordinator` y la tabla de fases.
- Resultado esperado: tres palabras intermedias y reapertura de `Descartado` en «Quién escribe»;
  `sdd-plan.md` y `sdd-implement.md` solo remiten; `/sdd-constitucion` sin fila de fase y exento en E10.
- Herramienta: a mano
- Escribe: no

### CP-12 Bandera y marcador de la raíz (RF-18, RF-19, RF-30)
- RF: RF-18, RF-19, RF-30
- Datos de partida: F7 terminada.
- Destino: `local`
- Pasos:
  1. Correr `/sdd-estructura` en el clon.
  2. `git check-ignore .agents/local/estructura-ok` y leer el archivo.
  3. Leer el resultado de E24.
- Resultado esperado: bandera con `alineada`, `version` (= `VersionAgentKitSdd`) y `fecha`, ignorada y
  sin `git add`; E24 informa el marcador de la raíz frente a `template/AGENTS.md`.
- Herramienta: a mano
- Escribe: no

### CP-13 Instalación, réplica y manifiesto (RF-31, RNF-5, RNF-8)
- RF: RF-31
- Datos de partida: F8 terminada.
- Destino: `local`
- Pasos:
  1. `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` y revisar que se copie todo.
  2. `/sdd-estructura` en el ensayo, frente a F0.2.
  3. `diff -rq template/.agents .agents`.
  4. Comparar las huellas de `manifiestos/0.13.0.md` con `template/`.
- Resultado esperado: nada se pisa; sin `FALLA` nueva; solo diferencias aceptadas; huellas iguales;
  versión 0.13.0 en `VersionAgentKitSdd` y en el changelog.
- Herramienta: a mano
- Escribe: no

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-22 | obtenido | F0.1 antes: raíz 12 267, `template/` 11 061; regla 1 2 640; «Antecedentes» 893; «Deuda técnica» 691/697; «Cada sesión» 2 456/2 354. Copias en `.agents/local/0017-base/` |
| CP-01 | 2026-11-22 | ok | Después: `template/AGENTS.md` 9 603 (≤ 9 800), raíz 10 815 (≤ 11 000). Arranque del escenario base: pendiente de medir en validate |
| CP-02 | 2026-11-22 | obtenido | Regla 1: 1 886 bytes en las dos capas, `diff` vacío. 10 de 10 piezas señaladas por la sesión (líneas 16–41 de `template/AGENTS.md`). Revisión independiente: validate |
| CP-03 | 2026-11-22 | obtenido | Encabezados con condición y remisión en las dos capas (líneas 68–75); paso 6 y tabla de skills siguen citando «Antecedentes» |
| CP-04 | 2026-11-22 | ok | Paso 8 sobre índices; sin repetidos en `002_specs`, `003_deuda`, `004_adr`, `005_contexto` |
| CP-08 | 2026-11-22 | obtenido | Solo quedan frases en negativo («no hay camino fuera del flujo») en `topes-y-modos.md:75`, `SKILL.md:17`, `coordinator.md:16` |
| CP-09 | 2026-11-22 | ok | `005_contexto/README.md` idéntico en las dos capas (`cmp`) |
| CP-10 | 2026-11-22 | ok | 7 de 7 clientes en el `deny`; el catálogo no cita URL ni fuentes externas (`grep` vacío) |
| CP-11 | 2026-11-22 | ok | «Quién escribe» idéntica en las dos capas |
| CP-13 | 2026-11-22 | obtenido | `diff -rq template/.agents .agents`: solo `napkin.md`, `agents/claudio.md`, `skills/caveman`, `skills/reporte-sesiones` y `local` (aceptadas). Manifiesto 0.13.0: 54 rutas (49 + 5 nuevas), 18 huellas cambian = 18 archivos modificados de `template/` |
| CP-12 | 2026-11-22 | obtenido | `/sdd-estructura` posterior en el clon: bandera `alineada: no` (FALLA previas E3, E8, E19), `version: 0.13.0`, ignorada por `git check-ignore`, sin `git add`. E24 OK: raíz 0 marcadores, `template/AGENTS.md` 1 con la misma cadena |
| CP-13 | 2026-11-22 | ok | Ensayo: 54 de 54 archivos copiados sin pisar. Clon: 3 FALLA (E3, E8, E19) y 4 AVISO (E1, E10, E22, E23), ninguna nueva frente a F0.2; E11 desaparece. Ensayo: 1 FALLA (E11, igual que la base) y 3 AVISO (E1, E10, E25 nuevo y esperado: sin `.kit-origen` antes de `ACTUALIZAR.md`). E20 OK con `manifiestos/0.13.0.md` |
| CP-01 | 2026-11-22 | ok | Validate, `tester` (claude-haiku-5-5): `AGENTS.md` 10 815, `template/AGENTS.md` 9 603. Sesión: arranque del escenario base de CP-10 de la 0015 = 12 515 > 12 110 (falla) por «Chequeo de arranque» 3 139 → 3 544 (F1.5); tras compactar su redacción (decisión 24), 3 136 y suma 12 107 ≤ 12 110 |
| CP-02 | 2026-11-22 | obtenido | `tester`: regla 1 = 1 886 bytes; `diff` de las dos capas `exit=0`. Las 10 piezas: juicio del reviewer |
| CP-03 | 2026-11-22 | obtenido | `tester`: encabezados y remisiones en las dos capas (líneas 68–75); `antecedentes-y-deuda.md` existe en `template/.agents/` y `.agents/` |
| CP-05 | 2026-11-22 | obtenido | `tester`: excepción de red en «Git» (línea 113); `GIT_TERMINAL_PROMPT=0` en `sdd-version.md`. El `grep` del tester etiquetó mal la línea de la raíz; la sesión comprobó `AGENTS.md:113` |
| CP-07 | 2026-11-22 | obtenido | `tester`: `sdd-pausa.md` existe; fila «Pausa» en `plantillas.md:110` y `avance-y-tiempos.md:37`, fuente única en `avance-y-tiempos.md:41` |
| CP-09 | 2026-11-22 | ok | `tester`: `cmp` `exit=0` |
| CP-10 | 2026-11-22 | ok | `tester`: 7 de 7 clientes; 0 apariciones de `http` en el catálogo |
| CP-11 | 2026-11-22 | ok | `tester`: «Quién escribe» `diff` `exit=0` |
| CP-13 | 2026-11-22 | ok | `tester`: `diff -rq` solo con lo aceptado; `0.13.0`; changelog `## 0.13.0 — 2026-11-22`; 54 filas de manifiesto = 54 archivos de `template/` |
| CP-02 | 2026-11-22 | ok | Reviewer (chat, claude-opus-5-5): 10 de 10 piezas en `template/AGENTS.md:14-41` frente a la copia de partida |
| CP-03 | 2026-11-22 | ok | Reviewer: el cuerpo de las dos secciones es idéntico a la copia de partida (`diff` vacío) |
| CP-05 | 2026-11-22 | falla | Reviewer, vuelta 1: `sdd-version.md:13` sin `GIT_TERMINAL_PROMPT=0` en el clon y temporal no borrable |
| CP-05 | 2026-11-22 | ok | Tras la corrección (implementer, claude-sonnet-5-5): `tmp=$(mktemp -d)`, `GIT_TERMINAL_PROMPT=0 git clone … "$tmp"`, `rm -rf "$tmp"` |
| CP-06 | 2026-11-22 | ok | Reviewer: `INSTALAR.md` y `ACTUALIZAR.md` › «Origen del kit» y «Commit propuesto» (3 grupos); E25 solo `AVISO` |
| CP-07 | 2026-11-22 | ok | Reviewer: RF-1 a RF-5 y los casos límite en `sdd-pausa.md` y `sdd-status.md`; «Marca de pausa» corregida (anotación de diferimiento) |
| CP-08 | 2026-11-22 | ok | Reviewer: una sola definición de cambio pequeño; sin camino fuera del flujo |
| CP-12 | 2026-11-22 | ok | Reviewer: bandera según «Bandera de estructura»; Lectura corregida con `VersionAgentKitSdd` en el repo del kit; E24 en `AVISO` |

## Evidencia formal (opcional)
No aplica.
