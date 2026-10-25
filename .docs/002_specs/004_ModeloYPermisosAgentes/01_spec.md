# 01 — Spec: Modelo por rol y permisos reales del frontmatter de los agentes
Estado: aprobada
Ticket: sin ticket

## Contexto y objetivo
La `003_OptimizarArnes` redujo lo que cada rol lee, pero dejó fuera dos asuntos de los agentes
(`coordinator`, `planner`, `implementer`, `reviewer`) que atiende esta spec (deuda
`003_ModeloYPermisosDeAgentes`). La verificación contra la documentación de subagentes de Claude Code
(2026-10-25, decisión 2 de `05_avance.md`) y una segunda lectura el mismo día dieron estos hechos:

- Ningún agente declara `model`: todos usan el de la sesión. Claude Code admite `model` por subagente
  (alias, ID completo o `inherit`). Orden de resolución: parámetro de la llamada, luego `model` del
  frontmatter, luego `CLAUDE_CODE_SUBAGENT_MODEL`, luego el modelo de la sesión; con
  `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` se ignora el frontmatter. Un alias bloqueado por la organización
  cae a la versión permitida o al modelo heredado, con aviso (no es un error).
- Los 4 agentes declaran `mode` y `permissions` (reglas action/resource/effect). Claude Code **no** los
  soporta y los ignora sin aviso (se detecta con `claude --debug`). Las restricciones que hoy solo
  existen en esas reglas no se cumplen por la herramienta, solo porque la instrucción de texto las dice:
  - `reviewer`: no debe modificar código, pero tiene `Write` en `tools` (y `Bash`).
  - `planner`: debe escribir solo en `.docs/002_specs/**`, pero nada se lo impide.
  - `coordinator`: debe editar solo `05_avance.md` y usar el shell solo para `date`, pero tiene
    `Write`, `Edit` y `Bash` sin límite.
  - `implementer`: tiene el shell abierto y nada impide `git commit`, `push` o `fetch`, que
    `AGENTS.md` › Git prohíbe.
  - `reviewer`: cada comando de lint pide confirmación (las reglas que lo evitarían se ignoran).
  - Los cuatro: `webfetch: deny` (y el equivalente de búsqueda web) aparentaba prohibir la red; y el
    "SQL solo lectura" del `planner` solo lo dice el texto.
- `tools` es una lista permitida y es el control de herramientas garantizado por subagente.
  `disallowedTools` también existe, pero **no acepta patrones con argumentos**: una entrada como
  `Bash(git push *)` quita la herramienta Bash entera, así que no sirve para bloquear solo comandos de
  Git. Bloquear `git commit/push/fetch` exige una regla `permissions.deny` (p. ej. `Bash(git commit *)`)
  en archivos de settings; vale para toda la sesión, no por subagente, y cubre comandos compuestos. El
  `.claude/` del kit está en el `.gitignore` y no se versiona; por eso la regla va como fragmento
  opcional documentado.
- `Agent(tipo1, tipo2)` en `tools` solo se aplica a un agente que corre como hilo principal con
  `claude --agent`. En un subagente la lista entre paréntesis se ignora y `Agent` permite lanzar
  subagentes hasta 3 capas. `planner`, `implementer` y `reviewer` no listan `Agent`, así que no pueden
  lanzar subagentes (garantizado por `tools`).
- Por defecto el subagente carga `CLAUDE.md` y, a través de su importación, `AGENTS.md` completo
  (8.403 bytes en `template/`, ~2.100 tokens por arranque en frío) aunque la `003` fijó que cada rol
  lea solo las secciones que usa. El campo `omitClaudeMd` lo evita, pero quita **todo** el `CLAUDE.md`
  (el del proyecto y su jerarquía), no solo `AGENTS.md`: si un proyecto no importa `AGENTS.md` desde su
  `CLAUDE.md`, el ahorro es cero. También quita las reglas de Git y las reglas duras que el rol sí debe
  cumplir.
- El kit replica los agentes hacia Cursor, Gemini y Copilot. Lo que hacen `mode`, `permissions`,
  `model` y `omitClaudeMd` en esas herramientas **no está verificado**; `model` y `omitClaudeMd` solo
  están documentados para Claude Code.
- La documentación se leyó completa el 2026-10-25. Además: los `hooks` por subagente existen
  (`PreToolUse` con `matcher: "Bash"` y exit code 2 para bloquear), pero su ejemplo usa un script
  externo; el ejemplo de la documentación para un revisor de solo lectura usa `tools: Read, Grep, Glob,
  Bash`, sin `Edit` ni `Write`; las `description` de los subagentes comparten un presupuesto conjunto de
  15.000 tokens; `omitClaudeMd` no afecta a los `CLAUDE.md` gestionados por política.

Objetivo: bajar el costo por feature por la vía del **modelo** y de **qué carga cada subagente**, y
hacer que cada restricción de rol tenga un mecanismo que la herramienta sí aplique o que quede dicha
como no garantizada, **sin bajar la calidad** del flujo (mismo principio de la `003`).

Decisiones de contexto (ya tomadas):
- Se atiende por el flujo SDD (principio 2). Alternativa descartada: editar el frontmatter sin spec.
- El kit sigue siendo solo Markdown, sin scripts versionados (ADR 004), genérico (principio 1) y en
  español.
- Se parte del contenido actual de los agentes (la `003` ya reescribió sus cuerpos) y no se contradice
  la `003` ni sus ADR (en especial ADR 009: el procedimiento de una fase vive solo en su comando
  `/sdd-<fase>`; el agente nombra y enlaza). El procedimiento de actualización y los manifiestos
  (ADR 007 y 008) no cambian.
- Orden con la `003`: la `003` **cierra primero** (la cierra otro agente del responsable) y esta spec
  parte de ese estado; esta spec cambia los agentes y regenera el manifiesto de 0.5.0 al final. No se
  toca la carpeta de la `003`.
- Decisiones del responsable en la clarificación y después (no se reabren):
  1. Restricciones que `tools` no cubre: `tools` donde alcance, más instrucción marcada "no
     garantizada" en la tabla de restricciones, más una regla `deny` OPCIONAL documentada para la
     configuración local. Sin hooks ni código ejecutable versionado; no se enmienda el ADR 004.
  2. Modelo: `implementer` y `coordinator` declaran `model: sonnet` (valor documentado por la
     herramienta); `planner` y `reviewer` declaran `inherit`. Si el alias no está disponible, cae al
     modelo heredado con aviso. Cada cambio se acepta solo con la comparación de RF-4; el rol que no
     pase conserva el modelo actual.
  3. `omitClaudeMd: true` en `planner`, `reviewer` y `coordinator` es FIRME: la medición documenta el
     neto, pero no lo revierte salvo regresión de calidad medida. No en `implementer` (depende de las
     reglas de Git y las reglas duras de `AGENTS.md`), salvo la condición de RF-11.
  4. `mode` y `permissions` se eliminan de los 4 agentes; se anota en el registro de RF-1 que son
     formato de otra herramienta, ignorado por Claude Code.
  5. `reviewer` sin `Write` ni `Edit`. Evidencia formal: la sesión principal escribe
     `07_pruebas_qa.md` y el harness de `tests/`; el reviewer, en una segunda invocación, ejecuta el
     harness en modo lectura. Esa invocación cuenta como una de las 2 vueltas de `/sdd-validate`.
  6. Versión: dentro de la 0.5.0 sin publicar.
  7. El "no pide confirmación en comandos de verificación" no es una restricción de rol: va como
     requisito aparte (RF-10). Que `/sdd-init` sugiera reglas `allow` queda fuera de alcance.

## Usuarios y roles
- **Responsable del proyecto**: quiere menos costo por feature con la misma calidad, y confianza en que
  los roles no hacen lo que no deben.
- **Agentes**: reciben un frontmatter que la herramienta realmente aplica.
- **Mantenedor del kit**: debe saber qué campos honra cada herramienta y qué restricciones están
  garantizadas.
- **Proyectos que ya tienen el kit**: reciben el cambio por el procedimiento de actualización.

## Historias de usuario
- HU-1. Como responsable, quiero que los roles de trabajo mecánico usen un modelo más barato y los de
  juicio conserven el fuerte, para bajar el costo sin bajar la calidad.
- HU-2. Como responsable, quiero que las restricciones de cada rol (reviewer sin escribir archivos,
  implementer sin commit, etc.) las haga cumplir la herramienta cuando pueda, y saber cuáles no, para
  no depender solo de que el texto se obedezca.
- HU-3. Como mantenedor, quiero saber qué campos del frontmatter honra cada herramienta, para no dejar
  reglas que se ignoran sin que nadie lo sepa.
- HU-4. Como responsable, quiero que el subagente no cargue lo que su rol no usa, para no pagar en cada
  arranque la lectura de `AGENTS.md` completo.
- HU-5. Como responsable, quiero poder comprobar con evidencia que cambiar de modelo y de arranque no
  bajó la calidad, para aceptar el cambio.
- HU-6. Como responsable de un proyecto ya instalado, quiero recibir el cambio sin perder
  personalizaciones, para actualizar con el procedimiento de siempre.

## Definiciones
- **Campo soportado**: campo del frontmatter que la herramienta documenta y aplica. **Campo ignorado**:
  el que la herramienta descarta sin aviso (hoy en Claude Code: `mode`, `permissions`).
- **Restricción de rol**: algo que el rol no debe hacer o solo debe hacer acotado (lista en RF-7).
- **Garantizada**: la herramienta la impide aunque la instrucción de texto no se obedezca. **No
  garantizada**: solo la dice el texto del agente (o una regla local opcional).
- **Arranque en frío** de un rol: lo que se carga al lanzarlo antes de leer lo que pide su instrucción:
  el texto del agente, el `CLAUDE.md`/`AGENTS.md` heredados y el encargo. Se mide en bytes (bytes ÷ 4 ≈
  tokens, solo referencia), igual que en la `003`.
- **Pasos de referencia**: los tres de `001_ActualizarKit` fijados en la `003` (menor, mediana y mayor
  lectura mínima), reutilizados para comparar antes y después.
- **Modelo actual** (para RF-4): el modelo de la sesión que ejecuta la medición; se anota su nombre
  exacto en `05_avance.md` al medir. Es el que hoy usan los agentes por heredar.
- **Código ejecutable** (definición de la `003`): incluye `tools`, `mode` y la configuración con
  permisos del frontmatter. Por eso este cambio, aunque sea `.md`, cuenta como tal para la revisión de
  seguridad.
- **Pieza de restricciones**: el único documento donde viven el registro de campos (RF-1), la tabla de
  restricciones (RF-7) y los fragmentos opcionales (RF-9, RF-10). Ver RF-7.

## Requisitos funcionales (EARS)

Verificación y registro de campos
- RF-1: CUANDO se decida el frontmatter de un agente, EL SISTEMA PARTIRÁ de un registro de campos
  verificados que indique, por campo del frontmatter, si Claude Code lo soporta o lo ignora, con fecha y
  fuente (la documentación de subagentes y de permisos de Claude Code, leída el 2026-10-25). El registro
  incluye `mode` y `permissions` como formato de otra herramienta, ignorado por Claude Code, y estos
  hechos verificados el 2026-10-25: (a) `Agent(tipo1, tipo2)` en `tools` solo se aplica al agente que
  corre como hilo principal con `claude --agent`; en un subagente se ignora la lista y `Agent` permite
  lanzar subagentes hasta 3 capas; (b) `disallowedTools` no acepta patrones con argumentos y una entrada
  `Bash(git push *)` quita Bash entero; bloquear comandos exige `permissions.deny` en settings, que vale
  para toda la sesión y cubre comandos compuestos; (c) el orden de resolución del modelo (parámetro de la
  llamada, `model` del frontmatter, `CLAUDE_CODE_SUBAGENT_MODEL`, modelo de la sesión;
  `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` ignora el frontmatter) y el alias bloqueado o ausente que cae al
  modelo heredado con aviso; (d) `omitClaudeMd` quita todo el `CLAUDE.md`, no solo `AGENTS.md`. SI un
  campo que se vaya a usar no está verificado en la documentación, ENTONCES se registra como no
  verificado y no se usa.
- RF-2: CUANDO se documente qué hace cada campo en otras herramientas del kit (Cursor, Gemini, Copilot),
  EL SISTEMA LO MARCARÁ como "no verificado" salvo que se verifique con su documentación; no se asume.
  Hoy `mode`, `permissions`, `model` y `omitClaudeMd` están "no verificado" en esas herramientas.

Modelo por rol
- RF-3: CUANDO se lance un subagente, EL SISTEMA APLICARÁ el modelo declarado para su rol: cada uno de
  los 4 agentes declarará su `model` de forma explícita, para que la elección sea visible y no dependa
  del orden de resolución por defecto. Asignación: `implementer` y `coordinator` con `model: sonnet`
  (trabajo mecánico o acotado por un paso ya definido); `planner` y `reviewer` con `model: inherit`
  (juicio: diseño y revisión). `sonnet` es un valor que la herramienta documenta, por lo que cumple
  RNF-3. La asignación de `implementer` y `coordinator` queda sujeta a RF-4.
- RF-4: CUANDO se proponga cambiar el modelo de un rol, EL SISTEMA EXIGIRÁ evidencia de no regresión
  antes de aceptarlo. Aplica solo al `implementer` y al `coordinator` (`planner` y `reviewer` son
  `inherit`: no cambian de modelo y no pasan por RF-4). Procedimiento:
  - Casos. `implementer`: los 3 pasos de referencia. `coordinator`: una lista fija de 5 peticiones de
    enrutado, escrita en `05_avance.md` antes de medir: (1) petición libre de análisis nuevo; (2) spec
    aprobada que pide el plan; (3) petición de implementar sin plan aprobado, que debe frenar en la
    puerta de aprobación; (4) petición de validar un feature con implementación terminada; (5) proyecto
    con `SDD-INIT-PENDIENTE`, que no debe ejecutar comandos `/sdd*`. Por caso se anota qué comando elige
    y si respeta las puertas de aprobación.
  - Estado del repo. El `implementer` re-ejecuta cada paso sobre una copia desechable del repo en el
    estado previo a ese paso, fuera del árbol de trabajo real; lo que escriba allí se descarta y no se
    versiona. El `coordinator` se prueba sin escribir en el repo real: se compara la decisión de
    enrutado, no los documentos.
  - Corridas. Una corrida por caso y por modelo (modelo propuesto y modelo actual). Si el modelo
    propuesto falla un caso, se repite una vez para descartar azar; si vuelve a fallar, el rol no pasa.
  - Criterio de calidad. Se compara contra el "Hecho cuando" de cada paso (implementer) o contra la
    respuesta esperada de cada petición (coordinator). Se acepta solo si el modelo propuesto cumple todo
    lo que cumplía el actual.
  - Umbral de costo. Por caso, los bytes que la instrucción manda leer y los bytes de la respuesta del
    modelo propuesto no superan el 125 % de los del actual, y sus reintentos no son más que los del
    actual (los tokens reales por subagente no se pueden medir; no hay un número de tokens que exigir).
  - La evidencia, con el nombre exacto del modelo actual y el del propuesto, queda en `05_avance.md`.
  SI un rol no pasa la comparación, ENTONCES conserva el modelo actual (hereda).
- RF-5: CUANDO el modelo declarado no exista en la herramienta o en el plan del usuario, o lo bloquee la
  organización, EL SISTEMA LO DOCUMENTARÁ como riesgo con lo verificado: el alias cae a la versión
  permitida o al modelo heredado con un aviso, no es un error. El valor de respaldo es `inherit`. Si la
  medición de RF-4 se hizo con un alias que cayó al heredado, esa corrida no sirve y se repite. CUANDO
  otra herramienta del kit lea el agente, el valor de `model` queda como no verificado (RF-2) y el kit
  no depende de él para ninguna restricción ni para la calidad.

Campos ignorados y restricciones
- RF-6: EL SISTEMA NO DEJARÁ en el frontmatter de ningún agente un campo que Claude Code ignora sin que
  quede dicho qué herramienta lo usa (si alguna) y que Claude Code no lo aplica. El lugar fijo donde se
  anota es el registro de RF-1 (en la pieza de restricciones). `mode` y `permissions` se **eliminan** de
  los 4 agentes. La tabla de RF-7 sustituye lo que esas reglas aparentaban, sin que quede una regla que
  aparente proteger algo que no protege.
- RF-7: EL SISTEMA TENDRÁ una tabla única de restricciones de rol con tres columnas: restricción,
  mecanismo y si está garantizada. Vive en una sola pieza, la pieza de restricciones:
  `.agents/skills/sdd/restricciones-de-rol.md`, junto a `avance-y-tiempos.md`. Cae en el **grupo A** de
  `ACTUALIZAR.md` (bajo `.agents/`): en un proyecto instalado se ofrece como NUEVO (no existe en la
  base) y los agentes la enlazan. Consecuencia para RNF-4 y para proyectos ya instalados: si el
  responsable no incorpora el documento, el enlace del agente queda roto pero el agente sigue
  funcionando; el `CHANGELOG.md` pide incorporarlo junto con los agentes. Cubre como mínimo:
  1. `reviewer` sin `Write` ni `Edit`: GARANTIZADA por `tools`. Que no modifique archivos por Bash
     (`sed -i`, redirecciones, `tee`): NO garantizada, instrucción de texto del agente.
  2. `planner` escribe solo los cinco documentos de su carpeta de feature: NO garantizada (tiene
     `Write`/`Edit`); instrucción más `deny` opcional.
  3. `coordinator` escribe solo `05_avance.md` y usa el shell solo para la hora: NO garantizada;
     instrucción.
  4. `implementer` no hace `git commit`, `push`, `fetch`, `pull` ni `sync`, y no ejecuta SQL: NO
     garantizada; instrucción más `deny` opcional (`disallowedTools` no sirve, RF-9).
  5. `planner`, `reviewer` y `coordinator`, que conservan `Bash`, tampoco registran en Git: NO
     garantizada; instrucción más `deny` opcional.
  6. Ningún subagente lanza otros subagentes salvo el `coordinator` a los tres roles: GARANTIZADA para
     `planner`, `implementer` y `reviewer` (no listan `Agent`). Para el `coordinator`, la lista
     `Agent(planner, implementer, reviewer)` solo restringe cuando corre como hilo principal; como
     subagente se ignora y podría lanzar subagentes hasta 3 capas: NO garantizada en ese caso.
  7. Ningún agente usa la red (`webfetch` y búsqueda web denegados): GARANTIZADA por `tools`, que no
     lista esas herramientas.
  8. `planner` consulta datos solo en modo lectura, con el comando de `AGENTS.md` › Comandos: NO
     garantizada (tiene `Bash`); instrucción.
  Cada fila nombra un mecanismo que la herramienta aplica, o dice "no garantizada" con la instrucción
  de texto que la respalda.
- RF-8: CUANDO una restricción de RF-7 se pueda garantizar con campos soportados por agente (`tools`,
  `disallowedTools` aplicado a una herramienta entera, u otro campo verificado en RF-1), EL SISTEMA LA
  GARANTIZARÁ así, sin ampliar nunca un permiso. En particular, el `reviewer` pierde `Write` (y no tiene
  `Edit`), de modo que "sin `Write`/`Edit`" queda garantizada por `tools`. No se usa `permissionMode`
  para ampliar permisos, porque contradeciría que nunca se amplía el permiso sin confirmación del
  responsable.
- RF-9: CUANDO una restricción no se pueda garantizar por campos del subagente (escritura acotada por
  ruta del `planner` y del `coordinator`; que el `implementer` no haga `git commit`, `push` ni `fetch`),
  EL SISTEMA dejará en la tabla de RF-7 la instrucción de texto marcada "no garantizada" y documentará,
  en la pieza de restricciones, un fragmento OPCIONAL de `permissions.deny` (p. ej. `Bash(git commit *)`)
  para la configuración local (settings; `.claude/` no se versiona), con la advertencia de que vale para
  toda la sesión y no por subagente. `disallowedTools` no se usa para bloquear comandos (quita la
  herramienta entera, RF-1). Es recomendación, no garantía del kit. Los hooks por subagente se
  descartan (exigen un script externo versionado, ADR 004, que no se enmienda).
- RF-10: CUANDO el `reviewer` deba correr los comandos de verificación de `AGENTS.md` › Comandos, EL
  SISTEMA DOCUMENTARÁ en la pieza de restricciones un fragmento OPCIONAL de `permissions.allow` para la
  configuración local del responsable, para que esos comandos no pidan confirmación uno por uno. No es
  una restricción de rol y no tiene columna "garantizada". Ningún agente lo aplica ni cambia su modo de
  permisos: solo el responsable lo copia a su configuración. Sugerirlo desde `/sdd-init` queda fuera de
  alcance.

Arranque de subagentes
- RF-11: CUANDO se lance cada subagente, EL SISTEMA DECLARARÁ `omitClaudeMd: true` en `planner`,
  `reviewer` y `coordinator` (decisión firme), y no en el `implementer`. El `implementer` solo lo
  adopta si, medido en los 3 pasos de referencia, se cumplen las dos condiciones: (a) el neto en bytes
  es una reducción y (b) las reglas de Git y las reglas duras le siguen llegando por su instrucción o
  por la delegación, comprobado porque el paso no intenta registrar en Git ni sale del plan. La
  comparación por rol es: bytes ahorrados en el arranque en frío, y las reglas que el rol necesita y
  dejaría de recibir. SI un rol necesita una regla que `omitClaudeMd` le quita, ENTONCES esa regla llega
  por la instrucción del agente o por la delegación de la sesión principal, sin duplicar su redacción
  (una sola fuente; se nombra y enlaza). SI la herramienta no permite cargar solo secciones de
  `AGENTS.md`, ENTONCES el costo de cargarlo completo se registra como límite conocido.
- RF-12: CUANDO un rol no cargue `AGENTS.md` por defecto, EL SISTEMA CONSERVARÁ lo que hoy ya hace la
  `003`: el rol lee solo las secciones de `AGENTS.md` que su instrucción nombra (`AGENTS.md` › "Qué lee
  cada rol"), y el `coordinator` sigue pudiendo detectar `SDD-INIT-PENDIENTE`. Con `omitClaudeMd`,
  `planner` y `reviewer` (y el `coordinator`) pierden estas reglas de `AGENTS.md`: de "Git", no hacer
  `git commit` sin confirmación, no `push`/`pull`/`fetch`/`sync`, un commit por módulo con `git add`
  explícito y el formato del mensaje; y de "Reglas duras", no secretos en archivos versionados, los
  cambios fuera del plan se consultan, las entradas de `MEMORY.md` se proponen y no se agregan, y no
  reinstalar el kit. Siguen llegando porque el agente nombra esas dos secciones entre las que lee (se
  amplía "Qué lee cada rol" para ellos) o porque la delegación las trae, sin copiar su redacción. El
  cambio de carga no puede dejar a ningún rol sin una regla obligatoria que hoy sí le llega; esto se
  verifica rol por rol, listando cada regla y la pieza por la que le sigue llegando.

Medición y calidad
- RF-13: CUANDO termine el cambio, EL SISTEMA PERMITIRÁ comparar antes y después, por rol y para el mismo
  caso (los pasos de referencia), el arranque en frío (bytes del agente + lo heredado) y las lecturas
  mínimas que la `003` ya medía. La línea base se registra en `05_avance.md` **antes de cambiar** cualquier
  archivo de `template/` y después de cerrada la `003` (RF-18). La medición posterior es sobre el
  contenido final de `template/`.
- RF-14: CUANDO se compare antes y después, EL SISTEMA DEMOSTRARÁ que cada verificación existente del
  arnés sigue presente, cada una con la pieza donde sigue: los criterios de aceptación y el "Hecho cuando"
  que valida el `reviewer`; los chequeos de `AGENTS.md` › Comandos; la revisión de seguridad con alcance
  código; las verificaciones de `/sdd-cierre` (deuda, ADR, índices); la regla de que el `implementer` no
  registra en Git ni escribe `05_avance.md`; la regla de que E1 a E20 los ejecuta la sesión principal; las
  puertas de aprobación del `coordinator`; y la clasificación de la `003` (lectura mínima, lecturas
  permitidas, aviso de respaldo).
- RF-15: CUANDO se cambie un campo que antes aparentaba proteger algo (RF-6), EL SISTEMA REVISARÁ el
  cambio con la skill `seguridad-basica` completa, porque `tools`, `mode` y los permisos cuentan como
  código ejecutable (definición de la `003`); la verificación de secretos aplica siempre.

Compatibilidad y distribución
- RF-16: CUANDO el kit se instale o actualice en un proyecto, EL SISTEMA LLEVARÁ los agentes por el
  procedimiento actual: grupo A, se reemplazan solo si son KIT-VIEJO (ADR 007) y se respetan si fueron
  personalizados. Un agente personalizado que conserve `mode`/`permissions` sigue funcionando (se ignoran
  como hoy). Cambio concreto en `AGENTS.md` (grupo B): en "Qué lee cada rol" se ajusta la frase que dice
  que no se supone que la herramienta inyecta el archivo (el `implementer` puede recibirlo; los otros
  tres no) y las filas de `planner`, `reviewer` y `coordinator` añaden "Git" y "Reglas duras" (RF-12). La
  tabla y la frase existen, solo cambia su contenido: no se detecta como faltante y llega por fusión
  manual descrita en el `CHANGELOG.md`, sin cambiar qué se clasifica. El `CHANGELOG.md` dice qué cambia
  y qué debe revisar quien personalizó un agente: (1) quitar `mode`/`permissions` (se ignoran); (2) que
  el `reviewer` ya no tiene `Write`; (3) que sin `model` el agente hereda el de la sesión; (4) que sin
  `omitClaudeMd` el agente sigue cargando `AGENTS.md` completo.
- RF-17: CUANDO un agente se use en otra herramienta del kit (Cursor, Gemini, Copilot), EL SISTEMA NO
  AFIRMARÁ que el cambio de frontmatter no la afecta, porque no está verificado: quedará dicho en el
  registro de RF-1 (RF-2, RF-6) que se desconoce si esas herramientas leen `model` y `omitClaudeMd`, y
  que borrar `mode` y `permissions` puede hacerles perder la función que tuvieran allí (no verificada).
  Lo que sí se garantiza: ningún campo añadido ni quitado es necesario para que el cuerpo del agente
  siga siendo legible y para que las restricciones de la tabla de RF-7 tengan su instrucción de texto.
- RF-18: CUANDO cambie algo de `template/`, EL SISTEMA CERRARÁ en este orden. Precondición de
  implementación: la `003` está cerrada, verificable porque su fila en `.docs/002_specs/README.md`
  marca el estatus de cerrada y su `05_avance.md` registra el cierre; y los archivos de `template/` que
  esta spec toca no tienen cambios sin registrar ajenos. Luego: cambios en `template/` y en los docs de
  la raíz; manifiesto de 0.5.0 regenerado al final según el procedimiento de la `001`; E20 pasa;
  contraste de 0.4.0 contra 0.5.0; chequeos de `AGENTS.md` › Comandos tras regenerar; medición de RF-13 y
  RF-14 sobre ese contenido final. Si `template/` cambia después, se regenera y se repite.
- RF-19: CUANDO se publique esta mejora, EL SISTEMA LA DECLARARÁ dentro de la 0.5.0 sin publicar: el
  bloque de 0.5.0 en `CHANGELOG.md` ya está fechado y no se publica aún, así que la línea nueva va en
  ese bloque sin cambiar versión ni fecha; el manifiesto de 0.5.0 se regenera al cerrar. "El clon" es la
  raíz de este repo: su copia local de `.agents/` no se versiona (ADR 001), pero su `AGENTS.md` sí. Ambos
  se alinean con `template/` en lo genérico (el `AGENTS.md` de la raíz ya tiene "Qué lee cada rol"), pero
  esa alineación no bloquea el cierre: si la copia local no se refrescó, queda registrada como pendiente
  de refrescar (coherente con los criterios de finalización).
- RF-20: CUANDO el `reviewer` ya no escriba `07_pruebas_qa.md` ni `tests/` (RF-7 fila 1), EL SISTEMA
  CAMBIARÁ el paso "si se pide evidencia formal" de `/sdd-validate`: la sesión principal escribe
  `07_pruebas_qa.md` y el harness de `tests/`; el reviewer, en una segunda invocación, ejecuta el
  harness en modo lectura y devuelve el resultado, que la sesión principal anota en `07_pruebas_qa.md`.
  Esa invocación cuenta como una de las 2 vueltas de `/sdd-validate`. La fuente única del procedimiento
  es el comando `sdd-validate` y el agente `reviewer` solo lo enlaza (ADR 009, RNF-4). Queda en el
  `CHANGELOG.md` como cambio de comportamiento.
- RF-21: CUANDO un proyecto actualice solo uno de `reviewer.md` y `sdd-validate.md` (ambos grupo A, se
  clasifican por separado), EL SISTEMA LO DEJARÁ FUNCIONANDO sin error: (a) `reviewer` nuevo con
  `sdd-validate` anterior: la validación sigue y, si se pide evidencia formal, el reviewer devuelve el
  contenido en lugar de escribirlo y la sesión principal lo escribe; (b) `reviewer` anterior con
  `sdd-validate` nuevo: la sesión principal escribe la evidencia y el reviewer ejecuta el harness; el
  reviewer anterior aún tendría `Write`, así que solo se pierde la garantía de RF-7 fila 1. El
  `CHANGELOG.md` pide actualizar ambos a la vez y dice qué ocurre en cada combinación.

## Requisitos no funcionales
- RNF-1: Sin pérdida de calidad. Ningún rol sale de la comparación de RF-4 con menos resultados que antes;
  ninguna verificación de RF-14 desaparece.
- RNF-2: Reducción medible del arranque en frío de los subagentes que adopten `omitClaudeMd`: al menos el
  tamaño del `CLAUDE.md` y `AGENTS.md` que dejan de cargar, menos lo que haya que añadir en el agente.
  Si el proyecto no importa `AGENTS.md` desde su `CLAUDE.md`, el ahorro es cero y así se registra. Para
  `planner`, `reviewer` y `coordinator` (decisión firme) la medición documenta el neto, sea cual sea,
  pero no revierte el campo salvo regresión de calidad medida (RF-14). Para el `implementer`, si el neto
  no es una reducción, no lo adopta.
- RNF-3: Solo Markdown, UTF-8 sin BOM, en español, genérico y sin scripts ni nombres de modelo o stack de
  un proyecto concreto salvo los valores de campo que la herramienta documenta (`sonnet`, `inherit`).
- RNF-4: Un solo lugar por regla (principio 3, ADR 009): la tabla de RF-7 es la fuente de las
  restricciones; el comando `sdd-validate` es la fuente del procedimiento de validación; los agentes y
  comandos nombran y enlazan sin repetir.
- RNF-5: Compatibilidad hacia atrás: los features existentes y los proyectos ya instalados siguen
  válidos; un agente personalizado no se rompe.
- RNF-6: Cada afirmación sobre el comportamiento de la herramienta cita fecha y fuente; lo no verificado
  se rotula como tal.
- RNF-7: Las `description` de los agentes no exceden el presupuesto conjunto de 15.000 tokens que la
  herramienta fija para los subagentes.

## Casos límite
- La herramienta cambia el soporte de un campo (p. ej. empieza a honrar `permissions`): el registro de
  RF-1 lleva fecha; se revisa al actualizar la herramienta.
- El modelo `sonnet` del `implementer` falla un paso que el modelo actual hacía: RF-4 lo detecta en la
  prueba; en uso real, el paso se devuelve a la sesión principal y se registra como decisión.
- El `coordinator` con `sonnet` enruta mal o salta una puerta de aprobación: RF-4 lo detecta con la lista
  de 5 peticiones; si no pasa, conserva el modelo actual.
- El `reviewer` sin escritura no puede producir `07_pruebas_qa.md` cuando piden evidencia formal: la
  escribe la sesión principal y el reviewer ejecuta el harness en una segunda invocación (RF-20). Si ya
  se gastó una vuelta de las 2 por CAMBIOS NECESARIOS, solo queda una para esa invocación.
- El `reviewer` conserva `Bash`: no puede garantizarse que no escriba por `sed -i`, redirecciones o
  `tee`; queda como instrucción no garantizada (RF-7 fila 1).
- `omitClaudeMd` quita las reglas de Git del `implementer`: la restricción de RF-7(4) debe seguir
  llegándole (RF-11).
- El `coordinator` invocado como subagente: la lista `Agent(planner, implementer, reviewer)` se ignora y
  `Agent` permite lanzar subagentes hasta 3 capas (hecho verificado); se registra como no garantizado
  (RF-7 fila 6). Cualquier cambio de campos no debe quitar `Agent` ni la lista del `coordinator`
  corriendo como hilo principal.
- El rol con `omitClaudeMd: true` pierde una regla que necesitaba y que no llega por otra vía: RF-12
  lo detecta en la verificación por rol; para el `implementer` el rol no adopta el campo, y para los otros
  tres se corrige cómo llega la regla, sin revertir el campo.
- Un alias de modelo que la herramienta no conoce o que el plan del usuario no permite: RF-5.
- Una variable `CLAUDE_CODE_SUBAGENT_MODEL` o `_FORCE` activa en la sesión que mide: la medición de RF-4
  no vale hasta desactivarla o anotarla.
- Un proyecto con `AGENTS.md` propio sin la tabla "Qué lee cada rol" (instalado antes de la `003`): los
  agentes reemplazados siguen funcionando porque nombran sus secciones; no se supone que la tabla exista.
- Un proyecto cuyo `CLAUDE.md` no importa `AGENTS.md`: `omitClaudeMd` no ahorra nada y quita su propio
  `CLAUDE.md` (RNF-2).
- Actualización parcial de `reviewer.md` y `sdd-validate.md`: RF-21.
- La `003` no está cerrada cuando se quiere empezar: no se implementa (RF-18); se espera su cierre.
- La comparación de RF-4 cuesta tokens reales: se acota a los tres pasos de referencia (implementer) y a
  las 5 peticiones (coordinator), una corrida por caso y modelo.

## Fuera de alcance
- Cambiar los cuerpos de los agentes o los comandos más allá de lo que exigen estos requisitos (la `003`
  ya los reescribió).
- Cambiar el procedimiento de actualización, los manifiestos, su formato o la definición de E20 (ADR 007
  y 008).
- Agregar scripts, hooks que exijan código ejecutable versionado o medición automática (ADR 004).
- Versionar el `.claude/` ni la configuración local de ningún usuario.
- Que `/sdd-init` sugiera reglas `allow` o `deny` desde los comandos del proyecto (se podría proponer
  como deuda aparte con visto bueno del responsable).
- Re-ejecutar un feature completo para comparar calidad (la prueba es RF-4, RF-13 y RF-14).
- Verificar y adaptar el comportamiento de Cursor, Gemini y Copilot más allá de rotular lo no verificado
  (RF-2, RF-17).
- Optimizar el tamaño de `AGENTS.md` o de los agentes por sí mismo (se mide su efecto, no se reescribe).
- Cambiar `effort`, `maxTurns`, `memory`, `isolation` u otros campos soportados, salvo que la
  clarificación los incluya.

## Criterios de finalización
- Precondición verificada antes de implementar: la `003` cerrada (RF-18).
- Registro de campos verificados (RF-1, RF-2) con fecha, fuente y limitación, en la pieza de
  restricciones.
- Línea base registrada en `05_avance.md` antes del primer cambio a `template/`.
- Cada uno de los 4 agentes con `model` explícito; `implementer` y `coordinator` con la evidencia de
  RF-4 (casos, corridas, umbral, modelo actual y propuesto) en `05_avance.md`.
- `reviewer` sin `Write` ni `Edit`, con "sin `Write`/`Edit`" garantizada por `tools` y "no modifica
  archivos por Bash" registrada como no garantizada; `/sdd-validate` describe el procedimiento de
  evidencia formal (RF-20) y el agente `reviewer` solo lo enlaza.
- Tabla de restricciones (RF-7) completa, con mecanismo y garantía por fila, y los agentes enlazándola;
  fragmentos opcionales `deny` (RF-9) y `allow` (RF-10) documentados en la misma pieza.
- Decisión por rol de `omitClaudeMd` con su medición (RF-11, RF-12, RNF-2).
- `mode` y `permissions` resueltos según RF-6, sin reglas que aparenten proteger algo que no protegen.
- Comparación antes/después (RF-13) y lista de verificaciones con la pieza donde siguen (RF-14).
- Revisión con `seguridad-basica` completa (RF-15).
- `CHANGELOG.md` actualizado según RF-16, RF-19, RF-20 y RF-21 en el bloque de 0.5.0; `AGENTS.md` de
  `template/` con el ajuste de RF-16.
- Cierre en el orden de RF-18; chequeos de `AGENTS.md` › Comandos pasan en cada archivo tocado; ensayo
  de instalación sin pisar nada; chequeos de réplicas de `/sdd-estructura` pasan o la copia local de la
  raíz queda registrada como pendiente de refrescar (RF-19).
- Deuda `003_ModeloYPermisosDeAgentes` marcada `Resuelta` (con "Atendida por" y "Cómo se atendió") en
  el cierre.

## Dudas abiertas
Ninguna.
