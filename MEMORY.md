# Memoria del proyecto — agent-kit-sdd

Decisiones duraderas y trampas vigentes que un agente no puede deducir del código.
Los agentes **proponen** entradas; solo se agregan con el visto bueno del usuario.
Una entrada = una línea de índice + (si hace falta) un archivo aparte con el detalle.

## Decisiones

- [2026-10-24] La raíz es el entorno de mantenimiento del kit: se versiona su conocimiento propio
  (`AGENTS.md`, `CLAUDE.md`, `MEMORY.md`, `.docs/`, `.agents/napkin.md`) y lo que sale de `template/` queda
  local en `.git/info/exclude`. Detalle y alternativas: `.docs/004_adr/001_ModoMantenimientoDelKit.md`.
- [2026-10-25] En un feature que solo edita Markdown y lo ejecutan agentes, estima las fases en minutos; el real ronda un tercio del estimado. Un ítem nuevo conserva el ancho de su carpeta: `.docs/004_adr/014_ConvivenciaDeAnchosDeNumeracion.md`.
- [2026-10-25] Un implementer por paso no baja el tiempo a un tercio: en `007_DeudasAbiertas` el real de F0 a F6 fue 84 min contra 85 estimados. El tercio de la línea anterior vale cuando una persona edita el Markdown en una sentada.
- [2026-11-16] La leyenda de estatus de una spec se enuncia una sola vez en `.docs/002_specs/README.md` y la definición de deuda en `.docs/003_deuda/README.md`; comandos, skill, plantilla y `AGENTS.md` remiten y no copian. Detalle y alternativas: `.docs/004_adr/0024_LeyendaDeEstatusEnElIndice.md`.

## Trampas conocidas

- [2026-10-25] Un archivo que termina en CR sin LF no entra en la prueba de humo: `sed` lo quita y el normalizador de Python lo conserva.
- [2026-10-25] Una fase del plan llamada Cierre no ejecuta `/sdd-cierre`.
- [2026-10-25] El conteo de un análisis queda viejo si `template/` cambia antes de congelar el manifiesto; manda la medición.
- [2026-10-25] Si no hay un solo paso, la sesión sigue «Si no hay un solo paso» en la skill `sdd` y no espera a que un subagente abra a otro. Detalle: `.docs/004_adr/010_RutaSiNoHayUnSoloPaso.md`.
- [2026-10-25] No hay revisión en la que F1.4 de `001_ActualizarKit` ya esté y F1.5 todavía no. Rehacer ese paso no prueba un modelo.
- [2026-10-25] El protocolo P de las tareas compara con copias hechas justo antes de editar y no delata ediciones ajenas previas: contrasta también con `git diff` contra `HEAD`.
- [2026-10-25] Reanudar un subagente con `SendMessage` a veces no lo arranca: confirma con `ListAgents` que figura como `running` antes de esperar.
- [2026-11-03] `.agents/` de la raíz está fuera de Git (`.git/info/exclude`): un cambio en `template/.agents` se replica con `cp` y se comprueba con `diff -rq template/.agents .agents`, que admite `napkin.md`, `skills/caveman` y, desde 2026-11-16, `agents/claudio.md` (diferencia previa que la persona aceptó para la línea base). `git status` no lo muestra.
- [2026-11-03] Un `awk` que busca la primera línea no vacía falla con CRLF: la línea en blanco es `\r` y cuenta como no vacía. Quita el `\r` con `sed` antes de `awk`.
- [2026-11-16] Antes de repartir un cambio entre varios agentes y revisores, define la fuente única de cada regla: en `0012_EstatusDeSpecYCierre` dos revisores con criterios cruzados (uno pidió la lista de archivos del criterio 2, otro pidió quitar copias) duplicaron texto y costaron tres rondas de corrección. Una regla se define una vez y todo lo demás remite.

## Referencias externas

- Remoto: https://github.com/rosinet-89/agent-kit-sdd
