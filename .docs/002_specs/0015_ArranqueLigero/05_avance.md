# 05 — Avance: Arranque ligero de sesión
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo`. Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): solo cumple la 5 (sin datos ni esquema); fallan 1, 2, 3, 4, 6, 7 y 8 |
| Última sesión | 2026-11-21 11:00 |
| Fase actual | `clarify` (cerrada) |
| Último paso cerrado | `clarify`: spec aprobada el 2026-11-21 11:00 (pasada 1 de 3) |
| Siguiente paso | `/sdd-plan 0015_ArranqueLigero` (la implementación espera a la 0013) |
| Bloqueos abiertos | Ninguno para el plan. Implementar después de `0013_ModelosPorAgente` |
| Estado BD | No aplica |
| Código | Ninguno |

## Pasos
(se llena en `tasks`)

## Decisiones
1. **[2026-11-21 10:36] Origen y alcance:** la persona pidió revisar el contexto que se carga al inicio de cada sesión. La medición de la primera llamada de la sesión (transcript `.jsonl`) dio ~56k tokens de entrada, de ellos ~26k de sistema y herramientas no controlables. Se aprobaron dos frentes para esta spec: (#5) `AGENTS.md` siempre cargado (13 314 bytes, ~3.8k tokens) y (#6) el protocolo «Cada sesión» (~9k tokens si se cumple completo: `.agents/sync-agents.md` 10 689 bytes, `PREFERENCIAS.md` 7 194, `.agents/README.md` 4 927, `MEMORY.md` 3 415, `formato-respuesta` 2 443, `napkin.md` 1 579, constitución 1 133). Fuera de esta spec, ya resueltos en la sesión: plugin caveman duplicado (desinstalado) y conectores MCP (la persona no los quiere quitar). Antecedente: `003_OptimizarArnes` (cerrada; redujo tokens de agentes y comandos, no el arranque de la sesión principal). Deuda abierta relacionada: ninguna.
2. **[2026-11-21 10:48] Respuestas a las 5 preguntas del planner:** (1) salen de `AGENTS.md` solo las secciones genéricas: «Qué lee cada rol»; «Formato y numeración», «Deuda técnica» y «Roles y comandos» se evalúan en el plan; «Comandos», «Stack» y «Reglas de negocio» se quedan. Descartadas: sacar todo salvo Guardrails y «Cada sesión», o solo «Qué lee cada rol». (2) S-1 fija en ≥35 %; el porcentaje de S-2 se fija en el plan. (3) Se implementa después de la 0013; clarify y plan pueden ir antes. (4) Proyectos ya instalados: el nuevo «Cada sesión» se incorpora a mano según `ACTUALIZAR.md` (ADR 007). Descartado: que `/sdd-enlaces` lo proponga. (5) La 0014 sigue el criterio de RF-1; anotado en su avance.
3. **[2026-11-21 11:00] Spec aprobada** por la persona tras la pasada 1 de clarify (rol reviewer en el chat): 10 hallazgos corregidos por el rol planner en el chat (método y escenario de medición, RF-6, RF-7, RF-9 con renglón de remisión, línea base de réplicas, tres casos límite y ensayo de actualización).

## Pruebas
(se llena en `validate`)

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-21 10:36 | 2026-11-21 10:40 | 4 | 0 | Borrador de `01_spec.md` por el planner; 5 preguntas abiertas |
| clarify | 2026-11-21 10:51 | 2026-11-21 11:00 | 9 | 0 | pasada 1 de 3; rol reviewer y ajuste rol planner en el chat; 10 hallazgos corregidos; spec aprobada |

## Plan contra real
(se llena al cierre)
