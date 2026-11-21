# 05 — Avance: Cierre en una corrida con sus commits
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo`, sin flujo (decisión 4). Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): cumple la 1 (3 archivos sin réplicas ni publicación), la 2, la 5 y la 7; no cumple la 3 (cambia la leyenda, un contrato entre piezas), la 4 (ADR 0031), la 6 (cambia qué hace `/sdd-cierre`) ni la 8 (toca `commands` y el índice de specs) |
| Última sesión | 2026-11-21 15:57 |
| Fase actual | `cierre`: `Cerrado` en una corrida con sus 3 commits (decisión 7) |
| Último paso cerrado | Cierre (decisión 7) |
| Siguiente paso | Ninguno en esta spec |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | Kit 0.11.0 en 3 commits locales, sin push |

## Pasos
Detalle en `04_tasks.md`. Casos: `07_casos_prueba.md`.

| Fase | Pasos | Estado |
|---|---|---|
| F1 Leyenda y comando | F1.1 a F1.2 | `[x]` |
| F2 Publicación | F2.1 a F2.2 | `[x]` |

## Decisiones
1. **[2026-11-21 15:53] Origen y aprobación en cascada.** Tras cerrar la 0014 con la regla actual forzada (decisión 27 de su avance), la persona pide analizar el cierre que «pide el commit a fuerza» y, ante la propuesta (cierre en una corrida que baja a `Implementado` sin confirmación), dice «resuélvelo de una vez». Se toma como visto bueno para spec, plan y tareas en cascada; los commits siguen pidiendo su confirmación. Antecedentes: `0012_EstatusDeSpecYCierre` (leyenda y cierre), ADR 0024 y 0027. Deuda relacionada: 0013 (mismo archivo, otros puntos; queda abierta) y 0012 (no la toca).
2. **[2026-11-21 15:54] Clarify, pasada 1 de 3** (reviewer en el chat, modo automático): 2 hallazgos. (a) Contradicción: RF-1 escribía `Cerrado` antes de decidir el criterio 2, contra la frase de los tres resultados de la leyenda; RF-7 ahora exige reescribir esa frase. (b) Caso límite: sesión cortada entre escribir `Cerrado` y confirmar; la siguiente corrida lo trata como no confirmado. Pasada 2 de 3: sin hallazgos. Spec `aprobada` por la decisión 1.
3. **[2026-11-21 15:55] ADR 0031** (`El cierre decide el criterio 2 con sus commits en una corrida`), aceptado con la decisión 1.
4. **[2026-11-21 15:55] Sin flujo:** cambia el kit y no hay operación de usuario final; mismo criterio que la persona dio en la 0014 (decisión 25 de su avance). Supuesto explícito: la persona puede pedir el flujo.
5. **[2026-11-21 15:56] Versión 0.11.0** y no un parche de 0.10.0: 0.10.0 ya está en commit y su manifiesto congelado; esto cambia el comportamiento de `/sdd-cierre`. Proyectos actualizados copian la leyenda a mano (como en 0.8.0).
6. **[2026-11-21 15:57] Validate aprobado** (reviewer en el chat, claude-opus-5-5). Evidencia: RF-1 (punto 8 y orden del cierre), RF-2 a RF-6 (las viñetas del punto 16), RF-7 (criterio 2 y tres resultados en las dos leyendas; CP-02 vacío); manifiesto 0.11.0 coincide con `template/`; réplica igual; lint y rutas OK; seguridad: solo Markdown, modo «solo secretos», sin hallazgos. Observación opcional: si un cierre baja de `Cerrado` a `Implementado` en el punto 16, las deudas que el punto 14 ya pasó a `Resuelta` no vuelven atrás; riesgo bajo, sin deuda propuesta.
7. **[2026-11-21 15:57] Cierre con la regla nueva: `Cerrado`.** Previa OK: 4 pasos `[x]`, sin verificaciones manuales pendientes (CP-03 es este mismo cierre), sin flujo por el modo, sin pausa y validate aprobado. Criterios 1 (fila `cierre` y plan contra real), 3 (sin ticket ni aviso), 4 (pendiente vacío) y 5 (ninguna deuda con Atendida por 0016) cumplen. El criterio 2 se decide con los 3 commits propuestos en esta corrida: confirmados, queda `Cerrado`; sin confirmación, esta misma corrida baja a `Implementado` y reescribe esta decisión. Sin deuda nueva; `MEMORY.md`: sin propuesta nueva.

## Pruebas
Corridas y resultados: [07_casos_prueba.md](07_casos_prueba.md) › «Corridas». CP-03: este cierre (decisión 7).

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-21 15:53 | 2026-11-21 15:54 | 1 | 0 | rol planner en el chat (claude-opus-5-5); origen en la 0014 |
| clarify | 2026-11-21 15:54 | 2026-11-21 15:54 | 0 | 0 | pasada 1 de 3: 2 hallazgos, corregidos por el planner (modo automático) |
| clarify | 2026-11-21 15:54 | 2026-11-21 15:55 | 1 | 0 | pasada 2 de 3: sin hallazgos; reviewer en el chat (claude-opus-5-5) |
| plan | 2026-11-21 15:55 | 2026-11-21 15:55 | 0 | 0 | `02_analisis_impacto.md`, `03_plan.md` y ADR 0031; planner en el chat |
| tasks | 2026-11-21 15:55 | 2026-11-21 15:55 | 0 | 0 | `04_tasks.md` (4 pasos, 10 min) y `07_casos_prueba.md` (3 casos) |
| F1 | 2026-11-21 15:55 | 2026-11-21 15:55 | 0 | 0 | Leyenda en las dos copias, `sdd-cierre.md` y réplica; la sesión (claude-opus-5-5) |
| F2 | 2026-11-21 15:56 | 2026-11-21 15:56 | 0 | 0 | 0.11.0, changelog, manifiesto (humo con las tres huellas), ensayo de instalación y lint |
| validate | 2026-11-21 15:56 | 2026-11-21 15:57 | 1 | 0 | APROBADO en la vuelta 1; reviewer en el chat (claude-opus-5-5); CP-01 y CP-02 ok |
| cierre | 2026-11-21 15:57 | 2026-11-21 15:57 | 0 | 0 | la sesión (claude-opus-5-5); regla nueva: `Cerrado` y 3 commits propuestos con una sola confirmación (decisión 7) |

## Plan contra real

El real es la suma de la columna Minutos (el reloj del sistema marcó minutos cortos en esta corrida). Documentación SDD = `spec`, dos pasadas de `clarify`, `plan`, `tasks`, `validate` y `cierre` (3 min).

| Fase | Estimado | Real | Delta |
|---|---|---|---|
| Documentación SDD | sin estimado | 3 min | — |
| F1 | 5 min | 0 min | −5 min |
| F2 | 5 min | 0 min | −5 min |

Qué ajustar en el siguiente plan: nada nuevo; el estimado por lote de la 0013 y la 0014 se aplicó.
