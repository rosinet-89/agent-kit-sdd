# ADR 0038: La bitácora de uso es Markdown local por spec y cuenta los tokens por la ventana del tramo
- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `.docs/002_specs/0021_BitacoraDeUso/` (decisiones 2 y 5 de `03_plan.md` › 6; decisión 13 de
  `05_avance.md`, donde la persona aprobó el ADR)

## Contexto
El kit mide minutos por tramo en la tabla Tiempos de `05_avance.md`, pero no tokens. Para levantar issues
de rendimiento hace falta saber qué modelo y qué rol gastó cuánto. El kit es solo Markdown y no versiona
scripts (constitución, principio 5). Además, el agente no ve sus propios tokens ni el id de su sesión: los
datos solo existen en lo que la herramienta guarda en local, y no todas las herramientas guardan algo.
Lo que se registre es de cada dev y no puede contener el texto de los prompts.

## Decisión
La bitácora es un archivo Markdown por spec o por actividad en `.agents/local/bitacora/`, fuera de Git,
con la decisión por spec en su encabezado y una fila por tramo. Cada entrada se anota al escribir la fila
de Tiempos, en la misma escritura. Los tokens se completan al cerrar o al pedir el reporte, desde la
fuente local de la herramienta. Para cada tramo se toma la ventana de Inicio a Fin de su fila de Tiempos,
pasada a UTC, y se suma una vez por `message.id`, incluidos los subagentes. Fuente de las reglas:
`template/.agents/skills/bitacora-uso/SKILL.md`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Un solo archivo para todas las specs | El reporte por spec tendría que filtrarlo, y la decisión por spec quedaría lejos de sus entradas |
| JSONL u otro formato de datos | Sin código no se lee ni se edita cómodo, y el kit es solo Markdown |
| Guardar el id de sesión al registrar | El agente no lo conoce; habría que adivinarlo |
| Estimar los tokens cuando no hay fuente | Un dato estimado no sirve para levantar un issue; RF-12 lo prohíbe |
| Capturar todo al cierre o solo a pedido | Al cierre ya no se sabe qué modelo y qué motivo tuvo cada tramo; se perdería el dato de la Nota |

## Consecuencias
- Se gana: tokens reales por modelo y rol sin versionar código, y un reporte listo para copiar a un issue.
- Se paga: cada entrada cuesta tokens; si se escribe en una llamada aparte, cada llamada relee todo el
  contexto. Medido en la 0021: 25–30 % más de lectura de caché por spec si va aparte, y 2–3 % más de salida
  si va en la misma escritura (decisión 21). Por eso la skill exige la misma escritura.
- Queda condicionado: con dos sesiones del proyecto en la misma ventana, el tramo queda «no disponible
  (sesiones paralelas)». En una herramienta sin transcripts locales solo hay modelo y minutos. Los tramos
  cuyas ventanas comparten un minuto cuentan dos veces los mensajes de ese minuto (visto en el cierre de
  la 0021; se propone como deuda).
