# 01 — Spec: Reducir la ceremonia del flujo SDD
Estado: implementada (2026-11-03 23:03)
Ticket: sin ticket

## Contexto y objetivo
La spec `003_OptimizarArnes` redujo ≥70 % las lecturas por paso del implementer y de `/sdd-status`,
medido en bytes con un caso de referencia. No tocó dos fuentes de consumo del proceso: las pasadas
de `/sdd-clarify` (hoy sin tope) y la ceremonia fija de seis documentos para cualquier cambio.

Evidencia propia:
- En `0010_FalsoPositivoInitPendiente` el clarify llevó 7 pasadas y cada una abrió casos límite
  nuevos. La implementación tomó 24 min contra 115 estimados.
- En `0008_GuardrailsBdSoloLectura` la implementación tomó 31 min contra 150.
- El tiempo grande está en las esperas y en el clarify, no en escribir el cambio.

Referencia externa (github/spec-kit, analizado por el usuario): limita a 3 los marcadores
`[NEEDS CLARIFICATION]` por spec y a 3 las iteraciones de validación. Su preset `lean` reduce cada
comando a ~600 bytes frente a 8-22 KB de los completos. Los comandos del kit ya miden de 1 a 5 KB,
así que el margen está en el proceso y no en el tamaño de los comandos.

Objetivo: reducir el consumo de tokens del proceso SDD, sin bajar la calidad ni quitar las puertas
de aprobación, con tres cambios: (1) tope de pasadas de clarify y de marcadores abiertos; (2) modo
ligero para cambios chicos; (3) opcional, revisión cruzada como modo del reviewer. Versión objetivo
0.7.0 (0.6.0 ya es el bump de las specs 0008, 0009 y 0010).

## Usuarios y roles
- Persona usuaria del kit (mantenedor o proyecto): decide, confirma el modo y aprueba spec y tareas.
- Sesión principal: ejecuta los comandos y aplica el contrato de avance y tiempos.
- Agente planner: redacta los documentos, rellena la lista de condiciones y propone el modo.
- Agente reviewer: hace el clarify y, si se aprueba el punto 3, la revisión cruzada de solo lectura.

## Historias de usuario
- HU-1. Como persona usuaria, quiero que el clarify tenga un tope de pasadas para no iterar sin fin
  sobre casos límite cada vez más remotos.
- HU-2. Como persona usuaria, quiero que la spec admita pocos marcadores `[ACLARAR: …]` y que solo
  esos bloqueen el plan, para decidir lo que importa y dejar el resto como supuestos explícitos.
- HU-3. Como persona usuaria, quiero un modo ligero para cambios chicos para no producir seis
  documentos cuando el cambio cabe en una spec corta y una lista de tareas.
- HU-4. Como persona usuaria, quiero que el paso a ligero se confirme con una sola pregunta y que
  ante la duda se vaya a completo, para no perder rigor por error.
- HU-5. Como persona usuaria, quiero que un cambio ligero que crece se promueva a completo, para no
  seguir en ligero algo que dejó de serlo.
- HU-6. (Opcional) Como persona usuaria, quiero una revisión de solo lectura que cruce spec, plan y
  tareas para detectar requisitos sin tarea y contradicciones antes de implementar.

## Definiciones
- **Pasada de clarify**: cada revisión del reviewer sobre `01_spec.md`. Los ajustes del planner
  entre revisiones no cuentan como pasada.
- **Marcador `[ACLARAR: …]`**: duda en el cuerpo de la spec que bloquea el paso a `/sdd-plan`
  hasta resolverse. Reemplaza al marcador `[NECESITA ACLARACIÓN]` de la plantilla anterior: hay una
  sola convención, y el tope de 3 aplica solo a los marcadores que bloquean el plan.
- **Modo completo**: el flujo actual de `01_spec` a `06_flujo`, más `05_avance`.
- **Modo ligero**: flujo con `01_spec` corta, `04_tasks` con el plan dentro y `05_avance` con tiempos.
- **Réplica**: copia de un archivo del kit en otra ruta (por ejemplo `template/` y la raíz, o los
  enlaces por herramienta). No cuenta como archivo tocado en la condición 1.
- **Lista de condiciones**: las 8 condiciones del modo ligero, evaluadas por el planner.
- **Caso de referencia**: cambio fijo, descrito en el plan, sobre el que se mide el ahorro en bytes.

## Requisitos funcionales (EARS)

Tope de clarify y marcadores
- RF-1: CUANDO el reviewer completa una revisión en `/sdd-clarify`, EL SISTEMA cuenta una pasada y permite
  como máximo 3 pasadas por spec en modo completo. La sesión principal anota «pasada n de 3» en la
  fila `clarify` de `05_avance.md`. Una pasada incompleta o interrumpida no cuenta; una pasada solo de
  verificación cuenta si la hace el reviewer.
- RF-2: CUANDO el reviewer o el planner detectan una duda que impide redactar el plan (de alcance,
  seguridad, experiencia o detalle técnico), EL SISTEMA la registra como marcador `[ACLARAR: …]` en
  la spec. Una duda que no impide redactar el plan no es marcador: es un supuesto explícito.
- RF-3: CUANDO una spec tiene más de 3 dudas candidatas a marcador, EL SISTEMA conserva como
  marcadores 3, con prioridad alcance > seguridad > experiencia > detalle técnico y, en empate
  dentro de la misma categoría, la que afecta a más RF; para el resto toma un supuesto razonable y lo deja escrito de forma explícita en la spec.
- RF-4: CUANDO una spec tiene al menos un marcador `[ACLARAR: …]` sin resolver, EL SISTEMA bloquea
  `/sdd-plan` y dice cuáles marcadores faltan.
- RF-5: CUANDO se resuelve un marcador, EL SISTEMA lo reemplaza por la decisión tomada y anota la
  decisión con su hora en `05_avance.md`.
- RF-6: CUANDO se llega al tope de pasadas con hallazgos pendientes, EL SISTEMA no abre otra
  pasada automática y convierte lo pendiente de alcance y seguridad en marcadores `[ACLARAR: …]` (hasta 3,
  con la regla de prioridad y desempate de RF-3), pasa el resto a supuestos explícitos y deja que la
  persona decida si aprueba la spec así.

Modo ligero
- RF-7: CUANDO el planner recibe una petición de cambio, EL SISTEMA rellena la lista de las 8
  condiciones con un sí/no y una línea de evidencia por condición, y propone modo ligero o completo.
- RF-8: CUANDO las 8 condiciones se cumplen, EL SISTEMA propone modo ligero y pide a la persona
  una sola confirmación; las condiciones son: (1) toca 3 archivos o menos, sin contar réplicas;
  (2) ningún archivo compartido con otra spec en curso; (3) no cambia reglas globales (Guardrails,
  constitución, numeración y los contratos entre piezas que citan los índices de `AGENTS.md`); (4) sin decisión con alternativas reales, es
  decir, sin ADR; (5) sin datos ni esquema, es decir, sin `sql/`; (6) sin requisitos de
  comportamiento nuevos, es decir, que no cambia qué hace o decide un comando, agente o chequeo
  (corrección de texto, bug acotado o ajuste de lo ya especificado); (7) estimado de 1 hora o
  menos, juzgado solo con el estimado al planear, no con el tiempo real; (8) un solo módulo: archivos
  de una sola carpeta de primer nivel de `template/.agents` (`agents`, `commands` o `skills`), más sus
  réplicas. Los umbrales de 3 archivos y 1 hora son fijos y provisionales.
- RF-9: CUANDO no se cumple alguna condición o no se puede comprobar, EL SISTEMA propone modo
  completo sin pedir más confirmación que la habitual.
- RF-10: CUANDO la persona confirma modo ligero, EL SISTEMA crea `01_spec.md` corta (requisitos y
  criterios), `04_tasks.md` con el plan dentro y `05_avance.md` con tiempos, y no crea
  `02_analisis_impacto.md` ni `06_flujo.md`.
- RF-11: MIENTRAS una spec está en modo ligero, EL SISTEMA permite una sola pasada de clarify y
  mantiene las puertas de aprobación de la spec y de las tareas.
- RF-12: CUANDO durante el trabajo en modo ligero el `coordinator`, al cerrar cada paso, detecta
  que se incumple alguna de las 8 condiciones (salvo la 7, que no se reevalúa), EL
  SISTEMA detiene el avance en ligero, avisa cuál condición se incumplió y promueve el cambio a
  completo con `/sdd-change`; no continúa en ligero.
- RF-13: CUANDO se crea una spec, EL SISTEMA anota el modo (ligero o completo) en `05_avance.md`
  junto con la lista de condiciones evaluada, para que `/sdd-status` y los demás comandos sepan qué
  documentos esperar.
- RF-14: CUANDO un comando o la skill `sdd` encuentra una carpeta con documentos ausentes por ser
  ligera o por ser anterior a esta spec, EL SISTEMA no la trata como incompleta ni exige crear los
  documentos faltantes.
- RF-15: CUANDO el cambio que se pide en modo ligero se distribuye (toca `template/` o los docs
  distribuibles), EL SISTEMA lo mantiene elegible para ligero y exige, como tarea de `04_tasks.md`, el
  bump de `VersionAgentKitSdd` y la entrada en `ChangelogAgentKitSdd.md`, como manda la constitución.

Revisión cruzada (opcional, priorizable)
- RF-16 (opcional, entra en 0.7.0 solo si el plan la estima en menos de 30 min; si no, pasa a la deuda con visto bueno de la persona): CUANDO la persona pide la revisión cruzada, EL SISTEMA ejecuta el reviewer en
  modo de solo lectura sobre spec, plan y tareas y lista requisitos sin tarea y contradicciones
  entre los tres documentos, sin modificar ningún archivo y sin crear un comando nuevo.

Medición y distribución
- RF-19: CUANDO se unifica el marcador en `[ACLARAR: …]`, EL SISTEMA actualiza la plantilla de
  `01_spec.md` en `.agents/skills/sdd/plantillas.md` (sección «Dudas abiertas») y todos los archivos
  que la citan o usan `[NECESITA ACLARACIÓN]`, con sus réplicas. Transición: esta spec se rige por
  la convención vigente (`[NECESITA ACLARACIÓN]`); `[ACLARAR: …]` aplica desde que se publique 0.7.0.
- RF-17: CUANDO se cierra esta spec, EL SISTEMA deja en `05_avance.md` o en el plan una medición en
  bytes, antes y después, sobre un caso de referencia definido, con la métrica y su límite
  declarados.
- RF-18: CUANDO se publica la versión, EL SISTEMA actualiza las réplicas, `VersionAgentKitSdd`
  a 0.7.0 y `ChangelogAgentKitSdd.md`, e indica qué debe incorporar a mano un proyecto ya instalado
  en `AGENTS.md`.

## Requisitos no funcionales
- Solo Markdown, UTF-8 sin BOM, en español. Sin scripts ni código ejecutable (ADR 004).
- Kit genérico: nada de stack ni dominio de un proyecto en `template/`.
- Cada regla vive en una sola fuente: el tope, el umbral y la lista de condiciones se escriben una
  vez (en la skill `sdd`) y los comandos la enlazan; no se copia.
- El ahorro no puede bajar la calidad: se conservan las puertas de aprobación y los Guardrails.
- Las lecturas por paso optimizadas en `003_OptimizarArnes` no cambian ni crecen.
- Métrica de ahorro: bytes que el proceso lee y escribe en el caso de referencia (documentos
  generados más pasadas de clarify), antes y después. Límite declarado: el kit no mide tokens
  reales; los bytes son un indicador aproximado, y el número de pasadas y de documentos se informa
  aparte.
- Los umbrales de 3 archivos y 1 hora quedan fijos y provisionales. Se contrastan con las specs `001`
  a `0010` (archivos tocados sin réplicas y minutos reales de sus `05_avance.md`); si la calibración
  propone otros, se cambian con `/sdd-change` antes de aprobar el plan.

## Casos límite
- Petición ligera que luego crece: se promueve a completo con `/sdd-change` (RF-12). Lo ya
  escrito en `01_spec` y `04_tasks` se conserva y se completan `02`, `03` y `06` en lugar de
  rehacerlos.
- Spec ligera con archivos compartidos con otra spec en curso: incumple la condición 2, por lo
  que va a completo desde el inicio. Si la coincidencia aparece después, aplica la promoción.
- Tope de clarify alcanzado con hallazgos bloqueantes: no se abre una pasada 4 automática (RF-6);
  los hallazgos de alcance y seguridad pasan a marcadores (hasta 3), el resto a supuestos, y decide la persona.
- Más de 3 dudas bloqueantes de igual impacto: se conservan 3 y las demás se vuelven supuestos
  explícitos; la persona puede revertir cualquier supuesto en la aprobación de la spec.
- Estimado de 1 hora que no se puede comprobar o condición ambigua: ante la duda, completo.
- Proyectos ya instalados: `AGENTS.md` es grupo B, así que los comandos y la skill `sdd` se
  actualizan con el kit y cada proyecto incorpora a mano lo que cambie en `AGENTS.md`. Hasta
  hacerlo, sigue operando el flujo completo.
- Coexistencia con specs antiguas de 6 documentos (y la `002_Kit050`, que solo tiene `01` y `05`):
  siguen válidas, no se migran ni se reclasifican, y no cuentan como pasadas ni modos retroactivos.
- Spec ligera cuyo clarify revela una ambigüedad que exigiría más de una pasada: se promueve a
  completo en lugar de abrir una segunda pasada.
- Un marcador `[ACLARAR: …]` olvidado al aprobar la spec: `/sdd-plan` lo detecta y bloquea (RF-4).
- Cambio que toca `template/` y su réplica en la raíz: la réplica no cuenta para la condición 1,
  pero `diff -rq template/.agents .agents` debe seguir limpio salvo `napkin.md` y `skills/caveman`.

## Fuera de alcance
- Scripts `.sh` o `.ps1` (ADR 004: el kit es solo Markdown).
- Generar archivos por herramienta en lugar de enlaces simbólicos (ADR 009, fuente única).
- ISO 8859-1 (el kit es UTF-8 sin BOM).
- CLI en Python y las ~50 integraciones de agentes de spec-kit.
- Un comando `/sdd-checklist` (prioridad baja; `seguridad-basica` ya cubre una parte).
- Un comando nuevo `/sdd-analyze`: el punto 3 se resuelve solo como modo del reviewer.
- Cambiar las lecturas por paso ya optimizadas en `003_OptimizarArnes`.
- Migrar o reescribir specs cerradas.
- Medir tokens reales.

## Criterios de finalización
1. El tope de 3 pasadas, el máximo de 3 marcadores, la prioridad de impacto y el bloqueo de
   `/sdd-plan` están escritos en una sola fuente y enlazados desde `sdd-clarify`, `sdd-spec` y
   `sdd-plan` (RF-1 a RF-6), y la plantilla de `plantillas.md` y los archivos que citan el marcador
   anterior usan `[ACLARAR: …]` (RF-19).
2. La lista de 8 condiciones y la regla de promoción están escritas en una sola fuente; `sdd-spec`,
   `sdd-tasks`, `sdd-change` y `sdd-status` reconocen el modo ligero (RF-7 a RF-14).
3. Los umbrales de 3 archivos y 1 hora (fijos y provisionales) se contrastan con una tabla de las specs `001` a `0010`
   (archivos sin réplicas, minutos reales y modo que habrían tenido), y se declara cuáles habrían sido
   ligeras.
4. Medición en bytes, antes y después, sobre el caso de referencia, con la métrica y su límite
   declarados, al estilo de `003_OptimizarArnes` (RF-17). El caso se define en el plan; la meta
   numérica de ahorro no se fija: se declara el resultado medido y el límite de la métrica.
5. Cada archivo tocado pasa los chequeos de `AGENTS.md` › Comandos: `file -i` indica UTF-8, no hay
   BOM, `name` del frontmatter igual a su carpeta, y `diff -rq template/.agents .agents` solo
   muestra `napkin.md` y `skills/caveman`.
6. El ensayo de instalación no pisa nada, según `INSTALAR.md`.
7. `VersionAgentKitSdd` pasa a 0.7.0 y `ChangelogAgentKitSdd.md` registra la versión, incluida la nota
   para proyectos ya instalados sobre lo que cambia en `AGENTS.md` (RF-18).
8. Las specs existentes (`001` a `0010`) no cambian de contenido.
9. Si el punto 3 se aprueba, el modo de revisión cruzada existe en el reviewer, es de solo lectura
   y no añade ningún comando; si no, se registra como diferido en la deuda (RF-16).
10. Esta spec se cierra con `/sdd-cierre`: fila del índice al día y ADR si hubo decisión con
    alternativas.

## Dudas abiertas
Resueltas por la persona usuaria el 2026-11-03: tope con hallazgos pendientes (RF-6), modo ligero
para cambios que se distribuyen (RF-15), marcador unificado (RF-19), definición de pasada (Definiciones),
sin meta numérica de ahorro (se mide en el plan y se declara el resultado con el límite de la
métrica) y condición de entrada de RF-16.
Sin dudas abiertas.

## Pendiente para el plan
Hallazgos menores del clarify, pasada 1, sin corregir en la spec:
- Hallazgo 5 (RF-10): «spec corta» no tiene medida; el plan fija qué secciones de la plantilla se omiten y un límite de longitud.
- Hallazgo 6 (RF-17, criterio 4): «caso de referencia» sin criterio de selección; el plan fija cuántas pasadas de clarify asume el escenario «antes» y separa los bytes de documentos de las pasadas de clarify en la métrica.
- Hallazgo 8 (RF-4, RF-5, RF-14): en modo ligero no hay `03_plan.md`; el plan define si el bloqueo por marcadores se aplica en `/sdd-tasks` y dónde.
- Hallazgo 9 (RF-11, RF-12, RF-1): al promover un ligero a completo, el plan define si la pasada ya hecha cuenta como pasada 1 del completo.
- Hallazgo 10 (RF-12, RF-13): al promover un ligero a completo, el plan define cómo se corrige el modo registrado en `05_avance.md` y qué ven los comandos que esperan documentos según el modo (RF-14) durante el estado intermedio.
- Hallazgo 11 (RF-15, condición 1): el plan define si el bump de versión y el changelog (`VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, que no son réplicas) cuentan como archivos tocados en la condición 1.
- Hallazgo 12 (RNF fuente única, criterio 1): el plan confirma que `SKILL.md:97` y `plantillas.md:25` quedan como únicas fuentes del marcador y que el resto de archivos enlaza sin copiar el contenido (principio 3).
- Editar también en este repo la nota de `AGENTS.md` (línea 105) que exige seis documentos por feature, para que admita el modo ligero.
