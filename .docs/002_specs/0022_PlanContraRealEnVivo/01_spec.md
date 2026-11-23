# 01 — Spec: Plan contra real en vivo
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
Hoy el estimado de cada fase vive en el plan (`03_plan.md` › Fases; en modo ligero, `04_tasks.md`) y
lo real, fila por fila, en la tabla Tiempos de `05_avance.md`. La tabla «Plan contra real» solo se
escribe en el cierre. Para saber a media implementación cómo va el tiempo hay que abrir dos archivos
y sumar a mano.

Palabras de la persona: «el avance registra los tiempos de tarea vs plan. Al final se corre Plan vs
avance, pero ambos datos ya los tenemos (plan) y vamos llenando los datos de avance. Si en un punto
quiero saber cómo vamos en cuestión de tiempo, tengo que leer el avance y el plan, cuando debería
poder leerlo en el mismo avance.» Y en la entrevista: «practicamente es poner todos los puntos del
plan/task e ir llenando con los tiempos que van saliendo».

Objetivo: abrir solo `05_avance.md` en cualquier momento de la implementación y ver, por fase y por
paso, el estimado, lo real acumulado y el delta, con los totales a la vista en el PUNTO DE RETOMO.

## Usuarios y roles
- Persona que lleva el feature: consulta cómo va el tiempo sin abrir el plan.
- Sesión principal (o coordinator como sesión): siembra y actualiza los tiempos al registrar cada
  tramo; es quien ya escribe `05_avance.md`.
- `/sdd-status`: informa las horas a partir de lo ya escrito.
- `/sdd-cierre`: completa lo vivo en vez de crear otra tabla.

## Historias de usuario
- HU-1. Como persona que lleva el feature, quiero ver en el avance cada fase y paso del plan con su
  estimado y lo real que se va registrando, para saber cómo vamos sin abrir el plan ni sumar.
- HU-2. Como persona, quiero los totales (estimado, real, delta) en el PUNTO DE RETOMO, para verlos de
  un vistazo al retomar.
- HU-3. Como persona, quiero que el estado y el cierre usen esos mismos números, para no tener dos
  cuentas distintas.

## Definiciones
- **Tabla Pasos con tiempos:** la tabla Pasos de `05_avance.md` (espejo de `04_tasks.md`) con tres
  columnas más: Estimado, Real y Delta.
- **Fila de fase:** fila de una fase `Fn` en la tabla Pasos con tiempos; lleva el estimado de la fase
  y el subtotal real de sus pasos.
- **Fila de paso:** fila de un paso `Fn.m`; lleva su estado y su real.
- **Real:** minutos de la tabla Tiempos (columna Minutos), nunca un cálculo nuevo ni una estimación.
- **Delta:** real menos estimado, en la unidad del estimado.
- **Fila «Tiempo»:** fila del PUNTO DE RETOMO con los totales.

## Requisitos funcionales (EARS)
- RF-1: CUANDO se aprueban las tareas, EL SISTEMA escribe en `05_avance.md` la tabla Pasos con
  tiempos: una fila por fase con su estimado tomado del plan (modo completo: `03_plan.md`; modo
  ligero: `04_tasks.md`), una fila por paso, una fila «Total implementación» y una fila
  «Documentación SDD» con estimado «sin estimado».
- RF-2: CUANDO la sesión principal escribe en Tiempos la fila de un paso `Fn.m` o de una fase `Fn` que
  existe en las tareas (de implementación o de prueba), EL SISTEMA anota en la misma escritura el real
  en la fila de ese paso y actualiza el subtotal real y el delta de su fase y del «Total
  implementación».
- RF-3: CUANDO la fila de Tiempos es de un lote, EL SISTEMA anota su real una sola vez, en la fila de su
  primer paso, y lo suma al subtotal de la fase de ese paso; marca las demás filas de paso del lote con
  «en lote <nombre del lote>». Una fase cuyos pasos registrados están todos en lotes que cuentan en otra
  fase lleva real «en lote» y delta «—». Los lotes paralelos suman cada uno sus minutos.
- RF-4: CUANDO la sesión principal escribe en Tiempos la fila de una fase del flujo (`spec`,
  `clarify`, `plan`, `tasks`, `validate`, `flujo`, `cierre` u otra fase sin id `Fn`) y la tabla Pasos
  con tiempos ya existe, EL SISTEMA actualiza en la misma escritura el real de «Documentación SDD»; al
  sembrar la tabla (RF-1), ese real arranca con la suma de las filas de documentación ya registradas.
  Una fila con id `Fn` o `Fn.m` que no existe en las tareas también se suma aquí, y la sesión lo avisa
  en una línea.
- RF-5: CUANDO se siembra la tabla o se actualiza un real, EL SISTEMA escribe en la misma escritura la
  fila «Tiempo» del PUNTO DE RETOMO con el estimado total de implementación, el real acumulado, el
  delta, el real de documentación y cuántas filas quedan en `pausa por confirmar`.
- RF-6: MIENTRAS una fila de Tiempos esté en `pausa por confirmar`, EL SISTEMA no la suma al real; CUANDO
  se resuelve, EL SISTEMA recalcula en esa misma escritura el real de su fase y la fila «Tiempo».
- RF-7: SI una fase aún no tiene filas en Tiempos, EL SISTEMA deja su real vacío y su delta como
  «pendiente». El estimado del «Total implementación» es la suma de todas las fases; su delta compara
  el real solo contra el estimado de las fases que ya tienen real o real «en lote» (RF-3).
- RF-8: SI el plan no trae estimado para una fase, EL SISTEMA anota «sin estimado» y delta «—», sin
  inventarlo.
- RF-9: CUANDO se registra un paso y la carpeta no tiene la tabla Pasos con tiempos (carpeta anterior a
  esta versión), EL SISTEMA la siembra en ese momento una sola vez, según RF-1, con el real de las filas
  de Tiempos ya escritas.
- RF-10: CUANDO un cambio de requisito reaprobado altera fases, pasos o estimados, EL SISTEMA reescribe
  los estimados y agrega las filas nuevas conservando lo real ya anotado; una fase o paso retirado con
  real conserva su fila marcada «retirado», su real sigue sumando en su fase y en el total, y una fase
  retirada no aporta estimado.
- RF-11: CUANDO `/sdd-status` informa horas y existe la tabla Pasos con tiempos, EL SISTEMA toma
  estimado, real y delta de esa tabla y de la fila «Tiempo», sin leer el plan ni volver a sumar
  Tiempos; sin la tabla, informa como hoy.
- RF-12: CUANDO `/sdd-cierre` escribe el plan contra real, EL SISTEMA recalcula la tabla Pasos con
  tiempos desde Tiempos, la completa y escribe en la sección «Plan contra real» del avance una sola
  línea de qué ajustar en el siguiente plan, o «sin ajuste»; no crea una segunda tabla. Sin tabla viva,
  la crea como hoy.
- RF-13: CUANDO existe la tabla Pasos con tiempos, EL SISTEMA la reconoce como la tabla de plan contra
  real del criterio 1 de la leyenda de estatus; sin ella, rige la tabla de la sección «Plan contra
  real», como hoy.
- RF-14: EL SISTEMA no cuenta las filas de fase, «Total implementación» ni «Documentación SDD» como
  pasos: el conteo de pasos hechos y pendientes (estado, leyenda de estatus, etapa de la carpeta) sigue
  saliendo solo de las filas de paso.

## Requisitos no funcionales
- Sin llamadas extra: sembrar y actualizar van en la misma escritura que la fila de Tiempos (y que la
  entrada de la bitácora, si está activa), según `MEMORY.md` (2026-11-22).
- Fuente única: la regla vive en un solo documento del contrato de avance; los comandos y el
  coordinator la nombran y no la copian.
- Tras sembrar, la sesión no relee el plan para actualizar lo real; solo lo relee al reaprobar tras un
  cambio (RF-10).
- Lo real sale solo de la tabla Tiempos; si no cuadra, manda Tiempos.

## Casos límite
- Lote cuyos pasos son de fases distintas: cuenta completo en la fase de su primer paso (RF-3).
- Ola de lotes paralelos: cada lote suma sus minutos aunque en reloj se solapen.
- Paso trivial con Nota «sin trabajo real»: real 0 y cuenta como registrado (no «pendiente»).
- Fila con pausa mayor que 0: entra su columna Minutos, ya descontada la pausa.
- Fila «Pausa» de `/sdd-pausa`: no crea tiempo ni cambia la tabla.
- Estimado en rango (`2–3 h`): el delta se calcula contra el extremo superior (supuesto S2).
- Estimados de fases en unidades distintas (min y h): el total se expresa en la unidad mayor, con
  conversión explícita.
- Modo ligero promovido a completo: si las fases cambian, aplica RF-10.
- Paso diferido con decisión y deuda: deja de contar como paso, pero su real ya registrado se conserva
  en su fase.
- Fila de Tiempos con id que no existe en las tareas: RF-4.
- Paso de prueba `Fn.m`: su tiempo suma en su fase (RF-2), aunque la leyenda de estatus no lo cuente
  como paso.
- Modo ligero con un solo estimado total en `04_tasks.md`: las fases llevan «sin estimado» y el
  «Total implementación» lleva ese total; su delta compara el real acumulado contra ese total (excepción
  a RF-7).
- Spec en modo «solo análisis» (termina en `plan`, sin tareas): no hay tabla Pasos con tiempos ni fila
  «Tiempo»; el tiempo de documentación queda solo en Tiempos, como hoy.

## Fuera de alcance
- Aviso de desvío en el momento: la persona eligió «sin aviso»; el desvío solo se ve en la tabla.
- Cambiar cómo se estima (por lote u ola, ajuste de los estimados altos): queda en `MEMORY.md` y, si se
  quiere regla, en una deuda o spec aparte.
- Tokens y costo: los cubre `0021_BitacoraDeUso`.
- Reescribir las tablas de plan contra real de specs ya cerradas.

## Criterios de finalización
- Con una spec de prueba, tras aprobar las tareas, `05_avance.md` muestra cada fase y paso con su
  estimado, y la fila «Tiempo» en el PUNTO DE RETOMO, sin abrir el plan.
- Tras registrar un paso, un lote y una fase de documentación, los reales, subtotales, deltas y la fila
  «Tiempo» cuadran con la tabla Tiempos.
- Una fila en `pausa por confirmar` no entra al total hasta resolverse.
- `/sdd-status` informa las horas desde la tabla sin leer el plan.
- `/sdd-cierre` deja una sola tabla de plan contra real (criterio 1 de la leyenda).
- Los chequeos de `AGENTS.md` › Comandos pasan en cada archivo tocado.

## Dudas abiertas
Supuestos explícitos (la persona puede revertirlos al aprobar):
- S1. La tabla Pasos con tiempos **es** la tabla «Plan contra real» del cierre: el cierre la completa y
  el criterio 1 de la leyenda de estatus se cumple con ella (una sola tabla, sin duplicar; RF-13). La
  sección «Plan contra real» del avance queda solo para la línea de ajuste (RF-12).
- S2. Con estimado en rango, el delta se calcula contra el extremo superior.
- S3. Los pasos no llevan estimado propio: el estimado es por fase, como hoy (la persona eligió «Pasos
  con tiempos», no «Estimado también por paso»).
- S4. Una carpeta en curso sin la tabla la gana al registrar su siguiente paso (RF-9); las cerradas no
  se tocan.
- S5. El cierre escribe siempre una línea de ajuste (o «sin ajuste») en vez de juzgar un «desvío
  fuerte» sin umbral (RF-12).
- S6. Una fase «sin estimado» con real suma su real al «Total implementación» y no aporta estimado:
  el delta del total lo incluye y no se inventa un estimado (RF-7, RF-8). Queda de la pasada 3 (tope).
