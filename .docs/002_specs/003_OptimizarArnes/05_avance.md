# 05 — Avance: Optimizar el arnés SDD (agentes y comandos)
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 13:42 |
| Fase actual | Implementación en pausa; `F0.1` cerrado; `F1.1` abierto |
| Último paso cerrado | `F0.1` línea base copiada en este avance, sin tocar `template/` |
| Siguiente paso | Esperar al usuario. Otro agente commitea los cambios ya hechos. Al volver, cerrar `F1.1` con el resultado de ese commit y no abrir `F2` antes |
| Bloqueos abiertos | Ninguno |
| Coordinación | Corlos edita `AGENTS.md`, `template/.agents/commands/` y otros archivos que este feature también tocaría (26 archivos modificados sin commit al abrir la carpeta). Antes de implementar, acordar con el usuario quién commitea 001 y 002 y partir del contenido actual de esos archivos, sin revertirlos. |
| Traspaso (Claudio → siguiente agente) | La spec está aprobada; `02` a `06` no existen. **Antes de `/sdd-plan`:** (1) acordar con el usuario quién commitea las carpetas `001`, `002` y `003`, las deudas, los ADR 007/008 y los cambios de `template/` (decisión 5; hay decenas de archivos sin commit). (2) La spec exige fijar la línea base de lectura en este `05_avance.md` ANTES de cambiar nada (definición de línea base y pasos de referencia: mínimo, mediana y máximo de `001_ActualizarKit/04_tasks.md`). (3) La deuda del modelo por rol y los permisos del frontmatter ya está dada de alta (`.docs/003_deuda/003_ModeloYPermisosDeAgentes.md`). (4) Partir del contenido actual de `AGENTS.md`, `template/.agents/commands/` y `validar-estructura.md` (otro agente los editó); no revertirlos. (5) Orden de cierre: cambios en `template/`, manifiesto de 0.5.0, E20, contraste 0.4.0 vs 0.5.0, chequeos de Comandos y medición. Decisiones 1 a 13 vigentes. |
| Estado BD | No aplica |
| Código | Ninguno |

## Pasos
- [x] F0.1 línea base en este avance
- [ ] F1.1 confirmación de coordinación, antes de editar

## Decisiones
1. **[2026-10-25 12:53] Se abre esta spec a partir de la revisión de los 4 agentes y 14 comandos del `template/`.** Origen: petición del usuario de reducir consumo de tokens sin bajar la calidad (lecturas innecesarias, pasos repetidos o asumidos, trabajo fuera de su rol). Alternativa descartada: aplicar los cambios directo sin spec (contradice el principio 2 de la constitución).
2. **[2026-10-25 12:58] Alcance: modelo por rol y permisos del frontmatter quedan fuera**; se darán de alta como deuda tras verificar cómo trata Claude Code `mode`/`permissions`. Alternativas descartadas: incluirlos (más riesgo de regresión y de choque con Corlos) y solo el permiso del implementer.
3. **[2026-10-25 12:58] Medición de calidad:** tabla de bytes antes/después sobre `001_ActualizarKit` (al menos 70 % menos de lectura por paso del implementer y por `/sdd-status`) más lista de verificaciones existentes con la pieza donde sigue cada una. Alternativas descartadas: re-ejecutar un feature completo (no reproducible) y sin objetivo numérico.
4. **[2026-10-25 12:58] Versión: este feature sale dentro de 0.5.0** (sin publicar); al cerrar se regenera el manifiesto de 0.5.0. Alternativa descartada: 0.5.1.
5. **[2026-10-25 12:58] Antes de implementar se acuerda con Corlos quién commitea 001 y 002**; se parte del contenido actual sin revertir trabajo ajeno. Alternativas descartadas: que lo haga Claudio o implementar sobre el árbol sucio.
6. **[2026-10-25 12:58] Tiempos: se prohíbe el lote con "0 minutos"**, salvo un paso marcado "sin trabajo real". Alternativas descartadas: sin excepción y permitir el lote.
7. **[2026-10-25 13:04] Clarify: 25 hallazgos del reviewer (11 ALTA); veredicto CAMBIOS NECESARIOS.** Corrección propia: la spec tiene 24 RF distintos, no 26 (conté líneas, no ids).
8. **[2026-10-25 13:04] Tiempos:** la prohibición del lote con "0 minutos" aplica solo a features abiertos desde esta versión (los existentes siguen válidos); mínimo 1 minuto por paso real; "sin trabajo real" se marca con esa frase en la columna Nota. Alternativas descartadas: solo advertir y prohibir también en los existentes.
9. **[2026-10-25 13:04] Herencia de `AGENTS.md`:** no se supone que la herramienta la inyecte; cada subagente lee solo las secciones que usa. Alternativas descartadas: suponer herencia y verificar después, y no tocar la lectura.
10. **[2026-10-25 13:04] Los cambios en `AGENTS.md` llegan a los proyectos por fusión manual** (grupo B); se actualizan `template/AGENTS.md` y el de la raíz sin divergir. Alternativa descartada: sacar esos cambios de `AGENTS.md`.
11. **[2026-10-25 13:04] `/sdd-estructura` lo ejecuta la sesión principal, sin subagente;** el reviewer conserva `validate`. Alternativa descartada: mantener el reviewer.
12. **[2026-10-25 13:07] Spec aprobada por el usuario** (`Estado: aprobada`; 24 RF, 6 RNF), incluidas las 6 decisiones del planner (documentos de menos de 4 KB se leen enteros; tope de `/sdd-init` y `/sdd-constitucion` de 30 archivos o 150 KB; modo "solo secretos" en `seguridad-basica`; sin meta del 70 % para esos dos comandos; 70 % en el paso de mediana del implementer; la lectura cuenta también el texto del agente y del comando).
13. **[2026-10-25 13:07] No se invoca `/sdd-plan` en esta sesión:** el usuario cambia de agente para la siguiente fase. Alternativa descartada: seguir con el plan ahora.
14. **[2026-10-25 13:09] Se da de alta la deuda `003_ModeloYPermisosDeAgentes`** (modelo por rol y permisos del frontmatter), con el visto bueno del usuario. Resuelve el pendiente de la decisión 2.
15. **[2026-10-25 13:28] Plan redactado.** Pasos de referencia: menor `F6.2` (mínima 834, línea base 92 280), mediana `F5.1` (15 181 / 89 004), mayor `F1.5` (21 248 / 89 004). `/sdd-status` 122 731 bytes. La deuda 003 ya estaba abierta; este plan no la crea. La fuente única por fase queda marcada para ADR al cierre, sin archivo. F1 sigue esperando la confirmación de coordinación.
16. **[2026-10-25 13:37] `04_tasks.md` entregado: 37 pasos, 4,0 h, RF-1 a RF-24 cubiertos, casillas abiertas.** Plan y tareas siguen sin el visto bueno. No se implementa.
17. **[2026-10-25 13:38] El usuario aprobó el plan y las tareas.** Siguiente paso: `/sdd-implement 003_OptimizarArnes F0.1`. F1 sigue sin la confirmación de coordinación.
18. **[2026-10-25 13:39] F0.1 cerrado.** La línea base de `02` §3 quedó copiada en Pruebas, sin recalcular. `template/` y los docs de la raíz no se tocaron en este paso.
19. **[2026-10-25 13:42] El usuario pausa F1.1.** Otro agente commitea los cambios que ya están. Esta sesión no commitea, no abre F2 y no edita `template/` ni los docs de ese commit. Al volver se cierra F1.1 con quién commiteó y qué quedó fuera. (a) y (b) siguen sin confirmar.

## Pruebas
- **F0.1 (2026-10-25 13:39).** Línea base copiada de `02_analisis_impacto.md` §3, sin recalcular. Origen: `wc -c` del 2026-10-25. El `AGENTS.md` de la raíz (8 667) no entra.

### 3.1 Piezas

| Archivo | Bytes |
|---|---:|
| `template/.agents/agents/coordinator.md` | 6 679 |
| `template/.agents/agents/planner.md` | 3 510 |
| `template/.agents/agents/implementer.md` | 2 074 |
| `template/.agents/agents/reviewer.md` | 3 175 |
| `template/.agents/commands/sdd.md` | 1 361 |
| `template/.agents/commands/sdd-spec.md` | 1 630 |
| `template/.agents/commands/sdd-clarify.md` | 924 |
| `template/.agents/commands/sdd-plan.md` | 1 350 |
| `template/.agents/commands/sdd-tasks.md` | 903 |
| `template/.agents/commands/sdd-implement.md` | 1 142 |
| `template/.agents/commands/sdd-validate.md` | 1 064 |
| `template/.agents/commands/sdd-flujo.md` | 884 |
| `template/.agents/commands/sdd-cierre.md` | 1 614 |
| `template/.agents/commands/sdd-change.md` | 783 |
| `template/.agents/commands/sdd-status.md` | 658 |
| `template/.agents/commands/sdd-constitucion.md` | 1 254 |
| `template/.agents/commands/sdd-init.md` | 3 013 |
| `template/.agents/commands/sdd-estructura.md` | 900 |
| `template/AGENTS.md` | 7 798 |
| `template/.agents/skills/sdd/SKILL.md` | 8 551 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | 4 907 |
| `template/.agents/skills/sdd/plantillas.md` | 3 970 |
| `template/.agents/skills/seguridad-basica/SKILL.md` | 3 276 |
| `.docs/001_constitucion/constitution.md` | 1 111 |

Secciones de `template/AGENTS.md` que el implementer usa hoy: **505**.

### 3.2 Caso congelado `001_ActualizarKit`

| Archivo | Bytes |
|---|---:|
| `.docs/002_specs/001_ActualizarKit/01_spec.md` | 22 394 |
| `.docs/002_specs/001_ActualizarKit/02_analisis_impacto.md` | 13 954 |
| `.docs/002_specs/001_ActualizarKit/03_plan.md` | 16 330 |
| `.docs/002_specs/001_ActualizarKit/04_tasks.md` | 29 604 |
| `.docs/002_specs/001_ActualizarKit/05_avance.md` | 24 928 |
| `.docs/002_specs/001_ActualizarKit/06_flujo.md` | 13 796 |
| `MEMORY.md` | 1 067 |

Secciones de ese avance: PUNTO DE RETOMO 2 478, Pasos 45, Pruebas 11 541, Tiempos 4 234. Línea `Estado` 21.

### 3.3 Tres pasos de referencia de `001` y `F5.9`

| Paso de `001` | Mínima | Línea base |
|---|---:|---:|
| `F6.2` (menor) | 834 | 92 280 |
| `F5.1` (mediana, posición 16 de 32) | 15 181 | 89 004 |
| `F1.5` (mayor) | 21 248 | 89 004 |
| `F5.9` (no es paso de referencia; sin RF y sin sección de plan) | 1 452 | — |

### 3.4 Línea base del implementer

| Paso de `001` | Skill de dominio | Línea base |
|---|---|---:|
| `F6.2` | `seguridad-basica` (3 276) | 92 280 |
| `F5.1` | ninguna | 89 004 |
| `F1.5` | ninguna | 89 004 |

Tope de la mediana: 26 701. Quedan 11 520 para el texto final de `implementer.md` más `sdd-implement.md`.

### 3.5 `/sdd-status`

658 + 22 394 + 13 954 + 16 330 + 29 604 + 24 928 + 13 796 + 1 067 = **122 731**. Tope: 36 819.

### 3.6 Arranque

| Pieza | Bytes |
|---|---:|
| `coordinator` | 34 533 |
| `planner` | 35 063 |
| `reviewer` | 75 284 |
| `/sdd` | 21 498 |
| `/sdd-spec` | 21 686 |
| `/sdd-implement` (solo el comando) | 64 536 |
| `/sdd-estructura` | 12 194 |
| `/sdd-constitucion` | 47 364 |
| `/sdd-init` | 11 564 |
| `/sdd-plan` | 1 350 |
| `/sdd-tasks` | 903 |
| `/sdd-clarify` | 924 |
| `/sdd-validate` | 1 064 |
| `/sdd-flujo` | 884 |
| `/sdd-cierre` | 1 614 |
| `/sdd-change` | 783 |

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 12:53 | 2026-10-25 12:58 | 5 | 0 | planner (24 RF, 6 RNF), 5 dudas respondidas y ajustes |
| clarify | 2026-10-25 12:58 | 2026-10-25 13:07 | 9 | 0 | reviewer (25 hallazgos), 4 decisiones, planner ajusta la spec y aprobación |
| plan | 2026-10-25 13:09 | 2026-10-25 13:28 | 19 | 0 | planner midió con wc -c y escribió 02 y 03 |
| tasks | 2026-10-25 13:30 | 2026-10-25 13:38 | 8 | 0 | 04_tasks.md entregado; el usuario aprobó plan y tareas a las 13:38 |
| F0.1 | 2026-10-25 13:39 | 2026-10-25 13:39 | 1 | 0 | línea base copiada; el reloj no cambió de minuto; 1 por el mínimo de RF-22 |

## Plan contra real
(Al cierre.)
