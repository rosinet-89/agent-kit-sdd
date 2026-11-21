# 01 — Spec: Enlaces tras un pull
Estado: cerrada
Ticket: issue #2 rosinet-89/agent-kit-sdd

## Contexto y objetivo
Los enlaces `.claude/`, `.cursor/`, `.gemini/` y `.github/` hacia `.agents/` no se versionan (`.gitignore`); cada persona los crea en su máquina. Hoy el paso 7 de «Cada sesión» de `AGENTS.md` solo lo sugiere (pasivo). Si no se sigue, Claude Code o Cursor no ven los `/sdd*` ni los agentes, y no hay ningún error que lo delate. `/sdd-estructura` solo reporta; no ofrece crear ni corregir enlaces. Tras un pull que actualiza el kit (`.kit-version`, comandos, agentes, skills nuevos) nada avisa qué cambió ni qué hacer. Caso real: al pasar a 0.5.0 el equipo necesitó un mensaje manual con comandos para Windows y macOS/Linux, y dudó si correr `/sdd-init` o `/sdd-estructura`.

Objetivo: que tras un pull que cambia el kit, la persona se entere sola de qué cambió, pueda crear o corregir los enlaces de sus herramientas con confirmación mediante `/sdd-enlaces`, y sepa qué hacer (y qué no) sin depender de un mensaje manual. Todo sin scripts versionados (ADR 004).

## Usuarios y roles
- Persona del equipo que hace pull de un proyecto con el kit instalado.
- Persona que clona el proyecto por primera vez (máquina o clon nuevo).
- Mantenedor del kit, que publica versiones y las documenta.

## Historias de usuario
- HU-1. Como persona del equipo, quiero que al abrir sesión tras un pull se me avise qué trae la actualización del kit, para no depender de un mensaje manual.
- HU-2. Como persona con un clon nuevo, quiero recibir en mi primera sesión la propuesta de enlaces, para ver los comandos y agentes sin investigar.
- HU-3. Como persona del equipo, quiero que el agente me ofrezca crear o corregir los enlaces con el comando exacto para mi sistema, para no equivocarme ni pedir ayuda.
- HU-4. Como persona del equipo, quiero que solo se creen los enlaces de las herramientas que uso, para no llenar mi copia de carpetas ajenas.
- HU-5. Como persona del equipo, quiero saber qué hacer tras un pull que actualiza el kit y que `/sdd-init` no se vuelve a correr, para no repetir pasos destructivos o innecesarios.

## Definiciones
- Estado «visto»: registro local, por máquina y no versionado, en `.agents/local/kit-visto`. Guarda la última `.kit-version` observada, las piezas existentes en ese momento, las herramientas declaradas por la persona y las excepciones de destinos OCUPADO intencionales. Lo escribe únicamente `/sdd-enlaces`.
- Pieza: cada entrada de primer nivel de `.agents/agents`, `.agents/commands` y `.agents/skills` (archivo o carpeta, por nombre). Es nueva si está en disco y no en «visto»; retirada si está en «visto» y no en disco.
- Formato de «visto»: texto plano UTF-8 con una línea `version: X.Y.Z`, una línea `herramientas: ...`, una línea por excepción y una línea por pieza con su ruta relativa. Cualquier otra cosa se trata como corrupto. El detalle exacto se fija en el plan.
- Estados de enlace: OK, FALTA, DIFERENTE y OCUPADO, según `.agents/sync-agents.md`.
- Herramientas declaradas: las que la persona indicó a `/sdd-enlaces` y quedaron registradas en «visto».
- Clon nuevo: copia del proyecto sin «visto» (equivale a «todo nuevo»).
- Aviso combinado: único mensaje de la sesión que reúne los cambios del kit (versión, piezas) y los enlaces pendientes, y ofrece `/sdd-enlaces` una sola vez.

## Requisitos funcionales (EARS)
- RF-1 (disparadores): CUANDO inicia una sesión, EL SISTEMA MOSTRARÁ el aviso combinado (paso 7 de «Cada sesión», solo lectura) en cualquiera de estos casos: (a) la `.kit-version` difiere de la de «visto», incluido un retroceso; (b) «visto» no existe o está corrupto, o `.kit-version` no existe: se trata como «todo nuevo»; (c) la versión es igual y hay piezas nuevas o retiradas; (d) la versión es igual, no hay piezas nuevas ni retiradas, pero hay enlaces FALTA o DIFERENTE de herramientas declaradas: aviso solo de enlaces. En todos recomienda `/sdd-enlaces` sin que la persona lo pida. Sin cambios ni enlaces pendientes, no dice nada.
  - Nota (2026-11-02, cierre F6): RF-1d se amplía de «FALTA o DIFERENTE» a «FALTA, DIFERENTE u OCUPADO no registrado» (un OCUPADO sin `excepcion:` cuenta como pendiente, según RF-4 y RF-8). Ampliación aprobada por el usuario; no pasó por `/sdd-change`. Decisión 3 del plan.
  - Nota (2026-11-02, cierre F6): RF-4 no aplica cuando no hay versión legible (sin `.agents/.kit-version` ni `template/`): `/sdd-enlaces` crea los enlaces, no escribe «visto» e informa que falta el marcador. Decisión 11 del plan.
- RF-2 (contenido y límite del aviso): EL AVISO tendrá 2-3 líneas, aparecerá como máximo una vez por sesión (aunque la persona lo posponga) e indicará qué trae la actualización. Sin changelog en el proyecto, listará las piezas nuevas y retiradas, truncadas a las primeras N con «y X más» (N se fija en el plan), sin atribuirlas a un feature.
- RF-3 («visto» local): EL SISTEMA GUARDARÁ «visto» solo en `.agents/local/kit-visto`, local por máquina y no versionado, y solo `/sdd-enlaces` lo escribirá; NO MODIFICARÁ ningún archivo versionado al registrarlo. SI `.agents/local/` no existe, `/sdd-enlaces` la creará previa confirmación. ANTES de escribir «visto», `/sdd-enlaces` verificará con `git check-ignore` que la ruta está ignorada por Git; si no lo está, propondrá añadir la regla a `.gitignore` y solo escribirá «visto» tras la confirmación de la persona.
- RF-4 (cuándo se marca «visto»): CUANDO `/sdd-enlaces` termina con éxito, EL SISTEMA ACTUALIZARÁ «visto» con la `.kit-version`, las piezas actuales, las herramientas declaradas y las excepciones. Esto incluye el caso sin nada que corregir. Éxito = la re-verificación queda OK sobre las herramientas declaradas (un OCUPADO registrado como excepción no cuenta como pendiente). Con rechazo de la persona, fallo parcial o fallo de escritura, NO se marca «visto» y el aviso se repite en la siguiente sesión.
- RF-5 (herramientas declaradas): ANTES de crear o corregir enlaces, `/sdd-enlaces` PREGUNTARÁ qué herramientas usa la persona, las registrará en «visto» y solo tratará los enlaces de esas; sin herramientas declaradas no crea ninguno y omite los enlaces ya OK. «Todo OK» (RF-1d y RF-4) se mide solo sobre las herramientas declaradas; el paso 7 ignora las demás filas de la tabla.
- RF-6 (comandos y ejecución): CUANDO existen enlaces FALTA o DIFERENTE de herramientas declaradas, `/sdd-enlaces` mostrará un comando exacto por enlace. Criterio de elección: enlace simbólico (`ln -s` en macOS/Linux; en Windows si hay permiso) -> junction (Windows sin administrador) -> copia solo si los anteriores fallan, con aviso de que no se sincroniza. El agente ejecutará el comando mostrado tras la confirmación; no genera ni versiona scripts (ADR 004).
- RF-7 (confirmaciones): `/sdd-enlaces` PEDIRÁ una confirmación explícita por tipo de acción y no ejecutará nada sin ella: crear enlaces, reemplazar un enlace DIFERENTE (mostrando destino actual y esperado) y retirar un enlace colgante. Lo mismo aplica a las acciones que asume de `sync-agents.md` (RF-10).
- RF-8 (OCUPADO): CUANDO un destino está OCUPADO, EL SISTEMA NO LO BORRARÁ NI MOVERÁ, lo informará y `/sdd-enlaces` preguntará si es intencional. Si lo es, lo registrará como excepción en «visto» y deja de contar como pendiente; si no, queda pendiente y no se marca «visto». Se continúa con las demás herramientas.
- RF-9 (enlaces colgantes): CUANDO un enlace apunta a una pieza retirada, `/sdd-enlaces` lo reportará y propondrá retirar solo el enlace (confirmación según RF-7). Nunca borra contenido real.
- RF-10 (pasos de `sync-agents.md`): LOS pasos destructivos de `.agents/sync-agents.md` (paso 7: `git rm --cached` de archivos ignorados; paso 8: réplicas huérfanas o faltantes) PASAN a ejecutarse desde `/sdd-enlaces`. Sus reglas siguen en `sync-agents.md` (única fuente); `/sdd-enlaces` las referencia sin copiarlas (constitución §3). El paso 7 del arranque queda solo lectura y remite a `/sdd-enlaces`.
- RF-11 (separación lectura/escritura): `/sdd-estructura` y el chequeo del paso 7 serán solo lectura y NO MODIFICARÁN enlaces ni archivos; la única escritura (enlaces, `.gitignore` por RF-3, «visto») es de `/sdd-enlaces`.
- RF-12 (resultado): CUANDO termina de aplicar, `/sdd-enlaces` RE-VERIFICARÁ y reportará el estado resultante por herramienta. Una copia se reporta como «OK (copia)», sin sincronización. Un fallo parcial se informa por herramienta y se continúa con las demás.
- RF-13 (qué hacer tras un pull): EL KIT DOCUMENTARÁ en `ACTUALIZAR.md` (raíz, que es la fuente; si se replica en `template/` se decide en el plan) una sección corta «Qué hacer después de un pull que actualiza el kit»: no se corre `/sdd-init`, se abre sesión para recibir el aviso, se corre `/sdd-enlaces`, `/sdd-estructura` sirve para comprobar, y `PREFERENCIAS.md` se crea si no existe (paso 10). Como el proyecto no incluye `ACTUALIZAR.md`, el aviso del paso 7 traerá los pasos mínimos en una línea y remitirá al kit original para el detalle.
- RF-14 (pieza nueva y sin scripts): `/sdd-enlaces` es una pieza nueva en `template/.agents/commands/`, replicada en el clon y citada en `AGENTS.md` y `template/AGENTS.md`. EL KIT NO INCLUIRÁ scripts versionados ni hook Git post-merge como pieza propia (ADR 004); el hook podrá mencionarse solo como opción manual de cada persona.

## Requisitos no funcionales
- Solo Markdown, sin scripts ni binarios versionados (ADR 004, constitución §5).
- Avisos breves y no repetitivos: un mensaje combinado por sesión.
- Genérico: sin supuestos de stack (constitución §1); el cambio se hace en `template/` y se replica en el clon (constitución §3), cada regla en una sola fuente.
- Nunca sobrescribe ni borra contenido existente (constitución §5); solo se reemplaza o retira un enlace, con confirmación.
- UTF-8 sin BOM, en español.
- Versión 0.6.0 compartida con 0008 y 0010. `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` se actualizan (manifiesto y changelog) una sola vez al final, tras cerrar 0009 (constitución §6).
- Verificado antes de cerrar (constitución §4).

## Casos límite
- Clon nuevo o `.agents/local/` inexistente: el paso 7 no escribe; `/sdd-enlaces` crea la carpeta con confirmación (RF-3).
- «visto» obsoleto por proyecto movido de carpeta: se trata como una diferencia normal.
- Cambio de rama con otra `.kit-version`: mismo aviso.
- Dos ejecuciones de `/sdd-enlaces` en paralelo (dos sesiones): cada una escribe «visto» completo al terminar; gana la última escritura, sin daño.
- La persona rechaza la oferta: no se modifica nada, no se marca «visto» y no se insiste en la sesión; el aviso vuelve en la siguiente.
- Windows sin permiso de enlace simbólico: junction; si no es posible, copia con aviso.
- Fallo de escritura en `.agents/local/`: se informa y no se marca «visto».
- Cambios concurrentes de las specs 0008 y 0010 sobre `AGENTS.md` y `template/AGENTS.md`: riesgo de conflicto de edición; orden de fusión: 0008, luego 0010, luego 0009.
- Múltiples worktrees o clones en la misma máquina: cada uno tiene su «visto» propio.

## Fuera de alcance
- Hook Git post-merge como pieza del kit (ADR 004); solo mención manual.
- Scripts de instalación o sincronización versionados.
- Re-ejecución de `/sdd-init` o cambios a su comportamiento: `/sdd-init` no escribe «visto»; el primer `/sdd-enlaces` lo crea.
- Sincronizar o migrar contenido de proyectos más allá de los enlaces (lo cubre `ACTUALIZAR.md`).
- Cambios a la tabla de herramientas de `sync-agents.md`: ya incluye Copilot y deja Kiro fuera.
- Cambios a las reglas E1-E20 de validación más allá de lo necesario para separar validar de aplicar.

## Criterios de finalización
- CF-1: tras un pull que cambia la versión o las piezas, o con enlaces pendientes de herramientas declaradas, o en un clon nuevo, la siguiente sesión muestra un único aviso combinado, breve y con límite, que recomienda `/sdd-enlaces` (RF-1, RF-2).
- CF-2: «visto» es local, no versionado, ignorado por Git (verificado con `git check-ignore`) y solo lo escribe `/sdd-enlaces` al terminar con éxito, incluso sin nada que corregir; rechazo o fallo no lo marcan (RF-3, RF-4).
- CF-3: solo se tratan las herramientas declaradas; el paso 7 ignora las demás (RF-5).
- CF-4: `/sdd-enlaces` crea, reemplaza o retira enlaces con una confirmación por tipo de acción y el comando adecuado para Windows y macOS/Linux, y reporta el estado resultante (RF-6, RF-7, RF-9, RF-12).
- CF-5: un destino OCUPADO o DIFERENTE nunca pierde contenido real; un OCUPADO intencional queda como excepción (RF-8, RF-7).
- CF-6: `/sdd-estructura` y el paso 7 son solo lectura; los pasos destructivos de `sync-agents.md` se ejecutan desde `/sdd-enlaces` con sus reglas en una sola fuente (RF-10, RF-11).
- CF-7: queda documentado qué hace el equipo tras un pull y que `/sdd-init` no se vuelve a correr (RF-13).
- CF-8: no se agrega ningún script versionado; `/sdd-enlaces` existe en `template/` y en el clon y se cita en `AGENTS.md`; versión 0.6.0 y changelog actualizados una vez al final (RF-14).

## Cambio 1 (2026-11-03): E1 y E7
Estado del cambio: aprobado por el usuario (2026-11-03 21:28). La spec original sigue aprobada; lo anterior no se modifica. Las decisiones del usuario ya están incorporadas como requisitos.

Motivo: al cerrar F5.2 y F6.9 quedaron sin marcar dos FALLA preexistentes de `/sdd-estructura` (fallan igual en HEAD). El usuario decidió corregirlas dentro de 0009. Esto acota la exclusión «Cambios a las reglas E1-E20» de «Fuera de alcance» solo para E1 y E7; RF-17 y RF-18 son coherencias de texto que el mismo cierre dejó pendientes.

- RF-15 (E7, skills en la tabla): LA REGLA E7 de `validar-estructura.md` EXIGIRÁ fila en «Skills de proyecto» de `AGENTS.md` para cada skill de `.agents/skills/`, EXCEPTO en el repo del kit (el que tiene `template/`), donde toda skill presente en `.agents/skills/` y ausente de `template/.agents/skills/` queda exenta (no exige fila ni es FALLA). En un proyecto instalado (sin `template/`) la exención no aplica. La raíz `AGENTS.md` añadirá las 3 filas del kit (`adr`, `migracion-bd`, `seguridad-basica`), iguales a las de `template/AGENTS.md`. NO se añadirá fila versionada para `caveman` ni para otra skill local.
- RF-16 (E1, enlaces sin «visto»): CUANDO no hay «visto» legible, LA REGLA E1 MEDIRÁ todas las filas de `sync-agents.md` con esta severidad: `DIFERENTE` y `OCUPADO` sin `excepcion:` son FALLA; `FALTA` es AVISO (herramienta no declarada, porque sin «visto» ninguna lo está) con el texto exacto «enlace FALTA de herramienta no declarada: correr `/sdd-enlaces`». Con «visto» legible E1 no cambia (RF-5). `/sdd-enlaces` no cambia: sigue sin escribir «visto» cuando no hay versión legible (decisión 11 del plan, nota de RF-4).
- RF-17 (aviso, línea 2): `sync-agents.md` › «Chequeo de arranque» ACLARARÁ que los enlaces pendientes van en la línea 2 del aviso, y su ejemplo será coherente con ello. Hoy el paso 4 (`template/.agents/sync-agents.md:108-111`) manda los enlaces pendientes a la línea 2, pero el ejemplo (líneas 116-118) los pone en la línea 1 («...faltan 2 enlaces de claude.»), con la línea 2 solo como recomendación.
- RF-18 (git-commit remite a sync-agents): el paso 3 de `skills/git-commit/SKILL.md` (`template/.agents/skills/git-commit/SKILL.md:43-45`) REMITIRÁ al paso 7 de `sync-agents.md` (regla de versionados ignorados, `sync-agents.md:35-39`) en vez de repetir `git ls-files -ci --exclude-standard` y la propuesta de `git rm --cached`; una sola fuente (constitución §3). Réplica: `.agents/skills/git-commit/SKILL.md:43-45`, idéntica hoy.

Casos límite del cambio:
- Clon con una skill local nueva: E7 no falla; en un proyecto instalado (sin `template/`) toda skill exige fila.
- Skill local con el mismo nombre que una del kit: cuenta como del kit (está en `template/`) y exige fila.
- Fila de `AGENTS.md` que apunta a una skill inexistente sigue siendo FALLA (no se relaja).
- Máquina del mantenedor sin `.gemini/` ni `.github/` (no se crean; decisión del usuario): sus 7 filas FALTA (3 de Gemini y 4 de GitHub) quedan como AVISO y ningún FALLA; un destino OCUPADO o DIFERENTE en la misma máquina sigue siendo FALLA.
- Proyecto con «visto» corrupto: se trata como sin «visto» legible (RF-1b), mismo criterio de RF-16.

Criterios de finalización del cambio:
- CF-9: `/sdd-estructura` en el clon no reporta FALLA de E7 por `adr`, `migracion-bd`, `seguridad-basica` ni `caveman`, y E1 no reporta FALLA por filas FALTA; conserva FALLA para DIFERENTE y OCUPADO (RF-15, RF-16).
- CF-10: `validar-estructura.md` es idéntico en `template/` y el clon; el manifiesto 0.6.0 tiene las huellas recalculadas (E20) y el changelog 0.6.0 menciona E1 y E7 (RF-15, RF-16).
- CF-11: el ejemplo del «Chequeo de arranque» pone los enlaces pendientes en la línea 2, como indica el paso 4 (RF-17).
- CF-12: `git-commit/SKILL.md` no repite el comando de versionados ignorados y remite al paso 7 de `sync-agents.md`; réplicas idénticas (RF-18).

Archivos tocados (con réplica):
- `template/.agents/validar-estructura.md` y `.agents/validar-estructura.md` (RF-15, RF-16).
- `AGENTS.md` raíz (3 filas de RF-15).
- `template/.agents/sync-agents.md` y `.agents/sync-agents.md` (RF-17).
- `template/.agents/skills/git-commit/SKILL.md` y `.agents/skills/git-commit/SKILL.md` (RF-18).
- `manifiestos/0.6.0.md`: huellas de `validar-estructura`, `sync-agents` y `git-commit`.
- `ChangelogAgentKitSdd.md`: una línea en 0.6.0.

Estimado del cambio: spec ya hecha (0 h); `02`/`03` 0,7 h; `04` 0,3 h; implementación ~1,2 h (RF-17 y RF-18 suman 0,4 h); verificación 0,4 h. Total pendiente: 2,6 h.

Alternativas descartadas: E7 con fila versionada para `caveman` o con lista de exenciones fija; E1 que trate toda fila FALTA sin «visto» como FALLA o que escriba «visto» desde `/sdd-estructura`; crear `.gemini/` y `.github/` en la máquina del mantenedor.

Fuera de alcance del cambio: crear enlaces en la máquina del mantenedor (acción local, no versionada); declarar herramientas en un «visto» del clon; reglas E2-E6, E8-E21.

## Dudas abiertas
Ninguna. Las dudas del Cambio 1 quedaron cerradas con su aprobación (2026-11-03 21:28).
