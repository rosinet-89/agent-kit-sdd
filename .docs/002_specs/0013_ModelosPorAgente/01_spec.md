# 01 — Spec: Modelos por agente
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo
Hoy los cuatro agentes del kit declaran `model: inherit` (ADR 013, spec `004_ModeloYPermisosAgentes`): todo corre con el modelo de la sesión y cada fase se delega a un subagente aunque use el mismo modelo. Se eligió `inherit` porque en aquella sesión el alias `sonnet` no se podía lanzar y no hubo comparación de modelos.
Objetivo: en Claude Code, cada rol usa el modelo que le corresponde. Opus para lo que diseña, coordina, revisa o decide; Sonnet para implementar un diseño cerrado; Haiku para ejecutar pruebas y explorar, solo con encargos cerrados. Un rol con el mismo modelo que la sesión corre en el chat. Las pruebas de pantalla tienen un guion escrito antes de implementar y, por omisión, no tocan nada que pueda escribir datos. En otras herramientas el kit no fija modelo sin preguntar.

## Usuarios y roles
- Persona que usa el kit en Claude Code: quiere el reparto de modelos sin configurarlo a mano.
- Persona que usa otra herramienta (Cursor u otra): quiere que se le pregunte antes de configurar modelos.
- Persona que valida con un entorno de QA propio: quiere que las pantallas se prueben solas solo contra ese entorno.
- Roles: planner, coordinator, reviewer, implementer, y dos nuevos, `tester` y `explore`.

## Historias de usuario
- HU-1. Como persona en Claude Code, quiero que cada rol use su modelo para bajar costo y tiempo sin perder calidad en el diseño.
- HU-2. Como persona en otra herramienta, quiero que el kit me pregunte antes de fijar modelos para no romper mi entorno.
- HU-3. Como persona que valida, quiero un guion de casos escrito antes de implementar, que un rol rápido lo ejecute y que el veredicto lo siga dando un rol de diseño.
- HU-4. Como persona con entorno de QA, quiero declarar ese entorno una vez y que nunca se ejecute nada fuera de él.

## Definiciones
- **Encargo cerrado**: prompt que trae todo lo que el rol necesita y no le deja decidir nada: objetivo, alcance (rutas o destinos), pasos o casos y formato de salida.
- **Rol Haiku**: `tester` y `explore`. Son rápidos más que profundos: no diseñan, no deciden, no juzgan.
- **Tramo**: cada ejecución de un rol para una fase o un paso; corresponde a una fila de la tabla de tiempos de `05_avance.md`.
- **Mismo modelo**: misma familia (`opus`, `sonnet` o `haiku`) entre el alias de la tabla «Modelo por rol» y el modelo de la sesión, según el id que la herramienta le da a la sesión (`claude-opus-…` es `opus`). Si la sesión no conoce su id, no es el mismo modelo.
- **Documento de casos de prueba** (`07_casos_prueba.md`): documento de la carpeta del feature que reemplaza a `07_pruebas_qa.md`. Tiene tres partes: los casos (guion), las corridas (resultados) y, si se pide, la evidencia formal.
- **Caso**: RF que cubre, datos de partida, destino, pasos numerados, resultado esperado, si se ejecuta con herramienta o a mano, y si escribe datos (`sí`, `no`; la duda cuenta como `sí`).
- **Destino**: URL o host contra el que corre el caso (`esquema://host[:puerto]`), `local` (solo la máquina de la persona: `localhost`, `127.0.0.1` o `::1`) o `ninguno` (no ejecuta contra una aplicación).
- **Usar la red**: un caso usa la red si su destino no es `local` ni `ninguno`, o si su herramienta de apoyo se conecta a un host que no es local.
- **Estado de un caso en las corridas** (conjunto cerrado): `obtenido` (`tester` lo ejecutó y devuelve lo obtenido), `bloqueado` (no se pudo ejecutar; con motivo), `manual` (no lo ejecuta `tester`; con motivo), `ok` y `falla` (resultado tras comparar esperado contra obtenido, o confirmado por la persona en un caso `manual`).
- **Paso verificable por ejecución**: comprobación de un RF que exige ejecutar algo (una pantalla, un comando, una llamada) o que la persona lo haga a mano. No incluye los chequeos de lint y sintaxis de `AGENTS.md` › Comandos, que corre el reviewer.
- **Herramienta de apoyo**: MCP u otra herramienta que el proyecto declara para ejecutar pasos (por ejemplo, Playwright). Es del proyecto.
- **Configuración de entornos**: archivo `.agents/local/entornos.md`, no versionado, de cada persona, con la lista blanca de destinos de QA contra los que `tester` puede ejecutar. Es de cada dev, no del proyecto.
- **Modo seguro**: todo destino que no es `local` ni `ninguno` se trata como productivo.
- **Archivo del kit**: archivo de `template/` o de los documentos distribuibles de la raíz.

## Requisitos funcionales (EARS)

### Modelo por rol
- RF-1: EL SISTEMA define una sola tabla «Modelo por rol» en `.agents/skills/sdd/restricciones-de-rol.md`, junto a las restricciones de cada rol, con `opus` para planner, coordinator y reviewer, `sonnet` para implementer y `haiku` para `tester` y `explore`; el `model:` del frontmatter de cada agente es su aplicación y cualquier otro texto remite a la tabla sin copiarla.
- RF-2: CUANDO se comprueba la estructura del kit, EL SISTEMA verifica que el `model:` de cada agente coincide con la tabla del proyecto y avisa si no; la tabla del proyecto registra los valores que se aplicaron (los de RF-1, `inherit` o los que eligió la persona), y un agente personalizado que no coincide da aviso, no falla.
- RF-3: La tabla remite al orden de resolución del modelo en Claude Code que ya documenta `restricciones-de-rol.md` (hecho (c)) y no lo copia.
- RF-4: CUANDO un rol termina un tramo, el rol declara en su respuesta el modelo con que corrió (el id que le da la herramienta, o «modelo no confirmado» si no lo conoce), y la sesión principal lo anota en la nota de la fila de ese tramo en `05_avance.md`.
- RF-5: CUANDO `/sdd-init` instala el kit, EL SISTEMA aplica los modelos por omisión según las herramientas declaradas en el estado «visto» de `.agents/sync-agents.md` o, sin declaración, según las carpetas de herramienta presentes (`.claude/`, `.cursor/`, `.gemini/`, `.github/`, `.kiro/`): solo Claude Code, la tabla de RF-1; solo otras herramientas, `inherit`; Claude Code junto a otras, la tabla de RF-1 (manda Claude Code). Las preguntas para elegir otros valores las hace el asistente de `0014_ConfiguracionGuiada`; esta spec no agrega preguntas a `/sdd-init`.
- RF-6: CUANDO se actualiza un proyecto que ya tiene el kit, EL SISTEMA aplica a un agente KIT-VIEJO confirmado los mismos valores por omisión de RF-5, y no cambia el modelo de un agente personalizado.

### Dónde corre cada rol
- RF-7: CUANDO una fase pide un rol con el mismo modelo que la sesión, EL SISTEMA lo ejecuta en el chat con la definición de ese rol, incluido el reviewer que revisa o valida lo producido en el mismo chat; solo lanza un subagente cuando el rol necesita otro modelo o la persona lo pide. La separación entre producir y validar es por rol: el chat cambia de rol, lo muestra, y nunca valida con el rol con que produjo.
- RF-8: EL SISTEMA permite lanzar `tester` y `explore` a quien atiende la fase como sesión (la sesión principal, o el coordinator cuando corre como sesión); SI el coordinator corre como subagente, ENTONCES no los lanza y los lanza la sesión principal.
- RF-9: SI el coordinator del proyecto está personalizado y no lista `tester` o no lista `explore`, ENTONCES EL SISTEMA no lo reemplaza y avisa, por cada rol que falte, que no podrá lanzarlo hasta que la persona lo agregue.

### Roles Haiku
- RF-10: EL SISTEMA ofrece el rol `explore` en `haiku`, de solo lectura (sin escribir archivos ni usar shell ni red), que localiza archivos y ocurrencias y devuelve lo que pide el formato de salida. Como subagente, esa solo lectura la garantizan sus herramientas; en el chat (sesión con su mismo modelo), es instrucción.
- RF-11: EL SISTEMA ofrece el rol `tester` en `haiku`, con lectura, shell para los comandos de los casos y las herramientas de apoyo declaradas, sin herramientas de escritura de archivos ni de red propias; ejecuta los casos que recibe y devuelve por caso el estado `obtenido`, `bloqueado` o `manual`, sin marcar `ok` o `falla` y sin emitir veredicto. Como subagente, la falta de escritura de archivos y de red la garantizan sus herramientas; no escribir por shell es instrucción, y en el chat todo es instrucción.
- RF-12: ANTES de lanzar un rol Haiku, quien lo lanza (la sesión principal, o el coordinator cuando atiende la fase como sesión) comprueba que el encargo trae objetivo, alcance, pasos o casos y formato de salida, y que no pide diseñar, decidir, elegir ni juzgar; SI falta algo o lo pide, ENTONCES no lo lanza así: completa el encargo o hace esa parte él. El rol Haiku repite la comprobación como segunda barrera, que es solo instrucción: SI falta algo, responde `ENCARGO INCOMPLETO`, dice qué falta y no trabaja. SI un caso concreto no trae destino o datos de partida, ENTONCES ese caso queda `bloqueado` con el motivo y el resto del encargo sigue.
- RF-13: SI el encargo de un rol Haiku pide diseñar, decidir, elegir o juzgar, ENTONCES el rol responde `ENCARGO FUERA DE ROL`, dice qué parte lo es y no la hace.
- RF-30: SI un rol Haiku no entrega lo pedido (no responde; devuelve fuera del formato de salida; se sale del alcance; rellena con supuestos lo que faltaba; o responde `ENCARGO INCOMPLETO` o `ENCARGO FUERA DE ROL` a un encargo que quien lanza ya comprobó), ENTONCES quien lo lanzó descarta ese resultado y relanza una sola vez el mismo rol, con el mismo encargo y el modelo del implementer según «Modelo por rol»; SI el relanzamiento también falla, ENTONCES la sesión principal hace la tarea en el chat con la definición de ese rol, y sus restricciones pasan a ser instrucción. Cada intento queda en la Nota de la fila del tramo, con su modelo (RF-4).
- RF-14: SI durante una corrida la herramienta de apoyo no responde, un paso no se puede ejecutar o falta la variable de entorno de una credencial (o está vacía), ENTONCES `tester` devuelve el caso como `bloqueado` con el motivo, sin inventar resultados.
- RF-15: EL SISTEMA da de alta las restricciones de `tester` y `explore` en la misma fuente que las de los otros roles, indicando cuáles garantiza la herramienta y cuáles son solo instrucción; suma el documento de casos de prueba a los que escribe el planner, y la parte de corridas a lo que escribe quien atiende la fase como sesión (incluido el coordinator).

### Documento de casos de prueba
- RF-16: CUANDO un feature tiene pasos verificables por ejecución, el planner genera en `tasks`, antes de implementar, el documento de casos de prueba con un caso por comprobación, derivado de los RF. Solo aplica a features que lleguen a `tasks` después de instalar o actualizar el kit; un feature con `tasks` ya aprobadas sigue como estaba.
- RF-17: SI el proyecto declara una herramienta de apoyo, ENTONCES el planner redacta los pasos con las acciones de esa herramienta (abrir URL, llenar el campo con id, pulsar, comparar texto); SI no declara ninguna, ENTONCES los redacta para una persona y el caso es manual.
- RF-18: EL SISTEMA marca en cada caso su destino y si escribe datos; un caso que usa una aplicación con base de datos (incluido iniciar sesión o navegar) se marca como que escribe, y la duda también.
- RF-19: EL SISTEMA no guarda valores de credenciales en el documento, en el encargo ni en la configuración de entornos: solo el nombre de la variable de entorno que los tiene. El valor lo lee la herramienta de apoyo del entorno; el agente nunca lo lee, imprime ni copia.
- RF-20: CUANDO se valida, quien atiende la fase como sesión lanza `tester` con los casos ejecutables antes del reviewer, anota sus resultados en las corridas y marca `ok` o `falla` cuando la comparación entre esperado y obtenido es literal; si requiere juicio, la deja al reviewer. El reviewer emite el veredicto con esas corridas. La corrida de `tester` y la del reviewer forman una sola vuelta del tope de validate.
- RF-21: EL SISTEMA anota los resultados de los casos y las confirmaciones manuales de la persona solo en el documento de casos de prueba cuando existe, y el avance lleva solo un enlace; sin documento, se anotan en el avance como hoy.
- RF-22: SI un feature no tiene pasos verificables por ejecución, ENTONCES no se crea el documento; SI aun así se pide evidencia formal, ENTONCES se crea solo con esa parte, y el harness de solo lectura de `tests/` se conserva.
- RF-23: SI la carpeta de un feature anterior tiene `07_pruebas_qa.md`, ENTONCES EL SISTEMA lo conserva sin renombrarlo y el reviewer lo lee como evidencia en validate.

### Entornos: modo seguro y QA
- RF-24: EL SISTEMA trabaja en modo seguro por omisión: `tester` solo ejecuta casos que no escriben datos ni usan la red; el resto queda `manual` con el motivo.
- RF-25: SI la persona declara destinos de QA en su configuración de entornos, ENTONCES `tester` puede ejecutar, solo contra esos destinos, casos de pantalla o de llamada a la aplicación que escriben datos o usan la red; nunca sentencias contra una base de datos, scripts de `sql/` ni comandos de shell que escriban. Esta es la única excepción a `AGENTS.md` › Guardrails: queda escrita en la regla 1, el preámbulo de Guardrails admite solo las excepciones escritas en sus reglas, y `restricciones-de-rol.md` suma la excepción de red solo para `tester` contra esos destinos.
- RF-26: ANTES de cada caso, y en cada navegación o llamada durante el caso, `tester` compara el destino con la lista blanca; SI la configuración no existe, está mal formada o el destino no coincide exactamente (mismo esquema, host y puerto; sin puerto, el del esquema; la ruta no cuenta), ENTONCES no lo ejecuta o lo detiene, y el caso queda `manual` con el motivo.
- RF-27: EL SISTEMA guarda la configuración de entornos en `.agents/local/entornos.md`, fuera de Git y por persona, con una sección `## QA` y una línea `- esquema://host[:puerto]` por destino; el kit trae el ejemplo `.agents/entornos.ejemplo.md`, que advierte que declarar un destino afirma que su base de datos es de QA. Nunca contiene valores de credenciales.
- RF-28: EL SISTEMA lee la herramienta de apoyo del proyecto en `AGENTS.md` › Comandos (por omisión, «No aplica»); la pregunta para declararla la hace el asistente de `0014_ConfiguracionGuiada`. Agregar las herramientas de apoyo a `tester.md` no lo vuelve personalizado: la actualización conserva esas herramientas al reemplazarlo.

### Decisión
- RF-29: EL SISTEMA registra un ADR nuevo que reemplaza al ADR 013: modelos por rol, rol en el chat salvo cambio de modelo (también el reviewer; la separación entre producir y validar es por rol), Sonnet para implementer por decisión de la persona sin comparación previa (se mide en uso real y se revisa tras 3 features), y la excepción de QA de la regla 1.

## Requisitos no funcionales
- Solo Markdown y UTF-8 sin BOM; sin scripts (ADR 004).
- Un proyecto que ya tiene el kit no pierde lo personalizado: la actualización solo reemplaza archivos KIT-VIEJO confirmados (ADR 007).
- Los agentes nuevos cumplen `AGENTS.md` › Guardrails; la única excepción es la de RF-25.
- Esta spec no agrega preguntas a `/sdd-init`; las de modelos y herramienta de apoyo son de `0014_ConfiguracionGuiada`.
- Ningún archivo del kit nombra `07_pruebas_qa.md`, salvo la nota de compatibilidad de RF-23 en la plantilla del documento de casos de prueba y en el procedimiento de validate (`/sdd-validate` y reviewer).

## Casos límite
- Alias de un modelo ausente en la sesión (causa del ADR 013): Claude Code cae al heredado con aviso; el tramo queda anotado con el modelo que declara el rol (RF-4).
- `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1`: el frontmatter se ignora; el tramo se anota con el modelo que declara el rol.
- La sesión corre en `haiku`: `tester` y `explore` corren en el chat y sus restricciones son instrucción (RF-10, RF-11).
- `explore` convive con el agente incorporado `Explore` de Claude Code (riesgo aceptado, decisión 6).
- `tester` recibe un caso sin destino o sin datos de partida: ese caso queda `bloqueado` (RF-12).
- La URL de QA redirige a un host fuera de la lista blanca: el caso se detiene y queda `manual` (RF-26).
- Una página de QA llama en segundo plano a otro host: riesgo asumido por la persona al declarar el destino (S-8).
- La configuración de entornos tiene una línea que no es `esquema://host[:puerto]` o un destino repetido: formato roto, modo seguro.
- Un destino de QA cuya aplicación usa una base de datos productiva o compartida: `tester` no puede saberlo; declarar el destino lo afirma la persona (RF-27).
- Proyecto en modo seguro con todos sus casos de pantalla: `tester` no ejecuta ninguno y la persona los hace a mano con el mismo guion.
- La herramienta no permite elegir el modelo al lanzar un subagente: no hay relanzamiento con el modelo del implementer y, tras el primer fallo, la sesión principal hace la tarea (RF-30).
- La sesión principal hace una tarea de `tester` tras dos fallos: aplica `tester.md` con modo seguro, lista blanca y la excepción de QA, que la regla 1 da al rol y no al modelo. Sus restricciones son instrucción (RF-30).
- Un rol Haiku devuelve un resultado útil, pero rellenó con un supuesto lo que faltaba: cuenta como falla y el resultado no se usa (RF-30).

## Fuera de alcance
- El modelo de la sesión principal (lo elige la persona con `/model`).
- Corlos, Claudio y el auditor externo (en pausa).
- Comparación de modelos antes de implementar.
- Renombrar o convertir un `07_pruebas_qa.md` existente.
- Nuevos roles distintos de `tester` y `explore`.
- Las preguntas de `/sdd-init` para elegir modelos y herramienta de apoyo (`0014_ConfiguracionGuiada`).

## Criterios de finalización
- Los seis agentes declaran el modelo de la tabla y `/sdd-estructura` lo comprueba sin avisos.
- Lanzar `explore` en Claude Code llega al agente del kit y corre en `haiku`, comprobado en validate con el modelo que registra el transcript del subagente y el que declara el rol (RF-4).
- En validate, un encargo incompleto no se lanza a un rol Haiku (RF-12), y un fallo de un rol Haiku se relanza una vez con el modelo del implementer y después pasa a la sesión (RF-30).
- El documento de casos de prueba tiene plantilla y lo usan `tasks`, `tester` y `validate`.
- La regla 1 de Guardrails tiene la excepción de QA, el preámbulo la admite y el kit trae `.agents/entornos.ejemplo.md`.
- ADR nuevo aceptado y ADR 013 marcado como reemplazado.
- Cada RF verificado por `/sdd-validate`; versión, changelog y manifiesto del kit actualizados.

## Dudas abiertas
Ninguna. El `[ACLARAR]` de la pasada 3 se resolvió como supuesto S-8.

Supuestos explícitos (cierre del tope de clarify, pasada 3 de 3; los aprueba la persona con la spec):
- S-1 (RF-28): las «herramientas de apoyo» de `tester.md` son las entradas de `tools:` que empiezan por `mcp__`; si `tester.md` solo difiere del kit en esas entradas, no cuenta como personalizado y la actualización las conserva.
- S-2 (RF-5): `.github/` cuenta como otra herramienta solo si tiene `copilot-instructions.md` o la carpeta `agents/`.
- S-3 (RF-5): el estado «visto» es el archivo `.agents/local/kit-visto` que define `.agents/sync-agents.md`.
- S-4 (RF-7): el chat muestra el rol en la línea «quién responde» de la skill `formato-respuesta` (y en la barra, si la persona la tiene).
- S-5 (RF-20): una comparación es literal cuando el resultado esperado es un texto o valor exacto y basta compararlo carácter por carácter con lo obtenido.
- S-6 (RF-1 y RF-2): RF-1 describe la tabla del kit en `template/`; la del proyecto parte de ella y registra lo aplicado.
- S-7 (RF-6): si la tabla de `restricciones-de-rol.md` del proyecto está personalizada, la actualización no cambia el modelo de los agentes KIT-VIEJO reemplazados: aplica el de esa tabla.
- S-8 (RF-26 y RF-27, decisión de la persona): una página del destino de QA puede hacer llamadas en segundo plano a otros hosts (APIs de terceros o un API productivo) que `tester` no comprueba; es riesgo asumido: declarar un destino de QA afirma que todo lo que esa página llama es de QA, y `.agents/entornos.ejemplo.md` lo advierte. `tester` comprueba solo sus navegaciones y llamadas propias.
