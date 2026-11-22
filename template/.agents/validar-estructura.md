# Validación de la estructura del kit (checklist que corre el agente)

Lo usa el comando `/sdd-estructura`. Solo **lee y reporta**: no corrige nada sin confirmación.
Sirve para comprobar que el kit mantiene su estructura: una sola copia canónica en `.agents/`,
réplicas por herramienta como enlaces, y skills, agentes, comandos e índices coherentes.

## Alcance

La sesión principal corre E1 a E23. No se delegan a un subagente.

`/sdd-estructura` sin argumento corre todos los bloques. Con argumento corre uno:
`enlaces`, `skills`, `agentes`, `comandos`, `docs`, `codificacion` o `kit` (solo en el repo del kit).

## Chequeos

| # | Bloque | Qué se comprueba | Cómo |
|---|---|---|---|
| E1 | enlaces | Se mide sobre las herramientas declaradas en `.agents/local/kit-visto`; sin «visto» legible (ausente o corrupto) se miden todas las filas y ninguna herramienta está declarada: `FALTA` es `AVISO` con el texto exacto «enlace FALTA de herramienta no declarada: correr `/sdd-enlaces`», y `DIFERENTE` u `OCUPADO` sin `excepcion:` son `FALLA`. Con «visto» legible: cada fila de `.agents/sync-agents.md` está `OK`; ningún `OCUPADO` ni `DIFERENTE`. Filas de herramientas no declaradas, `excepcion:` y `copia:` son `AVISO`, no `FALLA` | Estados de `sync-agents.md` |
| E2 | enlaces | **Réplicas huérfanas:** skill, agente o comando que existe en una carpeta de herramienta (`.claude/`, `.cursor/`, `.gemini/`, `.github/`, `.opencode/`) y no en `.agents/` | `sync-agents.md` paso 8 |
| E3 | enlaces | **Réplicas faltantes:** entrada de `.agents/` que una carpeta de herramienta real (no enlace) no tiene. En toda carpeta de agentes por archivo (`sync-agents.md` › «Carpeta de agentes por archivo»; la excepción de `.opencode/agents/` también remite ahí) una entrada de `.agents/agents/` es válida si hay un enlace por rol o un adaptador que cumple esa definición; solo falta si no hay ninguno de los dos | `sync-agents.md` paso 8 |
| E4 | skills | Cada carpeta de `.agents/skills/` tiene `SKILL.md`; sin archivos sueltos en `skills/` | `ls` |
| E5 | skills | `name` del frontmatter = nombre de la carpeta (minúsculas, guiones, sin acentos) | Leer la línea `name:` |
| E6 | skills | `description` presente, con qué hace **y** cuándo activarla; máx. 1024 caracteres | Contar la descripción |
| E7 | skills | Cada skill está en la tabla de "Skills de proyecto" de `AGENTS.md` (las de dominio y las del kit); ninguna fila apunta a una skill inexistente. Exención solo si existe `template/` (repo del kit): una skill presente en `.agents/skills/` y ausente de `template/.agents/skills/` no exige fila ni es FALLA; una skill local con el nombre de una del kit cuenta como del kit. En un proyecto instalado (sin `template/`) no aplica y toda skill exige fila. Una fila a una skill inexistente sigue siendo FALLA | Cruzar tabla y carpetas |
| E8 | agentes | Cada `agents/<rol>.md` tiene `name` (= archivo), `description` y `tools`; los roles coinciden con «Roles y comandos» de `.agents/README.md` | Leer frontmatter |
| E9 | comandos | Cada `commands/<n>.md` tiene `description` y `argument-hint` (si recibe argumentos); los comandos citados en `.agents/README.md` › «Roles y comandos» existen y los que existen están citados ahí (que `AGENTS.md` los cite ya no cuenta: uno viejo con su sección no es `FALLA`) | Cruzar lista y carpeta |
| E10 | comandos | Los comandos del flujo `sdd` están en la tabla de fases de `skills/sdd/SKILL.md` y en el ruteo de `coordinator.md` | Cruzar tablas |
| E11 | rutas | Toda ruta citada con comillas inversas en `AGENTS.md`, `.agents/**/*.md` y `.docs/**/README.md` existe. No aplica a los manifiestos de versión: pueden listar rutas retiradas | `grep` de rutas + `ls` |
| E12 | docs | Sin prefijos repetidos **por valor numérico** en los ítems de `.docs/002_specs/`, `.docs/003_deuda/` y `.docs/004_adr/`. Remite a `.docs/002_specs/README.md` › « Numeración única ». El prefijo numérico vale su entero: los ceros de relleno y el ancho escrito no son `FALLA` y el nombre no se reescribe. Dos ítems con el mismo entero son `FALLA` y esa igualdad no es `AVISO`. Un nombre que no empieza por dígitos y `_` no entra en el consecutivo ni en el ancho. En el clon, los 4 dígitos valen para el ítem nuevo: el ancho ya escrito no pasa a `FALLA` y una mezcla válida no es `AVISO`. El `AVISO` de mezcla válida sigue siendo solo de proyecto. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014. Las carpetas de sección (`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`) se quedan en 3 | Listar y contar prefijos |
| E13 | docs | Cada índice (`002_specs`, `003_deuda`, `004_adr`) coincide con sus carpetas o archivos; si discrepan, manda la carpeta | Cruzar fila y archivo |
| E14 | docs | Toda carpeta de `.docs/002_specs/` tiene `01_spec.md` | `ls` |
| E15 | codificación | Archivos `.md` en UTF-8 sin BOM | `file -i`; `grep -l $'^\xEF\xBB\xBF'` vacío |
| E16 | sesión | Existen `AGENTS.md`, `CLAUDE.md` (solo `@AGENTS.md`), `MEMORY.md`, `.agents/napkin.md` y `.docs/001_constitucion/constitution.md` | `ls` y leer `CLAUDE.md` |
| E17 | sesión | Sin copias del cuerpo de una skill en `AGENTS.md` ni `CLAUDE.md`; napkin solo en `.agents/napkin.md` | Revisar anti-patrones y «Qué va en `AGENTS.md`» de `.agents/README.md` |
| E18 | kit | Solo en el repo del kit: los comandos de verificación de `AGENTS.md` › Comandos (sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`; ensayo de instalación en `mktemp -d`) | `AGENTS.md` › Comandos |
| E19 | kit | Solo en el repo del kit: `diff -rq template/.agents .agents` solo debe mostrar `napkin.md` (se ignora `.agents/local/`). Otra diferencia es una réplica desfasada: proponer el refresco de `INSTALAR.md` › Modo mantenimiento › "Refrescar la copia local" (sobrescribe desde `template/.agents/` y restaura `napkin.md`; no borra lo que solo está en `.agents/`). No ejecutarlo sin confirmación | `diff -rq` |
| E20 | kit | Solo en el repo del kit: recalcula la huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` y la compara con manifiestos/<VERSION>.md. Falla por diferencia, por archivo sin fila o por fila sin archivo. En un proyecto instalado no aplica: ahí no hay manifiestos | Algoritmo de `ACTUALIZAR.md` |
| E21 | sesión | **Marcador de instalación:** solo con .agents/.kit-version. Existe `AGENTS.md` y el literal `SDD-INIT-PENDIENTE` solo aparece dentro del marcador válido (cero veces en un proyecto personalizado, una en uno pendiente) | «Detalle de E21» |
| E22 | docs | **Estatus de la fila del índice de specs y estado en el índice de deuda.** Solo `AVISO`: no es `FALLA`, no frena y no escribe. Cita `.docs/002_specs/README.md` › «Leyenda de estatus» y `.docs/003_deuda/README.md` › «Estados», y no las pega. Avisa si, según esa leyenda, la palabra está fuera de la lista, la celda está mal formada, faltan fecha o detalle, hay un `Cerrado`, `Descartado` o `Diferido` sin su forma, el siguiente paso no coincide, o la fila del índice está ausente, duplicada o desfasada de la etapa. Avisa también si, según esa leyenda, la columna de pendiente de la fila del índice de un `Cerrado` o un `Implementado` trae texto libre: en esas filas solo valen enlaces a deudas numeradas, o vacía. Avisa si, según «Estados», en el índice de deuda el estado no es `Abierta`, `En atención`, `Resuelta` ni `Descartada`, o una `Resuelta` no tiene «Cómo se atendió». La fila del índice no es la fila `cierre` ni la fila de modo. Si esa leyenda no trae `Cerrado` y `Descartado`, el aviso lo dice y no reescribe el índice. El cruce de la fila del índice con la carpeta o el archivo sigue en E13 | Leer las filas del índice de specs y aplicar la leyenda citada; leer el estado del índice de deuda y aplicar «Estados»; anotar `AVISO`; no escribir |
| E23 | agentes | **Modelo por rol.** Solo `AVISO`: no es `FALLA`, no frena y no escribe. Compara el `model:` del frontmatter de cada agente de `.agents/agents/` con la tabla «Modelo por rol» del proyecto (`.agents/skills/sdd/restricciones-de-rol.md` › «Modelo por rol»), que se cita y no se pega. Avisa si un agente no declara `model:` o si su valor difiere de la fila de su rol. Un agente personalizado con un modelo distinto avisa y no falla: la persona decide si lo deja. Un rol sin fila en la tabla no se compara. Además compara el `model:` de cada adaptador de una carpeta de agentes por archivo con `.agents/local/configuracion.md` › `## Modelos` (si no existe, con los sugeridos de `.agents/configuracion.ejemplo.md`); también solo `AVISO`. Un adaptador cuyo frontmatter difiere del canónico fuera de lo que permite la definición (`sync-agents.md` › «Carpeta de agentes por archivo») da `AVISO` de adaptador viejo y sugiere `/sdd-enlaces` | Leer el `model:` de cada agente y la tabla citada; anotar `AVISO` por cada diferencia; no escribir |

## Detalle de E21

Aplica solo si existe .agents/.kit-version. Si no existe (p. ej. la raíz del clon del kit), E21 no
aplica: ni `OK` ni `FALLA`. Lee `AGENTS.md` (un enlace simbólico se lee como su destino). Pasos, en
este orden; el primer `FALLA` termina E21:

1. `AGENTS.md` no existe: `FALLA`.
2. «La cadena» es el párrafo «Cadena del marcador» de este archivo (`validar-estructura.md`). Si no
   es un comentario HTML de una línea que contiene el literal una sola vez: `FALLA` (cadena ausente
   o alterada). Sin template/ este paso es lo único que se puede comprobar: la cadena se detecta
   solo por forma, no por contenido (diferencia ya declarada en la spec).
3. Si existe template/AGENTS.md: su primera línea no vacía tras el título, comparada como en el
   punto 5, debe ser igual a la cadena. Si no: `FALLA` (desincronizada). Sin template/, se omite.
4. Cuenta de ocurrencias del literal: `grep -o 'SDD-INIT-PENDIENTE' AGENTS.md | wc -l`. Si da 0: no
   hay marcador (proyecto personalizado); `OK`. Un archivo vacío cae aquí.
5. Si da 1 o más: extrae la primera línea no vacía tras el título con
   `sed 's/\r$//' AGENTS.md | awk 'f&&NF{print;exit} /^# /{f=1}' | sed 's/[[:space:]]*$//'`.
   Es el marcador válido si la cuenta es 1 y esa línea es igual a la cadena (solo se ignoran el fin
   de línea CRLF o LF y los espacios o tabuladores finales). Entonces `OK` (proyecto pendiente). En
   cualquier otro caso (cuenta mayor que 1, otra posición, comentario repetido, variante, BOM, `\r`
   suelto, multilínea): `FALLA`.

Cadena del marcador (copia literal del comentario de template/AGENTS.md; quien cambie uno cambia
el otro en el mismo cambio). Es la única copia fuera de la plantilla:

`<!-- SDD-INIT-PENDIENTE: este marcador lo quita /sdd-init al terminar la personalización. -->`

Mensajes de `FALLA` (columnas de «Cómo responder»; severidad ALTA):

| Caso | Hecho | Corrección propuesta |
|---|---|---|
| Sin `AGENTS.md` | `AGENTS.md` no existe | Restaurarlo desde Git o desde template/AGENTS.md; no se crea sin confirmación |
| Cadena (paso 2) | Cadena del marcador ausente o alterada en `validar-estructura.md` | Restaurarla desde el comentario de template/AGENTS.md |
| Desincronía (paso 3) | La cadena difiere del comentario de template/AGENTS.md | Igualar la cadena y el comentario en el mismo cambio |
| Literal (paso 5) | `AGENTS.md:N` contiene el literal fuera del marcador válido (cuenta C) | Reformular la frase sin el literal; si el proyecto ya está personalizado, incorporar el paso 1 nuevo (Changelog 0.6.0, grupo B) |

## Cómo responder

1. Corre los bloques pedidos y, de cada chequeo, anota `OK`, `AVISO` o `FALLA` con `archivo:línea`.
2. Reporta una tabla corta con solo lo que falla, severidad primero. Tras la tabla, un bloque
   **Avisos** (carpeta y hecho, una línea cada uno) con los `AVISO`; no es una corrección:

```
ALTA   | E2  | .claude/skills/probando/ | Skill fuera de .agents/ | Mover a .agents/skills/probando/ y enlazar
MEDIA  | E7  | .agents/skills/adr       | Falta fila en AGENTS.md | Agregar la ruta a la tabla
BAJA   | E15 | ChangelogAgentKitSdd.md  | BOM al inicio           | Quitar el BOM
```

3. Sin fallos: `estructura OK (E1–E23, N archivos revisados)`; si hubo avisos, se añade
   `, M avisos` y se listan en **Avisos**. No listes los OK uno a uno.
4. Cierra con **Correcciones propuestas**: lista numerada, sin aplicarlas. Las correcciones de
   enlaces y réplicas se aplican con `/sdd-enlaces`; las demás, solo si el usuario las pide como
   acción aparte y las confirma. Nunca borres ni muevas un archivo (estado `OCUPADO` o
   huérfano) sin esa confirmación.
5. Una skill, agente o comando **en prueba** que el dev quiere dejar local no es un fallo: se
   anota como `EN PRUEBA` y se propone excluirlo en `.git/info/exclude` si no debe versionarse.
