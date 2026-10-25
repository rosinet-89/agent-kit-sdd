# ADR 009: Una fuente por fase del flujo SDD

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `003_OptimizarArnes`

## Contexto
El procedimiento de cada fase del flujo `sdd` estaba repetido en la skill, en el coordinator y en el agente del rol. Esas copias ya se habían desfasado (numeración de carpetas, cierre sin deuda ni ADR, tiempos copiados). El principio 3 de la constitución pide que cada regla viva en una sola fuente. El cambio es transversal: aplica a todo feature, no solo a esta carpeta.

## Decisión
El procedimiento de una fase vive solo en su comando `/sdd-<fase>`. La skill `sdd` asocia fase, documento y rol, y enlaza ese comando. El coordinator solo enruta. El agente del rol nombra y enlaza; no repite la secuencia.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Dejar la secuencia en la skill `sdd` | El comando y el agente seguían con otra copia, y ya se habían desfasado |
| Dejar la secuencia en el coordinator | Enrutar y ejecutar el procedimiento son trabajos distintos; la copia del coordinator ya omitía pasos del comando |

## Consecuencias
Cambiar una fase es editar su comando. La skill, el coordinator y el agente no se reescriben con la secuencia. `AGENTS.md` › "Qué lee cada rol" nombra secciones; no es otra copia del procedimiento. La deuda 003 (modelo por rol y permisos del frontmatter) queda fuera de esta decisión.
