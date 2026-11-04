# 03 — Plan: Guardrails globales, la base de datos solo se lee
Spec: 01_spec.md (aprobada 2026-10-31 23:33)

## 1. Ubicación y capas
Todo es Markdown del kit, sin código. La regla vive una sola vez por árbol, en `AGENTS.md` › «Guardrails (todos los agentes, siempre)»: en `template/AGENTS.md` y en el `AGENTS.md` de la raíz del clon. Las demás piezas remiten (ADR 009).

Capas: instrucciones globales (`AGENTS.md`), agentes (`.agents/agents/`), skills (`.agents/skills/`), comandos (`.agents/commands/`). Los archivos de `.agents/` son iguales en `template/` y en la raíz; se edita `template/` y se replica byte a byte. El `AGENTS.md` de la raíz se edita aparte, con lo propio del clon.

Forma de remisión (única, para que no diverjan): `` `AGENTS.md` › Guardrails, regla 1 ``. Sin frases de contexto y sin copiar la regla. Dentro de `AGENTS.md` se escribe «Guardrails», regla 1.

## 2. Archivos que se crean o modifican
Capa en la columna «Capa». Todos existen; no se crea ninguno, salvo el ADR 0019 de F7 (incondicional, RF-18).

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/AGENTS.md` | Instrucciones globales | Sección «Guardrails» con la regla 1; fila de «Qué lee cada rol»; línea de Comandos remite | 1 a 8, 10, 11 |
| `AGENTS.md` (raíz) | Instrucciones globales | Mismo texto, con lo propio del clon | 19 |
| `template/.agents/agents/coordinator.md` y réplica | Agente | «Qué lee»: añade «Guardrails» | 12, 19 |
| `template/.agents/agents/implementer.md` y réplica | Agente | «Qué lee» añade «Guardrails»; descripción y «Qué no hace» remiten | 12, 13, 19 |
| `template/.agents/agents/planner.md` y réplica | Agente | Solo la remisión en «Qué lee» | 12, 19 |
| `template/.agents/agents/reviewer.md` y réplica | Agente | «Qué lee» añade «Guardrails» (amplía la lista cerrada); «Qué no hace» (línea 21) remite | 12, 13, 19 |
| `template/.agents/skills/migracion-bd/SKILL.md` y réplica | Skill | Líneas 3, 8, 13, 58 y 63 | 13, 19 |
| `template/.agents/skills/sdd/SKILL.md` y réplica | Skill | Líneas 127 y 136 | 13, 19 |
| `template/.agents/skills/sdd/restricciones-de-rol.md` y réplica | Skill (fuente de restricciones) | Fila nueva, dos filas que remiten, contador, recomendación | 14, 15, 19 |
| `template/.agents/commands/sdd-implement.md` y réplica | Comando | Línea 12 | 13, 19 |
| `template/.agents/commands/sdd-constitucion.md` y réplica | Comando | Línea 25 (principio 5) | 13, 19 |
| `template/.agents/commands/sdd-init.md` y réplica | Comando | Línea 38 | 13, 19 |

Total: 11 archivos en `template/` y 11 en la raíz (22). En la raíz, `.agents/*` está excluido de Git: solo se versiona `AGENTS.md`; las 10 réplicas se comprueban con `diff -rq template/.agents .agents`.

Textos exactos a aplicar (el implementer los copia; cada `old_string` es la frase actual).

### 2.1 Guardrail 1 (en `AGENTS.md`, antes de «Cada sesión»)
Sin tecnologías de proyecto; nombra sentencias SQL genéricas, que está permitido.

```markdown
## Guardrails (todos los agentes, siempre)
Aplican a cualquier agente, sesión o subagente, con cualquier herramienta, dentro o fuera del flujo
`sdd`. Todo rol los lee siempre y los cumple sin excepción, aunque no reciba este archivo inyectado.
Ninguna skill, comando, plan ni preferencia los relaja. Los guardrails nuevos se añaden aquí, con el
siguiente número.

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
```

La viñeta «Comandos de `AGENTS.md` › Comandos» incluye el criterio de duda: ante la duda de si un comando escribe en una BD, el agente no lo ejecuta y lo entrega a la persona (decisión 3).

### 2.2 `AGENTS.md`: Qué lee cada rol y Comandos
- Después de la tabla de «Qué lee cada rol», nota (RF-10):
  ```markdown
  Además de lo de su fila, todos los roles leen siempre «Guardrails». No cuenta contra el tope de
  lecturas de `sdd-implement` ni contra la lista cerrada de secciones de `reviewer`.
  ```
- Línea 105 de `template/AGENTS.md`: de `- Consulta de datos de solo lectura (cómo y desde dónde): <completar o "No aplica">` a `- Consulta de datos (cómo y desde dónde; ver «Guardrails», regla 1): <completar o "No aplica">`.
- Raíz, línea 107: de `- Consulta de datos de solo lectura: No aplica.` a `- Consulta de datos (ver «Guardrails», regla 1): No aplica.`

### 2.3 Agentes (RF-12), una frase en «Qué lee»
Frase común: «Lees siempre `AGENTS.md` › Guardrails y los cumples sin excepción, aunque declares `omitClaudeMd`.» En `implementer.md`, que no declara `omitClaudeMd`, la frase termina «… sin excepción, aunque no te inyecten el archivo.»
- `coordinator.md` línea 10: tras «y lees las secciones "Git" y "Reglas duras"» se añade «y Guardrails» y se agrega la frase al final del párrafo, antes de la frase de las restricciones de rol.
- `planner.md`: la frase va como oración propia en la línea 10 de «Qué lee», fuera de la lista de cualquier documento (es común a los cinco). Cambio solo ese; «Qué no hace» y la línea 20 no se tocan.
- `reviewer.md` línea 10: la lista pasa a «solo "Comandos", "Stack y convenciones de código", "Git", "Reglas duras" y "Guardrails"», con la frase; «sin copiar la redacción» pasa a «de "Git", "Reglas duras" ni "Guardrails"». Línea 21 («Qué no hace»): de «No ejecutas escrituras de datos ni cambios de esquema.» a «Cumples `AGENTS.md` › Guardrails.»
- `implementer.md`: línea 3, de «Genera SQL pero nunca lo ejecuta.» a «Cumple `AGENTS.md` › Guardrails.»; línea 9, la lista pasa a «solo "Comandos", "Stack y convenciones de código" y "Guardrails"», con la frase; línea 13, de «No ejecutas SQL.» a «Cumples `AGENTS.md` › Guardrails.»

### 2.4 Skills y comandos (RF-13), forma exacta
- `migracion-bd/SKILL.md`:
  - :3: se retira «Los agentes generan el SQL, nunca lo ejecutan.» (sin reemplazo; el resto de la descripción y los disparadores se conservan).
  - :8: «Complementa a `sdd` y al implementer: ellos fijan **que** el SQL se genera y no se ejecuta; esta skill fija **cómo se escribe**.» pasa a «Complementa a `sdd` y al implementer: lo que un agente puede hacer con la base de datos está en `AGENTS.md` › Guardrails, regla 1; esta skill fija **cómo se escribe**.»
  - :13: de «El agente **genera** DDL/DML en `sql/` y **nunca lo ejecuta**. Solo consultas de solo lectura.» a «Qué puede hacer un agente con la base de datos: `AGENTS.md` › Guardrails, regla 1.» La línea :14 («Lo ejecuta el usuario, y `05_avance.md` › "Estado BD"…») no cambia.
  - :58: «Escribe los scripts con esta skill; no los ejecuta; los reporta como "pendiente de ejecutar"» pasa a «Escribe los scripts con esta skill; los reporta como "pendiente de ejecutar"».
  - :63: «- Ejecutar SQL de escritura desde un agente.» pasa a «- Lo que un agente no hace con la base de datos: `AGENTS.md` › Guardrails, regla 1.» Los otros dos anti-patrones (SQL suelto fuera de `sql/`; editar un script ya ejecutado) se conservan.
- `sdd/SKILL.md`:
  - :127: «- Datos: los agentes solo ejecutan consultas… lo ejecuta el usuario. Cómo se escribe: …» pasa a «- Datos: `AGENTS.md` › Guardrails, regla 1. Cómo se escribe: `.agents/skills/migracion-bd/SKILL.md`.»
  - :136: «- SQL de escritura ejecutado por un agente, o SQL suelto fuera de `sql/`.» pasa a «- Incumplir un guardrail: `AGENTS.md` › Guardrails.»
- `reviewer.md` :21: ver 2.3 (remisión en «Qué no hace»; entra en la lista fija de RF-13 por decisión del usuario).
- `sdd-implement.md` :12: «No ejecuta SQL.» pasa a «Cumple `AGENTS.md` › Guardrails.»
- `sdd-constitucion.md` :25: «5. Datos y seguridad (solo lectura para agentes, `sql/` con rollback, sesión/permisos, sin secretos).» pasa a «5. Datos y seguridad: `AGENTS.md` › Guardrails.»
- `sdd-init.md` :38: «cómo se consulta la base de datos en solo lectura (o si no aplica)» pasa a «cómo se consulta la base de datos (ver Guardrails) (o si no aplica)». Si el salto de línea de la frase cambia, se conserva el ancho del párrafo.

### 2.5 `restricciones-de-rol.md` (RF-14, RF-15)
- Línea 54: «Ocho filas» pasa a «Nueve filas» (se comprueba contra la tabla real tras añadir la fila).
- Fila 4 (`implementer`): «…ni `sync`, y no ejecuta SQL» pasa a «…ni `sync`». La regla de BD queda en la fila nueva.
- Fila 8 (`planner` consulta datos): «`planner` consulta datos solo en modo lectura, con el comando de `AGENTS.md` › Comandos» pasa a «`planner` consulta datos con el comando de `AGENTS.md` › Comandos, según `AGENTS.md` › Guardrails, regla 1». Mecanismo y garantía no cambian; no se amplían permisos.
- Fila nueva al final (fila 9, para no renumerar las anteriores):
  `| Ningún agente escribe en la base de datos (`AGENTS.md` › Guardrails, regla 1) | instrucción | NO garantizada; instrucción. Sin permisos de BD que la impongan |`
- Sección nueva, tras la tabla y antes de «Fragmento opcional `permissions.deny`»:
  ```markdown
  ## Recomendación: usuario de BD de solo lectura

  Sugerencia, no garantía del kit. La regla 1 de «Guardrails» es una instrucción. Como refuerzo
  externo, las consultas de los agentes conviene que usen un usuario de BD con permisos solo de lectura.
  ```

## 3. Base de datos
No aplica. No hay scripts `sql/` ni datos.

| Script `sql/` | Tipo (DDL/seed) | Rollback |
|---|---|---|
| — | — | — |

## 4. Seguridad
- La regla se entrega como instrucción, no como garantía técnica, y así se declara en `restricciones-de-rol.md` (fila nueva con «NO garantizada»). Se recomienda un usuario de BD de solo lectura como refuerzo externo (RF-15).
- Sin credenciales en los textos nuevos. Los textos no nombran motores, frameworks ni productos concretos de un proyecto (RF-9).
- Doble envío e idempotencia: no aplica. Las ediciones se aplican de forma idempotente por `old_string`: si la frase ya cambió, el Edit falla sin efecto.
- Compatible con `omitClaudeMd`: la regla llega a los tres roles porque cada agente nombra «Guardrails» en su propio «Qué lee».

## 5. Liberación / piloto
No aplica piloto. Versión 0.6.0, única para 0008, 0009 y 0010. Orden de fusión: 0008, 0010, 0009.

No se hace en 0008 (RF-16):
- No se crea el manifiesto `manifiestos/0.6.0.md`.
- No se cambia `VersionAgentKitSdd`.
- No se crea la entrada 0.6.0 de `ChangelogAgentKitSdd.md`.
- No se toca `ACTUALIZAR.md`, la constitución del kit, `INSTALAR.md` ni `README.md`.
- No se verifica E20: se acepta la ventana entre la fusión de 0008 y el cierre de 0009 (decisión firme del usuario). 0008 se valida sin manifiesto 0.6.0, y E20 se verifica una sola vez al crear el manifiesto.

Esas tres piezas se crean una vez, al final, tras cerrar 0009. 0008 solo entrega sus líneas para esa entrada (RF-17):
```markdown
- `AGENTS.md` incorpora la sección «Guardrails (todos los agentes, siempre)», con la regla 1: la base de
  datos solo se lee. Los agentes, las skills `sdd` y `migracion-bd`, los comandos de implementación,
  constitución e inicio y `restricciones-de-rol.md` remiten a ella; esta suma la fila correspondiente
  (instrucción, no garantía) y la recomendación de un usuario de BD de solo lectura.
- `AGENTS.md` es grupo B: en un proyecto existente la sección llega como faltante y se incorpora a
  mano, junto a la nota «Qué lee cada rol» de 0.5.0. Los agentes (grupo A) salen KIT-VIEJO o
  PERSONALIZADO según cada proyecto.
```
Las líneas se dejan en el `05_avance.md` de 0008 (confirmado por el usuario) para que quien cierre 0009 las pegue; este plan no las escribe en `ChangelogAgentKitSdd.md`.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| 1. La regla vive en una sección global de `AGENTS.md`, con numeración abierta; todos los roles la leen siempre | Repartirla por rol y skill (estado actual); una constitución por proyecto; un archivo aparte de guardrails | Una fuente única (ADR 009), visible fuera del flujo `sdd`, con sitio para más guardrails. Un archivo aparte obligaría a cada agente a enlazar otro archivo y a proyectos existentes a incorporar dos piezas. ADR 0019 (ver 6.1) |
| 2. Una sola forma de remisión: `` `AGENTS.md` › Guardrails, regla 1 ``, sin frases de contexto | Resumir la regla en cada pieza | Un resumen diverge con el tiempo y rompe la verificación de RF-13 |
| 3. Criterio de duda sobre comandos de `Comandos`: si no se sabe si un comando escribe en una BD, no se ejecuta y se entrega a la persona. Va escrito dentro del guardrail 1 | Dejar al agente juzgar caso a caso; permitir lo «probablemente inocuo» | Igual que la precedencia del SQL (lo no demostrado como lectura se trata como prohibido). Una BD local o descartable tampoco lo relaja (RF-5, decisión firme) |
| 4. Al reducir `sdd/SKILL.md`:136 a una remisión, el anti-patrón «SQL suelto fuera de `sql/`» deja de estar ahí y sigue vivo en `migracion-bd/SKILL.md`:63. Es intencional | Mantener el anti-patrón duplicado en `sdd/SKILL.md` | Es una regla de cómo se escribe el script, no de la prohibición. Vive en la skill que fija la escritura (ADR 009). RF-13 pide dejar solo la remisión |
| 5. La fila nueva de `restricciones-de-rol.md` va al final (fila 9) | Insertarla junto a las de `implementer` | No renumera las filas 2 y 3, que otras piezas citan como «filas 2 y 3» |
| 6. La nota de «Guardrails» fuera de topes va en `AGENTS.md` › «Qué lee cada rol»; no se toca el texto de `sdd-implement.md` (tope de 3) ni la lista de `reviewer` más allá de añadir «Guardrails» | Repetir la excepción en `sdd-implement.md` | La lista de `reviewer.md` es la que RF-12 amplía; el tope vive en un solo sitio y no se modifica (RF-10) |
| 7. Versión 0.6.0, manifiesto, `VersionAgentKitSdd` y changelog se crean una vez, tras 0009; 0008 aporta sus líneas | Crear 0.6.0 al cerrar 0008 | Tres cambios paralelos comparten versión (RF-16). Ventana de E20 aceptada |
| 8. Las 10 réplicas de `.agents/` de la raíz se copian de `template/` (mismo texto); el `AGENTS.md` de la raíz se edita aparte | Reescribir cada réplica a mano | Evita divergencias; E19 las detecta |

### 6.1 ADR 0019 (se crea en F7, incondicional)
- Título propuesto: «Regla global de la base de datos solo lectura en AGENTS.md › Guardrails».
- Marca: decisión con alternativas reales y efecto duradero (afecta a todo proyecto que use el kit y a cada agente nuevo).
- Alternativas: (a) sección global en `AGENTS.md` (elegida); (b) repartirla por rol y skill; (c) archivo aparte de guardrails; (d) solo en la constitución de cada proyecto; (e) garantía técnica por permisos (fuera de alcance).
- Consecuencias: una fuente por árbol; los agentes deben nombrar «Guardrails» en su lectura; los proyectos existentes incorporan la sección a mano; la regla sigue siendo instrucción, no garantía.
- Número libre hoy: 0019 (0009 y 0010 podrían reclamarlo; se recalcula al crearlo por valor numérico). RF-18 exige el ADR; se crea en F7 sin condición.

## 7. Fases y estimado
Estimado en minutos (solo Markdown). Total: 150 minutos.

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Diagnóstico / preparación: releer el estado de 0009 y 0010 y los archivos comunes; confirmar con `grep` las líneas de RF-13 y un `diff -rq template/.agents .agents` previo | 13, 19 | 10 |
| F1 | `template/AGENTS.md`: sección «Guardrails», nota de «Qué lee cada rol», línea de Comandos | 1 a 8, 10, 11 | 25 |
| F2 | Los 4 agentes de `template/.agents/agents/` (incluye `reviewer.md`:21) | 12, 13 | 20 |
| F3 | Skills y comandos: `migracion-bd`, `sdd`, `sdd-implement`, `sdd-constitucion`, `sdd-init` | 13 | 25 |
| F4 | `restricciones-de-rol.md`: fila nueva, dos filas que remiten, contador, recomendación | 14, 15 | 15 |
| F5 | Raíz del clon: `AGENTS.md` de la raíz (sección y remisiones) y copia de los 10 archivos de `.agents/` desde `template/` | 19 | 15 |
| F6 | Verificación final (ver sección 8) | 3, 9, 13, 14, 19 | 25 |
| F7 | Entregar las líneas de changelog en `05_avance.md`; crear el ADR 0019 | 17, 18 | 15 |

Orden: F0, F1, F2, F3, F4, luego F5 (la raíz copia de `template/`, que debe estar terminado), F6 y F7. Cada fase deja un estado revisable. Los commits los decide el usuario (un commit por módulo, según `AGENTS.md` › Git).

## 8. Criterios de aceptación
- La regla aparece una sola vez por árbol, en `AGENTS.md` › Guardrails (template y raíz), con numeración abierta. Verificación: lectura de ambas secciones.
- Cada línea de la lista fija de RF-13, leída línea por línea y no con `grep` de sentencias, queda solo con la remisión, en `template/` y en la raíz. Las reglas de redacción del SQL entregado (`DROP`, `INSERT`, etc. del script y su respaldo) quedan fuera de la verificación. Las líneas excluidas por la spec se dejan como están.
- Búsqueda en `template/` de la lista de RF-9 con cero coincidencias. Lista cerrada de esta verificación (amplía la mínima de la spec): mysql, mariadb, postgres, postgresql, oracle, sqlserver, mssql, sql server, db2, cassandra, dynamodb, firebird, sqlite, mongodb, redis, hibernate, jpa, laravel, django, spring, eloquent, sequelize, typeorm, prisma, sqlalchemy, rails, symfony, dapper, mybatis, entity framework, flask. Con `grep -riw`, para que «rails» no coincida con «guardrails». Hoy da cero.
- Los 4 agentes nombran «Guardrails» en su «Qué lee». `planner.md` solo cambia ahí; su «Qué no hace» y el de `coordinator` no cambian.
- `restricciones-de-rol.md` tiene la fila «Ningún agente escribe en la base de datos» con «instrucción» y «NO garantizada», dice «Nueve filas» y la tabla tiene nueve filas; incluye la recomendación de usuario de BD de solo lectura; `planner` no gana permisos.
- `diff -rq template/.agents .agents` admite solo las diferencias `napkin.md` y `skills/caveman` (E19); cualquier otra es una réplica rota. Alternativa equivalente: comparar solo los 10 archivos replicados.
- `reviewer.md` › «Qué no hace» ya no tiene redacción propia de la prohibición (línea 21 con la remisión), en `template/` y en la raíz.
- UTF-8 sin BOM en los archivos tocados (`file -i`; `grep -l $'^\xEF\xBB\xBF'` vacío).
- No se tocó `manifiestos/`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `ACTUALIZAR.md` ni la constitución del kit.
- Riesgo de concurrencia controlado: antes de cada edición se relee el archivo, y los `old_string` de los Edit son mínimos. Orden de fusión 0008, 0010, 0009. Si 0010 o 0009 ya movieron una línea, manda el texto citado, no el número.
- Las líneas de changelog de 0008 quedan entregadas para la entrada 0.6.0 (RF-17) y el ADR 0019 queda creado (RF-18).
