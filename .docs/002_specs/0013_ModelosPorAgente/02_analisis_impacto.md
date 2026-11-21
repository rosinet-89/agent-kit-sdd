# 02 — Análisis de impacto: Modelos por agente

## 1. Resumen del requerimiento
En Claude Code, cada rol usa su modelo: `opus` para planner, coordinator y reviewer, `sonnet` para implementer y `haiku` para dos roles nuevos, `tester` y `explore`, que solo aceptan encargos cerrados. Un rol con el mismo modelo que la sesión corre en el chat. El documento `07_casos_prueba.md` reemplaza a `07_pruebas_qa.md` y se redacta en `tasks`. Por omisión, `tester` trabaja en modo seguro. Contra los destinos de QA que cada persona declara, la regla 1 de Guardrails tiene una excepción escrita. La spec reemplaza el ADR 013. Spec: `01_spec.md` (aprobada el 2026-11-21 a las 10:25, 29 RF y supuestos S-1 a S-8).

### 1.1 Antecedentes
- `004_ModeloYPermisosAgentes` (Cerrado): creó `restricciones-de-rol.md` y `model: inherit` en los cuatro roles (ADR 013). Se reutilizan el registro de campos y el hecho (c), que da el orden de resolución del modelo y lo cita RF-3. Esta spec contradice la decisión de `inherit`, y el ADR 013 pasa a `Reemplazado`.
- `0008_GuardrailsBdSoloLectura` (Cerrado): creó la regla 1 de Guardrails. La spec le suma la única excepción escrita (RF-25). La redacción actual dice «Ninguna skill, comando, plan ni preferencia los relaja», así que el preámbulo debe admitir las excepciones escritas en las reglas.
- `0009_EnlacesTrasPull` (Cerrado): creó el estado `.agents/local/kit-visto` y sus herramientas declaradas (`claude`, `cursor`, `gemini` y `github`). RF-5 lo lee (S-3).
- `0012_EstatusDeSpecYCierre` (Cerrado): fijó la regla de fuente única que dejó `MEMORY.md` (2026-11-16). Este plan define una fuente por regla antes de repartir el trabajo.
- Specs que dependen de esta: `0014_ConfiguracionGuiada` (preguntas de RF-5 y RF-28) y `0015_ArranqueLigero` (toca `AGENTS.md` después de la 0013).

### 1.2 Deuda técnica relacionada
- `0009_GarantiaTecnicaBdSoloLectura` (Abierta, Media). Comparte el módulo: Guardrails y `restricciones-de-rol.md`. Esta spec no la resuelve. Añade una excepción a la regla 1, que sigue siendo instrucción. Queda abierta.
- No hay más deuda abierta sobre agentes, modelos ni pruebas. La 003 (modelo y permisos) ya está `Resuelta`.

## 2. Módulos, menú, tablas y archivos involucrados

### 2.1 Menú / opciones / rutas de acceso
No aplica: el kit es solo Markdown. Los puntos de entrada son los comandos `/sdd*` y los agentes de `.agents/agents/`, que se lanzan como subagente o con `claude --agent`.

### 2.2 Tablas (existentes y nuevas)
No aplica: no hay base de datos. Las tablas Markdown que cambian:
- `restricciones-de-rol.md`: tabla nueva «Modelo por rol» y filas nuevas de restricciones para `tester` y `explore`.
- `template/AGENTS.md` › «Qué lee cada rol»: filas de `tester` y `explore`.
- `validar-estructura.md`: chequeo nuevo para RF-2.
- `formato-respuesta/SKILL.md`: cuadros de color de `tester` y `explore` (S-4: el chat muestra el rol).

### 2.3 Archivos de código (ruta, capa)
Todas las rutas son de `template/` salvo que se indique otra cosa. La capa sigue `.agents/README.md`.

| Archivo | Capa | Qué cambia | RF |
|---|---|---|---|
| `.agents/skills/sdd/restricciones-de-rol.md` | skill (fuente única) | Tabla «Modelo por rol» que remite al hecho (c). Restricciones de `tester` y `explore`. Excepción de red de `tester`. La fila «Ningún subagente lanza otros» suma a `tester` y `explore`. El encabezado deja de decir «cuatro roles» | RF-1, RF-3, RF-10, RF-11, RF-15, RF-25 |
| `.agents/agents/planner.md`, `coordinator.md`, `reviewer.md` | agente | `model: opus` | RF-1 |
| `.agents/agents/implementer.md` | agente | `model: sonnet` | RF-1 |
| `.agents/agents/tester.md` | agente (nuevo) | `model: haiku`, `tools: Read, Glob, Grep, Bash`; comprueba el encargo, modo seguro, lista blanca y estados | RF-10 a RF-14, RF-24 a RF-26 |
| `.agents/agents/explore.md` | agente (nuevo) | `model: haiku`, `tools: Read, Glob, Grep`; comprueba el encargo y es de solo lectura | RF-10, RF-12, RF-13 |
| `.agents/agents/coordinator.md` | agente | `Agent(planner, implementer, reviewer, tester, explore)`. Como subagente no lanza `tester` ni `explore`. Si falta alguno, avisa | RF-8, RF-9 |
| `.agents/agents/reviewer.md` | agente | Veredicto con las corridas de `tester`. Lee `07_pruebas_qa.md` antiguo | RF-20, RF-23 |
| `.agents/agents/planner.md` | agente | Suma `07_casos_prueba.md` a lo que escribe | RF-15, RF-16 |
| `.agents/skills/sdd/SKILL.md` | skill | Sección nueva «Dónde corre cada rol» (fuente única de RF-7). Árbol de carpeta con `07_casos_prueba.md`. Tabla de fases y lista de agentes con `tester` y `explore` | RF-7, RF-16 |
| `.agents/skills/sdd/plantillas.md` | skill | Sección nueva `## 07_casos_prueba.md`: casos, corridas, evidencia formal y nota de compatibilidad | RF-16 a RF-19, RF-21 a RF-23 |
| `.agents/skills/sdd/avance-y-tiempos.md` | skill | Nota de la fila con el modelo declarado. Pruebas: enlace a `07_casos_prueba.md` | RF-4, RF-21 |
| `.agents/commands/sdd-spec.md`, `sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-change.md` | comando | «Delega en el subagente X» pasa a «ejecuta el rol X» y remite a «Dónde corre cada rol» | RF-7 |
| `.agents/commands/sdd-tasks.md` | comando | El planner genera `07_casos_prueba.md` si hay pasos verificables por ejecución | RF-16, RF-17 |
| `.agents/commands/sdd-validate.md` | comando | `tester` corre antes del reviewer. Corridas, marca literal, una sola vuelta. Compatibilidad con `07_pruebas_qa.md` | RF-20 a RF-23 |
| `.agents/commands/sdd-init.md` | comando | Modelos por omisión según las herramientas declaradas o detectadas, sin preguntas nuevas | RF-5 |
| `.agents/validar-estructura.md` | doc de apoyo | Chequeo nuevo: el `model:` de cada agente coincide con la tabla (`AVISO`) | RF-2 |
| `.agents/entornos.ejemplo.md` | doc de apoyo (nuevo) | Ejemplo de `## QA` con `- esquema://host[:puerto]`, advertencia de S-8 y credenciales solo por nombre de variable | RF-19, RF-27 |
| `.agents/README.md` | doc de apoyo | Lista de roles. `entornos.ejemplo.md` en el layout | RF-10, RF-11, RF-27 |
| `.agents/skills/formato-respuesta/SKILL.md` | skill | Cuadros de `tester` y `explore` | RF-7 (S-4) |
| `AGENTS.md` | instrucciones (grupo B) | Preámbulo de Guardrails y excepción de QA en la regla 1. «Roles y comandos» y «Qué lee cada rol». Línea «Herramienta de apoyo» en Comandos | RF-25, RF-28, RF-10, RF-11 |
| `.gitignore` | grupo B | Sin cambio: `.agents/local/` ya está ignorado, y ahí vive `entornos.md` | RF-27 |
| Raíz: `ACTUALIZAR.md` | doc distribuible | KIT-VIEJO de agentes con los valores de RF-5. Las entradas `mcp__` de `tester.md` no lo vuelven personalizado (S-1). Tabla personalizada (S-7). Aviso de grupo B para Guardrails | RF-6, RF-28 |
| Raíz: `README.md`, `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd`, `manifiestos/<versión>.md` | docs distribuibles | Lista de agentes, versión nueva, changelog con la nota del grupo B y manifiesto nuevo | Criterios |
| Raíz: `.docs/004_adr/<ADR nuevo>.md`, `013_ModeloExplicitoPorRol.md`, `README.md` | ADR | ADR nuevo. El 013 pasa a `Reemplazado`. Índice | RF-29 |
| Raíz: `AGENTS.md` (del clon) | instrucciones del clon | Mismo texto de Guardrails y de roles que `template/AGENTS.md` (hoy son idénticos: medido con `diff`) | RF-25 |
| Raíz: `.agents/` (local, excluido) | réplica | `cp` desde `template/.agents` y `diff -rq` (E19) | — |

### 2.4 Código legado o sin uso (no tocar, sí documentar)
- `07_pruebas_qa.md`: el kit lo nombra en `SKILL.md:42`, `sdd-validate.md:22`, `reviewer.md:18` y `avance-y-tiempos.md:58` (medido con `grep` el 2026-11-21). Tras el cambio, solo queda en la nota de compatibilidad de la plantilla y en el procedimiento de validate (RNF). Ninguna carpeta de este clon tiene ese archivo.
- `.agents/agents/claudio.md` y `.agents/skills/caveman` son diferencias locales admitidas en E19. `.agents/skills/reporte-sesiones` es una diferencia local nueva, ajena a esta spec (ver §6).
- ADR 013: no se borra; pasa a `Reemplazado` con un enlace al ADR nuevo.

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)
No aplica: `AGENTS.md` › Comandos dice «Consulta de datos: No aplica». Mediciones de archivos del 2026-11-21 a las 11:20, en el árbol de trabajo:
- Los cuatro agentes de `template/.agents/agents/` declaran `model: inherit` (`grep model:`).
- `diff` entre las secciones Guardrails y «Roles y comandos» del `AGENTS.md` de la raíz y el de `template/` no da diferencias.
- `.claude/agents` del clon es un enlace a `../.agents/agents`: los agentes nuevos llegan solos a Claude Code tras la réplica en `.agents/`.
- Kit en `0.8.0`. El último manifiesto es `manifiestos/0.8.0.md`. El último ADR es `0027`.

## 4. Flujo actual
1. Cada comando de fase dice «Delega en el subagente X». La sesión levanta un subagente aunque use el mismo modelo. Con `inherit`, ese subagente usa el modelo de la sesión.
2. La directiva #5 de `PREFERENCIAS.md` ya hace en el chat lo que pide RF-7, pero es personal: el kit no lo dice.
3. En validate, el reviewer revisa, da el veredicto y lista las verificaciones manuales. Si se pide evidencia formal, la sesión escribe `07_pruebas_qa.md`. No hay guion de casos antes de implementar ni ejecución separada de pruebas.
4. Guardrails, regla 1: ningún agente ejecuta nada que pueda escribir en una BD, en ningún entorno. No hay excepción.
5. `/sdd-init` no toca el modelo. `ACTUALIZAR.md` reemplaza agentes KIT-VIEJO con el contenido del kit, que hoy trae `inherit`.

## 5. Reglas de negocio: conocidas vs código
| Regla (spec o ADR) | Estado en el kit | Brecha |
|---|---|---|
| Fuente única por regla (ADR 009, `MEMORY.md` 2026-11-16) | Se cumple en `restricciones-de-rol.md` | La tabla «Modelo por rol» y «Dónde corre cada rol» deben tener una sola casa cada una. El resto remite |
| Solo Markdown, sin scripts (ADR 004) | Se cumple | `tester` ejecuta casos con las herramientas del proyecto. El kit no trae scripts |
| La actualización solo reemplaza KIT-VIEJO (ADR 007) | `ACTUALIZAR.md` | RF-6, S-1 y S-7 añaden casos a la clasificación de agentes |
| `AGENTS.md` es grupo B: nunca se reemplaza | `ACTUALIZAR.md` › Grupo B | La excepción de QA y las filas de roles no llegan solas a proyectos existentes. Mientras falten, `tester` queda en modo seguro (lado seguro) |
| Guardrails: «Ninguna skill, comando, plan ni preferencia los relaja» | `AGENTS.md` | El preámbulo debe admitir solo las excepciones escritas en sus reglas (RF-25) |
| Ningún agente usa la red | `restricciones-de-rol.md`, fila 7 | Excepción solo para `tester` contra destinos de QA y por su herramienta de apoyo |
| Herramientas de `kit-visto`: `claude`, `cursor`, `gemini` y `github` | `sync-agents.md` | RF-5 menciona `.kiro/`, que no es un valor de `kit-visto`: solo se detecta por carpeta |

## 6. Riesgos y casos borde
1. **Alias ausente** (causa del ADR 013). Claude Code cae al modelo heredado con aviso. RF-4 lo deja anotado y no bloquea.
2. **Choque de nombres `explore` y `Explore`.** Riesgo aceptado (decisión 6). En validate se comprueba que se lanza el agente del kit (criterio de finalización).
3. **`tester` con `Bash`.** Que no escriba por shell es solo instrucción, y queda así marcado en la tabla (RF-11). Haiku obedece menos que Opus. Mitigación: modo seguro por omisión y encargo cerrado.
4. **Proyectos existentes (grupo B).** Sin incorporar a mano la excepción y los roles, `tester` no tiene la excepción y no puede saltar la regla 1. La nota del changelog lo dice.
5. **Proyectos solo con otras herramientas.** El kit en `template/` trae los alias de Claude. `/sdd-init` y `ACTUALIZAR.md` deben dejar `inherit` (RF-5 y RF-6). Si se omite ese paso, Cursor u otra herramienta reciben `opus`. Cursor cae a un modelo compatible, pero Kiro puede fallar en el primer prompt (`restricciones-de-rol.md`).
6. **Conflicto con la 0015.** Las dos tocan `AGENTS.md`. La 0015 se implementa después y parte de este texto.
7. **E19 en el clon.** `reporte-sesiones` ya es una diferencia nueva no admitida en `MEMORY.md`, ajena a esta spec. Al replicar, se informa y no se toca.
8. **Tope de validate.** La corrida de `tester` y la del reviewer cuentan como una vuelta (RF-20). La evidencia formal ya consumía una vuelta. Hay que redactarlo sin contradecir el punto 5 actual de `sdd-validate.md`.
9. **Redacción repartida en ocho comandos.** Si cada comando reescribe RF-7, se repite el problema de la 0012. Mitigación: una sola sección en `SKILL.md` y una línea igual en cada comando que remita a ella.
10. **Llamadas en segundo plano desde una página de QA** (S-8): riesgo asumido. Lo advierte `entornos.ejemplo.md`.

## 7. Preguntas abiertas
Ninguna bloqueante. Las decisiones que debe aprobar la persona están en `03_plan.md` › §6.

## Cambio 1 (2026-11-21)
RF-12 pasa la comprobación del encargo a quien lanza el rol Haiku, y RF-30 agrega el relanzamiento con el modelo del implementer y el paso a la sesión. Se tocan los archivos de la fila de cambio de `03_plan.md` › §2. No toca la base de datos ni Guardrails. Riesgo: más llamadas en un encargo que falla, con tope de una por rol antes de la sesión.
