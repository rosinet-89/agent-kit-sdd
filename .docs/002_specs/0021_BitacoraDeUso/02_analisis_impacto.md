# 02 — Análisis de impacto: Bitácora de uso (beta)

## 1. Resumen del requerimiento
Bitácora local por dev, opcional y en beta: por cada fila de la tabla Tiempos de una spec con la
bitácora activa (o por cada turno de una actividad con etiqueta), una entrada con modelo, rol, motivo
del lanzamiento, minutos y nota; los tokens se completan al cierre o al pedir el reporte, desde los
transcripts de la herramienta. Reporte en el chat, listo para un issue. Una skill nueva es la fuente
única; `/sdd-config`, `/sdd-spec` y `/sdd-cierre` remiten a ella (spec, 17 RF).

### 1.1 Antecedentes
- `0020_RuteoDeModelos` (Cerrado): la Nota de cada fila de Tiempos ya lleva modelo y motivo del
  lanzamiento (`avance-y-tiempos.md` › «Tiempos reales», viñeta **Modelo**). Se reutiliza tal cual
  como dato de RF-8; su spec deja la bitácora fuera de alcance y la manda aquí. No contradice nada.
- `0014_ConfiguracionGuiada` (Cerrado): pasos de `/sdd-config`, lista de nombres fijos de
  `configuracion.ejemplo.md` › «Formato del archivo local» y patrón beta del auditor externo
  (apagado por omisión, declarado en el ejemplo y en `SKILL.md`). Se reutiliza el patrón; el paso
  nuevo entra a la lista fija, que el chequeo de arranque ya usa (RF-3 sin cambio en `sync-agents.md`).
- `0015_ArranqueLigero`: el arranque no suma lecturas. La skill nueva no entra al arranque.
- `0022_PlanContraRealEnVivo` (Análisis): probablemente toca `sdd-cierre.md` y
  `avance-y-tiempos.md`, los mismos que esta spec. Va después; hereda estos cambios.
- Grep del índice del 2026-11-22 (`bitácora`, `tokens`, `medición`, `reporte`): sin otra carpeta.

### 1.2 Deuda técnica relacionada
Ninguna abierta en `.docs/003_deuda/README.md` sobre medición, tokens o configuración.

## 2. Módulos, menú, tablas y archivos involucrados

### 2.1 Menú / opciones / rutas de acceso
- `/sdd-config` (paso nuevo `bitacora`), `/sdd-spec` (una pregunta), `/sdd-cierre` (completar tokens).
- Cualquier comando que agregue una fila de Tiempos dispara la entrada (vía `avance-y-tiempos.md`).
- Petición en lenguaje natural: «registra la actividad <etiqueta>», «reporte de la bitácora de 0021»
  (S-1, S-2). Sin comando `/sdd-*` nuevo.

### 2.2 Tablas (existentes y nuevas)
No hay BD. «Tablas» del kit:
- Tabla Tiempos de `05_avance.md`: no cambia de formato. Es el disparador de RF-8.
- Nueva: tabla de entradas en cada archivo de `.agents/local/bitacora/` (local, fuera de Git).
- Sección nueva `## Bitácora` en `.agents/local/configuracion.md` (local).

### 2.3 Archivos (ruta, capa)
| Archivo | Capa | Qué cambia | RF |
|---|---|---|---|
| `template/.agents/skills/bitacora-uso/SKILL.md` | skill (nueva) | Fuente única: activación, formato, registro, actividad, fuente de tokens, reporte, privacidad | 6, 8–16 |
| `template/.agents/configuracion.ejemplo.md` | config ejemplo | `## Bitácora` en la lista fija; «Paso 6. Bitácora de uso (beta)» | 1, 2, 3, 17 |
| `template/.agents/commands/sdd-config.md` | comando | `bitacora` en la lista de `[paso]`; el paso remite a la skill (beta, datos que guarda) | 1, 2, 17 |
| `template/.agents/commands/sdd-spec.md` | comando | Una pregunta antes de redactar, por remisión a la skill | 4, 5 |
| `template/.agents/commands/sdd-cierre.md` | comando | En cada corrida, completar tokens por remisión | 12 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | contrato | Una línea: con bitácora activa, la sesión agrega la entrada (remite) | 8, 11 |
| `template/AGENTS.md` | instrucciones | Fila de la skill en «Skills de proyecto» | 16 |
| `AGENTS.md` (raíz) | instrucciones del clon | Misma fila (E7 en el clon) | 16 |
| `README.md` (raíz) | doc | Skill en el árbol de `template/` y mención beta | 17 |
| `INSTALAR.md` | doc | Paso 7: bitácora beta en `/sdd-config`, apagada por omisión | 17 |
| `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd` | versión | 0.16.0 con «beta» | 17 |
| `manifiestos/0.16.0.md` | manifiesto (no se distribuye) | Huellas de `template/` | criterio E20 |

Sin cambio (verificado): `sync-agents.md` (el chequeo lee la lista fija del ejemplo), `ACTUALIZAR.md`
(pasos faltantes por regla genérica), `validar-estructura.md` (E4–E7 cubren la skill nueva),
`catalogo-skills.md` (no es skill de dominio), `template/.agents/README.md` (su árbol no lista skills).

### 2.4 Código legado o sin uso
- Piezas personales fuera del kit, no se tocan: `turno_tokens.py`, `metricas_subagentes.py` y la
  skill local `reporte-sesiones` (diferencia aceptada en `diff -rq`, `MEMORY.md`). La skill nueva no
  las cita ni depende de ellas.

## 3. Datos reales
Sin BD (`AGENTS.md` › Comandos: «Consulta de datos: No aplica»). Medición de solo lectura sobre los
transcripts de Claude Code de este repo, 2026-11-22 20:20, `python3 -I` sin escribir:
- Carpeta: `$CLAUDE_CONFIG_DIR/projects/-home-cromero-dev-agent-kit-sdd/` (aquí `CLAUDE_CONFIG_DIR`
  no es `~/.claude`). 37 `.jsonl` de sesión; los subagentes en `<sesión>/subagents/agent-*.jsonl`
  con su `agent-*.meta.json`.
- Línea `type: assistant`: `timestamp` en UTC (`2026-11-23T02:14:52Z` = 20:14 local), `message.model`
  (`claude-opus-5-5`), `message.id`, `requestId` y `message.usage` con `input_tokens`,
  `output_tokens`, `cache_read_input_tokens`, `cache_creation_input_tokens`.
- En la sesión actual, 19 líneas `assistant` y 11 `message.id` distintos: un mensaje con varios
  bloques repite su `usage` en cada línea.

## 4. Flujo actual
1. La sesión principal agrega una fila a Tiempos al cerrar cada fase o paso, con modelo y motivo en
   la Nota (`avance-y-tiempos.md`). No hay tokens ni vista por modelo y rol.
2. `/sdd-cierre` arma plan contra real con minutos; no ve tokens.
3. Los tokens solo se ven con piezas personales sueltas, fuera del kit.

## 5. Reglas de negocio: conocidas vs kit
| Regla | Dónde vive hoy | Efecto aquí |
|---|---|---|
| Solo la sesión principal escribe `05_avance.md` | `avance-y-tiempos.md` › «Quién y cuándo» | Igual para la bitácora (RF-11) |
| Fila de lote = una fila | `avance-y-tiempos.md` › «Mínimo y lote» | Una entrada por lote |
| Paso nuevo de config entra a la lista fija | `configuracion.ejemplo.md` › «Formato del archivo local» | `## Bitácora` se agrega ahí |
| Sección faltante toma el sugerido | mismo archivo | RF-3 ya cubierto: apagada |
| Una regla, una fuente | `MEMORY.md` 2026-11-16 | Comandos remiten a la skill |
| Sin código versionado | constitución, principio 5 | La lectura de `.jsonl` es instrucción, no script |

## 6. Riesgos y casos borde
| Riesgo | Mitigación en el plan |
|---|---|
| Ruta de transcripts distinta de `~/.claude` | La skill toma `CLAUDE_CONFIG_DIR` si existe; si no, `~/.claude` |
| Hora local en Tiempos y UTC en el `.jsonl` | La skill convierte la ventana con la zona del sistema |
| `usage` repetido por `message.id` | Sumar una vez por `message.id` |
| Tokens del subagente fuera del `.jsonl` principal | Sumar también `subagents/*.jsonl` con mensajes dentro de la ventana |
| Varias sesiones del mismo proyecto en la misma ventana | Sin forma de separarlas: «no disponible (sesiones paralelas)» salvo que la persona elija la sesión |
| Fila con pausa > 0: la ventana incluye trabajo ajeno | Se suman los tokens y la entrada lleva la marca «ventana con pausa»; el reporte la cuenta aparte |
| Transcript limpiado por la herramienta | «no disponible» (RF-12) |
| Leer `.jsonl` expone texto de prompts | Solo se leen `timestamp`, `message.id`, `message.model` y `usage`; nunca se copia contenido (RF-13) |
| Conflicto de edición con `0022` en `sdd-cierre.md` y `avance-y-tiempos.md` | 0021 va primero y en commit propio |
| Manifiesto viejo tras un ajuste tardío de `template/` | E20 al final, después del último cambio (`MEMORY.md` 2026-11-21) |
| Skill nueva sin réplica en `.claude/skills/` del clon | F5: `cp` a `.agents/` y `/sdd-enlaces` con confirmación |

## 7. Preguntas abiertas
Ninguna que frene. Decisión con alternativas y efecto duradero para valorar como ADR: formato y
almacenamiento de la bitácora más la captura de tokens por ventana (decisiones 2 y 5 del plan).
