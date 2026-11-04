# 04 — Tareas: Falso positivo de SDD-INIT-PENDIENTE tras /sdd-init
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado
Aprobadas por el usuario: 2026-11-01 15:17. Orden de implementación: 0008, 0010, 0009; 0010 no empieza hasta que 0008 esté implementado.

Textos exactos: `03_plan.md` (2.1 paso 1 y fila coordinator, 2.2 piezas de RF-5, 2.3 E21 y mensajes de FALLA, 2.4 sdd-estructura). El implementer los copia de ahí; esta lista solo los referencia. Estimado total: 115 min.

Reglas para todos los pasos (no se repiten abajo):
- Archivos compartidos con 0008 y 0009 (`coordinator.md`, `sdd-estructura.md`, `template/AGENTS.md`, `AGENTS.md` raíz, `sdd.md`, `sdd-init.md`, `skills/sdd/SKILL.md`, `validar-estructura.md`): releer el archivo justo antes de cada Edit; `old_string` mínimo (la frase actual, nunca número de línea); lo que 0008 o 0009 hayan añadido en la misma línea no se toca.
- Se edita `template/` y se replica a `.agents/` con `cp`. El `AGENTS.md` de la raíz se edita aparte.
- Rutas que no existen en un proyecto instalado (`template/AGENTS.md`, `.agents/.kit-version`, `template/`) van sin comillas inversas en las piezas nuevas (plan, decisión 5).
- Lint por archivo tocado (`AGENTS.md` › Comandos): `file -i <archivo>` da utf-8; `grep -l $'^\xEF\xBB\xBF' <archivo>` vacío; rutas citadas existentes; en `template/` sin `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. Cada «Hecho cuando» lo da por incluido como «Lint OK».
- Evidencia de datos y verificación manual UI: No aplica en todos los pasos (consulta de datos de `AGENTS.md` › Comandos: No aplica).
- Sin commits: los decide el usuario.

## F0 — Preparación (estimado: 10 min)
- [x] **F0.1 Releer el estado de 0008 y 0009 y de Git (4 min).** —
  - Hecho cuando:
    - [x] `git status --short` revisado; leídas `05_avance.md` de `0008_GuardrailsBdSoloLectura` y `0009_EnlacesTrasPull` (solo lectura) y anotado en `05_avance.md` de 0010 qué archivos compartidos ya tocaron (lo ejecuta la sesión principal; el implementer solo devuelve el texto, tiene prohibido escribir `05_avance.md`)
    - [x] No se tocó nada
- [x] **F0.2 Localizar las frases a cambiar y estado previo de réplicas (3 min).** RF-5
  - Hecho cuando:
    - [x] `grep -rn 'SDD-INIT-PENDIENTE' template .agents README.md AGENTS.md` muestra las piezas de `03_plan.md` 2.2 y las dos frases de la raíz
    - [x] `diff -rq template/.agents .agents` muestra solo `napkin.md` y `skills/caveman`
- [x] **F0.3 Confirmar punto de partida de E21 (3 min).** RF-6a
  - Hecho cuando:
    - [x] `grep -n 'E1 a E20\|E1–E20' .agents/validar-estructura.md .agents/commands/sdd-estructura.md` devuelve las líneas esperadas (plan 2.3 y 2.4)
    - [x] `grep -c '^| E[0-9]' .agents/validar-estructura.md` = 20

## F1 — `AGENTS.md`: paso 1 y fila coordinator (estimado: 15 min)
- [x] **F1.1 Paso 1 de «Cada sesión» en `template/AGENTS.md` (5 min).** RF-1, RF-2, RF-3, RF-4, RF-7
  - Hecho cuando:
    - [x] Releído el archivo y Edit mínimo; el texto es el del plan 2.1 (paso 1), sin literal ni `<!--`, y con la ruta de la plantilla sin comillas inversas
    - [x] `grep -o 'SDD-INIT-PENDIENTE' template/AGENTS.md | wc -l` = 2 (hoy 3: marcador, paso 1 y fila coordinator; queda el marcador y la fila)
    - [x] `sed 's/\r$//' template/AGENTS.md | awk 'f&&NF{print;exit} /^# /{f=1}' | sed 's/[[:space:]]*$//'` es igual a la cadena del marcador de `01_spec.md` › Definiciones
    - [x] Lint OK en `template/AGENTS.md`
- [x] **F1.2 Fila coordinator de «Qué lee cada rol» en `template/AGENTS.md` (3 min).** RF-2
  - Hecho cuando:
    - [x] Releído y Edit mínimo; la fila es la del plan 2.1 (remite al paso 1, sin literal)
    - [x] `grep -o 'SDD-INIT-PENDIENTE' template/AGENTS.md | wc -l` baja de 2 a 1 (solo el marcador)
    - [x] Lint OK
- [x] **F1.3 Paso 1 en el `AGENTS.md` de la raíz (4 min).** RF-1, RF-7
  - Hecho cuando:
    - [x] Releído y Edit mínimo con la misma frase de 2.1; el paso 0 queda intacto
    - [x] `grep -o 'SDD-INIT-PENDIENTE' AGENTS.md | wc -l` baja a la cuenta de la fila coordinator pendiente (1 o menos)
    - [x] Lint OK en `AGENTS.md`
- [x] **F1.4 Fila coordinator en el `AGENTS.md` de la raíz (3 min).** RF-1, RF-7
  - Hecho cuando:
    - [x] Releído y Edit mínimo con la fila de 2.1
    - [x] `grep -o 'SDD-INIT-PENDIENTE' AGENTS.md | wc -l` = 0 (única vigilancia de la raíz)
    - [x] Lint OK

## F2 — Piezas que citan la regla (estimado: 20 min)
Cada paso: la pieza queda con una frase que remite al paso 1 de «Cada sesión» y sin condición «contiene»; frases exactas en el plan 2.2.
- [x] **F2.1 `template/.agents/commands/sdd.md` (4 min).** RF-5
  - Hecho cuando:
    - [x] Releído y Edit mínimo (compartido con 0008/0009); dos ediciones (líneas «y de `AGENTS.md` solo si contiene…» y «Si `AGENTS.md` contiene…») según el plan 2.2
    - [x] `grep -n 'contiene `SDD-INIT' template/.agents/commands/sdd.md` vacío
    - [x] Lint OK
- [x] **F2.2 `template/.agents/agents/coordinator.md` (4 min).** RF-5
  - Hecho cuando:
    - [x] Releído (archivo compartido con 0008); dos ediciones según el plan 2.2; lo que 0008 haya añadido en la misma línea intacto
    - [x] `grep -n 'contiene `SDD-INIT' template/.agents/agents/coordinator.md` vacío
    - [x] Lint OK
- [x] **F2.3 `template/.agents/skills/sdd/SKILL.md` (3 min).** RF-5
  - Hecho cuando:
    - [x] Releído y Edit mínimo (compartido con 0008/0009); una edición según el plan 2.2
    - [x] `grep -n 'contiene `SDD-INIT' template/.agents/skills/sdd/SKILL.md` vacío
    - [x] Lint OK
- [x] **F2.4 `template/.agents/commands/sdd-init.md` (3 min).** RF-5
  - Hecho cuando:
    - [x] Releído y Edit mínimo (compartido con 0008/0009); solo cambia la frase «ya no contiene…» (plan 2.2); la frase «Quita el marcador…» no se toca
    - [x] `grep -n 'SDD-INIT-PENDIENTE' template/.agents/commands/sdd-init.md` muestra solo la frase que nombra lo que se borra
    - [x] Lint OK
- [x] **F2.5 `README.md` (3 min).** RF-5
  - Hecho cuando:
    - [x] Solo cambia «Mientras exista…» (plan 2.2); la otra mención queda
    - [x] `grep -n SDD-INIT-PENDIENTE README.md` muestra solo la línea que cita la regla tras el cambio (o 0 si el plan 2.2 reformula «Mientras exista…» sin el literal; indicar cuál ocurrió según el plan 2.2)
    - [x] Rutas citadas existentes; `file -i README.md` utf-8; sin BOM
- [x] **F2.6 Copiar a `.agents/` y comparar (3 min).** RF-5
  - Hecho cuando:
    - [x] `cp` de `sdd.md`, `coordinator.md`, `SKILL.md` y `sdd-init.md` desde `template/.agents/` a `.agents/` (misma ruta relativa)
    - [x] `diff -rq template/.agents .agents` muestra solo `napkin.md` y `skills/caveman`
    - [x] `grep -rn 'contiene `SDD-INIT' template .agents README.md AGENTS.md` vacío

## F3 — E21 en `validar-estructura` y `sdd-estructura` (estimado: 25 min)
- [x] **F3.1 Fila E21 y rangos en `template/.agents/validar-estructura.md` (6 min).** RF-6, RF-6a
  - Hecho cuando:
    - [x] Releído y Edit mínimo (compartido con 0008/0009); fila nueva tras E20 con el texto del plan 2.3 (rutas inexistentes sin comillas inversas); línea 9 «E1 a E21» y «Cómo responder» 3 «E1–E21»
    - [x] `grep -c '^| E[0-9]' template/.agents/validar-estructura.md` = 21
    - [x] `grep -n 'E1 a E20\|E1–E20' template/.agents/validar-estructura.md` vacío
    - [x] Lint OK
- [x] **F3.2 Sección «Detalle de E21»: introducción y pasos 1 a 5 (8 min).** RF-6
  - Hecho cuando:
    - [x] Releído y Edit mínimo (compartido con 0008/0009); sección entre la tabla y «Cómo responder» con los 5 pasos del plan 2.3, en ese orden, con los comandos `grep -o … | wc -l` y `sed … | awk … | sed …` del plan
    - [x] Sin comillas inversas en template/AGENTS.md, .agents/.kit-version ni template/
    - [x] Lint OK
- [x] **F3.3 Cadena del marcador y mensajes de FALLA (6 min; requiere F1.1: compara con el comentario de la plantilla).** RF-6
  - Hecho cuando:
    - [x] Párrafo «Cadena del marcador» con la cadena exacta (copia del comentario de template/AGENTS.md) y tabla de 4 mensajes de FALLA del plan 2.3, severidad ALTA
    - [x] La cadena copiada es igual a la extracción de F1.1: comparación con `diff <(grep -F 'SDD-INIT-PENDIENTE: este marcador' template/AGENTS.md | sed 's/[[:space:]]*$//') <(grep -F '<!-- SDD-INIT-PENDIENTE: este marcador' template/.agents/validar-estructura.md | sed 's/^`//;s/`$//;s/[[:space:]]*$//')` sin salida
    - [x] Lint OK
- [x] **F3.4 `template/.agents/commands/sdd-estructura.md` (3 min).** RF-6a
  - Hecho cuando:
    - [x] Releído (compartido con 0009); solo «E1 a E20» a «E1 a E21» y «E1–E20» a «E1–E21» (plan 2.4); «ni E20» y lo de 0009 intactos
    - [x] `grep -c SDD-INIT-PENDIENTE template/.agents/commands/sdd-estructura.md` = 0
    - [x] Lint OK
- [x] **F3.5 Copiar a `.agents/` y comparar (2 min).** RF-6a
  - Hecho cuando:
    - [x] `cp` de `validar-estructura.md` y `commands/sdd-estructura.md` a `.agents/`
    - [x] `diff -rq template/.agents .agents` muestra solo `napkin.md` y `skills/caveman`

## F4 — Verificación (estimado: 30 min)
Todo ensayo en `tmp=$(mktemp -d)` fuera del repo; nunca se toca el repo. El implementer aplica E21 leyendo `.agents/validar-estructura.md` › «Detalle de E21».
- [x] **F4.1 Lint de todos los archivos tocados (5 min).** RF-1 a RF-7
  - Hecho cuando:
    - [x] `file -i` utf-8 y sin BOM en `template/AGENTS.md`, `AGENTS.md`, `README.md` y las 6 piezas de `template/.agents/` tocadas (sdd.md, coordinator.md, SKILL.md, sdd-init.md, validar-estructura.md, sdd-estructura.md)
    - [x] Rutas citadas existentes y sin `<<…>>` en `template/` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] E11 sobre la cadena del marcador: no se lee como ruta
- [x] **F4.2 Regla del cero y marcador válido (3 min).** RF-1, RF-3
  - Hecho cuando:
    - [x] `grep -o 'SDD-INIT-PENDIENTE' template/AGENTS.md | wc -l` = 1 y la extracción de E21 paso 5 es igual a la cadena
    - [x] `grep -o 'SDD-INIT-PENDIENTE' AGENTS.md | wc -l` = 0
- [x] **F4.3 Lectura pieza por pieza de RF-5 (4 min).** RF-2, RF-5
  - Hecho cuando:
    - [x] Skill sdd, `sdd.md`, coordinator, `sdd-init.md` (frase «ya no contiene…»), README y fila coordinator: cada una tiene una frase que remite al paso 1 y ninguna condición de reconocimiento ajena a él (anotado pieza por pieza en la respuesta del implementer)
    - [x] `grep -rn 'contiene `SDD-INIT' template .agents README.md AGENTS.md` vacío
- [x] **F4.4 Ensayo de instalación: casos base, borrado, vacío y faltante (6 min).** RF-3, RF-4, RF-6
  - Hecho cuando:
    - [x] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/; touch "$tmp/.agents/.kit-version"` (sin `template/` dentro de la copia: se retira o se ensaya aparte); E21 con marcador: OK (pendiente, cuenta 1)
    - [x] Borrar la línea del marcador (simula `/sdd-init`): cuenta 0, OK
    - [x] Insertar el literal fuera del marcador: FALLA; duplicar el marcador: FALLA; borrar `AGENTS.md`: FALLA; `AGENTS.md` vacío: OK
    - [x] CRLF y espacios finales tras el comentario: OK
- [x] **F4.5 Ensayo: variantes y posición (6 min).** RF-6
  - Hecho cuando:
    - [x] En la copia de F4.4: espacio inicial, BOM antes del título, BOM antes del comentario y `\r` suelto: FALLA cada uno
    - [x] Segundo comentario idéntico en otra posición (marcador válido más una copia abajo, cuenta 2): FALLA
    - [x] Marcador idéntico en otra posición con cuenta 1 (no es la primera línea no vacía tras el título): FALLA
- [x] **F4.6 Ensayo con template/ presente (3 min).** RF-6
  - Hecho cuando:
    - [x] En la copia, `cp -R template "$tmp/template"`; con ambos iguales, E21 OK; editar el comentario de `$tmp/template/AGENTS.md`: FALLA por desincronía
    - [x] Sin esa carpeta, el mismo cambio no se detecta (declarado, no es fallo)
    - [x] La copia temporal se borra al terminar (`rm -rf "$tmp"`, solo esa ruta)
- [x] **F4.7 Conteos y réplicas (3 min).** RF-6a
  - Hecho cuando:
    - [x] `grep -c '^| E[0-9]' .agents/validar-estructura.md` = 21
    - [x] `grep -rn 'E1 a E20\|E1–E20' .agents/validar-estructura.md .agents/commands/sdd-estructura.md` vacío
    - [x] `grep -c SDD-INIT-PENDIENTE .agents/commands/sdd-estructura.md` = 0
    - [x] `diff -rq template/.agents .agents` muestra solo `napkin.md` y `skills/caveman`

## F5 — Líneas de grupo B en `05_avance.md` (estimado: 5 min)
- [x] **F5.1 Dejar las líneas de Changelog 0.6.0 y la nota de merge en `05_avance.md` (5 min; ejecuta la sesión principal, el implementer solo devuelve el texto).** RF-8
  - Hecho cuando:
    - [x] Las tres viñetas del plan §5 están copiadas en `.docs/002_specs/0010_FalsoPositivoInitPendiente/05_avance.md` (única edición permitida a este paso; este paso lo pide la sesión principal, que lleva ese archivo)
    - [x] Nota de merge: orden 0008, 0010, 0009; el bump de `VersionAgentKitSdd` a 0.6.0, el Changelog y el manifiesto los hace la F6 de 0009
    - [x] `git status --short -- VersionAgentKitSdd ChangelogAgentKitSdd.md manifiestos` vacío

## ADR — Registrar la decisión 7 (estimado: 10 min)
Se hace al cerrar, tras la fusión de 0008. El ADR no forma parte de `template/` ni entra al manifiesto.
- [x] **ADR.1 Comprobar el número libre (2 min).** —
  - Hecho cuando:
    - [x] `ls .docs/004_adr` ejecutado en este momento y, además, la rama remota descargada contada con `git ls-tree --name-only @{u} .docs/004_adr/` (o `origin/<rama>` si no hay upstream); el siguiente número libre por valor numérico es 0020 si 0008 ya creó 0019. Si 0019 no existe aún, parar y consultar a la sesión principal antes de usar 0020
- [x] **ADR.2 Crear `.docs/004_adr/0020_<nombre>.md` con la skill `.agents/skills/adr/SKILL.md` (6 min).** —
  - Hecho cuando:
    - [x] Forma de `0018_*.md`; contenido: marcador sin literal fuera del comentario, regla del cero, paso 1 como fuente única, cadena copiada con E21 como control cruzado y excepción explícita al principio 3; alternativas descartadas de la decisión 7 del plan (archivo de estado, grep laxo, quitar la regla); Origen `0010_FalsoPositivoInitPendiente`
    - [x] Lint OK (`file -i`, sin BOM, rutas existentes) en el ADR
    - [x] El ADR no está en `template/` ni se lista en `manifiestos/`
- [x] **ADR.3 Fila del índice en `.docs/004_adr/README.md` (2 min).** —
  - Hecho cuando:
    - [x] Releído; fila 0020 al final de la tabla, mismo formato que 0016 a 0018 (0008 puede haber añadido la 0019)
    - [x] El enlace de la fila apunta a un archivo existente; Lint OK
    - [x] El ADR queda enlazado desde `03_plan.md` (paso 5 de la skill adr) con una referencia en `03_plan.md` a `0020_<nombre>.md`
    - [x] `template/.docs/004_adr/README.md` (índice vacío) sin cambios
