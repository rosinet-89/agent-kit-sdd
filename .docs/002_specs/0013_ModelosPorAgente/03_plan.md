# 03 — Plan: Modelos por agente
Spec: 01_spec.md (aprobada 2026-11-21 10:25)

## 1. Ubicación y capas
El producto vive en `template/` y en los docs distribuibles de la raíz (`AGENTS.md` › Reglas de negocio). Capas de `.agents/README.md`: agentes (`agents/<rol>.md`), comandos (`commands/`), skills (`skills/`) y documentos de apoyo (`validar-estructura.md`, `sync-agents.md` y ejemplos). La regla se escribe una vez y los demás textos remiten (ADR 009, `MEMORY.md` 2026-11-16).

Fuente única de cada regla nueva:

| Regla | Fuente única | Remiten |
|---|---|---|
| Modelo de cada rol (RF-1, RF-3) | `restricciones-de-rol.md` › «Modelo por rol» | frontmatter (`model:`, que la aplica), chequeo de estructura, `sdd-init`, `ACTUALIZAR.md` y ADR |
| Restricciones de `tester` y `explore` (RF-10, RF-11, RF-15) | `restricciones-de-rol.md` › tabla de restricciones | `tester.md`, `explore.md` |
| Dónde corre cada rol y quién lanza a los Haiku (RF-7, RF-8) | `skills/sdd/SKILL.md` › «Dónde corre cada rol» | los ocho comandos de fase y `coordinator.md` |
| Encargo cerrado y respuestas `ENCARGO INCOMPLETO` y `ENCARGO FUERA DE ROL` (RF-12, RF-13) | `restricciones-de-rol.md` › sección «Roles Haiku» | `tester.md`, `explore.md` |
| Forma del documento de casos, de un caso y de sus estados (RF-16 a RF-19, RF-21 a RF-23) | `plantillas.md` › `## 07_casos_prueba.md` | `sdd-tasks`, `sdd-validate`, `tester.md`, `reviewer.md`, `avance-y-tiempos.md` |
| Excepción de QA a la regla 1 (RF-24, RF-25) | `AGENTS.md` › Guardrails, regla 1 | `restricciones-de-rol.md` (excepción de red), `tester.md`, `entornos.ejemplo.md` |
| Formato de la configuración de entornos y comparación de destinos (RF-26, RF-27) | `entornos.ejemplo.md` (formato) y `tester.md` (comparación antes de cada caso) | `AGENTS.md` › Guardrails |
| Modelo declarado en la fila de tiempos (RF-4) | `avance-y-tiempos.md` | agentes (una línea en «Respuesta») |

## 2. Archivos que se crean o modifican
Las rutas sin prefijo son de `template/`.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `.agents/skills/sdd/restricciones-de-rol.md` | skill | «Modelo por rol» (6 filas: planner, coordinator y reviewer en `opus`; implementer en `sonnet`; `tester` y `explore` en `haiku`). Remite al hecho (c). La tabla del proyecto registra lo aplicado (S-6). Sección «Roles Haiku» con el encargo cerrado y las dos respuestas. Filas nuevas: `explore` (solo lectura por `tools`), `tester` (sin `Write` ni `Edit` por `tools`, sin escribir por shell por instrucción) y excepción de red de `tester`. Fila de `Agent` con los seis roles | RF-1, RF-3, RF-10 a RF-13, RF-15, RF-25 |
| `.agents/agents/planner.md`, `coordinator.md`, `reviewer.md` | agente | `model: opus`. Línea de respuesta: modelo con que corrió | RF-1, RF-4 |
| `.agents/agents/implementer.md` | agente | `model: sonnet`. Línea de respuesta: modelo | RF-1, RF-4 |
| `.agents/agents/explore.md` (nuevo) | agente | `model: haiku`, `tools: Read, Glob, Grep`. Comprueba el encargo, localiza y devuelve en el formato pedido. Sin `omitClaudeMd` (lee Guardrails igual) | RF-10, RF-12, RF-13, RF-4 |
| `.agents/agents/tester.md` (nuevo) | agente | `model: haiku`, `tools: Read, Glob, Grep, Bash` (más las `mcp__` del proyecto, S-1). Comprueba el encargo y cada caso. Modo seguro. Lista blanca en cada navegación. Estados `obtenido`, `bloqueado` y `manual`, sin veredicto. Credenciales solo por variable | RF-11, RF-12, RF-13, RF-14, RF-19, RF-24, RF-25, RF-26, RF-4 |
| `.agents/agents/coordinator.md` | agente | `Agent(planner, implementer, reviewer, tester, explore)`. Como subagente no lanza Haiku. Aviso si su copia personalizada no lista alguno | RF-8, RF-9 |
| `.agents/agents/reviewer.md` | agente | Veredicto con las corridas de `tester`. Lee el `07_pruebas_qa.md` antiguo como evidencia. Quita la mención de `07_pruebas_qa.md` como archivo que no escribe | RF-20, RF-23 |
| `.agents/agents/planner.md` | agente | Escribe también `07_casos_prueba.md` | RF-15, RF-16 |
| `.agents/skills/sdd/SKILL.md` | skill | Sección «Dónde corre cada rol» (chat si es el mismo modelo, subagente si cambia o lo pide la persona, separación por rol, quién lanza a los Haiku). «Si no hay un solo paso» remite a ella. Árbol de carpeta con `07_casos_prueba.md`. Descripción y tabla con los roles nuevos | RF-7, RF-8, RF-16 |
| `.agents/skills/sdd/plantillas.md` | skill | `## 07_casos_prueba.md`: casos (campos de la definición «Caso»), corridas (estados cerrados), evidencia formal opcional y nota de compatibilidad con `07_pruebas_qa.md` | RF-16 a RF-19, RF-21 a RF-23 |
| `.agents/skills/sdd/avance-y-tiempos.md` | skill | La nota de la fila lleva el modelo declarado. Pruebas: enlace a `07_casos_prueba.md` si existe | RF-4, RF-21 |
| `.agents/commands/sdd-spec.md`, `sdd-clarify.md`, `sdd-plan.md`, `sdd-implement.md`, `sdd-flujo.md`, `sdd-change.md` | comando | «Delega en el subagente X» pasa a «Ejecuta el rol X (dónde corre: `SKILL.md` › «Dónde corre cada rol»)», con una sola redacción | RF-7 |
| `.agents/commands/sdd-tasks.md` | comando | Lo de RF-7 y la generación de `07_casos_prueba.md` cuando hay pasos verificables por ejecución | RF-7, RF-16, RF-17, RF-22 |
| `.agents/commands/sdd-validate.md` | comando | Lo de RF-7. `tester` corre antes del reviewer con los casos ejecutables, se anotan las corridas, se marcan las comparaciones literales (S-5), el veredicto es del reviewer y todo es una sola vuelta. El punto 5 se reescribe para `07_casos_prueba.md`. Compatibilidad con `07_pruebas_qa.md` | RF-7, RF-20 a RF-23 |
| `.agents/commands/sdd-init.md` | comando | Paso de modelos por omisión según `kit-visto` o las carpetas, con S-2. Sin preguntas nuevas | RF-5 |
| `.agents/validar-estructura.md` | doc de apoyo | Chequeo E23 «modelo por rol»: compara el `model:` de cada agente con la tabla del proyecto. Solo da `AVISO` | RF-2 |
| `.agents/entornos.ejemplo.md` (nuevo) | doc de apoyo | Formato de `## QA`, ejemplo, advertencia de S-8 y de la BD de QA, credenciales solo por nombre, y que el formato roto vuelve al modo seguro | RF-19, RF-26, RF-27 |
| `.agents/README.md` | doc de apoyo | Roles en el layout. `entornos.ejemplo.md` | RF-10, RF-11, RF-27 |
| `.agents/skills/formato-respuesta/SKILL.md` | skill | Filas `tester` 🟨 y `explore` 🟫 | RF-7 (S-4) |
| `AGENTS.md` | instrucciones | Preámbulo de Guardrails: «salvo las excepciones escritas en sus reglas». Regla 1 con la sub-viñeta de la excepción de QA. «Roles y comandos» y «Qué lee cada rol» con `tester` y `explore`. Comandos: línea «Herramienta de apoyo para casos: No aplica» | RF-25, RF-28, RF-10, RF-11 |
| Raíz `AGENTS.md` | instrucciones del clon | Mismo texto de Guardrails y de roles que `template/AGENTS.md`. Comandos del clon: «Herramienta de apoyo: No aplica» | RF-25 |
| Raíz `ACTUALIZAR.md` | doc distribuible | Agentes KIT-VIEJO con el modelo de RF-5. S-1 (entradas `mcp__` de `tester.md`). S-7 (tabla personalizada). Recordatorio de grupo B: Guardrails y roles a mano | RF-6, RF-28 |
| Raíz `README.md` | doc distribuible | Lista de agentes | — |
| Raíz `.docs/004_adr/0028_ModeloPorRolYRolEnElChat.md` (nuevo), `013_ModeloExplicitoPorRol.md`, `README.md` | ADR | ADR nuevo, el 013 en `Reemplazado por 0028` e índice | RF-29 |
| Raíz `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.9.0.md` (nuevo) | release | `0.9.0`, entrada del changelog con la nota de grupo B y manifiesto | Criterios |
| Raíz `.agents/` (local) | réplica | `cp` de lo cambiado en `template/.agents` y `diff -rq` | — |

**Cambio 1 (2026-11-21, decisiones 32 y 33): RF-12 modificado y RF-30 nuevo.** Fuentes únicas:
- La comprobación del encargo antes de lanzar va en `restricciones-de-rol.md` › «Roles Haiku».
- El relanzamiento y el paso a la sesión van en `SKILL.md` › «Dónde corre cada rol».
- Los demás archivos remiten.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `.agents/skills/sdd/restricciones-de-rol.md` | skill | «Roles Haiku»: quien lanza comprueba el encargo (completo y dentro de rol) antes de lanzar y, si falla, lo completa o hace esa parte. La comprobación del rol queda como segunda barrera, solo instrucción | RF-12, RF-13 |
| `.agents/skills/sdd/SKILL.md` | skill | «Dónde corre cada rol»: qué es no entregar, un solo relanzamiento con el modelo del implementer (remite a «Modelo por rol»), después la sesión con la definición del rol, cada intento en la Nota con su modelo, y la herramienta sin modelo por llamada | RF-30, RF-4 |
| `.agents/agents/coordinator.md`, `.agents/commands/sdd-validate.md` | agente, comando | Antes de lanzar `tester` o `explore`, comprobar el encargo; si no entrega, la regla de RF-30. Remiten, no copian | RF-12, RF-30 |
| `.agents/agents/explore.md`, `tester.md` | agente | La comprobación propia es segunda barrera: quien lanza ya la hizo | RF-12 |
| Raíz `.docs/004_adr/0028_…`, `ChangelogAgentKitSdd.md` | ADR, release | Consecuencia y línea de 0.9.0: comprueba quien lanza, con respaldo `sonnet` y después la sesión | RF-29 |
| Raíz `manifiestos/0.9.0.md`, `.agents/` (local) | release, réplica | Se regenera el manifiesto; `cp` y `diff -rq` | Criterios |

## 3. Base de datos
No aplica: no hay `sql/`.

## 4. Seguridad
- **Regla 1 de Guardrails.** La excepción es una sola, está escrita y tiene alcance cerrado: solo `tester`, solo contra destinos de la lista blanca y solo con pantallas o llamadas a la aplicación. Nunca sentencias, `sql/` ni shell que escriba. Sin configuración, o si está mal formada, `tester` trabaja en modo seguro.
- **Red.** La excepción va solo a `tester` y por medio de su herramienta de apoyo. `explore` no tiene `Bash` ni red. Los otros roles no cambian.
- **Credenciales.** Solo se escribe el nombre de la variable de entorno. La herramienta de apoyo lee el valor; el agente nunca lo ve, imprime ni copia (RF-19). `seguridad-basica` se aplica en validate al ejemplo de entornos y a la plantilla.
- **Garantías.** La tabla dice qué garantiza `tools` y qué es solo instrucción. No se afirma una garantía que no existe (ADR 011).
- **Doble envío e idempotencia.** No aplica (no hay aplicación).

## 5. Liberación / piloto
Versión `0.9.0`. Los proyectos existentes la reciben con `ACTUALIZAR.md`:
- Los agentes y comandos KIT-VIEJO se reemplazan.
- `AGENTS.md` es grupo B: la excepción de QA y los roles se incorporan a mano.
- Hasta entonces, `tester` queda en modo seguro.

Piloto: el propio clon. Validate lanza `explore` en Claude Code y comprueba el modelo en el transcript (criterio de finalización).

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| D1. «Dónde corre cada rol» vive en `skills/sdd/SKILL.md`, junto a «Si no hay un solo paso» | En `restricciones-de-rol.md`; repetida en cada comando | Es regla del flujo, no una restricción de herramienta. Una casa y ocho remisiones evitan el problema de la 0012 |
| D2. RF-2 es un chequeo nuevo E23 que solo da `AVISO` | Ampliar E8 (que da `FALLA`) | RF-2 dice que un agente personalizado distinto avisa y no falla. E8 conserva su sentido |
| D3. ADR nuevo `0028` (4 dígitos, el siguiente libre); el 013 pasa a `Reemplazado por 0028` | Editar el 013 | Un ADR no se reescribe: se reemplaza y queda la historia |
| D4. Versión `0.9.0` | `0.8.1` | Agrega roles, un documento nuevo y una excepción a Guardrails: es un cambio menor con comportamiento nuevo, no un arreglo |
| D5. El `AGENTS.md` de la raíz copia el cambio de Guardrails y de roles | Dejar la raíz igual | Hoy las dos secciones son idénticas (medido con `diff`). Mantenerlas así evita que el clon cuente otra cosa que el producto |
| D6. Cuadros de color: `tester` 🟨 y `explore` 🟫 | Usar el cuadro del rol que los lanza | S-4 exige que el chat muestre el rol. Un cuadro propio lo distingue |
| D7. `tester` y `explore` sin `omitClaudeMd` | Declararlo como en planner y reviewer | Haiku con menos contexto obedece menos. Que reciban `AGENTS.md` (Guardrails) inyectado da más margen que el ahorro de tokens. Revisable con la 0015 |
| D8. Un solo ADR (RF-29) para las cuatro decisiones | Un ADR por decisión | Lo pide la spec. Las cuatro decisiones nacieron juntas y se revisan juntas (tras 3 features) |

Marcadas como posibles ADR: D1 y D2 no tienen efecto duradero fuera del kit, así que no llevan ADR propio. La única que lo lleva es RF-29 (D8).

## 7. Fases y estimado
Estimado en minutos (`MEMORY.md` 2026-10-25: Markdown ejecutado por agentes). Implementer en `sonnet` por subagente; el resto, en el chat.

| Fase | Contenido | RF | Estimado |
|---|---|---|---|
| F0 | Preparación: copia previa de `template/` en el scratchpad, `git diff` limpio de lo ajeno y lista final de archivos | — | 10 min |
| F1 | Modelo por rol: tabla, restricciones nuevas y sección «Roles Haiku» en `restricciones-de-rol.md`, más el `model:` de los cuatro agentes | RF-1, RF-3, RF-12, RF-13, RF-15 | 30 min |
| F2 | Roles Haiku: `explore.md`, `tester.md`, `coordinator.md`, `formato-respuesta`, `.agents/README.md` y `README.md` raíz | RF-4, RF-8 a RF-14, RF-24, RF-26 | 45 min |
| F3 | Dónde corre cada rol: sección en `SKILL.md`, ocho comandos y nota del modelo en `avance-y-tiempos.md` | RF-4, RF-7, RF-8 | 40 min |
| F4 | Casos de prueba: plantilla `07_casos_prueba.md`, `sdd-tasks`, `sdd-validate`, `reviewer.md`, `planner.md`, `avance-y-tiempos.md` y árbol de `SKILL.md` | RF-16 a RF-23 | 45 min |
| F5 | Entornos y Guardrails: preámbulo y regla 1 en `AGENTS.md` (`template/` y raíz), Comandos, `entornos.ejemplo.md` y excepción de red | RF-19, RF-24 a RF-28 | 35 min |
| F6 | Instalación y estructura: `sdd-init`, `ACTUALIZAR.md` y E23 en `validar-estructura.md` | RF-2, RF-5, RF-6, RF-28 | 40 min |
| F7 | ADR `0028`, el 013 reemplazado e índice | RF-29 | 20 min |
| F8 | Release y réplica: versión, changelog, manifiesto `0.9.0`, `cp` a `.agents/`, E19, ensayo de instalación y lint de todo lo tocado | Criterios | 30 min |
| F9 | Cambio 1: comprueba quien lanza (RF-12), relanzamiento y paso a la sesión (RF-30), remisiones, ADR y changelog, manifiesto y réplica | RF-12, RF-13, RF-30 | 30 min |
| | **Total** | | **325 min (≈ 5.4 h)** |

Orden: F1 va antes de F2 (las restricciones definen a los agentes). F3 y F4 tocan `SKILL.md` y `sdd-validate`, así que van en serie. F5 y F6 son independientes de F3 y F4. F7 y F8 van al final.

## 8. Criterios de aceptación
- Los seis agentes declaran el modelo de la tabla y `/sdd-estructura` (E23) no da avisos en el clon.
- Lanzar `explore` en Claude Code llega al agente del kit y corre en `haiku`. Lo comprueban el transcript del subagente y el modelo que declara el rol (RF-4).
- `07_casos_prueba.md` tiene plantilla y lo usan `sdd-tasks`, `tester` y `sdd-validate`. Fuera de las dos notas de compatibilidad, ningún archivo del kit nombra `07_pruebas_qa.md` (`grep`).
- La regla 1 de Guardrails tiene la excepción de QA, el preámbulo la admite y existe `.agents/entornos.ejemplo.md`.
- «Dónde corre cada rol» existe una sola vez y los ocho comandos remiten a ella con la misma línea (`grep`).
- El ADR `0028` está aceptado y el 013 figura como `Reemplazado`.
- Versión `0.9.0`, changelog con la nota de grupo B, manifiesto `0.9.0` y ensayo de instalación sin pisar nada.
- Lint de `AGENTS.md` › Comandos en cada archivo tocado: UTF-8 sin BOM, `name` igual a la carpeta y rutas que existen.
- `/sdd-validate` verifica cada RF.
