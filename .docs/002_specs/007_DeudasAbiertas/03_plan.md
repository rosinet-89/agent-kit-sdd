# 03 — Plan: Deudas abiertas del kit
Spec: 01_spec.md (aprobada 2026-10-25 21:29)

## 1. Ubicación y capas

El kit es solo Markdown (constitución, principios 1, 3, 5 y 6). No hay capas de aplicación, pantallas ni base de datos. El principio 6 ya nombra `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` (2026-10-25 21:28). Los archivos siguen con el nombre viejo hasta la fase F3.

Patrón:

- **Producto:** `template/`. Los comandos, la skill y el chequeo son grupo A. El índice de numeración del producto es grupo C y es la fuente única (ADR 015). Editarlo aquí cambia lo que recibe una instalación nueva. La actualización no lee ni reemplaza el grupo C de un proyecto ya instalado.
- **Mantenimiento del clon:** `AGENTS.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, `CHANGELOG.md`, `VERSION`, `LICENSE` y los índices de `.docs/` de esta copia. No salen de `template/`.
- **Una fuente por regla.** El veredicto de campos vive en `restricciones-de-rol.md`. Los fragmentos allow y deny viven ahí. La numeración vive en « Numeración única » del índice del producto. E12 dice el resultado del chequeo y remite a esa sección. La frase mínima de un proyecto no se reescribe.
- **Copia local.** Tras la última edición de `template/.agents/`, el refresco es el de `INSTALAR.md` › Modo mantenimiento › « Refrescar la copia local ». No se redefine. Conserva `napkin.md` y no borra lo que solo está en `.agents/`.

## 2. Archivos que se crean o modifican

No se crean archivos en `template/`. Los tres de la raíz se renombran con `git mv`. El resto son ediciones.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Producto (grupo A) | Añade el veredicto de Cursor, Gemini, Copilot y Kiro, cada uno con fecha y fuente, para `model`, `omitClaudeMd` y `tools`. No reescribe las filas ya cerradas de Claude Code. Ajusta la columna `Garantizada` para que no afirme una garantía en una herramienta donde el campo no la sostiene. Los dos fragmentos se quedan en esta pieza. | RF-1, RF-2, RF-3, RF-5 |
| `template/.agents/sync-agents.md` | Producto (grupo A) | Kiro: una fila de enlace, o el otro formato descrito junto a la tabla, o « no aplica » con el porqué. El resultado sale de la lectura de F1, no se inventa. | RF-4 |
| `template/.agents/commands/sdd-init.md` | Producto (grupo A) | En cada ejecución muestra el deny y el allow de la pieza, con los cinco patrones y la advertencia, y el allow genérico. No los aplica, no los activa y no los copia a una configuración versionada. No pega una segunda copia del YAML. | RF-6, RF-7, RF-8 |
| `VERSION` → `VersionAgentKitSdd` | Raíz del clon | `git mv`. El contenido sigue siendo `0.5.0`, sin añadir el LF final que hoy no tiene. No se abre `0.5.1`. | RF-9, RF-11 |
| `CHANGELOG.md` → `ChangelogAgentKitSdd.md` | Raíz del clon | `git mv`. En el mismo archivo: la cita de la línea 21 pasa a `VersionAgentKitSdd`; la viñeta que dice que el clon sigue en 3 se corrige; se añade una viñeta de este feature dentro de `## 0.5.0 — 2026-10-24`, sin cambiar ese encabezado ni la fecha. | RF-9, RF-13, RF-15 |
| `LICENSE` → `LicenseAgentKitSdd` | Raíz del clon | `git mv`. El texto de la licencia no cambia. En `template/` no hay cita que actualizar. | RF-9, RF-11 |
| `template/.docs/002_specs/README.md` | Producto (grupo C) | « Numeración única »: entero con ceros de relleno, nombre no numérico, ítem nuevo del clon en 4 dígitos, y los nombres nuevos en la definición de clon. Un proyecto conserva el ancho de su carpeta. | RF-9, RF-12, RF-13, RF-14, RF-15, RF-16 |
| `template/.agents/validar-estructura.md` | Producto (grupo A) | Fila E12 con el resultado de la decisión 4. Ejemplo de BOM de E15: `ChangelogAgentKitSdd.md`. La fila E20 y el marcador `manifiestos/<VERSION>.md` no se redefinen ni se sustituyen. | RF-9, RF-12, RF-13, RF-14, RF-15 |
| `AGENTS.md` | Docs raíz | Líneas 111 y 120: nombres nuevos. Líneas 51 y 67: dejan de decir que el clon sigue en 3 y no sube a 4; remiten a la declaración del índice del producto. Los ya creados no se renumeran. | RF-9, RF-15 |
| `ACTUALIZAR.md` | Docs raíz | Líneas 51, 57, 184 y 313: nombres nuevos. No se tocan el marcador `<VERSION>` (líneas 53 y 64), la ruta `.agents/skills/napkin/LICENSE` (línea 287), los grupos ni el aviso de numeración. | RF-9, RF-16 |
| `INSTALAR.md` | Docs raíz | Líneas 16, 54, 56, 73 y 97: nombres nuevos. | RF-9 |
| `README.md` | Docs raíz | Líneas 68 y 73: nombres nuevos. El archivo no se renombra. | RF-9 |
| `.docs/002_specs/README.md` | Mantenimiento del clon | Solo la frase « En este clon no se sube a 4 ». Pasa a remitir a la declaración del índice del producto. No se copia aquí la regla completa. La fila `007` del índice de planes no se reescribe en estas fases. | RF-15 |
| `.docs/004_adr/README.md` | Mantenimiento del clon | Solo la frase « en este clon, 3 dígitos; no subirlos a 4 ». Remite igual. No se edita ningún ADR. | RF-15 |
| `manifiestos/0.5.0.md` | Docs raíz | Regenerado una vez al final, con el algoritmo vigente de `ACTUALIZAR.md`. No se renombra y no se edita a mano. | — |

Cobertura: RF-1 a RF-5 en el registro, la columna y `sync-agents.md`; RF-6 a RF-8 en `sdd-init.md`; RF-9 a RF-11 en el renombre y las citas; RF-12 a RF-16 en el índice del producto y en E12; la frase del clon, en los tres documentos de mantenimiento que hoy dicen « no subir a 4 ».

`05_avance.md` lo escribe la sesión principal durante la implementación. Esta fase de plan no lo toca. Los archivos de las deudas `005` a `008` no se editan en F0–F6: `/sdd-cierre` llena « Cómo se atendió » y el estado.

No se editan: la constitución; los cuatro agentes; los ADR 014 y 015; `ACTUALIZAR.md` › grupos y aviso de numeración; los README de deuda y de ADR dentro de `template/`; `.docs/003_deuda/README.md` de este clon; las carpetas `001` a `006`; las deudas `001` a `004`; un README de proyecto ya instalado; `.agents/.kit-version`; el marcador `<VERSION>`; `.agents/skills/napkin/LICENSE`.

## 3. Base de datos

No aplica. Sin `sql/`, sin DDL ni seed, sin rollback.

## 4. Seguridad

No aplica autenticación ni permisos de una aplicación. El cambio es documentación del kit.

- El deny se sugiere y no se aplica, no se activa y no se copia a `.claude/`, a un settings versionado ni a ninguna configuración del repo.
- No hay secretos ni credenciales en los archivos que se renombran o se editan.
- No se versionan scripts de instalación. El ensayo de F6 es el comando ya escrito en `AGENTS.md` › Comandos, dentro de `mktemp -d`.
- No se sobrescribe un proyecto. El grupo C no se lee ni se reemplaza.
- La búsqueda de secretos del commit queda para cuando se pida el commit, con `seguridad-basica`. Esta fase no hace commit.

## 5. Liberación / piloto

Dentro de 0.5.0 sin publicar. No se abre `0.5.1`. `VersionAgentKitSdd` sigue diciendo `0.5.0`. El encabezado `## 0.5.0 — 2026-10-24` no cambia de versión ni de fecha. No hay feature flag ni piloto.

## 6. Decisiones

| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| **(1) Veredicto en el registro, sin tocar Claude Code.** F1 lee la documentación oficial vigente de Cursor, Gemini, Copilot y Kiro. Anota, por herramienta, la fecha y la fuente (URL o nombre del documento). Cada uno de `model`, `omitClaudeMd` y `tools` queda en soportado, ignorado o provoca error. Si la fuente no alcanza para uno de los tres, esa celda no se rellena con una suposición: se dice qué falta y no hay veredicto de esa celda. Las filas de Claude Code se conservan. Si una clave no viaja igual, el registro lo dice y el frontmatter de los cuatro agentes no cambia. La columna `Garantizada` se lee junto con ese registro y no extiende una garantía a la herramienta donde el campo no la sostiene. | Adelantar « ignorado » en las cuatro. Reabrir `mode` y `permissions`. Cambiar el frontmatter de los agentes. | Sin fecha y fuente no hay veredicto (RF-1, RNF-3). El comportamiento de Claude Code ya está cerrado (RF-5). |
| **(2) Kiro en `sync-agents.md`, con uno de los tres resultados de RF-4.** Fila de enlace si la herramienta usa el mismo esquema de carpetas. Si usa otro formato, ese formato queda descrito junto a la tabla. Si no aplica, queda el porqué. No se inventa una fila de enlace que la documentación no sostenga. | Omitir Kiro, o copiar las filas de Cursor aunque el formato sea otro. | Hoy no tiene fila (RF-4). Una fila falsa empeora el chequeo de enlaces. |
| **(3) `/sdd-init` muestra la pieza, no la copia.** En la propuesta (paso 3) y también cuando el proyecto ya está personalizado, el comando manda leer y mostrar entero el deny y el allow de `restricciones-de-rol.md`: los cinco patrones, la advertencia de que el deny vale para toda la sesión y puede bloquear el Git de quien lo copie, y el allow genérico para que quien instala lo llene. El paso 4 no los escribe. No se añaden comandos de este kit ni de ningún proyecto. | Pegar el YAML en `sdd-init.md`. Aplicarlo en el paso 4. Mostrarlo solo en la primera personalización. | Una segunda copia se desfasa (RNF-1). Aplicarlo sale del alcance (RF-6). RF-6 pide la sugerencia cuando el comando se ejecuta, también si el proyecto ya estaba personalizado. |
| **(4) Numeración viva en el índice del producto y resultado en E12.** En « Numeración única »: un prefijo numérico vale su entero (`17` = `017` = `0017` = `00000017`); los ceros de relleno y el ancho escrito no son fallo y el nombre no se reescribe; dos ítems de la misma carpeta con el mismo entero son duplicado y es `FALLA`, y esa igualdad no es `AVISO`; un nombre que no empieza por dígitos y `_` (`abc_Mal`) no entra en el consecutivo ni en el ancho y no se renombra; un ítem nuevo del clon se escribe con 4 dígitos; los ya creados no se renumeran (`001` sigue `001`); `015` y `0015` son el mismo entero; en un proyecto, un ítem nuevo conserva el ancho de su carpeta. Un prefijo relleno cuenta para el entero y el duplicado, y no fija el ancho de la carpeta con su cantidad de dígitos. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014. La fila E12 dice esos resultados y remite a la sección. En el clon, los 4 dígitos valen para el ítem nuevo: el ancho ya escrito no pasa a `FALLA` y una mezcla válida no es `AVISO`. El `AVISO` de mezcla válida sigue siendo solo de proyecto. `AGENTS.md`, `.docs/002_specs/README.md` y `.docs/004_adr/README.md` de este clon dejan la frase « no subir a 4 » y remiten; no copian la regla. No se reemplazan el ADR 014 ni el ADR 015. No se cambia el grupo C. No se lee ni se reemplaza un README de proyecto. | Seguir marcando el relleno como ancho inválido. Renumerar `001` a `0001`. Dejar la declaración solo en el índice de este clon. Hacer que E12 falle todo ancho distinto de 4 en el clon. Avisar también en el clon. | Son los cuatro bordes ya cerrados el 2026-10-25. Fallar el ancho distinto de 4 dejaría en rojo los ítems ya creados. Una segunda copia de la regla rompe el ADR 015. |
| **(5) Citas vivas en el mismo `git mv`.** Pasan al nombre nuevo las rutas de la sección 2 del análisis, `README.md` incluido. No se sustituye `<VERSION>` ni `.agents/skills/napkin/LICENSE`. No se reescriben las carpetas `001` a `006` ni las deudas históricas. | Dejar `README.md` con los nombres viejos porque la lista de la sesión no lo nombró. Renombrar también el riesgo bajo. | `README.md` ordena anotar la versión y ver la licencia. Dejarlo rompe la portada en el mismo cambio. El corte de tres nombres ya está aprobado (RF-9, RF-10). |
| **(6) Manifiesto una sola vez, al final.** F5 regenera `manifiestos/0.5.0.md` cuando `template/` ya no va a cambiar, y repite E20 con el chequeo vigente. No se reescribe la fila E20 ni el algoritmo de `ACTUALIZAR.md`. Si F6 corrige un archivo de `template/`, se repite F5. El ensayo de instalación y cualquier proyecto simulado van en `mktemp -d`, nunca en este repo ni en un proyecto real. | Regenerar el manifiesto después de cada fase. Probar la numeración dentro del clon. | Un manifiesto intermedio deja E20 en rojo. El clon real no es el lugar del ensayo. |

### Candidatas a ADR

- **Ítem nuevo del clon en 4 dígitos, sin renumerar los viejos** (decisión 4). Alternativa real: dejar el clon en 3, o renumerar `001` a `0001`. Efecto duradero sobre el siguiente ítem de este repo y sobre lo que declara el índice del producto. No reemplaza al ADR 014: la convivencia de un proyecto, el `AVISO` de proyecto y el desbordamiento siguen ahí.
- **Un prefijo numérico cuenta como entero y los ceros de relleno no son fallo** (decisión 4). Alternativa real: conservar el `FALLA` por ancho inválido que E12 marca hoy. Efecto duradero sobre el chequeo. No reemplaza al ADR 014: el duplicado por valor sigue siendo fallo.
- **Solo tres nombres salen del patrón que un plugin de release escribe solo** (decisión 5). Alternativa real: renombrar también el riesgo bajo, o dejar `VERSION`, `CHANGELOG.md` y `LICENSE`. Efecto duradero sobre la raíz del clon y sobre las reglas que apuntan a esos archivos.

Creados el 2026-10-25 con el visto bueno del usuario: ADR 0016, 0017 y 0018. No reemplazan al ADR 014.

## 7. Fases y estimado

La unidad es minutos. La plantilla dice horas; en un feature que solo edita Markdown el estimado de este repo va en minutos. El real de este repo ronda un tercio del estimado.

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Preparación. Confirmar que los tres archivos siguen con el nombre viejo, que el punto 6 de la constitución ya está actualizado y que no hay otra spec editando `template/`. Al tocar un archivo, releerlo y partir de ese texto. | — | 5 |
| F1 | Frontmatter. Lectura con fecha y fuente. Registro, columna `Garantizada` y Kiro (decisiones 1 y 2). | RF-1, RF-2, RF-3, RF-4, RF-5 | 20 |
| F2 | `/sdd-init` muestra los dos fragmentos y no los aplica (decisión 3). | RF-6, RF-7, RF-8 | 8 |
| F3 | `git mv` de los tres archivos y citas vivas, incluida la línea 21 del changelog ya renombrado (decisión 5). El contenido de `VersionAgentKitSdd` sigue en `0.5.0` sin LF nuevo. | RF-9, RF-10, RF-11 | 12 |
| F4 | Índice del producto, fila E12, frases « no subir a 4 » del clon y viñeta de `0.5.0` en el changelog ya renombrado (decisión 4). | RF-12, RF-13, RF-14, RF-15, RF-16 | 16 |
| F5 | Regenerar `manifiestos/0.5.0.md` y repetir E20 (decisión 6). No se redefine E20. | — | 6 |
| F6 | Refresco de `.agents/` según `INSTALAR.md` y validación de abajo, en `mktemp -d`. Si una corrección toca `template/`, se repiten el refresco y F5. | RF-1 a RF-16 | 18 |
| | **Total** | | **85** |

El tercio observado de estos 85 minutos son unos 28 minutos.

**F6, validación.** En `mktemp -d`. Nada en este repo ni en un proyecto real.

1. Ensayo de instalación de `AGENTS.md` › Comandos: `cp -R --update=none template/. "$tmp"/` copia todo y no pisa nada.
2. `grep` del conjunto vivo (`template/`, `AGENTS.md`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, el índice de numeración del producto y el ejemplo de E15): no quedan `VERSION`, `CHANGELOG.md` ni `LICENSE` como nombre de esos tres archivos. Siguen `manifiestos/<VERSION>.md` y `.agents/skills/napkin/LICENSE`.
3. `VersionAgentKitSdd` es `0.5.0`, sin encabezado `0.5.1` en el changelog. El LF final de ese archivo sigue ausente, como hoy.
4. El registro tiene fecha y fuente por herramienta, y un veredicto por campo. La columna `Garantizada` no cubre una herramienta donde el campo no la sostiene. Las filas de Claude Code siguen. Kiro tiene uno de los tres resultados de RF-4.
5. El texto de `sdd-init.md` manda mostrar los cinco patrones, la advertencia y el allow genérico, y prohíbe aplicarlos o copiarlos. No trae comandos de un proyecto.
6. Casos de numeración sobre carpetas simuladas, con el número que se propone y el listado antes y después: `17`, `017`, `0017` y `00000017` valen 17, no son fallo y no se reescriben; dos ítems con el mismo entero son `FALLA` y no hay `AVISO`; `015` y `0015` son duplicado; `1000` junto a un ancho 3 sigue en el desbordamiento del ADR 014; `abc_Mal` no entra en el consecutivo ni en el ancho y no se renombra; una mezcla válida de proyecto es `AVISO`; en el clon, `001` junto a `0008` no es `AVISO` ni `FALLA`, el siguiente ítem se propone con 4 dígitos y `001` no se renombra.
7. E20 en verde con el manifiesto regenerado. `file -i` utf-8 y sin BOM en cada archivo tocado. E19 solo difiere en `napkin.md`, más lo que ya vivía solo en `.agents/` antes de este feature.

## 8. Criterios de aceptación

- Cursor, Gemini, Copilot y Kiro tienen fecha y fuente. `model`, `omitClaudeMd` y `tools` tienen, en cada una, soportado, ignorado o provoca error. Sin esa cita, la celda no cuenta como veredicto.
- La columna `Garantizada` y el registro dicen lo mismo. Claude Code se comporta como hasta ahora. Si una clave no viaja igual, el registro lo dice y el frontmatter no cambia.
- Kiro tiene fila de enlace, u otro formato descrito, o « no aplica » con el porqué.
- `/sdd-init` muestra el deny (cinco patrones y advertencia de toda la sesión) y el allow genérico. No los aplica, no los activa y no los copia a una configuración versionada.
- Existen `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `LicenseAgentKitSdd`. No existen ya `VERSION`, `CHANGELOG.md` ni `LICENSE` en la raíz. Las citas de la sección 2 apuntan a los nombres nuevos. El marcador `<VERSION>` y la licencia de napkin siguen. El resto de nombres del corte no cambió.
- `17`, `017`, `0017` y `00000017` valen 17 y no se reescriben. El duplicado por entero es `FALLA` y no es aviso. El clon no avisa por una mezcla válida. El proyecto sí. `abc_Mal` no cuenta y no se renombra. Un ítem nuevo del clon se escribe con 4 dígitos y `001` sigue `001`. El índice de un proyecto ya instalado no se reescribió. Los ADR 014 y 015 y el grupo C siguen.
- `manifiestos/0.5.0.md` regenerado y E20 en verde. `VersionAgentKitSdd` dice `0.5.0`. No hay `0.5.1`.
