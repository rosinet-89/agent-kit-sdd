# agent-kit — flujo SDD con fuente de verdad `AGENTS.md`

Kit genérico (en español) para que cualquier proyecto trabaje con agentes de IA bajo un mismo
esquema: **`AGENTS.md`** como fuente de verdad, **`.agents/`** con agentes, comandos y skills, y
**`.docs/`** con constitución, planes (Spec-Driven Development) y deuda técnica.

## Qué contiene `template/`

```
AGENTS.md  CLAUDE.md  MEMORY.md  .gitignore
.agents/
  README.md  napkin.md  sync-agents.md  preferencias.ejemplo.md
  agents/    coordinator, planner, implementer, reviewer
  commands/  sdd, sdd-init, sdd-spec, -clarify, -plan, -tasks, -implement,
             -validate, -flujo, -cierre, -change, -status, -constitucion
  skills/    sdd (+ plantillas, avance y tiempos), napkin, git-commit, formato-respuesta,
             adr, migracion-bd, seguridad-basica
.docs/
  001_constitucion/  002_specs/ (índice)  003_deuda/ (índice)  004_adr/ (índice)
```

En la raíz del kit, fuera de `template/` (no se copian al instalar):

```
ACTUALIZAR.md    procedimiento para actualizar un proyecto que ya tiene el kit
manifiestos/     huellas por versión; no se distribuye
```

## Instalación

El kit no trae scripts (dependen del SO y del shell). Abre esta carpeta con tu agente y pídele:
**"instala el kit en `<ruta del proyecto>` siguiendo `INSTALAR.md`"**. El agente detecta tu SO y
shell, te ofrece un script a tu medida o el paso a paso, y copia `template/` sin sobrescribir nada.
También puedes hacerlo a mano: ver `INSTALAR.md`.

Si el proyecto ya tiene el kit, no lo instales otra vez. Sigue `ACTUALIZAR.md`.

Si lo instalas en un proyecto existente, solo se copia `template/`. Si abres el propio clon del kit, el
agente te pregunta si lo quieres para un proyecto nuevo o para trabajar el kit (modo mantenimiento, que
conserva sus specs, ADR y memoria).

## Primer uso: personalizar

1. Abre el proyecto con tu agente (Claude Code, Cursor, …).
2. Corre **`/sdd-init`**. El agente lee el repo, te entrevista (máx. 6 preguntas) y propone, sin
   escribir nada hasta que apruebes: los huecos de `AGENTS.md` (stack, comandos, reglas de negocio),
   la constitución, el napkin inicial, `MEMORY.md` y la lista de skills de dominio.
3. Con tu aprobación escribe todo y quita el marcador `SDD-INIT-PENDIENTE`.
4. Verifica los enlaces de `.agents/` hacia la carpeta de tu herramienta (`.claude/`, `.cursor/`…):
   el agente lo revisa cada sesión y propone cómo crearlos según tu entorno
   (ver `.agents/sync-agents.md`).
5. Empieza a trabajar con **`/sdd <lo que necesitas>`**.

Mientras exista `SDD-INIT-PENDIENTE` en `AGENTS.md`, los agentes piden correr `/sdd-init` antes de
cualquier otro comando `/sdd*`.

## Preferencias por usuario

`PREFERENCIAS.md` (raíz, **en `.gitignore`**) guarda las directivas personales de cada dev según su
entorno (p. ej. "limpiar contexto" en Zed, que no puede borrar el chat). Los agentes lo leen cada
sesión y, si no existe, lo crean desde `.agents/preferencias.ejemplo.md`. Lo del equipo va en
`AGENTS.md`, el napkin o `MEMORY.md`.

## Reglas del kit

- Lo genérico vive aquí; lo propio del proyecto (skills de dominio, reglas de negocio, trampas)
  se crea en el proyecto, nunca en el kit.
- Cada versión se anota en `CHANGELOG.md` y `VERSION`. Para actualizar un proyecto que ya tiene
  el kit, sigue `ACTUALIZAR.md`.

## Licencia

MIT. Ver `LICENSE`.
