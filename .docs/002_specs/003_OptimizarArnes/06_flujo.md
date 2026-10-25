# 06 — Flujo operativo: Optimizar el arnés SDD

Guía de uso del arnés tal como queda. Cada fase tiene un solo comando, y ese comando es el procedimiento.

## 1. ¿Qué es y para qué sirve?

El arnés SDD es la forma de pedir un análisis o un desarrollo nuevo. Tú escribes la petición; el kit elige la fase, lee solo lo que esa fase usa y deja el resultado en `.docs/002_specs/<NNN>_<Feature>/`.

Cada fase tiene un solo comando en `.agents/commands/` que define el procedimiento (la lista numerada). La skill `sdd`, el coordinator y el agente del rol nombran ese comando y lo enlazan. No reescriben la lista. La forma de cada documento está en `.agents/skills/sdd/plantillas.md`. Los tiempos están en `.agents/skills/sdd/avance-y-tiempos.md`. El arranque de sesión está en `AGENTS.md`.

Sirve para obtener el mismo resultado de siempre (spec, plan, tareas, un paso implementado, validación, esta guía y el cierre) sin que un rol relea el proyecto entero ni repita el trabajo de otro.

## 2. ¿Quién lo usa? (roles y permisos)

No hay pantallas ni permisos de interfaz. El acceso es el comando `/sdd` o el comando de la fase.

| Quién | Qué hace |
|---|---|
| Tú | Pides con `/sdd <petición>` o lanzas la fase. Apruebas spec, plan, tareas, flujo y, si lo pides, el commit. |
| Sesión principal | Hace el arranque de "Cada sesión" en `AGENTS.md`. Es la única que escribe `05_avance.md`. Corre `/sdd-estructura` y `/sdd-cierre`. Registra los tiempos. |
| coordinator | Elige un comando con su tabla y lo sigue. Para enrutar solo comprueba si `AGENTS.md` contiene `SDD-INIT-PENDIENTE`. |
| planner | Redacta `01_spec.md`, `02_analisis_impacto.md`, `03_plan.md`, `04_tasks.md` y `06_flujo.md`. |
| implementer | Ejecuta un solo paso `Fn.m`, cumple su "Hecho cuando" y lo marca en `04_tasks.md`. |
| reviewer | Revisa la spec en `clarify` y valida en `validate`. Clasifica el modo de seguridad. |
| Mantenedor del kit | Comprueba que el procedimiento de cada fase está solo en su comando. |

Un subagente no repite el arranque (enlaces, prefijos, preferencias). No se supone que la herramienta le inyecte `AGENTS.md`. Cada rol lee solo las secciones que nombra su instrucción, en "Qué lee cada rol":

| Rol | Secciones de `AGENTS.md` |
|---|---|
| coordinator | Ninguna para enrutar. Solo el marcador `SDD-INIT-PENDIENTE`. |
| planner | "Comandos" y, al planear, "Stack y convenciones de código". |
| implementer | "Comandos" y "Stack y convenciones de código". |
| reviewer | "Comandos" y "Stack y convenciones de código". |

El implementer no registra en Git y no escribe `05_avance.md`. El reviewer no corrige código y no corre E1 a E20. Esas declaraciones están en el texto de cada rol. El modelo por rol y los permisos del frontmatter no forman parte de este uso: siguen en la deuda `003_ModeloYPermisosDeAgentes`.

`AGENTS.md` de un proyecto ya instalado recibe un cambio de redacción por fusión manual (grupo B del procedimiento de actualización). Este clon ya tiene la sección "Qué lee cada rol" en la raíz y en `template/AGENTS.md`.

## 3. ¿Cuándo aplica?

Aplica cuando pides un requerimiento, una spec, un análisis, un plan, tareas, implementar un paso, validar, el flujo operativo, cerrar, cambiar un requisito, saber en qué va un feature, revisar la estructura del kit, personalizar el kit la primera vez o revisar la constitución.

Si `AGENTS.md` todavía contiene `SDD-INIT-PENDIENTE`, el único comando que sigue es `/sdd-init`. Los demás `/sdd*` se detienen y te lo sugieren.

Un arreglo chico que no merece carpeta queda fuera de este flujo: el coordinator te lo dice y no abre feature.

## 4. Flujo paso a paso (acceso → pantalla → acción → resultado)

No hay pantallas. El acceso es `/sdd <petición>` o el comando de la fase (`/sdd-spec`, `/sdd-clarify`, `/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/sdd-validate`, `/sdd-flujo`, `/sdd-cierre`, `/sdd-change`, `/sdd-status`, `/sdd-estructura`, `/sdd-init`, `/sdd-constitucion`).

### Arranque

La sesión principal ejecuta "Cada sesión" de `AGENTS.md` (napkin, memoria, antecedentes, enlaces, prefijos, preferencias). Un subagente no lo repite. En cada llamada recibe la fase, tu petición con tus palabras, las decisiones, las rutas y el resultado de la fase anterior.

### Cómo entra una petición

El coordinator, con la tabla "Cómo decides qué hacer", anuncia en una línea cuál comando eligió y por qué, antes de abrirlo. El comando `/sdd` dice en una línea la carpeta (nueva o existente) y hasta dónde llega. Si la petición es ambigua, pregunta antes de crear o escribir nada.

La skill `sdd` y el contrato de tiempos se cargan solo si la fase elegida los usa. Para `status` y `estructura` no se cargan.

| Si pides… | Comando |
|---|---|
| Un requerimiento nuevo o la spec | `/sdd-spec` |
| Revisar o aclarar la spec | `/sdd-clarify` |
| Análisis, impacto o el plan | `/sdd-plan` |
| Tareas o el desglose | `/sdd-tasks` |
| Implementar o seguir con el código | `/sdd-implement` |
| Validar, probar o QA | `/sdd-validate` |
| El flujo operativo o la guía de uso | `/sdd-flujo` |
| Cerrar o terminar | `/sdd-cierre` |
| Cambiar un requisito ya escrito | `/sdd-change` |
| Cómo va, el estado o dónde quedamos | `/sdd-status` |
| Validar la estructura del kit | `/sdd-estructura` |
| Continuar, sin nombrar la fase | El comando del PUNTO DE RETOMO |
| El flujo de punta a punta | `/sdd-spec` y, tras cada puerta aprobada, el siguiente de la tabla de fases |

"Solo análisis" termina en el plan, con `Estado: analisis`. "Plan de trabajo" termina en las tareas.

### Dónde está el procedimiento

| Fase | Comando (ahí está la lista) | Quién solo nombra y enlaza |
|---|---|---|
| `spec` | `sdd-spec.md` | skill `sdd`, coordinator, planner |
| `clarify` | `sdd-clarify.md` | skill `sdd`, coordinator, reviewer |
| `plan` | `sdd-plan.md` | skill `sdd`, coordinator, planner |
| `tasks` | `sdd-tasks.md` | skill `sdd`, coordinator, planner |
| Implementación | `sdd-implement.md` | skill `sdd`, coordinator, implementer |
| `validate` | `sdd-validate.md` | skill `sdd`, coordinator, reviewer |
| `flujo` | `sdd-flujo.md` | skill `sdd`, coordinator, planner y reviewer |
| `cierre` | `sdd-cierre.md` | skill `sdd`, coordinator |
| `change` | `sdd-change.md` | skill `sdd`, coordinator, planner |
| Estado | `sdd-status.md` | skill `sdd` (no se carga), coordinator, `sdd.md` |
| Estructura | `sdd-estructura.md` | skill `sdd` (no se carga), coordinator, `sdd.md` |
| Entrada | `sdd.md` | coordinator |
| Inicio | `sdd-init.md` | `sdd.md`, marcador en `AGENTS.md` |
| Constitución | `sdd-constitucion.md` | `sdd.md`. El tope lo apunta este comando hacia `sdd-init.md` |

### Spec

`/sdd-spec` crea la carpeta y `05_avance.md`, agrega la fila al índice con estatus `Análisis` y delega `01_spec.md` en borrador. La sesión principal lee una sola vez la constitución y `MEMORY.md` y se las pasa al planner; el planner no las relee. Si la idea es ambigua, te pregunta de una en una, hasta cinco, y vuelve a redactar con tus respuestas.

El número de la carpeta sale de "Numeración única" del README de specs de esa copia. En un proyecto el ancho es de 4 dígitos (`0001`) y el mismo ancho vale para deudas y ADR. En el clon del kit manda el ancho de ese README: este clon sigue en 3. Las carpetas de sección (`.docs/001_constitucion/`, `002_specs/`, `003_deuda/`, `004_adr/`) se quedan en 3. El chequeo E12 conserva esa redacción.

### Clarify

`/sdd-clarify` pide al reviewer una lista de ambigüedades, contradicciones, casos límite y conflictos. No propone la solución. Si apruebas correcciones, el planner ajusta la spec y ves el párrafo de antes y el de después. Si el archivo no tiene seguimiento en Git, o el proyecto no usa Git, ese contraste es el medio: no depende de `git diff`. La fase para hasta que apruebes. Entonces el estado pasa a `aprobada`.

### Plan y tareas

`/sdd-plan` solo sigue si la spec está `aprobada` y sin dudas abiertas. El planner redacta el análisis y el plan con la forma de `plantillas.md` (secciones `## 02_analisis_impacto.md` y `## 03_plan.md`). El comando no copia esa plantilla. Si pediste solo análisis, el estado pasa a `analisis` y el plan termina ahí. Ese estado no detiene `/sdd-implement`: ese comando mira si `04_tasks.md` está aprobado.

`/sdd-tasks` redacta `04_tasks.md` con la sección `## 04_tasks.md` de `plantillas.md`, la spec, el plan y `AGENTS.md` › Comandos. Para hasta que apruebes plan y tareas.

Los tiempos de ambas fases se anotan con el contrato `avance-y-tiempos.md`. El comando no redefine la fila.

### Implementar un paso

`/sdd-implement <carpeta> <Fn.m>` hace un solo paso. Si `04_tasks.md` no está aprobado, para.

Le pasa al implementer la lectura mínima: el paso con su "Hecho cuando", los RF que cita, la parte del plan que lo cubre y las secciones de `AGENTS.md` que el rol usa. De la constitución y de la skill `sdd`, solo las secciones que el paso o el plan citan. Un documento de menos de 4 096 bytes puede leerse entero.

Si el paso no cita ningún RF, lee la fase `Fn` y avisa. Si el plan no tiene sección que cubra el paso, lee el plan completo y avisa. La ampliación queda dentro de esta lista, con tope de 3 secciones: (a) una sección del plan o de la spec nombrada por un RF o paso citado; (b) una decisión de `05_avance.md` que el paso cite; (c) una sección de `AGENTS.md` distinta de las que el rol ya usa. El implementer devuelve el motivo de cada ampliación y los avisos de respaldo. La sesión principal los anota.

Cuando cumple el "Hecho cuando", marca ese paso en `04_tasks.md` y para. No registra en Git y no escribe `05_avance.md`. No ejecuta SQL: lo deja en `sql/` para que lo corras tú. No empieza el siguiente.

### Validar

`/sdd-validate` revisa el alcance del feature: archivos citados en avance, plan y tareas, más los archivos nuevos sin seguimiento que liste `git status` (no `git diff`) y que estén en la carpeta del feature o citados. Cualquier otro archivo modificado o sin seguimiento se informa como "ajeno o no atribuible" y no se revisa como propio. Tú decides si alguno pertenece al feature. Con seguimiento se usa la diferencia del archivo; en los nuevos, su contenido. Si un archivo mezcla cambios del feature y ajenos, avisa. Sin Git, el alcance es solo lo citado.

El reviewer lee el "Hecho cuando" del paso o fase y los criterios de aceptación de los RF que ese paso cita. No lee las tareas ni la spec enteras. Aplica la seguridad como dice la sección 8. No corrige código. Si el veredicto es CAMBIOS NECESARIOS, se vuelve al implementer con la lista exacta y después otra vez al reviewer, con un máximo de 2 vueltas. Una verificación manual de interfaz queda pendiente hasta que la confirmes; entonces se anota con su hora en Pruebas.

### Flujo operativo

`/sdd-flujo` pide esta guía a partir de lo que ya existe, con el mismo alcance que la validación. Con seguimiento se usa la diferencia del archivo; en los nuevos, su contenido. El reviewer la contrasta con ese alcance. Para hasta que la apruebes.

### Estado

`/sdd-status` no modifica archivos. Responde con la línea `Estado` de `01_spec.md`, la lista de documentos de la carpeta, cuatro secciones de `05_avance.md` (PUNTO DE RETOMO, Pasos, Pruebas y Tiempos) y el conteo de identificadores `Fn.m` en `04_tasks.md`. Con eso entrega la fase y el Estado, el punto de retomo, los pasos hechos y pendientes (x de y), las verificaciones manuales pendientes, las horas acumuladas y las filas con `pausa por confirmar`, y el siguiente paso con su comando `/sdd-*`. No entrega los documentos enteros ni lee `MEMORY.md`, salvo que pidas el detalle de uno. Si no hay `05_avance.md`, deduce la fase por los documentos presentes y el `Estado` de la spec.

### Estructura del kit

`/sdd-estructura` lo corre la sesión principal. Ejecuta E1 a E20 de `.agents/validar-estructura.md`. El reviewer no. No lanza un subagente y no toca `05_avance.md`: no es una fase del flujo de un feature. Muestra solo lo que falla y propone correcciones. Cuando confirmas una, la aplica una por una. E20 no se redefine. E12 no se reescribió.

### Inicio y constitución

`/sdd-init` se corre una sola vez. Reconoce el proyecto con un tope de 30 archivos o 150 KB acumulados, lo que ocurra primero, y pide confirmación para ampliarlo. No tiene meta de reducción de lectura. No escribe nada hasta que apruebes la propuesta. Al terminar quita el marcador `SDD-INIT-PENDIENTE`. Si el marcador ya no está, te dice que el proyecto ya está personalizado y pregunta si quieres revisar algo concreto.

`/sdd-constitucion` apunta al tope declarado en `sdd-init.md` (30 archivos o 150 KB, lo que ocurra primero). No declara otro y no tiene meta de reducción. `sdd-init.md` no nombra este comando. Propone los principios y espera tu aprobación antes de escribir.

### Cambio de requisito

`/sdd-change` actualiza primero la spec (RF nuevo o modificado, casos límite y fuera de alcance) y te muestra qué habría que tocar después en el análisis, el plan y las tareas. No reescribe esos tres hasta que la spec quede aprobada. No toca código.

### Cierre

`/sdd-cierre` lo ejecuta la sesión principal. Busca en la tabla de Pasos de `05_avance.md` estados distintos de `[x]` (`[ ]`, `[~]`, `[!]`). No lee las tareas enteras. Un paso que no esté `[x]` impide el cierre, salvo que decidas diferirlo y quede registrado, con propuesta de deuda. También exige que no queden verificaciones manuales en Pruebas ni en el PUNTO DE RETOMO, que `06_flujo.md` esté aprobado y que no haya filas `pausa por confirmar`.

Agrega a `05_avance.md` la tabla plan contra real, según el contrato, y una línea de qué ajustar en el siguiente plan si alguna fase se desvió fuerte. En todo feature revisa deuda y ADR: pasa a `Resuelta` lo que el feature resolvió y propone el alta de lo diferido. No crea deuda, memoria ni ADR sin tu visto bueno. Al cerrar, la spec pasa a `Estado: implementada` y se actualiza la fila del índice. El commit, solo si lo pides y lo confirmas.

### Tiempos

Los anota solo la sesión principal, con inicio, fin y minutos, según `.agents/skills/sdd/avance-y-tiempos.md`. En un feature abierto desde esta versión, cada paso real mide al menos 1 minuto. No se cierran varios pasos con filas en lote de 0 minutos. La única excepción es un paso trivial cuya Nota dice exactamente "sin trabajo real". Un feature ya cerrado conserva sus filas; ninguna comprobación le exige reescribirlas. `001_ActualizarKit` es el ejemplo y sigue válido con las suyas. La pausa de más de 30 minutos está en ese contrato; aquí no se copia.

## 5. Estatus y transiciones

La spec usa cuatro estados: `borrador`, `aprobada`, `analisis`, `implementada`.

| Desde | Qué ocurre | Hacia |
|---|---|---|
| (no existe) | `/sdd-spec` entrega la spec | `borrador` |
| `borrador` | Apruebas en `/sdd-clarify` | `aprobada` |
| `aprobada` | Pediste solo análisis y termina el plan | `analisis` |
| `aprobada` | `/sdd-cierre` comprueba que se puede cerrar | `implementada` |

`Estado: analisis` es el cierre de un pedido de solo análisis. Pedir implementar sigue yendo a `/sdd-implement`, que se detiene si `04_tasks.md` no está aprobado. Un cambio de requisito vuelve a la spec y espera aprobación antes de tocar plan y tareas.

Los pasos del avance usan `[ ]` pendiente, `[~]` en curso, `[x]` hecho y `[!]` bloqueado. El cierre solo acepta `[x]`, o un diferido que tú registres con propuesta de deuda.

La spec de esta carpeta (`003_OptimizarArnes`) pasó a `implementada` en el cierre del 2026-10-25 14:23.

## 6. Notificaciones (quién recibe qué)

No aplica: no hay notificaciones.

## 7. Piloto / liberación (si aplica)

No aplica: no hay piloto ni liberación a otros proyectos.

## 8. Errores frecuentes y qué hacer

- **Sigue el marcador `SDD-INIT-PENDIENTE`.** Para y corre `/sdd-init`. No sigas con otro `/sdd*`.
- **La petición no se puede clasificar.** El coordinator pregunta. No se crea carpeta ni se escribe nada hasta tu respuesta.
- **`/sdd-plan` se detiene.** La spec no está `aprobada` o aún tiene dudas abiertas. Vuelve a `/sdd-clarify`.
- **`/sdd-implement` se detiene.** `04_tasks.md` no está aprobado, o el paso es imposible. No improvisa otro camino.
- **El paso no cita RF, o el plan no tiene sección que lo cubra.** Lee la fase `Fn`, o el plan completo, y avisa. El implementer devuelve esos avisos de respaldo y el motivo de cada ampliación. La sesión principal los anota. La ampliación queda en la lista de 3 secciones de ese comando.
- **`/sdd-clarify` y no hay Git.** Ves el párrafo de antes y el de después. Ese es el contraste.
- **Aparece "ajeno o no atribuible".** Ese archivo no se revisa como del feature. Dices si alguno sí lo es. Si un mismo archivo mezcla cambios tuyos y ajenos, te avisan.
- **El cierre no avanza.** Hay un paso distinto de `[x]`, una verificación manual pendiente, el flujo sin aprobar o una fila `pausa por confirmar`. Cierras eso o registras el diferido con propuesta de deuda.
- **Init o constitución piden más lectura.** `sdd-init.md` declara el tope de 30 archivos o 150 KB y pide confirmación para ampliarlo. `sdd-constitucion.md` apunta a ese tope. `sdd-init.md` no nombra la constitución. No hay meta de reducción.
- **El prefijo `NNN_` ya existe.** Se conserva el de la rama remota y se te propone el siguiente libre. No se renombra solo.
- **La seguridad duda del alcance.** Se aplica el modo completo. La búsqueda de secretos corre siempre.

La skill `seguridad-basica` es la dueña de los dos modos. `/sdd-validate` y el reviewer solo la enlazan.

- **Solo secretos.** El cambio es solo documentación: prosa que explica, describe o enumera, sin añadir ni modificar código ejecutable. Se busca secretos y se dice que el modo fue "solo secretos".
- **Completo.** El cambio toca código ejecutable, datos, accesos o secretos. Se buscan secretos y se aplica el checklist. Ante duda, este modo.

Código ejecutable: lo que un agente, un shell o una herramienta ejecuta o interpreta como instrucción operativa (scripts, comandos de shell o bloques copiables, configuración con permisos, `tools` o `mode` en el frontmatter, SQL, y cualquier cambio de datos, accesos o secretos). Un `.md` cuenta como código ejecutable solo si añade o modifica uno de esos elementos.

## 9. Fuera de alcance

Sigue fuera de este uso:

- El modelo por rol y los permisos del frontmatter (`mode`, `permissions`). Están en la deuda `.docs/003_deuda/003_ModeloYPermisosDeAgentes.md`.
- Publicar 0.5.0. La versión sigue local, sin distribución a proyectos.

Tampoco entra aquí cambiar las skills de dominio, reducir el tamaño de los documentos de un feature, cambiar el procedimiento de actualización ni la definición de E20, ni agregar scripts.

## 10. Referencias (01_spec, 03_plan, pantallas)

No hay pantallas.

- `.docs/002_specs/003_OptimizarArnes/01_spec.md` — qué debe hacer el arnés. Estado vigente: `implementada`.
- `.docs/002_specs/003_OptimizarArnes/03_plan.md` — dónde vive cada regla (comando, skill, contrato, `AGENTS.md`).
- `.docs/002_specs/003_OptimizarArnes/05_avance.md` — punto de retomo, decisiones y la tabla de fuente única en Pruebas.
- `.agents/skills/sdd/SKILL.md` — tabla fase, enlace, rol y documento.
- `.agents/skills/sdd/plantillas.md` — forma de cada documento. Plan y tareas apuntan aquí.
- `.agents/skills/sdd/avance-y-tiempos.md` — tiempos, pausa, leyenda de pasos y quién escribe el avance.
- `.agents/skills/seguridad-basica/SKILL.md` — sección Modo.
- `AGENTS.md` — "Cada sesión" y "Qué lee cada rol".
- `.agents/validar-estructura.md` — Alcance: la sesión principal corre E1 a E20. E12 conserva su redacción.
- `.docs/002_specs/README.md` de la copia — ancho del consecutivo. En este clon, 3 dígitos.
