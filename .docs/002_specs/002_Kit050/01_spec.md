# 01 — Spec: Registro retroactivo de la versión 0.5.0
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo
La versión 0.5.0 se publicó el 2026-10-24 (commits `b13e406` a `1c35f0c`) sin carpeta de plan,
sin requisitos escritos y sin tiempos. Eso contradice el principio 2 de la constitución. Las
decisiones sí quedaron en los ADR 001, 005 y 006 y en el registro de cambios de 0.5.0.

Objetivo: dejar por escrito el qué y el por qué de lo que **ya está publicado** en 0.5.0, para
que el próximo ajuste del kit tenga requisitos contra los cuales contrastar. Este documento no
pide comportamiento nuevo y no reconstruye el plan ni las horas de aquel trabajo.

Los ADR 002, 003 y 004 se escribieron en los mismos commits, pero la decisión que registran es
de 0.4.0. No forman parte de los requisitos de esta spec.

## Usuarios y roles
- **Mantenedor del kit:** publica decisiones y skills de equipo, y trabaja el clon.
- **Responsable del proyecto:** elige cómo se instala el kit.
- **Agente:** aplica las skills, valida la estructura y no ejecuta correcciones ni scripts de
  datos sin confirmación.

## Historias de usuario
- HU-1. Como mantenedor, quiero que una decisión de arquitectura quede registrada con sus
  alternativas, para no volver a discutirla de memoria.
- HU-2. Como mantenedor, quiero skills de equipo para decisiones, scripts de datos y una
  revisión ligera de secretos, para no improvisarlas en cada proyecto.
- HU-3. Como mantenedor, quiero poder comprobar que la estructura del kit sigue coherente, para
  detectar réplicas y enlaces rotos sin que el agente los corrija solo.
- HU-4. Como responsable, quiero que al abrir el clon del kit me pregunten si instalo en un
  proyecto o trabajo el kit, para no pisar el conocimiento propio del clon.

## Definiciones
- **Conocimiento propio del kit:** reglas, memoria, notas de trabajo del agente, specs y
  decisiones de arquitectura. Se versiona con el clon. No es lo que se copia a otros proyectos.
- **Copia de la herramienta:** agentes, comandos y skills que salen del producto y viven en
  local en el clon, salvo las notas de trabajo del agente.
- **Réplica:** skill, agente o comando que aparece en la carpeta de una herramienta y también
  (o solo) en la copia canónica.

## Requisitos funcionales (EARS)
- RF-1: CUANDO se toma una decisión de arquitectura con alternativas reales y efecto duradero,
  EL SISTEMA DEBE registrarla con contexto, alternativas descartadas y consecuencias, y NO DEBE
  crearla sin el visto bueno de quien decide.
- RF-2: CUANDO un plan o un cierre detecta una decisión de ese tipo, EL SISTEMA DEBE proponer
  el registro y esperar confirmación.
- RF-3: CUANDO un cambio incluye scripts de datos, EL SISTEMA DEBE escribirlos con orden,
  encabezado de ejecución manual, idempotencia y rollback, y el agente NO DEBE ejecutarlos.
- RF-4: CUANDO se prepara un commit o se valida un feature, EL SISTEMA DEBE buscar secretos en
  lo que se va a versionar y recorrer un checklist corto de riesgos, y NO DEBE presentar eso
  como una auditoría ni como sustituto de una revisión de seguridad dedicada.
- RF-5: EL SISTEMA DEBE incluir en el producto las skills de equipo de decisiones de
  arquitectura, de scripts de datos y de revisión ligera de secretos, conectadas al flujo de
  plan, cierre, commit y validación.
- RF-6: EL SISTEMA NO DEBE incluir en el producto una skill de revisión de código con el nombre
  de la integrada en la herramienta. Tampoco entran las de pruebas, notas de versión ni
  revisión de sprint (quedan diferidas), ni las de riesgos y onboarding. Las de spec desde
  ticket y de minuta, si se retoman, son de la capa de usuario y no de esta versión.
- RF-7: CUANDO el usuario pida validar la estructura del kit, EL SISTEMA DEBE revisar enlaces,
  réplicas, skills, agentes, comandos, índices y codificación, y proponer correcciones sin
  aplicarlas.
- RF-8: CUANDO se agrega una skill, un agente o un comando, EL SISTEMA DEBE sugerir dejarlo en
  la copia canónica y replicarlo hacia cada herramienta en uso, o excluirlo en local si solo
  está en prueba, y NO DEBE borrar ni mover nada sin confirmación.
- RF-9: CUANDO se abre el clon del kit o el destino es un proyecto nuevo, EL SISTEMA DEBE
  preguntar si se usa el kit en un proyecto o se trabaja el propio kit, y NO DEBE copiar hasta
  tener una respuesta clara.
- RF-10: CUANDO el modo es proyecto, EL SISTEMA DEBE instalar solo lo distribuible, sin
  sobrescribir lo que ya existe.
- RF-11: CUANDO el modo es mantenimiento, EL SISTEMA DEBE conservar el conocimiento propio ya
  versionado en el clon y dejar en local la copia de la herramienta, con las exclusiones
  ancladas de modo que no oculten el producto.
- RF-12: EL SISTEMA NO DEBE incluir Graphify ni otra herramienta que exija código ejecutable,
  un intérprete o hooks que escriban sus propias instrucciones.

## Requisitos no funcionales
- RNF-1: El kit sigue siendo solo Markdown, sin scripts de instalación versionados.
- RNF-2: Textos en español y archivos en UTF-8 sin BOM.
- RNF-3: Nada propio de un proyecto (stack, nombres, dominio, estas specs o estos ADR) entra
  en lo que se distribuye.
- RNF-4: Este registro no inventa tiempos del trabajo ya publicado.

## Casos límite
- Una decisión de una línea o una trampa recurrente no es un ADR: sigue en la memoria del
  proyecto.
- Una skill en prueba local no es un fallo de estructura: se excluye y no se versiona.
- Si la respuesta al modo de instalación no es clara, se vuelve a preguntar.
- Un clon nuevo conserva el conocimiento propio versionado; la copia de la herramienta se
  instala en local y puede quedar detrás del producto hasta un refresco aparte.

## Fuera de alcance
- Reconstruir horas, análisis de impacto, plan o tareas del trabajo ya commiteado.
- Cambiar el comportamiento publicado en 0.5.0.
- El procedimiento para actualizar un proyecto ya instalado (spec `001_ActualizarKit`).
- El refresco de la copia local del clon cuando el producto cambia (deuda 001, posterior a
  estos commits).
- Reabrir o reescribir los ADR 002, 003 y 004.
- Editar el registro de cambios o el número de versión: el cierre de `001_ActualizarKit` es
  quien refleja ahí el procedimiento de actualización.
- Incorporar Graphify más adelante.

## Criterios de finalización
- Los RF-1 a RF-12 se pueden contrastar con el registro de cambios de 0.5.0 y con los ADR 001,
  005 y 006, sin comportamiento que esos documentos no respalden.
- La carpeta existe con esta spec y con `05_avance.md`.
- `05_avance.md` no contiene minutos reconstruidos del trabajo de los commits `b13e406` a
  `1c35f0c`.
- La deuda 002 queda en estado `Resuelta` y apunta a esta carpeta.

## Dudas abiertas
Ninguna.
