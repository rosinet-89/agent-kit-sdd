# ADR 001: Entorno de mantenimiento versionado en la raíz del kit

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; decidida durante 0.5.0)
- **Origen:** Conversación de mantenimiento, 0.5.0

## Contexto
El kit se mantiene con su propio flujo (SDD). Antes, toda la instalación de la raíz era local y estaba excluida: al clonar el repo en otra máquina se perdían las reglas, la memoria, el napkin y las decisiones.

## Decisión
La raíz del kit se trata como un entorno de mantenimiento. Se versiona el conocimiento propio del kit (`AGENTS.md`, `CLAUDE.md`, `MEMORY.md`, `.docs/`, `.agents/napkin.md`). Lo que sale de `template/` (resto de `.agents/`, `.gitignore`, `PREFERENCIAS.md`, `.claude/`, `.cursor/`) sigue local y excluido en `.git/info/exclude`. `INSTALAR.md` distingue **Modo proyecto** (solo `template/`) de **Modo mantenimiento** y pregunta cuál cuando se abre el clon del kit.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Todo local y excluido (estado anterior) | El conocimiento del kit no sobrevive a un clon nuevo |
| Carpeta `decisiones/` versionada aparte de `.docs/` | Duplica la estructura del kit y obliga a sincronizar a mano con la instalación |
| Copiar un "semillero" desde `template/` al instalar | Mezcla producto y conocimiento propio; `template/` no debe llevar nada del kit

## Consecuencias
Una instalación nueva en el clon deja las mismas skills y configuración que cualquier proyecto, más las specs, ADR y memoria del kit. Hay que mantener las exclusiones ancladas con `/`. Los cambios en `AGENTS.md`/`.docs/` ahora se commitean (con confirmación). La parte local de `.agents/` puede quedar desactualizada respecto de `template/`: se refresca al instalar sin sobrescribir o con una actualización manual.
