# Constitución — agent-kit-sdd
Principios innegociables. Todo plan, cambio y revisión los cumple; si el código existente choca con uno, se consulta antes de tocarlo.

1. **Kit genérico:** `template/` no lleva stack, nombres ni dominio de ningún proyecto.
2. **El plan manda:** todo cambio al kit pasa por el flujo `sdd` con carpeta en `.docs/002_specs/` (versionada).
3. **Capas separadas:** `template/` (se distribuye), docs de la raíz (manual del kit) y entorno de mantenimiento de la raíz (conocimiento propio versionado; la parte copiada de `template/` es local). Ver ADR 001. Cada regla vive en una sola fuente.
4. **Verificado antes de cerrar:** los chequeos de `AGENTS.md` › Comandos pasan en cada archivo tocado y el ensayo de instalación no pisa nada.
5. **Datos y seguridad:** la copia nunca sobrescribe (salvo el reemplazo confirmado de archivos del kit sin personalizar al actualizar, ADR 007); sin secretos; sin scripts de instalación versionados.
6. **Codificación e idioma:** UTF-8 sin BOM, español, commits `<tipo>(<módulo>): …`, y `VersionAgentKitSdd` + `ChangelogAgentKitSdd.md` en cada versión.
