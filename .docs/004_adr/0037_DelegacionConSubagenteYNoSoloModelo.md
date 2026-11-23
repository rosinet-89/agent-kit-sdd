# ADR 0037: Las tareas sin rol cambian de modelo con un subagente, no solo con `model:`

- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `0020_RuteoDeModelos`

## Contexto
El kit fijaba el modelo de cada rol, pero lo que no es un rol (comandos como `/sdd-status` o
`/sdd-estructura`, la preparación de commits y las tareas sueltas del chat) corría con el modelo de
la sesión. Con la sesión en nivel Alto, esas tareas pagaban ese nivel sin necesitarlo.

Restricciones:
- Cambiar de modelo a media sesión obliga a escribir otra vez en la caché todo el contexto con el
  modelo nuevo.
- El subagente no tiene la conversación: solo puede recibir tareas con encargo cerrado.
- Delegar tiene un costo fijo de dos llamadas de la sesión sobre todo su contexto: una para lanzar y
  otra para recibir.

## Decisión
Una tarea sin rol solo baja de modelo si se delega en un subagente del kit: `auxiliar` (nivel Medio)
o `explore` (nivel Bajo), con encargo cerrado.
- `/sdd-status` y `/sdd-estructura` se delegan de forma fija, con `context: fork` y `agent: auxiliar`
  en su frontmatter.
- El resto lo decide el chat con la tabla y la regla de
  `.agents/skills/sdd/restricciones-de-rol.md` › «Nivel por tarea sin rol».
- Declarar `model:` en un comando sin delegarlo en un subagente está prohibido.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| `model:` en el comando, sin subagente | Reescribe en la caché todo el contexto de la sesión con otro modelo. En la simulación del 2026-11-22 costó de 1.5 a 17 veces lo mismo hecho en el chat. |
| Que la persona cambie `/model` a mano | Depende de que la persona se acuerde, y además paga la misma reescritura de caché. La persona lo descartó en la spec. |
| Un rol nuevo de nivel Bajo, o `auxiliar` con modelo elegido en cada llamada | Lo Bajo sin `Bash` ya lo cubre `explore`. Un modelo por llamada no cabe en un frontmatter fijo (decisión 12 de la spec). |

## Consecuencias
- Se gana en sesiones chicas, o cuando el chat seguiría un comando lectura por lectura. Ejemplo:
  `/sdd-status` con unos 36k de contexto costó 0.15 USD delegado frente a 0.33 USD en el chat.
- Se paga en sesiones grandes, donde las dos llamadas de lanzar y recibir pesan más:
  - `/sdd-status` con unos 215k de contexto costó un 20 % más delegado. La persona lo mantuvo
    delegado de todos modos.
  - La preparación de un commit costó entre un 11 % y un 47 % más, y salió de la tabla (RF-12).
- Condiciona:
  - Toda fila nueva de la tabla necesita medirse antes, en tokens, USD con fecha y tiempo.
  - Un rol recién creado o cambiado tarda en cargarse en la herramienta. Mientras no carga,
    `agent: <rol>` cae sin avisar a `general-purpose` con el modelo de la sesión: después de crear un
    rol, espera a que cargue o abre una sesión nueva.
