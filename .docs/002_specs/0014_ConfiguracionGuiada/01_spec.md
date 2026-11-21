# 01 — Spec: Configuración guiada del kit
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
Hoy `/sdd-init` hace una entrevista de hasta 6 preguntas sobre el proyecto y no configura el flujo: los modelos por rol, dónde corre cada rol, el tope de clarify y los entornos de QA son reglas fijas o quedan sin decidir. La spec `0013_ModelosPorAgente` define esos comportamientos y sus valores por omisión; esta spec los vuelve configurables por cada dev.
Objetivo: un asistente ofrece configurar el flujo paso a paso, con un contexto breve en cada paso, al instalar, al actualizar y cuando un dev lo pide. El kit solo sugiere: cada dev elige, puede reconfigurar cuando quiera y su configuración no se versiona. Todo paso se puede saltar, y saltarlo aplica el valor sugerido. El auditor externo es una opción beta que solo aparece si la persona la pide o si se detectan dos herramientas de agente.

## Usuarios y roles
- Persona que instala el kit en un proyecto nuevo.
- Persona que actualiza un proyecto que ya tiene el kit.
- Cada dev del proyecto, con su configuración propia (no versionada), incluido el que llega a un proyecto ya instalado.

## Historias de usuario
- HU-1. Como persona que instala, quiero configurar el flujo paso a paso entendiendo qué decide cada paso, para no tener que leer todo el kit.
- HU-2. Como persona con prisa, quiero saltar el asistente y quedarme con una configuración que ya funciona.
- HU-3. Como persona con dos herramientas de agente, quiero que se me sugiera un auditor de otra familia de modelo, sabiendo que es beta.
- HU-4. Como dev que usa otra herramienta de agente que el resto del equipo, quiero elegir mis modelos sin que mi elección cambie archivos versionados.

## Definiciones
- **Asistente**: secuencia de pasos de configuración, uno a la vez. Lo corre el comando `/sdd-config`; `/sdd-init` y el procedimiento de actualización lo invocan.
- **Paso**: una decisión con contexto breve (qué es y por qué importa, una o dos líneas), las opciones, y el valor sugerido marcado.
- **Valor sugerido** (o por omisión): el que el kit propone y se aplica al saltar un paso o todo el asistente.
- **Paso sin valor**: paso cuya sección falta en `.agents/local/configuracion.md`, o todos los pasos si el archivo no existe.
- **Sugerencias del kit**: `.agents/configuracion.ejemplo.md`. Viaja con el kit, como `entornos.ejemplo.md` y `preferencias.ejemplo.md`. Trae los valores sugeridos de cada paso y la tabla de equivalencias de modelos por herramienta; los valores que ya tienen fuente única en el kit los cita y no los copia.
- **Configuración de cada dev**: `.agents/local/configuracion.md`, más `.agents/local/entornos.md` (lo fijó la 0013) y `.agents/local/auditor.md`. No se versiona (`.agents/local/` está fuera de Git).
- **Nivel de modelo**: alto (planner, coordinator, reviewer), medio (implementer) y bajo (`tester`, `explore`). En Claude Code equivalen a Opus, Sonnet y Haiku: `restricciones-de-rol.md` › «Modelo por rol».
- **Adaptador de rol**: archivo `<rol>.md` dentro de la carpeta de agentes de una herramienta (`.claude/agents/`, `.cursor/agents/`, `.opencode/agents/`…), no versionado, con el mismo frontmatter que `.agents/agents/<rol>.md` salvo `model:`, los campos que la herramienta no admite y los que exige (como `mode` en OpenCode, `.agents/sync-agents.md` › «OpenCode»), y un cuerpo que solo remite a `.agents/agents/<rol>.md`.
- **Carpeta de agentes por archivo**: carpeta real de agentes de una herramienta en la que cada rol es un enlace a `../../.agents/agents/<rol>.md` o un adaptador de rol, en lugar de un único enlace de carpeta.
- **Auditor externo**: revisión de solo lectura hecha por un agente de otra familia de modelo que la sesión principal.

## Requisitos funcionales (EARS)
- RF-1: CUANDO se instala el kit, `/sdd-init` invoca el asistente después de la entrevista del proyecto, sin contar sus pasos dentro del tope de esa entrevista.
- RF-2: CUANDO se actualiza un proyecto que ya tiene el kit, EL SISTEMA invoca el asistente solo para los pasos sin valor (pasos nuevos del kit o secciones que faltan), y conserva los valores ya configurados.
- RF-3: EL SISTEMA presenta los pasos de uno en uno, cada uno con su contexto breve y su valor sugerido como primera opción.
- RF-4: SI la persona salta un paso o el asistente completo, ENTONCES EL SISTEMA aplica el valor sugerido de cada paso saltado y lo dice en una línea al final.
- RF-5: EL SISTEMA guarda cada valor elegido en un solo lugar, la configuración de cada dev, y no escribe ningún valor de la configuración en un archivo versionado. `PREFERENCIAS.md` solo remite a `.agents/local/configuracion.md`, sin copiar valores. Ninguna regla del kit copia un valor configurado.
- RF-6: EL SISTEMA ofrece como pasos, con estos valores sugeridos:
  1. Modelos por rol, por nivel: en Claude Code, los de `restricciones-de-rol.md` › «Modelo por rol». En las demás herramientas, la fila de esa herramienta en la tabla de equivalencias; en una herramienta sin fila, `inherit`.
  2. Roles que corren siempre como subagente: ninguno; rige `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol», que ya admite el subagente cuando la persona lo pide. El valor configurado es ese pedido, hecho una vez.
  3. Clarify: tope de pasadas, el de `.agents/skills/sdd/topes-y-modos.md`; y modo de corrección (RF-22), sugerido «automático».
  4. Entornos de QA: modo seguro, sin destinos de QA.
  5. Presentación de las respuestas: secciones con ícono fijo, preguntas una por una, y tabla de cascada al iniciar o retomar una fase.
- RF-7: SI un valor configurado no es válido o no se puede leer, ENTONCES EL SISTEMA usa el valor sugerido de ese paso y avisa en una línea.
- RF-8: EL SISTEMA no ofrece el auditor externo como paso del asistente, salvo que la persona lo pida o que se detecten dos herramientas de agente instaladas.
- RF-9: CUANDO el asistente corre y se detectan dos herramientas de agente, con el criterio de detección de `/sdd-init` (línea `herramientas:` de `.agents/local/kit-visto` o, sin ella, las carpetas presentes), EL SISTEMA sugiere una vez el auditor externo, marcado como beta, con la combinación probada (Claude Code como sesión principal y Cursor como auditor, en Linux); las demás combinaciones se marcan como no probadas.
- RF-10: CUANDO la persona activa el auditor externo, EL SISTEMA guarda en `.agents/local/auditor.md` quién audita (herramienta, modelo, comando) y en qué puntos del flujo, elegidos de esta lista cerrada: `clarify`, `plan`, `tasks`, `validate`, `cierre`; sugerido al activarlo: `validate`. Por omisión está apagado y el archivo no existe.
- RF-11: MIENTRAS el auditor externo esté activo, EL SISTEMA lo lanza solo en modo de solo lectura, cumpliendo `AGENTS.md` › Guardrails. Guarda su salida completa en `.agents/local/auditor/` (no versionada) y anota en `05_avance.md` quién auditó, en qué punto, la ruta de la salida y cada observación en una línea.
- RF-12: CUANDO un dev corre `/sdd-config`, EL SISTEMA muestra el valor actual de cada paso y permite cambiar uno o más; los que no toca se conservan.
- RF-13: EL SISTEMA trae en `.agents/configuracion.ejemplo.md` la tabla de equivalencias de modelos: una fila por herramienta (Cursor, Antigravity, Codex, OpenCode) y una columna por nivel; la fila de Claude Code no se copia: remite a `restricciones-de-rol.md` › «Modelo por rol». Cuando una celda ofrece dos modelos, el primero es el valor sugerido y el segundo se muestra en el paso como alternativa. El esfuerzo (effort o reasoning) de cada celda se muestra como sugerencia y el asistente no lo escribe en ningún archivo.
- RF-14: CUANDO un dev elige, en una herramienta, un modelo distinto del que trae `.agents/agents/<rol>.md` para algún rol, EL SISTEMA convierte la carpeta de agentes de esa herramienta en carpeta de agentes por archivo, con un adaptador de rol para cada rol con modelo propio y un enlace para el resto. En Claude Code, CUANDO todos los roles vuelven al modelo de `.agents/agents/`, EL SISTEMA puede restaurar el enlace de carpeta; en las demás herramientas la carpeta sigue por archivo (RF-17). En ningún caso escribe en `.agents/agents/`.
- RF-15: EL SISTEMA no escribe `model:` en `.agents/agents/`: ese frontmatter conserva siempre los valores de `restricciones-de-rol.md` › «Modelo por rol». Esto reemplaza el paso de `/sdd-init` que escribía `inherit` en los agentes cuando solo hay otras herramientas: en ese caso el modelo de cada herramienta va en sus adaptadores de rol.
- RF-16: «La tabla del proyecto» de `restricciones-de-rol.md` › «Modelo por rol» se reemplaza por la sección de modelos de `.agents/local/configuracion.md`; `restricciones-de-rol.md` queda como fuente única de los valores sugeridos de Claude Code.
- RF-17: `/sdd-enlaces` crea, corrige y reconoce la carpeta de agentes por archivo y sus adaptadores de rol. Al crear la carpeta de agentes de una herramienta distinta de Claude Code, la crea por archivo con adaptadores cuyo `model:` es el configurado o, sin configuración, el sugerido de su fila (o `inherit`); así ninguna herramienta lee un `model:` que no entiende. `/sdd-estructura` toma como válido un adaptador cuyo frontmatter cumple la definición de adaptador de rol y cuyo cuerpo remite a ese archivo; si el frontmatter del rol cambió, lo reporta como adaptador viejo.
- RF-18: `.agents/local/configuracion.md` anota en su primera línea la versión del kit con que se configuró. SI al arrancar una sesión falta el archivo, o esa versión es distinta de `.agents/.kit-version` y el ejemplo trae pasos que el archivo no tiene, ENTONCES el chequeo de arranque (`.agents/sync-agents.md` › «Chequeo de arranque») lo suma a su aviso combinado y sugiere `/sdd-config`, sin escribir nada; mientras tanto rigen los valores sugeridos.
- RF-19: Cada valor se lee solo donde se usa: la sección «Presentación», en el arranque, por la remisión de `PREFERENCIAS.md` (solo esa sección); los roles que corren como subagente, al iniciar una fase; el tope de clarify, en `/sdd-clarify`; los modelos, en `/sdd-config` y `/sdd-enlaces`; el auditor, en los puntos del flujo que tenga activos. El chequeo de arranque lee solo la primera línea (RF-18). Ningún otro comando lee `.agents/local/configuracion.md`.
- RF-20: CUANDO el paso de entornos de QA ofrece declarar destinos, EL SISTEMA muestra antes la advertencia de `entornos.ejemplo.md` › «Advertencia (S-8)» (remitiendo, sin copiarla) y pide confirmación explícita por cada destino; sin confirmación no se declara.
- RF-21: La fila `clarify` de `05_avance.md` anota «pasada n de N», donde N es el tope de quien corre la pasada. Las pasadas se cuentan por spec, no por dev: si otro dev con otro tope continúa, el conteo sigue y rige su N.
- RF-22: El modo de corrección de clarify admite dos valores:
  - «automático»: CUANDO el reviewer detecta hallazgos, el planner corrige `01_spec.md` sin pedir aprobación por pasada y la spec vuelve a clarify, hasta que una pasada no deje hallazgos o se llegue al tope. Si corregir un hallazgo exige decidir alcance o seguridad, el planner corrige con la opción más acotada, la escribe en la spec como supuesto explícito y la marca para revisión. Al terminar, EL SISTEMA muestra a la persona lo que cambió en cada pasada, los supuestos marcados y lo que quedó pendiente; las correcciones que eligió el planner quedan anotadas como decisiones en `05_avance.md`.
  - «con aprobación»: tras cada pasada EL SISTEMA pregunta a la persona si aprueba las correcciones (comportamiento actual de `/sdd-clarify`).
  En los dos modos, al llegar al tope con hallazgos pendientes rige `topes-y-modos.md`, y aprobar la spec (`Estado: aprobada`) lo decide siempre la persona.

## Requisitos no funcionales
- Solo Markdown y UTF-8 sin BOM; sin scripts (ADR 004, constitución 5). El asistente no instala hooks ni scripts.
- La instalación y la actualización nunca sobrescriben lo personalizado (ADR 007): `.agents/configuracion.ejemplo.md` se actualiza como archivo del kit, y `.agents/local/` y los adaptadores de rol nunca se tocan sin confirmación.
- Configurar el asistente no cambia ningún archivo versionado: tras instalar o reconfigurar, `git status` no muestra cambios atribuibles a la configuración.
- Nada de la configuración entra a `AGENTS.md`, que se carga en cada turno (criterio de la `0015_ArranqueLigero`, RF-1).
- Cada regla y cada valor sugerido viven en una sola fuente (constitución 3).
- Contexto de cada paso: dos líneas como máximo.

## Casos límite
- La persona contesta el primer paso y salta el resto: los saltados toman su valor sugerido.
- Proyecto actualizado que ya tenía valores distintos del sugerido: se conservan.
- Segundo dev en un proyecto ya instalado: el chequeo de arranque le sugiere `/sdd-config` (RF-18); hasta que lo corra, rigen los valores sugeridos.
- Dos devs del mismo proyecto con herramientas distintas: cada uno tiene su `.agents/local/configuracion.md` y sus adaptadores; el repo no cambia.
- Dev en Claude Code que deja todos los modelos sugeridos: `.claude/agents` sigue como enlace de carpeta.
- Kit actualizado con un paso nuevo y otro dev que no corrió la actualización: el chequeo de arranque le avisa (RF-18); hasta que corra `/sdd-config`, el paso nuevo toma su valor sugerido.
- Proyecto que ya tiene `model: inherit` en `.agents/agents/` por la regla anterior de `/sdd-init`: la actualización lo señala y propone volver a los valores de `restricciones-de-rol.md` y pasar `inherit` a los adaptadores; no lo cambia sin confirmación.
- Herramienta cuyo formato de agente no admite modelo por rol: el paso de modelos se muestra como no aplicable para ella y rige `inherit`. Qué herramientas lo admiten se comprueba en el plan.
- Adaptador de rol cuyo `.agents/agents/<rol>.md` cambió al actualizar el kit: el cuerpo nuevo le llega por remisión; si cambió el frontmatter, `/sdd-estructura` lo reporta y `/sdd-enlaces` lo regenera conservando el `model:`.
- Herramienta sin fila en la tabla de equivalencias: el paso de modelos sugiere `inherit`.
- Una sola herramienta de agente detectada: el auditor no se sugiere, pero se puede pedir.
- Dos herramientas detectadas en una combinación no probada: se sugiere como beta no probada.
- Dos devs con topes de clarify distintos en la misma spec: RF-21.
- `PREFERENCIAS.md` de un proyecto que ya trae directivas de presentación o de dónde corre cada rol: el asistente propone moverlas a `.agents/local/configuracion.md` y dejar la remisión; no las mueve sin el visto bueno de la persona.

## Fuera de alcance
- Lo que define la 0013 (comportamiento de los roles, documento de casos de prueba, reglas de QA); aquí solo se configura, salvo el reemplazo del paso de `inherit` de `/sdd-init` (RF-15) y de «la tabla del proyecto» (RF-16).
- La herramienta de apoyo para casos: es un dato del proyecto, sigue en `AGENTS.md` › Comandos y la pregunta la entrevista de `/sdd-init` (0013).
- La barra de estado: requiere hooks y scripts, que el kit no lleva; la da el kit personal.
- Aplicar el esfuerzo (effort o reasoning) en una herramienta.
- Probar el auditor en combinaciones distintas de Claude Code más Cursor en Linux.
- Reactivar la auditoría de la directiva personal en pausa.

## Criterios de finalización
- Instalar en un temporal saltando todo deja `.agents/local/configuracion.md` con los valores sugeridos y ningún aviso; con solo Claude Code, `.claude/agents` sigue como enlace de carpeta; con otra herramienta, su carpeta de agentes queda por archivo con los modelos sugeridos de su fila.
- Instalar respondiendo cada paso, con modelos distintos de los sugeridos, deja cada valor en `.agents/local/` y los adaptadores de rol, sin cambios en archivos versionados.
- Actualizar un proyecto con valores propios los conserva.
- Cada RF verificado por `/sdd-validate`; versión, changelog y manifiesto del kit actualizados.

## Dudas abiertas
(ninguna)
