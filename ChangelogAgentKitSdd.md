# Changelog

## 0.14.0 — 2026-11-22
- Spec `0019_FlujoEficiente`: menos tokens y espera por spec, sin bajar la verificación. ADR 0036.
- `skills/sdd/SKILL.md` › «Dónde corre cada rol»: comprobación antes de cada lanzamiento (rol del kit
  y motivo) y solo roles del kit en una fase SDD (sin `general-purpose`); tabla de fases con cascada.
- `commands/sdd-implement.md`: tres formas (todo, fase `Fn`, paso `Fn.m`); cascada con tabla previa,
  lotes de hasta 4 pasos, olas paralelas, revisión por lote y paro solo en lo ajeno.
  `agents/implementer.md`: paso o lote.
- `skills/sdd/avance-y-tiempos.md`: motivo del lanzamiento en la Nota; fila de lote.
- `skills/sdd/topes-y-modos.md` y `commands/sdd-clarify.md`: autochequeo del planner antes de la
  pasada 1 y pasadas por delta desde la 2.
- `commands/sdd-tasks.md`: la validación de estructura no se delega; siguiente paso en cascada.
- `commands/sdd-cierre.md`, `AGENTS.md` (`Chequeos de cierre: No aplica`) y `commands/sdd-init.md`:
  la comprobación previa del cierre corre los chequeos de cierre que declare el proyecto.
- `README.md`: `/sdd-implement` en cascada o por paso.

## 0.13.0 — 2026-11-22
- Spec `0017_PausaOrigenYContexto`. Issues #4, #6, #7, #8, #10 y #11; deudas 0009 a 0014; ADR 0033 a 0035.
- Nuevos: `commands/sdd-pausa.md`, `commands/sdd-version.md`, `catalogo-skills.md`,
  `skills/sdd/antecedentes-y-deuda.md` y `.docs/005_contexto/README.md`; archivo de proyecto
  `.agents/.kit-origen`; bandera local `.agents/local/estructura-ok`.
- `AGENTS.md`: Antecedentes y Deuda técnica bajo demanda; regla 1 condensada a 1 885 bytes sin perder
  piezas; paso 8 solo sobre índices; excepción de red en Git; línea de contexto. La plantilla pasa de
  11 061 a 9 603 bytes.
- `topes-y-modos.md`: cambio pequeño. `sync-agents.md`: chequeo de arranque reducido.
- Pausa en `avance-y-tiempos.md`, `plantillas.md` y `sdd-status.md`.
- `sdd-init.md`: contexto, stack, BD de solo lectura, allow y formato. `restricciones-de-rol.md`: deny
  con 7 clientes de BD.
- `sdd-cierre.md`: contexto y deuda 0013. `sdd-plan.md` y `sdd-implement.md`: remisión a «Quién escribe».
- `validar-estructura.md` (E10, E13, E22, E24–E26, bandera) y `sdd-estructura.md`; `coordinator.md`,
  `SKILL.md`, `.agents/README.md` y `reviewer.md` (E1–E26).
- `INSTALAR.md` (`.kit-origen`) y `ACTUALIZAR.md` (origen, tres commits, skills al final).
- Leyenda de estatus: «Quién escribe» con etapas intermedias y diferimiento distinto de la fila
  «Pausa»; «Numeración única» con `005_contexto`.
- Nota para proyectos actualizados: `AGENTS.md` es grupo B y `.docs/002_specs/README.md` es grupo C; se
  incorporan a mano (`ACTUALIZAR.md`). `.gitignore` no cambia si ya ignora `.agents/local/`. Un
  `AGENTS.md` viejo sigue funcionando.

## 0.12.0 — 2026-11-21
- Arranque ligero (ADR 0032): el criterio de qué va en `AGENTS.md` (siempre cargado) y qué se lee
  bajo demanda vive en `.agents/README.md` › «Qué va en `AGENTS.md`».
- Salen de `AGENTS.md` a un renglón de remisión: «Qué lee cada rol» (cada rol ya declara lo suyo en
  `.agents/agents/<rol>.md`), «Roles y comandos» (pasa a `.agents/README.md` › «Roles y comandos») y
  «Formato y numeración» (fuente: `.docs/002_specs/README.md` › «Numeración única»).
- «Cada sesión»: el paso 4 lee `.agents/README.md` solo al crear o modificar agentes, skills o
  comandos; el paso 7 lee solo «Chequeo de arranque» y lo que indica; el paso 8 remite a
  «Numeración única» solo si hay repetidos.
- `sync-agents.md` reordenado: «Chequeo de arranque» al inicio con su «Qué lee» y sección nueva
  «Estados de enlace» (tabla de estados y definiciones de `OK`).
- E8 y E9 comparan con `.agents/README.md` › «Roles y comandos»; E17 cita también el criterio.
- Nota para proyectos actualizados: `AGENTS.md` es grupo B; los pasos 4, 7 y 8 se incorporan a mano
  (`ACTUALIZAR.md`); conservar las secciones viejas no rompe nada.

## 0.11.0 — 2026-11-21
- Cierre en una corrida con sus commits (ADR 0031): si pasan la previa y los criterios 1, 3, 4 y 5,
  `/sdd-cierre` escribe `Cerrado` y propone en la misma corrida los commits del requerimiento y, al
  final, el de sus propios archivos, con una sola confirmación. Confirmados, queda `Cerrado`; sin
  confirmación, o si un commit falla, baja a `Implementado` en esa corrida. Ya no hace falta una
  segunda corrida ni un commit solo para el estatus.
- Leyenda de estatus (`.docs/002_specs/README.md`): el criterio 2 pasa a «ningún cambio del
  requerimiento fuera de los commits confirmados en la corrida del cierre», y la frase de los tres
  resultados lo refleja.
- Nota para proyectos actualizados: `.docs/002_specs/README.md` es del proyecto; la leyenda nueva se
  copia a mano en su sección «Leyenda de estatus».

## 0.10.0 — 2026-11-21
- Asistente nuevo `/sdd-config` (`.agents/commands/sdd-config.md`): configura paso a paso, por dev y
  sin versionar, los modelos por rol, los roles que corren siempre como subagente, el tope y el modo de
  corrección de clarify, los entornos de QA y la presentación de las respuestas. Se puede saltar (aplica
  los sugeridos) y repetir. Lo invocan `/sdd-init` y la actualización.
- Configuración por dev no versionada (ADR 0029): el kit solo sugiere en `.agents/configuracion.ejemplo.md`
  (incluye la tabla de equivalencias de modelos por herramienta); lo elegido vive en
  `.agents/local/configuracion.md`. `PREFERENCIAS.md` solo remite. Nada de la configuración entra a
  `AGENTS.md`.
- Carpeta de agentes por archivo con adaptadores de rol (ADR 0030), definida en `sync-agents.md` ›
  «Carpeta de agentes por archivo»: en toda herramienta distinta de Claude Code, y en Claude Code solo con
  modelos propios. La aplica `/sdd-enlaces`; E3 y E23 la reconocen.
- `/sdd-init` ya no escribe `model:` en `.agents/agents/` (fin del `inherit` de 0.9.0 para proyectos con
  otras herramientas): el modelo de cada herramienta va en sus adaptadores.
- Clarify: modo de corrección «automático» (sugerido) o «con aprobación» en `topes-y-modos.md`; la fila
  de avance anota «pasada n de N».
- Auditor externo (beta), apagado por omisión: `SKILL.md` › «Auditor externo (beta)».
- El chequeo de arranque avisa si falta `.agents/local/configuracion.md` o si el kit trae pasos nuevos.
- Nota para proyectos actualizados: tras actualizar, corre `/sdd-config` y luego `/sdd-enlaces`. Si
  `.agents/agents/` tiene `model: inherit` de 0.9.0, la actualización propone pasarlo a los adaptadores.
  `AGENTS.md` es grupo B: el paso 10 de «Cada sesión» y `/sdd-config` en «Roles y comandos» se
  incorporan a mano.

## 0.9.0 — 2026-11-21
- Modelos por rol: tabla en la sección «Modelo por rol» de `restricciones-de-rol.md`. Planner, coordinator
  y reviewer en opus; implementer en sonnet. Roles nuevos `tester` y `explore`, en haiku y con encargo
  cerrado. El ADR 0028 reemplaza al ADR 013.
- Rol en el chat salvo cambio de modelo: la regla vive en `SKILL.md` › «Dónde corre cada rol». Los
  comandos de fase dicen «Ejecuta el rol».
- `07_casos_prueba.md` reemplaza a `07_pruebas_qa.md`. Un `07_pruebas_qa.md` ya existente se conserva.
- Excepción de QA a `AGENTS.md` › Guardrails, regla 1, con `.agents/local/entornos.md` (ejemplo en
  `.agents/entornos.ejemplo.md`). Modo seguro por omisión.
- Chequeo nuevo E23 en `/sdd-estructura`.
- Nota de grupo B: `AGENTS.md` (Guardrails, roles y Comandos) no se pisa; cada proyecto lo incorpora a
  mano. Hasta entonces `tester` queda en modo seguro.
- Proyectos que usan solo otras herramientas: el modelo va como `inherit`.
- Encargo a un rol Haiku: lo comprueba quien lo lanza, y el rol lo repite como segunda barrera. Si no
  entrega, un solo relanzamiento con el modelo del implementer y después la sesión principal
  (`SKILL.md` › «Dónde corre cada rol»). El ADR 0028 descarta aceptarlo solo como instrucción con deuda.

## 0.8.0 — 2026-11-16
- Estatus terminal de una spec: `Cerrado` y `Descartado`. La leyenda de estatus es la fuente única y vive
  en `## Leyenda de estatus` de `.docs/002_specs/README.md` (la definición de deuda, bajo `## Estados`
  de `.docs/003_deuda/README.md`). Comandos, skill, plantilla y `AGENTS.md` remiten a ella y no la copian.
  Los puntos 5 y 6 de «Antecedentes» de `AGENTS.md` también remiten a la leyenda.
- El cierre (`/sdd-cierre`) tiene tres resultados, y deja la fila del índice con el estatus que le
  corresponde según la leyenda.
- Chequeo nuevo E22 en `/sdd-estructura` (`.agents/validar-estructura.md`), después de E21. Es solo
  `AVISO`: no bloquea. E13 no cambia.
- Nota para proyectos ya instalados: `AGENTS.md` es grupo B y no se pisa. Cada proyecto debe incorporar
  a mano en `AGENTS.md` los puntos 5 y 6 de «Antecedentes» que remiten a la leyenda de estatus. Hasta
  entonces sigue valiendo el texto anterior. Las piezas del grupo A (agentes, comandos, skill `sdd` y
  `validar-estructura.md`) salen KIT-VIEJO o PERSONALIZADO según cada proyecto. El `.docs/` es grupo C
  y no se pisa: la leyenda y la definición de deuda se copian a mano a sus README.
- Filas ya escritas del índice, sin reescribir (mapa de `ACTUALIZAR.md` › `### Reclasificación de filas
  ya escritas`, que se aplica a mano): `Plan` → `Planificado`; `Spec aprobada` → `Análisis` sin plan y
  `Planificado` con plan; `Implementado en local` → `Implementado`. Un `Cerrado` ya escrito sigue la
  fila de la leyenda. Qué cuenta como plan lo dice la leyenda.

## 0.7.0 — 2026-11-03
- Tope de clarify: 3 pasadas por spec en modo completo (1 en ligero). Una pasada es cada revisión del
  reviewer sobre `01_spec.md`; la sesión principal anota «pasada n de 3» en `05_avance.md`. Al llegar al
  tope no se abre otra pasada automática: lo pendiente pasa a marcadores o a supuestos explícitos y la
  persona decide.
- Marcador único `[ACLARAR: <duda>]`, que reemplaza a `[NECESITA ACLARACIÓN]` en la plantilla de la
  spec. Tope de 3 por spec, con prioridad alcance > seguridad > experiencia > detalle técnico; el resto
  pasa a supuestos explícitos. Un marcador sin resolver bloquea `/sdd-plan` (en ligero, `/sdd-tasks`).
- Modo ligero para cambios chicos, con 8 condiciones (3 archivos o menos, sin archivo compartido con
  otra spec en curso, sin cambio de reglas globales, sin ADR, sin `sql/`, sin comportamiento nuevo,
  estimado de 1 hora o menos, un solo módulo). Documentos: spec corta (hasta 5 000 bytes y 8 RF),
  `04_tasks.md` con el plan dentro (hasta 8 000 bytes) y `05_avance.md`; sin `02`, `03` ni `06`. Si una
  condición se incumple o un documento excede su límite, se promueve a completo con `/sdd-change`,
  conservando `01` y `04`.
- Revisión cruzada como modo del reviewer, sin comando nuevo: a petición, cruza `01_spec.md`,
  `03_plan.md` y `04_tasks.md` en solo lectura y lista requisitos sin tarea y contradicciones.
- Pieza nueva `skills/sdd/topes-y-modos.md`, fuente única de estas reglas; los comandos y agentes la
  enlazan sin copiarla. Es ruta nueva: se agrega en los proyectos instalados. Las piezas del grupo A
  que la enlazan (agentes, comandos y skill `sdd`) salen KIT-VIEJO o PERSONALIZADO según cada proyecto.
- Nota para proyectos ya instalados: `AGENTS.md` es grupo B y no se pisa. Cada proyecto debe incorporar
  a mano en `AGENTS.md` la línea de documentos por feature («`01_spec.md`, `04_tasks.md` y
  `05_avance.md` siempre; `02_analisis_impacto.md`, `03_plan.md` y `06_flujo.md` en modo completo;
  `sql/` si hay cambios de datos», con enlace a `.agents/skills/sdd/topes-y-modos.md`) **antes de usar
  el modo ligero**. Hasta entonces sigue operando el flujo completo. La línea 17 de
  `.docs/002_specs/README.md` (grupo C) es opcional.
- Honestidad sobre el alcance: ninguna de las specs 001 a 0010 habría calificado para el modo ligero.
  Los umbrales de 3 archivos y 1 hora son provisionales y se revisan tras 3 usos reales.

## 0.6.0 — 2026-11-02
- `AGENTS.md` incorpora la sección «Guardrails (todos los agentes, siempre)», con la regla 1: la base de
  datos solo se lee. Los agentes, las skills `sdd` y `migracion-bd`, los comandos de implementación,
  constitución e inicio y `restricciones-de-rol.md` remiten a ella; esta suma la fila correspondiente
  (instrucción, no garantía) y la recomendación de un usuario de BD de solo lectura.
- `AGENTS.md` es grupo B: en un proyecto existente la sección llega como faltante y se incorpora a
  mano, junto a la nota «Qué lee cada rol» de 0.5.0. Los agentes (grupo A) salen KIT-VIEJO o
  PERSONALIZADO según cada proyecto.
- Falso positivo tras `/sdd-init`: el paso 1 de «Cada sesión» y la fila coordinator de `AGENTS.md`
  describen el marcador de instalación pendiente sin escribir el literal y remiten al comentario de
  la plantilla. `/sdd`, el coordinator, la skill `sdd`, `/sdd-init` y el README remiten al paso 1.
- Nuevo chequeo E21 en `validar-estructura` (bloque sesión, solo con `.agents/.kit-version`): FALLA si
  `AGENTS.md` no existe, si contiene el literal fuera del marcador válido, o si la cadena del
  marcador falta o no coincide con la plantilla (con `template/` presente).
- `AGENTS.md` es grupo B: los proyectos existentes incorporan a mano el paso 1 y la fila coordinator
  nuevos. Hasta entonces, E21 da FALLA en un proyecto ya personalizado con una versión anterior
  (esperado).
- Nuevo comando `/sdd-enlaces`: crea o corrige los enlaces de cada herramienta tras un pull o un clon
  nuevo. Pregunta qué herramientas usas, muestra el comando exacto de cada enlace y confirma por tipo de
  acción, nunca por enlace. No borra contenido real: un destino ocupado se pregunta como excepción.
- Aviso al arranque (paso 7 de «Cada sesión», solo lectura): si cambió la versión del kit o sus piezas, o
  faltan enlaces de tus herramientas, muestra un único aviso de hasta 3 líneas y recomienda `/sdd-enlaces`.
  Compara con el estado local `.agents/local/kit-visto` (ignorado por Git), que solo escribe
  `/sdd-enlaces` al terminar bien. El arranque ya no detecta réplicas ni ignorados versionados: eso lo
  hace `/sdd-enlaces` y lo reporta `/sdd-estructura`.
- `/sdd-estructura` es solo lectura: propone y remite a `/sdd-enlaces` para enlaces y réplicas. E1 se mide
  sobre las herramientas declaradas y E19 ignora `.agents/local/`.
- `sync-agents.md`: nuevas secciones «Estado «visto»» y «Chequeo de arranque»; los pasos 7 y 8 pasan a
  ser reglas que ejecuta `/sdd-enlaces` y el disparador de piezas nuevas sugiere `/sdd-enlaces`.
- `ACTUALIZAR.md`: nueva sección «Qué hacer después de un pull que actualiza el kit» (no correr
  `/sdd-init`; abrir sesión, `/sdd-enlaces`, comprobar con `/sdd-estructura`).
- `AGENTS.md` es grupo B: en los proyectos existentes el paso 7 nuevo de «Cada sesión» (chequeo de
  enlaces y kit) se incorpora a mano, como el paso 1 y la sección Guardrails.
- Ajustes de `validar-estructura` y `sync-agents.md`: E1 sin «visto» legible mide todas las filas y un
  `FALTA` de herramienta no declarada es `AVISO` (`DIFERENTE` y `OCUPADO` sin `excepcion:` siguen en
  `FALLA`); E7 exime en el repo del kit las skills locales presentes en `.agents/skills/` y ausentes de
  `template/` (3 filas en la raíz). El ejemplo del aviso lleva los enlaces pendientes en la línea 2, y
  `git-commit` remite al paso 7 de `sync-agents.md` para la regla de enlaces.

## 0.5.0 — 2026-10-24
- Nueva skill `adr` y carpeta `.docs/004_adr/` (índice): decisiones de arquitectura con contexto,
  alternativas descartadas y consecuencias. `/sdd-plan` y `/sdd-cierre` proponen el ADR (sin crearlo
  sin visto bueno).
- Nueva skill `migracion-bd`: cómo se escriben los scripts `sql/` (orden, encabezado, idempotencia,
  rollback). El reviewer los revisa con ella; `sdd` y `/sdd-plan` la referencian.
- Nueva skill `seguridad-basica`: búsqueda de secretos antes del commit y checklist OWASP ligero.
  `git-commit` (fase 2) y el reviewer (`validate`) la invocan.
- `AGENTS.md`: 3 filas nuevas en la tabla de skills.
- Nuevo comando `/sdd-estructura` y `.agents/validar-estructura.md`: el agente valida la estructura del kit
  (enlaces, réplicas, skills, agentes, comandos, índices, codificación) y propone correcciones sin aplicarlas.
- `sync-agents.md` paso 8: al agregar una skill, agente o comando, sugiere replicarlo en las carpetas de cada
  herramienta (o excluirlo si es una prueba local). `.agents/README.md` lo referencia al agregar cada tipo.
- `INSTALAR.md`: elige modo al empezar. **Modo proyecto** instala solo `template/`; **Modo mantenimiento**
  (clon del kit) conserva el conocimiento propio versionado (specs, ADR, memoria, reglas) y pregunta antes de copiar.
- Actualización de un proyecto que ya tiene el kit: `ACTUALIZAR.md`, en lugar de reinstalar encima.
- Manifiestos de huellas por versión en `manifiestos/` (fuera de `template/`, no se distribuyen).
- Marcador de versión `.agents/.kit-version`, escrito al instalar en modo proyecto.
- Chequeo E20, solo en el repo del kit: las huellas de `template/` coinciden con el manifiesto de `VersionAgentKitSdd`.
- Numeración: un proyecto nuevo numera specs, deudas y ADR con 4 dígitos (`0001`); un proyecto con ítems
  conserva el ancho de cada carpeta (`017_` sigue en 3). El siguiente libre se calcula por valor numérico
  (`017` = `0017`) y nunca se renumera. Las carpetas de sección de `.docs/` siguen en 3 y el clon remite a
  la declaración de `template/.docs/002_specs/README.md` › « Numeración única »: los ya creados no se renumeran.
  Lo aplican los comandos `/sdd-*`, las skills `adr`, `git-commit`, `migracion-bd` y `sdd`, y el chequeo E12,
  que informa los anchos mezclados como `AVISO`.
- `ACTUALIZAR.md` avisa, por los nombres de carpeta y sin leer los README, si el proyecto tiene ítems de 3
  dígitos, y ofrece el texto de la regla; solo escribe si lo confirmas. Mientras no lo incorpores, las piezas
  aplican la regla mínima « un ítem nuevo conserva el ancho de su carpeta ».
- Menos lectura por rol y por comando, y una sola fuente por fase del flujo `sdd`. El cambio de
  redacción en `AGENTS.md` llega a un proyecto por fusión manual.
- La skill `sdd` manda que el chat lance al coordinator y no ejecute las fases.
- Si una fase no cabe en un solo paso, sigue en esa misma entrada y deja constancia en el avance. Un pedido de hacerla en el chat se atiende en la sesión.
- Modelo explícito por rol; `omitClaudeMd` en planner, reviewer y coordinator; se quitan `mode` y
  `permissions`; el reviewer ya no tiene `Write` y la evidencia formal la escribe la sesión;
  `restricciones-de-rol.md` es NUEVO (grupo A) y se incorpora junto con los agentes; «Qué lee cada rol»
  llega por fusión manual. Quien personalizó un agente: (1) quitar `mode`/`permissions` (se ignoran);
  (2) el reviewer ya no tiene `Write`; (3) sin `model` el agente hereda el de la sesión; (4) sin
  `omitClaudeMd` el agente sigue cargando `AGENTS.md` completo. Hay que actualizar `reviewer.md` y
  `sdd-validate.md` a la vez: si el reviewer es el anterior y el comando el nuevo, la sesión escribe la
  evidencia y solo se pierde la garantía de que el reviewer no escriba; al revés, el procedimiento queda
  desfasado porque el reviewer devuelve el contenido y la sesión lo escribe.
- Veredicto de `model`, `omitClaudeMd` y `tools` en Cursor, Gemini CLI, GitHub Copilot y Kiro, con fecha y
  fuente, y celda vacía si la fuente no alcanza. `/sdd-init` muestra el deny y el allow y no los aplica. Los
  tres archivos de la raíz pasan a `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `LicenseAgentKitSdd`. Un
  ítem nuevo del clon se escribe con 4 dígitos y los ya creados no se renumeran.

## 0.4.0 — 2026-10-20
- Se eliminan `install.ps1` e `install.sh`: el kit es genérico. Nuevo `INSTALAR.md` con instrucciones para que el
  agente detecte SO/shell y genere un script o dé el paso a paso (sin sobrescribir).
- Nuevo `PREFERENCIAS.md` en la raíz del proyecto (en `.gitignore`): directivas personales por usuario/entorno.
  `AGENTS.md`: paso 10 de "Cada sesión". Plantilla en `.agents/preferencias.ejemplo.md` (ejemplo: Zed y
  "limpiar contexto"). El napkin aclara que sus User Directives son del equipo.

## 0.3.0 — 2026-10-09
- Nueva skill `formato-respuesta`: línea de cierre "quién responde" (rol con cuadro de color, skill, contexto,
  desvíos) en cada respuesta. `AGENTS.md`: paso 9 de "Cada sesión" y fila en la tabla de skills.

## 0.2.0 — 2026-10-07
- `sync-agents.md`: nueva fila `.github/copilot-instructions.md` → `AGENTS.md`; paso 7 que detecta archivos
  versionados que el `.gitignore` ya ignora y propone `git rm --cached`; notas sobre copias heredadas.
- `skills/git-commit`: la fase 2 revisa `git ls-files -ci --exclude-standard`.
- `.agents/README.md`: fila de GitHub Copilot.

## 0.1.0 — 2026-10-07
- Primera versión genérica, extraída del flujo SDD de COMPRE_sistema.
- Incluye: `AGENTS.md` como fuente de verdad, `.agents/` (4 agentes, 13 comandos `/sdd*` con `/sdd-init`,
  skills `sdd`, `napkin` y `git-commit`), `.docs/` (constitución, índice de planes, deuda técnica)
  y `sync-agents.md` (enlaces: solo documento, el entorno decide cómo crearlos).
- Incluye `.gitignore` (archivos locales de agentes y carpetas de enlaces por herramienta).
- No incluye skills de dominio, `plan-implementacion` (legado), `pruebas-implementacion` ni `deploy-project`.
