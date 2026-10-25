# Memoria del proyecto — agent-kit-sdd

Decisiones duraderas y trampas vigentes que un agente no puede deducir del código.
Los agentes **proponen** entradas; solo se agregan con el visto bueno del usuario.
Una entrada = una línea de índice + (si hace falta) un archivo aparte con el detalle.

## Decisiones

- [2026-10-24] La raíz es el entorno de mantenimiento del kit: se versiona su conocimiento propio
  (`AGENTS.md`, `CLAUDE.md`, `MEMORY.md`, `.docs/`, `.agents/napkin.md`) y lo que sale de `template/` queda
  local en `.git/info/exclude`. Detalle y alternativas: `.docs/004_adr/001_ModoMantenimientoDelKit.md`.

## Trampas conocidas

- [2026-10-25] Un archivo que termina en CR sin LF no entra en la prueba de humo: `sed` lo quita y el normalizador de Python lo conserva.
- [2026-10-25] Una fase del plan llamada Cierre no ejecuta `/sdd-cierre`.
- [2026-10-25] El conteo de un análisis queda viejo si `template/` cambia antes de congelar el manifiesto; manda la medición.

## Referencias externas

- Remoto: https://github.com/rosinet-89/agent-kit-sdd
