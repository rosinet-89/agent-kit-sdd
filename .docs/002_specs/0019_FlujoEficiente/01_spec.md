# 01 — Spec: Flujo eficiente (lanzamientos, cascada, clarify por delta y huella)
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
El reporte de sesiones del 2026-11-21 y 2026-11-22 (specs 0013 a 0018) midió 1 148 llamadas de la
sesión principal contra 495 de 90 subagentes. La sesión concentra el 94 % de la lectura de caché.
Fallas de flujo que el kit puede evitar:

- Lanzamientos que la regla de `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol» no admite: un
  `reviewer` Opus en subagente, con sesión Opus, en la validación de 0017 (1.2 M de lectura de
  caché; se detuvo y se rehízo en el chat).
- Agentes genéricos fuera del kit: `general-purpose` para la línea base y el chequeo posterior de
  estructura en 0017 (3.3 M de lectura de caché, 3 errores), aunque `sdd-estructura` dice que no
  lanza subagentes.
- Un «procede» de la persona por cada paso: F1.1, F1.2 y F2.1 de 0015. Y un `implementer` por paso
  en 0013 (38 lanzamientos para 42 pasos; cada arranque en frío escribe de 15 000 a 20 000 tokens de
  caché). 0017 agrupó 36 pasos en 12 lanzamientos sin perder calidad.
- Clarify de 3 pasadas completas en 0013, 0014 y 0017 (primeras pasadas de 14 a 31 hallazgos), cada
  una sobre la spec entera.
- Huella del manifiesto vieja al cerrar (0014; trampa de `MEMORY.md` del 2026-11-21, dos veces en 0014).

Objetivo: menos tokens y menos espera por spec, sin bajar la verificación. La validación sigue RF
por RF, con tester y reviewer.

## Usuarios y roles
- Persona que usa el kit: aprueba spec y tareas, pide la cascada y confirma los commits.
- Sesión principal (o coordinator como sesión): decide dónde corre cada rol y lanza los subagentes.
- `planner`: autochequeo de la spec y correcciones de clarify.
- `reviewer`: pasadas de clarify.
- `implementer`: pasos o lotes de pasos.

## Historias de usuario
- HU-1. Como persona, quiero que el flujo no lance subagentes que la regla no admite, para no pagar
  dos veces el mismo trabajo.
- HU-2. Como persona, quiero aprobar las tareas y pedir la implementación una sola vez, para no
  escribir «procede» en cada paso.
- HU-3. Como persona, quiero que clarify revise lo que cambió y no la spec entera en cada pasada,
  para que las pasadas 2 y 3 cuesten menos.
- HU-4. Como mantenedor del kit, quiero que el cierre detecte una huella vieja del manifiesto, para
  no publicar una versión con filas que no coinciden.

## Definiciones
- **Fase SDD:** cualquiera de `spec`, `clarify`, `plan`, `tasks`, `implement`, `validate`, `flujo`
  y `cierre`, y los comandos `/sdd-*` que las corren.
- **Rol del kit:** `planner`, `coordinator`, `reviewer`, `implementer`, `tester` o `explore`.
- **Lote:** varios pasos de `04_tasks.md` que se dan a un solo lanzamiento del `implementer`.
- **Ola:** lotes sin archivos en común que se lanzan en paralelo.
- **Lo ajeno:** lo que la sesión no puede resolver sola: un `sql/` que ejecuta la persona, una
  verificación manual, una decisión o visto bueno, un commit.
- **Pasada por delta:** pasada de clarify que revisa solo lo que cambió desde la pasada anterior y
  lo que lo cita.

## Requisitos funcionales (EARS)

### Lanzamientos
- RF-1: ANTES de lanzar un subagente en una fase SDD, quien lanza DEBE comprobar que se cumplen dos
  condiciones. (a) Es un rol del kit. (b) Se da una de estas cuatro: el modelo del rol no es el de
  la sesión; la persona lo pidió; el rol está en `.agents/local/configuracion.md` ›
  `## Subagentes`; o es el relanzamiento de un rol Haiku que no entregó. SI alguna condición no se
  cumple, el rol corre en el chat. La regla vive en `SKILL.md` › «Dónde corre cada rol»; los
  comandos remiten y no la copian.
- RF-2: EL SISTEMA NO DEBE lanzar, dentro de una fase SDD, un agente que no sea rol del kit
  (`general-purpose`, agentes integrados de la herramienta, otros). Una búsqueda acotada va a
  `explore`; lo demás lo hace el chat con el rol de la fase. Hay dos excepciones: la persona lo pide
  de forma explícita, o es el auditor externo activado según `SKILL.md` › «Auditor externo (beta)».
  La constancia dice cuál de las dos aplica.
- RF-3: CUANDO un tramo usa un subagente, la Nota de su fila de tiempos DEBE decir el motivo del
  lanzamiento: «otro modelo», «pedido», «configurado», «relanzamiento» o «auditor».
- RF-4: `04_tasks.md` NO DEBE tener pasos que deleguen a un subagente la validación de estructura
  (línea base o chequeo posterior). Esos pasos citan `/sdd-estructura`, que corre la sesión.

### Cascada y lotes
- RF-5: CUANDO la persona corre `/sdd-implement <feature>` sin paso, o con una fase (`Fn`), EL
  SISTEMA implementa en cascada los pasos pendientes de todo el plan, o de esa fase, sin pedir
  «procede» entre pasos. `/sdd-implement <feature> Fn.m` sigue implementando un solo paso y para.
  SI `04_tasks.md` no está aprobado, EL SISTEMA para sin implementar nada, en cualquiera de las tres
  formas.
- RF-6: ANTES de arrancar la cascada, EL SISTEMA DEBE mostrar una tabla con los pasos pendientes y
  estas columnas: Paso, Qué hago, Lote u ola, Se detiene por. «Se detiene por» lista solo lo ajeno
  o dice «—».
- RF-7: En cascada, EL SISTEMA DEBE agrupar en lotes de hasta 4 pasos pendientes. Un lote junta
  pasos de la misma fase, o de fases distintas que tocan el mismo archivo. Ningún paso del lote puede
  depender de un paso pendiente que no esté en el lote. Tampoco puede pedir, en su «Hecho cuando»,
  verificación manual, evidencia de datos ni un `sql/`: los comandos de lint y de prueba sí se
  permiten. Un paso que no cumple va solo. Lotes sin archivos en común pueden ir en la misma ola.
  Un paso depende de otro si su texto lo nombra, o si toca un archivo que el otro también toca y va
  después en `04_tasks.md`.
- RF-8: CUANDO un lote termina, la sesión DEBE revisar su diff y el «Hecho cuando» de cada paso, y
  marcarlos uno por uno.
- RF-9: SI un paso falla su «Hecho cuando» o llega a algo ajeno, EL SISTEMA detiene ese paso y los
  que dependen de él, sigue con los pasos independientes y al final informa qué falta.
- RF-10: Las filas de tiempos de un lote DEBEN cumplir `avance-y-tiempos.md` › «Mínimo y lote». Es
  una fila por lote: nombra sus pasos, lleva los minutos reales del lote y en la Nota dice que es un
  lote. Esa fila cuenta en la fase de su primer paso.

### Clarify
- RF-11: ANTES de la pasada 1 de clarify, el `planner` DEBE hacer un autochequeo de `01_spec.md` con
  las cuatro listas del reviewer y dos comprobaciones más: que cada RF esté en EARS y que existan las
  rutas citadas. Corrige lo que encuentra. El autochequeo no cuenta como pasada (`topes-y-modos.md`)
  y queda como decisión en `05_avance.md` con el número de correcciones. Es una revisión del autor
  sobre su texto: no sustituye ninguna pasada. La separación entre producir y validar sigue en las
  pasadas del reviewer.
- RF-12: Cada pasada a partir de la 2 DEBE ser por delta. Al corregir, el planner entrega la lista
  de RF, secciones y casos límite que tocó. La sesión se la pasa al reviewer, que revisa esos
  elementos y los que los citan.
- RF-13: SI en una pasada por delta el reviewer encuentra una contradicción entre lo cambiado y una
  parte no cambiada, DEBE reportarla como hallazgo de esa pasada.
- RF-14: El tope de pasadas, el modo de corrección y la aprobación de la persona NO cambian
  (`topes-y-modos.md`).

### Huella del manifiesto (solo repo del kit)
- RF-15: CUANDO `AGENTS.md` › Comandos declara chequeos de cierre, la comprobación previa de
  `/sdd-cierre` DEBE correrlos. SI alguno falla, el cierre no escribe `Cerrado` ni `Implementado` y
  dice cuál falló. Sin chequeos de cierre declarados, la comprobación previa no cambia. Esta parte es
  genérica y entra en `template/`: `template/AGENTS.md` › Comandos lleva la línea «Chequeos de
  cierre: No aplica», que `/sdd-init` deja así o completa.
- RF-16: El `AGENTS.md` de la raíz (repo del kit) DEBE declarar como chequeo de cierre el E20 de
  `validar-estructura.md`. Aplica cuando el requerimiento cambió `VersionAgentKitSdd` y se corre
  después del último cambio a `template/`. SI E20 falla, la sesión regenera las filas de los
  archivos tocados, lo anota como decisión y vuelve a correr E20. E20 y los manifiestos no entran en
  `template/` (constitución, principio 1).

## Requisitos no funcionales
- RNF-1: Fuente única. Lanzamientos en `SKILL.md` › «Dónde corre cada rol». Cascada y lotes en
  `sdd-implement`. Fila de lote en `avance-y-tiempos.md`. Autochequeo y delta en `topes-y-modos.md`.
  Chequeos de cierre en `sdd-cierre` (cuándo correrlos) y `AGENTS.md` › Comandos (cuáles). Lo demás
  remite.
- RNF-2: Ningún Guardrail cambia. Commits solo con confirmación, nunca push.
- RNF-3: El kit sube de versión con changelog y manifiesto (constitución, principio 6).
- RNF-4: `template/` queda genérico. E20 y los manifiestos son del repo del kit.

## Casos límite
- La herramienta no deja elegir el modelo al lanzar: rige «Sin elegir modelo» de `SKILL.md`, sin
  cambio.
- La sesión no conoce su id de modelo: no es el mismo modelo (regla vigente). El lanzamiento cumple
  RF-1 (b).
- Un coordinator que corre como subagente lanza roles: RF-1 y RF-2 aplican a quien lanza.
- La persona pide un subagente para un rol con el mismo modelo: se lanza; la Nota dice «pedido».
- Un lote en el que falla el segundo de tres pasos: el primero queda marcado. El segundo y el
  tercero, si depende de él, quedan `[ ]` o `[!]`; los independientes siguen (RF-9).
- Cascada en un plan con `sql/`: el paso del script se detiene por lo ajeno; los pasos que no
  dependen de los datos siguen.
- Modo ligero (1 pasada de clarify): aplica el autochequeo y la pasada por delta no se usa.
- Corrección de clarify que reescribe la spec casi entera: el delta es toda la spec y la pasada es
  completa.
- Spec que no cambia `VersionAgentKitSdd`: el E20 de RF-16 no aplica.
- Proyecto que no es el repo del kit, con «Chequeos de cierre: No aplica»: el cierre no cambia
  (RF-15).

## Fuera de alcance
- Una sesión por spec, `/clear` o `/compact` entre fases y el modelo de la sesión principal según la
  tarea: son hábitos de cada dev (`PREFERENCIAS.md`), no reglas del kit.
- Conectores MCP y contexto de arranque de la herramienta.
- Estimación por ola: ya está en `MEMORY.md` (2026-11-22).
- Cambiar el tope de clarify o el modo de corrección.

## Criterios de finalización
1. Ninguna fase del kit lanza un subagente que RF-1 o RF-2 no admitan, y la Nota dice el motivo de
   cada lanzamiento.
2. `/sdd-implement <feature>` corre en cascada con lotes y olas, y se detiene solo en lo ajeno o en
   un fallo.
3. Clarify hace el autochequeo antes de la pasada 1 y revisa por delta desde la pasada 2.
4. En el repo del kit, el cierre de una versión no pasa con E20 en falla.
5. Lint de `AGENTS.md` › Comandos en cada archivo tocado, ensayo de instalación sin pisar nada,
   réplica `template/.agents` = `.agents` salvo las excepciones aceptadas, versión, changelog y
   manifiesto.

## Dudas abiertas
Supuestos explícitos (la persona puede revertirlos al aprobar):
- S-1: `/sdd-implement <feature>` sin paso pasa a ser cascada de todo el plan (antes no estaba
  definido). La forma de un paso no cambia.
- S-2: Máximo 4 pasos por lote. Se revisa tras 3 usos, como los umbrales del modo ligero.
- S-3: El enganche es genérico: `sdd-cierre` corre los chequeos de cierre que declare
  `AGENTS.md` › Comandos (RF-15). El E20 solo lo declara la raíz del repo del kit (RF-16).
