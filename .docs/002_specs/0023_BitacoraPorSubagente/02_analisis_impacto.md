# 02 — Análisis de impacto: Bitácora por subagente

## 1. Resumen del requerimiento
Que cada entrada de la bitácora de uso sume solo los tokens de quien hizo el tramo: una entrada
delegada, el transcript de su subagente; una entrada en el chat, su propia sesión; la coordinación
de la sesión principal durante una delegación, una entrada aparte (`01_spec.md`, RF-1 a RF-13).

### 1.1 Antecedentes
- `0021_BitacoraDeUso` (Cerrado): crea la skill `bitacora-uso` que aquí se cambia. Se reutiliza su
  activación, su privacidad y su disparador «misma escritura que la fila de Tiempos». Se contradice
  su «Un lote es una entrada, como es una fila en Tiempos» (RF-3 lo reemplaza) y su ventana común a
  sesión y subagentes en «Completar tokens» (RF-5 a RF-8).
- `0022_PlanContraRealEnVivo` (Cerrado): primer uso real con olas; midió el problema (decisión 22) y
  dejó la trampa en `MEMORY.md`. Su tabla Pasos con tiempos no cambia.
- ADR 0038 (bitácora de uso): fija captura desde transcripts y almacenamiento local; su cálculo por
  ventana lo reemplaza el ADR 0040.

### 1.2 Deuda técnica relacionada
Ninguna: `.docs/003_deuda/README.md` no tiene deudas de la bitácora (`grep -i bitác` vacío).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica (sin pantallas). Entradas: escritura de filas de Tiempos en cualquier fase con bitácora
activa; `/sdd-cierre` punto 1 (completar tokens); reporte pedido en lenguaje natural.

### 2.2 Tablas (existentes y nuevas)
No aplica (sin BD). La «tabla» es la de entradas de `.agents/local/bitacora/<carpeta>.md`, local.

### 2.3 Archivos de código (ruta, capa)
| Archivo | Capa | Qué cambia |
|---|---|---|
| `template/.agents/skills/bitacora-uso/SKILL.md` | template | Formato, Registro por tramo, Actividad, Fuente de tokens, Completar tokens, Reporte |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | template | Líneas 98 y 157: «la entrada» pasa a «las entradas»; sigue remitiendo |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.18.0.md` | raíz | Publicación 0.18.0 |
| `.agents/` (réplica local) | mantenimiento | `cp` de los dos archivos de `template/` |

Citan la skill y no cambian (remiten sin copiar la regla): `template/.agents/commands/sdd-cierre.md`
(punto 1), `template/.agents/commands/sdd-spec.md` (paso 3), `template/.agents/commands/sdd-config.md`,
`template/.agents/configuracion.ejemplo.md` (paso 6), `template/AGENTS.md` (tabla de skills),
`README.md` (fila de la bitácora), `INSTALAR.md` (línea 63).

### 2.4 Código legado o sin uso
Bitácoras ya escritas (`0021_BitacoraDeUso.md`, `0022_PlanContraRealEnVivo.md`,
`actividad-prueba-bitacora.md`): sin columnas nuevas; se completan con la regla anterior (RF-9).

## 3. Datos reales (transcripts de Claude Code, solo `timestamp`, `message.id`, `message.model` y `message.usage`; 2026-11-22 21:40)
Origen: `~/.claude/projects/-home-cromero-dev-agent-kit-sdd/`, sesión `0efc4732…` (0022) y
`dc9870c7…` (validate de 0022). Cada `message.id` una vez (último `usage`).

| Tramo de 0022 | Fuente | Tokens |
|---|---|---|
| L1 avance-y-tiempos (F1.1–F1.3) | `subagents/agent-<id>.jsonl` | 79 084 |
| L2 remisiones (F3.1–F3.4) | idem | 140 814 |
| L3 leyenda (F4.1, F4.2) | idem | 52 190 |
| L4 plantilla (F2.1) | idem | 67 400 |
| L5 versión (F5.1) | idem | 45 345 |
| Sesión principal 21:03–21:05 (coordinación ola 1) | `<sesión>.jsonl` en ventana | 1 822 349 |
| Coordinación con la regla (21:03–21:06 menos F0.1 y F6.4, por `message.id`) | `<sesión>.jsonl` en ventana | 1 033 088 |
| Bitácora actual, cada lote de la ola 1 | ventana común | 3 182 217 (×5) |
| auxiliar `/sdd-estructura` (F6.3) | transcript propio | 755 664 |

- Con la regla actual la ola suma 15,9 M; con la nueva, 2,21 M (lotes 385 mil + coordinación 1,82 M).
  Con la ventana de delegación de la regla (RF-7, RF-8), la ola da ≈ 1,42 M (lotes 385 mil +
  coordinación 1 033 088); criterio 2 corregido en la decisión 20 de `05_avance.md`.
- Las cifras de la idea (77–171 mil por lote, 2,49 M) usaban otro conteo; no coinciden con la regla
  de deduplicación de la skill. Afecta al criterio de finalización 2 de la spec (pregunta 1).
- `CLAUDE_CODE_SESSION_ID` está en el entorno y coincide con el nombre del `<sesión>.jsonl`; cada
  subagente tiene `agent-<agentId>.jsonl` y `.meta.json` (`agentType`, `model`, `description`).

## 4. Flujo actual
1. La sesión cierra un tramo, escribe la fila de Tiempos y, en la misma escritura, una entrada con
   tokens «pendiente».
2. En `/sdd-cierre` o al pedir el reporte, «Completar tokens» suma por ventana Inicio–Fin todos los
   mensajes de la sesión principal y de todos los `subagents/*.jsonl`.
3. Con dos sesiones en la ventana, la entrada queda «no disponible (sesiones paralelas)».
4. El reporte suma entradas; con olas, la persona corrige a mano («contando la ola una vez»).

## 5. Reglas de negocio: conocidas vs código
| Regla | Dónde vive hoy | Cambio |
|---|---|---|
| Una fila de Tiempos, una entrada | «Registro por tramo» | Varias entradas por fila (RF-3, RF-4) |
| Ventana común a sesión y subagentes | «Completar tokens» | Delegada sin ventana; chat por sesión; coordinación por resta (RF-5 a RF-8) |
| «no disponible (sesiones paralelas)» | «Completar tokens» | Solo para entradas viejas o sin id (RF-9) |
| Solo la sesión principal escribe | «Registro por tramo» | Sin cambio |
| Privacidad: solo métricas | «Privacidad» | Se añaden nombres de archivo e ids (sin contenido) |

## 6. Riesgos y casos borde
- R1. La sesión olvida el `agentId` entre el lanzamiento y la fila (cascada larga): entrada delegada
  «no disponible». Mitigación: la skill pide anotarlo al recibir el resultado del lanzamiento.
- R2. Subagente reanudado con `SendMessage`: mismo transcript en dos entradas; RF-5 parte por ventana.
- R3. Coordinación que se cruza con un paso en el chat: RF-8 da prioridad al chat.
- R4. Coordinator como subagente que lanza implementers (profundidad 2): el id llega solo si el
  coordinator lo devuelve; si no, «no disponible».
- R5. Otra herramienta sin ids: «no disponible» (RF-11, RF-12); el kit sigue genérico.
- R6. Archivo compartido: ninguna spec en curso toca la skill.

## 7. Preguntas abiertas
1. Resuelta (2026-11-22, decisión 8 de `05_avance.md`): el criterio 2 de la spec pasa a las cifras
   medidas, por `/sdd-change`.
2. Resuelta (2026-11-22, decisión 9): ADR [0040](../../004_adr/0040_BitacoraConTokensPorSubagenteYSesion.md),
   que reemplaza al 0038.
