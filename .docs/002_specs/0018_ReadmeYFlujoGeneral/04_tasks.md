# 04 — Tareas: README del repo con flujo general del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## Plan
- Archivos: `README.md` (reescritura) y `.docs/002_specs/README.md` (filas `0012`, `0015` y `0018`).
- Fuentes que se leen para resumir, sin copiar reglas: `INSTALAR.md`, `ACTUALIZAR.md`,
  `.agents/README.md` › «Roles y comandos», `template/.agents/commands/`, `template/.agents/agents/`,
  `template/.agents/*.ejemplo.md`, `template/.agents/commands/sdd-config.md`.
- Decisiones: supuestos 1 a 3 de la spec, aprobados (decisión 4 del avance). Sin bump de versión,
  así que no hay tarea de changelog.
- Estimado: 45 min (F1 10 min, F2 35 min). Cambio 2026-11-22 (RF-9): F2.2, +10 min.

## F1 — Índice (estimado: 10 min)
- [x] **F1.1 Quitar los enlaces a deudas resueltas de las filas `0012` y `0015`.** RF-8
  - Hecho cuando:
    - [x] La columna «Pendiente / deuda» de ambas filas queda vacía y el resto de la fila igual
    - [x] Lint de `AGENTS.md` › Comandos OK en `.docs/002_specs/README.md`
    - [x] Verificación manual UI: No aplica

## F2 — README (estimado: 35 min)
- [x] **F2.1 Reescribir `README.md`.** RF-1 a RF-7
  - Hecho cuando:
    - [x] Secciones: qué es, flujo general (Mermaid + lista), ciclo de un requerimiento (Mermaid),
      instalar, configurar (tabla + Mermaid), usar (ejemplo ≤ 15 líneas), actualizar, comandos y
      roles, preferencias, reglas del kit y licencia
    - [x] Cada sección remite a su fuente y no redefine reglas
    - [x] Comandos y roles listados = archivos de `template/.agents/commands/` y `agents/`
    - [x] Lint de `AGENTS.md` › Comandos OK en `README.md` (UTF-8 sin BOM, rutas citadas existentes)
    - [x] Verificación manual UI: los diagramas se ven en el visor de GitHub (la persona, al publicar). Confirmado por la persona en vista local, 2026-11-22 15:22
- [x] **F2.2 Árbol comentado del proyecto instalado en `README.md`.** RF-9 (cambio 2026-11-22)
  - Hecho cuando:
    - [x] Árbol con lo versionado y lo local marcado según `template/.gitignore`, y una carpeta de
      spec de ejemplo con `01`–`07` y `sql/`
    - [x] Lint de `AGENTS.md` › Comandos OK en `README.md`
    - [x] Verificación manual UI: la persona ve el árbol legible (decisión 10 del avance)
