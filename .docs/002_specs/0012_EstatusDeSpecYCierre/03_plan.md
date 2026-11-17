# 03 — Plan: Estatus de spec y criterio de cerrado
Spec: 01_spec.md (Estado: aprobada; el archivo no trae hora de aprobación)

Cambio al kit: solo Markdown, sin scripts y sin base de datos. Las cuatro candidatas a ADR y el número de versión quedan recomendadas aquí; ninguna se aplica hasta que la persona la apruebe. Los trece supuestos de `01_spec.md` no se reabren. La deuda de que cada comando de fase actualice la celda de `Planificado`, `En curso` y `En pruebas` no se crea en este plan: se propone y la persona la da de alta.

## 1. Ubicación y capas
No hay aplicación, pantallas ni módulos de código. El conocimiento que se distribuye se edita en `template/`. Los docs de la raíz (`ACTUALIZAR.md`, `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd`) se editan en la raíz. `AGENTS.md` de la raíz es conocimiento versionado del clon; `template/AGENTS.md` es el que se instala (grupo B).

`.agents/` de la raíz es la réplica local de `template/.agents`, fuera de Git (`.git/info/exclude`). No se edita a mano como si fuera otra fuente: se replica con `cp` desde `template/.agents/` y se comprueba con `diff -rq template/.agents .agents`. `MEMORY.md` (2026-11-03) admite en esa diferencia solo `napkin.md` y `skills/caveman`. E19 (`template/.agents/validar-estructura.md`) exige que el diff solo muestre `napkin.md`, ignora `.agents/local/` y trata cualquier otra diferencia como réplica desfasada. F0 anota el diff real antes de editar; la réplica de este cambio no puede añadir rutas a esa línea base.

Constitución, principio 3: cada regla vive en una sola fuente. Principio 6: cada versión actualiza `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`. `AGENTS.md` › Comandos: build no aplica; la consulta de datos no aplica; el ensayo de instalación copia `template/` sin pisar.

## 2. Archivos que se crean o modifican
La tabla sale de la sección 2.3 del análisis. Cada réplica local se escribe en la fase de réplica, no como redacción paralela. Los ADR no están en esa tabla: se crean en el clon solo si la persona aprueba la candidata, con el siguiente libre `0024` (el índice llega a `0023`) y ancho de 4 dígitos. `template/.docs/004_adr/README.md` no se toca: el template solo lleva el índice vacío.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.docs/002_specs/README.md` | Distribuible | Fuente única de la leyenda: las ocho palabras, la forma de la celda, pendiente, pendiente de desarrollo, grupo, desfase y quién escribe cada transición. El bullet «Estatus y fechas salen del PUNTO DE RETOMO» deja de ser la fuente del pendiente. «Numeración única» no se reescribe. | RF-1, RF-2, RF-7, RF-10 |
| `.docs/002_specs/README.md` | Raíz versionada | La misma leyenda y el mismo cambio de bullet. Las filas ya escritas no se reclasifican. | RF-1, RF-2, RF-7, RF-10, RF-11 |
| `template/.docs/003_deuda/README.md` | Distribuible | Bajo «Estados», deuda cerrada y deuda pendiente, y el bloqueo de `Cerrado` por Atendida por. Los cuatro nombres se conservan. El índice vacío sigue vacío. | RF-8 |
| `.docs/003_deuda/README.md` | Raíz versionada | La misma definición. Las filas 0009, 0010 y 0011 no cambian de estado. | RF-8 |
| `template/.agents/commands/sdd-spec.md` | Distribuible | Sigue creando la fila en `Análisis` y el documento en `borrador`. La forma de la celda remite a la leyenda. | RF-1 |
| `.agents/commands/sdd-spec.md` | Réplica local | Copia de la fuente. No se redacta aparte. | RF-1 |
| `template/.agents/commands/sdd-cierre.md` | Distribuible | Deja de fijar siempre `implementada`. Nombra `Implementado`, `Cerrado` o `Descartado` según la previa, los cinco criterios y el descarte. La previa se alinea con la spec. Las transiciones de deuda se juzgan y las escribe este cierre, con la decisión de la persona. | RF-1, RF-4, RF-5, RF-8, RF-12 |
| `.agents/commands/sdd-cierre.md` | Réplica local | Copia de la fuente. | RF-1, RF-4, RF-5, RF-8, RF-12 |
| `template/.agents/commands/sdd-status.md` | Distribuible | Informa la etapa de esa carpeta, avisa el desfase con la celda y no modifica archivos. | RF-3, RF-7 |
| `.agents/commands/sdd-status.md` | Réplica local | Copia de la fuente. | RF-3, RF-7 |
| `template/.agents/commands/sdd-estructura.md` | Distribuible | El rango pasa de E1–E21 a E1–E22. No escribe y no redefine el resto de chequeos. | RF-9 |
| `.agents/commands/sdd-estructura.md` | Réplica local | Copia de la fuente. | RF-9 |
| `template/.agents/validar-estructura.md` | Distribuible | Chequeo nuevo E22, después de E21, solo `AVISO`. E12, E13 y E21 no cambian de hecho. El rango de las líneas de alcance y del mensaje `estructura OK` pasa a E22. | RF-9 |
| `.agents/validar-estructura.md` | Réplica local | Copia de la fuente. | RF-9 |
| `template/.agents/agents/reviewer.md` | Distribuible | La cita «E1–E21» pasa a «E1–E22». | RF-9 |
| `.agents/agents/reviewer.md` | Réplica local | Copia de la fuente. | RF-9 |
| `template/.agents/skills/sdd/SKILL.md` | Distribuible | La lista de `Estado` del documento suma `cerrada` y `descartada`. El significado de la celda no se copia. | RF-1, RNF-2, RNF-4 |
| `.agents/skills/sdd/SKILL.md` | Réplica local | Copia de la fuente. | RF-1, RNF-2, RNF-4 |
| `template/.agents/skills/sdd/plantillas.md` | Distribuible | El campo `Estado` de la plantilla de `01_spec.md` suma `cerrada` y `descartada`. El resto no copia la tabla. | RF-1, RNF-2 |
| `.agents/skills/sdd/plantillas.md` | Réplica local | Copia de la fuente. | RF-1, RNF-2 |
| `template/AGENTS.md` | Distribuible, grupo B | Los puntos 5 y 6 de Antecedentes remiten a la leyenda. Un proyecto ya instalado lo incorpora a mano. | RF-6, RF-7 |
| `AGENTS.md` | Raíz versionada, grupo B | Las mismas dos frases. | RF-6, RF-7 |
| `ACTUALIZAR.md` | Raíz versionada | Nota de reclasificación: guía a la persona y no reescribe el índice. `.docs/` sigue en grupo C. | RF-11 |
| `VersionAgentKitSdd` | Raíz versionada | De `0.7.0` a la versión que la persona apruebe. | Criterio de finalización |
| `ChangelogAgentKitSdd.md` | Raíz versionada | Entrada de esa versión, con el comportamiento nuevo y la misma nota de reclasificación. | Criterio de finalización |
| `manifiestos/<versión>.md` | Manifiesto, no se distribuye | Se crea al congelar `template/`. Huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` › Huella. Los manifiestos ya publicados no se reescriben. | E20, criterio de finalización |
| `.docs/004_adr/0024_LeyendaDeEstatusEnElIndice.md` | ADR del clon | Decisión 1, solo si se aprueba. | Decisiones |
| `.docs/004_adr/0025_AvisoDeEstatusTrasE21.md` | ADR del clon | Decisión 2, solo si se aprueba. | Decisiones |
| `.docs/004_adr/0026_AntecedentesRemitenALaLeyenda.md` | ADR del clon | Decisión 3, solo si se aprueba. | Decisiones |
| `.docs/004_adr/0027_LeyendaCopiadaEnElIndiceDelClon.md` | ADR del clon | Decisión 4, solo si se aprueba. | Decisiones |
| `.docs/004_adr/README.md` | Raíz versionada | Una fila por ADR aprobado. Un rechazo no ocupa número. | Decisiones |

Quedan fuera, como ya dijo el análisis: `sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks`, `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-change`, `coordinator.md`, `topes-y-modos.md`, `avance-y-tiempos.md`, la skill `adr`, la constitución, `MEMORY.md`, E12, E13, E21 y los manifiestos `0.4.0.md` a `0.7.0.md`. No se crea un comando para `Diferido`.

## 3. Base de datos
No aplica. No hay tablas ni `sql/`. `AGENTS.md` › Comandos declara la consulta de datos como «No aplica».

| Script `sql/` | Tipo (DDL/seed) | Rollback |
|---|---|---|
| No aplica | — | — |

## 4. Seguridad
No hay autenticación, permisos de aplicación ni doble envío. Se cumple `AGENTS.md` › Guardrails: este plan no ejecuta sentencias ni activa nada que escriba en una base. No se añaden secretos ni credenciales. No se versionan scripts. La instalación sigue copiando sin sobrescribir; el reemplazo confirmado de KIT-VIEJO queda en el ADR 007 y este cambio no lo amplía. Los archivos nuevos son Markdown de procedimiento.

## 5. Liberación / piloto
No hay feature flag ni usuarios piloto. La liberación es una versión del kit.

Recomendación: `0.8.0`, porque hay comportamiento nuevo (estatus terminal, tres resultados del cierre y un chequeo nuevo). Alternativa: `0.7.1`. La persona elige el número antes de F8. Hasta entonces `VersionAgentKitSdd` sigue en `0.7.0`.

Al publicar:

- `VersionAgentKitSdd` queda en una sola línea con ese número.
- `ChangelogAgentKitSdd.md` abre una entrada `## <versión> — <fecha>` con el comportamiento nuevo, la nota de que `AGENTS.md` es grupo B y se incorpora a mano, y el mapa de filas ya escritas (el mismo de `ACTUALIZAR.md`).
- Se crea `manifiestos/<versión>.md` con el formato de `manifiestos/0.7.0.md`: ruta bajo `template/` sin el prefijo `template/`, y huella SHA-256 del algoritmo de `ACTUALIZAR.md` › Huella. E20 falla si falta una fila, sobra una fila o la huella no coincide. El conteo se toma al congelar, no del análisis: si `template/` cambió, manda la medición.
- El ensayo de instalación, en un temporal y nunca en un proyecto real, es el de `AGENTS.md` › Comandos: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/`. Se revisa que la copia traiga lo nuevo y no pise un archivo que ya estaba en el destino. No cubre `manifiestos/` ni el `.docs/` versionado del clon.

`.docs/` es grupo C: actualizar un proyecto no pisa su índice. Los comandos y `validar-estructura.md` son grupo A (clasificación KIT-VIEJO o PERSONALIZADO). `AGENTS.md` es grupo B: solo faltante, a mano.

## 6. Decisiones
La persona aprueba o rechaza cada fila. El ADR se escribe en F9 solo con el visto bueno, y registra la opción elegida. Si elige la alternativa, se replantea la fase afectada antes de editar.

| Decisión | Recomendación | Alternativa descartada | Por qué |
|---|---|---|---|
| 1. Dónde vive la definición | Una sección de `template/.docs/002_specs/README.md`. La de deuda, en `template/.docs/003_deuda/README.md`. Comandos, skill, plantilla y `AGENTS.md` remiten y no copian la tabla. ADR `0024` si se aprueba. | Una pieza nueva al estilo de `topes-y-modos.md`, o copiar la tabla en cada comando. | RF-2 responde desde el índice, sin abrir carpetas. Copiar la tabla choca con el principio 3. Una pieza nueva obligaría al índice a remitir y dejaría de ser la fuente que el agente lee. |
| 2. Aviso del chequeo | Chequeo nuevo E22, después de E21, solo `AVISO`: no es `FALLA` y no escribe. E13 no se edita. ADR `0025` si se aprueba. | Ampliar E13. | E13 cruza fila y archivo y dice que manda la carpeta. El aviso de la spec no frena. Meterlo en E13 mezcla ese cruce con la forma de la celda. La frase de E13 queda para el cruce fila/archivo; el desfase de la celda lo dice E22, que cita la leyenda. |
| 3. `AGENTS.md` (grupo B) | Los puntos 5 y 6 de Antecedentes remiten a la leyenda, en `template/AGENTS.md` y en el `AGENTS.md` del clon. ADR `0026` si se aprueba. | Dejar «manda la carpeta» y «lo hacen `/sdd-spec` y `/sdd-cierre`». | Esas frases contradicen el desfase y a la persona que escribe `Diferido`. El changelog avisa que un proyecto ya instalado las incorpora a mano. |
| 4. Leyenda de este clon | Copiar la leyenda en `.docs/002_specs/README.md` y la definición de deuda en `.docs/003_deuda/README.md`, en el mismo cambio que `template/`. ADR `0027` si se aprueba. | Remitir a `template/` como ya hace la numeración. | Antecedentes abre `.docs/002_specs/README.md`. Si ese índice solo remite, RF-2 no se responde sin abrir `template/`. En un proyecto instalado la definición completa tiene que estar en el README de `template/.docs/`, porque ahí no hay `template/` como fuente. El duplicado clon/plantilla es el que ya existe en las líneas 20-21; no se suma una tercera copia en los comandos. |
| 5. Versión (no es ADR) | `0.8.0`. | `0.7.1`. | Hay comportamiento nuevo, no un parche de 0.7.0. La spec pide versión y changelog y no fija el número. |

Desvío aprobado por la persona el 2026-11-16, tras la revisión independiente de los lotes de implementación: E22 cubre además el texto libre en la columna de pendiente de un `Cerrado` o `Implementado` y una deuda con estado fuera de los cuatro valores o `Resuelta` sin «Cómo se atendió», y `reviewer.md` pasa a E1–E22. No abre una fila nueva de esta tabla; F5, F7 y RF-9 ya lo incorporan.

Tercera corrección de la sesión (2026-11-16): los comandos, la leyenda y `AGENTS.md` remiten a la leyenda y no la copian; el bloqueo de deuda vive en `.docs/003_deuda/README.md`.

## 7. Fases y estimado
Horas de implementación siguiendo este plan. Cada fase depende de la anterior. F1 a F6 editan la fuente (`template/` o la raíz); la réplica espera a F7. F8 va después de que `template/` deje de cambiar. F9 espera el visto bueno de la sección 6.

| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | Línea base, antes de editar. Anotar el resultado de E19 (`diff -rq template/.agents .agents`), de E20 contra `manifiestos/0.7.0.md` y del ensayo de instalación (`cp -R --update=none` a un temporal). Si alguno falla por algo ajeno a este cambio, parar y reportar. No se edita nada. | — | 0.4 |
| F1 | Leyenda en los dos README de specs, si la persona aprueba las decisiones 1 y 4. Ocho palabras, forma de la celda, pendiente, pendiente de desarrollo, grupo, desfase, quién escribe cada transición, previa y tres resultados, y los cinco criterios citando el dato y el valor. El bullet del punto de retomo deja de ser la fuente del pendiente. No se reescribe «Numeración única». No se reclasifican las filas ya escritas. | RF-1, RF-2, RF-7, RF-10 | 1.2 |
| F2 | Definición bajo «Estados» en los dos README de deuda. Cerrada = `Resuelta` o `Descartada`. Pendiente = `Abierta`, `En atención` u otra palabra. El bloqueo de `Cerrado` cita la leyenda y Atendida por. Los cuatro nombres se conservan. Las filas 0009, 0010 y 0011 no cambian. Depende de F1. | RF-8 | 0.5 |
| F3 | Comandos de apertura, cierre y estado, solo en `template/.agents/commands/`: `sdd-spec.md`, `sdd-cierre.md`, `sdd-status.md`. Remiten a la leyenda. El cierre deja los tres resultados, el descarte sin la previa, la reapertura y las transiciones de deuda con la decisión de la persona. El estado informa la etapa, avisa el desfase, localiza deudas por Atendida por y no modifica archivos. Depende de F1 y F2. | RF-1, RF-3, RF-4, RF-5, RF-7, RF-8, RF-12 | 1.3 |
| F4 | Skill, plantilla y `AGENTS.md`. En `template/.agents/skills/sdd/SKILL.md` y `plantillas.md`, el `Estado` del documento suma `cerrada` y `descartada`, sin copiar la tabla. Si la persona aprueba la decisión 3, los puntos 5 y 6 de Antecedentes en `template/AGENTS.md` y en `AGENTS.md` remiten a la leyenda (desfase y `Diferido` lo escribe la persona). Depende de F1. | RF-1, RF-6, RF-7, RNF-2 | 0.6 |
| F5 | Chequeo de estructura, si la persona aprueba la decisión 2. En `template/.agents/validar-estructura.md`, E22 después de E21: palabra fuera de la lista, celda mal formada, fecha o detalle faltantes, `Cerrado`, `Descartado` o `Diferido` sin su forma, siguiente paso que no coincide, fila ausente, duplicada o desfasada de la etapa. Solo `AVISO`, no escribe. Cubre además (a) texto libre en la columna de pendiente de la fila del índice de un `Cerrado` o `Implementado` y (b) una deuda con estado fuera de los cuatro valores o `Resuelta` sin «Cómo se atendió». Cita la leyenda y no la pega. Si la leyenda del índice no trae `Cerrado` y `Descartado`, el aviso lo dice y no reescribe (grupo C). E12, E13 y E21 no cambian de hecho. El alcance, el mensaje `estructura OK (E1–E21…)`, `template/.agents/commands/sdd-estructura.md` y `template/.agents/agents/reviewer.md` pasan a E1–E22. Depende de F1. | RF-9 | 1.0 |
| F6 | Nota de reclasificación en `ACTUALIZAR.md`, después de `### Grupo C` y antes de `## Plan completo`. Mapa: `Plan` → `Planificado`; `Spec aprobada` → `Análisis` sin plan y `Planificado` con plan; `Implementado en local` → `Implementado`. Guía a la persona, no reescribe celdas y no es un pendiente que bloquee `.agents/.kit-version`. El grupo C sigue sin comparar ni reemplazar `.docs/`. Depende de F1. | RF-10, RF-11 | 0.4 |
| F7 | Réplica. Copiar desde `template/.agents/` los archivos de comandos, skill, plantilla, `validar-estructura.md` y `agents/reviewer.md` tocados en F3–F5. `diff -rq template/.agents .agents` no debe añadir rutas a la línea base de F0 (`napkin.md` y `skills/caveman`; `.agents/local/` se ignora). `git status` no muestra esta réplica. Depende de F3, F4 y F5. | RF-1, RF-3, RF-4, RF-5, RF-9, RF-12 | 0.4 |
| F8 | Versión, changelog y manifiesto, con el número aprobado en la decisión 5. `template/` ya no cambia en las fases siguientes. Regenerar el manifiesto entero; no se editan a mano las huellas viejas. Depende de F1–F7. | Criterio de finalización, E20 | 0.7 |
| F9 | ADR del clon, solo los que la persona haya aprobado, en el orden 0024–0027, más su fila en `.docs/004_adr/README.md`. Un rechazo no crea archivo ni ocupa número. No toca `template/`. Puede ir después de F8. | Decisiones 1 a 4 | 0.9 |
| F10 | Validación. Criterios de la sección 8, chequeos de `AGENTS.md` › Comandos en cada archivo tocado, E19, E20 y el ensayo de instalación otra vez. No reclasifica filas. | RF-1 a RF-12 | 0.7 |

Total: 8.1 h.

## 8. Criterios de aceptación
Cada criterio se comprueba leyendo el archivo citado. Los comandos de `AGENTS.md` › Comandos se corren en F0 y en F10, y el de codificación en cada archivo tocado.

Codificación, en cada archivo de la sección 2 que se haya escrito:

- `file -i <archivo>` da utf-8.
- `grep -l $'^\xEF\xBB\xBF' <archivo>` sale vacío.
- En comandos y en la skill, `name` del frontmatter es el nombre de su carpeta.
- Toda ruta citada con comillas inversas existe. No se exige eso de `manifiestos/`.
- En `template/` no quedan marcadores `<<…>>` nuevos; los únicos permitidos siguen siendo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- Build: no aplica. Consulta de datos: no aplica.
- Ensayo: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia sin pisar. Nunca en un proyecto real.

| RF | Se cumple cuando | Comprobación |
|---|---|---|
| RF-1 | La apertura sigue creando la fila en `Análisis` y el documento en `borrador`, y la forma de la celda remite a la leyenda. El cierre solo escribe `Implementado`, `Cerrado` o `Descartado`, y el documento `implementada`, `cerrada` o `descartada`, según su transición. La plantilla y la skill listan `cerrada` y `descartada` junto a `borrador`, `aprobada`, `analisis` e `implementada`. | Leer `sdd-spec.md`, `sdd-cierre.md`, `SKILL.md` y `plantillas.md` en `template/` y en la réplica. Las dos copias de cada archivo coinciden. |
| RF-2 | La leyenda del índice nombra las ocho palabras, pendiente, pendiente de desarrollo, grupo y cerrado, y dice que la respuesta es la celda. Ningún comando pide abrir cada carpeta para ese conteo. | Leer la sección de leyenda de `template/.docs/002_specs/README.md` y de `.docs/002_specs/README.md`. Buscar que `sdd-status.md` distinga el conteo del índice de la consulta de una carpeta. |
| RF-3 | `/sdd-status` informa la etapa de esa carpeta, avisa si la celda no coincide y declara que no modifica archivos. Las deudas se localizan por la columna Atendida por, sin recorrer cada detalle. | Leer `template/.agents/commands/sdd-status.md` y su réplica. |
| RF-4 | El cierre, hacia `Cerrado`, exige la previa y los cinco criterios, y deja uno de tres resultados: parar sin escribir esas palabras si la previa falla; `Implementado` e `implementada` si falta un criterio; `Cerrado` y `cerrada` si los cinco pasan. Esos dos documentos no cuentan como commit pendiente. Con la línea `analisis`, no exige la validación del conjunto. | Leer `template/.agents/commands/sdd-cierre.md`. El paso que antes fijaba siempre `implementada` ya no lo hace. Los criterios citan la leyenda y no la vuelven a redactar. |
| RF-5 | El descarte escribe `Descartado` y `descartada` sin la previa, y elige por cada `En atención` de este requerimiento según el supuesto de transiciones de deuda. | Leer el tramo de descarte de `sdd-cierre.md`. |
| RF-6 | No hay comando nuevo. La leyenda dice que la persona escribe `Diferido` con motivo y estatus de regreso, y que al reactivar devuelve esa celda. La línea del documento no cambia. Si se aprobó la decisión 3, Antecedentes ya no dice que solo `/sdd-spec` y `/sdd-cierre` actualizan cualquier estatus. | Leer la leyenda y los puntos 5 y 6 de ambos `AGENTS.md`. Confirmar que no existe un comando nuevo de pausa. |
| RF-7 | La leyenda dice que, si la celda y la etapa discrepan, los conteos siguen la celda y el estado y el chequeo avisan y no escriben. `sdd-status` y E22 citan ese párrafo. | Leer la leyenda, `sdd-status.md` y la fila E22. |
| RF-8 | Los README de deuda dicen qué deuda está cerrada y cuál está pendiente, y que una `En atención` o una pendiente sin clasificar con Atendida por de este requerimiento bloquea `Cerrado`. Siguen `Abierta`, `En atención`, `Resuelta` y `Descartada`. Las filas 0009, 0010 y 0011 siguen en su estado. | Leer los dos README de deuda y esas tres filas. |
| RF-9 | E22 existe después de E21, es `AVISO`, no escribe y cubre palabra fuera de la lista, celda mal formada, fecha o detalle faltantes, siguiente paso que no coincide, fila ausente, duplicada o desfasada. Cubre además, como `AVISO`, texto libre en la columna de pendiente de la fila del índice de un `Cerrado` o `Implementado`, y una deuda con estado fuera de los cuatro valores o `Resuelta` sin «Cómo se atendió». El alcance, el mensaje `estructura OK`, `sdd-estructura.md` y `reviewer.md` dicen E1–E22. E12, E13 y E21 conservan su hecho. | Leer `template/.agents/validar-estructura.md`, su réplica, `sdd-estructura.md` y `template/.agents/agents/reviewer.md` con su réplica. |
| RF-10 | La nota trae el mapa `Plan` → `Planificado`, `Spec aprobada` → `Análisis` sin plan y `Planificado` con plan, `Implementado en local` → `Implementado`. Las filas ya escritas del índice de este clon no fueron reescritas por la implementación. | Leer la nota y contrastar las filas del índice con su texto previo a F1. |
| RF-11 | `ACTUALIZAR.md` guía a la persona y dice que el procedimiento no reescribe el índice. El grupo C sigue sin comparar ni reemplazar `.docs/`. El changelog repite el mapa y no es una tercera leyenda. | Leer la nota, `### Grupo C` y la entrada nueva del changelog. |
| RF-12 | El cierre reabre `Cerrado` a `Implementado` e `implementada`, y `Descartado` a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa. Motivo, quién y fecha quedan en el punto de retomo. Una pausa no baja un `Cerrado` bien formado. | Leer el tramo de reapertura de `sdd-cierre.md`. |

Cierre de la validación: E19 igual que la línea base de F0 salvo los archivos de `template/.agents` ya replicados; E20 coincide con `manifiestos/<versión>.md`; el ensayo de instalación no pisa. Dos lecturas de las mismas celdas, la misma línea de documento y la misma fila `cierre` dan el mismo pendiente, el mismo pendiente de desarrollo, los mismos grupos y la misma respuesta de cerrado (RNF-1).
