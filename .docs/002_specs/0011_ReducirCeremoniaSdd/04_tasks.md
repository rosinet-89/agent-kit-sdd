# 04 — Tareas: Reducir la ceremonia del flujo SDD
Spec: 01_spec.md (aprobada 2026-11-03 22:14) · Plan: 03_plan.md (aprobado 2026-11-03 22:22)
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## Reglas para el implementer
- `Read` antes de editar; `Edit` mínimo, nunca reescribir un archivo entero salvo la pieza nueva.
- Nunca ejecuta SQL (no aplica: sin `sql/`). Sin scripts ni código. Sin `git add` ni commit.
- No toca `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` ni `manifiestos/` fuera de F9.
- No escribe `05_avance.md` ni marca `[x]`: lo hace la sesión principal.
- Regla única: tope, prioridad y condiciones se escriben solo en `topes-y-modos.md`; el resto enlaza en una frase.
- Se edita en `template/`; la réplica a la raíz se hace solo en F6.
- «Lint» = `file -i <f>` da utf-8; `command grep -l $'^\xEF\xBB\xBF' <f>` vacío; `name` = carpeta; rutas citadas existen; en `template/` sin `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.

## Pasos de la sesión principal (no del implementer)
Todas las marcas `[x]`, toda escritura en `05_avance.md` (incluida F7.5 M1/M2), F8.1 (delegar al reviewer), F9.2 (ADR con la skill `adr` tras visto bueno) y el cierre `/sdd-cierre`.

## F0 — Preparación (estimado: 5 min)
- [x] **F0.1 Estado de partida.** —
  - Hecho cuando:
    - [x] `git status --short` solo muestra la carpeta 0011 y `.docs/002_specs/README.md`
    - [x] `diff -rq template/.agents .agents` solo muestra `napkin.md` y `skills/caveman`
- [x] **F0.2 Líneas a editar localizadas.** RF-10, RF-19
  - Hecho cuando:
    - [x] `command grep -n 'NECESITA ACLARACI' -r template .agents AGENTS.md` lista los archivos a tocar
    - [x] `command grep -n 'seis documentos' template/AGENTS.md AGENTS.md` da las líneas 104 y 105
- [x] **F0.3 Sin spec en curso que comparta archivos.** RF-8
  - Hecho cuando:
    - [x] `ls .docs/002_specs` no muestra otra carpeta sin cerrar que toque `template/`

## F1 — Fuente única y skill `sdd` (estimado: 20 min)
- [x] **F1.1 Crear `template/.agents/skills/sdd/topes-y-modos.md`.** RF-1 a RF-9, RF-11 a RF-16
  - Hecho cuando:
    - [x] Contiene: pasada y tope 3, nota «pasada n de 3», marcador `[ACLARAR: …]` (tope 3, prioridad alcance > seguridad > experiencia > detalle, desempate, bloqueo), 8 condiciones con umbrales, spec corta (≤ 5 000 B, 8 RF), promoción (pasada cuenta como 1, fila `Modo` corregida), revisión cruzada
    - [x] Lint OK en el archivo (`name` = `sdd` si lleva frontmatter; si no lo lleva, igual que sus hermanas)
- [x] **F1.2 `SKILL.md` línea 97 sin copiar el marcador.** RF-19
  - Hecho cuando:
    - [x] Remite a `plantillas.md` y a `topes-y-modos.md`; `command grep -n 'NECESITA' template/.agents/skills/sdd/SKILL.md` da 0
- [x] **F1.3 `SKILL.md`: fila de enlace a la pieza y `01`/`06` según modo.** RF-7 a RF-14
  - Hecho cuando:
    - [x] La pieza está enlazada y las rutas citadas existen
    - [x] Lint OK en `SKILL.md` (`name: sdd`)
- [x] **F1.4 Verificación de F1.** —
  - Hecho cuando:
    - [x] `command grep -c '3 pasadas' template/.agents/skills/sdd/SKILL.md` da 0

## F2 — Plantillas y avance (estimado: 15 min)
- [x] **F2.1 `plantillas.md`: línea 25 `- [ACLARAR: <duda>]`.** RF-19
  - Hecho cuando:
    - [x] `command grep -n 'ACLARAR' template/.agents/skills/sdd/plantillas.md` muestra la forma; `NECESITA` da 0
- [x] **F2.2 `plantillas.md`: spec corta y `04_tasks.md` con plan dentro.** RF-10
  - Hecho cuando:
    - [x] Spec corta omite Usuarios, Historias, Definiciones, RNF y Fuera de alcance, con límite de 5 000 B y 8 RF
    - [x] `04` ligero declara límite de 8 000 B; fila «Modo» en PUNTO DE RETOMO de `05_avance.md` (RF-13)
    - [x] Lint OK en `plantillas.md`
- [x] **F2.3 `avance-y-tiempos.md`: remite a «Modo» y «pasada n de 3» sin repetir la regla.** RF-1, RF-5, RF-13
  - Hecho cuando:
    - [x] `command grep -n '3 pasadas' template/.agents/skills/sdd/avance-y-tiempos.md` da 0; lint OK

## F3 — Siete comandos (estimado: 28 min)
Cada uno: una frase que enlaza `topes-y-modos.md`, sin copiar reglas. Hecho cuando común: lint OK en el archivo y `command grep -n 'topes-y-modos' <archivo>` da ≥ 1.
- [x] **F3.1 `sdd-clarify.md`: cuenta pasada, anota «pasada n de 3», cierre de tope, una pasada en ligero.** RF-1, RF-2, RF-3, RF-6, RF-11
  - Hecho cuando:
    - [x] Lint OK y enlace presente; `NECESITA` da 0
- [x] **F3.2 `sdd-spec.md`: lista de 8 condiciones (sí/no y evidencia), propone modo, una confirmación, crea los documentos del modo.** RF-2, RF-3, RF-7 a RF-10, RF-13
  - Hecho cuando:
    - [x] Lint OK y enlace presente; `NECESITA` da 0
- [x] **F3.3 `sdd-plan.md`: paso 1 para si hay `[ACLARAR` sin resolver y dice cuáles; anota la decisión al resolver.** RF-4, RF-5
  - Hecho cuando:
    - [x] Lint OK y enlace presente
- [x] **F3.4 `sdd-tasks.md`: en ligero, `04` con plan dentro y bloqueo por marcadores en el paso 1.** RF-4, RF-10, RF-11
  - Hecho cuando:
    - [x] Lint OK y enlace presente; incluye tarea de bump y changelog si el cambio se distribuye (RF-15)
- [x] **F3.5 `sdd-change.md`: promoción a completo.** RF-12
  - Hecho cuando:
    - [x] Dice qué se conserva (`01`, `04`) y qué se completa (`02`, `03`, `06`); lint OK
- [x] **F3.6 `sdd-status.md`: lee el modo de PUNTO DE RETOMO; no espera `02`, `03`, `06` en ligero ni en specs antiguas.** RF-13, RF-14
  - Hecho cuando:
    - [x] Lint OK; no añade lecturas a las de 003 (`Estado`, lista, PUNTO DE RETOMO, Pasos, Pruebas, Tiempos)
- [x] **F3.7 `sdd-cierre.md`: `06_flujo.md` aprobado solo si el modo es completo.** RF-14
  - Hecho cuando:
    - [x] Lint OK y enlace presente

## F4 — Agentes: promoción y revisión cruzada (estimado: 15 min)
- [x] **F4.1 `reviewer.md`: modo «revisión cruzada» de solo lectura y una pasada en ligero.** RF-1, RF-16
  - Hecho cuando:
    - [x] `tools` sin `Write` ni `Edit`; enlaza la pieza; lint OK
- [x] **F4.2 `coordinator.md`: al cerrar cada paso en ligero revisa las condiciones salvo la 7 y promueve; fila de ruteo de la revisión cruzada.** RF-12, RF-16
  - Hecho cuando:
    - [x] Enlaza la pieza sin copiar las condiciones; lint OK
- [x] **F4.3 Sin comando nuevo.** RF-16
  - Hecho cuando:
    - [x] `ls template/.agents/commands | wc -l` da 15

## F5 — AGENTS.md y READMEs de specs (estimado: 10 min)
- [x] **F5.1 `template/AGENTS.md` línea 104.** RF-10, RF-18
  - Hecho cuando:
    - [x] Dice «`01`, `04`, `05` siempre; `02`, `03`, `06` en modo completo» con enlace a la pieza; sin copiar condiciones; lint OK
- [x] **F5.2 `AGENTS.md` raíz línea 105, conservando lo propio del clon.** RF-10
  - Hecho cuando:
    - [x] Misma frase que F5.1; lint OK (sin la regla `<<…>>`, no aplica a raíz)
- [x] **F5.3 Línea 17 de `template/.docs/002_specs/README.md` y de `.docs/002_specs/README.md`.** RF-14
  - Hecho cuando:
    - [x] La frase admite el modo ligero; la fila 0011 NO se toca (la hace `/sdd-cierre`); lint OK en ambos

## F6 — Réplicas (estimado: 5 min)
- [x] **F6.1 `cp` de cada archivo editado de `template/.agents/` a `.agents/`.** criterio 5
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents` solo muestra `napkin.md` y `skills/caveman`
- [x] **F6.2 Lint de las réplicas.** criterio 5
  - Hecho cuando:
    - [x] `file -i` utf-8 y sin BOM en `.agents/skills/sdd/topes-y-modos.md` y los demás copiados

## F7 — Verificación y medición (estimado: 30 min)
- [x] **F7.1 Fuente única.** RF-19
  - Hecho cuando:
    - [x] `command grep -rn 'NECESITA ACLARACI' template .agents AGENTS.md` da 0
    - [x] `command grep -rn 'ACLARAR' template` solo muestra la forma (`plantillas.md`), la regla (`topes-y-modos.md`) y enlaces
    - [x] «3 pasadas» solo en `topes-y-modos.md`
- [x] **F7.2 Ensayo de instalación.** criterio 6
  - Hecho cuando:
    - [x] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia 44 archivos; repetido sobre un destino con `AGENTS.md` propio, no cambia
- [x] **F7.3 Lecturas de 003 sin crecer.** RNF
  - Hecho cuando:
    - [x] `git diff --stat` no incluye `implementer.md` ni `sdd-implement.md`; `sdd-status.md` sin lecturas nuevas
- [x] **F7.4 Specs existentes intactas.** criterio 8
  - Hecho cuando:
    - [x] `git diff --stat -- '.docs/002_specs/00*'` vacío (no cuenta la carpeta 0011)
- [x] **F7.5 Medición M1 y M2 (sesión principal escribe en `05_avance.md`).** RF-17
  - Hecho cuando:
    - [x] M1: bytes de lectura por pasada = spec + constitución + `sdd-clarify.md` + `reviewer.md` (`wc -c`, tamaños nuevos), con 0010 en 7 pasadas frente al tope de 3 y 0011 en 2; M2: 006 (34 031 B) frente al conjunto ligero con sus límites
    - [x] Declarado: es estimación en bytes de lectura, no tokens; pasadas y documentos se informan aparte
- [x] **F7.6 Tabla de calibración de umbrales.** criterio 3
  - Hecho cuando:
    - [x] `02_analisis_impacto.md` § 3.1 existe y declara qué specs 001 a 0010 habrían sido ligeras (solo lectura)

## F8 — Revisión (estimado: 15 min)
- [x] **F8.1 `/sdd-validate` por el reviewer, máximo 3 pasadas (lo delega la sesión principal).** todos
  - Hecho cuando:
    - [x] Sin hallazgos críticos abiertos; el reviewer no escribió archivos (`git status --short` sin cambios nuevos)
- [x] **F8.2 Corregir hallazgos con Edit mínimo y repetir lint de F1 a F6 en lo tocado.** todos
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents` sigue limpio salvo `napkin.md` y `skills/caveman`

## F9 — Cierre y publicación (estimado: 30 min). Última: tras la última edición de `template/`
- [x] **F9.1 `VersionAgentKitSdd` a `0.7.0`.** RF-18
  - Hecho cuando:
    - [x] `cat VersionAgentKitSdd` da `0.7.0`; sin BOM
- [x] **F9.2 Crear el ADR único «Ceremonia proporcional» (sesión principal, skill `adr`).** plan § 6
  - Hecho cuando:
    - [x] Número libre verificado en local y remoto: `ls .docs/004_adr/` y `git ls-tree --name-only @{u} .docs/004_adr/`; se espera 0023
    - [x] El ADR cubre modo ligero con 8 condiciones y promoción, tope de 3 pasadas, y marcador único `[ACLARAR: …]` con prioridad, con sus alternativas descartadas
    - [x] Fila nueva en `.docs/004_adr/README.md`; lint OK en ambos
- [x] **F9.3 `ChangelogAgentKitSdd.md`: entrada 0.7.0.** RF-18
  - Hecho cuando:
    - [x] Registra los tres cambios y la nota: «incorpora a mano en `AGENTS.md` la línea de documentos por feature antes de usar el modo ligero»; lint OK
- [x] **F9.4 Confirmar que no habrá más ediciones de `template/`.** plan F9
  - Hecho cuando:
    - [x] `git status --short template` estable tras F8; sin cambios pendientes por hacer
- [x] **F9.5 `manifiestos/0.7.0.md` con 44 filas y huellas `kit_huella` (`ACTUALIZAR.md`).** RF-18
  - Hecho cuando:
    - [x] 44 filas; recalcular `kit_huella` sobre `template/` da 0 diferencias
    - [x] Frente a `manifiestos/0.6.0.md` solo cambian las piezas editadas y una fila nueva (`topes-y-modos.md`)
- [x] **F9.6 Verificación final de publicación.** criterios 5, 7
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents` limpio salvo `napkin.md` y `skills/caveman`; lint OK en `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`
- [x] **F9.7 Cierre `/sdd-cierre` (sesión principal).** criterio 10
  - Hecho cuando:
    - [x] Fila 0011 del índice de specs al día; sin commit hasta que la persona lo pida
