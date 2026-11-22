# 04 — Tareas: Flujo eficiente
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

«Hecho cuando» común a todo paso que toca un archivo (no se repite abajo): lint de `AGENTS.md` ›
Comandos en el archivo (UTF-8 sin BOM; `name` del frontmatter igual a su carpeta; rutas citadas
existentes; en `template/`, sin marcadores `<<…>>` salvo los dos admitidos). Evidencia de datos y
verificación manual de UI: No aplica, salvo que el paso diga otra cosa.

## Lotes y olas (aplica RF-7 a esta misma spec)
| Ola | Lote | Pasos | Archivos | Quién |
|---|---|---|---|---|
| 0 | — | F0.1 | — | sesión |
| 1 | L1 | F1.1, F2.4 | `SKILL.md` | implementer |
| 1 | L2 | F1.2, F2.5 | `avance-y-tiempos.md` | implementer |
| 1 | L3 | F1.3, F2.6 | `sdd-tasks.md` | implementer |
| 1 | L4 | F2.1, F2.2 | `sdd-implement.md`, `implementer.md` | implementer |
| 1 | L5 | F3.1, F3.2 | `topes-y-modos.md`, `sdd-clarify.md` | implementer |
| 1 | L6 | F4.1, F4.2, F4.3 | `sdd-cierre.md`, `template/AGENTS.md`, `sdd-init.md`, `AGENTS.md` | implementer |
| 1 | L7 | F2.3 | `README.md` | implementer |
| 2 | — | F5.1, F5.2, F5.3 | réplica, versión, changelog, ADR | sesión |
| 3 | — | F5.4 | `manifiestos/0.14.0.md` | sesión (último cambio a `template/`) |

## F0 — Línea base (estimado: 3 min)
- [x] **F0.1 Línea base.** —
  - Hecho cuando:
    - [x] `/sdd-estructura` corrido por la sesión, sin subagente (RF-4); resultado anotado en `05_avance.md`
    - [x] `git status --short template/` vacío antes de empezar

## F1 — Lanzamientos (estimado: 6 min)
- [x] **F1.1 `SKILL.md` › «Dónde corre cada rol»: comprobación previa y lista cerrada.** RF-1, RF-2
  - Hecho cuando:
    - [x] Un punto con las condiciones (a) y (b) de RF-1, incluido el relanzamiento
    - [x] Un punto con la prohibición de agentes fuera del kit, la remisión a `explore` y las dos
      excepciones de RF-2
    - [x] Ningún comando copia la regla (grep de «general-purpose» solo en `SKILL.md`)
- [x] **F1.2 `avance-y-tiempos.md`: motivo del lanzamiento en la Nota.** RF-3
  - Hecho cuando:
    - [x] El punto «Modelo» suma los 5 motivos de RF-3
- [x] **F1.3 `sdd-tasks.md`: sin validación de estructura delegada.** RF-4
  - Hecho cuando:
    - [x] Una línea dice que los pasos de línea base o chequeo posterior de estructura citan
      `/sdd-estructura`, que corre la sesión

## F2 — Cascada y lotes (estimado: 10 min)
- [x] **F2.1 `sdd-implement.md`: tres formas, tabla previa, lotes, olas, revisión y paros.** RF-5 a RF-9
  - Hecho cuando:
    - [x] `description` y `argument-hint` muestran `[Fn | Fn.m]`, con E9 cumplido
    - [x] Las formas todo y fase hacen cascada; la de un paso para; las tres paran si `04_tasks.md`
      no está aprobado
    - [x] La tabla de RF-6, el criterio de lote de RF-7 (con la definición de «depende»), la
      revisión por lote de RF-8 y los paros de RF-9 están; S-2 (tope de 4) queda escrito como
      umbral que se revisa tras 3 usos
- [x] **F2.2 `implementer.md`: paso o lote.** RF-7
  - Hecho cuando:
    - [x] `description` y cuerpo dicen «un paso o un lote de pasos» y remiten a `sdd-implement.md`
- [x] **F2.3 `README.md`: cascada.** RF-5
  - Hecho cuando:
    - [x] Diagrama (línea «un paso a la vez»), ejemplo de uso y tablas de comando y rol reflejan
      las tres formas
    - [x] Los bloques Mermaid siguen cerrados (mismo número de aperturas y cierres de bloque)
- [x] **F2.4 `SKILL.md`: fila «Implementación» de la tabla de fases.** RF-5
  - Hecho cuando:
    - [x] La celda dice «el paso `Fn.m`, la fase o todo (cascada)»
- [x] **F2.5 `avance-y-tiempos.md`: fila de lote.** RF-10
  - Hecho cuando:
    - [x] «Mínimo y lote» dice: una fila por lote, nombra sus pasos, minutos reales, Nota «lote»,
      cuenta en la fase de su primer paso
- [x] **F2.6 `sdd-tasks.md`: siguiente paso.** RF-5
  - Hecho cuando:
    - [x] El paso 5 propone `/sdd-implement $1` (cascada) o el primer paso

## F3 — Clarify (estimado: 5 min)
- [x] **F3.1 `topes-y-modos.md`: autochequeo y delta.** RF-11 a RF-14
  - Hecho cuando:
    - [x] Bajo «Pasadas de clarify», el autochequeo (no cuenta como pasada, no es validación) y la
      pasada por delta (quién arma la lista, RF-13, delta igual a la spec entera cuando la corrección
      la reescribe)
    - [x] Tope y modo de corrección sin cambio
- [x] **F3.2 `sdd-clarify.md`: remisión.** RF-11, RF-12
  - Hecho cuando:
    - [x] Los pasos 2 y 3 remiten a `topes-y-modos.md` sin copiar la regla

## F4 — Chequeos de cierre (estimado: 6 min)
- [x] **F4.1 `sdd-cierre.md`: la previa corre los chequeos de cierre.** RF-15
  - Hecho cuando:
    - [x] El punto 4 dice que la previa corre los chequeos de cierre de `AGENTS.md` › Comandos, si
      los hay, y no escribe `Cerrado` ni `Implementado` si alguno falla
- [x] **F4.2 `template/AGENTS.md` y `sdd-init.md`.** RF-15
  - Hecho cuando:
    - [x] Comandos lleva `- Chequeos de cierre: No aplica`; tamaño medido y anotado
    - [x] `sdd-init.md` propone esa línea junto con Comandos
- [x] **F4.3 `AGENTS.md` de la raíz: E20.** RF-16
  - Hecho cuando:
    - [x] Comandos declara E20 como chequeo de cierre cuando cambia `VersionAgentKitSdd`, después del
      último cambio a `template/`, con la regeneración de filas si falla

## F5 — Réplica, versión, ADR y manifiesto (estimado: 8 min)
- [x] **F5.1 Réplica y lint.** RNF-1
  - Hecho cuando:
    - [x] `cp` de cada archivo de `template/.agents/` tocado a `.agents/`; `diff -rq template/.agents
      .agents` solo con las excepciones aceptadas
- [x] **F5.2 Versión 0.14.0 y changelog.** RNF-3
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` = `0.14.0`; entrada en `ChangelogAgentKitSdd.md`
- [x] **F5.3 ADR 0036 (si se aprueba).** RF-5, RF-7
  - Hecho cuando:
    - [x] Skill `adr`: archivo e índice de `.docs/004_adr/`; enlace desde `03_plan.md`
- [x] **F5.4 Manifiesto 0.14.0 y E20.** RF-16
  - Hecho cuando:
    - [x] `manifiestos/0.14.0.md` generado después del último cambio a `template/`; E20 sin fallos
    - [x] Ensayo de instalación en `mktemp -d` sin pisar nada
