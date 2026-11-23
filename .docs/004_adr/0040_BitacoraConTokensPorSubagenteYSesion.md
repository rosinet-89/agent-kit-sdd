# ADR 0040: La bitácora de uso cuenta los tokens por id de subagente y por sesión, con la coordinación aparte
- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `.docs/002_specs/0023_BitacoraPorSubagente/` (decisiones D1 a D3 de `03_plan.md` › 6;
  pregunta 2 de `02_analisis_impacto.md`, aprobada por la persona). Reemplaza al ADR 0038.

## Contexto
El ADR 0038 contaba los tokens de cada entrada por la ventana Inicio–Fin de su fila de Tiempos, sumando
la sesión principal y todos los subagentes. Con lotes en paralelo cada lote recibió los tokens de toda
la ola: en `0022_PlanContraRealEnVivo`, ola 1, 3,18 M por lote (15,9 M la ola) contra 45 mil a 141 mil
por lote y 1,82 M de coordinación medidos por transcript (2,21 M la ola). Con dos sesiones en la ventana,
la entrada quedaba «no disponible (sesiones paralelas)». El 0038 descartó guardar el id de sesión porque
el agente no lo conocía; hoy la herramienta lo da (en Claude Code, una variable de entorno que coincide
con el nombre del transcript) y devuelve un id por cada subagente lanzado. El kit sigue siendo solo
Markdown y genérico.

## Decisión
Se conserva del 0038 el archivo Markdown local por spec o actividad, la anotación en la misma escritura
que la fila de Tiempos y el cálculo desde la fuente local, sin estimar. Cambia el cálculo: cada entrada
anota el id de sesión y, si es delegada, el id de su subagente. Una entrada delegada suma solo el
transcript de su subagente, sin ventana; una entrada en el chat suma solo su sesión en su ventana; la
sesión principal durante una delegación va a una entrada de coordinación aparte, que se queda lo que
ninguna entrada en el chat contó. Cada `message.id` cuenta una vez en la bitácora de la spec. Las
entradas sin ids siguen con la regla del 0038. Fuente de las reglas:
`template/.agents/skills/bitacora-uso/SKILL.md`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Ventana filtrada por sesión y por subagente activo | Corta lo que el subagente hace tras el Fin de su fila y obliga a adivinar qué transcript es de qué lote |
| Repartir la coordinación entre los lotes de la ola | Inventa una proporción: es estimar, y la skill lo prohíbe |
| Ignorar la coordinación | Oculta el mayor costo de una ola (1,82 M de 2,21 M en la 0022) |
| Seguir marcando «sesiones paralelas» | Con el id de sesión el tramo se ubica sin preguntar |

## Consecuencias
- Se gana: totales por modelo y rol sin ajustes a mano; el costo de coordinar una ola queda visible.
- Se paga: una fila de Tiempos puede dar varias entradas; la sesión debe anotar el id de cada
  lanzamiento hasta escribir su fila.
- Queda condicionado: una herramienta sin ids de sesión o de subagente sigue con «no disponible»; un
  subagente lanzado por otro subagente depende de que este devuelva el id; los tokens dentro de un
  envoltorio de otra herramienta quedan fuera.
