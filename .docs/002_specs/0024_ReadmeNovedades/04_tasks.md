# 04 — Tareas: README con la versión actual y sus novedades
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## Plan
- Archivos: `README.md`. El índice de specs (`.docs/002_specs/README.md`) se toca solo como registro
  del flujo (fila `0024`).
- Fuentes que se leen para resumir, sin copiar reglas: `VersionAgentKitSdd`,
  `ChangelogAgentKitSdd.md` (entradas 0.17.0 y 0.18.0), ADR 0039 y 0040,
  `template/.agents/skills/sdd/avance-y-tiempos.md` › «Pasos con tiempos» y
  `template/.agents/skills/bitacora-uso/SKILL.md`.
- Decisiones: supuestos 1 a 3 de la spec. Sin bump de versión, así que no hay tarea de changelog.
- Estimado: 15 min (F1).

## F1 — README (estimado: 15 min)
- [x] **F1.1 Versión, «Novedades» y filas de `05_avance.md` y bitácora en `README.md`.** RF-1 a RF-4
  - Hecho cuando:
    - [x] Línea de versión bajo la descripción inicial = `VersionAgentKitSdd`, con enlace al changelog
    - [x] Sección «Novedades» antes de «Flujo general»: 0.18.0 y luego 0.17.0, ≤ 3 líneas de fuente
      (≤ 100 caracteres) cada una, con spec, ADR y enlace a la skill de `template/`; cierra con el
      enlace al changelog
    - [x] Fila `05_avance.md` (tabla Pasos con estimado, real y delta en vivo) y atajo de
      `/sdd-status` con las horas
    - [x] Fila «Bitácora de uso (beta)» con tokens por subagente y por sesión, coordinación aparte
    - [x] Ninguna regla redefinida: solo resume y remite
    - [x] Lint de `AGENTS.md` › Comandos OK en `README.md` (UTF-8 sin BOM, rutas citadas existentes)
    - [x] Verificación manual UI: la persona ve la versión y «Novedades» legibles en su visor
