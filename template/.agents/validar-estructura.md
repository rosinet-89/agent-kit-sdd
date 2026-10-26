# Validación de la estructura del kit (checklist que corre el agente)

Lo usa el comando `/sdd-estructura`. Solo **lee y reporta**: no corrige nada sin confirmación.
Sirve para comprobar que el kit mantiene su estructura: una sola copia canónica en `.agents/`,
réplicas por herramienta como enlaces, y skills, agentes, comandos e índices coherentes.

## Alcance

La sesión principal corre E1 a E20. No se delegan a un subagente.

`/sdd-estructura` sin argumento corre todos los bloques. Con argumento corre uno:
`enlaces`, `skills`, `agentes`, `comandos`, `docs`, `codificacion` o `kit` (solo en el repo del kit).

## Chequeos

| # | Bloque | Qué se comprueba | Cómo |
|---|---|---|---|
| E1 | enlaces | Cada fila de `.agents/sync-agents.md` está `OK`; ningún `OCUPADO` ni `DIFERENTE` | Estados de `sync-agents.md` |
| E2 | enlaces | **Réplicas huérfanas:** skill, agente o comando que existe en una carpeta de herramienta (`.claude/`, `.cursor/`, `.gemini/`, `.github/`) y no en `.agents/` | `sync-agents.md` paso 8 |
| E3 | enlaces | **Réplicas faltantes:** entrada de `.agents/` que una carpeta de herramienta real (no enlace) no tiene | `sync-agents.md` paso 8 |
| E4 | skills | Cada carpeta de `.agents/skills/` tiene `SKILL.md`; sin archivos sueltos en `skills/` | `ls` |
| E5 | skills | `name` del frontmatter = nombre de la carpeta (minúsculas, guiones, sin acentos) | Leer la línea `name:` |
| E6 | skills | `description` presente, con qué hace **y** cuándo activarla; máx. 1024 caracteres | Contar la descripción |
| E7 | skills | Cada skill está en la tabla de "Skills de proyecto" de `AGENTS.md` (las de dominio y las del kit); ninguna fila apunta a una skill inexistente | Cruzar tabla y carpetas |
| E8 | agentes | Cada `agents/<rol>.md` tiene `name` (= archivo), `description` y `tools`; los roles coinciden con "Roles y comandos" de `AGENTS.md` | Leer frontmatter |
| E9 | comandos | Cada `commands/<n>.md` tiene `description` y `argument-hint` (si recibe argumentos); los comandos citados en `AGENTS.md` existen y los que existen están citados | Cruzar lista y carpeta |
| E10 | comandos | Los comandos del flujo `sdd` están en la tabla de fases de `skills/sdd/SKILL.md` y en el ruteo de `coordinator.md` | Cruzar tablas |
| E11 | rutas | Toda ruta citada con comillas inversas en `AGENTS.md`, `.agents/**/*.md` y `.docs/**/README.md` existe. No aplica a los manifiestos de versión: pueden listar rutas retiradas | `grep` de rutas + `ls` |
| E12 | docs | Sin prefijos repetidos **por valor numérico** en los ítems de `.docs/002_specs/`, `.docs/003_deuda/` y `.docs/004_adr/`. Remite a `.docs/002_specs/README.md` › « Numeración única ». El prefijo numérico vale su entero: los ceros de relleno y el ancho escrito no son `FALLA` y el nombre no se reescribe. Dos ítems con el mismo entero son `FALLA` y esa igualdad no es `AVISO`. Un nombre que no empieza por dígitos y `_` no entra en el consecutivo ni en el ancho. En el clon, los 4 dígitos valen para el ítem nuevo: el ancho ya escrito no pasa a `FALLA` y una mezcla válida no es `AVISO`. El `AVISO` de mezcla válida sigue siendo solo de proyecto. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014. Las carpetas de sección (`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`) se quedan en 3 | Listar y contar prefijos |
| E13 | docs | Cada índice (`002_specs`, `003_deuda`, `004_adr`) coincide con sus carpetas o archivos; si discrepan, manda la carpeta | Cruzar fila y archivo |
| E14 | docs | Toda carpeta de `.docs/002_specs/` tiene `01_spec.md` | `ls` |
| E15 | codificación | Archivos `.md` en UTF-8 sin BOM | `file -i`; `grep -l $'^\xEF\xBB\xBF'` vacío |
| E16 | sesión | Existen `AGENTS.md`, `CLAUDE.md` (solo `@AGENTS.md`), `MEMORY.md`, `.agents/napkin.md` y `.docs/001_constitucion/constitution.md` | `ls` y leer `CLAUDE.md` |
| E17 | sesión | Sin copias del cuerpo de una skill en `AGENTS.md` ni `CLAUDE.md`; napkin solo en `.agents/napkin.md` | Revisar anti-patrones de `.agents/README.md` |
| E18 | kit | Solo en el repo del kit: los comandos de verificación de `AGENTS.md` › Comandos (sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`; ensayo de instalación en `mktemp -d`) | `AGENTS.md` › Comandos |
| E19 | kit | Solo en el repo del kit: `diff -rq template/.agents .agents` solo debe mostrar `napkin.md`. Otra diferencia es una réplica desfasada: proponer el refresco de `INSTALAR.md` › Modo mantenimiento › "Refrescar la copia local" (sobrescribe desde `template/.agents/` y restaura `napkin.md`; no borra lo que solo está en `.agents/`). No ejecutarlo sin confirmación | `diff -rq` |
| E20 | kit | Solo en el repo del kit: recalcula la huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` y la compara con manifiestos/<VERSION>.md. Falla por diferencia, por archivo sin fila o por fila sin archivo. En un proyecto instalado no aplica: ahí no hay manifiestos | Algoritmo de `ACTUALIZAR.md` |

## Cómo responder

1. Corre los bloques pedidos y, de cada chequeo, anota `OK`, `AVISO` o `FALLA` con `archivo:línea`.
2. Reporta una tabla corta con solo lo que falla, severidad primero. Tras la tabla, un bloque
   **Avisos** (carpeta y hecho, una línea cada uno) con los `AVISO`; no es una corrección:

```
ALTA   | E2  | .claude/skills/probando/ | Skill fuera de .agents/ | Mover a .agents/skills/probando/ y enlazar
MEDIA  | E7  | .agents/skills/adr       | Falta fila en AGENTS.md | Agregar la ruta a la tabla
BAJA   | E15 | ChangelogAgentKitSdd.md  | BOM al inicio           | Quitar el BOM
```

3. Sin fallos: `estructura OK (E1–E20, N archivos revisados)`; si hubo avisos, se añade
   `, M avisos` y se listan en **Avisos**. No listes los OK uno a uno.
4. Cierra con **Correcciones propuestas**: lista numerada, sin aplicarlas. Cada corrección se
   ejecuta solo si el usuario la confirma. Nunca borres ni muevas un archivo (estado `OCUPADO` o
   huérfano) sin esa confirmación.
5. Una skill, agente o comando **en prueba** que el dev quiere dejar local no es un fallo: se
   anota como `EN PRUEBA` y se propone excluirlo en `.git/info/exclude` si no debe versionarse.
