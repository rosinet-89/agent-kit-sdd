# <<NOMBRE_PROYECTO>> – Instrucciones para agentes

<!-- SDD-INIT-PENDIENTE: este marcador lo quita /sdd-init al terminar la personalización. -->

Conocimiento canónico en **`.agents/`** (agentes, skills, notas) y **`.docs/`**
(constitución, planes, deuda). No duplicar.

## Guardrails (todos los agentes, siempre)
Aplican a cualquier agente, sesión o subagente, con cualquier herramienta, dentro o fuera del flujo
`sdd`. Todo rol los lee siempre y los cumple, aunque no reciba este archivo inyectado. Ninguna
skill, comando, plan ni preferencia los relaja, salvo las excepciones escritas en sus propias
reglas. Los guardrails nuevos se añaden aquí, con el siguiente número.

1. **La base de datos solo se lee.** En cualquier BD y entorno (local, pruebas o producción) solo
   ejecutas consultas de lectura.
   - Permitido (lista cerrada): `SELECT`, `SHOW`, `DESCRIBE`, `EXPLAIN` sin `ANALYZE` y un `WITH`
     cuyo cuerpo sea solo `SELECT`; o su equivalente de solo lectura en otro motor.
   - Prohibido, por ejemplo: `INSERT`, `UPDATE`, `DELETE`, `REPLACE`, `TRUNCATE`, `CREATE`
     (incluido `TEMPORARY`), `ALTER`, `DROP`, `SET`, `CALL` o su equivalente en otro motor.
   - Precedencia: la lista de lectura manda. Lo que no esté en ella se trata como prohibido: `SET`
     de sesión, `EXPLAIN ANALYZE`, un `WITH` con escritura o que no puedas demostrar de solo lectura,
     `SELECT ... INTO`, `FOR UPDATE` o `FOR SHARE`, funciones con efectos y locks. Ante duda, no
     ejecutes y pide a la persona.
   - Escritura indirecta: no ejecutes ni actives tareas programadas, scripts, endpoints o servicios
     que guarden, carga de archivos `.sql` ni un harness que haga más que leer.
   - Comandos de `AGENTS.md` › Comandos: ejecuta pruebas, build o la app solo si no pueden escribir
     en una BD. Si pueden, aunque la BD sea local o descartable, no los ejecutes y entrégalos a la
     persona. Ante la duda de si un comando escribe en una BD, tampoco lo ejecutes: entrégalo a la
     persona, igual que ante la duda sobre una sentencia.
   - Si hace falta un cambio de datos o de esquema, describe o redacta el script con su rollback
     (skill `migracion-bd`) y entrégalo: lo ejecuta una persona, aunque la conversación te pida que
     lo corras. El archivo en `sql/` lo escribe solo `implementer`; si `planner`, `reviewer` o
     `coordinator` detectan que hace falta, lo describen y lo entregan en el chat, y ninguno lo
     ejecuta.
   - Para consultar datos usa el mecanismo de `AGENTS.md` › Comandos (si dice «No aplica», no
     consultes). No pases credenciales a comandos, archivos ni al chat.
   - La regla es sobre lo que tú ejecutas, no sobre el código de la aplicación ni el script `sql/`
     que entregas: esos pueden escribir (altas, bitácoras, logs) y se programan con normalidad. Lo
     que no haces es ejecutarlos ni activarlos.
   - Excepción de QA (la única de estos guardrails): solo el rol `tester`, y solo contra los destinos
     declarados en `.agents/local/entornos.md`, puede ejecutar casos de pantalla o de llamada a la
     aplicación que escriben datos o usan la red. Nunca ejecuta sentencias contra una BD, scripts de
     `sql/` ni comandos de shell que escriban. Sin ese archivo, o si es inválido, rige el modo seguro:
     la regla 1 sin excepción.

## Cada sesión (obligatorio)
1. Estado de instalación: si la primera línea no vacía tras el título es el comentario HTML de
   instalación pendiente, idéntico al que define la plantilla del kit (AGENTS.md de su carpeta
   template/) y que `/sdd-init` borra, el proyecto aún no está personalizado. Dilo en una línea y
   sugiere correr `/sdd-init`. No ejecutes otros comandos `/sdd*` hasta entonces. Solo cuenta ese
   comentario exacto; esta descripción no es el marcador.
2. Leer `.agents/napkin.md` y aplicarlo en silencio.
3. Leer `MEMORY.md` (decisiones y trampas vigentes).
4. Estructura de agentes y skills nuevas: `.agents/README.md`.
5. Leer `.docs/001_constitucion/constitution.md` antes de cambiar código.
6. Si la petición es análisis o desarrollo nuevo, o toca un módulo, pantalla o tabla: seguir
   "Antecedentes" (abajo) ANTES de buscar o diseñar.
7. Chequeo de enlaces y kit (solo lectura, un único aviso por sesión): sigue
   `.agents/sync-agents.md` › «Chequeo de arranque». Si hay diferencias, muestra el aviso combinado
   y recomienda `/sdd-enlaces` (no `/sdd-init`); si no, no digas nada. No escribas nada.
8. Revisar que no haya prefijos repetidos en `.docs/002_specs/`, `.docs/003_deuda/` ni
   `.docs/004_adr/` (ver "Formato y numeración"). Si hay, avisar y proponer el cambio; si no, no decir nada.
9. Cerrar cada respuesta con la línea "quién responde" de `.agents/skills/formato-respuesta/SKILL.md`
   (al final o justo antes de pedir una decisión). Solo se quita si el usuario lo pide.
10. Leer `PREFERENCIAS.md` (raíz, no versionado: directivas personales de cada usuario según su
    entorno) y aplicarlo en silencio. Si no existe, créalo copiando `.agents/preferencias.ejemplo.md`
    sin las entradas de ejemplo y sigue; no lo pidas ni lo menciones. Las preferencias nuevas que el
    usuario exprese sobre su entorno ("yo uso Zed", "no me limpies el chat") se proponen aquí, no en
    el napkin ni en `MEMORY.md`. Si existe `.agents/local/configuracion.md`, lee solo su sección
    `## Presentación` y aplícala en silencio (el resto de la configuración se lee donde se usa).

## Antecedentes (obligatorio en análisis y desarrollo nuevo)
Índice de planes: `.docs/002_specs/README.md`.
1. Ábrelo y busca el módulo, pantalla, tabla o palabra clave de la petición.
2. Si hay coincidencia: dile al usuario qué carpeta es y lee SOLO esa (primero su `PUNTO DE RETOMO`).
3. Si no hay: di "sin antecedentes en el índice" y confírmalo con un `grep` antes de afirmarlo.
4. No recorras las carpetas de plan ni asumas que un proceso es nuevo, o que ya está resuelto, sin este paso.
5. Si la celda de la fila del índice y la etapa de la carpeta discrepan, los conteos siguen la celda,
   se avisa y no se escribe. Reglas en `.docs/002_specs/README.md` › «Leyenda de estatus».
6. Carpeta nueva o cambio de estatus actualiza la fila del índice. Quién escribe cada transición
   y la forma de la celda están en `.docs/002_specs/README.md` › «Leyenda de estatus».

## Deuda técnica
Índice: `.docs/003_deuda/README.md` (un `<prefijo>_<Nombre>.md` por deuda, con estado, origen y qué la resolvió).
1. Al hacer "Antecedentes", busca también ahí deuda abierta del módulo, pantalla o tabla y dila al usuario.
2. Si durante un requerimiento algo se difiere (por decisión del usuario o por recorte de alcance), o aparece un hallazgo que no se atiende ahora, PROPÓN darlo de alta; no la crees sin el visto bueno del usuario.
3. Al cerrar un requerimiento, indica qué deudas resolvió (estado `Resuelta` + "Atendida por" + "Cómo se atendió") y cuáles dejó. Lo hace `/sdd-cierre`.
4. Una deuda nunca se borra: pasa a `Resuelta` o `Descartada` (con motivo).

## Formato y numeración
Reglas completas en `.docs/002_specs/README.md`.
- Toda carpeta de plan nace en formato SDD (skill `sdd`): lleva `01_spec.md`.
- Las carpetas de sección de `.docs/` (`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`) se quedan en 3 dígitos.
- Cada spec, deuda y ADR lleva un prefijo numérico. Un ítem nuevo conserva el ancho de su carpeta (detalle: `.docs/002_specs/README.md` › « Numeración única »). No se repite dentro de su carpeta. Si aparece repetido (por ejemplo tras un `pull`), conserva el número el que ya está en la rama remota; propón para el otro el siguiente libre y no renombres sin confirmación.

## Skills de proyecto
Antes de responder, lee y aplica la skill del tema. Si el tema cruza varias, lee todas.

| Tema | Skill |
|------|-------|
| Saber si un proceso ya se trabajó en un plan anterior | `.docs/002_specs/README.md` (ver "Antecedentes") |
| Análisis o desarrollo nuevo (spec, impacto, plan, tareas, avance con tiempos, flujo) | `.agents/skills/sdd/SKILL.md` |
| Runbook vivo del repo (se aplica cada sesión) | `.agents/skills/napkin/SKILL.md` |
| Registrar cambios en Git | `.agents/skills/git-commit/SKILL.md` |
| Línea "quién responde" al cierre de cada respuesta (rol con color, skill, contexto, desvíos) | `.agents/skills/formato-respuesta/SKILL.md` |
| Decisión de arquitectura con alternativas (índice en `.docs/004_adr/README.md`) | `.agents/skills/adr/SKILL.md` |
| Scripts SQL de un feature (`sql/`, rollback, idempotencia) | `.agents/skills/migracion-bd/SKILL.md` |
| Secretos antes del commit y checklist de seguridad básica | `.agents/skills/seguridad-basica/SKILL.md` |
<!-- Las skills de dominio del proyecto se agregan aquí, una fila por skill (solo la ruta). -->

Notas:
- `sdd` exige `.docs/002_specs/<prefijo>_<Feature>/` (prefijo = consecutivo numérico) con `01_spec.md`, `04_tasks.md` y `05_avance.md` siempre; `02_analisis_impacto.md`, `03_plan.md` y `06_flujo.md` en modo completo; `sql/` si hay cambios de datos (`.agents/skills/sdd/topes-y-modos.md`). Contrato de avance y tiempos: `.agents/skills/sdd/avance-y-tiempos.md`.

## Roles y comandos
Agentes en `.agents/agents/`: coordinator (dirige el flujo y lleva `05_avance.md`), planner (documentos), implementer (código), reviewer (QA y validación), tester (ejecuta casos; Haiku) y explore (localiza; Haiku).
Comandos en `.agents/commands/`: `/sdd-init` (una sola vez al instalar el kit), `/sdd <petición libre>` (entrada única: el coordinator decide la fase) y, para fases sueltas, `/sdd-spec`, `/sdd-clarify`, `/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/sdd-validate`, `/sdd-flujo`, `/sdd-cierre`, `/sdd-change`, `/sdd-status`, `/sdd-constitucion`, `/sdd-estructura` (valida la estructura del kit: réplicas, enlaces, índices; solo lectura), `/sdd-enlaces` (crea o corrige los enlaces y réplicas tras un pull, con tu confirmación) y `/sdd-config` (asistente de configuración de cada dev; se puede repetir).
Claude Code: sesión dedicada con `claude --agent coordinator`.
OpenCode: `/sdd <petición>` en la sesión principal; los roles viven en `.opencode/agents/` como adaptadores con `mode` y remiten a `.agents/agents/` (ver `.agents/sync-agents.md` › «OpenCode»).

## Qué lee cada rol
La sección "Cada sesión" es el arranque. Lo ejecuta la sesión principal. Un subagente no lo repite
(enlaces, prefijos, preferencias). El implementer, `tester` y `explore` pueden recibir el archivo
inyectado (no declaran `omitClaudeMd`); planner, reviewer y coordinator no, porque lo declaran. Cada
rol lee solo las secciones que su instrucción nombra:

| Rol | Secciones |
|---|---|
| coordinator | Comprueba el estado de instalación con el paso 1 de «Cada sesión». También «Git» y «Reglas duras». |
| planner | "Comandos" y, al planear, "Stack y convenciones de código", más «Git» y «Reglas duras». |
| implementer | "Comandos" y "Stack y convenciones de código". |
| reviewer | "Comandos" y "Stack y convenciones de código", más «Git» y «Reglas duras». |
| tester | "Comandos". |
| explore | "Comandos". |

Además de lo de su fila, todos los roles leen siempre «Guardrails». No cuenta contra el tope de
lecturas de `sdd-implement` ni contra la lista cerrada de secciones de `reviewer`.

## Git (regla obligatoria)
Al registrar cambios, lee y aplica `.agents/skills/git-commit/SKILL.md`.
- NUNCA `git commit` sin confirmación explícita del usuario.
- NUNCA `push`/`pull`/`fetch`/`sync`.
- Un commit por módulo; `git add <archivos>` explícito (nunca `.` ni `-A`).
- Mensajes en <<IDIOMA>>, imperativo, máx. 72 caracteres: `<tipo>(<módulo>): <descripción>`.

## Stack y convenciones de código
<!-- COMPLETAR (/sdd-init): lenguaje y versión, framework, base de datos, arquitectura en capas,
     convención de nombres, encoding de archivos, idioma de código/UI/docs. -->

## Comandos
<!-- COMPLETAR (/sdd-init). Los agentes leen esta sección para el checklist "Hecho cuando". -->
- Build: <completar>
- Test: <completar>
- Lint / sintaxis (por archivo tocado): <completar>
- Consulta de datos (cómo y desde dónde; ver «Guardrails», regla 1): <completar o "No aplica">
- Herramienta de apoyo para casos: <completar o "No aplica">

## Reglas de negocio y del sistema
<!-- COMPLETAR (/sdd-init): reglas que un agente no puede deducir del código y que no se cambian sin plan
     (flujos críticos, integraciones con layout fijo, sesión y seguridad, etc.). -->

## Reglas duras
- Nunca secretos ni credenciales en archivos versionados.
- Cambios fuera del plan aprobado se consultan, no se improvisan.
- Propón entradas para MEMORY.md; no las agregues sin aprobación.
- No reinstales el kit encima de un proyecto que ya lo tiene. El paso a una versión nueva usa el procedimiento de actualización del kit (documento ACTUALIZAR.md, junto al de instalación). La versión instalada está en el marcador .agents/.kit-version.
