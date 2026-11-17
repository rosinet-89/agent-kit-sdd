# 01 — Spec: Estatus de spec y criterio de cerrado
Estado: implementada
Ticket: issue #5

## Contexto y objetivo
El índice no tiene estatus terminal. El cierre cambia el estatus sin nombrarlo y deja el documento en `implementada`. `Implementado` mezcla trabajo validado, pasos sueltos de cierre y trabajo tomado por terminado. Hay palabras fuera de la lista y la celda se desfasa de la carpeta. El documento no tiene terminal. La deuda no dice qué estados están cerrados ni cuál frena el cierre. Dos agentes, con el mismo índice, no coinciden en qué está pendiente.

Objetivo: definir cerrado, pendiente, pendiente de desarrollo y pendiente sin clasificar en el índice y en el documento. Qué está pendiente sale de la celda. Al consultar una carpeta manda la carpeta, y el chequeo avisa del desfase. No hay un validador automático.

## Usuarios y roles
- Persona: confirma ticket, commit y deuda, y decide descarte, reapertura y pausa.
- Apertura: crea la fila en `Análisis`.
- Cierre: escribe `Implementado`, `Cerrado` y `Descartado`, y juzga la previa y los criterios solo al escribir.
- Estado: consulta una carpeta y no modifica archivos.
- Agente: responde el pendiente de todas las filas desde las celdas, sin abrir cada carpeta.
- Chequeo de estructura: avisa, no frena y no escribe.

## Historias de usuario
- HU-1. Como persona, quiero saber por la celda del índice qué requerimientos siguen pendientes.
- HU-2. Como agente, quiero dejar `Implementado` si falta un criterio de cierre y no escribir estatus si falla la comprobación previa.
- HU-3. Como persona, quiero que una deuda `En atención` atendida por este requerimiento impida cerrarlo.

## Definiciones (solo si hay términos ambiguos)
Se conservan `Análisis`, `Planificado`, `En curso`, `En pruebas`, `Implementado` y `Diferido`. Se añaden `Cerrado` y `Descartado`.

La celda, al escribirla, empieza por una de esas palabras, exacta en mayúsculas y tildes, seguida de ` (AAAA-MM-DD): ` y de un detalle no vacío. La fecha es la del último avance y es la única. La respuesta del índice es la celda: no la sustituye la etapa de la carpeta.

| Estatus | Cuándo se usa | ¿Cuenta como pendiente? | Grupo al resumir |
|---|---|---|---|
| `Descartado` | No se hará. Bien formado: forma de la celda, documento en `descartada`, motivo y quién. | no | no se hará |
| `Cerrado` | Terminado. Bien formado: forma de la celda, documento en `cerrada` y una sola fila `cierre`. Leer no juzga los criterios. | no | cerrado |
| `Diferido` | Pausado, con motivo y estatus de regreso. La línea del documento no cambia. | sí, y se informa aparte | aparte |
| `Implementado` | Validado, con un criterio de cierre abierto. | sí | por cerrar |
| `En pruebas` | Falta la validación del conjunto o el flujo que el modo exige. | sí | desarrollo |
| `En curso` | Implementación empezada y no terminada. | sí | desarrollo |
| `Planificado` | Hay plan y no hay implementación empezada. | sí | diseño |
| `Análisis` | Redacción, aclaración o aprobada aún sin plan. | sí | diseño |
| `sin clasificar` | Palabra fuera de la lista, fila ausente, o `Cerrado`, `Descartado` o `Diferido` sin su forma. No se escribe. | sí | sin clasificar |

- **Forma que clasifica.** En `Análisis`, `Planificado`, `En curso`, `En pruebas` e `Implementado`, la palabra clasifica aunque falten fecha o detalle, y el chequeo avisa. En `Cerrado`, `Descartado` y `Diferido`, sin su forma, el grupo es `sin clasificar`.
- **Pendiente** = la celda no es un `Cerrado` bien formado ni un `Descartado` bien formado.
- **Pendiente de desarrollo** = pendiente salvo `Implementado` y `Diferido`. Incluye diseño, el grupo desarrollo y `sin clasificar`. No es el grupo «desarrollo».
- **Grupo.** El del índice es el de la celda. La etapa de la carpeta no asigna grupo ni pendiente.
- **Desfase.** Si la etapa de la carpeta y la celda no coinciden, los conteos de pendiente y de pendiente de desarrollo siguen la celda. Un desfase en `Planificado`, `En curso` o `En pruebas` no los cambia: solo salen del pendiente un `Cerrado` o un `Descartado` bien formados, y esas palabras las escribe el cierre. El estado y el chequeo avisan y no escriben.
- **Documento.** Minúsculas: `borrador`, `aprobada`, `analisis` (sin tilde), `implementada`, `cerrada`, `descartada`. Las dos últimas solo acompañan a un `Cerrado` o un `Descartado` bien formados. `implementada` no es cerrado. En `Diferido` la línea que había se queda.
- **Deuda cerrada** = `Resuelta` o `Descartada`, palabra exacta, salvo el supuesto de casos límite. **Deuda pendiente** = `Abierta`, `En atención` u otra palabra. Esa otra es deuda pendiente sin clasificar: el chequeo avisa y, si Atendida por es este requerimiento, bloquea igual que `En atención`.
- **Atendida por** = la columna del índice de deuda. **Referencia de origen** = Origen del detalle (tipo + referencia). La columna Origen del índice es solo el tipo. Si discrepan, manda el detalle. Bloquea `Cerrado` una `En atención` o una pendiente sin clasificar con Atendida por de este requerimiento. Si el origen es este y la atiende otro, no bloquea. Una `Abierta` no bloquea: va enlazada en la columna de pendiente. Se juzgan una por una.
- **Productivo.** No es criterio de `Cerrado`, ni los cambios de datos. Si siguen pendientes, son deuda numerada y enlazada. No existe `Liberado`.
- **Aviso o ticket.** El aviso aplica si hay una comunicación abierta anotada. El ticket aplica si la spec o el punto de retomo lo nombran. Se cumplen donde dice el supuesto de registro. Sin nota y sin ticket, el criterio se cumple. El agente no cierra ni consulta el gestor.
- **Columna de pendiente.** En `Cerrado` e `Implementado`, solo enlaces a deudas numeradas, o vacía. Un texto libre no es enlace: el chequeo avisa y el criterio 4 sigue abierto. En `Descartado`, motivo y quién. En `Diferido`, motivo y deudas. En el resto, el siguiente paso.
- **Siguiente paso.** Ese texto cuando no es enlace. No coincide si nombra `plan`, `tareas`, `flujo`, `validar` o `cierre` y eso ya está (documento de plan, tareas, documento de flujo, validación en el punto de retomo, o fila `cierre`). Si no nombra ninguno, no hay aviso.
- **Filas ya escritas.** Las aplica la persona: `Plan` → `Planificado`; `Spec aprobada` → `Análisis` sin plan y `Planificado` con plan; `Implementado en local` → `Implementado`. Un `Cerrado` ya escrito sigue su fila de la tabla.
- **Chequeo.** Avisa, no frena y no escribe si la palabra está fuera de la lista, la celda está mal formada, faltan fecha o detalle, el siguiente paso no coincide, falta la fila, está duplicada o no coincide con la etapa. Sin fila, el cierre crea esa única fila. Con la fila duplicada, ninguna celda autoriza `Cerrado` y el cierre se detiene sin escribir.

### Comprobación previa y criterios de cierre
Antes de escribir `Implementado`, `Cerrado`, `implementada` o `cerrada`, el cierre exige: todo paso de implementación que cuenta está hecho; no hay verificaciones manuales pendientes en pruebas ni en el punto de retomo; el flujo está aprobado si el modo lo incluye; no hay `pausa por confirmar`; y el punto de retomo registra la validación del conjunto aprobada. Si algo falla, se detiene, lista el fallo y no escribe esas palabras.

Con la línea `analisis`, «todo paso está hecho» se cumple si no queda un paso de implementación abierto, y no se exige la validación del conjunto. `Descartado` y `Diferido` no usan esta comprobación.

Si la previa pasó y falta un criterio, la celda queda en `Implementado` y el documento en `implementada`, y se enumeran los que faltan. Si todos pasan, la celda queda en `Cerrado` y el documento en `cerrada`. Esos dos documentos son el resultado, no un paso anterior, y no cuentan como commit pendiente.

1. Una sola fila `cierre` y una sola tabla de plan contra real. Si ya existen, se actualizan. Vale para `Cerrado` y para `Implementado`.
2. No quedan commits pendientes, según su supuesto. `DEPLOY-<ID>` solo si el supuesto de marcadores dice que el proyecto los usa.
3. Si había aviso o ticket, se cumplió. Si hay ticket y no consta la confirmación, falla.
4. No queda nada por hacer, o cada resto es una deuda numerada cuyo detalle registra el visto bueno. Sin visto bueno, falla. Si no queda nada, se cumple.
5. No queda una deuda que bloquee. Una `En atención` con Atendida por de este requerimiento sigue el supuesto de transiciones de deuda. Sin esa decisión, falla.

### Transiciones
La apertura escribe `Análisis`. El cierre escribe `Implementado` o `Cerrado` según la previa y los criterios, y `Descartado` si no se hará. No escribe `Diferido`, `Planificado`, `En curso` ni `En pruebas`. La persona, al pausar, escribe `Diferido` con motivo y estatus de regreso y anota la pausa en el punto de retomo; al reactivar, retira la pausa y devuelve la celda al estatus de regreso. La línea del documento no cambia. `Cerrado` y `Descartado` son terminales: reabrirlos es el supuesto de reapertura. La deuda cambia según el supuesto de transiciones de deuda.

### Etapa de la carpeta
Solo al consultar esa carpeta: informa y no escribe. La primera que cuadra: pausa anotada en el punto de retomo → `Diferido`, salvo un `Cerrado` bien formado (supuesto de reapertura); validación del conjunto aprobada y, si el modo incluye flujo, flujo aprobado → `Implementado`; todo paso que cuenta está hecho y falta esa validación o ese flujo → `En pruebas`; hay un paso empezado y no todos → `En curso`; hay plan y ninguno empezado → `Planificado`; si no → `Análisis`.

Pasos, plan y carpeta sin fila de modo son supuestos. El modo sale de la fila de modo del punto de retomo. Sin esa fila no se exige el flujo. La validación y el flujo están aprobados solo si el punto de retomo los registra. Sin punto de retomo, manda la línea del documento: `implementada` → `Implementado`, `descartada` → `Descartado`, `analisis` → `Análisis`, `cerrada` solo si `Cerrado` está bien formado, y el resto → `Análisis`.

Si el cierre corre y la fila no está duplicada, iguala la celda solo con `Implementado`, `Cerrado` o `Descartado`.

## Requisitos funcionales (EARS)
- RF-1: CUANDO la apertura o el cierre escriben la celda, usan la forma y solo la palabra de su transición.
- RF-2: CUANDO un agente responde pendiente, pendiente de desarrollo, grupo o cerrado del índice, usa la celda y no abre cada carpeta.
- RF-3: CUANDO el estado consulta una carpeta, informa su etapa, avisa el desfase y no modifica archivos. Localiza las deudas según el supuesto de localización.
- RF-4: CUANDO el cierre va hacia `Cerrado`, aplica la previa y los cinco criterios, con los tres resultados definidos.
- RF-5: CUANDO la persona descarta, el cierre aplica la transición `Descartado`, sin la previa, y el supuesto de transiciones de deuda.
- RF-6: CUANDO la persona pausa o reactiva, aplica la transición `Diferido`.
- RF-7: CUANDO la celda y la etapa discrepan, se aplica el desfase.
- RF-8: CUANDO se juzga una deuda, se usan las definiciones de deuda.
- RF-9: CUANDO el chequeo revisa el índice, aplica su definición.
- RF-10: CUANDO una fila ya escrita trae una palabra del mapa, la persona lo aplica.
- RF-11: CUANDO se publica la actualización del kit, el procedimiento incluye una nota con ese mapa: guía a la persona y no reescribe el índice.
- RF-12: CUANDO la persona reabre, el cierre aplica el supuesto de reapertura.

## Requisitos no funcionales
- RNF-1: Dos consultas de las mismas celdas, la misma línea de documento y la misma fila `cierre` dan el mismo pendiente, el mismo pendiente de desarrollo, los mismos grupos y la misma respuesta de cerrado.
- RNF-2: Cada nombre se compara como la palabra exacta, con sus mayúsculas y tildes. En el documento van en minúsculas y `analisis` va sin tilde.
- RNF-3: Cada criterio nombra el dato — celda, línea de estado, fila de Pasos, campo del detalle, columna Atendida por o anotación del punto de retomo — y el valor que lo cumple.
- RNF-4: Cada regla se enuncia una vez en Definiciones. Un requisito o un supuesto la cita y no la vuelve a redactar.

## Casos límite
Solo análisis, reintento, fila ausente o duplicada, texto libre, celda incompleta y palabra vieja usan la regla ya enunciada. Reapertura, `Resuelta` incompleta, pausa sobre un cerrado y dos requerimientos sobre la misma deuda son supuestos.

## Fuera de alcance
Se conservan la numeración, los guardrails, los principios y los nombres `Abierta`, `En atención`, `Resuelta` y `Descartada`. Despliegues y cambios de datos los ejecuta una persona. Que cada comando de fase actualice la celda de `Planificado`, `En curso` y `En pruebas` queda fuera: se propone como deuda y la persona la da de alta. Mientras no exista, la celda puede seguir en la palabra de la apertura o del cierre, y el grupo es el de esa celda.

## Criterios de finalización
- Dos consultas de las mismas celdas coinciden en pendiente, pendiente de desarrollo, grupo y cerrado.
- La versión del kit y su changelog quedan actualizados.
- La nota de actualización contiene el mapa de filas ya escritas, guía a la persona y no reescribe el índice.
- En cada archivo tocado pasan los chequeos de codificación de AGENTS.md › Comandos.

## Dudas abiertas
Supuestos reversibles al aprobar:

1. Reapertura. Reabrir `Cerrado` lo devuelve a `Implementado` e `implementada`. Reabrir `Descartado` lo devuelve a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa. Motivo, quién y fecha quedan en el punto de retomo. Lo escribe el cierre. Una pausa no baja un `Cerrado` bien formado: primero se reabre y después se anota la pausa.
2. Commits. Son los cambios sin registrar de este requerimiento, salvo los archivos que el mismo cierre escribe en la corrida. El resto del repositorio queda fuera.
3. Pasos. Fuente: tabla Pasos de `05_avance.md`, fila de implementación. Hecho: `[x]`. Empezado: `[x]`, `[~]` o `[!]`. Vacía: ni empezada ni hecha. Prueba, flujo y cierre no cuentan. Un paso diferido, con decisión y deuda propuesta, deja de contar. Hay plan si, en completo, está el documento de plan y, en ligero, el plan va en las tareas.
4. Carpeta anterior. Sin fila de modo y con plan, la etapa es `Planificado`. Sin plan, `Análisis`.
5. Registro. Motivo y quién del descarte, la confirmación del ticket y el aviso ya dado quedan en el punto de retomo. El visto bueno de la deuda queda en su detalle. La fecha del último avance es la de la celda.
6. Transiciones de deuda. El agente propone y no escribe. Las escribe el cierre del requerimiento de Atendida por, con la decisión de la persona: `Abierta` → `En atención` al empezar, con visto bueno; `En atención` → `Resuelta` al atenderla, con cómo se atendió, o → `Abierta` si el trabajo se detiene; `Abierta` o `En atención` → `Descartada` con motivo y quién. `Resuelta` y `Descartada` permanecen. Al descartar la spec, ese cierre elige `Abierta` o `Descartada` para cada `En atención` de este requerimiento. Una `Abierta` cuyo origen es este sigue `Abierta`, salvo descarte en esa decisión. La que atiende otro no se toca. Al diferir no corre el cierre: la persona deja esa elección en el detalle al anotar la pausa.
7. Marcadores. `DEPLOY-<ID>` entra en el criterio 2 solo si el proyecto los usa. Si no, el criterio mira solo los commits. No exigen productivo.
8. Localizar deudas. Se encuentran por la columna Atendida por del índice de deuda, sin recorrer cada detalle.
9. Casos límite. `Resuelta` sin «Cómo se atendió» es deuda pendiente sin clasificar. Si dos carpetas reclaman la misma deuda, hay un solo Atendida por: manda el detalle, bloquea solo a ese y el otro no la modifica.
10. Paso vacío. La persona lo aprobó el 2026-11-16. «todo paso que cuenta está hecho» exige al menos un paso de implementación; una carpeta con plan y ningún paso empezado es `Planificado` (no `En pruebas`).
11. Reapertura de `Cerrado`. La persona lo aprobó el 2026-11-16. Reabrir un `Cerrado` pasa por la comprobación previa; no vuelve a juzgar los cinco criterios. Si la previa falla, no se escribe `Implementado` ni `implementada`.
12. Reapertura de `Descartado`. La persona lo aprobó el 2026-11-16. Si al reabrir un `Descartado` el punto de retomo conserva otra etapa, se pregunta a la persona; no se elige palabra ni línea.
13. Descarte en el índice. La persona lo aprobó el 2026-11-16. Motivo y quién del descarte van también en la columna de pendiente de la fila del índice (además del punto de retomo).
