---
description: SDD · Propone o revisa la constitución del proyecto (principios innegociables)
argument-hint: <contexto adicional opcional>
---
Vamos a crear (o revisar, si ya existe) `.docs/001_constitucion/constitution.md`. Usa la skill `sdd`.
`/sdd-init` ya propone una primera versión; este comando sirve para revisarla después.

Qué lee: `AGENTS.md`, `MEMORY.md`, `.agents/napkin.md`, las skills de `.agents/skills/` y el código
del proyecto, dentro del tope declarado en `.agents/commands/sdd-init.md` (30 archivos o 150 KB, lo
que ocurra primero).

Qué hace: propone 6 principios y espera aprobación antes de escribir.

Qué no hace: no declara un tope distinto del de `sdd-init.md`. No tiene meta de reducción de lectura. No escribe
`constitution.md` antes de la aprobación.

Contexto adicional: $ARGUMENTS

Propón 6 principios innegociables, cortos y verificables (sí/no), máximo 15 líneas, que
cubran:
1. Stack (lenguaje, base de datos, librerías presentes; no se agrega dependencia sin plan).
2. El plan manda (flujo `sdd` en `.docs/002_specs/`).
3. Capas separadas (según la arquitectura del proyecto).
4. Verificación (checklist "Hecho cuando"; qué pruebas automatizadas existen o no).
5. Datos y seguridad (solo lectura para agentes, `sql/` con rollback, sesión/permisos, sin secretos).
6. Codificación e idioma.

Cada principio debe apoyarse en algo verificable del repo; lo que no, márcalo
`(por confirmar)`. Incluye dónde lo verificaste y qué partes del código ya lo incumplen.
NO escribas el archivo todavía: espera mi aprobación.
