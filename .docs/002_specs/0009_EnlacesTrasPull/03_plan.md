# 03 — Plan: Enlaces tras un pull
Spec: 01_spec.md (aprobada 2026-10-31 23:33)
Plan: aprobado por el usuario 2026-11-01 13:56

## 1. Ubicación y capas
Kit de Markdown, sin código (ADR 004). Capas (constitución §3, ADR 001/002):
- Producto: `template/` (comando, `sync-agents.md`, `validar-estructura.md`, `AGENTS.md`, README del layout, tabla de la skill `sdd`).
- Réplica en el clon: `.agents/` copia de `template/.agents/` (local, excluida de Git; E19 exige que solo `napkin.md` difiera).
- Entorno de mantenimiento versionado: `AGENTS.md` raíz.
- Manual del kit (no viaja al proyecto): `ACTUALIZAR.md`, `INSTALAR.md`.

Reparto de reglas, cada una en una fuente (constitución §3):
- `sync-agents.md`: estados, tabla de enlaces, mecanismo de creación, reglas de los pasos 7 y 8, **formato de «visto»** y **chequeo de arranque** (comparación y texto del aviso). Es la fuente porque lo leen el arranque y `/sdd-enlaces`.
- `sdd-enlaces.md`: procedimiento de escritura, orden de confirmaciones, re-verificación. Remite a `sync-agents.md`, no copia.
- `AGENTS.md` paso 7: solo remite a `sync-agents.md` › «Chequeo de arranque», sin repetir qué se compara.
- `sdd-enlaces.md` no copia la regla de `git ls-files -ci`: remite a la skill git-commit / `sync-agents.md`.
- Pasos mínimos (aviso, `/sdd-enlaces`): fuente = sección de `ACTUALIZAR.md`; el aviso los resume.

## 2. Archivos que se crean o modifican
Regla de edición: se edita en `template/` y se replica el mismo cambio en el clon (`.agents/`). Edit mínimo; releer cada archivo antes de editar (0008 y 0010 pueden tocar `AGENTS.md`).

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/commands/sdd-enlaces.md` (nuevo) | Producto | `/sdd-enlaces`: sesión principal, sin subagente; ver §6.2 | 3–9, 12, 14 |
| `.agents/commands/sdd-enlaces.md` (nuevo) | Réplica | Copia idéntica | 14 |
| `template/.agents/sync-agents.md` | Producto | Pasos 1–2 = lectura del arranque; 3–8 los ejecuta `/sdd-enlaces`; añade «Estado «visto»» y «Chequeo de arranque» (se omite si falta `.agents/.kit-version` y existe `template/`); disparador «al agregar pieza» apunta a `/sdd-enlaces`; se conserva la numeración 1–8 | 1, 2, 3, 5, 10, 11 |
| `.agents/sync-agents.md` | Réplica | Copia | 10 |
| `template/.agents/validar-estructura.md` | Producto | E1 se mide sobre las herramientas declaradas en «visto» (las no declaradas = AVISO o fuera de E1; excepciones y copias también AVISO); E19: ignora `local/`; «Cómo responder» 4: las correcciones de enlaces y réplicas se aplican con `/sdd-enlaces`; «E1 a E20» -> «E1 a E21» (colisión con 0010) | 11 |
| `.agents/validar-estructura.md` | Réplica | Copia | 11 |
| `template/.agents/commands/sdd-estructura.md` | Producto | Quita «Cuando el usuario confirme una corrección, aplícala una por una»; solo propone y remite | 11 |
| `.agents/commands/sdd-estructura.md` | Réplica | Copia | 11 |
| `template/AGENTS.md` | Producto | Paso 7 de «Cada sesión» reescrito (solo lectura, aviso combinado); `/sdd-enlaces` en «Roles y comandos». Paso 8 (prefijos) sin cambios | 1, 2, 13, 14 |
| `AGENTS.md` (raíz) | Entorno versionado | Mismo cambio, conservando las diferencias propias de la raíz (paso 0, etc.) | 1, 2, 13, 14 |
| `template/.agents/README.md` y `.agents/README.md` | Producto + réplica | Árbol con `local/kit-visto`; «Agregar skill/agente/comando»: las citas a «paso 8» (líneas 59 en «Agregar skill», 66 y 75 en «Agregar agente/comando»; verificadas) pasan a «sugerir `/sdd-enlaces`» (sin mencionar «pasos 3-4»); «Validar la estructura» menciona `/sdd-enlaces` | 10, 14 |
| `template/.agents/skills/sdd/SKILL.md` y réplica | Producto + réplica | Fila `Enlaces | /sdd-enlaces | sesión principal | —` tras `/sdd-estructura`; «E1 a E20» -> «E1 a E21» donde aparezca | 14 |
| `template/.agents/agents/coordinator.md` y réplica | Producto + réplica | Fila de ruteo «Pide crear o corregir enlaces…» -> `sdd-enlaces.md`; frase «lo ejecuta la sesión principal»; «E1–E20» -> «E1–E21» | 14 |
| `template/.agents/agents/reviewer.md` y réplica | Producto + réplica | «E1–E20» -> «E1–E21» (línea 21, «Qué no hace») | 14 |
| `template/.agents/commands/sdd.md` y réplica | Producto + réplica | Añade `sdd-enlaces` a la lista de comandos; «E1 a E20» -> «E1 a E21» donde aparezca | 14 |
| `template/.agents/commands/sdd-init.md` y réplica | Producto + réplica | Paso 4.3: «sugiere correr `/sdd-enlaces`» (no escribe «visto»); paso 4.2/4.3: «E1 a E20» -> «E1 a E21» | 14 |
| `INSTALAR.md` (línea 112) | Manual raíz | «Propone `/sdd-enlaces`» | 14 |
| `ACTUALIZAR.md` | Manual raíz | Sección nueva «Qué hacer después de un pull que actualiza el kit» (+ entrada del índice); `## Enlaces`: verifica solo lectura y remite a `/sdd-enlaces`; `## Huérfanos`: `.agents/local/` no es huérfano. La sección RF-13 es la **fuente de los pasos mínimos** (el aviso los resume) y consigna la nota del grupo B: el paso 7 vive en `AGENTS.md`, que una actualización no pisa, así que los proyectos existentes lo incorporan a mano | 13 |
| `template/.gitignore`, `.gitignore` | — | **Sin cambio**: ya traen `.agents/local/` (línea 4 en ambos) | 3 |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.6.0.md` | Raíz | Solo el bump final (F6): 0009 lo hace una vez, tras 0008 y 0010; el changelog consigna la nota del grupo B (el paso 7 de `AGENTS.md` se incorpora a mano en proyectos existentes); los manifiestos usan el algoritmo de huella de `ACTUALIZAR.md` (E20) | 8 (CF-8) |

No se crea `05_avance.md`, `sql/` ni código. No se toca `template/` para `ACTUALIZAR.md` (decisión 14).

## 3. Base de datos
No aplica. Sin scripts `sql/`.

## 4. Seguridad
- Nada se borra ni sobrescribe (constitución §5): solo se reemplaza o retira un **enlace**, con confirmación. Un directorio o archivo real (OCUPADO, copia) nunca se borra; `git rm --cached` conserva el archivo en disco.
- `/sdd-enlaces` ejecuta solo el comando mostrado y confirmado. Se validan las rutas contra la tabla de `sync-agents.md` (sin rutas libres) antes de ejecutar.
- Sin secretos: «visto» solo guarda versión, nombres de herramientas y rutas relativas.
- El arranque no escribe nada. Doble ejecución: gana la última escritura (spec).
- Entradas de «visto» corruptas: se tratan como inexistentes; no se ejecuta contenido de «visto», solo se compara texto.

## 5. Liberación / piloto
Sin feature flag. Sale en 0.6.0 junto con 0008 y 0010. Piloto: ensayo en un proyecto temporal (`mktemp -d`) más el propio clon del kit (F5). Orden de fusión: 0008, 0010, 0009; 0009 hace el bump (F6) al final.

## 6. Decisiones

### 6.1 Los 10 menores del clarify final
| # | Decisión | Alternativa descartada | Por qué |
|---|---|---|---|
| 1 | RF-3 «no modifica archivos versionados» aplica solo a **escribir «visto»**. Editar `.gitignore` es una acción distinta y propia de `/sdd-enlaces`, con su confirmación, y ocurre **antes** de escribir «visto». | Prohibir toda edición de `.gitignore` | RF-3 ya exige proponer la regla si falta; las dos frases solo cuadran separando «registrar» de «preparar el ignore». En la práctica `.gitignore` ya trae la regla; solo cambia en proyectos con `.gitignore` personalizado |
| 2 | **Copia propia ≠ OCUPADO**: al crear una copia, `/sdd-enlaces` la anota en «visto» con la línea `copia: <ruta>`. Un destino real que **no** figura en `copia:` ni en `excepcion:` es OCUPADO. La re-verificación y el paso 7 reportan una ruta en `copia:` como `OK (copia)`; si la ruta pasó a ser enlace, la línea se descarta | Detectar copias por contenido (hash o comparación) | Comparar contenido es costoso y frágil; el registro es explícito y se pierde sin riesgo (sin «visto» la copia sale OCUPADO y la persona la reconoce como excepción) |
| 3 | Un OCUPADO nuevo y **no registrado** de una herramienta declarada cuenta como pendiente: entra al **aviso combinado**, aunque la versión y las piezas no cambien. **RF-1d se amplía** de «FALTA, DIFERENTE» a «FALTA, DIFERENTE u OCUPADO no registrado» (según RF-4 y RF-8); aprobado por el usuario, se anota en la spec en el cierre (F6), sin `/sdd-change`. Una vez registrado como excepción deja de contar | Ignorar OCUPADO en el arranque | Sería un fallo silencioso (sin enlace, sin aviso) y contradice RF-4 y RF-8, que ya tratan «no registrado» como pendiente |
| 4 | **`sync-agents.md` conserva la numeración 1–8**. Pasos 1–2 (estados, silencio) = lectura del paso 7 de arranque. Pasos 3–6 (sugerir, mecanismo, OCUPADO nunca se borra, preguntar herramientas) se redactan como lo que `/sdd-enlaces` muestra y pregunta; pasos 7 y 8 se redactan como reglas que `/sdd-enlaces` ejecuta tras confirmar. El disparador «cada vez que el agente crea o agrega una skill, agente o comando» se queda en `sync-agents.md` y dice «sugerir `/sdd-enlaces`». **El arranque ya no detecta** ignorados versionados ni réplicas huérfanas/faltantes: lo hace `/sdd-enlaces` y lo reportan E2/E3 en `/sdd-estructura`; el aviso no los menciona | Renumerar o partir el archivo; mantener los detectores en el arranque como aviso de solo lectura | Renumerar rompe 6+ referencias a «paso 8» (README x2, E2/E3 x2). Detectar réplicas al arranque añade una línea al aviso y rompe el tope de 2–3. El costo (perder detección automática) lo aceptó el usuario y se registra en ADR-B |
| 5 | La regla `.agents/local/` **ya está** en `template/.gitignore` y en `.gitignore` de la raíz (línea 4; `git check-ignore -v .agents/local/kit-visto` la confirma). `check-ignore` queda como control para proyectos cuyo `.gitignore` no la tiene | Añadir la regla otra vez o tocar los `.gitignore` | Ya existe; duplicarla viola «una sola fuente». Los `.gitignore` no cambian en este feature |
| 6 | N = **5**: las 5 primeras piezas entre nuevas y retiradas (nuevas primero), luego «y X más». Presupuesto del aviso: **3 líneas** (1: versión y qué trae; 2: piezas; 3: enlaces pendientes y llamada a la acción con los pasos mínimos). El aviso **no lee changelog**: el proyecto no incluye `ChangelogAgentKitSdd.md` y no se puede suponer; siempre lista piezas y no las atribuye a un feature | Leer un changelog si existe; N = 10 | Sin changelog garantizado, la lista de piezas es la única fuente; 5 nombres caben en una línea de ~160 caracteres y 10 romperían el tope |
| 7 | Cero herramientas declaradas **no es éxito**: no se crean enlaces, **no se escribe «visto»**, se informa y el aviso vuelve en la siguiente sesión | Aceptar «ninguna» como declaración válida | Confirmado por el usuario; RF-4 mide éxito sobre herramientas declaradas. Consecuencia: quien no usa ninguna herramienta de la tabla verá el aviso cada sesión |
| 8 | **Trazabilidad CF-8**: tabla RF -> archivo -> verificación en §6.3, y la verificación se anota por RF en `05_avance.md` | Solo criterios de aceptación | CF-8 exige comprobar «ningún script nuevo» y que existan réplicas; sin tabla no hay forma de auditar los 14 RF |
| 9 | **Sin repositorio Git**: `/sdd-enlaces` detecta con `git rev-parse --is-inside-work-tree`; si falla, omite `check-ignore` y la edición de `.gitignore`, avisa que «visto» no queda protegido por Git y pide confirmación **antes de crear `.agents/local/` y `kit-visto`**. Los pasos 7 (`git rm --cached`) no aplican | Negarse a funcionar sin Git | Los enlaces no dependen de Git; solo «visto» lo hace |
| 10 | **Confirmaciones**: una por tipo de acción, nunca por enlace, con comandos exactos visibles. Orden fijo (de menos a más riesgo; el estado propio al final): (1) crear enlaces y réplicas faltantes; (2) reemplazar DIFERENTE (destino actual y esperado); (3) retirar enlaces colgantes (RF-9); (4) pregunta OCUPADO «¿es intencional?» (no ejecuta nada); (5) mover réplicas huérfanas a `.agents/`; (6) `git rm --cached` de ignorados versionados; (7) crear `.agents/local/` si no existe; (8) editar `.gitignore` si `check-ignore` falla; (9) escribir «visto». Un «no» en un tipo omite solo ese tipo, salvo (7)–(9): sin ellos no hay «visto» y el aviso vuelve | Una confirmación global; una por enlace | Global oculta acciones destructivas; por enlace cansa. Mover huérfanas y `git rm --cached` tocan contenido real, por eso van tras lo no destructivo. El estado propio va al final porque RF-4 lo marca solo tras la re-verificación OK |

### 6.2 Otras decisiones de diseño
| # | Decisión | Alternativa descartada | Por qué |
|---|---|---|---|
| 11 | RF-1b literal (opción A, aprobada): falta `kit-visto` = «todo nuevo». Si falta `.agents/.kit-version` **y** existe `template/` (clon del kit), el chequeo de arranque se omite: sin aviso. Sin `template/`, la ausencia de `.kit-version` no se trata aparte: el proyecto avisa como «todo nuevo»; `/sdd-enlaces` crea los enlaces pero, sin versión legible, no escribe «visto» e informa que falta `.agents/.kit-version` (ver `ACTUALIZAR.md` › Marcador de versión). No existe `version: ninguna` | Excepción `version: ninguna` con `/sdd-change` (B) | Respeta la spec sin modificarla; `template/` presente es una señal local y fiable de clon del kit |
| 12 | Las reglas del chequeo de arranque y el formato de «visto» viven en `sync-agents.md`; `AGENTS.md` paso 7 solo remite | Poner la lógica en `AGENTS.md` | Evita inflar `AGENTS.md` (zona de conflicto con 0008/0010) y mantiene una fuente |
| 13 | `/sdd-enlaces` lo ejecuta la **sesión principal, sin subagente**, y no pasa por planner/implementer/reviewer; no toca `05_avance.md`. No delega al flujo `sdd` (no es fase). Sí delega: reglas, estados y mecanismo a `sync-agents.md`; formato a `sync-agents.md`; mover skills a `.agents/README.md` › «Agregar…» | Subagente `implementer` | Pregunta al usuario varias veces y ejecuta en su máquina; un subagente no puede preguntar. Igual que `/sdd-estructura` (`restricciones-de-rol.md` no cambia) |
| 14 | `ACTUALIZAR.md` **no se replica en `template/`**; el aviso del paso 7 trae los pasos mínimos en una línea y remite al kit | Replicarlo | RF-13 lo permite y el proyecto no usa la guía de mantenimiento; replicarlo contradice «fuente única» y añadiría un archivo que 0008/0010 no esperan |
| 15 | Se añade `/sdd-enlaces` a SKILL (tabla de fases), coordinator (ruteo) y `sdd.md` (lista), como `/sdd-estructura` | Solo `AGENTS.md` | E9 exige que los comandos citados existan y los existentes estén citados; E10 y el ruteo siguen el precedente |
| 16 | `sdd-init.md` e `INSTALAR.md` cambian una frase para sugerir `/sdd-enlaces` en vez de «los pasos de `sync-agents.md`» | No tocarlos | Dejarían una remisión a pasos que ya no sugieren nada; no cambia lo que `/sdd-init` escribe |
| 17 | E1 se mide sobre las herramientas declaradas en «visto»; las no declaradas salen como AVISO o quedan fuera de E1, y las filas `excepcion:`/`copia:` también como AVISO, no como FALLA. E19: se ignora `.agents/local/`. Cualquier otro cambio a E1–E21 queda fuera | Tocar más chequeos | Mínimo para separar validar de aplicar (fuera de alcance de la spec) y para que el clon no falle E19 al correr `/sdd-enlaces` |
| 18 | `/sdd-estructura` y `validar-estructura.md` ya no «aplican»; remiten a `/sdd-enlaces` para enlaces y réplicas. Las demás correcciones las pide la persona como acción aparte | Mantener «aplícala una por una» | Contradice RF-11 |
| 19 | El bump (`VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.6.0.md`) es la fase F6 de 0009, **después** de fusionar 0008 y 0010 | Bump parcial por feature | Las tres comparten 0.6.0; los manifiestos necesitan los archivos finales (E20) |

**ADR aprobados (se crean en el cierre, F6, con la skill `adr`)**
- ADR-A «Estado local `kit-visto` y aviso al arranque; solo `/sdd-enlaces` escribe». Alternativas reales: hook Git post-merge (rechazado por ADR 004), marcador versionado (ruido y conflictos en merges), sin estado (no distingue qué cambió). Efecto duradero: formato de «visto» y separación lectura/escritura que otros features reutilizarán.
- ADR-B «El arranque ya no detecta réplicas ni ignorados versionados». Alternativa: mantenerlos como aviso de solo lectura. Efecto: menos detección automática a cambio de un aviso único.
- Aprobados por el usuario (2026-11-01). Se crean en el cierre (F6) con la skill `adr`, ambos separados, con el siguiente número libre al crearlos (hoy el último es 0018; 0008/0010 pueden tomar antes).

### 6.3 Diseño

**Formato de `.agents/local/kit-visto`** (texto plano UTF-8 sin BOM, una línea por dato; ejemplo):

```
version: 0.6.0
herramientas: claude, cursor
excepcion: .cursor/skills
copia: .gemini/commands
pieza: agents/coordinator.md
pieza: commands/sdd-spec.md
pieza: skills/sdd
```

- Prefijos válidos de «visto»: `version`, `herramientas`, `excepcion`, `copia`, `pieza`. `copia:` y el caso sin Git (decisión 9) son **extensiones de detalle del plan**, no están en la spec.
- Orden: `version`, `herramientas`, `excepcion*`, `copia*`, `pieza*`. `version` es `X.Y.Z` (nunca `ninguna`). `herramientas`: lista separada por comas de `claude`, `cursor`, `gemini`, `github` (nunca vacía: sin ellas no se escribe). `excepcion`/`copia`: ruta de enlace de la tabla. `pieza`: ruta relativa a `.agents/` de cada entrada de primer nivel de `agents/`, `commands/` y `skills/`.
- Corrupto: falta `version` o `herramientas`, cualquier línea con un prefijo distinto de los cinco válidos, o `version` inválida. Se trata como inexistente.
- Escritura: se arma el contenido completo y se escribe de una vez al terminar con éxito (RF-4).

**Paso 7 de arranque** (solo lectura; el texto en `AGENTS.md` y `template/AGENTS.md`, la lógica en `sync-agents.md`):

```
7. Chequeo de enlaces y kit (solo lectura, un único aviso por sesión): sigue `.agents/sync-agents.md` › «Chequeo de arranque».
   Si hay diferencias, muestra el aviso combinado y recomienda `/sdd-enlaces` (no `/sdd-init`); si no, no digas nada. No escribas nada.
```

Lógica en `sync-agents.md` (qué se compara vive solo allí): si falta `.agents/.kit-version` y existe `template/` (clon del kit), omitir el chequeo y no decir nada (decisión 11); si no, leer `kit-visto` (ausente o corrupto = todo nuevo, RF-1b literal). Calcular: versión distinta (incluye retroceso), piezas nuevas/retiradas, y para herramientas declaradas, filas FALTA, DIFERENTE u OCUPADO sin `excepcion:`; ignorar las demás filas y tratar `copia:` como `OK (copia)`. Sin cambios, silencio. Una vez por sesión, aunque se posponga.

**Ejemplo del aviso** (3 líneas):

```
El kit cambió de 0.5.0 a 0.6.0: 3 piezas nuevas (commands/sdd-enlaces.md, ...) y 1 retirada (skills/x); faltan 2 enlaces de claude.
Corre `/sdd-enlaces` para crearlos con tu confirmación. No corras `/sdd-init`.
Mínimo: abre sesión, `/sdd-enlaces`, comprueba con `/sdd-estructura`; detalle: ACTUALIZAR.md del kit.
```

Variantes: omisión silenciosa en el clon del kit; clon nuevo («Clon nuevo: elige tus herramientas con `/sdd-enlaces`»); solo enlaces (RF-1d, sin línea 1 de versión). Tope 3 líneas; piezas listadas hasta N = 5 con «y X más».

**Estructura de `/sdd-enlaces`** (frontmatter `description`, `argument-hint` vacío; sesión principal sin subagente; no escribe `05_avance.md`):
1. Qué lee: `.agents/sync-agents.md` (estados, tabla, mecanismo, pasos 3–8, formato y chequeo), `.agents/local/kit-visto` si existe.
2. Preparar: detectar SO/shell y si es repositorio Git (decisión 9); leer «visto».
3. Preguntar herramientas (RF-5); si ninguna, terminar sin escribir (decisión 7).
4. Clasificar solo las filas de esas herramientas; detectar colgantes, réplicas, ignorados versionados.
5. Mostrar un plan con comando exacto por enlace (symlink, junction, copia; criterio de `sync-agents.md`) y pedir confirmaciones en el orden de la decisión 10.
6. Ejecutar solo lo confirmado, una acción a la vez; un fallo se anota y se continúa con las demás herramientas (RF-12).
7. Re-verificar y reportar por herramienta (`OK`, `OK (copia)`, FALLA, excepción).
8. Si todo OK sobre las herramientas declaradas (excepciones cuentan como OK), crear `.agents/local/` si falta, comprobar `check-ignore`, escribir «visto» (RF-3/4). Si no, no escribir y avisar que el aviso volverá. Rama sin `.agents/.kit-version` ni `template/`: crear enlaces, NO escribir «visto» e informar que falta el marcador (ver `ACTUALIZAR.md` › Marcador de versión).
Qué no hace: no escribe scripts, no corre `/sdd-init`, no commitea, no toca contenido real, no modifica `AGENTS.md`.

**Sección de `ACTUALIZAR.md`** («Qué hacer después de un pull que actualiza el kit», corta):
1. No corras `/sdd-init`: ya se corrió al instalar y vuelve a personalizar el proyecto.
2. Abre una sesión: el paso 7 muestra el aviso con lo que cambió.
3. Corre `/sdd-enlaces`: elige tus herramientas, confirma por tipo de acción; marca «visto» solo si termina bien. Si cambiaste de rama o clonaste de nuevo, vale lo mismo.
4. `/sdd-estructura` sirve para comprobar (solo lectura).
5. Si `PREFERENCIAS.md` no existe se crea sola en la sesión (paso 10).
6. Un hook Git post-merge es opción manual de cada persona; el kit no lo incluye (ADR 004).
Se añade al índice del documento y se ubica antes de `## Enlaces`.

**Tabla RF -> archivo -> verificación**
| RF | Archivo(s) | Verificación |
|---|---|---|
| 1, 2 | `sync-agents.md` › Chequeo de arranque; `AGENTS.md` paso 7 | Escenarios E-1…E-4, E-10 y E-11 del ensayo (F5): aviso de ≤3 líneas, una vez, N = 5; silencio en el clon del kit |
| 3 | `sdd-enlaces.md`; `.gitignore` (sin cambio) | `git check-ignore -v .agents/local/kit-visto` imprime la regla; ensayo sin Git y con `.gitignore` sin la regla |
| 4, 5 | `sdd-enlaces.md` | Escenarios: éxito sin nada que corregir escribe «visto»; rechazo y cero herramientas no lo escriben |
| 6, 7 | `sdd-enlaces.md`, `sync-agents.md` | Revisión: un comando por enlace, SO y mecanismo, una confirmación por tipo en el orden de la decisión 10 |
| 8, 9 | `sdd-enlaces.md`, `sync-agents.md` | Escenarios OCUPADO (excepción / no) y colgante; contenido real intacto (`ls` antes y después) |
| 10 | `sync-agents.md`, `sdd-enlaces.md`, `AGENTS.md` | `grep -n "paso 7\|paso 8" ` y lectura: reglas en una sola fuente, sin copias |
| 11 | `sdd-estructura.md`, `validar-estructura.md`, `AGENTS.md` | `grep -n "aplícala" template/.agents/commands/sdd-estructura.md` vacío; los dos comandos solo leen |
| 12 | `sdd-enlaces.md` | Reporte con `OK (copia)` en el escenario de copia |
| 13 | `ACTUALIZAR.md`, aviso | Sección presente y enlazada en el índice; aviso con los pasos mínimos |
| 14 (CF-8) | `template/.agents/commands/sdd-enlaces.md`, réplica, `AGENTS.md` x2, SKILL, coordinator, `sdd.md` | `diff -rq template/.agents .agents` (solo `napkin.md`; `caveman` y `local/` fuera); `grep -rn "sdd-enlaces" AGENTS.md template/AGENTS.md`; `find template -name "*.sh" -o -name "*.ps1" -o -name "*.py"` vacío; E9; E20; versión 0.6.0 en `VersionAgentKitSdd` y Changelog (F6) |

## 7. Fases y estimado
| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | Diagnóstico y preparación: partir de una rama base que contenga 0008 y 0010 fusionadas; releer `AGENTS.md`, `template/AGENTS.md`, `validar-estructura.md` (E21), `sdd-estructura.md`, `sdd-init.md`, SKILL, `sdd.md` y coordinator; estado de `.gitignore`. **Si 0010 aún no está fusionada**: no editar los textos «E1 a E20/E21» ni hacer F6; avisar al usuario, avanzar solo con F1 y lo que no colisione, y reabrir F0 cuando 0010 se fusione (rebase y releer) | — | 0.5 |
| F1 | `sdd-enlaces.md` en `template/` y réplica | 3–9, 12, 14 | 2.5 |
| F2 | `sync-agents.md` (formato, chequeo de arranque, pasos 3–8), `validar-estructura.md`, `sdd-estructura.md` y réplicas | 1, 2, 3, 10, 11 | 2.0 |
| F3 | `AGENTS.md` x2 (paso 7, comandos), `README.md` de layout, SKILL, coordinator, `sdd.md`, `sdd-init.md` y réplicas | 1, 2, 13, 14 | 1.5 |
| F4 | `ACTUALIZAR.md` (sección, Enlaces, Huérfanos) e `INSTALAR.md` | 13 | 1.0 |
| F5 | Verificación: comandos de §8 y ensayo de escenarios (incluidos E-10, clon del kit, y E-11, sin `.kit-version` ni `template/`) en proyecto temporal y en el clon | todos | 3.5 |
| F6 | Cierre tras fusionar 0008 y 0010: bump 0.6.0 (`VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` con la nota del grupo B, `manifiestos/0.6.0.md` y E20); crear ADR-A y ADR-B con la skill `adr` (siguiente número libre); anotar en `01_spec.md` la ampliación de RF-1d (OCUPADO no registrado), sin `/sdd-change`; anotar junto a ella que RF-4 no aplica sin versión legible (sin `.kit-version` ni `template/` no se escribe «visto») | CF-8, 1 | 1.5 |
| Total | | | 12.5 |

## 8. Criterios de aceptación
Verificación con `AGENTS.md` › Comandos del kit (Build: No aplica; consulta de datos: No aplica):
- Test: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` y revisar que copie todo sin pisar nada (nunca en un proyecto real).
- Lint por archivo tocado: `file -i <archivo>` da utf-8; `grep -l $'^\xEF\xBB\xBF' <archivo>` vacío; `name` del frontmatter = carpeta; rutas citadas existentes; sin marcadores `<<…>>` en `template/` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- Estructura: `/sdd-estructura` (E1–E21) y `diff -rq template/.agents .agents` (solo `napkin.md`; el `caveman` del clon es ajeno).
- Ensayo de escenarios (agente real sobre un proyecto temporal): E-1 clon nuevo; E-2 versión nueva con enlaces FALTA; E-3 misma versión con piezas nuevas; E-4 solo enlaces; E-5 OCUPADO intencional y no; E-6 rechazo; E-7 sin Git; E-8 cero herramientas; E-9 dos herramientas distintas; E-10 clon del kit (sin `.kit-version` y con `template/`): silencio; E-11 sin `.kit-version` ni `template/`: aviso «todo nuevo», se crean los enlaces, no se escribe «visto» y se informa que falta el marcador.

| CF | Se cumple si |
|---|---|
| CF-1 | E-1, E-2, E-3 y E-4 muestran un único aviso de ≤3 líneas con `/sdd-enlaces`; sin cambios, silencio; E-10 (clon del kit) silencio (RF-1, RF-2) |
| CF-2 | `git check-ignore -v .agents/local/kit-visto` imprime la regla; «visto» solo aparece tras éxito (E-6 y E-8 no lo crean) (RF-3, RF-4) |
| CF-3 | Con solo `claude` declarado, no se crea nada de `.cursor/` y el arranque ignora las demás filas (E-9) (RF-5) |
| CF-4 | Se crea/reemplaza/retira con una confirmación por tipo y comando por SO; el reporte final muestra el estado por herramienta (RF-6, 7, 9, 12) |
| CF-5 | En E-5 el contenido real sigue igual (`ls`); la excepción queda en «visto» y deja de avisar (RF-7, 8) |
| CF-6 | `/sdd-estructura` y el arranque no escriben; los pasos 7 y 8 se ejecutan solo desde `/sdd-enlaces`, con reglas únicamente en `sync-agents.md` (RF-10, 11) |
| CF-7 | `ACTUALIZAR.md` trae la sección con «no correr `/sdd-init`» y el aviso trae los pasos mínimos (RF-13) |
| CF-8 | Sin scripts versionados (`find`), `/sdd-enlaces` en `template/` y clon, citado en `AGENTS.md` x2, 0.6.0 y changelog actualizados una vez (RF-14) |

**Riesgos del plan**
- Conflictos con 0008 y 0010 en `AGENTS.md`/`template/AGENTS.md`: releer antes de editar, Edit mínimo, no reescribir el archivo, aplicar 0009 después de ambos.
- Réplica del clon: aplicar el mismo cambio en `.agents/` y repetir `diff -rq`.
- Huellas de los manifiestos: recalcular tras el último cambio de 0008 y 0010 (F6).
- Detección perdida de réplicas/ignorados al arranque: aceptada por el usuario, ver ADR-B.
- Clon del kit: el chequeo se omite solo con `template/` presente y `.kit-version` ausente; un proyecto sin `.kit-version` ni `template/` avisa como «todo nuevo» (RF-1b literal).
- Colisión con 0010 (E21): «E1 a E20» pasa a «E1 a E21» en `validar-estructura.md`, `sdd-estructura.md` y réplicas, `sdd-init.md` (pasos 4.2/4.3), `skills/sdd/SKILL.md`, `commands/sdd.md` y `agents/coordinator.md` y `agents/reviewer.md`. Mitigación: F0 relee tras fusionar 0008 y 0010, desde una rama base con ambas; si 0010 no está fusionada, ver F0.
- Paso 7 en `AGENTS.md`: los proyectos existentes no lo reciben por actualización; se incorpora a mano (ACTUALIZAR.md y changelog).

**Decisiones aprobadas por el usuario (2026-11-01)**
1. Cero herramientas declaradas no es éxito: no se escribe «visto» y el aviso vuelve (decisión 7).
2. Falta `.agents/.kit-version`: opción A, RF-1b literal (todo nuevo); solo si además existe `template/` (clon del kit) se omite el chequeo de arranque. Sin `version: ninguna` ni excepción (decisión 11).
3. El arranque deja de detectar ignorados versionados y réplicas huérfanas; pasa a `/sdd-enlaces` y E2/E3 (decisión 4).
4. Ediciones de `sdd-init.md` e `INSTALAR.md` por una frase (decisión 16) y ADR-A y ADR-B, creados en el cierre (F6) con la skill `adr`.
5. RF-1d ampliado a OCUPADO no registrado (decisión 3): se anota en la spec en el cierre (F6), sin `/sdd-change`.
6. Sin `.agents/.kit-version` ni `template/`: `/sdd-enlaces` crea los enlaces, no escribe «visto» e informa que falta el marcador (ver `ACTUALIZAR.md` › Marcador de versión); RF-4 no aplica sin versión legible (decisión 11, paso 8 de `/sdd-enlaces`). Aprobado por el usuario el 2026-11-01.

**Pendiente / abierto**: Ninguna.

## Delta Cambio 1 (2026-11-03): E1, E7, ejemplo del aviso y git-commit
Spec: `01_spec.md` › «Cambio 1» (aprobado 2026-11-03 21:28), RF-15 a RF-18, CF-9 a CF-12. Lo anterior del plan, aprobado, no cambia. Análisis: `02_analisis_impacto.md` › «Delta Cambio 1». Plan del delta: pendiente de aprobación.

### Decisiones del delta
| # | Decisión | Alternativa descartada | Por qué |
|---|---|---|---|
| D-1 | **RF-16, solo la diferencia.** El texto vigente de E1 ya cubre «herramientas declaradas», `AVISO` para no declaradas, `excepcion:` y `copia:`, y exige sin `OCUPADO` ni `DIFERENTE`; eso no se toca. Se añade el caso sin «visto» legible (ausente o corrupto): se miden todas las filas; `FALTA` = `AVISO` con el texto exacto «enlace FALTA de herramienta no declarada: correr `/sdd-enlaces`»; `DIFERENTE`/`OCUPADO` sin `excepcion:` = `FALLA`. Con «visto» legible, E1 queda igual | Reescribir E1; tratar toda `FALTA` sin «visto» como FALLA; escribir «visto» desde `/sdd-estructura` | El hueco real es solo la severidad de `FALTA` sin «visto» (y la contradicción «sobre todas las filas» frente a «no declaradas = AVISO»). Mínimo cambio, consistente con RF-5 y RF-11 |
| D-2 | **RF-17 y RF-18 son coherencias de texto fuera de E1/E7.** Se hacen aquí porque el mismo cierre las dejó pendientes y tocan archivos del propio feature (`sync-agents.md`) o una regla que 0009 desplazó (ignorados versionados pasaron a `/sdd-enlaces`). Se anota como **ampliación leve del «Fuera de alcance»** de la spec original (que acotaba los cambios a E1-E20 «a lo necesario para separar validar de aplicar»): ya está aprobada en el Cambio 1; este plan solo la deja explícita para la persona que apruebe | Dos specs/features aparte para dos retoques de texto | Costo de proceso mayor que el cambio; comparten archivos y manifiesto con el resto del delta |
| D-3 | **RF-15, E7 con exención por presencia en `template/`.** Texto: en el repo del kit (el que tiene `template/`), toda skill presente en `.agents/skills/` y ausente de `template/.agents/skills/` queda exenta (no exige fila ni es FALLA); en un proyecto instalado no aplica y toda skill exige fila. El texto de E7 repite expresamente: «una fila a una skill inexistente sigue siendo FALLA» (sin relajar filas a skills inexistentes) | Fila versionada de `caveman`; lista fija de exenciones | Una skill local no debe versionarse en `AGENTS.md`; la lista fija caduca. `template/` presente es la señal fiable de clon del kit (decisión 11) |
| D-4 | **`AGENTS.md` raíz: 3 filas** (`adr`, `migracion-bd`, `seguridad-basica`) con el texto de `template/AGENTS.md:98-100`, tras la fila de `formato-respuesta` y antes del comentario `<!-- Las skills de dominio... -->`. Solo esas líneas (Edit mínimo) | Copiar la tabla entera | Una sola fuente de redacción; evita tocar zonas de 0008/0010 |
| D-5 | **RF-17.** En `sync-agents.md` › «Chequeo de arranque»: el ejemplo pasa a línea 1 «El kit cambió de 0.5.0 a 0.6.0: 3 piezas nuevas (commands/sdd-enlaces.md, ...) y 1 retirada (skills/x).» y línea 2 «Faltan 2 enlaces de claude: corre `/sdd-enlaces` para crearlos con tu confirmación. No corras `/sdd-init`.»; el paso 4 añade una frase explícita «los enlaces pendientes van siempre en la línea 2». Línea 3 y variantes sin cambio | Dejarlo como está | El ejemplo contradice al paso 4 (líneas 108-111) |
| D-6 | **RF-18.** `git-commit/SKILL.md` paso 3 pasa a: «**Versionados e ignorados**: aplica la regla del paso 7 de `.agents/sync-agents.md` (archivos versionados que el `.gitignore` ya ignora). Si hay, no incluirlos en el commit; el commit aparte lo propone esa regla. No ejecutar sin confirmación.» El paso 3 deja explícito que git-commit remite al paso 7 de `sync-agents.md`, que es la fuente de la regla y la ejecuta el comando `/sdd-enlaces`; quien commitea sigue ese paso para obtenerla (una sola fuente). Sin `git ls-files -ci` ni `git rm --cached` | Mantener el comando duplicado | Una sola fuente (constitución §3); `grep` de `ls-files -ci` sobre git-commit queda vacío |
| D-7 | **Manifiesto y changelog.** Recalcular con el algoritmo de huella de `ACTUALIZAR.md` solo las 3 rutas tocadas de `template/` (`.agents/validar-estructura.md`, `.agents/sync-agents.md`, `.agents/skills/git-commit/SKILL.md`) y correr E20 completo. Changelog 0.6.0: una línea que menciona E1, E7, el ejemplo del aviso y git-commit. Sin cambio en `VersionAgentKitSdd` ni en ADR | Re-generar todo el manifiesto | Menor riesgo de sobrescribir huellas correctas |

### Texto propuesto
- **E1** (celda «Qué se comprueba»): «Se mide sobre las herramientas declaradas en `.agents/local/kit-visto`: cada fila de `.agents/sync-agents.md` está `OK`; ningún `OCUPADO` ni `DIFERENTE`. Sin «visto» legible (ausente o corrupto) se miden todas las filas: `FALTA` es `AVISO` («enlace FALTA de herramienta no declarada: correr `/sdd-enlaces`»); `DIFERENTE` y `OCUPADO` sin `excepcion:` son `FALLA`. Filas de herramientas no declaradas (con «visto»), `excepcion:` y `copia:` son `AVISO`, no `FALLA`.»
- **E7**: añadir al final de la celda: «En el repo del kit (el que tiene `template/`), toda skill presente en `.agents/skills/` y ausente de `template/.agents/skills/` queda exenta: no exige fila ni es `FALLA`. En un proyecto instalado no aplica».

### Fase F7 y estimado del delta
| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| (02/03 del delta) | Este análisis y plan | — | 0.7 |
| (04 del delta) | Tareas F7 | — | 0.3 |
| F7.1-F7.6 | Implementación: E1, E7 + réplica, `AGENTS.md` raíz, `sync-agents.md`, `git-commit`, manifiesto y changelog | 15-18 | 1.2 |
| F7.7-F7.8 | Verificación: lint, `diff`, E20, E1 y E7 reejecutados, marcar F5.2 y F6.9 | 15-18 | 0.4 |
| Total delta | | | 2.6 |
En `04_tasks.md` › Estimado solo se suman las horas de F7 (1.6 h); las 1.0 h de documentación del delta se cuentan aparte (ya gastadas al aprobar).

### Criterios de aceptación del delta
Comandos de `AGENTS.md` › Comandos (Build y datos: No aplica).
| CF | Se cumple si |
|---|---|
| CF-9 | E7 reejecutado en el clon: sin FALLA por `adr`, `migracion-bd`, `seguridad-basica` ni `caveman`; E1 reejecutado: las 7 filas FALTA de Gemini/GitHub salen como AVISO y ningún FALLA; un `DIFERENTE` u `OCUPADO` sin excepción simulado sobre el texto sigue siendo FALLA |
| CF-10 | `diff template/.agents/validar-estructura.md .agents/validar-estructura.md` vacío; E20 recalculado sin FALLA; changelog 0.6.0 menciona E1 y E7 |
| CF-11 | El ejemplo del «Chequeo de arranque» pone los enlaces pendientes en la línea 2 |
| CF-12 | `grep -n "ls-files -ci" template/.agents/skills/git-commit/SKILL.md` vacío y el paso 3 remite al paso 7 de `sync-agents.md`; réplica idéntica |

### Riesgos del delta
- Huellas obsoletas si se edita algo tras recalcular: recalcular al final (F7.6 después de F7.2, F7.4, F7.5).
- Réplica desfasada: copiar y repetir `diff -rq template/.agents .agents` (solo `napkin.md`).
- Otro FALLA oculto en `/sdd-estructura` fuera de E1/E7: se anota y se consulta, no se corrige aquí.

**Pendiente de aprobación**: la ampliación leve del «Fuera de alcance» (D-2) y el texto de D-1, D-3, D-5 y D-6.
