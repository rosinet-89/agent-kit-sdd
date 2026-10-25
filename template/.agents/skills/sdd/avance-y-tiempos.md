# Contrato de avance, decisiones y tiempos

Aplica a `05_avance.md` de cada carpeta SDD en `.docs/002_specs/`.

## Quién y cuándo

- Lo escribe **solo la sesión principal** (comando `/sdd-*` o coordinator). Los
  subagentes devuelven qué hicieron y la sesión principal lo anota.
- Se crea en la fase `spec`, **no** al arrancar código: así quedan las horas del
  análisis. Se actualiza **al cerrar cada fase o paso**, nunca al final de memoria.
- Hora local del usuario, tomada con `date '+%Y-%m-%d %H:%M'` al empezar y al
  terminar cada fase o paso.

## Estructura de `05_avance.md`

1. **PUNTO DE RETOMO** (siempre primero; otro agente debe poder continuar leyendo solo esto)
2. Pasos (espejo de `04_tasks.md` con su estado)
3. Decisiones
4. Pruebas
5. Tiempos
6. Plan contra real (al cierre)

Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

### PUNTO DE RETOMO

| Dato | Contenido |
|---|---|
| Última sesión | `AAAA-MM-DD HH:MM` |
| Fase actual | `spec`, `clarify`, `plan`, `tasks`, `Fn.m`, `validate`, `flujo` o `cierre` |
| Último paso cerrado | Qué se hizo |
| Siguiente paso | Una sola acción concreta, con el comando `/sdd-*` |
| Bloqueos abiertos | O "Ninguno" |
| Estado BD | Qué scripts `sql/NN` ya ejecutó el usuario |
| Código | Archivos tocados / pendientes |

## Decisiones

Cada decisión lleva fecha y hora local, en el momento en que se toma.
No se fechan todas al cierre del día.

```
26. **[2026-09-23 15:44] Serie de Carta Porte = intTipoSerie + bitTraslado.**
    Por qué: ... Alternativa descartada: ...
```

- La hora es la de la instrucción que cerró la decisión, no la del inicio del paso.
- Si la decisión se corrige, se actualiza el texto y la hora pasa a ser la de la corrección. No se dejan dos horas en el mismo renglón.
- Orden en el avance: por hora, no por el número viejo.
- Misma zona horaria que la tabla de tiempos.
- Si una decisión cambia el alcance, se actualizan también `01_spec`/`03_plan`/`04_tasks`. El avance no sustituye al plan.

## Pruebas

Por cada paso o RF: qué se verificó (checklist "Hecho cuando"), resultado y
evidencia breve (consulta, salida de comandos de verificación, confirmación manual del usuario
con su hora). Si hay `07_pruebas_qa.md`, aquí solo va el puntero, sin duplicar la tabla.

## Tiempos reales

El avance sirve, al cerrar, para comparar lo planeado contra lo que sí se
trabajó y afinar las estimaciones del siguiente plan. Cada fase o paso cerrado
agrega un renglón.

```
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
| spec | 2026-10-06 09:10 | 2026-10-06 09:40 | 30 | 0 | entrevista + 01_spec |
| F1.1 | 2026-10-06 11:00 | 2026-10-06 11:25 | 25 | 0 | repository + clase PDO |
```

- **Paso:** nombre de la fase del flujo (`spec`, `clarify`, `plan`, `tasks`, `validate`, `flujo`, `cierre`) o id del paso del plan (`Fn` / `Fn.m`).
- **Inicio:** hora del mensaje que arranca el paso, o la hora en que se retoma si hubo pausa confirmada.
- **Fin:** hora en que el paso queda cerrado (respuesta entregada o el usuario confirma que el script ya corrió).
- **Minutos:** `fin - inicio - pausa`. Solo entra lo que fue trabajo de este feature: codear, revisar SQL juntos, decidir, probar.
- **Pausa:** minutos que el usuario dijo que no fueron de este desarrollo (comida, otra tarea, junta, fin de día).
- **Mínimo y lote.** En un feature abierto desde esta versión, cada paso real mide al menos 1 minuto. No se cierran varios pasos con filas en lote de 0 minutos. La única excepción es un paso trivial cuya Nota dice exactamente "sin trabajo real". Un feature ya cerrado conserva sus filas; ninguna comprobación le exige reescribirlas. `001_ActualizarKit` sigue válido con las suyas.

Las reglas de pausa, de formato de la fila, de quién escribe este avance y de la leyenda de estados viven solo en este contrato. Los comandos las nombran; no las vuelven a definir. El arranque de sesión no vive aquí: su dueño es `AGENTS.md`. Cuándo se toma la otra ruta vive en la skill `sdd`, sección «Si no hay un solo paso». Aquí no se copia.

### Constancia cuando no hay un solo paso

Es una decisión de este avance, con hora, en el momento: qué rol no pudo delegar o qué pidió el usuario, el porqué en una línea, qué ruta se usó y quién hizo el trabajo o lanzó al subagente. No se abre otro registro.

La fase lleva una sola fila. Los minutos del intento que no pudo delegar entran en esa fila y la nota dice que hubo un intento que no delegó. Si ese agente no devolvió texto, la línea la escribe la sesión que toma la otra ruta, con la causa que vio: no puede abrir al subagente, el subagente no arranca, o la herramienta para abrirlo no está.

### Hueco largo: preguntar antes de contar

Si entre el fin registrado y la siguiente instrucción, o entre dos
instrucciones, pasan **más de 30 minutos** y ese hueco no es trabajo visible del
agente en el mismo turno, no se suman esos minutos. Se pregunta una sola vez:

"De HH:MM a HH:MM hay N min. ¿Cuántos fueron pausa y cuántos seguiste en este desarrollo?"

Hasta la respuesta, el renglón queda `pausa por confirmar` y **no entra al
total**. Un hueco de un día o de una noche es pausa salvo que el usuario diga lo
contrario. Esperar a que el usuario ejecute un SQL no cuenta como trabajo.

## Plan contra real (al cierre)

En `03_plan.md` cada fase lleva **tiempo estimado en horas** (aunque sea un
rango corto, por ejemplo 2–3 h). Al cerrar, el avance agrega:

```
| Fase | Estimado | Real | Delta |
| F0   | 1 h      | 0.5 h | -0.5 h |
```

- El real sale de la tabla de tiempos, no de un cálculo nuevo. Las fases del flujo (`spec`…`cierre`) se suman en un renglón `Documentación SDD`.
- Si el plan no traía estimados, no se inventan hacia atrás: se anota "sin estimado" y el real queda como referencia para el próximo feature parecido.
- Si una fase se desvió fuerte, una línea dice qué ajustar en el siguiente plan.

## Anti-patrones

- Avance como log de sesión sin siguiente paso ni estado de BD.
- Cerrar un paso solo con fecha, sin hora de inicio y fin.
- Anotar una decisión solo con la fecha, sin hora.
- Sumar un hueco mayor a 30 minutos sin preguntar si hubo pausa.
- Inventar el tiempo estimado de un plan que no lo traía.
