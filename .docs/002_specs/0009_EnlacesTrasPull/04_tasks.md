# 04 — Tareas: Enlaces tras un pull
Tareas: aprobadas por el usuario 2026-11-01 14:51. La implementación espera a que 0010 tenga plan y tareas (orden de fusión 0008, 0010, 0009).
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Reglas comunes a todos los pasos (no se repiten):
- Se edita en `template/` y se replica el mismo cambio en el clon (`.agents/`) en el mismo paso o en el paso de réplica indicado. Edit mínimo; releer cada archivo antes de editar.
- **Lint(archivo)** = `AGENTS.md` › Comandos, Lint / sintaxis: `file -i <archivo>` da utf-8; `grep -l $'^\xEF\xBB\xBF' <archivo>` vacío; `name` del frontmatter = carpeta; rutas citadas existentes; en `template/` sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- Convenciones de archivo: UTF-8 sin BOM, español.
- Build: No aplica. Evidencia de datos: No aplica. Verificación manual UI: No aplica (kit de Markdown, sin UI ni BD), salvo donde se indique revisión de texto.
- Sin scripts: nada `.sh`, `.ps1` ni `.py` (ADR 004).
- Ningún paso hace commit.

## F0 — Diagnóstico y preparación (estimado: 0.5 h)
- [x] **F0.1 Confirmar la rama base: 0008 y 0010 fusionadas (E21).** CF-8
  - Hecho cuando:
    - [ ] `git log --oneline` muestra 0008 y 0010 en la rama base; `grep -n "E21" template/.agents/validar-estructura.md` encuentra la regla
    - [ ] **Condición de bloqueo**: si 0010 no está (sin E21), marcar `[!]`, avisar al usuario, no editar los textos «E1 a E20/E21» ni ejecutar F6; avanzar solo con F1 y lo que no colisione; reabrir F0 (rebase y releer) cuando 0010 se fusione
    - [ ] Evidencia de datos: No aplica · UI: No aplica
- [x] **F0.2 Releer los archivos de colisión tras 0008 y 0010.** RF-1, RF-2, RF-11, RF-14
  - Archivos: `AGENTS.md`, `template/AGENTS.md`, `template/.agents/validar-estructura.md`, `template/.agents/commands/sdd-estructura.md`, `template/.agents/commands/sdd-init.md`, `template/.agents/skills/sdd/SKILL.md`, `template/.agents/commands/sdd.md`, `template/.agents/agents/coordinator.md`
  - Hecho cuando:
    - [ ] Anotado en `05_avance.md` dónde aparece «E1 a E20/E21» y el texto vigente del paso 7 en ambos `AGENTS.md`
    - [ ] `grep -rn "E1 a E20\|E1–E20" template .agents AGENTS.md` anotado (lista de ocurrencias a cambiar en F2/F3)
    - [ ] UTF-8 sin BOM sin cambios (solo lectura)
- [x] **F0.3 Estado de `.gitignore` y réplica base.** RF-3
  - Hecho cuando:
    - [ ] `git check-ignore -v .agents/local/kit-visto` imprime la regla (línea 4 de `.gitignore`); `grep -n "agents/local" template/.gitignore` la encuentra
    - [ ] `diff -rq template/.agents .agents` anotado como línea base (solo `napkin.md`; `caveman` y `local/` fuera)

## F1 — `/sdd-enlaces` (estimado: 2.5 h)
Archivo: `template/.agents/commands/sdd-enlaces.md` (nuevo); réplica `.agents/commands/sdd-enlaces.md`. Estructura según plan §6.3. No copia reglas de `sync-agents.md`: remite.
- [x] **F1.1 Frontmatter, «Qué lee» y «Preparar» (pasos 1–2).** RF-3, RF-14
  - Hecho cuando:
    - [ ] Frontmatter con `description` y `argument-hint` vacío; indica sesión principal, sin subagente, sin `05_avance.md`
    - [ ] Lee `.agents/sync-agents.md` y `.agents/local/kit-visto`; detecta SO/shell y `git rev-parse --is-inside-work-tree` (decisión 9)
    - [ ] Lint(`template/.agents/commands/sdd-enlaces.md`) OK
- [x] **F1.2 Pasos 3–4: preguntar herramientas y clasificar.** RF-5, RF-8, RF-9
  - Hecho cuando:
    - [ ] Cero herramientas termina sin crear ni escribir «visto» (decisión 7); clasifica solo filas de las herramientas declaradas; detecta colgantes, réplicas e ignorados versionados remitiendo a `sync-agents.md`
    - [ ] `grep -n "ls-files -ci" template/.agents/commands/sdd-enlaces.md` vacío (no copia la regla)
    - [ ] Lint OK
- [x] **F1.3 Paso 5: plan con comando exacto y orden de confirmaciones.** RF-6, RF-7
  - Hecho cuando:
    - [ ] Un comando por enlace (symlink, junction, copia, criterio de `sync-agents.md`); una confirmación por tipo, nunca por enlace, en el orden (1)–(9) de la decisión 10; valida rutas contra la tabla de `sync-agents.md`
    - [ ] Lint OK
- [x] **F1.4 Pasos 6–7: ejecutar y re-verificar.** RF-7, RF-8, RF-9, RF-12
  - Hecho cuando:
    - [ ] Ejecuta solo lo confirmado, una acción a la vez; fallo parcial se anota y continúa; OCUPADO nunca se borra y se pregunta «¿es intencional?» (registra `excepcion:`); copia se anota como `copia: <ruta>`
    - [ ] Reporte por herramienta con `OK`, `OK (copia)`, FALLA o excepción
    - [ ] Lint OK
- [x] **F1.5 Paso 8 y «Qué no hace».** RF-3, RF-4, RF-5, RF-14
  - Hecho cuando:
    - [ ] Todo OK: crea `.agents/local/` (confirmado), `check-ignore`, edita `.gitignore` si falta la regla, escribe «visto» completo de una vez; si no, no escribe y avisa que el aviso volverá
    - [ ] Rama sin `.agents/.kit-version` ni `template/`: crea enlaces, NO escribe «visto» e informa que falta el marcador (ver `ACTUALIZAR.md` › Marcador de versión)
    - [ ] «Qué no hace»: sin scripts, sin `/sdd-init`, sin commit, sin tocar contenido real ni `AGENTS.md`
    - [ ] Lint OK
- [x] **F1.6 Réplica en el clon.** RF-14
  - Hecho cuando:
    - [ ] `cp template/.agents/commands/sdd-enlaces.md .agents/commands/sdd-enlaces.md`; `diff template/.agents/commands/sdd-enlaces.md .agents/commands/sdd-enlaces.md` vacío
    - [ ] Lint(`.agents/commands/sdd-enlaces.md`) OK

## F2 — `sync-agents.md`, `validar-estructura.md`, `sdd-estructura.md` (estimado: 2.0 h)
- [x] **F2.1 `sync-agents.md`: sección «Estado «visto»» (formato).** RF-3
  - Archivo: `template/.agents/sync-agents.md` (réplica en F2.5)
  - Hecho cuando:
    - [ ] Formato según plan §6.3: prefijos `version`, `herramientas`, `excepcion`, `copia`, `pieza`; orden; corrupto = inexistente; escritura completa de una vez; sin `version: ninguna`
    - [ ] Lint OK
- [x] **F2.2 `sync-agents.md`: sección «Chequeo de arranque».** RF-1, RF-2
  - Hecho cuando:
    - [ ] Se omite si falta `.agents/.kit-version` y existe `template/`; si no, compara versión (incluye retroceso), piezas nuevas/retiradas, y FALTA, DIFERENTE u OCUPADO sin `excepcion:`; `copia:` = `OK (copia)`; ausente o corrupto = todo nuevo
    - [ ] Aviso de ≤3 líneas, N = 5 con «y X más», sin leer changelog, una vez por sesión, con pasos mínimos y sin mencionar réplicas ni ignorados versionados
    - [ ] Lint OK
- [x] **F2.3 `sync-agents.md`: pasos 1–6 (lectura y qué muestra `/sdd-enlaces`).** RF-7, RF-10
  - Hecho cuando:
    - [ ] Numeración 1–8 conservada; pasos 1–2 = lectura del arranque; 3–6 redactados como lo que `/sdd-enlaces` muestra y pregunta
    - [ ] Lint OK
- [x] **F2.4 `sync-agents.md`: pasos 7–8 y disparador.** RF-7, RF-10
  - Hecho cuando:
    - [ ] 7–8 como reglas que ejecuta tras confirmar; disparador «al agregar pieza» dice «sugerir `/sdd-enlaces`»
    - [ ] `grep -n "paso 7\|paso 8" template/.agents/sync-agents.md template/.agents/README.md template/.agents/validar-estructura.md` revisado: sin referencias rotas
    - [ ] Lint OK
- [x] **F2.5 Réplica de `sync-agents.md`.** RF-10
  - Hecho cuando:
    - [ ] `cp template/.agents/sync-agents.md .agents/sync-agents.md`; `diff` vacío; Lint(`.agents/sync-agents.md`) OK
- [x] **F2.6 `validar-estructura.md`: E1, E19, «Cómo responder» 4 y E21.** RF-11
  - Archivo: `template/.agents/validar-estructura.md`
  - Hecho cuando:
    - [ ] E1 se mide sobre herramientas declaradas en «visto» (no declaradas, `excepcion:` y `copia:` = AVISO); E19 ignora `local/`; «Cómo responder» 4 remite a `/sdd-enlaces`; «E1 a E20» y «E1–E20» (ambas formas) pasan a «E1 a E21» / «E1–E21»; ningún otro chequeo cambia
    - [ ] Lint OK
- [x] **F2.7 `sdd-estructura.md`: solo propone y remite.** RF-11
  - Archivo: `template/.agents/commands/sdd-estructura.md`
  - Hecho cuando:
    - [ ] `grep -n "aplícala" template/.agents/commands/sdd-estructura.md` vacío; remite a `/sdd-enlaces` para enlaces y réplicas; «E1 a E20» y «E1–E20» (ambas formas) pasan a «E1 a E21» / «E1–E21»
    - [ ] Lint OK
- [x] **F2.8 Réplicas de `validar-estructura.md` y `sdd-estructura.md`.** RF-11
  - Hecho cuando:
    - [ ] `cp` de ambos a `.agents/` y `diff -rq template/.agents .agents` sin diferencias nuevas (solo `napkin.md`); Lint de ambas réplicas OK

## F3 — `AGENTS.md`, README, SKILL, coordinator, `sdd.md`, `sdd-init.md` (estimado: 1.5 h)
- [x] **F3.1 `template/AGENTS.md`: paso 7 y `/sdd-enlaces` en «Roles y comandos».** RF-1, RF-2, RF-13, RF-14
  - Hecho cuando:
    - [ ] Paso 7 con el texto del plan §6.3 (solo lectura, remite a `sync-agents.md` › «Chequeo de arranque», recomienda `/sdd-enlaces`, no `/sdd-init`); paso 8 intacto; zonas de 0008/0010 sin tocar
    - [ ] `grep -n "sdd-enlaces" template/AGENTS.md` ≥ 2; Lint OK; marcadores `<<…>>` solo los permitidos
- [x] **F3.2 `AGENTS.md` (raíz): mismo cambio.** RF-1, RF-2, RF-13, RF-14
  - Hecho cuando:
    - [ ] Mismo cambio conservando diferencias propias de la raíz (paso 0, etc.); `grep -rn "sdd-enlaces" AGENTS.md template/AGENTS.md` en ambos; Lint(`AGENTS.md`) OK
- [x] **F3.3 README del layout y réplica.** RF-10, RF-14
  - Archivos: `template/.agents/README.md` → `.agents/README.md`
  - Hecho cuando:
    - [ ] Árbol con `local/kit-visto`; citas a «paso 8» (líneas ~59, 66, 75) pasan a «sugerir `/sdd-enlaces`»; «Validar la estructura» menciona `/sdd-enlaces`
    - [ ] Réplica idéntica (`diff` vacío); Lint de ambos OK
- [x] **F3.4 SKILL de `sdd` y réplica.** RF-14
  - Archivos: `template/.agents/skills/sdd/SKILL.md` → `.agents/skills/sdd/SKILL.md`
  - Hecho cuando:
    - [ ] Fila `Enlaces | /sdd-enlaces | sesión principal | —` tras `/sdd-estructura`; «E1 a E20» → «E1 a E21»; réplica idéntica; `name` = carpeta; Lint OK
- [x] **F3.5 `coordinator.md` y réplica.** RF-14
  - Archivos: `template/.agents/agents/coordinator.md` → `.agents/agents/coordinator.md`
  - Hecho cuando:
    - [ ] Fila de ruteo «Pide crear o corregir enlaces…» → `sdd-enlaces.md`; frase «lo ejecuta la sesión principal»; «E1–E20» → «E1–E21»; réplica idéntica; Lint OK
- [x] **F3.6 `sdd.md` y réplica.** RF-14
  - Archivos: `template/.agents/commands/sdd.md` → `.agents/commands/sdd.md`
  - Hecho cuando:
    - [ ] `sdd-enlaces` en la lista de comandos; «E1 a E20» → «E1 a E21»; réplica idéntica; Lint OK
- [x] **F3.7 `reviewer.md` y réplica.** RF-14
  - Archivos: `template/.agents/agents/reviewer.md` → `.agents/agents/reviewer.md`
  - Hecho cuando:
    - [ ] «E1–E20» → «E1–E21» (línea 21, «Qué no hace»); nada más; réplica idéntica (`diff` vacío); Lint de ambos OK
- [x] **F3.8 `sdd-init.md` y réplica.** RF-14
  - Archivos: `template/.agents/commands/sdd-init.md` → `.agents/commands/sdd-init.md`
  - Hecho cuando:
    - [ ] Paso 4.3: «sugiere correr `/sdd-enlaces`» (no escribe «visto»); 4.2/4.3: «E1 a E21»; réplica idéntica; Lint OK
    - [ ] `grep -rn "E1 a E20\|E1–E20" template .agents AGENTS.md` vacío (incluye `reviewer.md` en `template/.agents/agents/` y `.agents/agents/`)

## F4 — `ACTUALIZAR.md` e `INSTALAR.md` (estimado: 1.0 h)
Estos manuales son de la raíz y no se replican en `template/` (decisión 14).
- [x] **F4.1 `ACTUALIZAR.md`: sección «Qué hacer después de un pull que actualiza el kit» e índice.** RF-13
  - Hecho cuando:
    - [x] Sección antes de `## Enlaces`, con los 6 puntos del plan §6.3 (incluye «no correr `/sdd-init`») y entrada en el índice; consigna la nota del grupo B (paso 7 de `AGENTS.md` a mano en proyectos existentes)
    - [x] Lint(`ACTUALIZAR.md`) OK
- [x] **F4.2 `ACTUALIZAR.md`: `## Enlaces`, `## Huérfanos` y `## Marcador de versión`.** RF-4, RF-10
  - Hecho cuando:
    - [x] `## Enlaces`: verifica solo lectura y remite a `/sdd-enlaces`; `## Huérfanos`: `.agents/local/` no es huérfano
    - [x] `## Marcador de versión` (hoy línea 189) recibe un párrafo para el caso sin `.kit-version` ni `template/`: aviso «todo nuevo»; `/sdd-enlaces` crea enlaces, no escribe «visto» e informa que falta el marcador. Se aclara que no contradice «asume 0.4.0», que es la suposición del flujo de actualización (con confirmación), mientras que `/sdd-enlaces` no usa versión supuesta
    - [x] `grep -n "^## Marcador de versión" ACTUALIZAR.md` encuentra el encabezado exacto citado por `/sdd-enlaces` (si no existe, avisar al usuario antes de seguir)
    - [x] Lint OK
- [x] **F4.3 `INSTALAR.md` (línea ~112): «Propone `/sdd-enlaces`».** RF-14
  - Hecho cuando:
    - [x] Frase cambiada y nada más; Lint(`INSTALAR.md`) OK

## F5 — Verificación y ensayo (estimado: 3.5 h)
Los resultados por RF se anotan en `05_avance.md` (decisión 8).
- [x] **F5.1 Lint de todos los archivos tocados.** todos
  - Hecho cuando:
    - [ ] Lint OK en cada archivo de F1–F4; `grep -rn "<<" template | grep -v "NOMBRE_PROYECTO\|IDIOMA"` vacío
- [x] **F5.2 Estructura y sin scripts.** RF-11, RF-14 (CF-8)
  - Hecho cuando:
    - [ ] `diff -rq template/.agents .agents` solo `napkin.md` (el `caveman` del clon es ajeno)
    - [ ] `find template \( -name "*.sh" -o -name "*.ps1" -o -name "*.py" \)` vacío
    - [ ] `/sdd-estructura` (E1–E21) sin FALLA; solo lectura. Si E21 no está (0010 sin fusionar), correr E1–E20 y anotarlo en `05_avance.md`
- [x] **F5.3 Ensayo de instalación.** RF-14
  - Hecho cuando:
    - [ ] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia todo sin pisar nada (nunca en un proyecto real)
- [x] **F5.4 Ensayo E-1…E-4 (aviso).** RF-1, RF-2 (CF-1)
  - Hecho cuando:
    - [ ] En proyecto temporal: E-1 clon nuevo, E-2 versión nueva con FALTA, E-3 misma versión con piezas nuevas, E-4 solo enlaces; cada uno un único aviso de ≤3 líneas, N = 5, recomienda `/sdd-enlaces`; sin cambios, silencio
- [x] **F5.5 Ensayo E-5 y E-6 (OCUPADO y rechazo).** RF-6, RF-7, RF-8, RF-9 (CF-3, CF-4)
  - Hecho cuando:
    - [ ] E-5 OCUPADO intencional y no (`ls` antes/después igual); E-6 rechazo no crea «visto»
    - [ ] Una confirmación por tipo en el orden de la decisión 10
- [x] **F5.6 Ensayo E-7…E-9 (Git, cero herramientas, escritura de «visto»).** RF-3, RF-4, RF-5, RF-12 (CF-2, CF-5)
  - Hecho cuando:
    - [ ] E-7 sin Git; E-8 cero herramientas no crea «visto»; E-9 dos herramientas distintas (con solo `claude` no se crea `.cursor/`)
    - [ ] `git check-ignore -v .agents/local/kit-visto` imprime la regla; escenario con `.gitignore` sin la regla propone añadirla; escenario de copia reporta `OK (copia)`
- [x] **F5.7 Ensayo E-10 y E-11 (sin marcador).** RF-1, RF-4 (CF-1, CF-2)
  - Hecho cuando:
    - [ ] E-10 (sin `.kit-version` y con `template/`, clon del kit): silencio
    - [ ] E-11 (sin `.kit-version` ni `template/`): aviso «todo nuevo», se crean enlaces, no se escribe «visto» y se informa que falta el marcador
    - [ ] Proyecto temporal y también el propio clon del kit
- [x] **F5.8 Trazabilidad RF→verificación en `05_avance.md`.** CF-8
  - Hecho cuando:
    - [ ] Los 14 RF tienen verificación anotada (tabla del plan §6.3) y `grep -n "paso 7\|paso 8" template/AGENTS.md template/.agents/sync-agents.md` confirma reglas en una sola fuente

## F6 — Cierre (estimado: 1.5 h)
Requiere 0008 y 0010 fusionadas (ver F0.1). Si no lo están, `[!]`.
- [x] **F6.1 Verificar prerrequisitos.** CF-8
  - Hecho cuando:
    - [ ] 0008 y 0010 en la rama base; `diff -rq template/.agents .agents` solo `napkin.md`
- [x] **F6.2 Bump 0.6.0 en `VersionAgentKitSdd`.** CF-8
  - Hecho cuando:
    - [ ] Contiene `0.6.0`; una sola vez para las tres specs; sin BOM
- [x] **F6.3 `ChangelogAgentKitSdd.md`.** CF-8, RF-13
  - Hecho cuando:
    - [ ] Entrada 0.6.0 con 0008, 0010 y 0009, incluida la nota del grupo B (el paso 7 de `AGENTS.md` se incorpora a mano en proyectos existentes); Lint OK
- [x] **F6.4 `manifiestos/0.6.0.md` (E20).** CF-8
  - Hecho cuando:
    - [ ] Huellas calculadas con el algoritmo de `ACTUALIZAR.md` tras el último cambio de 0008, 0010 y 0009; E20 sin FALLA (los manifiestos pueden listar rutas retiradas)
- [x] **F6.5 ADR-A con la skill `adr`.** RF-3, RF-4
  - Hecho cuando:
    - [ ] «Estado local `kit-visto` y aviso al arranque; solo `/sdd-enlaces` escribe», siguiente número libre (hoy el último es 0018; 0008/0010 pueden tomar antes), alternativas: hook post-merge, marcador versionado, sin estado; Lint OK
- [x] **F6.6 ADR-B con la skill `adr`.** RF-10
  - Hecho cuando:
    - [ ] «El arranque ya no detecta réplicas ni ignorados versionados», número siguiente a ADR-A, alternativa: aviso de solo lectura; Lint OK
- [x] **F6.7 Nota en `01_spec.md`: RF-1d ampliado y RF-4 sin versión legible.** RF-1, RF-4
  - Hecho cuando:
    - [ ] RF-1d: «FALTA, DIFERENTE u OCUPADO no registrado», aprobado por el usuario, sin `/sdd-change`; junto a ella, nota: RF-4 no aplica sin versión legible (sin `.kit-version` ni `template/` se crean enlaces y no se escribe «visto»); Lint OK
- [x] **F6.8 Fila del índice de specs.** CF-8
  - Hecho cuando:
    - [ ] `.docs/002_specs/README.md` tiene la fila de 0009 con estado y enlace existente (solo esa fila); Lint OK
- [x] **F6.9 Chequeo final.** CF-8
  - Hecho cuando:
    - [ ] `/sdd-estructura` (E1–E21) sin FALLA; `diff -rq template/.agents .agents` solo `napkin.md`; `find` de scripts vacío
    - [ ] Si E21 no está (0010 sin fusionar): correr E1–E20 y dejar F6.9 `[!]`

## F7 — Delta Cambio 1: E1, E7, aviso y git-commit (estimado: 1.6 h)
Spec: `01_spec.md` › «Cambio 1»; plan: `03_plan.md` › «Delta Cambio 1» (decisiones D-1 a D-7). Mismas reglas comunes de arriba. Plan y tareas del delta aprobados por el usuario 2026-11-03 con los 5 menores del reviewer aplicados.
- [x] **F7.1 `template/.agents/validar-estructura.md`: E1 sin «visto» legible.** RF-16 (0.3 h)
  - Hecho cuando:
    - [ ] La celda de E1 usa el texto de D-1: sin «visto» legible, `FALTA` = `AVISO` con «enlace FALTA de herramienta no declarada: correr `/sdd-enlaces`»; `DIFERENTE`/`OCUPADO` sin `excepcion:` = `FALLA`; con «visto» legible el resto del texto de E1 queda igual
    - [ ] Ningún otro chequeo cambia; Lint(`template/.agents/validar-estructura.md`) OK
- [x] **F7.2 Mismo archivo: E7 con exención y réplica.** RF-15 (0.25 h)
  - Hecho cuando:
    - [ ] E7 añade la exención de D-3 (solo con `template/`; skill en `.agents/skills/` y ausente de `template/.agents/skills/`; no aplica en proyecto instalado); el texto de E7 repite expresamente «una fila a una skill inexistente sigue siendo FALLA» (sin relajar filas a skills inexistentes)
    - [ ] `cp template/.agents/validar-estructura.md .agents/validar-estructura.md`; `diff` vacío; Lint de ambos OK
- [x] **F7.3 `AGENTS.md` raíz: 3 filas de skills.** RF-15 (0.1 h)
  - Hecho cuando:
    - [ ] Releído el archivo; filas `adr`, `migracion-bd`, `seguridad-basica` idénticas a `template/AGENTS.md:98-100`, tras `formato-respuesta`; sin fila de `caveman`; `template/AGENTS.md` sin cambio
    - [ ] `grep -c "skills/adr/SKILL.md\|skills/migracion-bd/SKILL.md\|skills/seguridad-basica/SKILL.md" AGENTS.md` = 3; Lint(`AGENTS.md`) OK
- [x] **F7.4 `sync-agents.md`: ejemplo y línea 2, con réplica.** RF-17 (0.2 h)
  - Hecho cuando:
    - [ ] Ejemplo según D-5 (enlaces pendientes en la línea 2); el paso 4 dice que los enlaces pendientes van en la línea 2; línea 3 y variantes sin cambio, aviso de 3 líneas
    - [ ] `cp` a `.agents/sync-agents.md`; `diff` vacío; Lint de ambos OK
- [x] **F7.5 `skills/git-commit/SKILL.md`: paso 3 remite a `sync-agents.md`, con réplica.** RF-18 (0.2 h)
  - Hecho cuando:
    - [ ] Paso 3 con el texto de D-6; `grep -n "ls-files -ci" template/.agents/skills/git-commit/SKILL.md` vacío; pasos 1, 2 y 4 intactos
    - [ ] El paso 3 deja explícito que git-commit remite al paso 7 de `sync-agents.md`, que es la fuente de la regla y la ejecuta el comando `/sdd-enlaces`; quien commitea sigue ese paso para obtenerla (una sola fuente)
    - [ ] `cp` a `.agents/skills/git-commit/SKILL.md`; `diff` vacío; `name` = carpeta; Lint de ambos OK
- [x] **F7.6 `manifiestos/0.6.0.md` y `ChangelogAgentKitSdd.md`.** CF-10 (0.15 h)
  - Hecho cuando:
    - [ ] Recalculadas las huellas de `.agents/validar-estructura.md`, `.agents/sync-agents.md` y `.agents/skills/git-commit/SKILL.md` con el algoritmo de `ACTUALIZAR.md` › Huella (después de F7.2, F7.4 y F7.5, y solo tras `diff -rq template/.agents .agents` sin diferencias nuevas); solo esas 3 filas cambian en el manifiesto
    - [ ] Una línea nueva en la entrada 0.6.0 del changelog que menciona E1, E7, el ejemplo del aviso y git-commit; Lint de ambos OK
- [x] **F7.7 Verificación de archivos y réplicas.** CF-10, CF-11, CF-12 (0.2 h)
  - Hecho cuando:
    - [ ] Primero: `diff -rq template/.agents .agents` solo `napkin.md` (las réplicas quedan verificadas antes de calcular huellas en F7.6)
    - [ ] E20 recomputado para todos los archivos de `template/` con el algoritmo de `ACTUALIZAR.md`: sin FALLA
    - [ ] CF-10: `grep` en `ChangelogAgentKitSdd.md` confirma que la línea nueva de 0.6.0 menciona E1, E7, el ejemplo del aviso y git-commit
    - [ ] CF-11: el ejemplo de `sync-agents.md` tiene los enlaces pendientes en la línea 2 (revisión del texto o `grep -n`)
    - [ ] `find template \( -name "*.sh" -o -name "*.ps1" -o -name "*.py" \)` vacío
    - [ ] UTF-8 sin BOM (`file -i`, `grep -l $'^\xEF\xBB\xBF'` vacío) en todos los archivos tocados de F7; `grep -rn "<<" template | grep -v "NOMBRE_PROYECTO\|IDIOMA"` vacío
- [x] **F7.8 Reejecutar E1 y E7 y marcar F5.2 y F6.9.** CF-9 (0.2 h)
  - Hecho cuando:
    - [ ] E7 en el clon: sin FALLA por `adr`, `migracion-bd`, `seguridad-basica` ni `caveman`
    - [ ] E1 en el clon sin «visto»: las 7 filas FALTA (Gemini x3, GitHub x4) antes FALLA ahora AVISO con el texto exacto; DIFERENTE/OCUPADO sin excepcion: siguen FALLA
    - [ ] `/sdd-estructura` completo (E1-E21) sin FALLA; F5.2 y F6.9 marcados `[x]` con la fecha; si queda otro FALLA, se deja `[!]` y se avisa al usuario

## Trazabilidad RF/CF → paso
| RF / CF | Pasos |
|---|---|
| RF-1 | F2.2, F3.1, F3.2, F5.4, F5.7, F6.7 |
| RF-2 | F2.2, F3.1, F3.2, F5.4 |
| RF-3 | F0.3, F1.1, F1.5, F2.1, F5.5, F5.6, F6.5 |
| RF-4 | F1.5, F4.2, F5.5, F5.6, F5.7, F6.5, F6.7 |
| RF-5 | F1.2, F1.5, F5.6 |
| RF-6 | F1.3, F1.4, F5.5 |
| RF-7 | F1.3, F1.4, F2.3, F2.4, F5.5 |
| RF-8 | F1.2, F1.4, F5.5 |
| RF-9 | F1.2, F1.4, F5.5 |
| RF-10 | F2.3, F2.4, F2.5, F3.3, F4.2, F5.8, F6.6 |
| RF-11 | F2.6, F2.7, F2.8, F5.2 |
| RF-12 | F1.4, F5.6 |
| RF-13 | F3.1, F3.2, F4.1, F6.3 |
| RF-14 | F1.1, F1.5, F1.6, F3.1–F3.8, F4.3, F5.2, F5.3 |
| CF-1 | F5.4, F5.7 |
| CF-2 | F5.6, F5.7 |
| CF-3 | F1.2, F5.5 |
| CF-4 | F1.3, F1.4, F5.5 |
| CF-5 | F1.4, F5.6 |
| CF-6 | F2.6, F2.7, F5.2, F5.8 |
| CF-7 | F4.1 |
| CF-8 | F5.2, F5.3, F6.1–F6.4, F6.8, F6.9 |
| RF-15 | F7.2, F7.3, F7.8 |
| RF-16 | F7.1, F7.8 |
| RF-17 | F7.4, F7.7 |
| RF-18 | F7.5, F7.7 |
| CF-9 | F7.2, F7.3, F7.8 |
| CF-10 | F7.2, F7.6, F7.7 |
| CF-11 | F7.4, F7.7 |
| CF-12 | F7.5, F7.7 |

## Estimado
| Fase | Pasos | Horas |
|---|---|---|
| F0 | 3 | 0.5 |
| F1 | 6 | 2.5 |
| F2 | 8 | 2.0 |
| F3 | 8 | 1.5 |
| F4 | 3 | 1.0 |
| F5 | 8 | 3.5 |
| F6 | 9 | 1.5 |
| F7 (delta Cambio 1) | 8 | 1.6 |
| Total | 53 | 14.1 |
Nota: F7 suma implementación (1.2 h) y verificación (0.4 h) del delta; las 1.0 h de documentación del delta (02/03 0.7 h, 04 0.3 h) se cuentan aparte (total del delta 2.6 h).
