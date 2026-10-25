# 04 — Tareas: Ruta si no delega
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado
Spec: `01_spec.md` (aprobada 2026-10-25 15:37) · Plan: `03_plan.md` · Total estimado: 2,25 h

F0 a F6 están hechos. `/sdd-cierre` no es una fase ni un paso. El ADR de la fuente única se propone en ese cierre; este archivo no lo crea. No se edita `template/` hasta que F0 anote el texto del que se parte. No se tocan las carpetas `003`, `004` ni `005`.

## F0 — Partir del texto actual (estimado: 0,25 h)

- [x] **F0.1 Releer lo compartido.**
  - Hacer: leer el texto actual de `template/.agents/skills/sdd/SKILL.md`, `template/.agents/skills/sdd/avance-y-tiempos.md`, `template/.agents/agents/coordinator.md` y la última viñeta de `0.5.0` en `CHANGELOG.md`. Anotar en `05_avance.md` la hora y que se parte de ese texto, sin revertir líneas de la `004` ni de la `005`.
  - Hecho cuando:
    - [x] La nota está en las decisiones del avance, con hora
    - [x] No se editó `template/` en este paso

## F1 — Regla en la skill (estimado: 0,5 h)

- [x] **F1.1 Un solo paso y otra ruta.** RF-1, RF-2, RF-3, RF-4, RF-6, RF-7, RF-8, RF-11, RF-12
  - Hacer: en `template/.agents/skills/sdd/SKILL.md`, sustituir el párrafo que obliga a lanzar al coordinator y a no ejecutar las fases. La regla queda ahí: un solo paso usa la ruta diseñada; si no se puede o insertaría un segundo paso, la misma entrada sigue por la otra ruta; si el usuario pide la fase en el chat, esa entrada se hace en la sesión. La constancia se enlaza al contrato, no se copia. Cierre, estado y estructura siguen en la sesión principal. La entrada siguiente vuelve a la ruta diseñada, salvo un pedido nuevo.
  - Hecho cuando:
    - [x] El párrafo viejo ya no obliga el segundo paso sin salida
    - [x] La regla no nombra una herramienta ni un proyecto
    - [x] Lint L en `template/.agents/skills/sdd/SKILL.md`

## F2 — Constancia (estimado: 0,25 h)

- [x] **F2.1 Fila única.** RF-5, RF-9, RF-10
  - Hacer: en `template/.agents/skills/sdd/avance-y-tiempos.md`, la constancia es una decisión con hora (rol o pedido, porqué en una línea, ruta, quién hizo el trabajo). Una sola fila por fase. Los minutos del intento que no delegó entran en esa fila y la nota lo dice. Si el agente no devolvió texto, la línea la escribe la sesión con la causa que vio.
  - Hecho cuando:
    - [x] No se abre un segundo registro
    - [x] Lint L en `template/.agents/skills/sdd/avance-y-tiempos.md`

## F3 — Coordinator (estimado: 0,25 h)

- [x] **F3.1 Soltar la fase.** RF-2, RF-10
  - Hacer: en `template/.agents/agents/coordinator.md`, si no puede abrir al rol, devuelve una línea con el porqué y suelta la fase. No redacta `01`–`04` ni `06`. No reintenta. No se cambia el frontmatter (eso es la `004`).
  - Hecho cuando:
    - [x] El frontmatter de `coordinator.md` queda como estaba al empezar F3
    - [x] Lint L en `template/.agents/agents/coordinator.md`

## F4 — Enlace en los comandos (estimado: 0,5 h)

- [x] **F4.1 Una frase, nueve archivos.** RF-2, RF-11
  - Hacer: en `template/.agents/commands/sdd.md`, `sdd-spec.md`, `sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md` y `sdd-change.md`, una frase que enlaza la sección de la skill. No se copia la regla. `sdd-flujo.md` conserva el orden planner y luego reviewer.
  - Hecho cuando:
    - [x] Los nueve archivos tienen la frase y no el procedimiento
    - [x] `sdd-cierre.md`, `sdd-status.md` y `sdd-estructura.md` no se editaron
    - [x] Lint L en los nueve archivos

## F5 — Changelog (estimado: 0,25 h)

- [x] **F5.1 Viñeta de 0.5.0.** RF-1, RF-2
  - Hacer: en `CHANGELOG.md`, una viñeta en el bloque `0.5.0`. No se abre `0.5.1`. No se reescribe la viñeta que ya dice que el chat lanza al coordinator: se agrega la salida.
  - Hecho cuando:
    - [x] `VERSION` sigue en `0.5.0`
    - [x] Lint L en `CHANGELOG.md`

## F6 — Copia local (estimado: 0,25 h)

- [x] **F6.1 Refrescar solo esos archivos.** RF-1
  - Hacer: copiar desde `template/.agents/` hacia `.agents/` únicamente los archivos tocados en F1–F4. No copiar `napkin.md` ni el resto de `.agents/`. `.cursor/` no se edita: ya enlaza a `.agents/`.
  - Hecho cuando:
    - [x] Cada archivo copiado coincide con su par de `template/`
    - [x] `.agents/napkin.md` no cambió en este paso

## Lint L

Cuando un paso diga «Lint L», sobre cada archivo tocado: `file -i` da `utf-8` y `grep` del BOM sale vacío. En `template/` no hay marcadores `<<…>>` nuevos. No aplica verificación de interfaz ni de base de datos.
