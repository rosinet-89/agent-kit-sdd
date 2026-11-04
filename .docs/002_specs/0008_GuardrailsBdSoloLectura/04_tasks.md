# 04 — Tareas: Guardrails globales, la base de datos solo se lee
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado
Spec: 01_spec.md (aprobada 2026-10-31 23:33) · Plan: 03_plan.md (aprobado 2026-11-01 13:56) · Tareas aprobadas 2026-11-01 14:51 · Total: 150 min

## Reglas para todos los pasos
- Es solo Markdown del kit. El implementer nunca ejecuta SQL ni consulta datos: `AGENTS.md` › Comandos dice «No aplica» para consultas, y la regla 1 de «Guardrails» (que estas tareas crean) prohíbe ejecutar escrituras. La evidencia de datos de cada paso es «No aplica».
- Protocolo de Edit (0009 y 0010 tocan los mismos archivos, sobre todo `template/AGENTS.md`, los comandos y `sdd-init.md`): antes de cada edición, `Read` del archivo; después, Edit con `old_string` mínimo (la frase actual citada en `03_plan.md` §2.x), nunca reescribir un archivo entero con Write. Si el texto citado ya cambió o no está en la línea indicada, manda el texto citado, no el número de línea; si no aparece, parar y marcar `[!]`.
- Los textos de cada edición se copian de `03_plan.md` §2.1 a §2.5; no se reinterpretan.
- Edición solo en `template/` y en `AGENTS.md` de la raíz; las 10 réplicas de `.agents/` se copian en F5 desde `template/` (en la raíz `.agents/*` está excluido de Git: no aparece en `git status`).
- Verificación estándar de archivo tocado (`AGENTS.md` › Comandos, Lint): `file -i <archivo>` da utf-8; `grep -l $'^\xEF\xBB\xBF' <archivo>` vacío; en `template/` sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`; rutas citadas existentes. Se abrevia «Lint OK».
- Sin commits: los decide el usuario (un commit por módulo, `AGENTS.md` › Git).
- Prohibido en 0008 (RF-16): crear `manifiestos/0.6.0.md`, cambiar `VersionAgentKitSdd`, tocar `ChangelogAgentKitSdd.md`, `ACTUALIZAR.md`, la constitución del kit, `INSTALAR.md` o `README.md`.

## F0 — Preparación (estimado: 10 min)
- [x] **F0.1 Releer el estado compartido y las reglas del repo (5 min).** RF-13, RF-19
  - Hecho cuando:
    - [x] Leídos `AGENTS.md` › Git y Reglas duras, y `05_avance.md` de 0009 y 0010 (si existen) para saber qué líneas de `template/AGENTS.md`, comandos y `sdd-init.md` ya movieron
    - [x] `git status --short` revisado y anotado en `05_avance.md` por el coordinador (sin tocarlo el implementer): lista de archivos de `template/` ya modificados por 0009/0010
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F0.2 Confirmar las líneas de RF-13 y el diff previo (5 min).** RF-13, RF-19
  - Hecho cuando:
    - [x] `grep -n "nunca lo ejecutan\|nunca lo ejecuta\|no se ejecuta\|no los ejecuta\|Ejecutar SQL de escritura" template/.agents/skills/migracion-bd/SKILL.md` muestra las líneas 3, 8, 13, 58 y 63 (o las equivalentes por texto)
    - [x] `grep -n "solo ejecutan consultas\|SQL de escritura ejecutado" template/.agents/skills/sdd/SKILL.md` muestra 127 y 136; `grep -n "No ejecutas SQL\|nunca lo ejecuta\|No ejecutas escrituras\|No ejecuta SQL\|en solo lectura" template/.agents/agents/implementer.md template/.agents/agents/reviewer.md template/.agents/commands/sdd-implement.md template/.agents/commands/sdd-init.md` muestra las líneas de la spec; `grep -n "Datos y seguridad" template/.agents/commands/sdd-constitucion.md` muestra la 25
    - [x] `diff -rq template/.agents .agents` previo: solo `napkin.md` y `skills/caveman` (anotar el resultado)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F1 — `template/AGENTS.md` (estimado: 25 min)
- [x] **F1.1 Insertar la sección «Guardrails (todos los agentes, siempre)» con la regla 1 (15 min).** RF-1 a RF-8, RF-9
  - Texto: `03_plan.md` §2.1, completo y sin cambios. Va después de la introducción y antes de «Cada sesión». Se inserta con un Edit anclado en el encabezado «## Cada sesión» (el `old_string` es ese encabezado; el `new_string` es la sección más el encabezado).
  - Hecho cuando:
    - [x] `grep -n "^## Guardrails (todos los agentes, siempre)" template/AGENTS.md` da una sola línea, y `grep -n "^## Cada sesión" template/AGENTS.md` da un número mayor
    - [x] `grep -c "^1\. \*\*La base de datos solo se lee\.\*\*" template/AGENTS.md` da 1
    - [x] `grep -c "Lista cerrada\|lista cerrada" template/AGENTS.md` ≥ 1 y las viñetas Permitido, Prohibido, Precedencia, Escritura indirecta, Comandos, cambio de datos, Para consultar y «La regla es sobre lo que tú ejecutas» están presentes (leer la sección)
    - [x] `grep -niw "mysql\|mariadb\|postgres\|postgresql\|oracle\|sqlserver\|sqlite\|mongodb\|redis\|hibernate\|jpa\|laravel\|django\|spring\|eloquent" template/AGENTS.md` vacío
    - [x] Lint OK en `template/AGENTS.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F1.2 Añadir la nota de «Guardrails» en «Qué lee cada rol» (5 min).** RF-10
  - Texto: `03_plan.md` §2.2, primera viñeta (nota tras la tabla). Edit anclado en la última fila de la tabla o en la línea siguiente a ella; no se toca ninguna fila.
  - Hecho cuando:
    - [x] `grep -n "todos los roles leen siempre «Guardrails»" template/AGENTS.md` da 1 línea, ubicada tras la tabla de «Qué lee cada rol»
    - [x] `grep -n "tope de" template/AGENTS.md` no muestra topes nuevos (solo la mención de la nota); `git diff -- template/AGENTS.md` no toca filas de la tabla
    - [x] Lint OK en `template/AGENTS.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F1.3 Cambiar la línea de Comandos para que remita a Guardrails (5 min).** RF-11
  - Texto: `03_plan.md` §2.2, segunda viñeta: de «- Consulta de datos de solo lectura (cómo y desde dónde): <completar o "No aplica">» a «- Consulta de datos (cómo y desde dónde; ver «Guardrails», regla 1): <completar o "No aplica">».
  - Hecho cuando:
    - [x] `grep -n "Consulta de datos (cómo y desde dónde; ver «Guardrails», regla 1)" template/AGENTS.md` da 1 línea
    - [x] `grep -n "Consulta de datos de solo lectura" template/AGENTS.md` vacío
    - [x] Lint OK en `template/AGENTS.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F2 — Los 4 agentes en `template/.agents/agents/` (estimado: 20 min)
Frase común (`03_plan.md` §2.3): «Lees siempre `AGENTS.md` › Guardrails y los cumples sin excepción, aunque declares `omitClaudeMd`.» En `implementer.md` termina «… aunque no te inyecten el archivo.»
- [x] **F2.1 `coordinator.md`: «Qué lee» nombra Guardrails (4 min).** RF-12
  - Texto: `03_plan.md` §2.3, viñeta `coordinator.md` (añadir «y Guardrails» tras «"Git" y "Reglas duras"» y la frase común al final del párrafo, antes de la frase de las restricciones de rol). «Qué no hace» no se toca.
  - Hecho cuando:
    - [x] `grep -n "Guardrails" template/.agents/agents/coordinator.md` muestra la mención en «Qué lee» y la frase «sin excepción, aunque declares `omitClaudeMd`»
    - [x] `git diff -- template/.agents/agents/coordinator.md` no toca «Qué no hace»
    - [x] Lint OK en `template/.agents/agents/coordinator.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F2.2 `planner.md`: solo la remisión en «Qué lee» (4 min).** RF-12
  - Texto: `03_plan.md` §2.3, viñeta `planner.md`: la frase común como oración propia en la línea 10 de «Qué lee», fuera de la lista de cualquier documento. Nada más cambia.
  - Hecho cuando:
    - [x] `grep -n "Guardrails" template/.agents/agents/planner.md` da una sola línea, la de «Qué lee»
    - [x] `git diff --stat -- template/.agents/agents/planner.md` muestra 1 línea cambiada (o el equivalente de una oración añadida) y `git diff -- template/.agents/agents/planner.md` no toca «Qué no hace» ni la línea de «Las consultas de datos son de solo lectura…»
    - [x] Lint OK en `template/.agents/agents/planner.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F2.3 `reviewer.md`: lista de «Qué lee» y línea 21 (6 min).** RF-12, RF-13
  - Texto: `03_plan.md` §2.3, viñeta `reviewer.md`: lista pasa a «solo "Comandos", "Stack y convenciones de código", "Git", "Reglas duras" y "Guardrails"» con la frase común; «sin copiar la redacción» pasa a «de "Git", "Reglas duras" ni "Guardrails"»; en «Qué no hace» la línea «No ejecutas escrituras de datos ni cambios de esquema.» pasa a «Cumples `AGENTS.md` › Guardrails.»
  - Hecho cuando:
    - [x] `grep -n "Guardrails" template/.agents/agents/reviewer.md` muestra la lista de «Qué lee», la frase común y la línea de «Qué no hace»
    - [x] `grep -n "No ejecutas escrituras" template/.agents/agents/reviewer.md` vacío
    - [x] Lint OK en `template/.agents/agents/reviewer.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F2.4 `implementer.md`: descripción, «Qué lee» y «Qué no hace» (6 min).** RF-12, RF-13
  - Texto: `03_plan.md` §2.3, viñeta `implementer.md`: línea 3 «Genera SQL pero nunca lo ejecuta.» → «Cumple `AGENTS.md` › Guardrails.»; línea 9, lista a «solo "Comandos", "Stack y convenciones de código" y "Guardrails"» con la frase terminada en «… aunque no te inyecten el archivo.»; línea 13 «No ejecutas SQL.» → «Cumples `AGENTS.md` › Guardrails.»
  - Hecho cuando:
    - [x] `grep -n "nunca lo ejecuta\|No ejecutas SQL" template/.agents/agents/implementer.md` vacío
    - [x] `grep -n "Guardrails" template/.agents/agents/implementer.md` da 3 líneas (descripción, «Qué lee», «Qué no hace»); la descripción sigue siendo una sola línea de frontmatter válida (`name: implementer` intacto)
    - [x] Lint OK en `template/.agents/agents/implementer.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F3 — Skills y comandos (estimado: 25 min)
- [x] **F3.1 `migracion-bd/SKILL.md`: líneas 3, 8, 13, 58 y 63 (10 min).** RF-13
  - Texto: `03_plan.md` §2.4, viñeta `migracion-bd/SKILL.md`, cinco ediciones por separado (un Edit por frase). Se conservan :14, :27 y :59, y los otros dos anti-patrones (SQL suelto fuera de `sql/`; editar un script ya ejecutado).
  - Hecho cuando:
    - [x] `grep -n "nunca lo ejecutan\|nunca lo ejecuta\|y no se ejecuta\|no los ejecuta\|Ejecutar SQL de escritura desde un agente" template/.agents/skills/migracion-bd/SKILL.md` vacío
    - [x] `grep -n "AGENTS.md\` › Guardrails" template/.agents/skills/migracion-bd/SKILL.md` da 3 líneas (:8, :13, :63); la descripción de :3 ya no contiene la frase retirada y conserva los disparadores
    - [x] `grep -n "EJECUCIÓN MANUAL\|Lo ejecuta el usuario\|pendiente de ejecutar" template/.agents/skills/migracion-bd/SKILL.md` sigue mostrando :14, :27 y :58 (sin cambios de texto en las dos primeras); los anti-patrones «SQL suelto» y «editar un script ya ejecutado» siguen presentes
    - [x] Lint OK en `template/.agents/skills/migracion-bd/SKILL.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.2 `sdd/SKILL.md`: líneas 127 y 136 (4 min).** RF-13
  - Texto: `03_plan.md` §2.4, viñeta `sdd/SKILL.md`.
  - Hecho cuando:
    - [x] `grep -n "solo ejecutan consultas\|SQL de escritura ejecutado" template/.agents/skills/sdd/SKILL.md` vacío
    - [x] `grep -n "Guardrails" template/.agents/skills/sdd/SKILL.md` muestra la línea de Datos (con el enlace a `.agents/skills/migracion-bd/SKILL.md`) y el anti-patrón «Incumplir un guardrail»
    - [x] Lint OK en `template/.agents/skills/sdd/SKILL.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.3 `sdd-implement.md`: línea 12 (3 min).** RF-13
  - Texto: `03_plan.md` §2.4: «No ejecuta SQL.» → «Cumple `AGENTS.md` › Guardrails.» No se toca el tope de 3 secciones (RF-10).
  - Hecho cuando:
    - [x] `grep -n "No ejecuta SQL" template/.agents/commands/sdd-implement.md` vacío y `grep -n "Guardrails" template/.agents/commands/sdd-implement.md` da 1 línea en «Qué no hace»
    - [x] `grep -n "3 secciones\|tope" template/.agents/commands/sdd-implement.md` muestra el texto del tope sin cambios (`git diff` lo confirma)
    - [x] Lint OK en `template/.agents/commands/sdd-implement.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.4 `sdd-constitucion.md`: principio 5 (4 min).** RF-13
  - Texto: `03_plan.md` §2.4: «5. Datos y seguridad (solo lectura para agentes, `sql/` con rollback, sesión/permisos, sin secretos).» → «5. Datos y seguridad: `AGENTS.md` › Guardrails.»
  - Hecho cuando:
    - [x] `grep -n "5\. Datos y seguridad" template/.agents/commands/sdd-constitucion.md` da «5. Datos y seguridad: `AGENTS.md` › Guardrails.»
    - [x] `grep -n "solo lectura para agentes" template/.agents/commands/sdd-constitucion.md` vacío
    - [x] Lint OK en `template/.agents/commands/sdd-constitucion.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.5 `sdd-init.md`: línea 38 (4 min).** RF-13
  - Texto: `03_plan.md` §2.4: «cómo se consulta la base de datos en solo lectura (o si no aplica)» → «cómo se consulta la base de datos (ver Guardrails) (o si no aplica)», conservando el ancho del párrafo. Es el archivo con más riesgo de choque con 0009 y 0010: releer justo antes.
  - Hecho cuando:
    - [x] `grep -n "en solo lectura" template/.agents/commands/sdd-init.md` vacío y `grep -n "ver Guardrails" template/.agents/commands/sdd-init.md` da 1 línea
    - [x] `git diff -- template/.agents/commands/sdd-init.md` solo muestra el cambio de 0008 en esta línea (más lo ya hecho por 0009/0010, no tocado)
    - [x] Lint OK en `template/.agents/commands/sdd-init.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F4 — `restricciones-de-rol.md` (estimado: 15 min)
- [x] **F4.1 Filas 4 y 8, fila nueva y contador (10 min).** RF-14
  - Texto: `03_plan.md` §2.5: fila 4 (quitar «y no ejecuta SQL»); fila 8 (`planner` consulta datos con el comando de `AGENTS.md` › Comandos, según `AGENTS.md` › Guardrails, regla 1; mecanismo y garantía sin cambio); fila 9 nueva al final de la tabla «Ningún agente escribe en la base de datos…» | instrucción | NO garantizada; instrucción…; «Ocho filas» → «Nueve filas».
  - Hecho cuando:
    - [x] `grep -n "Nueve filas" template/.agents/skills/sdd/restricciones-de-rol.md` da 1 línea y `grep -n "Ocho filas" template/.agents/skills/sdd/restricciones-de-rol.md` vacío
    - [x] Las filas de datos de la tabla son 9 (contarlas: `grep -c "^| [^-]" ` sobre el bloque de la tabla menos el encabezado, comprobado leyendo la tabla) y `grep -n "Ningún agente escribe en la base de datos" template/.agents/skills/sdd/restricciones-de-rol.md` muestra la fila con «instrucción» y «NO garantizada»
    - [x] `grep -n "no ejecuta SQL" template/.agents/skills/sdd/restricciones-de-rol.md` vacío; la fila de `planner` no gana permisos de escritura (leída)
    - [x] Lint OK en `template/.agents/skills/sdd/restricciones-de-rol.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.2 Sección «Recomendación: usuario de BD de solo lectura» (5 min).** RF-15
  - Texto: `03_plan.md` §2.5, última viñeta: sección nueva tras la tabla y antes de «Fragmento opcional `permissions.deny`».
  - Hecho cuando:
    - [x] `grep -n "^## Recomendación: usuario de BD de solo lectura" template/.agents/skills/sdd/restricciones-de-rol.md` da 1 línea, anterior a la línea de `grep -n "Fragmento opcional" template/.agents/skills/sdd/restricciones-de-rol.md`
    - [x] El texto dice «Sugerencia, no garantía del kit»
    - [x] Lint OK en `template/.agents/skills/sdd/restricciones-de-rol.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F5 — Raíz del clon (estimado: 15 min)
- [x] **F5.1 `AGENTS.md` de la raíz: sección, nota y línea de Comandos (8 min).** RF-19
  - Texto: el mismo de F1.1 y F1.2 (`03_plan.md` §2.1 y primera viñeta de §2.2) y, en Comandos, la línea de la raíz de §2.2: «- Consulta de datos de solo lectura: No aplica.» → «- Consulta de datos (ver «Guardrails», regla 1): No aplica.» Edit mínimo con `Read` previo; lo propio del clon no se toca.
  - Hecho cuando:
    - [x] `diff <(sed -n '/^## Guardrails (todos los agentes, siempre)/,/^## Cada sesión/p' template/AGENTS.md) <(sed -n '/^## Guardrails (todos los agentes, siempre)/,/^## Cada sesión/p' AGENTS.md)` vacío (misma sección)
    - [x] `grep -n "todos los roles leen siempre «Guardrails»" AGENTS.md` da 1 línea; `grep -n "Consulta de datos (ver «Guardrails», regla 1): No aplica" AGENTS.md` da 1 línea; `grep -n "Consulta de datos de solo lectura" AGENTS.md` vacío
    - [x] Lint OK en `AGENTS.md` (sin la regla de `<<…>>`, que aplica solo a `template/`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F5.2 Copiar los 10 archivos de `.agents/` de `template/` a la raíz (7 min).** RF-19
  - Texto: copia byte a byte (`cp`) de los archivos de F2, F3 y F4: `agents/{coordinator,implementer,planner,reviewer}.md`, `skills/migracion-bd/SKILL.md`, `skills/sdd/SKILL.md`, `skills/sdd/restricciones-de-rol.md`, `commands/{sdd-implement,sdd-constitucion,sdd-init}.md`. Antes de copiar, `diff` de cada uno contra la raíz para confirmar que la raíz seguía igual a la versión previa de `template/` (si difería por un cambio local ajeno, parar y marcar `[!]`).
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents` muestra solo `napkin.md` y `skills/caveman` (E19); cualquier otra línea es réplica rota
    - [x] `grep -rn "Guardrails" .agents/agents/ | wc -l` coincide con `grep -rn "Guardrails" template/.agents/agents/ | wc -l`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F6 — Verificación final (estimado: 25 min)
- [x] **F6.1 Lista fija de RF-13 leída línea por línea, en `template/` y en la raíz (8 min).** RF-13
  - Hecho cuando:
    - [x] Leídas y sin redacción propia de la prohibición (solo remisión o enlace): `migracion-bd/SKILL.md` :3, :8, :13, :58, :63; `sdd/SKILL.md` :127, :136; `implementer.md` :3, :13; `reviewer.md` :21; `sdd-implement.md` :12; `sdd-constitucion.md` :25; `sdd-init.md` :38; filas de `restricciones-de-rol.md` (cada frase por texto, no por grep de sentencias), en `template/` y en la raíz
    - [x] Siguen intactas por exclusión: `migracion-bd` :14, :27, :59 y las reglas de redacción del SQL; `planner.md` línea de consultas; descripciones de `planner`, `reviewer` y `sdd-validate`
    - [x] La regla completa aparece solo en `AGENTS.md` › Guardrails: `grep -rn "Lista cerrada\|lista cerrada" template/ AGENTS.md .agents/ --include=*.md` solo cae en `AGENTS.md`/`template/AGENTS.md` (y en textos previos ajenos a la prohibición, que se anotan)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.2 Búsqueda de RF-9 en `template/` (4 min).** RF-9
  - Hecho cuando:
    - [x] `grep -riwn "mysql\|mariadb\|postgres\|postgresql\|oracle\|sqlserver\|mssql\|sql server\|db2\|cassandra\|dynamodb\|firebird\|sqlite\|mongodb\|redis\|hibernate\|jpa\|laravel\|django\|spring\|eloquent\|sequelize\|typeorm\|prisma\|sqlalchemy\|rails\|symfony\|dapper\|mybatis\|entity framework\|flask" template/` da cero coincidencias (`-w` evita «guardrails»)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.3 Agentes, contador y réplicas (6 min).** RF-12, RF-14, RF-19
  - Hecho cuando:
    - [x] `grep -l "Guardrails" template/.agents/agents/*.md | wc -l` da 4 y lo mismo en `.agents/agents/*.md`
    - [x] `restricciones-de-rol.md` dice «Nueve filas» y la tabla tiene nueve filas de datos, en `template/` y en la raíz
    - [x] `diff -rq template/.agents .agents` solo `napkin.md` y `skills/caveman`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.4 Codificación y lint de todos los archivos tocados (4 min).** RF-9, RF-19
  - Hecho cuando:
    - [x] `file -i` da utf-8 en `template/AGENTS.md`, `AGENTS.md` y los 10 archivos de `template/.agents/` tocados y de su réplica
    - [x] `grep -l $'^\xEF\xBB\xBF' template/AGENTS.md AGENTS.md` y el mismo comando sobre los 10 archivos de `template/.agents/` y de `.agents/` dan vacío
    - [x] `name` del frontmatter = nombre de su carpeta en `migracion-bd/SKILL.md` y `sdd/SKILL.md`; en `template/` sin `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>` (`grep -rn "<<" template/AGENTS.md template/.agents/` revisado)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.5 Alcance no excedido (3 min).** RF-16
  - Hecho cuando:
    - [x] `git status --short` no muestra cambios de 0008 en `manifiestos/`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md` ni en la constitución del kit (`git diff --stat` revisado; lo de 0009/0010 no cuenta)
    - [x] Los archivos tocados por 0008 son solo los 11 de `template/`, `AGENTS.md` de la raíz y, en F7, el ADR 0019 y su índice
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F7 — Changelog en `05_avance.md` y ADR 0019 (estimado: 15 min)
- [x] **F7.1 Dejar las líneas de changelog de 0008 en `05_avance.md` (5 min).** RF-17
  - Texto: las dos viñetas de `03_plan.md` §5 («AGENTS.md incorpora la sección «Guardrails…» y «AGENTS.md es grupo B…»), copiadas tal cual bajo un encabezado en `05_avance.md` (sección Decisiones o una nota propia) para que quien cierre 0009 las pegue. Quien escribe en `05_avance.md` es el coordinator: el implementer entrega el texto de las dos viñetas en su reporte y no edita ese archivo. No se crea manifiesto, no se cambia `VersionAgentKitSdd` y no se crea la entrada 0.6.0 de `ChangelogAgentKitSdd.md`.
  - Hecho cuando:
    - [x] `grep -n "incorpora la sección «Guardrails (todos los agentes, siempre)»" .docs/002_specs/0008_GuardrailsBdSoloLectura/05_avance.md` y `grep -n "es grupo B" .docs/002_specs/0008_GuardrailsBdSoloLectura/05_avance.md` dan una línea cada uno
    - [x] `git status --short manifiestos VersionAgentKitSdd ChangelogAgentKitSdd.md` vacío (en lo que toca a 0008)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.2 Crear el ADR 0019 y su fila de índice (10 min).** RF-18
  - Texto: incondicional. Antes de crear, `ls .docs/004_adr | grep -E "^00(19|2)"`: si 0009 o 0010 ya tomaron el 0019, usar el siguiente número libre por valor numérico. Archivo `.docs/004_adr/0019_ReglaGlobalBdSoloLectura.md` con la forma de `0018_*.md` (Estado Aceptado; Fecha; Origen `0008_GuardrailsBdSoloLectura`; Contexto; Decisión; Alternativas descartadas en tabla; Consecuencias), con el contenido de `03_plan.md` §6.1: título «Regla global de la base de datos solo lectura en AGENTS.md › Guardrails»; alternativas (a) a (e); consecuencias. Fila nueva al final de la tabla de `.docs/004_adr/README.md`. Sin tecnologías de proyecto.
  - Hecho cuando:
    - [x] `ls .docs/004_adr/0019_*.md` lista el archivo y `grep -n "^| 0019 " .docs/004_adr/README.md` da 1 fila con enlace válido a ese archivo
    - [x] El ADR contiene las cinco alternativas (a a e) y las consecuencias de §6.1
    - [x] Lint OK en el ADR y en `.docs/004_adr/README.md` (`file -i` utf-8, sin BOM)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## Resumen de estimados
| Fase | Pasos | Min |
|---|---|---|
| F0 | 2 | 10 |
| F1 | 3 | 25 |
| F2 | 4 | 20 |
| F3 | 5 | 25 |
| F4 | 2 | 15 |
| F5 | 2 | 15 |
| F6 | 5 | 25 |
| F7 | 2 | 15 |
| Total | 25 | 150 |
