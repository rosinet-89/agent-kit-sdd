# 002 — Versión 0.5.0 sin spec, plan ni avance

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-10-24 |
| Origen | Hallazgo: el usuario detectó que los cambios de 0.5.0 no pasaron por el flujo `sdd` |
| Módulo | Proceso de mantenimiento del kit |
| Prioridad | Media |
| Atendida por | `002_Kit050` |

## Descripción
Las skills `adr`, `migracion-bd` y `seguridad-basica`, `/sdd-estructura` con las réplicas de skills, los modos de `INSTALAR.md` y el versionado del conocimiento propio de la raíz se implementaron sin carpeta en `.docs/002_specs/`, sin `05_avance.md` ni tiempos reales. Contradice el principio 2 de la constitución.

## Por qué quedó pendiente
El trabajo se hizo de forma conversacional y se commiteó antes de detectar la omisión.

## Impacto si no se atiende
Sin spec ni tiempos, no hay evidencia de los RF de esos cambios ni referencia de horas para cotizar el próximo ajuste del kit. Las decisiones sí quedaron en los ADR 001 a 006 y en `CHANGELOG.md`.

## Alcance (archivos, tablas)
Commits `b13e406` a `1c35f0c`; `.docs/004_adr/`; `CHANGELOG.md` (0.5.0).

## Cómo se atendería (propuesta)
Opciones: (a) dejar los ADR y el changelog como registro suficiente y descartar la deuda con motivo; (b) escribir una spec retroactiva `NNN_Kit050` solo con RF y criterios, sin inventar tiempos. Decide el usuario.

## Cómo se atendió (al resolver: qué se hizo y dónde)
Se eligió la opción (b): spec retroactiva en `.docs/002_specs/002_Kit050/` con RF y criterios
(`01_spec.md`, estado `implementada`) y `05_avance.md`. No se inventaron los tiempos de los
commits `b13e406` a `1c35f0c` ni se escribieron análisis, plan, tareas ni flujo. No se modificó
código, changelog, versión ni ADR. La opción (a), descartar la deuda, no se usó.

## Historial
- 2026-10-24 — Alta.
- 2026-10-25 — Resuelta con `002_Kit050` (opción b).
