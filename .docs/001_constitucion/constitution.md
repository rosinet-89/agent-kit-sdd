# Constitución — agent-kit-sdd
Principios innegociables. Todo plan, cambio y revisión los cumple; si el código existente choca con uno, se consulta antes de tocarlo.

1. **Kit genérico:** `template/` no lleva stack, nombres ni dominio de ningún proyecto.
2. **El plan manda:** todo cambio al kit pasa por el flujo `sdd` con carpeta en `.docs/002_specs/` (local).
3. **Capas separadas:** `template/` (se distribuye), docs de la raíz (manual del kit) e instalación en la raíz (local). Cada regla vive en una sola fuente.
4. **Verificado antes de cerrar:** los chequeos de `AGENTS.md` › Comandos pasan en cada archivo tocado y el ensayo de instalación no pisa nada.
5. **Datos y seguridad:** la copia nunca sobrescribe; sin secretos; sin scripts de instalación versionados.
6. **Codificación e idioma:** UTF-8 sin BOM, español, commits `<tipo>(<módulo>): …`, y `VERSION` + `CHANGELOG.md` en cada versión.
