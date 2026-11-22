# 01 — Spec: Arranque ligero de sesión
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
Cada sesión carga contexto antes de que la persona pida nada. La primera llamada de una sesión medida
el 2026-11-21 (transcript `.jsonl`) usó ~56k tokens de entrada. De ellos, ~26k son del sistema y de las
herramientas de Claude Code, y el kit no los controla. El resto lo pone el kit por dos vías:

- **#5, archivo siempre cargado:** `CLAUDE.md` inyecta `AGENTS.md` en cada sesión. Pesa 13 314 bytes
  (~3.8k tokens) en la raíz y 11 859 en `template/`. Algunas secciones solo las usa un rol concreto o
  un momento concreto, no cada turno. La idea inicial nombraba «Qué lee cada rol», «Reglas de
  negocio y del sistema» y «Comandos», entre otras; lo que sale se decide abajo.
- **#6, protocolo «Cada sesión»:** cumplido entero, lee ~9k tokens. El paso 7 abre
  `.agents/sync-agents.md` (10 689 bytes), aunque solo usa su «Chequeo de arranque» (2 063 bytes) y
  lo que ese chequeo necesita. El paso 4 abre `.agents/README.md` (4 927 bytes), que solo sirve al
  crear agentes, skills o comandos.

Objetivo: reducir de forma medible lo que el kit carga al arrancar. Se mantienen igual el
cumplimiento de los Guardrails, los avisos del arranque y lo que cada rol encuentra al leer una
sección por su nombre. El antecedente es `003_OptimizarArnes`, que redujo la lectura de agentes y
comandos con línea base en bytes, pero no tocó el arranque de la sesión principal. Esta spec reutiliza
su método: medir antes y después en bytes, y usar los tokens solo como referencia.

**Alcance (decisión de esta spec):** cubre las dos capas, la raíz (entorno de mantenimiento:
`AGENTS.md` y `.agents/`) y lo distribuible (`template/AGENTS.md` y `template/.agents/`). Motivo: el
protocolo «Cada sesión» y las secciones que leen los roles son el mismo texto en las dos capas, salvo
el paso 0 de la raíz. Además, el `.agents/` de la raíz es réplica de `template/.agents/`. Cambiar solo
una capa desincronizaría las referencias por nombre y los chequeos E7, E8, E9, E18 y E21.

**Secciones que salen (decisión de la persona, 2026-11-21):** solo las genéricas del kit que no se
usan en cada turno. Sale «Qué lee cada rol». En el plan se evalúan «Formato y numeración», «Deuda
técnica» y «Roles y comandos». Se quedan en `AGENTS.md` «Comandos», «Stack y convenciones de código»
y «Reglas de negocio y del sistema»: las llena `/sdd-init`, los roles las leen por nombre y, en
proyectos ya instalados, no se reemplazan.

## Usuarios y roles
- **Persona que trabaja con el kit:** paga el contexto del arranque en cada sesión, en el clon del kit
  y en cada proyecto instalado.
- **Sesión principal:** ejecuta «Cada sesión» y recibe `AGENTS.md` inyectado.
- **Roles con `omitClaudeMd` (planner, reviewer, coordinator):** no reciben `AGENTS.md` y leen
  secciones por nombre (Guardrails, Comandos, Stack, Git, Reglas duras, paso 1 de «Cada sesión»).
- **implementer:** puede recibir `AGENTS.md` inyectado y lee «Comandos» y «Stack».
- **Comandos y chequeos que citan secciones o archivos:** `/sdd-init` (llena Stack, Comandos y Reglas
  de negocio), `/sdd-estructura` (E7, E8, E9, E11, E16, E17, E18, E21), `/sdd-enlaces` (que cita
  `sync-agents.md` › «Chequeo de arranque») y `ACTUALIZAR.md` (grupo B: `AGENTS.md` nunca se
  reemplaza).

## Historias de usuario
- HU-1. Como persona que usa el kit, quiero que cada sesión arranque con menos contexto para gastar
  menos tokens y dejar más ventana para el trabajo.
- HU-2. Como sesión principal, quiero que el arranque me pida leer solo lo que uso en ese momento,
  para cumplir el protocolo completo sin cargar archivos enteros por una sección.
- HU-3. Como rol con `omitClaudeMd`, quiero que cada sección que mi instrucción nombra siga existiendo
  donde la busco, para no perder Guardrails, Comandos ni reglas.
- HU-4. Como persona con un proyecto ya instalado, quiero que actualizar el kit no rompa mi arranque
  aunque mi `AGENTS.md` conserve la forma anterior.

## Definiciones (solo si hay términos ambiguos)
- **Archivo siempre cargado:** `AGENTS.md`, que `CLAUDE.md` inyecta con `@AGENTS.md` en cada sesión.
- **Arranque:** lo que la sesión principal carga antes de atender la primera petición, es decir, el
  archivo siempre cargado más las lecturas de «Cada sesión».
- **Lectura del arranque bajo control del kit:** las lecturas obligatorias de «Cada sesión» que trae
  el kit: `.agents/napkin.md`, `MEMORY.md`, `.agents/README.md`, la constitución, lo leído para el
  chequeo de arranque y `.agents/skills/formato-respuesta/SKILL.md`. No incluye `PREFERENCIAS.md`
  (contenido personal de cada usuario, no versionado) ni lo que es condicional por petición (paso 6,
  «Antecedentes»).
- **Lectura bajo demanda:** contenido que se lee solo cuando el rol, comando o momento que lo usa lo
  pide, y no en cada sesión.
- **Referencia por nombre:** cita de la forma `AGENTS.md` › <Sección>, «<Sección>» o `<archivo>` ›
  «<Sección>» que un agente, comando, skill, chequeo o documento distribuible usa para localizar una
  regla.
- **Línea base (congelada 2026-11-21, `wc -c`, origen: este clon en `main`, 53f38a1 más cambios sin
  commit de las specs 0013 a 0015):**

| Pieza | Raíz (bytes) | `template/` (bytes) |
|---|---|---|
| `AGENTS.md` (siempre cargado) | 13 314 | 11 859 |
| `.agents/sync-agents.md` (entero) | 10 689 | 10 689 |
| de él, «Chequeo de arranque» | 2 063 | 2 063 |
| `.agents/README.md` | 4 927 | 4 927 |
| `MEMORY.md` | 3 415 | No aplica (lo crea el proyecto) |
| `.agents/skills/formato-respuesta/SKILL.md` | 2 443 | 2 443 |
| `.agents/napkin.md` | 1 579 | No aplica (lo crea el proyecto) |
| constitución | 1 133 | No aplica (la crea el proyecto) |
| **Lectura del arranque bajo control del kit (raíz)** | **24 186** | — |
| `PREFERENCIAS.md` (referencia, fuera de la medida) | 7 194 | No aplica |

  Bytes por sección del `AGENTS.md` de la raíz: Guardrails 2 568 · Cada sesión 2 245 · Antecedentes
  893 · Deuda técnica 691 · Formato y numeración 741 · Skills de proyecto 1 640 · Roles y comandos
  777 · Qué lee cada rol 948 · Git 374 · Stack 229 · Comandos 608 · Reglas de negocio 1 217 · Reglas
  duras 208.
- **Método de medición del arranque (antes y después):** se suma `wc -c` de cada archivo que el
  arranque abre entero. Si un paso remite a una sola sección de un archivo, cuenta esa sección: del
  encabezado al siguiente encabezado del mismo nivel, ambos con `wc -c`. La medición se hace en el
  **escenario base**: sesión en el clon (raíz) cuya petición no crea ni modifica agentes, skills ni
  comandos. El escenario que sí los crea se mide aparte, como referencia, y no cuenta para la meta.
- **Diferencias de réplica en la línea base (2026-11-21):** `diff -rq template/.agents .agents` ya
  muestra `napkin.md`, `agents/claudio.md`, `local/`, `skills/caveman` y `skills/reporte-sesiones`.
- **Renglón de remisión:** línea que nombra una sección o regla y dice dónde vive y cuándo leerla, sin
  repetir su contenido. No cuenta como texto duplicado.

## Requisitos funcionales (EARS)
- RF-1: EL SISTEMA define en una sola fuente el criterio de qué contenido va en el archivo siempre
  cargado y qué contenido es lectura bajo demanda. Los demás documentos remiten a ese criterio y no
  lo copian.
- RF-2: EL SISTEMA mantiene en el archivo siempre cargado, completa y sin resumir, la sección
  «Guardrails». La mantiene ahí también cuando otro contenido sale a lectura bajo demanda.
- RF-3: EL SISTEMA mantiene en el archivo siempre cargado el protocolo «Cada sesión», con todos sus
  pasos y en el mismo orden. Un paso puede remitir a una lectura bajo demanda. No puede desaparecer ni
  quedar sin efecto.
- RF-4: CUANDO una sección sale del archivo siempre cargado, EL SISTEMA deja en él un renglón que dice
  qué es la sección, dónde está y cuándo leerla. El contenido completo vive en una sola fuente.
- RF-5: CUANDO una sección cambia de lugar, EL SISTEMA actualiza en el mismo cambio toda referencia
  por nombre a ella en agentes, comandos, skills, chequeos, `INSTALAR.md`, `ACTUALIZAR.md` y
  `README.md`, en las dos capas. Ninguna referencia queda apuntando a un lugar donde la sección ya no
  está.
- RF-6: CUANDO un rol con `omitClaudeMd` busca una sección que su instrucción nombra, EL SISTEMA la
  tiene en el archivo que esa instrucción nombra, con el mismo nombre y el mismo contenido que antes
  del cambio. El número de archivos que el rol abre para reunir sus secciones no aumenta.
- RF-7: CUANDO la sesión principal hace el chequeo de arranque, EL SISTEMA le pide leer solo lo que
  ese chequeo necesita: sus reglas, los estados de enlace, el formato de «visto» y las filas de la
  tabla de enlaces de las herramientas declaradas en «visto». No le pide leer los pasos que ejecuta
  `/sdd-enlaces` ni las notas para el usuario.
- RF-8: CUANDO el chequeo de arranque corre con la lectura reducida, EL SISTEMA da el mismo resultado
  que antes en cada escenario: clon del kit sin `.kit-version` (se omite), proyecto sin «visto» (todo
  nuevo), proyecto con diferencias de versión, piezas o enlaces (un solo aviso de hasta 3 líneas) y
  proyecto sin diferencias (silencio). Sigue siendo de solo lectura.
- RF-9: EL SISTEMA mantiene una sola fuente para las reglas del chequeo de arranque y para las de
  `/sdd-enlaces`. Ningún texto queda duplicado entre las dos. Un renglón de remisión (ver
  «Definiciones») no cuenta como duplicado; así resuelve la referencia vieja de RF-13.
- RF-10: SI la petición de la sesión no crea ni modifica agentes, skills ni comandos, ENTONCES EL
  SISTEMA no pide leer `.agents/README.md` en el arranque. Lo pide cuando sí los crea o modifica,
  incluido `/sdd-init` al dar de alta skills.
- RF-11: CUANDO se comprueba la estructura del kit (`/sdd-estructura`), EL SISTEMA sigue verificando
  lo mismo que antes sobre las secciones y archivos que se movieron: tabla de skills (E7), roles
  (E8), comandos citados (E9), rutas citadas (E11), `CLAUDE.md` (E16), copias de skills (E17),
  Comandos (E18) y el marcador de instalación (E21). Lo verifica en el lugar donde ahora vive cada
  cosa, sin `FALLA` nueva en un kit correcto.
- RF-12: CUANDO `/sdd-init` personaliza un proyecto, EL SISTEMA escribe los valores del proyecto
  («Stack y convenciones de código», «Comandos» y «Reglas de negocio y del sistema») en `AGENTS.md`,
  como hoy. Cada rol encuentra ese valor donde su instrucción lo busca.
- RF-13: SI un proyecto se actualiza y conserva su `AGENTS.md` anterior (grupo B de `ACTUALIZAR.md`,
  nunca se reemplaza), ENTONCES EL SISTEMA sigue funcionando con él. Las referencias por nombre que
  ese `AGENTS.md` hace a otros archivos del kit siguen resolviendo. Los roles y comandos actualizados
  encuentran los valores del proyecto. `ACTUALIZAR.md` lista los cambios de «Cada sesión» y de
  secciones como faltantes que se incorporan a mano.
- RF-14: CUANDO termina la implementación, EL SISTEMA permite comparar contra la línea base, en bytes y
  con el mismo método (`wc -c`), el archivo siempre cargado (raíz y `template/`) y la lectura del
  arranque bajo control del kit. La comparación queda registrada en `05_avance.md`.

## Requisitos no funcionales
- Fuente única (constitución, punto 3; ADR 001): cada regla movida vive en un solo lugar. El archivo
  siempre cargado remite y no copia.
- `template/` no lleva nada propio de un proyecto ni del clon del kit (constitución, punto 1).
- La copia nunca sobrescribe (constitución, punto 5; ADR 007). Un proyecto instalado no pierde valores
  personalizados al actualizar.
- UTF-8 sin BOM, español. El cambio es distribuible: sube `VersionAgentKitSdd`, agrega la entrada de
  `ChangelogAgentKitSdd.md` y el manifiesto de la versión.
- Las réplicas se igualan: `diff -rq template/.agents .agents` no muestra diferencias nuevas frente a
  las de la línea base (ver «Definiciones»).
- Los pasos condicionales del arranque dicen en una línea cuándo aplican. No piden juicio largo para
  decidir si leer.

## Casos límite
- **Clon nuevo sin `.agents/skills/` (paso 0 de la raíz):** el arranque sigue avisando y remitiendo a
  `INSTALAR.md` › «Modo mantenimiento» sin depender de archivos que aún no existen. Como el paso 0
  detiene los pasos siguientes, una fuente nueva dentro de `.agents/` no se lee en ese caso.
- **Paso del arranque que depende de una sección que sale:** por ejemplo, el paso 8 (prefijos
  repetidos) usa «Formato y numeración». Si esa sección sale, el paso nombra dónde leerla y se sigue
  cumpliendo igual.
- **Subagente que recibe el archivo siempre cargado** (por ejemplo, implementer): no ejecuta «Cada
  sesión» ni sigue los renglones de remisión salvo que su instrucción lo pida. La regla «un subagente
  no repite el arranque» sigue en el archivo siempre cargado, aunque salga «Qué lee cada rol».
- **Instalación pendiente (paso 1, marcador `SDD-INIT-PENDIENTE`):** el marcador sigue siendo la
  primera línea no vacía tras el título del archivo siempre cargado. E21 y los roles que lo comprueban
  (coordinator, `/sdd`, skill `sdd`, `/sdd-init`, `README.md`) siguen leyéndolo igual.
- **Proyecto con «Reglas de negocio» grande:** la sección se queda en `AGENTS.md`, así que ninguna
  regla del proyecto queda sin lector. Su tamaño lo decide el proyecto y no cuenta para la meta.
- **Proyecto actualizado con `AGENTS.md` viejo:** su paso 7 sigue diciendo «`sync-agents.md` ›
  «Chequeo de arranque»». Esa referencia debe seguir resolviendo (RF-13).
- **Herramienta que no inyecta `AGENTS.md`** (por ejemplo, una sin `CLAUDE.md`): el ahorro no aplica,
  pero lo bajo demanda debe seguir localizable por nombre.
- **Choque con `0013_ModelosPorAgente`** (aprobada, siguiente paso plan): toca Guardrails regla 1
  (RF-25), `AGENTS.md` › Comandos (RF-28), `/sdd-init`, `restricciones-de-rol.md` y
  `validar-estructura.md`. Esta spec no cambia el contenido de Guardrails ni de Comandos, y los dos se
  quedan en `AGENTS.md`. Orden: la 0015 se implementa después de la 0013; clarify y plan pueden
  avanzar antes.
- **Choque con `0014_ConfiguracionGuiada`** (borrador): su duda abierta decide dónde va la configuración
  del proyecto. Sigue el criterio de RF-1: lo que no hace falta en cada turno no va en `AGENTS.md`.
- **Sesión que crea una skill a media conversación:** RF-10 exige leer `.agents/README.md` en ese
  momento, aunque no se leyera al arrancar.

## Fuera de alcance
- Contexto del sistema y de las herramientas de Claude Code (~26k tokens), conectores MCP, plugin
  caveman duplicado y skills de claude.ai (ya resuelto o descartado por la persona).
- El contenido de `PREFERENCIAS.md` (personal, no versionado), de `MEMORY.md`, de `.agents/napkin.md`
  y de la constitución. Se siguen leyendo enteros en el arranque.
- Reescribir la redacción de los Guardrails o de las reglas que se muevan. Se mueven, no se cambian.
- `.agents/skills/formato-respuesta/SKILL.md`: se sigue leyendo en el arranque y esta spec no lo
  reduce.
- La lectura de agentes y comandos fuera del arranque (cubierta por `003_OptimizarArnes`).

## Criterios de finalización
- La línea base de «Definiciones» está registrada en `05_avance.md` antes del primer cambio, y la
  medición final, con el mismo método (RF-14).
- La lectura del arranque bajo control del kit (raíz), medida con el método y en el escenario base de
  «Definiciones», baja al menos un 35 % frente a 24 186 bytes.
- La meta de reducción del `AGENTS.md` (raíz frente a 13 314 bytes y `template/` frente a 11 859) se
  fija en `03_plan.md`, cuando se sepa qué sale.
- «Guardrails» sigue en el archivo siempre cargado de las dos capas. Su texto es idéntico al de antes
  del cambio, salvo lo que haya aplicado otra spec.
- Cada referencia por nombre a una sección o archivo movido resuelve (E11 sin `FALLA`). `/sdd-estructura`
  da E1–E22 sin `FALLA` nueva en el clon y en el ensayo de instalación.
- Los cuatro escenarios de RF-8 dan el mismo aviso o el mismo silencio que antes.
- Ensayo de instalación en `mktemp -d` sin pisar nada. `diff -rq template/.agents .agents` sin
  diferencias nuevas frente a la línea base. Pasan los chequeos de lint de `AGENTS.md` › Comandos.
- En un ensayo de actualización (`mktemp -d`) con el `AGENTS.md` de la versión anterior y el
  `.agents/` nuevo, la referencia `sync-agents.md` › «Chequeo de arranque» resuelve y el chequeo da
  el mismo resultado que con el `AGENTS.md` nuevo (RF-13).
- `ACTUALIZAR.md` describe qué incorporar a mano en un `AGENTS.md` existente (RF-13).
- Version, Changelog y manifiesto actualizados.
- Referencia, no criterio: tokens de entrada de la primera llamada de una sesión nueva, medidos igual
  que la línea de ~56k.

## Dudas abiertas
Ninguna. Resueltas por la persona el 2026-11-21:
- Secciones: solo las genéricas del kit. Sale «Qué lee cada rol»; «Formato y numeración», «Deuda
  técnica» y «Roles y comandos» se evalúan en el plan; «Comandos», «Stack» y «Reglas de negocio» se
  quedan (ver «Contexto y objetivo»).
- Metas: la lectura del arranque bajo control del kit baja al menos un 35 %. La meta de `AGENTS.md`
  se fija en el plan.
- Orden: la 0015 se implementa después de la 0013. Clarify y plan pueden avanzar antes.
- Migración a mano: los proyectos ya instalados incorporan el nuevo «Cada sesión» siguiendo
  `ACTUALIZAR.md` (ADR 007). La referencia vieja a `sync-agents.md` › «Chequeo de arranque» sigue
  resolviendo (RF-13).
- La 0014 sigue el criterio de RF-1.
