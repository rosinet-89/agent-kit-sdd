# 04 — Tareas: Bitácora de uso (beta)
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

«Hecho cuando» común a todo paso que toca un archivo (no se repite abajo): lint de `AGENTS.md` ›
Comandos en el archivo (UTF-8 sin BOM; `name` del frontmatter igual a su carpeta; rutas citadas
existentes; en `template/`, sin marcadores `<<…>>` salvo los dos admitidos). Evidencia de datos y
verificación manual de UI: No aplica, salvo que el paso diga otra cosa.

Nombres fijos que usan varios pasos (se escriben tal cual, `03_plan.md` › 1 y 6):
- Skill: `template/.agents/skills/bitacora-uso/SKILL.md`, `name: bitacora-uso`.
- Secciones de la skill: «Activación», «Formato», «Registro por tramo», «Actividad», «Fuente de
  tokens», «Completar tokens», «Reporte», «Privacidad».
- Sección de la configuración local: `## Bitácora`, valores `encendida` o `apagada`; paso `bitacora`.
- Carpeta local: `.agents/local/bitacora/`; archivos `<carpeta>.md` y `actividad-<etiqueta>.md`.
- Marcas de la entrada: «pendiente», «no disponible», «no disponible (sesiones paralelas)»,
  «ventana con pausa».

Regla de remisión para F2 a F4: fuera de la skill no se copia ninguna regla de la bitácora; se
nombra la skill y la sección.

## Lotes y olas
| Ola | Lote | Pasos | Archivos | Quién |
|---|---|---|---|---|
| 0 | — | F0.1 | — | sesión |
| 1 | L1 | F1.1, F1.2, F1.3, F1.4 | `bitacora-uso/SKILL.md` | implementer |
| 1 | L2 | F2.1, F2.2 | `configuracion.ejemplo.md`, `sdd-config.md` | implementer |
| 1 | L3 | F3.1, F3.2, F3.3, F3.4 | `sdd-spec.md`, `sdd-cierre.md`, `avance-y-tiempos.md`, `AGENTS.md` (plantilla y raíz) | implementer |
| 1 | L4 | F4.1, F4.2, F4.3 | `README.md`, `INSTALAR.md`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` | implementer |
| 2 | — | F5.1, F5.2, F5.3, F5.4 | réplica, enlaces, ensayo, chequeos | sesión |
| 3 | — | F6.1 | `manifiestos/0.16.0.md` | sesión (último cambio a `template/`) |
| aparte | — | — | ADR 0038 | `/sdd-cierre`, punto 15 (decisión 13 de `05_avance.md`) |

## F0 — Preparación (estimado: 3 min)
- [x] **F0.1 Línea base.** —
  - Hecho cuando:
    - [x] `/sdd-estructura` corrido; resultado anotado en `05_avance.md`
    - [x] `git status --short template/` vacío antes de empezar
    - [x] Las secciones que citan F2 a F4 existen hoy: `configuracion.ejemplo.md` › «Formato del
      archivo local», `avance-y-tiempos.md` › «Tiempos reales», `restricciones-de-rol.md` › «Modelo
      por rol», `INSTALAR.md` › «Qué hace el agente» paso 7

## F1 — Skill `bitacora-uso` (estimado: 15 min)
- [x] **F1.1 Frontmatter, «Activación» y «Formato».** RF-4, RF-5, RF-6, RF-7, RF-16, RF-17
  - Hecho cuando:
    - [x] Frontmatter con `name: bitacora-uso` y `description` de 1024 caracteres o menos, con qué
      hace, cuándo activarla y disparadores («bitácora», «registrar actividad», «reporte de la
      bitácora», `/sdd-config`, `/sdd-spec`, `/sdd-cierre`); el cuerpo dice que es beta y local
    - [x] «Activación»: interruptor en `## Bitácora`; sin sección, `apagada` (RF-3); una sola
      pregunta en `/sdd-spec` antes de redactar, con sí sugerido si está `encendida` y no si está
      `apagada` (RF-4, RF-5); otros comandos sin decisión aplican el interruptor sin preguntar
      (RF-7); cambiar la decisión rige desde ese momento y conserva lo escrito (RF-6); casos límite
      de apagar o encender a mitad y de otro dev
    - [x] «Formato»: un archivo por spec o actividad en `.agents/local/bitacora/`, encabezado con
      decisión por spec y su hora (o `estado` de la actividad), tabla con las columnas de RF-8 más
      entrada, salida, lectura de caché y escritura de caché; la carpeta se crea con la primera
      entrada; ilegible o formato viejo: aviso en una línea y no se sobrescribe sin confirmar
- [x] **F1.2 «Registro por tramo» y «Actividad».** RF-8, RF-9, RF-10, RF-11
  - Hecho cuando:
    - [x] «Registro por tramo»: al escribir la fila de Tiempos, en el mismo momento, una entrada con
      fecha y hora, carpeta, paso, rol, modelo (el de la Nota), motivo si lo hubo, minutos, nota y
      tokens «pendiente»; un lote = una entrada; pausa > 0 lleva «ventana con pausa»; solo la sesión
      principal escribe y el subagente solo devuelve su modelo (RF-11); con una actividad abierta, un
      comando `/sdd-*` de una spec activa escribe en la de la spec
    - [x] «Actividad»: se abre solo si la persona la nombra con su etiqueta (S-2); una entrada por
      turno, sin carpeta ni paso; termina al cerrarla, al cambiar de etiqueta o con la sesión; una
      `estado: abierta` de otra sesión cuenta como cerrada (`03_plan.md`, decisión 9); con el
      interruptor `apagada`, una línea que lo dice y cómo encenderlo (`/sdd-config`) y no registra
- [x] **F1.3 «Fuente de tokens» y «Completar tokens».** RF-12, RF-13
  - Hecho cuando:
    - [x] «Fuente de tokens»: tabla por herramienta. Claude Code: `$CLAUDE_CONFIG_DIR/projects/` (o
      `~/.claude/projects/` sin esa variable), carpeta del proyecto con `/` cambiada por `-`,
      `*.jsonl` de sesión y `<sesión>/subagents/*.jsonl`. Cursor y OpenCode: «no disponible». Otra
      herramienta sin fila: «no disponible». Es el único lugar con nombres de herramienta (RNF)
    - [x] «Completar tokens»: cuándo (cada corrida de `/sdd-cierre` sobre una spec activa, en los
      tres caminos, o al pedir el reporte); ventana de Inicio a Fin, minutos completos, convertida a
      UTC con la zona del sistema; solo líneas `assistant` con `timestamp` dentro; una vez por
      `message.id`; se suman `input_tokens`, `output_tokens`, `cache_read_input_tokens` y
      `cache_creation_input_tokens`, la caché aparte (S-3); más de una sesión del proyecto en la
      ventana: «no disponible (sesiones paralelas)» salvo que la persona elija; tramo no ubicable o
      fuente ausente: «no disponible», sin estimar
    - [x] Solo se leen `timestamp`, `message.id`, `message.model` y `message.usage`; el comando de
      lectura es de un solo uso, no se guarda ni se versiona, y no imprime contenido de mensajes
- [x] **F1.4 «Reporte» y «Privacidad».** RF-13, RF-14, RF-15
  - Hecho cuando:
    - [x] «Reporte»: se pide en lenguaje natural para una spec, una actividad o un rango de fechas
      (S-1); primero completa tokens; en el chat, listo para copiar: entradas por tramo, totales de
      minutos y tokens por modelo y por rol, y los cuatro hallazgos de RF-14, ni uno más; el de
      modelo distinto del rol sigue `03_plan.md`, decisión 8 («sin comparar» fuera de Claude Code);
      sin entradas: lo dice y no inventa
    - [x] «Reporte» declara que no escribe archivos versionados, no abre issues y no usa la red (RF-15)
    - [x] «Privacidad»: solo métricas y notas cortas; nunca texto de prompts ni respuestas,
      credenciales, nombres de secretos ni datos personales (RF-13)

## F2 — Configuración (estimado: 6 min)
- [x] **F2.1 `configuracion.ejemplo.md`.** RF-1, RF-2, RF-3, RF-17
  - Hecho cuando:
    - [x] `## Bitácora` agregado a la lista de nombres fijos de «Formato del archivo local»
    - [x] «Paso 6. Bitácora de uso (beta)»: contexto en 2 líneas (beta, local, no se versiona; qué
      guarda y qué no, por remisión a la skill › «Privacidad»), valores `encendida` o `apagada`,
      sugerido `apagada`; sin copiar reglas de la skill
- [x] **F2.2 `sdd-config.md`.** RF-1, RF-2, RF-17
  - Hecho cuando:
    - [x] `bitacora` en la lista de `[paso]` de la línea de uso
    - [x] El paso se presenta como beta, con lo que guarda y lo que no, por remisión al ejemplo y a
      la skill; los demás pasos sin cambio

## F3 — Remisiones (estimado: 8 min)
- [x] **F3.1 `sdd-spec.md`.** RF-4, RF-5
  - Hecho cuando:
    - [x] Al final del paso 3, antes de ejecutar el planner: sin decisión por spec de este dev, una
      pregunta según la skill › «Activación»; sin renumerar los pasos (otros archivos los citan)
- [x] **F3.2 `sdd-cierre.md`.** RF-12
  - Hecho cuando:
    - [x] En el punto 1, una frase que vale en los tres caminos y en cada corrida, aunque la corrida
      se detenga después: si la spec tiene bitácora activa, completa tokens según la skill ›
      «Completar tokens»; sin renumerar los puntos ni cambiar el orden de los criterios
- [x] **F3.3 `avance-y-tiempos.md`.** RF-8, RF-11
  - Hecho cuando:
    - [x] En «Tiempos reales», una viñeta **Bitácora**: con bitácora activa, la sesión principal
      agrega la entrada al escribir la fila, según la skill › «Registro por tramo»; el formato de la
      fila no cambia
- [x] **F3.4 `AGENTS.md` (plantilla y raíz).** RF-16
  - Hecho cuando:
    - [x] Fila en «Skills de proyecto» de `template/AGENTS.md` y de `AGENTS.md`, con el tema de
      `03_plan.md` › 2 y la ruta `.agents/skills/bitacora-uso/SKILL.md`

## F4 — Docs y versión (estimado: 5 min)
- [x] **F4.1 `README.md`.** RF-17
  - Hecho cuando:
    - [x] `bitacora-uso` en la línea `skills/` del árbol de `template/`; una línea: bitácora de uso
      (beta), local, apagada por omisión, se activa con `/sdd-config`
- [x] **F4.2 `INSTALAR.md`.** RF-17
  - Hecho cuando:
    - [x] Paso 7 de «Qué hace el agente»: la bitácora de uso (beta) se activa en `/sdd-config` y está
      apagada por omisión
- [x] **F4.3 Versión y changelog.** RF-17
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` en `0.16.0`; entrada `## 0.16.0 — 2026-11-22` en
      `ChangelogAgentKitSdd.md` con la palabra beta y los archivos tocados

## F5 — Réplica y chequeos (estimado: 8 min)
- [x] **F5.1 Réplica local.** —
  - Hecho cuando:
    - [x] `cp` de lo tocado en `template/.agents/` (incluida `skills/bitacora-uso/`) a `.agents/`
    - [x] `diff -rq template/.agents .agents` solo con las diferencias admitidas en `MEMORY.md`
- [x] **F5.2 `/sdd-enlaces`.** RF-16
  - Hecho cuando:
    - [x] `.claude/skills` ve `bitacora-uso` (enlace de carpeta o enlace de la skill); confirmación de
      la persona si hay que crear o corregir algo
- [x] **F5.3 Ensayo de instalación.** criterio 2
  - Hecho cuando:
    - [x] Comando de `AGENTS.md` › Comandos (Test) en `mktemp -d`: `skills/bitacora-uso/SKILL.md` y
      los archivos tocados presentes; nada pisado; carpeta borrada al terminar
- [x] **F5.4 Chequeos.** RF-1, RF-16, RF-17
  - Hecho cuando:
    - [x] Lint en cada archivo tocado; casos CP-01 a CP-06 de `07_casos_prueba.md` (CP-07 tras F6.1)
    - [x] `/sdd-estructura` sin `FALLA` en E4 a E7
    - [x] CP-08 en el chat; CP-09 con la persona (escribe en `.agents/local/`, se detiene por su
      confirmación); corridas anotadas en `07_casos_prueba.md`

## F6 — Manifiesto (estimado: 5 min)
- [x] **F6.1 Manifiesto y E20.** criterio 3
  - Hecho cuando:
    - [x] `manifiestos/0.16.0.md` generado tras el último cambio a `template/`
    - [x] E20 de `template/.agents/validar-estructura.md` sin `FALLA`; si falla, se regeneran las
      filas tocadas, se anota como decisión y se vuelve a correr (`AGENTS.md` › Comandos)
