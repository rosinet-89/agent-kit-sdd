# 01 — Spec: Deudas abiertas del kit
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo
Cuatro deudas abiertas quedan en esta sola spec, por decisión del 2026-10-25 («mete las 4 deudas en una misma spec»). Cada alta proponía una spec propia; esa alternativa queda descartada.

| Deuda | Prioridad | Qué dejó pendiente |
|---|---|---|
| `005_FrontmatterAgentesPorHerramienta` | Media | El veredicto de `model`, `omitClaudeMd` y `tools` existe para Claude Code. En Cursor, Gemini y Copilot figura como no verificado. Kiro no tiene fila en la tabla de enlaces. |
| `006_SddInitSugiereAllowDeny` | Media | Hay dos fragmentos opcionales ya definidos y ningún comando los sugiere. `/sdd-init` no los aplica. |
| `007_NombresQueChocanConHerramientas` | Alta | Hay nombres que otra herramienta puede reescribir. No se ha renombrado nada. Solo tres pasan a nombre nuevo. El resto se sigue manejando como hasta ahora. |
| `008_LimitacionesDeLaNumeracion` | Baja | La revalidación de la `005` dejó cuatro bordes de numeración sin comportamiento explícito. El ADR 014 conserva la regla. |

La `004_ModeloYPermisosAgentes` cerró el frontmatter en Claude Code y dejó fuera la verificación de las otras herramientas y la sugerencia de `/sdd-init`. La `005_ConvivenciaNumeracion` cerró la convivencia de anchos. El ADR 014 conserva esa regla. El ADR 015 conserva la fuente única en el grupo C: la actualización no lee ni reemplaza ese grupo, y la frase mínima es la única repetición. Esta spec no reemplaza esos ADR.

El objetivo es dejar comprobable, para cada deuda, qué queda hecho y qué sigue sin decidir. Solo el qué y el porqué.

## Usuarios y roles
- **Mantenedor del kit**: registra el veredicto por herramienta, cuida el corte de nombres y deja explícitos los bordes de numeración.
- **Quien instala el kit**: ejecuta `/sdd-init` y decide si copia los fragmentos. El kit no los aplica.
- **Quien usa los agentes en Cursor, Gemini, Copilot o Kiro**: necesita saber si `model`, `omitClaudeMd` y `tools` se honran, se ignoran o provocan error.

## Historias de usuario
- HU-1. Como mantenedor, quiero el veredicto de `model`, `omitClaudeMd` y `tools` en Cursor, Gemini, Copilot y Kiro, con fecha y fuente por herramienta, para no afirmar una garantía que esa herramienta no sostiene.
- HU-2. Como mantenedor, quiero que Kiro tenga fila en la tabla de enlaces, o que quede escrito que usa otro formato o que no aplica, para no dejar una herramienta sin formato documentado.
- HU-3. Como quien instala el kit, quiero que `/sdd-init` me muestre el deny de Git y el allow genérico, sin aplicarlos, para decidir si los copio sabiendo que el deny puede bloquear mi Git.
- HU-4. Como mantenedor, quiero nombre nuevo solo para el riesgo alto y el medio, y conservar el bajo, para que un plugin de release no reescriba solo un archivo del kit.
- HU-5. Como mantenedor, quiero los cuatro bordes de la numeración escritos de forma explícita, para que el chequeo E12 no dependa de un comportamiento que hoy no está dicho.

## Definiciones
- **Soportado**: la herramienta documenta el campo y lo aplica. **Ignorado**: lo tolera y no lo aplica. **Provoca error**: impide cargar el agente o falla al leer ese campo. Hace falta fecha y fuente por herramienta. Sin esa cita no hay veredicto.
- **Registro de campos**: el lugar único donde consta, por campo y por herramienta, si está soportado, ignorado o provoca error.
- **Columna de qué queda garantizado**: la columna de la tabla de restricciones que dice si una restricción la sostiene la herramienta o solo el texto. Se lee junto con el registro. No afirma una garantía en una herramienta donde el campo no la sostiene.
- **Fragmento deny**: el fragmento opcional ya definido, con cinco patrones de Git (`commit`, `push`, `fetch`, `pull`, `sync`). Vale para toda la sesión.
- **Fragmento allow**: el fragmento opcional ya definido, genérico. Lo llena quien instala con los comandos de su proyecto.
- **Plugin de release**: un programa que crea o reescribe solo un nombre fijo (`VERSION`, `CHANGELOG.md` y el mismo archivo con otra capitalización). En un disco que no distingue mayúsculas, `version` y `VERSION` son el mismo archivo, igual que `Changelog.md` y `CHANGELOG.md`, y `LICENSE` con `LICENCE`.
- **Carpeta de agentes compartida**: `.agents/`.
- **Carpetas de herramienta**: `.claude/`, `.cursor/`, `.github/` y `.gemini/`. El riesgo alto de estas carpetas aplica cuando el enlace se vuelve carpeta real.
- **Marcador de versión de un proyecto**: `.agents/.kit-version`. Un proyecto no usa `VERSION` suelto.
- **Grupo C**: los índices que la actualización no lee ni reemplaza. La fuente única de la numeración vive ahí (ADR 015).
- **Frase mínima**: «un ítem nuevo conserva el ancho de su carpeta». Es la única repetición permitida de esa regla.
- **Mezcla válida**: una carpeta con ítems válidos de dos o más anchos, sin duplicado por valor. Incluye el desbordamiento que ya cerró el ADR 014 (un valor que necesita más dígitos que el ancho, sin ceros de relleno).
- **AVISO**: el estado de E12 que informa un hecho y no es defecto. En la regla ya cerrada, la mezcla válida de un proyecto es AVISO. El clon no entra en esa frase.
- **Ítem inválido**: un prefijo que E12 ya trata como ancho inválido. El ancho de la carpeta cuando ese ítem está presente no está definido: es un borde de esta spec, no una regla ya cerrada.
- **Índice que viaja en el producto**: el índice de numeración que recibe un proyecto. Hoy no declara el ancho del clon.
- **Índice del clon**: el índice de numeración de esta copia del kit. Hoy es el que declara el ancho del clon. En el clon, E12 compara contra ese índice.

## Requisitos funcionales (EARS)

### Frontmatter en Cursor, Gemini, Copilot y Kiro (deuda 005)
- RF-1: CUANDO se verifique el frontmatter fuera de Claude Code, EL SISTEMA registra, por separado, Cursor, Gemini, Copilot y Kiro, cada una con fecha y fuente. Sin fecha y fuente, esa herramienta no tiene veredicto.
- RF-2: CUANDO exista el registro de RF-1, EL SISTEMA anota `model`, `omitClaudeMd` y `tools` en cada una de esas cuatro herramientas como soportado, ignorado o provoca error. El veredicto es por campo y por herramienta.
- RF-3: CUANDO un campo quede anotado en RF-2, EL SISTEMA actualiza el registro de campos y la columna de qué queda garantizado con ese resultado, de modo que la columna no afirme una garantía en una herramienta donde el campo no la sostiene.
- RF-4: CUANDO se registre Kiro, EL SISTEMA deja escrito uno de estos tres resultados: Kiro tiene fila en la tabla de enlaces; Kiro usa otro formato, y ese formato queda descrito; o Kiro no aplica, y queda dicho por qué. Hoy no tiene fila.
- RF-5: CUANDO el veredicto de otra herramienta no contradiga lo ya cerrado en Claude Code, EL SISTEMA conserva ese comportamiento. SI una clave no viaja igual que en Claude Code, EL SISTEMA lo dice en el registro y conserva el comportamiento ya cerrado de Claude Code.

### Sugerencia de allow y deny en `/sdd-init` (deuda 006)
- RF-6: CUANDO se ejecuta `/sdd-init`, EL SISTEMA sugiere los dos fragmentos opcionales, el deny y el allow. La sugerencia no los aplica, no los activa y no los copia a una configuración versionada.
- RF-7: CUANDO `/sdd-init` sugiere el deny, EL SISTEMA muestra los cinco patrones de Git del fragmento ya definido y la advertencia de que vale para toda la sesión y puede bloquear el Git de quien lo copie.
- RF-8: CUANDO `/sdd-init` sugiere el allow, EL SISTEMA lo deja genérico, para que quien instala lo llene con los comandos de su proyecto. No trae comandos de ningún proyecto concreto.

### Nombres que otras herramientas pueden reescribir (deuda 007)
- RF-9: EL SISTEMA renombra solo tres archivos, y todavía no los mueve: `VERSION` pasa a `VersionAgentKitSdd`; `CHANGELOG.md` pasa a `ChangelogAgentKitSdd.md`; `LICENSE` pasa a `LicenseAgentKitSdd`. En el mismo cambio, cada cita del producto que nombra esos archivos pasa al nombre nuevo. Hoy esas citas son la definición de clon en la numeración (`VERSION` y `CHANGELOG.md`) y el ejemplo de BOM que nombra `CHANGELOG.md`. `LICENSE` no tiene cita en el producto. El marcador `<VERSION>` del manifiesto es el número de versión, no el archivo, y no cambia. El resto conserva el nombre y se sigue manejando como hasta ahora (decisión del usuario, 2026-10-25 21:08): `AGENTS.md`, `.agents/`, `.claude/`, `.cursor/`, `.github/`, `.gemini/`, `CLAUDE.md`, `MEMORY.md`, `.gitignore`, `README.md` de la raíz del clon, `template/`, `ACTUALIZAR.md`, `INSTALAR.md`, `manifiestos/`, `.docs/`, `SKILL.md`, `PREFERENCIAS.md` y `napkin.md`. El punto 6 de la constitución ya nombra `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` (2026-10-25 21:28). Los archivos todavía no se movieron.
- RF-10: CUANDO el nombre nuevo de un ítem de riesgo alto o medio aún no está aprobado, EL SISTEMA conserva el nombre actual de ese ítem. Sin aprobación no hay renombre.
- RF-11: CUANDO se apruebe un nombre nuevo, EL SISTEMA usa un nombre que un plugin de release no escribe solo. Otra capitalización, o el par `LICENSE`/`LICENCE`, no cuenta como nombre distinto de uno que ese plugin escribe. El marcador de versión de un proyecto permanece en `.agents/.kit-version`.

### Cuatro bordes de numeración (deuda 008)
- RF-12: CUANDO E12 lea un prefijo numérico, EL SISTEMA lo cuenta como entero. `17`, `017`, `0017` y `00000017` valen 17. Los ceros de relleno y el ancho con el que está escrito no cambian el valor y no son fallo. El nombre no se reescribe. SI en la misma carpeta hay dos ítems con el mismo entero, EL SISTEMA lo trata como duplicado, y ese duplicado es fallo. No emite aviso por esa igualdad: sirve para el conteo y para evitar el duplicado. Un prefijo sin ceros de relleno que usa los dígitos que su valor necesita (`1000` en una carpeta de ancho 3) sigue en el ADR 014 y no es este borde.
- RF-13: CUANDO el clon tenga una mezcla válida, EL SISTEMA no emite AVISO. El texto del chequeo dice que el aviso es solo de proyecto. La mezcla válida de un proyecto sigue siendo AVISO, como ya cerró el ADR 014.
- RF-14: CUANDO una carpeta mezcla prefijos numéricos y un nombre que no es número, EL SISTEMA calcula el consecutivo y el ancho solo con los numéricos. `abc_Mal` no entra en ese cálculo y no se renombra.
- RF-15: EL SISTEMA declara en el índice de numeración del producto que un ítem nuevo del clon se escribe con 4 dígitos. Los ítems ya creados no se renumeran: `001` sigue siendo `001`. El valor sigue el entero de RF-12, así que `015` y `0015` son el mismo número y, si coexisten, son duplicado. El índice de un proyecto ya instalado no se reescribe. En un proyecto, un ítem nuevo sigue conservando el ancho de su carpeta. E12, en el clon, usa la declaración de 4 dígitos para el ítem nuevo.
- RF-16: EL SISTEMA conserva la regla del ADR 014, la fuente única del ADR 015, los ítems ya numerados y el grupo C. La actualización no lee ni reemplaza el grupo C. La frase mínima sigue siendo la única repetición. Los ADR 014 y 015 no se reemplazan.

## Requisitos no funcionales
- RNF-1: Una sola fuente por regla. El veredicto por herramienta vive en el registro de campos. La numeración no abre una segunda fuente: sigue en el grupo C (ADR 015).
- RNF-2: El allow es genérico. No lleva comandos, stack ni nombres de ningún proyecto.
- RNF-3: No hay veredicto de campo sin fecha y fuente de esa herramienta (RF-1, RF-2).
- RNF-4: Nada se aplica ni se renombra por adelantado. El deny sigue siendo sugerencia (RF-6). El renombre espera el nombre aprobado (RF-10). Los ítems existentes no se renumeran (RF-16).

## Casos límite
- Kiro usa otro formato, o no tiene agentes: RF-4. No se inventa una fila de enlace que no corresponda.
- El mismo campo queda soportado en una herramienta y provoca error en otra: el registro lo dice por herramienta (RF-2) y la columna de garantía no extiende a la herramienta donde falla (RF-3).
- Una clave no viaja igual que en Claude Code: el registro lo dice y el comportamiento cerrado de Claude Code permanece (RF-5).
- Quien instala no copia el deny: la restricción de Git sigue sin estar garantizada, como ya está cerrada. Esta spec no la convierte en garantía.
- Quien copia el deny bloquea el Git de su sesión: la sugerencia ya trae esa advertencia (RF-7). El comando no lo aplica (RF-6).
- Un proyecto ya personalizado: la sugerencia vive en `/sdd-init`. Esta spec no añade otro comando que la repita.
- El nombre nuevo aún no está aprobado: ese ítem conserva su nombre (RF-10), aunque el corte ya lo marque como alto o medio.
- `ACTUALIZAR.md` e `INSTALAR.md` coincidirían con otra capitalización en un disco que no distingue mayúsculas: siguen en riesgo bajo y conservan el nombre (RF-9).
- Renombrar el `VERSION` del clon no cambia el marcador `.agents/.kit-version` de un proyecto (RF-11).
- Un `README.md` que no está en la raíz del clon no entra en el corte (fuera de alcance).
- El desbordamiento sin ceros de relleno (`1000` en una carpeta de ancho 3) sigue siendo mezcla válida del ADR 014. No es el borde de RF-12.
- La mezcla válida de un proyecto sigue siendo AVISO, como ya cerró el ADR 014. RF-13 solo cubre el clon.
- Un ítem inválido junto a ítems válidos: el ancho de esa carpeta no se supone (RF-14). Hoy el cálculo lo ignora.

## Fuera de alcance
- Publicar una versión.
- Aplicar el deny, activarlo o copiarlo a una configuración versionada.
- Renombrar cualquier nombre distinto de `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `LicenseAgentKitSdd`.
- Renumerar ítems existentes.
- Cambiar el grupo C, leerlo para esta regla o reemplazarlo en la actualización.
- Reemplazar los ADR 014 y 015, o cambiar la regla del 014 y la fuente del 015.
- Abrir cuatro specs, una por deuda.
- Cambiar el comportamiento ya cerrado de Claude Code. La única salvedad es la frase del registro cuando una clave no viaja igual (RF-5).
- Reabrir campos distintos de `model`, `omitClaudeMd` y `tools`.
- Los `README.md` que no están en la raíz del clon.
- Cambiar el marcador `.agents/.kit-version` de un proyecto.

## Criterios de finalización
- Cada RF se comprueba con un resultado observable. RF-12 ya tiene regla: `17`, `017`, `0017` y `00000017` valen 17. RF-13 ya tiene regla: el clon no avisa. RF-14 ya tiene regla: el consecutivo sale solo de los numéricos. RF-15 ya tiene regla: un ítem nuevo del clon se escribe con 4 dígitos y los ya creados no se renumeran.
- Las deudas `005_FrontmatterAgentesPorHerramienta`, `006_SddInitSugiereAllowDeny`, `007_NombresQueChocanConHerramientas` y `008_LimitacionesDeLaNumeracion` quedan citadas como origen.
- Cursor, Gemini, Copilot y Kiro tienen fecha y fuente. `model`, `omitClaudeMd` y `tools` tienen, en cada una, un veredicto de soportado, ignorado o provoca error. El registro y la columna de qué queda garantizado coinciden con ese veredicto. Kiro tiene fila de enlace, o queda escrito el otro formato, o queda escrito que no aplica.
- El comportamiento ya cerrado de Claude Code permanece. Si una clave no viaja igual, el registro lo dice.
- `/sdd-init` muestra los dos fragmentos, con la advertencia del deny y el allow genérico, y no los aplica ni los copia a una configuración versionada.
- Solo cambian `VERSION`, `CHANGELOG.md` y `LICENSE`, a los tres nombres aprobados, y las citas del producto que nombran esos archivos. El marcador `<VERSION>` del manifiesto no cambia. Esos nombres no son los que un plugin de release escribe solo. El resto conserva el nombre. El marcador de un proyecto sigue en `.agents/.kit-version`.
- Los cuatro bordes tienen una regla escrita. Esa regla cabe en RF-16: no contradice el ADR 014, no sustituye la fuente del ADR 015, no renumera y no cambia el grupo C.

## Dudas abiertas
Ninguna. Los cuatro bordes de numeración quedaron cerrados el 2026-10-25.
