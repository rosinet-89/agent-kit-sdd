# 04 — Tareas: Deudas abiertas del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Alcance de este archivo: F0 a F6. Cada paso parte del archivo releído. Justo antes de editarlo se guarda una copia fuera del repo. Al cerrar, el cambio se compara con esa copia y con `git diff HEAD -- <archivo>`. El commit, el push, el pull, el fetch, el sync, los ADR, las deudas `005` a `008` y su índice, la constitución, los cuatro agentes y las carpetas `001` a `006` quedan fuera de estas tareas.

## F0 — Preparación (estimado: 5 min)
- [x] **F0.1 Confirmar el punto de partida.** —
  - Hecho cuando:
    - [x] `VERSION`, `CHANGELOG.md` y `LICENSE` siguen en la raíz con el nombre viejo. `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `LicenseAgentKitSdd` todavía no existen
    - [x] `VERSION` contiene `0.5.0` y su último byte no es un salto de línea. Ese hecho queda anotado para F3.1
    - [x] El punto 6 de `.docs/001_constitucion/constitution.md` ya nombra `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`. La constitución no se edita
    - [x] Las carpetas `001` a `006` no están editando `template/`. Ninguna otra spec en curso toca `template/`
    - [x] Este paso no edita producto ni docs. Cada paso posterior relee el archivo y parte de ese texto
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F1 — Frontmatter (estimado: 20 min)
- [x] **F1.1 Registrar el veredicto de Cursor, Gemini, Copilot y Kiro.** RF-1, RF-2, RF-3, RF-5
  - Hecho cuando:
    - [x] Está leída la documentación oficial vigente de Cursor, Gemini, Copilot y Kiro. En `template/.agents/skills/sdd/restricciones-de-rol.md` cada herramienta tiene fecha y fuente (URL o nombre del documento)
    - [x] `model`, `omitClaudeMd` y `tools` quedan, por campo y por herramienta, en soportado, ignorado o provoca error. Si la fuente no alcanza para una celda, esa celda queda vacía y dice qué falta. No se rellena con una suposición
    - [x] La columna `Garantizada` se lee junto con ese registro y no afirma una garantía en una herramienta donde el campo no la sostiene
    - [x] Las filas de Claude Code siguen como estaban. Si una clave no viaja igual que en Claude Code, el registro lo dice y el comportamiento ya cerrado de Claude Code permanece
    - [x] El frontmatter de `coordinator.md`, `planner.md`, `implementer.md` y `reviewer.md` sigue igual
    - [x] `file -i template/.agents/skills/sdd/restricciones-de-rol.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' template/.agents/skills/sdd/restricciones-de-rol.md` no lista el archivo
    - [x] Si el archivo tiene frontmatter, `name` es el nombre de su carpeta. Las rutas de este repo citadas en el archivo existen. En `template/` no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] El diff contra la copia previa y `git diff HEAD -- template/.agents/skills/sdd/restricciones-de-rol.md` muestra solo este registro y el ajuste de `Garantizada`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F1.2 Dejar escrito el resultado de Kiro en la tabla de enlaces.** RF-4
  - Hecho cuando:
    - [x] El resultado sale de la lectura de F1.1. `template/.agents/sync-agents.md` tiene una fila de enlace de Kiro, u otro formato descrito junto a la tabla, o «no aplica» con el porqué
    - [x] No hay una fila de enlace que la documentación no sostenga. Si la fuente no muestra el esquema de carpetas ni otro formato, el texto dice «no aplica» y el porqué, con la fecha y la fuente de esa lectura
    - [x] `file -i template/.agents/sync-agents.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' template/.agents/sync-agents.md` no lista el archivo
    - [x] Si el archivo tiene frontmatter, `name` es el nombre de su carpeta. Las rutas de este repo citadas en el archivo existen. En `template/` no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] El diff contra la copia previa y `git diff HEAD -- template/.agents/sync-agents.md` muestra solo el resultado de Kiro
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F2 — Sugerencia en /sdd-init (estimado: 8 min)
- [x] **F2.1 Hacer que `/sdd-init` muestre el deny y el allow de la pieza.** RF-6, RF-7, RF-8
  - Hecho cuando:
    - [x] `template/.agents/commands/sdd-init.md` manda leer y mostrar entero el deny y el allow de `template/.agents/skills/sdd/restricciones-de-rol.md` en la propuesta (paso 3) y también cuando el proyecto ya está personalizado
    - [x] Lo que se muestra trae los cinco patrones de Git (`commit`, `push`, `fetch`, `pull`, `sync`) y la advertencia de que el deny vale para toda la sesión y puede bloquear el Git de quien lo copie
    - [x] El allow que se muestra es el genérico de la pieza, para que quien instala lo llene con los comandos de su proyecto. No trae comandos de este kit ni de ningún proyecto
    - [x] El paso 4 no los escribe. El comando no los aplica, no los activa y no los copia a `.claude/`, a un settings versionado ni a ninguna configuración del repo. `sdd-init.md` no pega una segunda copia del YAML
    - [x] `file -i template/.agents/commands/sdd-init.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' template/.agents/commands/sdd-init.md` no lista el archivo
    - [x] El `name` del frontmatter sigue siendo el del comando. Las rutas de este repo citadas en el archivo existen. En `template/` no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] El diff contra la copia previa y `git diff HEAD -- template/.agents/commands/sdd-init.md` muestra solo esa sugerencia
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F3 — Renombre y citas vivas (estimado: 12 min)
- [x] **F3.1 Renombrar `VERSION` a `VersionAgentKitSdd`.** RF-9, RF-10, RF-11
  - Hecho cuando:
    - [x] `git mv VERSION VersionAgentKitSdd` es el único cambio de este paso. Sin commit, sin push, sin pull, sin fetch y sin sync
    - [x] El contenido sigue siendo `0.5.0`. El último byte sigue sin ser un salto de línea, igual que la copia tomada justo antes. No aparece `0.5.1`
    - [x] `file -i VersionAgentKitSdd` da utf-8. `grep -l $'^\xEF\xBB\xBF' VersionAgentKitSdd` no lista el archivo
    - [x] La copia previa y `git diff HEAD -- VersionAgentKitSdd` muestran el renombre con el mismo contenido
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.2 Renombrar `CHANGELOG.md` y actualizar la cita de su línea 21.** RF-9, RF-10, RF-11
  - Hecho cuando:
    - [x] `git mv CHANGELOG.md ChangelogAgentKitSdd.md`. En ese mismo archivo, la cita de la línea 21 pasa a `VersionAgentKitSdd`. Depende de F3.1
    - [x] El encabezado `## 0.5.0 — 2026-10-24` y su fecha siguen iguales. La viñeta de numeración y la viñeta de este feature quedan para F4.6
    - [x] `file -i ChangelogAgentKitSdd.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' ChangelogAgentKitSdd.md` no lista el archivo
    - [x] La copia previa y `git diff HEAD -- ChangelogAgentKitSdd.md` muestran el renombre y, de texto, solo la cita de la línea 21
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.3 Renombrar `LICENSE` a `LicenseAgentKitSdd`.** RF-9, RF-10, RF-11
  - Hecho cuando:
    - [x] `git mv LICENSE LicenseAgentKitSdd`. El texto de la licencia es el de la copia previa
    - [x] En `template/` no hay cita de `LICENSE` que este paso actualice. `.agents/skills/napkin/LICENSE` sigue con su ruta
    - [x] `file -i LicenseAgentKitSdd` da utf-8. `grep -l $'^\xEF\xBB\xBF' LicenseAgentKitSdd` no lista el archivo
    - [x] La copia previa y `git diff HEAD -- LicenseAgentKitSdd` muestran el renombre con el mismo texto
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.4 Actualizar las citas de nombres en `AGENTS.md`.** RF-9
  - Hecho cuando:
    - [x] La línea 111 nombra `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd` y `LicenseAgentKitSdd`. La línea 120 dice que cada versión actualiza `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`
    - [x] Las líneas 51 y 67 quedan para F4.3. El resto del archivo sigue igual
    - [x] `file -i AGENTS.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' AGENTS.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- AGENTS.md` muestran solo las líneas 111 y 120
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.5 Actualizar las citas de nombres en `ACTUALIZAR.md`.** RF-9
  - Hecho cuando:
    - [x] Las líneas 51, 57, 184 y 313 nombran `VersionAgentKitSdd` o `ChangelogAgentKitSdd.md`, según el archivo que ya citaban
    - [x] El marcador `<VERSION>` de las líneas 53 y 64 sigue igual. La ruta `.agents/skills/napkin/LICENSE` de la línea 287 sigue igual. Los grupos y el aviso de numeración siguen igual
    - [x] `file -i ACTUALIZAR.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' ACTUALIZAR.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- ACTUALIZAR.md` muestran solo las líneas 51, 57, 184 y 313
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.6 Actualizar las citas de nombres en `INSTALAR.md`.** RF-9
  - Hecho cuando:
    - [x] Las líneas 16, 54, 56, 73 y 97 nombran `VersionAgentKitSdd` o `ChangelogAgentKitSdd.md`, según el archivo que ya citaban
    - [x] `file -i INSTALAR.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' INSTALAR.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- INSTALAR.md` muestran solo esas líneas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.7 Actualizar las citas de nombres en `README.md` de la raíz.** RF-9
  - Hecho cuando:
    - [x] La línea 68 nombra `ChangelogAgentKitSdd.md` y `VersionAgentKitSdd`. La línea 73 nombra `LicenseAgentKitSdd`. El archivo `README.md` no se renombra
    - [x] `file -i README.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' README.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- README.md` muestran solo las líneas 68 y 73
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F4 — Numeración (estimado: 16 min)
- [x] **F4.1 Escribir los cuatro bordes en el índice del producto.** RF-9, RF-12, RF-13, RF-14, RF-15, RF-16
  - Hecho cuando:
    - [x] `template/.docs/002_specs/README.md` › « Numeración única » dice: un prefijo numérico vale su entero (`17` = `017` = `0017` = `00000017`); los ceros de relleno y el ancho escrito no son fallo y el nombre no se reescribe; dos ítems de la misma carpeta con el mismo entero son duplicado y es `FALLA`, y esa igualdad no es `AVISO`
    - [x] Un nombre que no empieza por dígitos y `_` (`abc_Mal`) no entra en el consecutivo ni en el ancho y no se renombra. Un prefijo relleno cuenta para el entero y el duplicado, y no fija el ancho de la carpeta con su cantidad de dígitos. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014
    - [x] Un ítem nuevo del clon se escribe con 4 dígitos. Los ya creados no se renumeran (`001` sigue `001`). `015` y `0015` son el mismo entero. En un proyecto, un ítem nuevo conserva el ancho de su carpeta
    - [x] La definición de clon de esa sección (hoy línea 56) nombra `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`. La frase mínima de un proyecto sigue siendo la única repetición de esa regla
    - [x] Los ADR 014 y 015 no se editan. El grupo C de un proyecto ya instalado no se lee ni se reemplaza. Este paso edita el índice que viaja en `template/`
    - [x] `file -i template/.docs/002_specs/README.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' template/.docs/002_specs/README.md` no lista el archivo
    - [x] Si el archivo tiene frontmatter, `name` es el nombre de su carpeta. Las rutas de este repo citadas en el archivo existen. En `template/` no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] El diff contra la copia previa y `git diff HEAD -- template/.docs/002_specs/README.md` muestra solo « Numeración única », incluida la definición de clon
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.2 Actualizar E12 y el ejemplo de BOM de E15.** RF-9, RF-12, RF-13, RF-14, RF-15
  - Hecho cuando:
    - [x] La fila E12 de `template/.agents/validar-estructura.md` dice el resultado de la decisión 4 y remite a « Numeración única » del índice del producto. Depende de F4.1
    - [x] En el clon, los 4 dígitos valen para el ítem nuevo: el ancho ya escrito no pasa a `FALLA` y una mezcla válida no es `AVISO`. El `AVISO` de mezcla válida sigue siendo solo de proyecto
    - [x] El ejemplo de BOM de E15 (hoy línea 48) nombra `ChangelogAgentKitSdd.md`. La fila E20 y el marcador `manifiestos/<VERSION>.md` siguen como están
    - [x] `file -i template/.agents/validar-estructura.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' template/.agents/validar-estructura.md` no lista el archivo
    - [x] Si el archivo tiene frontmatter, `name` es el nombre de su carpeta. Las rutas de este repo citadas en el archivo existen. En `template/` no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] El diff contra la copia previa y `git diff HEAD -- template/.agents/validar-estructura.md` muestra solo E12 y el ejemplo de E15
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.3 Remitir la numeración del clon en `AGENTS.md`.** RF-15
  - Hecho cuando:
    - [x] Las líneas 51 y 67 remiten a la declaración del índice del producto y dejan de decir que el clon sigue en 3 y que no sube a 4. No copian la regla completa. Los ítems ya creados no se renumeran en este paso
    - [x] Las líneas 111 y 120 siguen como quedaron en F3.4
    - [x] `file -i AGENTS.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' AGENTS.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- AGENTS.md` añaden solo el cambio de las líneas 51 y 67 sobre el diff ya cerrado en F3.4
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.4 Remitir la numeración del clon en el índice de planes de esta copia.** RF-15
  - Hecho cuando:
    - [x] En `.docs/002_specs/README.md`, la frase « En este clon no se sube a 4 » pasa a remitir a la declaración del índice del producto. No se copia aquí la regla completa
    - [x] La fila `007` del índice de planes sigue igual
    - [x] `file -i .docs/002_specs/README.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' .docs/002_specs/README.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia tomada justo antes muestra solo esa frase. `git diff HEAD -- .docs/002_specs/README.md` también trae los cambios de las filas 004, 005 y 007 que ya estaban en el árbol; este paso no lo revierte ni reescribe la fila 007
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.5 Remitir la numeración del clon en el índice de ADR de esta copia.** RF-15
  - Hecho cuando:
    - [x] En `.docs/004_adr/README.md`, la frase « en este clon, 3 dígitos; no subirlos a 4 » remite a la declaración del índice del producto. No se copia la regla completa
    - [x] Ningún ADR se edita
    - [x] `file -i .docs/004_adr/README.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' .docs/004_adr/README.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- .docs/004_adr/README.md` muestran solo esa frase
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.6 Corregir la viñeta del clon y anotar este feature en el changelog ya renombrado.** RF-13, RF-15
  - Hecho cuando:
    - [x] En `ChangelogAgentKitSdd.md`, la viñeta que dice que el clon sigue en 3 pasa a remitir a la declaración del índice del producto, sin copiar la regla completa
    - [x] Hay una viñeta de este feature dentro de `## 0.5.0 — 2026-10-24`. Ese encabezado y su fecha siguen iguales. La cita de la línea 21 sigue en `VersionAgentKitSdd`. No aparece un encabezado `0.5.1`
    - [x] `file -i ChangelogAgentKitSdd.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' ChangelogAgentKitSdd.md` no lista el archivo. Las rutas de este repo citadas en el archivo existen
    - [x] La copia previa y `git diff HEAD -- ChangelogAgentKitSdd.md` añaden solo esas viñetas sobre el diff ya cerrado en F3.2
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F5 — Manifiesto (estimado: 6 min)
- [x] **F5.1 Regenerar `manifiestos/0.5.0.md` y repetir E20.** —
  - Hecho cuando:
    - [x] `manifiestos/0.5.0.md` se regenera una vez con el algoritmo vigente de `ACTUALIZAR.md`, cuando `template/` ya no va a cambiar. No se edita a mano y no se renombra. La fila E20 y el marcador `manifiestos/<VERSION>.md` siguen como están
    - [x] E20, con el chequeo vigente, queda en verde
    - [x] `file -i manifiestos/0.5.0.md` da utf-8. `grep -l $'^\xEF\xBB\xBF' manifiestos/0.5.0.md` no lista el archivo. La comprobación de rutas citadas no aplica a `manifiestos/`
    - [x] La copia previa y `git diff HEAD -- manifiestos/0.5.0.md` muestran el manifiesto regenerado
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F6 — Refresco y validación (estimado: 18 min)
- [x] **F6.1 Refrescar la copia local de `.agents/`.** —
  - Hecho cuando:
    - [x] El refresco es el de `INSTALAR.md` › Modo mantenimiento › « Refrescar la copia local ». No se redefine
    - [x] `napkin.md` sigue. Lo que ya vivía solo en `.agents/` antes de este feature sigue. `caveman` permanece en la copia local y no entra a `template/`
    - [x] Si un chequeo posterior corrige un archivo de `template/`, se repiten este refresco y F5.1, y se vuelve a correr el chequeo
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.2 Chequeo 1: ensayo de instalación.** —
  - Hecho cuando:
    - [x] En un directorio de `mktemp -d`, y nunca en este repo ni en un proyecto real, `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia todo `template/`
    - [x] Una segunda pasada del mismo comando sobre ese temporal no pisa lo ya copiado
    - [x] Evidencia de datos: el temporal contiene el árbol de `template/` y la segunda pasada lo deja igual. Temporal `/tmp/tmp.uDH72KcPpR` (borrado): 42 archivos en `template/` y 42 en el temporal; la segunda pasada no pisó `.agents/agents/coordinator.md`
    - [x] Verificación manual UI: se revisa el temporal, no una pantalla. El kit no tiene UI
- [x] **F6.3 Chequeo 2: los tres nombres viejos ya no son el nombre de esos archivos.** RF-9, RF-11
  - Hecho cuando:
    - [x] El `grep` del conjunto vivo (`template/`, `AGENTS.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, el índice de numeración del producto y el ejemplo de E15) ya no usa `VERSION`, `CHANGELOG.md` ni `LICENSE` como nombre de esos tres archivos
    - [x] Siguen el marcador `<VERSION>` (líneas 53 y 64 de `ACTUALIZAR.md`), `manifiestos/<VERSION>.md` y `.agents/skills/napkin/LICENSE`
    - [x] La lectura es de este clon. No se crea un proyecto ni se copia el kit encima de este repo
    - [x] Evidencia de datos: 2026-10-25. Salida de `grep -R -n -E -e 'VERSION' -e 'CHANGELOG\.md' -e 'LICENSE' template AGENTS.md ACTUALIZAR.md INSTALAR.md README.md`. El índice `template/.docs/002_specs/README.md` y el ejemplo de E15 entran en `template/`. El índice no tuvo coincidencia.

      ```
      ACTUALIZAR.md:53:   versión y la extensión `.md` (`manifiestos/<VERSION>.md`). Si no existe, informa y no
      ACTUALIZAR.md:64:`manifiestos/<VERSION>.md`. Si las huellas no coinciden, no modifiques nada.
      ACTUALIZAR.md:287:`.agents/preferencias.ejemplo.md` y `.agents/skills/napkin/LICENSE` son del grupo A. El archivo
      template/.agents/validar-estructura.md:37:| E20 | kit | Solo en el repo del kit: recalcula la huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` y la compara con manifiestos/<VERSION>.md. Falla por diferencia, por archivo sin fila o por fila sin archivo. En un proyecto instalado no aplica: ahí no hay manifiestos | Algoritmo de `ACTUALIZAR.md` |
      ```

      Hallazgos que permanecen (excepciones): `<VERSION>` en `ACTUALIZAR.md:53` y `ACTUALIZAR.md:64`; `manifiestos/<VERSION>.md` en esas dos líneas y en `template/.agents/validar-estructura.md:37`; `.agents/skills/napkin/LICENSE` en `ACTUALIZAR.md:287` y el archivo `template/.agents/skills/napkin/LICENSE`. `AGENTS.md`, `INSTALAR.md` y `README.md` no tuvieron coincidencia. `CHANGELOG` exacto no apareció.
    - [x] Verificación manual UI: se revisa esa salida, no una pantalla. El kit no tiene UI
- [x] **F6.4 Chequeo 3: la versión sigue en 0.5.0 y el archivo de versión no gana un LF.** RF-9, RF-11
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` es `0.5.0`. Su último byte no es un salto de línea
    - [x] `ChangelogAgentKitSdd.md` no tiene encabezado `0.5.1`. El encabezado `## 0.5.0 — 2026-10-24` sigue con esa fecha
    - [x] Evidencia de datos: 2026-10-25. Contenido de `VersionAgentKitSdd`: `0.5.0` (`wc -c` = 5; `xxd` = `30 2e 35 2e 30`). Último byte: `tail -c 1 VersionAgentKitSdd | od -An -tx1` = `30`, no `0a`. `ChangelogAgentKitSdd.md` no contiene `0.5.1`. El encabezado de la línea 3 sigue `## 0.5.0 — 2026-10-24`. Ninguno de los dos archivos se editó
    - [x] Verificación manual UI: se revisan esos dos archivos, no una pantalla. El kit no tiene UI
- [x] **F6.5 Chequeo 4: registro, columna Garantizada, Claude Code y Kiro.** RF-1, RF-2, RF-3, RF-4, RF-5
  - Hecho cuando:
    - [x] Cursor, Gemini, Copilot y Kiro tienen fecha y fuente. Cada uno de `model`, `omitClaudeMd` y `tools` tiene soportado, ignorado o provoca error, o la celda vacía dice qué falta y no cuenta como veredicto
    - [x] La columna `Garantizada` no cubre una herramienta donde el campo no la sostiene. Las filas de Claude Code siguen. Si una clave no viaja igual, el registro lo dice y el frontmatter de los cuatro agentes sigue igual
    - [x] Kiro tiene uno de los tres resultados de RF-4: fila de enlace, otro formato descrito junto a la tabla, o «no aplica» con el porqué
    - [x] Evidencia de datos: 2026-10-25. Puntero: `template/.agents/skills/sdd/restricciones-de-rol.md` (fuentes 26-33, campos 39-41, Garantizada 58-65) y `template/.agents/sync-agents.md` «### Kiro» (75-86). La tabla corta va en la respuesta del paso.
    - [x] Verificación manual UI: se revisan esos dos archivos, no una pantalla. El kit no tiene UI
- [x] **F6.6 Chequeo 5: el texto de `/sdd-init` muestra y no aplica.** RF-6, RF-7, RF-8
  - Hecho cuando:
    - [x] `template/.agents/commands/sdd-init.md` manda mostrar los cinco patrones (`commit`, `push`, `fetch`, `pull`, `sync`), la advertencia de toda la sesión y el allow genérico, en la propuesta y cuando el proyecto ya está personalizado
    - [x] El texto prohíbe aplicarlos, activarlos y copiarlos a una configuración versionada. No trae comandos de un proyecto ni una segunda copia del YAML
    - [x] Evidencia de datos: el texto del comando en los dos momentos (propuesta y proyecto ya personalizado)
    - [x] Verificación manual UI: se revisa ese comando, no una pantalla. El kit no tiene UI
- [x] **F6.7 Chequeo 6: casos de numeración en carpetas simuladas.** RF-12, RF-13, RF-14, RF-15, RF-16
  - Hecho cuando:
    - [x] Las carpetas simuladas están en `mktemp -d`, nunca en este repo ni en un proyecto real. De cada caso quedan el listado antes, el listado después y el número que se propone, aplicando la fila E12 y « Numeración única » ya escritas
    - [x] `17`, `017`, `0017` y `00000017` valen 17, no son fallo y no se reescriben
    - [x] Dos ítems con el mismo entero son `FALLA` y no hay `AVISO`. `015` y `0015` son duplicado
    - [x] `1000` junto a un ancho 3 sigue en el desbordamiento del ADR 014. `abc_Mal` no entra en el consecutivo ni en el ancho y no se renombra
    - [x] Una mezcla válida de proyecto es `AVISO`. En el clon, `001` junto a `0008` no es `AVISO` ni `FALLA`; el siguiente ítem se propone con 4 dígitos y `001` no se renombra
    - [x] Evidencia de datos: los listados antes y después y el número propuesto de cada caso
    - [x] Verificación manual UI: se revisan esas carpetas simuladas, no una pantalla. El kit no tiene UI
- [x] **F6.8 Chequeo 7: E20, encoding de lo tocado y E19.** —
  - Hecho cuando:
    - [x] E20 queda en verde con `manifiestos/0.5.0.md` regenerado. La fila E20 no se reescribe
    - [x] `file -i` da utf-8 y `grep -l $'^\xEF\xBB\xBF'` no lista ninguno de estos archivos: `template/.agents/skills/sdd/restricciones-de-rol.md`, `template/.agents/sync-agents.md`, `template/.agents/commands/sdd-init.md`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `LicenseAgentKitSdd`, `template/.docs/002_specs/README.md`, `template/.agents/validar-estructura.md`, `AGENTS.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, `.docs/002_specs/README.md`, `.docs/004_adr/README.md`, `manifiestos/0.5.0.md`
    - [x] Donde el archivo tiene frontmatter, `name` es el nombre de su carpeta; en `sdd-init.md`, `name` sigue siendo el del comando. Las rutas de este repo citadas existen, salvo en `manifiestos/`. En cada archivo bajo `template/` no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [x] E19 solo difiere en `napkin.md` más lo que ya vivía solo en `.agents/` antes de este feature. `caveman` es local y no entra al producto
    - [x] Si este chequeo corrige un archivo de `template/`, se repiten F6.1 y F5.1 y se vuelven a correr E20 y E19. Este chequeo no corrigió ninguno: no se repiten
    - [x] Evidencia de datos: 2026-10-25. `manifiestos/0.5.0.md` existe y tiene 42 filas de huella. La fila E20 no se reescribió. `file -i`: utf-8 en los 13 markdown; `charset=us-ascii` en `VersionAgentKitSdd` y `LicenseAgentKitSdd` (cuenta como UTF-8). `grep` de BOM vacío. Frontmatter solo en `sdd-init.md`, sin clave `name`. Marcadores en `template/`: solo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. E19:

      ```
      Los archivos template/.agents/napkin.md y .agents/napkin.md son distintos
      Sólo en .agents/skills: caveman
      ```

      `caveman` no está en `template/`. Ningún archivo de la lista se editó.
    - [x] Verificación manual UI: se revisan esos resultados, no una pantalla. El kit no tiene UI

Total: 85 min.
