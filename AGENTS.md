# agent-kit-sdd – Instrucciones para agentes

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
     (incluido `TEMPORARY`), `ALTER`, `DROP`, `SET`, `CALL`.
   - Precedencia: lo que no esté en la lista es prohibido: `SET` de sesión, `EXPLAIN ANALYZE`, `WITH`
     con escritura o no demostrable de solo lectura, `SELECT ... INTO`, `FOR UPDATE`/`FOR SHARE`,
     funciones con efectos, locks. Ante duda, no ejecutes y pide a la persona.
   - Escritura indirecta: no ejecutes ni actives tareas programadas, scripts, endpoints o servicios
     que guarden, carga de `.sql` ni un harness que haga más que leer.
   - Comandos (`AGENTS.md` › Comandos): pruebas, build o app solo si no pueden escribir en una BD,
     aunque sea local; si pueden o dudas, entrégalos a la persona.
   - Cambio de datos o esquema: script con rollback (skill `migracion-bd`) que ejecuta una persona,
     aunque se pida correrlo. `sql/` lo escribe solo `implementer`; `planner`, `reviewer` y
     `coordinator` lo describen en el chat.
   - Consulta de datos: mecanismo de Comandos («No aplica» = no consultes); sin pasar credenciales.
   - La regla es sobre lo que tú ejecutas, no sobre el código de la app ni el script `sql/` que
     entregas: esos pueden escribir.
   - Excepción de QA (la única): solo `tester`, solo contra destinos de `.agents/local/entornos.md`,
     puede ejecutar casos de pantalla o llamada a la app que escriben o usan red. Nunca sentencias
     contra una BD, scripts `sql/` ni shell que escriba. Sin ese archivo, o inválido, modo seguro:
     sin excepción.

## Cada sesión (obligatorio)
0. Si no existe `.agents/skills/`: el kit no está instalado en esta copia (clon nuevo). Dilo en
   una línea y sigue `INSTALAR.md` › "Modo mantenimiento" (pregunta antes de copiar). Sin eso no hay
   comandos `/sdd*` ni skills; no sigas con los pasos siguientes.
1. Estado de instalación: si la primera línea no vacía tras el título es el comentario HTML de
   instalación pendiente, idéntico al que define la plantilla del kit (AGENTS.md de su carpeta
   template/) y que `/sdd-init` borra, el proyecto aún no está personalizado. Dilo en una línea y
   sugiere correr `/sdd-init`. No ejecutes otros comandos `/sdd*` hasta entonces. Solo cuenta ese
   comentario exacto; esta descripción no es el marcador.
2. Leer `.agents/napkin.md` y aplicarlo en silencio.
3. Leer `MEMORY.md` (decisiones y trampas vigentes).
4. Antes de crear o modificar un agente, skill o comando (también a media sesión o en
   `/sdd-init`): leer `.agents/README.md`. Si la petición no los toca, no se lee.
5. Leer `.docs/001_constitucion/constitution.md` antes de cambiar código.
6. Si la petición es análisis o desarrollo nuevo, o toca un módulo, pantalla o tabla: seguir
   "Antecedentes" (abajo) ANTES de buscar o diseñar.
7. Chequeo de enlaces y kit (solo lectura, un único aviso por sesión): lee
   `.agents/sync-agents.md` › «Chequeo de arranque» y solo lo que esa sección indica, no el archivo
   entero. Si hay diferencias, muestra el aviso combinado y recomienda `/sdd-enlaces` (no
   `/sdd-init`); si no, no digas nada. No escribas nada.
8. Prefijos repetidos: mira solo las tablas de índice de `.docs/002_specs/`, `.docs/003_deuda/`,
   `.docs/004_adr/` y, si existe, `.docs/005_contexto/`; repetido = dos filas del mismo índice con
   el mismo entero (no listes carpetas ni abras documentos). Si hay, lee `.docs/002_specs/README.md`
   › «Numeración única», avisa y propone el cambio; si no, no digas nada.
9. Cerrar cada respuesta con la línea "quién responde" de `.agents/skills/formato-respuesta/SKILL.md`
   (al final o justo antes de pedir una decisión). Solo se quita si el usuario lo pide.
10. Leer `PREFERENCIAS.md` (raíz, no versionado: directivas personales de cada usuario según su
    entorno) y aplicarlo en silencio. Si no existe, créalo copiando `.agents/preferencias.ejemplo.md`
    sin las entradas de ejemplo y sigue; no lo pidas ni lo menciones. Las preferencias nuevas que el
    usuario exprese sobre su entorno ("yo uso Zed", "no me limpies el chat") se proponen aquí, no en
    el napkin ni en `MEMORY.md`.

## Antecedentes (obligatorio en análisis y desarrollo nuevo)
Índice de planes: `.docs/002_specs/README.md`. Procedimiento: `.agents/skills/sdd/antecedentes-y-deuda.md`
› «Antecedentes». Se lee al hacer análisis o desarrollo nuevo, o si la petición toca un módulo,
pantalla o tabla.

## Deuda técnica
Índice: `.docs/003_deuda/README.md`. Procedimiento: `.agents/skills/sdd/antecedentes-y-deuda.md` ›
«Deuda técnica». Se lee junto con «Antecedentes» y al diferir algo o cerrar un requerimiento.

Contexto del proyecto bajo demanda: índice `.docs/005_contexto/README.md`.

## Formato y numeración
Numeración, ancho y prefijos repetidos: `.docs/002_specs/README.md` › «Numeración única». Se lee al
crear un ítem o si el paso 8 halla repetidos.

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
| Bitácora de uso por dev (beta): registro por tramo y reporte de tokens | `.agents/skills/bitacora-uso/SKILL.md` |
<!-- Las skills de dominio del proyecto se agregan aquí, una fila por skill (solo la ruta). -->

Notas:
- `sdd` exige `.docs/002_specs/<NNN>_<Feature>/` (en este clon, un ítem nuevo se escribe con 4 dígitos y los ya creados no se renumeran; en un proyecto, el ancho de su carpeta según `template/.docs/002_specs/README.md` › « Numeración única ») con `01_spec.md`, `04_tasks.md` y `05_avance.md` siempre; `02_analisis_impacto.md`, `03_plan.md` y `06_flujo.md` en modo completo; `sql/` si hay cambios de datos (`.agents/skills/sdd/topes-y-modos.md`). Contrato de avance y tiempos: `.agents/skills/sdd/avance-y-tiempos.md`.

## Roles y comandos
Roles de `.agents/agents/` y comandos de `.agents/commands/`: `.agents/README.md` › «Roles y comandos».
Se lee al elegir o crear un rol o comando.
Tarea suelta sin rol (no diseño ni decisión): nivel y delegación en
`.agents/skills/sdd/restricciones-de-rol.md` › «Nivel por tarea sin rol»; se lee antes de empezarla.

## Qué lee cada rol
Cada rol lee de este archivo solo las secciones que nombra su `.agents/agents/<rol>.md`, y siempre
«Guardrails», que no cuenta contra el tope de lecturas de `sdd-implement` ni contra la lista cerrada
de `reviewer`. «Cada sesión» lo ejecuta la sesión principal; un subagente no lo repite.

## Git (regla obligatoria)
Al registrar cambios, lee y aplica `.agents/skills/git-commit/SKILL.md`.
- NUNCA `git commit` sin confirmación explícita del usuario.
- NUNCA `push`/`pull`/`fetch`/`sync`. Única excepción de red: `/sdd-version` (`git ls-remote` y un
  clon temporal `--depth 1` en `mktemp -d`, que no toca el proyecto ni el clon local y se borra).
- Un commit por módulo; `git add <archivos>` explícito (nunca `.` ni `-A`).
- Mensajes en español, imperativo, máx. 72 caracteres: `<tipo>(<módulo>): <descripción>`.

## Stack y convenciones de código
- Solo Markdown, UTF-8 sin BOM. Frontmatter YAML en agentes, comandos y skills.
- Sin código ejecutable ni scripts de instalación versionados.
- Idioma de docs, comandos y commits: español.

## Comandos
- Build: No aplica.
- Test (ensayo de instalación): `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` y revisar
  que se copie todo sin pisar nada. Nunca en un proyecto real.
- Lint / sintaxis (por archivo tocado): `file -i <archivo>` da utf-8; sin BOM
  (`grep -l $'^\xEF\xBB\xBF' <archivo>` vacío); `name` del frontmatter = nombre de su carpeta; rutas
  citadas existentes (no aplica a `manifiestos/`: pueden listar rutas retiradas); en `template/` sin
  marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- Consulta de datos (ver «Guardrails», regla 1): No aplica.
- Herramienta de apoyo para casos: No aplica.
- Chequeos de cierre: E20 de `template/.agents/validar-estructura.md` (huella de `manifiestos/<VERSION>.md`), cuando el requerimiento cambió `VersionAgentKitSdd`; se corre después del último cambio a `template/`. Si falla, regenera las filas de los archivos tocados, anótalo como decisión y vuelve a correrlo.

## Reglas de negocio y del sistema
- Lo distribuible se edita en `template/` y en los docs de la raíz (`README.md`, `INSTALAR.md`,
  `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd`, `LicenseAgentKitSdd`).
- La raíz es el **entorno de mantenimiento** del kit (ver `INSTALAR.md` › "Modo mantenimiento"):
  - **Versionado** (conocimiento propio del kit, no sale de `template/`): `AGENTS.md`, `CLAUDE.md`,
    `MEMORY.md`, `.docs/` (constitución, specs, deuda, ADR) y `.agents/napkin.md`.
  - **Local, excluido en `.git/info/exclude`** (sale de `template/` al instalar): el resto de
    `.agents/`, `.gitignore`, `PREFERENCIAS.md`, `.claude/`, `.cursor/`, `.opencode/`.
  - Las decisiones del kit se registran como ADR en `.docs/004_adr/` (skill `adr`); el `template/`
    solo lleva el índice vacío.
- Nada propio de un proyecto (stack, nombres, dominio) entra a `template/`.
- Cada versión actualiza `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`.
- La instalación copia sin sobrescribir nunca. Actualizar un proyecto que ya tiene el kit solo
  reemplaza archivos KIT-VIEJO confirmados (`.docs/004_adr/007_ActualizarSoloKitViejo.md`).
- Los manifiestos viven en `manifiestos/`, fuera de `template/`, y no se distribuyen.

## Reglas duras
- Nunca secretos ni credenciales en archivos versionados.
- Cambios fuera del plan aprobado se consultan, no se improvisan.
- Propón entradas para MEMORY.md; no las agregues sin aprobación.
