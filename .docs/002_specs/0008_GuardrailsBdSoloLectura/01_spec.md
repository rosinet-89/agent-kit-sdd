# 01 — Spec: Guardrails globales: la base de datos solo se lee
Estado: implementada
Ticket: issue #1 rosinet-89/agent-kit-sdd

## Contexto y objetivo
La regla «los agentes solo leen la base de datos» hoy está repartida en cuatro sitios: `AGENTS.md` › Comandos, la skill `sdd` (Implementación, reglas duras, y la línea de Datos), `restricciones-de-rol.md` (solo para `planner`) y la constitución que propone `/sdd-init`. Todos cubren el flujo `sdd` o un rol concreto. Un agente que trabaja fuera del flujo (fix chico, consulta suelta, otra herramienta) no tiene una regla explícita. Además, `planner`, `reviewer` y `coordinator` declaran `omitClaudeMd` (0.5.0): no reciben `AGENTS.md` inyectado y solo leen las secciones que su instrucción nombra.

Objetivo: que el kit entregue una sección global «Guardrails» en `template/AGENTS.md`, con la regla de solo lectura de la BD escrita una sola vez, y que el resto de piezas remita a ella (fuente única, ADR 009). La misma sección y las mismas remisiones se aplican a la raíz del clon (`AGENTS.md` y `.agents/`), que es el proyecto que mantiene el kit. Se deja sitio para más guardrails globales después. El motivo: una regla de seguridad sobre datos no puede depender de que el agente esté dentro del flujo ni de que reciba el archivo inyectado.

Antecedentes: 002_Kit050 (skill `migracion-bd`), 003_OptimizarArnes (fuente única, ADR 009), 004_ModeloYPermisosAgentes (`omitClaudeMd`, ADR 011/012/013). No hay deuda abierta del tema.

## Usuarios y roles
- Persona que mantiene un proyecto con el kit: quiere que ningún agente toque datos.
- Persona que ejecuta scripts de BD: recibe el script generado y lo corre.
- Agentes `planner`, `reviewer`, `coordinator` e implementador, y cualquier agente fuera del flujo `sdd`.
- Mantenedor del kit: publica la versión y el aviso de actualización.

## Historias de usuario
- HU-1. Como persona del proyecto, quiero que todo agente, dentro o fuera del flujo `sdd`, solo lea la base de datos, para que ningún agente modifique datos ni esquema.
- HU-2. Como persona del proyecto, quiero que, si hace falta un cambio de datos o esquema, el agente me entregue el script con su rollback, para ejecutarlo yo.
- HU-3. Como mantenedor del kit, quiero la regla en una sola fuente y el resto remitiendo a ella, para no tener redacciones que diverjan.
- HU-4. Como persona con un proyecto ya actualizado a 0.5.0, quiero saber qué incorporar a mano, para adoptar la sección sin perder mis personalizaciones.
- HU-5. Como persona del proyecto, quiero que `restricciones-de-rol.md` diga con honestidad que la regla es una instrucción y no una garantía técnica, para saber qué refuerzo externo conviene.

## Definiciones (solo si hay términos ambiguos)
- Guardrail global: regla que aplica a cualquier agente, sesión o subagente, con cualquier herramienta, dentro o fuera del flujo `sdd`, y que ninguna skill, comando, plan ni preferencia relaja.
- Consulta de lectura (en SQL): únicamente `SELECT`, `SHOW`, `DESCRIBE`, `EXPLAIN` sin `ANALYZE` (que ejecuta la sentencia) y un CTE `WITH` cuyo cuerpo sea solo `SELECT`; o su equivalente de solo lectura en otro motor. Lista cerrada y criterio único, sin depender del issue: lo no listado se trata como prohibido, aunque no figure en la lista de prohibidos (p. ej. `SET` de sesión, `EXPLAIN ANALYZE`, CTE con escritura, `CALL`, `WITH` que no se pueda demostrar de solo lectura).
- Variantes de lectura con efecto: `SELECT ... INTO`, `SELECT ... FOR UPDATE` o `FOR SHARE`, funciones con efectos y locks quedan fuera de la lista cerrada y están prohibidas por precedencia, aunque empiecen por `SELECT`. Decisión de la spec.
- Precedencia: la lista de lectura permitida manda sobre la de prohibidos; ante duda, el agente no ejecuta y pide a la persona.
- Repetición de la regla: la redacción propia de la prohibición que sigue en las líneas de la lista fija de RF-13, fuera de `AGENTS.md` › Guardrails (en `template/` y en la raíz del clon). No se verifica con un grep por sentencias sino con esa lista fija de líneas o frases por archivo (decisión de la spec). Quedan excluidas de la verificación las reglas de redacción del SQL entregado (p. ej. `DROP`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE` del script y su respaldo en `migracion-bd`), porque describen cómo escribir el script y no la prohibición. Mencionar «consultas de solo lectura» dentro de un procedimiento (p. ej. cómo consultar) tampoco cuenta.
- Tecnología concreta de un proyecto: nombre de motor de BD, framework, librería o producto de datos específico (p. ej. un SGBD, un ORM o una herramienta comercial de datos). Nombrar sentencias SQL genéricas o la carpeta `sql/` NO cuenta. Los nombres de herramientas de agente (Cursor, Claude Code, etc.) y los ejemplos de archivos de build no son tecnología de proyecto.
- Escritura indirecta: efecto de escritura obtenido sin sentencia directa, p. ej. tareas programadas, scripts, endpoints o servicios que guardan, carga de archivos `.sql`, o un harness que hace más que leer.

## Requisitos funcionales (EARS)
- RF-1: EL SISTEMA (`template/AGENTS.md`) DEBE incluir una sección «Guardrails (todos los agentes, siempre)» después de la introducción y antes de «Cada sesión», que declare su alcance global y que ninguna skill, comando, plan ni preferencia la relaja.
- RF-2: EL SISTEMA DEBE numerar los guardrails de esa sección y dejar la estructura abierta para añadir más guardrails globales después.
- RF-3: EL SISTEMA DEBE declarar en el guardrail 1 «La base de datos solo se lee»: en cualquier BD y entorno (local, pruebas o producción) el agente solo ejecuta consultas de lectura, según la lista cerrada de la definición.
- RF-4: EL SISTEMA DEBE enumerar en el guardrail 1 lo permitido (lista cerrada) y, a modo de ejemplo, lo prohibido: sentencias que escriban datos o esquema (`INSERT`, `UPDATE`, `DELETE`, `REPLACE`, `TRUNCATE`, `CREATE` incluido `TEMPORARY`, `ALTER`, `DROP`, `SET`, `CALL`) o su equivalente en otro motor. La lista de lectura tiene precedencia: todo lo no listado como lectura se trata como prohibido, incluido `SET` de sesión.
- RF-5: EL SISTEMA DEBE prohibir en el guardrail 1 la escritura indirecta que el agente EJECUTA o ACTIVA (tareas programadas, scripts, endpoints o servicios que guarden, carga de `.sql`, harness que haga más que leer). La prohibición recae solo sobre lo que el agente ejecuta; no alcanza al código de aplicación ni al script `sql/` que entrega (RF-8). Ejecutar pruebas, build o la app del proyecto contra una BD cuenta como ejecutar: el agente corre los comandos de `AGENTS.md` › Comandos solo si no pueden escribir en una BD; si un comando puede escribir en una BD (incluida una BD local o descartable), el agente no lo ejecuta y lo entrega a la persona, ni siquiera contra una BD local o descartable. Ante la duda de si un comando escribe en una BD, tampoco lo ejecuta y lo entrega a la persona. Decisión firme del usuario.
- RF-6: CUANDO haga falta un cambio de datos o de esquema, EL AGENTE DEBE describir o redactar el script con su rollback (según la skill `migracion-bd`) y entregarlo, y LA EJECUCIÓN LA HACE UNA PERSONA, incluso si la conversación pide que el agente lo corra.
- RF-6b: El archivo en `sql/` lo escribe SOLO `implementer`. CUANDO `planner`, `reviewer` o `coordinator` detecten que hace falta un script, EL SISTEMA DEBE hacer que lo describan y lo entreguen en el chat, para que lo escriba `implementer` o lo haga la persona; ninguno lo ejecuta. La instrucción vive solo en el guardrail 1, que todos los roles leen siempre (RF-10, RF-12), y se verifica ahí; no se añade en `planner.md` ni en `coordinator.md`. No se amplían permisos de escritura de ningún agente: `planner.md` (incluido su «Qué no hace») y su fila de `restricciones-de-rol.md` no cambian por este punto, ni el «Qué no hace» de `coordinator`.
- RF-7: CUANDO el agente consulte datos, DEBE usar el mecanismo de `AGENTS.md` › Comandos y NO DEBE pasar credenciales a comandos, archivos ni al chat.
- RF-8: EL SISTEMA DEBE aclarar en el guardrail 1 que la regla es sobre lo que el agente ejecuta, no sobre el código de la aplicación ni el script `sql/` que entrega, que sí puede escribir (altas, bitácoras, logs) y se programa con normalidad; lo que el agente no hace es ejecutarlos ni activarlos (RF-5). Esto incluye pruebas, build o la app que escriban en una BD: el agente no los corre y los entrega a la persona (RF-5), tampoco contra una BD local o descartable; ante la duda de si escriben, tampoco los corre. Decisión firme del usuario.
- RF-9: EL SISTEMA NO DEBE mencionar en ningún texto del template motores, frameworks o productos concretos de un proyecto (ver definición). Verificación: búsqueda en `template/` de una lista cerrada de nombres de motores de BD, frameworks y productos de datos; la spec fija la lista mínima: mysql, mariadb, postgres, postgresql, oracle, sqlserver, sqlite, mongodb, redis, hibernate, jpa, laravel, django, spring, eloquent, y el plan puede ampliarla; cero coincidencias. Se excluyen las herramientas de agente (Cursor, Claude Code, etc.) y los ejemplos de archivos de build. Nombrar sentencias SQL o `sql/` está permitido.
- RF-10: EL SISTEMA DEBE indicar en `AGENTS.md` › «Qué lee cada rol», con una fila o nota que cubra a todos los roles, que todos leen siempre «Guardrails». El «tope de lecturas» existe solo en `sdd-implement.md` (tope de 3 secciones de ampliación); «Guardrails» queda fuera de ese tope y de la lista cerrada de secciones de `reviewer.md` (que RF-12 amplía con «Guardrails»): no cuenta contra ellos. No se crea ni se modifica ningún otro tope.
- RF-11: EL SISTEMA DEBE hacer que la línea «Consulta de datos de solo lectura» de `AGENTS.md` › Comandos remita a «Guardrails», regla 1, sin repetir lo prohibido.
- RF-12: EL SISTEMA DEBE hacer que cada uno de los 4 agentes de `template/.agents/agents/*.md` diga en su «Qué lee» (para `planner`, que tiene una línea «Qué lee» por documento, en cada una o en una línea común previa) que lee siempre `AGENTS.md` › «Guardrails» y los cumple sin excepción, aunque declare `omitClaudeMd`. En `planner.md` el cambio es solo esa remisión en «Qué lee»; su «Qué no hace» no se toca.
- RF-13: EL SISTEMA DEBE retirar la redacción propia de la prohibición y dejar solo el enlace o la remisión a «Guardrails», sin frases de contexto y sin copiar la regla, en esta lista fija de líneas o frases, en `template/` y en la raíz del clon (los números son de la lectura actual; si difieren, manda el texto citado):
  - `migracion-bd/SKILL.md`:13: solo la frase «El agente **genera** DDL/DML en `sql/` y **nunca lo ejecuta**. Solo consultas de solo lectura.»; se reemplaza por la remisión. Se conservan las reglas de redacción del SQL entregado (`DROP`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE` del script, respaldo, encabezado, rollback) y el resto de la explicación de cómo generar el SQL.
  - `migracion-bd/SKILL.md`, otras cuatro líneas revisadas (decisión de la spec):
    - :3 (descripción del frontmatter): se retira la frase «Los agentes generan el SQL, nunca lo ejecutan.» porque repite la prohibición; se conservan el resto de la descripción y los disparadores. Si hace falta, queda una remisión breve a «Guardrails».
    - :8 («ellos fijan **que** el SQL se genera y no se ejecuta; esta skill fija **cómo se escribe**»): se reescribe sin «y no se ejecuta», con la remisión a «Guardrails» para el qué y conservando «esta skill fija **cómo se escribe**».
    - :58 (fila de `implementer`: «no los ejecuta»): se retira «no los ejecuta» y se deja «Escribe los scripts con esta skill; los reporta como "pendiente de ejecutar"». «Pendiente de ejecutar» describe el procedimiento y se conserva.
    - :63 (anti-patrón «Ejecutar SQL de escritura desde un agente»): repite la prohibición en una lista de «No hacer»; se sustituye por la remisión a «Guardrails». Se conservan los otros dos anti-patrones (SQL suelto fuera de `sql/`; editar un script ya ejecutado).
    - Se dejan como están, por quedar excluidas (RF-13, redacción del SQL entregado o procedimiento): el encabezado «EJECUCIÓN MANUAL. Un agente no ejecuta este script.» (:27), «Lo ejecuta el usuario, y `05_avance.md` › "Estado BD" registra…» (:14) y la fila de `reviewer` sobre la consulta «Verificar» de solo lectura (:59).
  - `sdd/SKILL.md`:127 (línea de Datos): se retira «los agentes solo ejecutan consultas de solo lectura. DDL/DML se genera… y lo ejecuta el usuario» y se conservan dos enlaces: a «Guardrails» y «Cómo se escribe: `.agents/skills/migracion-bd/SKILL.md`» (ambos son enlaces, no frases de contexto).
  - `sdd/SKILL.md`:136 (anti-patrón): «SQL de escritura ejecutado por un agente, o SQL suelto fuera de `sql/`» se reduce a la remisión.
  - `implementer.md`:3 (descripción: «Genera SQL pero nunca lo ejecuta») y :13 («No ejecutas SQL»): se reemplazan por la remisión.
  - `reviewer.md`:21 (en «Qué no hace»: «No ejecutas escrituras de datos ni cambios de esquema»): se reemplaza por la remisión (aclaración confirmada por el usuario).
  - `sdd-implement.md`:12 (en «Qué no hace»: «No ejecuta SQL»): se reemplaza por la remisión.
  - `sdd-constitucion.md`:25 (principio «5. Datos y seguridad (solo lectura para agentes, `sql/` con rollback, sesión/permisos, sin secretos)»): queda con texto mínimo, solo el enlace o la remisión a «Guardrails».
  - `sdd-init.md`:38 (tema de pregunta «cómo se consulta la base de datos en solo lectura (o si no aplica)»): se ajusta a «cómo se consulta la base de datos (ver Guardrails)» sin repetir la regla.
  - En `restricciones-de-rol.md`, las filas que remiten (RF-14).
  Las descripciones de `planner` y `reviewer` se ajustan solo si repiten la prohibición; en `planner.md` solo cambia la remisión en «Qué lee» (RF-12). Verificación: esa lista ya sin la redacción propia (leída línea por línea o frase por frase, no por grep de sentencias); las reglas de redacción del SQL entregado quedan excluidas.
- RF-14: EL SISTEMA DEBE añadir a `restricciones-de-rol.md` una fila «Ningún agente escribe en la base de datos», con mecanismo «instrucción» y garantía «NO garantizada», ajustar las filas existentes que repiten la regla (`implementer` «no ejecuta SQL»; y la de `planner` solo en cuanto a la remisión de su consulta de solo lectura, sin ampliar permisos de escritura) para que remitan a «Guardrails» sin repetir la regla, y actualizar el contador «Ocho filas» a «Nueve filas».
- RF-15: EL SISTEMA DEBE recomendar en `restricciones-de-rol.md`, como sugerencia y no como garantía del kit, un usuario de BD con solo permisos de lectura para las consultas de los agentes.
- RF-16: CUANDO se publique el cambio, EL SISTEMA DEBE entregar la versión 0.6.0 del kit, única para los tres cambios paralelos (0008, 0009 y 0010). Orden de fusión: 0008, luego 0010, luego 0009. El manifiesto 0.6.0 en `manifiestos`, `VersionAgentKitSdd` y la entrada de `ChangelogAgentKitSdd.md` se crean una sola vez, al final, tras cerrar 0009; cada spec aporta sus líneas a esa entrada (las de 0008 son las de RF-17). Ventana conocida: entre la fusión de 0008 y el cierre de 0009, la evaluación E20 (versión y manifiesto coherentes) puede no pasar; 0008 se valida sin el manifiesto 0.6.0 y E20 se verifica una sola vez, al crear el manifiesto. Decisión firme del usuario: se acepta la ventana.
- RF-17: EL SISTEMA DEBE indicar solo en el changelog (`ACTUALIZAR.md` y la constitución del kit no cambian) que `AGENTS.md` es grupo B (en proyectos existentes la sección llega como faltante y se incorpora a mano), junto a la nota «Qué lee cada rol» de 0.5.0, y que los agentes (grupo A) salen KIT-VIEJO o PERSONALIZADO según cada proyecto.
- RF-18: EL SISTEMA DEBE registrar la decisión en un ADR nuevo.
- RF-19: EL SISTEMA DEBE aplicar también a la raíz del clon (`AGENTS.md` y `.agents/`) la sección «Guardrails» y las remisiones de RF-10 a RF-14, con el mismo texto que el template salvo lo propio del clon, para que el proyecto del kit cumpla su propia regla.

## Requisitos no funcionales
- Fuente única: la regla vive una sola vez por árbol (template y raíz del clon); el resto remite (ADR 009 y constitución). Verificable: ninguna de las líneas de la lista fija de RF-13 conserva redacción propia, fuera de `AGENTS.md` › Guardrails.
- Genérico: sin tecnologías de proyecto concreto en el template.
- UTF-8 sin BOM, en español, con el estilo del kit; la redacción del issue es propuesta y se ajusta.
- Compatible con `omitClaudeMd`: la regla no depende de que `AGENTS.md` se inyecte.

## Casos límite
- Un agente fuera del flujo `sdd` recibe una petición de «arreglar un dato»: aplica la regla; entrega script, no ejecuta.
- La conversación pide explícitamente ejecutar el cambio: se sigue entregando el script.
- El proyecto no define consulta de datos (Comandos dice «No aplica»): el agente no consulta.
- Un harness o servicio de prueba que escribe al arrancar: queda prohibido como escritura indirecta.
- Proyecto existente que personalizó `AGENTS.md` o los agentes: la actualización no pisa; se fusiona a mano.
- Entorno local descartable: la regla aplica igual (cualquier entorno).
- `SET` de sesión, CTE u otra sentencia no listada como lectura: se trata como prohibida.
- `planner`, `reviewer` o `coordinator` detectan que falta un script `sql/`: lo describen y lo entregan en el chat; lo escribe `implementer` o la persona; ninguno lo ejecuta.
- El script `sql/` o el código de aplicación que entrega el agente escribe datos: no viola la regla, porque no lo ejecuta ni lo activa.
- Pruebas, build o app cuyo comando puede escribir en una BD, incluida una local o descartable: el agente no lo ejecuta y lo entrega a la persona (RF-5; decisión firme).
- `SELECT ... INTO`, `FOR UPDATE`, funciones con efectos o locks: fuera de la lista cerrada, prohibidos.
- Anotado sin cambio de alcance: al pasar de ocho a nueve filas, el contador de `restricciones-de-rol.md` se verifica contra la tabla real (RF-14). Y en la raíz del clon, `AGENTS.md` › Comandos puede decir «No aplica» para la consulta de datos: entonces el agente no consulta (caso ya cubierto arriba).

## Fuera de alcance
- Garantía técnica de solo lectura (permisos de BD, sandbox): el kit solo la recomienda.
- Otros guardrails distintos al de BD.
- Cambios en `ACTUALIZAR.md` y en la constitución del kit (la nota de fusión manual va solo en el changelog).
- Ampliar permisos de escritura de `planner`, `reviewer` o `coordinator`, o tocar el «Qué no hace» de `planner` y `coordinator`.
- Cambiar el flujo de `migracion-bd` o la forma de ejecutar scripts por una persona.
- Edición automática de `AGENTS.md` en proyectos existentes.
- Código de la aplicación del proyecto.

## Criterios de finalización
- La regla aparece una sola vez por árbol, en `AGENTS.md` › Guardrails (template y raíz del clon), y las demás piezas remiten a ella.
- Ningún texto del template menciona motores, frameworks o productos concretos (SQL y `sql/` permitidos).
- Los 4 agentes nombran «Guardrails» en su lectura.
- `restricciones-de-rol.md` refleja la regla y su garantía real.
- Versión 0.6.0 (manifiesto, `VersionAgentKitSdd` y changelog creados una vez, tras cerrar 0009) con la nota de fusión manual de 0008.
- ADR nuevo registrado; spec con el ancho de 4 dígitos.
- Verificado antes de cerrar (revisión de la lista fija de RF-13 en `template/` y en la raíz del clon, y búsqueda en `template/` de la lista cerrada de motores, frameworks o productos de datos de RF-9).

## Dudas abiertas
Ninguna. Resueltas por el usuario:
- Comandos de `AGENTS.md` › Comandos que pueden escribir en una BD (pruebas, build, app): el agente no los ejecuta, ni contra una BD local o descartable; los entrega a la persona (RF-5, RF-8, caso límite).
- Ventana de E20 (RF-16): se acepta; 0008 se valida sin manifiesto 0.6.0 y E20 se verifica una vez al crearlo, tras cerrar 0009.
