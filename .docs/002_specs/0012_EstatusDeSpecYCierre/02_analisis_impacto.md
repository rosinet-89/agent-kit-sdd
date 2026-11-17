# 02 — Análisis de impacto: Estatus de spec y criterio de cerrado

## 1. Resumen del requerimiento
La spec aprobada (`01_spec.md`, Estado: aprobada, issue #5) define qué cuenta como pendiente, pendiente de desarrollo y cerrado a partir de la celda del índice de specs, añade `Cerrado` y `Descartado`, y separa esos estatus de la línea `Estado` del documento. El cierre escribe `Implementado` o `Cerrado` solo si pasa la comprobación previa, y `Descartado` sin esa previa. El estado de una carpeta y el chequeo de estructura avisan el desfase y no escriben. La deuda gana las definiciones de cerrada y pendiente sin cambiar los nombres `Abierta`, `En atención`, `Resuelta` y `Descartada`.

Es un cambio al kit: Markdown, sin scripts y sin base de datos. Lo distribuible se edita en `template/` y en los docs de la raíz. El `.agents/` de la raíz es la réplica local de `template/.agents` (constitución, principio 3; `MEMORY.md` decisión 2026-10-24).

### 1.1 Antecedentes (índice de planes: carpetas que coinciden, qué se reutiliza y qué contradice; o "sin antecedentes en el índice" + grep que lo confirma)
Índice leído: `.docs/002_specs/README.md`.

La única carpeta cuyo tema es el vocabulario de estatus de una spec es esta: `0012_EstatusDeSpecYCierre` (fila en `.docs/002_specs/README.md:38`). El grep del índice confirma que `Cerrado`, `Descartado` y `pendiente de desarrollo` aparecen solo en esa fila (2026-11-16 14:12). No hay otra carpeta que ya defina un estatus terminal ni el criterio de cerrado.

Dependencias parciales, por el archivo que este cambio vuelve a tocar, no por el vocabulario:

- `001_ActualizarKit` (`.docs/002_specs/README.md:27`): el procedimiento de `ACTUALIZAR.md`. Ahí entra la nota de reclasificación (RF-11). No define estatus de spec.
- `0010_FalsoPositivoInitPendiente` (`.docs/002_specs/README.md:36`): el chequeo E21 vive en `validar-estructura.md`. El patrón reusable es un chequeo nuevo que avisa o falla sin escribir. El tema de esa carpeta es el marcador de instalación, no el estatus.
- `0011_ReducirCeremoniaSdd` (`.docs/002_specs/README.md:37`): `topes-y-modos.md` es la fuente única de sus reglas y los comandos la enlazan (`ChangelogAgentKitSdd.md:19-20`). Ese patrón es la alternativa ya usada en el kit para no copiar una regla en cada comando.

La leyenda vigente contradice la spec: seis palabras, sin terminal (`.docs/002_specs/README.md:20-21` y `template/.docs/002_specs/README.md:20-21`).

### 1.2 Deuda técnica relacionada (`.docs/003_deuda/README.md`: deudas abiertas del módulo y si este feature las resuelve o las deja; o "ninguna")
Abiertas en `.docs/003_deuda/README.md:51-53`. Ninguna describe el estatus de una spec ni el criterio de cerrado. Este feature no las resuelve.

| Prefijo | Título en el índice | Módulo | Por qué no es este tema |
|---|---|---|---|
| 0009 | Garantía técnica de que la base de datos solo se lee | Guardrails, `restricciones-de-rol.md` | Lectura de BD. Este cambio no tiene BD. |
| 0010 | La raíz del clon no tiene chequeo automático del marcador | `/sdd-estructura` (E21) | Mismo archivo de chequeos, otro hecho (el marcador). E21 se queda. |
| 0011 | `/sdd-constitucion` fuera de la tabla de fases y del ruteo | Skill `sdd`, `coordinator` | Ruteo de un comando. No define estatus. |

La spec deja fuera que cada comando de fase actualice la celda de `Planificado`, `En curso` y `En pruebas`, y pide proponer esa deuda para que la persona la dé de alta. No se crea en este análisis.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica. No hay aplicación ni pantallas. El acceso son los comandos `/sdd-spec`, `/sdd-cierre`, `/sdd-status` y `/sdd-estructura`, y los dos índices (`.docs/002_specs/README.md`, `.docs/003_deuda/README.md`).

### 2.2 Tablas (existentes y nuevas)
No aplica. Sin base de datos. `AGENTS.md` › Comandos: consulta de datos «No aplica».

### 2.3 Archivos de código (ruta, capa)
Capa: Markdown del kit. La fuente distribuida está en `template/`; la réplica local de `template/.agents` está en `.agents/` y E19 la compara (`template/.agents/validar-estructura.md:36`). Los docs de la raíz (`ACTUALIZAR.md`, `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd`) se editan en la raíz. `AGENTS.md` de la raíz es conocimiento versionado del clon; `template/AGENTS.md` es el que se instala (grupo B, `ACTUALIZAR.md:289`).

Los README de `.docs/` del clon no son el mismo archivo que los de `template/.docs/`. En el clon, la numeración ya remite a la plantilla (`.docs/002_specs/README.md:58`). La leyenda de estatus está copiada (líneas 20-21 en los dos).

| Archivo | Qué cambia | RF |
|---|---|---|
| `template/.docs/002_specs/README.md` | La leyenda de las líneas 20-21 pasa a las ocho palabras de la spec, con forma de la celda, pendiente, pendiente de desarrollo, grupo, desfase y quién escribe cada transición. El bullet de la línea 41 («Estatus y fechas salen del PUNTO DE RETOMO») deja de ser la fuente del pendiente. La sección «Numeración única» (desde la línea 44) no se reescribe. | RF-1, RF-2, RF-7, RF-10 |
| `.docs/002_specs/README.md` | La misma leyenda (líneas 20-21) y el bullet de la línea 53. Las filas ya escritas (líneas 27-38) no se reclasifican: la nota de actualización guía a la persona (RF-11). | RF-1, RF-2, RF-7, RF-10, RF-11 |
| `template/.docs/003_deuda/README.md` | Bajo «Estados» (líneas 25-34) se añade qué deuda está cerrada (`Resuelta` o `Descartada`) y cuál está pendiente, y el bloqueo de `Cerrado` por Atendida por. Los cuatro nombres se conservan. El índice vacío de la plantilla sigue vacío. | RF-8 |
| `.docs/003_deuda/README.md` | La misma definición bajo «Estados» (líneas 25-32). Las filas 0009, 0010 y 0011 no cambian de estado. | RF-8 |
| `template/.agents/commands/sdd-spec.md` | Sigue creando la fila en `Análisis` (línea 12) y el documento en `borrador` (línea 15). La forma de la celda remite a la leyenda, sin copiar la tabla. | RF-1 |
| `.agents/commands/sdd-spec.md` | Réplica local del comando anterior (hoy la línea 12 coincide). | RF-1 |
| `template/.agents/commands/sdd-cierre.md` | El paso 4 (línea 19) deja de fijar siempre `implementada`. El paso 6 (línea 21) nombra `Implementado`, `Cerrado` o `Descartado` según la previa, los cinco criterios y la transición de descarte. La previa que ya está en el paso 2 se alinea con la de la spec. Las transiciones de deuda se juzgan y las escribe este cierre, con la decisión de la persona. | RF-1, RF-4, RF-5, RF-8, RF-12 |
| `.agents/commands/sdd-cierre.md` | Réplica local (hoy las líneas 19 y 21 coinciden). | RF-1, RF-4, RF-5, RF-8, RF-12 |
| `template/.agents/commands/sdd-status.md` | Hoy lee la línea `Estado` y el punto de retomo, y no lee la celda (líneas 8-10). Pasa a informar la etapa de esa carpeta, avisar el desfase con la celda y no modificar archivos. | RF-3, RF-7 |
| `.agents/commands/sdd-status.md` | Réplica local (hoy la línea 6 coincide: no modifica archivos). | RF-3, RF-7 |
| `template/.agents/commands/sdd-estructura.md` | La línea 10 dice «E1 a E21». El rango incluye el aviso nuevo de estatus. Sigue sin escribir y sin redefinir el resto de chequeos. | RF-9 |
| `.agents/commands/sdd-estructura.md` | Réplica local (hoy la línea 10 coincide). | RF-9 |
| `template/.agents/validar-estructura.md` | Aviso nuevo: palabra fuera de la lista, celda mal formada, fecha o detalle faltantes, siguiente paso que no coincide, fila ausente, duplicada o desfasada de la etapa. Avisa, no es `FALLA` y no escribe (spec, definición de chequeo). Hoy el rango está en las líneas 9 y 88 (`E1 a E21`, `E1–E21`) y E13 (línea 30) solo cruza fila y archivo. E12 (línea 29) y E21 (línea 38) no cambian de hecho. | RF-9 |
| `.agents/validar-estructura.md` | Réplica local (hoy las líneas 9 y 88 coinciden). | RF-9 |
| `template/.agents/skills/sdd/SKILL.md` | La línea 98 lista `borrador \| aprobada \| analisis \| implementada`. Entran `cerrada` y `descartada`. La línea 25 sigue mandando a actualizar el índice al abrir carpeta y en el cierre; el significado de la celda no se copia aquí. | RF-1, RNF-2, RNF-4 |
| `.agents/skills/sdd/SKILL.md` | Réplica local (hoy las líneas 92 y 98 coinciden). | RF-1, RNF-2, RNF-4 |
| `template/.agents/skills/sdd/plantillas.md` | La línea 10 de la plantilla de `01_spec.md` añade `cerrada` y `descartada` al campo `Estado`. El resto de plantillas no copia la tabla de estatus del índice. | RF-1, RNF-2 |
| `.agents/skills/sdd/plantillas.md` | Réplica local (hoy la línea 10 coincide). | RF-1, RNF-2 |
| `template/AGENTS.md` | Antecedentes, puntos 5 y 6 (líneas 72-73): «manda la carpeta» y «lo hacen `/sdd-spec` y `/sdd-cierre`». Esas dos frases chocan con el desfase y con `Diferido`, que escribe la persona. El cambio, si se confirma, es un remitir a la leyenda, sin copiar la tabla. Grupo B: un proyecto ya instalado lo incorpora a mano. | RF-6, RF-7 |
| `AGENTS.md` | Las mismas frases en las líneas 73-74. Es el `AGENTS.md` versionado del clon. | RF-6, RF-7 |
| `ACTUALIZAR.md` | Nota de reclasificación (RF-11): `Plan` → `Planificado`; `Spec aprobada` → `Análisis` sin plan y `Planificado` con plan; `Implementado en local` → `Implementado`. Guía a la persona y no reescribe el índice. `.docs/` es grupo C (`ACTUALIZAR.md:293-294`): la actualización no pisa el README del proyecto. | RF-11 |
| `VersionAgentKitSdd` | Hoy `0.7.0` (archivo de una línea). Pasa a la versión que publique este cambio. La constitución (principio 6) pide versión y changelog en cada versión. | Criterio de finalización |
| `ChangelogAgentKitSdd.md` | Entrada de esa versión con el comportamiento nuevo y la misma nota de reclasificación. La última entrada es `## 0.7.0 — 2026-11-03` (línea 3). | Criterio de finalización |
| `manifiestos/<versión>.md` | Se crea. E20 compara la huella de cada archivo de `template/` con `manifiestos/<VERSION>.md` (`template/.agents/validar-estructura.md:37`). El manifiesto `0.7.0.md` ya lista los archivos de `template/` que este cambio edita (líneas 13, 17, 22, 23, 35, 37, 42, 44, 45 y 49). Esas huellas dejan de coincidir en cuanto `template/` cambie. | E20, criterio de finalización |

`Diferido`, la pausa y la reactivación los escribe la persona (spec, transiciones). No hay un comando que hoy lo haga, y no se crea uno: la leyenda y la nota dicen cómo.

### 2.4 Código legado o sin uso (no tocar, sí documentar)
Quedan como están. El grep del 2026-11-16 14:12 no encontró `Implementado`, `Diferido`, `En pruebas`, `En curso`, `Planificado`, `implementada`, `Cerrado`, `Descartado`, `Estado:` ni `estatus` en `README.md`, `INSTALAR.md`, `ChangelogAgentKitSdd.md` ni en `manifiestos/` (el changelog no usa esas palabras; igual se edita por el criterio de versión).

- `template/.agents/commands/sdd-clarify.md:20` escribe `Estado: aprobada`. Sigue siendo un valor del documento.
- `template/.agents/commands/sdd-plan.md:15` escribe `Estado: analisis` en solo análisis. La spec deja fuera que este comando pase la celda a `Planificado`.
- `sdd-tasks`, `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-change` y `coordinator.md`: el grep de estatus no los marca como escritores de la celda. `coordinator.md:10` lee el índice solo para ubicar una carpeta.
- `template/.agents/skills/sdd/topes-y-modos.md` y `avance-y-tiempos.md`. En este último, `[~] En curso` (línea 23) es la leyenda de un paso, no el estatus del índice.
- `template/.agents/skills/adr/SKILL.md:45` y `template/.docs/004_adr/README.md:16`: `Descartado` es un estado de ADR. Otro índice, otras palabras (`Propuesto`, `Aceptado`, `Reemplazado`).
- `manifiestos/0.4.0.md`, `0.5.0.md`, `0.6.0.md` y `0.7.0.md`: manifiestos ya congelados. El nuevo es otro archivo.
- Constitución, `MEMORY.md`, numeración única, E12 y E21: se citan y no se reescriben.
- La sección `## 06_flujo.md` de `plantillas.md` («Estatus y transiciones») es la guía operativa de un feature, no la leyenda del índice.

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)
No aplica. No hay base de datos. `AGENTS.md` › Comandos declara la consulta de datos como «No aplica», y este cambio no añade una.

Evidencia usada, 2026-11-16 14:12, solo lectura de archivos:

- Grep en `template/` de `Implementado`, `Diferido`, `En pruebas`, `En curso`, `Planificado`, `implementada`, `Cerrado`, `Descartado`, `Estado:` y `estatus`. Coincidencias en `template/.docs/002_specs/README.md`, `template/AGENTS.md`, `template/.agents/commands/sdd-cierre.md`, `sdd-plan.md`, `sdd-spec.md`, `sdd-clarify.md`, `template/.agents/skills/sdd/SKILL.md`, `avance-y-tiempos.md`, `plantillas.md`, `template/.agents/skills/adr/SKILL.md` y `template/.docs/004_adr/README.md`. `sdd-status.md` coincide por la palabra `Estado` de la spec, no por una celda. `sdd-estructura.md` no usa esas palabras; el rango E1–E21 está en su línea 10.
- El mismo grep en `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, `ChangelogAgentKitSdd.md` y `manifiestos/`: sin coincidencias.
- Grep de `estatus` en `template/`: solo `template/.docs/002_specs/README.md:39`, `template/AGENTS.md:73`, `sdd-cierre.md:21` y `sdd-spec.md:12`.
- Grep de `Cerrado`, `Descartado` y `pendiente de desarrollo` en `.docs/002_specs/README.md`: solo la fila 0012 (línea 38).
- `VersionAgentKitSdd` leído: `0.7.0`.

## 4. Flujo actual
Hoy hay dos vocabularios. La celda del índice y la línea `Estado` del documento no usan las mismas palabras, y ningún comando escribe `Cerrado` ni `Descartado`.

Quién escribe la fila del índice:

- Al crear la carpeta, `/sdd-spec` agrega la fila con estatus `Análisis` (`template/.agents/commands/sdd-spec.md:12`; la réplica `.agents/commands/sdd-spec.md:12` dice lo mismo). El índice lo repite: se actualiza al abrir carpeta y en el cierre (`template/.docs/002_specs/README.md:12-13`).
- En el cierre, el paso 6 actualiza estatus, fecha y pendiente sin nombrar el valor (`template/.agents/commands/sdd-cierre.md:21`).
- `AGENTS.md:74` y `template/AGENTS.md:73` dicen que la fila la actualizan `/sdd-spec` y `/sdd-cierre`.
- Ningún comando escribe `Planificado`, `En curso`, `En pruebas`, `Diferido`, `Cerrado` ni `Descartado`. La leyenda sí nombra los cuatro primeros (`template/.docs/002_specs/README.md:20-21`).

Quién escribe la línea `Estado` de `01_spec.md`:

- El planner, en borrador (`template/.agents/commands/sdd-spec.md:15`).
- `/sdd-clarify`, a `aprobada` (`template/.agents/commands/sdd-clarify.md:20`).
- `/sdd-plan`, a `analisis` si el trabajo es solo análisis (`template/.agents/commands/sdd-plan.md:15`).
- `/sdd-cierre`, a `implementada` (`template/.agents/commands/sdd-cierre.md:19`). No existe escritura de `cerrada` ni `descartada`. La plantilla ofrece `borrador | aprobada | analisis | implementada` (`template/.agents/skills/sdd/plantillas.md:10`).

`/sdd-status` informa fase y `Estado`, y no modifica archivos (`template/.agents/commands/sdd-status.md:6-12`). No lee la celda ni calcula una etapa de carpeta.

Hecho ya visible en este clon: la spec de 0012 está en `Estado: aprobada` y la celda sigue en `Análisis (2026-11-16)` con el pendiente «falta la aprobación de la persona» (`.docs/002_specs/README.md:38`). La regla vigente dice que, si fila y carpeta discrepan, manda la carpeta (`AGENTS.md:73`).

## 5. Reglas de negocio: conocidas vs código
Reglas que hoy están escritas y chocan con la spec aprobada:

1. La lista cerrada de la celda es `Análisis`, `Planificado`, `En curso`, `En pruebas`, `Implementado` y `Diferido` (`template/.docs/002_specs/README.md:20-21`). La spec conserva esas seis y añade `Cerrado` y `Descartado`.
2. «Estatus y fechas salen del PUNTO DE RETOMO» (`template/.docs/002_specs/README.md:41` y `.docs/002_specs/README.md:53`). La spec dice que pendiente, grupo y cerrado salen de la celda, y que la etapa de la carpeta no asigna grupo.
3. «Si el índice y la carpeta discrepan, manda la carpeta» (`AGENTS.md:73`, `template/AGENTS.md:72`, y el índice en `template/.docs/002_specs/README.md:13-14`). E13 repite que, si índice y carpeta discrepan, manda la carpeta (`template/.agents/validar-estructura.md:30`). La spec distingue: al consultar una carpeta se informa la etapa y se avisa; los conteos del índice siguen la celda.
4. El cierre siempre deja el documento en `implementada` y no nombra el estatus de la celda (`sdd-cierre.md:19` y `:21`). La spec exige tres resultados: parar sin escribir si falla la previa; `Implementado` e `implementada` si falta un criterio; `Cerrado` y `cerrada` si pasan los cinco.
5. La deuda define cuatro estados y dice que `Resuelta` y `Descartada` no se borran (`template/.docs/003_deuda/README.md:25-34`). No dice cuáles están cerrados ni que una `En atención` con Atendida por de este requerimiento impide `Cerrado`. Sí dice que, si índice y detalle discrepan, manda el detalle (línea 23), que es la regla que la spec reutiliza para el origen.
6. Cada regla vive en una sola fuente (constitución, principio 3). Hoy la leyenda está copiada en los dos README de specs, el campo `Estado` está copiado en la plantilla y en la skill, y «manda la carpeta» está en `AGENTS.md`, en el índice y en E13.

Reglas que la spec conserva y este cambio no reabre: numeración única, guardrails, los cuatro nombres de deuda, y que despliegues y cambios de datos los ejecuta una persona. Los nueve supuestos de `01_spec.md` quedan como están.

## 6. Riesgos y casos borde
Instalaciones ya escritas. `.docs/` es grupo C: actualizar el kit no reescribe el índice (`ACTUALIZAR.md:293-294`). Un proyecto que reciba los comandos nuevos (grupo A, solo si salen KIT-VIEJO) y conserve la leyenda vieja tendrá comandos que escriben `Cerrado` y un README que no lo lista. La nota de `ACTUALIZAR.md` es el único conducto, y no escribe celdas. En este clon, las filas 001 a 0011 empiezan por `Implementado` con formas distintas (`Implementado (2026-10-25)`, `Implementado (retroactivo, 2026-10-25)`). La spec clasifica esa palabra aunque falten fecha o detalle, y el chequeo avisa. Esas filas siguen contando como pendientes (grupo por cerrar) hasta que una persona las lleve a `Cerrado` o `Descartado`. El mapa de RF-10 no las convierte solo.

Ensayo de instalación. El comando de `AGENTS.md` › Comandos copia `template/` con `cp -R --update=none` a un temporal y no debe pisar. Este cambio no añade scripts. El ensayo cubre la copia de `template/`; no cubre `manifiestos/` ni el `.docs/` versionado del clon.

E12 y los chequeos. E12 (línea 29 de `validar-estructura.md`) cuenta prefijos; editar la leyenda en el mismo README no debe tocar «Numeración única». E13 hoy trata la discrepancia índice/carpeta como hecho del chequeo de docs, con la frase «manda la carpeta». Meter ahí el aviso de forma de la celda mezcla un cruce de filas con un aviso que no frena. E20 falla si `template/` cambia y el manifiesto de `VersionAgentKitSdd` no. E19 falla si `template/.agents` y `.agents/` quedan distintos, salvo `napkin.md`. El texto `estructura OK (E1–E21…)` (`validar-estructura.md:88`) queda viejo si el rango crece y no se actualiza en los dos sitios (líneas 9 y 88, más `sdd-estructura.md:10`).

Una sola fuente. Copiar la tabla en el README, en `sdd-cierre`, en `sdd-status`, en la skill y en `AGENTS.md` deja cinco sitios que pueden divergir. La constitución pide una fuente. El precedente de 0.7.0 es una pieza (`topes-y-modos.md`) que los demás enlazan. La leyenda del índice es lo que un agente lee para responder el pendiente sin abrir carpetas (RF-2): si la fuente se mueve a otro archivo, el índice tiene que remitir, no repetir.

Otras colisiones. `Descartado` ya es un estado de ADR (`template/.docs/004_adr/README.md:16`). `En curso` ya es la marca `[~]` de un paso (`avance-y-tiempos.md:23`). Un agente que mezcle esos vocabularios clasifica mal. `AGENTS.md` es grupo B (`ACTUALIZAR.md:289`): cambiar las líneas 73-74 obliga a cada proyecto a incorporarlas a mano, como ya pasó con el paso 1 y los guardrails (`ChangelogAgentKitSdd.md:36-38` y `:62-63`).

La fila 0012 de este clon ya está desfasada respecto de `Estado: aprobada` (sección 4). El análisis no la reescribe.

## 7. Preguntas abiertas
Candidatas a ADR (alternativas reales, efecto duradero; no se crea el ADR aquí):

1. Dónde vive la definición. Una sección de `template/.docs/002_specs/README.md` (y la de deuda en su README), y comandos, skill y `AGENTS.md` solo remiten. La alternativa es una pieza nueva al estilo de `topes-y-modos.md`, o copiar la tabla en cada comando. Lo segundo choca con el principio 3 de la constitución.
2. El aviso del chequeo. Un chequeo nuevo después de E21, solo `AVISO`, o ampliar E13. E13 hoy cruza fila y archivo y dice que manda la carpeta (`validar-estructura.md:30`). El aviso de la spec no frena y no escribe.
3. `AGENTS.md` (grupo B). Remitir los puntos 5 y 6 de Antecedentes a la leyenda, en la plantilla y en el clon, o dejar esas frases y aceptar que contradicen el desfase y a la persona que escribe `Diferido`.
4. La leyenda de este clon. Copiarla en `.docs/002_specs/README.md` y en `.docs/003_deuda/README.md`, o remitir a `template/` como ya hace la numeración (`.docs/002_specs/README.md:58`). En un proyecto instalado no existe ese `template/` como fuente: la definición que se distribuye tiene que estar completa en el README de `template/.docs/`.

Pregunta que no es ADR: qué número de versión se publica. La spec pide actualizar versión y changelog, y no fija el número. `VersionAgentKitSdd` está en `0.7.0`.
