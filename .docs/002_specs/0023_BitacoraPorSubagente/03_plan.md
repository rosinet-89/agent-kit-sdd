# 03 — Plan: Bitácora por subagente
Spec: 01_spec.md (aprobada 2026-11-22 21:37)

## 1. Ubicación y capas
- `template/.agents/skills/bitacora-uso/SKILL.md`: fuente única de la forma de la entrada y del
  cálculo de tokens (RNF «una regla, una fuente»).
- `template/.agents/skills/sdd/avance-y-tiempos.md`: solo remisión (plural «entradas»).
- Raíz: versión, changelog y manifiesto; réplica local de `.agents/` con `cp`.
- Lo propio de Claude Code (variable de entorno, ruta del transcript) va solo en la tabla «Fuente de
  tokens» (constitución 1).

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/bitacora-uso/SKILL.md` › Formato | template | Columnas «Sesión» y «Subagente»; ejemplo con una ola (2 delegadas + coordinación) | RF-1 |
| idem › Registro por tramo | template | Entradas delegadas con id, varias por fila, coordinación en la misma escritura; anotar el id al recibir el lanzamiento | RF-2, RF-3, RF-4 |
| idem › Actividad | template | Sesión anotada, subagentes del turno incluidos | RF-13 |
| idem › Fuente de tokens | template | Columnas «Id de sesión» y «Transcript del subagente» por herramienta | RF-12 |
| idem › Completar tokens | template | Delegada sin ventana; chat por sesión; coordinación por resta; un `message.id` una vez; regla anterior para entradas viejas o sin id; «no disponible» sin estimar | RF-5 a RF-9, RF-11 |
| idem › Reporte | template | Suma directa; coordinación dentro de `chat` y aparte; minutos de coordinación fuera; coordinación como tramo en hallazgos | RF-10 |
| idem › Privacidad | template | Se leen también nombres de archivo e ids | RNF |
| `template/.agents/skills/sdd/avance-y-tiempos.md` (líneas 98 y 157) | template | «agrega las entradas» y «las entradas de la bitácora»; sin copiar la regla | RF-3 |
| `VersionAgentKitSdd` | raíz | 0.18.0 | — |
| `ChangelogAgentKitSdd.md` | raíz | Entrada 0.18.0 | — |
| `manifiestos/0.18.0.md` | raíz | Huella de `template/` (algoritmo de `ACTUALIZAR.md`) | — |
| `.agents/skills/bitacora-uso/SKILL.md`, `.agents/skills/sdd/avance-y-tiempos.md` | local | Réplica con `cp` | — |

## 3. Base de datos
No aplica (sin `sql/`).

## 4. Seguridad
- Privacidad: el cálculo lee solo `timestamp`, `message.id`, `message.model`, `message.usage`,
  nombres de archivo e ids; nunca contenido. El comando de lectura sigue siendo de un solo uso.
- Sin secretos: el id de sesión no es credencial; no se anota ningún token de mensajería del entorno.
- Guardrail 1: no aplica (sin BD).

## 5. Liberación / piloto
- Beta, como la skill. Piloto: esta misma spec (bitácora activa) desde la primera fila escrita tras
  la réplica; sus entradas anteriores quedan con la regla vieja (RF-9).
- Prueba retroactiva con los transcripts de 0022 (sin reescribir su bitácora).

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| D1. Entrada delegada por id de subagente, sin ventana | Ventana filtrada por sesión y por subagente activo | La ventana corta lo que el subagente hace tras el Fin y obliga a adivinar qué subagente era de qué lote |
| D2. Coordinación en una entrada aparte | Repartir la coordinación entre los lotes; ignorarla | Repartir inventa una proporción (nunca se estima); ignorarla oculta el 80 % del costo de una ola (§3) |
| D3. Id de sesión en cada entrada | Seguir marcando «sesiones paralelas» | Con el id, el tramo se ubica sin preguntar (HU-3) |
| D4. Coordinación escrita al registrar, minutos vacíos | Crearla al completar tokens | Supuesto aprobado en la spec; mantiene «solo se escribe junto a la fila» |
| D5. Sin flujo (`06_flujo.md`) | Flujo operativo | Cambio interno de una skill beta sin pantalla ni rol nuevo; mismo criterio que 0021 y 0022 |
| D6. Solo dos archivos de `template/` | Tocar `sdd-cierre.md` y `sdd-implement.md` | Ya remiten a la skill; el id lo guarda la sesión según la skill |

D1 a D3: ADR [0040](../../004_adr/0040_BitacoraConTokensPorSubagenteYSesion.md), que reemplaza al 0038.

## 7. Fases y estimado
Estimado en minutos y por ola (`MEMORY.md`, 2026-11-22).

| Fase | Contenido | RF | Estimado |
|---|---|---|---|
| F0 | Línea base: copia de los archivos a tocar y `git diff` contra `HEAD` | — | 2 min |
| F1 | Skill `bitacora-uso`: Formato, Registro, Actividad, Fuente, Completar, Reporte, Privacidad | RF-1 a RF-13 | 12 min |
| F2 | Remisión en `avance-y-tiempos.md` | RF-3 | 2 min |
| F3 | Versión 0.18.0 y changelog | — | 3 min |
| F4 | Pruebas: retroactiva con 0022 (ola 1) y de dos sesiones; bitácora vieja con la regla anterior | RF-5 a RF-9 | 5 min |
| F5 | Réplica local, lint, ensayo de instalación, manifiesto 0.18.0 y E20 | — | 6 min |

Olas: F0; luego F1, F2 y F3 en paralelo (archivos disjuntos); luego F4; luego F5. Total ≈ 30 min.

## 8. Criterios de aceptación
1. La skill cumple RF-1 a RF-13 y las demás piezas remiten sin copiar la regla (`grep` de
   «Registro por tramo» y «Completar tokens» fuera de la skill solo da remisiones).
2. Prueba retroactiva 0022, ola 1 (criterio 2 de la spec): cada lote L1–L5 da el
   total de su transcript (45 345 a 140 814) y la coordinación, con su ventana de delegación
   (21:03–21:06) menos los mensajes que ya cuentan F0.1 y F6.4 (RF-7, RF-8), da 1 033 088; la suma
   (≈ 1,42 M) es menor que la de la regla actual (15,9 M).
3. Prueba de dos sesiones: una entrada en el chat con id de sesión, en una ventana con mensajes de
   otra sesión, da sus tokens.
4. La bitácora de 0022 se completa igual que antes (RF-9).
5. `VersionAgentKitSdd` = 0.18.0, changelog con la entrada, `manifiestos/0.18.0.md` y E20 en verde.
6. Lint de `AGENTS.md` › Comandos en cada archivo tocado; ensayo de instalación sin pisar nada;
   `diff -rq template/.agents .agents` solo con las diferencias admitidas en `MEMORY.md`.
