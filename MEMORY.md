# Memoria del proyecto — agent-kit-sdd

Decisiones duraderas y trampas vigentes que un agente no puede deducir del código.
Los agentes **proponen** entradas; solo se agregan con el visto bueno del usuario.
Una entrada = una línea de índice + (si hace falta) un archivo aparte con el detalle.

## Decisiones

- [2026-10-24] La raíz es el entorno de mantenimiento del kit: se versiona su conocimiento propio
  (`AGENTS.md`, `CLAUDE.md`, `MEMORY.md`, `.docs/`, `.agents/napkin.md`) y lo que sale de `template/` queda
  local en `.git/info/exclude`. Detalle y alternativas: `.docs/004_adr/001_ModoMantenimientoDelKit.md`.
- [2026-10-25] En un feature que solo edita Markdown y lo ejecutan agentes, estima las fases en minutos; el real ronda un tercio del estimado. Un ítem nuevo conserva el ancho de su carpeta: `.docs/004_adr/014_ConvivenciaDeAnchosDeNumeracion.md`.

## Trampas conocidas

- [2026-10-25] Un archivo que termina en CR sin LF no entra en la prueba de humo: `sed` lo quita y el normalizador de Python lo conserva.
- [2026-10-25] Una fase del plan llamada Cierre no ejecuta `/sdd-cierre`.
- [2026-10-25] El conteo de un análisis queda viejo si `template/` cambia antes de congelar el manifiesto; manda la medición.
- [2026-10-25] Si no hay un solo paso, la sesión sigue «Si no hay un solo paso» en la skill `sdd` y no espera a que un subagente abra a otro. Detalle: `.docs/004_adr/010_RutaSiNoHayUnSoloPaso.md`.
- [2026-10-25] No hay revisión en la que F1.4 de `001_ActualizarKit` ya esté y F1.5 todavía no. Rehacer ese paso no prueba un modelo.
- [2026-10-25] El protocolo P de las tareas compara con copias hechas justo antes de editar y no delata ediciones ajenas previas: contrasta también con `git diff` contra `HEAD`.
- [2026-10-25] Reanudar un subagente con `SendMessage` a veces no lo arranca: confirma con `ListAgents` que figura como `running` antes de esperar.

## Referencias externas

- Remoto: https://github.com/rosinet-89/agent-kit-sdd
