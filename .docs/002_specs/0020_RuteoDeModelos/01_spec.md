# 01 — Spec: Ruteo de modelos
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
El kit fija el modelo de cada rol (`restricciones-de-rol.md` › «Modelo por rol»), pero todo lo que
no es un rol corre con el modelo de la sesión: comandos sin rol (`/sdd-status`, `/sdd-version`,
`/sdd-estructura`), la preparación de commits (skill `git-commit`) y las tareas sueltas del chat
(leer conectores, revisar logs, resúmenes). Con una sesión en el nivel alto, esas tareas pagan el
costo del nivel alto sin necesitarlo. Hoy la única salida es que la persona cambie `/model` a mano,
y eso no es opción: además de depender de la persona, cambiar de modelo a media sesión reescribe la
caché de todo el contexto.

Objetivo: que el chat elija solo, para cada tarea, el nivel de modelo adecuado y si la hace él o la
delega en un subagente, sin que la persona cambie el modelo de la sesión. El ahorro se mide; no se
supone.

Alcance ampliado por cambio de requisito (2026-11-22, decisión de la persona): los comandos `/sdd-*`
leen sus argumentos con `$1` y `$2` como primero y segundo. En Claude Code la indexación es desde 0
(`$0` es el primero, `$1` el segundo, y un índice sin argumento queda como texto literal; fuente:
code.claude.com/docs/en/skills); en OpenCode es desde 1 (opencode.ai/docs/commands). Evidencia en
esta spec: `/sdd-spec 0020_RuteoDeModelos El chat …` tomó `El` como carpeta y `/sdd-clarify
0020_RuteoDeModelos` dejó `$1` sin sustituir. Una delegación fija (RF-4) recibe solo sus argumentos,
sin la conversación para corregirlos, así que este error bloquea el ruteo.

Base: idea aprobada por la persona el 2026-11-22 (`.agents/local/idea-0020-ruteo-modelos.md`, no
versionada) con su ajuste del mismo día: sin cambio manual de modelo.

## Usuarios y roles
- **Persona que usa el kit:** pide tareas en el chat y no quiere elegir modelo.
- **Sesión principal (chat):** decide dónde corre cada tarea y con qué nivel.
- **Subagente de ruteo:** corre una tarea delegada con encargo cerrado y devuelve el resultado.
- **Mantenedor del kit:** mantiene la tabla de niveles y mide el efecto.

## Historias de usuario
- HU-1. Como persona, quiero que `/sdd-status` y comandos parecidos corran con un modelo de nivel
  medio aunque mi sesión sea de nivel alto, para no pagar el nivel alto en una consulta.
- HU-2. Como persona, quiero que preparar un commit no use el modelo de la sesión cuando no hace
  falta, y que el commit siga necesitando mi confirmación.
- HU-3. Como persona, quiero que el chat delegue solo las tareas sueltas grandes y cerradas (leer un
  conector, revisar un log largo) en un modelo más barato, sin que yo lo pida.
- HU-4. Como persona que usa otra herramienta (Cursor, OpenCode), quiero que el kit siga funcionando
  aunque mi herramienta no permita delegar con otro modelo.
- HU-5. Como mantenedor, quiero ver en el avance y en la línea «quién responde» qué nivel hizo cada
  tarea y por qué, para medir el ahorro.

## Definiciones
- **Nivel:** Alto, Medio o Bajo. Es el mismo eje de `configuracion.ejemplo.md` › «Paso 1»; cada
  herramienta lo traduce a un modelo (Claude Code: `opus`, `sonnet`, `haiku`). El kit habla de
  niveles, no de nombres de modelo, salvo en la fila de Claude Code.
- **Tarea sin rol:** comando, skill o petición del chat que no es una fase con rol del flujo `sdd`.
- **Tarea delegable:** tarea sin rol que cumple las cuatro condiciones de RF-3.
- **Delegación fija:** la que declara el propio comando o skill en su definición, de modo que la
  herramienta la corre como subagente con su nivel, sin decisión del chat en cada uso.
- **Delegación por el chat:** la que el chat decide en el momento, lanzando un subagente con el
  nivel elegido.
- **Respaldo:** comportamiento cuando la herramienta no permite ninguna de las dos delegaciones.
- **Llamada:** una llamada a una herramienta (leer, buscar, ejecutar un comando, consultar un
  conector). La respuesta final del modelo no cuenta como llamada.

## Requisitos funcionales (EARS)
- RF-1: EL SISTEMA DEBE tener una sola tabla de nivel por tarea sin rol, junto a «Modelo por rol» de
  `restricciones-de-rol.md`, que indica para cada comando sin rol, para la preparación de commits y
  para cada tipo de tarea suelta su nivel (Alto, Medio o Bajo) y si es delegable. Los tipos de tarea
  suelta son una lista cerrada: lectura de conectores, revisión de logs o salidas largas, búsqueda en
  el repositorio, y reportes o resúmenes. Una tarea que no encaja en un tipo corre en el chat hasta
  que se agregue a la tabla. Bajo es para la extracción literal (conteos, `git status`, búsquedas,
  lectura de conectores o logs); Medio, para lo que interpreta reglas o redacta (estado de una
  carpeta, mensaje de commit, reportes). Todo lo demás del kit remite a esa tabla y no la copia.
- RF-2: LAS fases con rol DEBEN seguir rigiéndose por «Modelo por rol» y por `SKILL.md` › «Dónde corre
  cada rol», sin cambios de comportamiento por esta spec.
- RF-3: UNA tarea sin rol ES delegable solo SI cumple las cuatro condiciones: (a) no necesita el
  historial de la conversación (todo lo que usa está en archivos o en sus argumentos), (b) no pide
  una decisión ni una confirmación a la persona, (c) no escribe `05_avance.md` ni la celda del índice
  y (d) no ejecuta `git commit`. Si falta una, corre en el chat.
- RF-4: CUANDO un comando o skill delegable tiene nivel menor que Alto y la herramienta admite la
  delegación fija, EL SISTEMA DEBE declararla en la definición de ese comando o skill, para que corra
  como subagente con su nivel sin que la persona cambie el modelo de la sesión. La declaración nombra
  un rol, no un modelo: el nivel sale del modelo de ese rol en «Modelo por rol». Esto incluye
  `/sdd-status` y `/sdd-estructura`, que `SKILL.md` hoy deja en la sesión principal; esa regla se
  ajusta en su fuente para exceptuar la delegación fija.
- RF-5: CUANDO la persona pide un commit o una fase propone commits (como `/sdd-cierre`), EL SISTEMA DEBE separar la preparación (revisar cambios,
  agrupar por módulo, `git add` explícito, chequeos de `AGENTS.md` › Comandos, búsqueda de secretos y
  propuesta de mensaje), que se delega con el nivel de RF-1, del `git commit`, que hace siempre la
  sesión principal tras la confirmación explícita de la persona. El encargo lleva lo que la
  preparación necesitaría de la conversación (requerimiento, carpeta, resumen de lo hecho), así que
  cumple RF-3 (a). Si la preparación llega a un punto que exige decidir (prefijo repetido, archivos
  ajenos ya en el stage, agrupación dudosa), se detiene y devuelve la pregunta al chat, que la hace a
  la persona. Las reglas de `AGENTS.md` › Git y de la skill `git-commit` no cambian.
- RF-6: CUANDO el chat recibe una tarea suelta, EL SISTEMA DEBE aplicar una regla de decisión escrita
  en una sola fuente, antes de la primera llamada: la hace en el chat si es diseño o decisión, si
  necesita la conversación o si prevé una sola llamada sobre algo de tamaño conocido y pequeño (un
  archivo corto, un `git status`); en otro caso, si es delegable (RF-3), la delega con encargo
  cerrado y el nivel que indica RF-1. Si una llamada hecha en el chat devuelve más de lo previsto, la
  tarea no se vuelve a delegar a mitad.
- RF-7: EL SISTEMA DEBE tener un rol de uso general (nombre tentativo `auxiliar`) para las
  delegaciones de RF-4 a RF-6 que no tienen un rol del kit que les corresponda; dentro de una fase,
  solo en los casos de RF-8. Nunca hace
  commit, no escribe `05_avance.md` ni la celda del índice, cumple `AGENTS.md` › Guardrails y
  devuelve el resultado o un recibo con su modelo declarado.
- RF-8: DENTRO de una fase `sdd` DEBE seguir la regla «Solo roles del kit» de `SKILL.md` › «Dónde
  corre cada rol», y el rol de RF-7 solo se permite para la preparación de commits (RF-5) y las
  delegaciones fijas (RF-4); nunca hace el trabajo del rol que la fase nombra. FUERA de una fase EL
  SISTEMA DEBE permitir el rol de RF-7 para cualquier delegación de RF-4 a RF-6. La excepción se
  escribe en esa sección, que sigue siendo la única fuente.
- RF-9: CUANDO la herramienta no admite delegar con otro modelo, EL SISTEMA DEBE correr la tarea en el
  chat (respaldo). Solo en ese caso, al iniciar un comando cuyo nivel no es el de la sesión, avisa en
  una línea que el cambio de modelo conviene hacerlo al abrir la sesión o tras `/clear`, nunca a media
  sesión. No bloquea. Si la sesión no conoce su modelo, no avisa.
- RF-10: CUANDO una tarea se delega por ruteo, EL SISTEMA DEBE dejar constancia: la línea «quién
  responde» (`formato-respuesta`) nombra en Contexto el rol y el modelo del subagente. Si la tarea se
  hace dentro de una fase de un feature (la que corre en la sesión en ese momento), la Nota de la fila
  de tiempos de esa fase lleva el modelo y el motivo «ruteo», sumado a los motivos que ya define
  `avance-y-tiempos.md`. Fuera de una fase basta la línea «quién responde».
- RF-11: EL SISTEMA DEBE permitir que cada dev cambie el modelo de los roles que usa el ruteo con el
  mismo mecanismo que ya tiene para los roles (`## Modelos` de `.agents/local/configuracion.md` y
  adaptadores de `.agents/sync-agents.md`), con un paso opcional de `/sdd-config`. No se escribe en las
  definiciones versionadas. Sin valor, rige «Modelo por rol».
- RF-12: ANTES de cerrar la spec, EL SISTEMA DEBE contar con una medición, en tokens, costo en USD
  (precios de la página oficial del proveedor, con fecha) y tiempo, de al menos `/sdd-status` y la
  preparación de un commit, en el chat con nivel Alto frente a la delegación con el nivel que les da
  la tabla de RF-1. Una delegación ahorra si su costo en USD es menor. Si no ahorra, se retira de la
  tabla; si ahorra pero tarda más del doble, decide la persona. Cada resultado queda como decisión.
- RF-13: CUANDO se invoca un comando `/sdd-*` con argumentos, EL SISTEMA DEBE tomar como carpeta el
  primer argumento y como resto (alcance, idea, cambio) lo que sigue, con el mismo resultado en todas
  las herramientas con carpeta de comandos del kit, sin depender de una numeración posicional que
  cambia entre herramientas.
- RF-14: CUANDO a un comando le falta un argumento que necesita, o le llega un marcador sin sustituir
  (por ejemplo `$1` o `$2` literal), EL SISTEMA DEBE tratarlo como argumento ausente: pide el dato o
  aplica el comportamiento definido para su ausencia, y nunca usa el marcador ni otra palabra como
  carpeta.

## Requisitos no funcionales
- Solo Markdown y frontmatter; sin scripts versionados (constitución, principio 5).
- Kit genérico: el kit nombra niveles; los nombres de modelo de cada herramienta viven en la tabla de
  equivalencias y en la configuración local (constitución, principio 1).
- Una regla, una fuente: tabla de niveles en `restricciones-de-rol.md`, regla de delegación del chat
  en `SKILL.md` › «Dónde corre cada rol» o donde decida el plan, sin copias.
- La delegación no puede costar más que lo que ahorra: lo decide la medición de RF-12.

## Casos límite
- Comando delegable invocado sin el argumento que necesita: el subagente responde que falta el
  argumento y no supone nada (no tiene la conversación para deducirlo).
- La sesión ya está en el nivel de la tarea: la delegación fija corre igual. Su costo fijo queda en
  la medición de RF-12, que decide si conviene.
- El subagente falla o no entrega: uno de nivel Bajo se relanza una sola vez con nivel Medio; uno de
  nivel Medio, o un relanzamiento que vuelve a fallar, pasa a la sesión, con constancia. No se relanza
  en bucle. Es la regla de los roles Haiku de `SKILL.md` › «Dónde corre cada rol»; el plan remite a
  ella sin copiarla.
- Declarar solo el modelo en un comando, sin delegarlo en un subagente, queda prohibido: obliga a
  escribir otra vez en la caché todo el contexto de la sesión con otro modelo y en la simulación del
  2026-11-22 costó de 1.5 a 17 veces lo mismo en el chat.
- Comando con una parte delegable y una que pide confirmación (por ejemplo `/sdd-enlaces`): solo se
  delega la parte de lectura, si el plan lo separa; si no, todo corre en el chat.
- Commit en el que la persona no confirma: no se commitea; la sesión informa qué quedó en el stage y
  propone sacarlo, para que no se cuele en un commit posterior.
- Herramienta que lee `.agents/commands/` por enlace y no entiende la declaración de delegación fija:
  la ignora y el comando corre en el chat (respaldo, RF-9). Si en lugar de ignorarla falla, el plan
  resuelve la diferencia con un adaptador por herramienta y no con una copia del comando.
- Tarea de conector delegada a un rol sin acceso a ese conector: no es delegable y corre en el chat.
- Durante una delegación el chat espera el resultado (primer plano), salvo que la persona pida seguir
  con otra cosa; entonces corre en segundo plano.
- Subagente que a mitad de la tarea descubre que necesita una decisión o confirmación: se detiene y
  devuelve la pregunta al chat, sin suponer la respuesta.
- `/sdd-implement <carpeta> F2`: hoy Claude Code toma `F2` como carpeta y deja `$2` literal, que el
  comando lee como «sin alcance» (cascada completa). Con RF-13 y RF-14, la carpeta es la primera
  palabra y el alcance `F2`.
- Comando sin argumentos (`/sdd-status` solo): sigue su comportamiento sin carpeta (índice), sin
  confundirlo con un marcador literal (RF-14).
- Texto libre con varias palabras tras la carpeta (idea de `/sdd-spec`, cambio de `/sdd-change`): todo
  lo que sigue a la primera palabra es el resto, aunque lleve comillas o signos.
- Herramienta cuya documentación no confirma la sustitución de argumentos (Cursor al 2026-11-22): el
  comando debe funcionar igual si los argumentos llegan añadidos al final del texto, sin sustitución.
- Herramienta que admite subagentes pero no elegir su modelo: aplica el respaldo de RF-9.
- Petición fuera de una fase que el chat delega y luego resulta ser de diseño: el subagente la
  devuelve como fuera de alcance y la hace el chat.

## Fuera de alcance
- Cambiar el modelo de los roles de las fases (sigue «Modelo por rol»).
- Cambiar el modelo de la sesión en forma automática a media conversación.
- La bitácora de uso (qué modelo hizo qué, tokens y anotaciones por spec): va en la spec 0021.
- Nombres concretos de modelo y esfuerzo fuera de la fila de Claude Code.
- Piezas personales del mantenedor (`/barra`, barra de estado, `PREFERENCIAS.md`).
- Renombrar comandos o cambiar qué argumentos recibe cada uno: RF-13 y RF-14 solo cambian cómo se
  leen.

## Criterios de finalización
- Existe la tabla de RF-1 y ningún otro archivo la copia.
- Al menos `/sdd-status` corre delegado con nivel Medio en Claude Code desde una sesión de nivel
  Alto, sin cambiar `/model`, y la línea «quién responde» lo muestra.
- La preparación de un commit corre delegada y el `git commit` sigue en la sesión con confirmación.
- La regla de decisión del chat y la excepción a «Solo roles del kit» tienen una sola fuente.
- El respaldo de RF-9 está escrito y no bloquea.
- La medición de RF-12 está registrada en `05_avance.md` y la tabla refleja su resultado.
- La decisión de delegar con subagente en lugar de declarar solo el modelo queda en un ADR.
- Ningún comando de `template/.agents/commands/` lee sus argumentos con una numeración posicional
  que cambie entre herramientas, y `/sdd-implement <carpeta> Fn` y `/sdd-spec <carpeta> <idea>`
  toman bien carpeta y resto en Claude Code.
- Chequeos de `AGENTS.md` › Comandos en cada archivo tocado; versión y changelog actualizados.

## Dudas abiertas
- Supuesto: el nivel de `/sdd-tasks` y `/sdd-implement` no se toca; tienen rol (planner,
  implementer) y rige RF-2. La idea los dejaba por decidir; con RF-2 dejan de ser tarea sin rol.
- Supuesto: el nombre del rol de RF-7 es `auxiliar`; el plan puede proponer otro si choca con un
  nombre de otra herramienta (deuda 007). Como la delegación fija nombra un rol (RF-4), el plan decide
  cuántos roles hacen falta por nivel (por ejemplo, uno Medio y otro Bajo, o reutilizar `explore` para
  la búsqueda de nivel Bajo).
- Supuesto (corrección automática de la pasada 1, para revisión): una delegación ahorra si su costo en
  USD es menor (RF-12); el umbral de RF-6 es por llamadas previstas, no por tokens; la espera es en
  primer plano. Son las opciones más acotadas; la persona puede revertirlas al aprobar.
- Supuesto (hallazgo de la pasada 3, al tope): RF-5 cubre también los commits que propone una fase
  (`/sdd-cierre`), en coherencia con RF-8. Para revisión al aprobar.
- Supuesto: el umbral de RF-6 (una llamada corta, menos de unos 5 000 tokens) y los niveles de RF-1
  salen de la simulación del 2026-11-22 (decisión 4 de `05_avance.md`); la medición de RF-12 los
  confirma o los corrige.
