# 03 — Plan: Flujo eficiente
Spec: 01_spec.md (aprobada 2026-11-22 15:45)

## 1. Ubicación y capas
- `template/` (se distribuye): skill `sdd`, comandos, `implementer.md` y `template/AGENTS.md`.
- Docs de la raíz: `README.md`, versión, changelog y manifiesto.
- Entorno de mantenimiento: `AGENTS.md` de la raíz (RF-16) y la réplica `.agents/`.
- Cada regla vive en una fuente (RNF-1); el resto remite.

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/sdd/SKILL.md` | template | Comprobación previa y lista cerrada; tabla de fases | 1, 2, 5 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | template | Motivo en la Nota; fila de lote | 3, 10 |
| `template/.agents/commands/sdd-tasks.md` | template | Sin estructura delegada; siguiente paso | 4, 5 |
| `template/.agents/commands/sdd-implement.md` | template | Cascada, lotes, olas y paros | 5 a 9 |
| `template/.agents/agents/implementer.md` | template | Paso o lote | 7 |
| `template/.agents/skills/sdd/topes-y-modos.md` | template | Autochequeo y delta | 11 a 14 |
| `template/.agents/commands/sdd-clarify.md` | template | Remisión | 11, 12 |
| `template/.agents/commands/sdd-cierre.md` | template | Chequeos de cierre en la previa | 15 |
| `template/AGENTS.md` | template | Línea de chequeos de cierre | 15 |
| `template/.agents/commands/sdd-init.md` | template | Propone esa línea | 15 |
| `AGENTS.md` | raíz | E20 como chequeo de cierre | 16 |
| `README.md` | raíz | Cascada en el diagrama, el ejemplo y las tablas | 5 |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.14.0.md` | raíz | 0.14.0 | RNF-3 |
| `.docs/004_adr/0036_CascadaConLotesEnImplement.md` + índice | raíz | ADR (si se aprueba) | 5, 7 |

## 3. Base de datos
No aplica.

## 4. Seguridad
- Guardrails sin cambio (RNF-2).
- La cascada no hace commits ni ejecuta `sql/`: se detiene en lo ajeno (RF-9).

## 5. Liberación / piloto
No aplica. Los umbrales de S-2 se revisan tras 3 usos.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| `/sdd-implement <feature>` sin paso = cascada de todo (S-1) | Comando nuevo `/sdd-cascada` | Un comando menos que aprender; la forma de un paso no cambia. [ADR 0036](../../004_adr/0036_CascadaConLotesEnImplement.md) |
| Lotes de hasta 4 pasos (S-2) | Un implementer por paso; lotes sin tope | 0013 pagó un arranque en frío por paso; sin tope, un fallo cuesta más de revisar |
| Enganche genérico de chequeos de cierre (S-3) | E20 dentro de `sdd-cierre` | Constitución, principio 1: `template/` no lleva reglas del repo del kit |
| Pasadas por delta desde la 2 | Todas completas | La pasada 1 ya fue completa; RF-13 cubre lo que el delta podría perder |

## 7. Fases y estimado
Estimado en minutos y por ola (`MEMORY.md`, 2026-11-22).

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Línea base: `/sdd-estructura` en la sesión, Git limpio en `template/` | — | 3 |
| F1 | Lanzamientos | 1 a 4 | 6 |
| F2 | Cascada y lotes | 5 a 10 | 10 |
| F3 | Clarify | 11 a 14 | 5 |
| F4 | Chequeos de cierre | 15, 16 | 6 |
| F5 | Réplica, versión, ADR y manifiesto | RNF-3 | 8 |
| | **Total** | | **38** |

## 8. Criterios de aceptación
Los cinco criterios de finalización de `01_spec.md`, comprobados con `07_casos_prueba.md`.
