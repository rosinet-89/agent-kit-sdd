# 07 — Casos de prueba: Flujo eficiente

Destino de todos los casos: `local` (solo lectura con `grep`, `diff` y `cat`). Escribe: no.
Herramienta: shell de solo lectura (`AGENTS.md` › Comandos: «Herramienta de apoyo para casos: No
aplica»; los comandos de abajo son los de lint y ensayo de esa sección).

## Casos
### CP-01 Prohibición de agentes fuera del kit en una sola fuente
- RF: RF-2, RNF-1
- Pasos: `grep -rl 'general-purpose' template/.agents/`
- Resultado esperado: exactamente `template/.agents/skills/sdd/SKILL.md`

### CP-02 Motivos del lanzamiento
- RF: RF-3
- Pasos: `grep -c 'relanzamiento' template/.agents/skills/sdd/avance-y-tiempos.md`
- Resultado esperado: un número mayor o igual que `1`

### CP-03 Tres formas de `/sdd-implement`
- RF: RF-5
- Pasos: `grep '^argument-hint' template/.agents/commands/sdd-implement.md`
- Resultado esperado: `argument-hint: <prefijo_NombreFeature> [Fn | Fn.m]`

### CP-04 Tope de lote
- RF: RF-7
- Pasos: `grep -c 'hasta 4 pasos' template/.agents/commands/sdd-implement.md`
- Resultado esperado: `1`

### CP-05 Línea de chequeos de cierre en la plantilla
- RF: RF-15
- Pasos: `grep -c '^- Chequeos de cierre: No aplica$' template/AGENTS.md`
- Resultado esperado: `1`

### CP-06 E20 como chequeo de cierre en la raíz
- RF: RF-16
- Pasos: `grep -c '^- Chequeos de cierre:.*E20' AGENTS.md`
- Resultado esperado: `1`

### CP-07 Pasada por delta
- RF: RF-12
- Pasos: `grep -c 'por delta' template/.agents/skills/sdd/topes-y-modos.md`
- Resultado esperado: un número mayor o igual que `1`

### CP-08 Réplica
- RF: RNF-1
- Pasos: `diff -rq template/.agents .agents`
- Resultado esperado: solo `napkin.md`, `local`, `skills/caveman`, `skills/reporte-sesiones` y
  `agents/claudio.md`

### CP-09 Versión
- RF: RNF-3
- Pasos: `cat VersionAgentKitSdd`
- Resultado esperado: `0.14.0`

### CP-10 Ensayo de instalación
- RF: criterio 5
- Pasos: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/; diff -rq template "$tmp"`
- Resultado esperado: sin salida

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-22 | ok | `template/.agents/skills/sdd/SKILL.md` (tester, claude-haiku-5-5; comparación literal de la sesión) |
| CP-02 | 2026-11-22 | ok | `1` |
| CP-03 | 2026-11-22 | ok | `argument-hint: <prefijo_NombreFeature> [Fn \| Fn.m]` |
| CP-04 | 2026-11-22 | ok | `1` |
| CP-05 | 2026-11-22 | ok | `1` |
| CP-06 | 2026-11-22 | ok | `1` |
| CP-07 | 2026-11-22 | ok | `1` |
| CP-08 | 2026-11-22 | ok | Solo `claudio.md`, `local`, `napkin.md`, `caveman` y `reporte-sesiones` |
| CP-09 | 2026-11-22 | ok | `0.14.0` |
| CP-10 | 2026-11-22 | ok | Sin salida. La corrió la sesión en F5.4: escribe en un temporal y no es de `tester` |

## Evidencia formal (opcional)
No aplica.
