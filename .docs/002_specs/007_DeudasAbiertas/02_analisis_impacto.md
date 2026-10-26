# 02 — Análisis de impacto: Deudas abiertas del kit

## 1. Resumen del requerimiento

Cuatro deudas abiertas se atienden en esta carpeta. El registro de `model`, `omitClaudeMd` y `tools` queda verificado en Cursor, Gemini, Copilot y Kiro, con fecha y fuente, y la columna de qué queda garantizado no afirma una garantía donde el campo no la sostiene. `/sdd-init` muestra el deny de Git (cinco patrones) y el allow genérico, con la advertencia de la sesión, y no los aplica. Solo se renombran `VERSION`, `CHANGELOG.md` y `LICENSE`, y cada cita viva pasa al nombre nuevo. Los cuatro bordes de numeración quedan escritos: el entero, el clon sin AVISO, el nombre que no es número, y el ítem nuevo del clon en 4 dígitos sin renumerar los ya creados.

### 1.1 Antecedentes (índice de planes: carpetas que coinciden, qué se reutiliza y qué contradice; o "sin antecedentes en el índice" + grep que lo confirma)

Índice leído el 2026-10-25 21:36 (`.docs/002_specs/README.md`). La fila `007` coincide con esta carpeta.

- `004_ModeloYPermisosAgentes` (Implementado, cierre 2026-10-25 16:13). Cerró el frontmatter en Claude Code y dejó el veredicto de las otras herramientas y la sugerencia de `/sdd-init`. Se reutiliza el registro de campos y los dos fragmentos opcionales. No se reabre el comportamiento de Claude Code.
- `005_ConvivenciaNumeracion` (Implementado, cierre 2026-10-25 16:50). Cerró la convivencia de anchos, el ADR 014 y el ADR 015, y dejó los cuatro bordes en la deuda 008. Esta spec no reemplaza esos ADR ni cambia el grupo C.
- `001_ActualizarKit`, `002_Kit050`, `003_OptimizarArnes` y `006_RutaSiNoDelega` están implementadas. No definen estas cuatro deudas. Sus carpetas no se reescriben: son registro.
- No hay otra carpeta abierta para estos cuatro temas. La fila `007` es la de este caso.

### 1.2 Deuda técnica relacionada (`.docs/003_deuda/README.md`: deudas abiertas del módulo y si este feature las resuelve o las deja; o "ninguna")

Índice leído el 2026-10-25 21:36. Las cuatro están `En atención` por esta carpeta y este feature las resuelve. El estado pasa a `Resuelta` en `/sdd-cierre`, no en el análisis ni en el plan.

| Deuda | Qué hace este feature |
|---|---|
| `005_FrontmatterAgentesPorHerramienta` | La resuelve: veredicto por herramienta y por campo, y fila o formato de Kiro. |
| `006_SddInitSugiereAllowDeny` | La resuelve: `/sdd-init` sugiere los dos fragmentos y no los aplica. |
| `007_NombresQueChocanConHerramientas` | La resuelve en el corte ya aprobado: solo los tres nombres de riesgo alto y medio. El riesgo bajo se queda. |
| `008_LimitacionesDeLaNumeracion` | La resuelve: los cuatro bordes quedan escritos sin reemplazar el ADR 014 ni el ADR 015. |

Las deudas `001` a `004` están resueltas. No se reabren ni se reescriben.

## 2. Módulos, menú, tablas y archivos involucrados

### 2.1 Menú / opciones / rutas de acceso

No aplica una pantalla. El acceso es el flujo del kit: `/sdd-init`, el chequeo E12 y E15 de `/sdd-estructura`, y los documentos de regla de la raíz.

### 2.2 Tablas (existentes y nuevas)

No aplica.

### 2.3 Archivos de código (ruta, capa)

Lectura y `grep` del 2026-10-25 21:36. El kit es Markdown. No hay capas de aplicación.

**Frontmatter (deuda 005).** El veredicto de Claude Code ya está. Cursor, Gemini y Copilot figuran como no verificados. Kiro no tiene fila.

| Ruta | Capa | Qué tiene hoy |
|---|---|---|
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Producto (grupo A) | Registro de campos con columnas solo de Claude Code (leído 2026-10-25). Línea 26: `model`, `omitClaudeMd`, `mode` y `permissions` no verificados en Cursor, Gemini y Copilot. La columna `Garantizada` no distingue herramienta. |
| `template/.agents/sync-agents.md` | Producto (grupo A) | Tabla de enlaces, líneas 61-73: Claude Code, Cursor, Gemini y GitHub (Copilot). No hay fila de Kiro. |
| `template/.agents/agents/coordinator.md`, `planner.md`, `implementer.md`, `reviewer.md` | Producto (grupo A) | Frontmatter ya cerrado en Claude Code. Esta spec no lo cambia. |

**Allow y deny (deuda 006).** Los fragmentos existen. `/sdd-init` no los menciona (`grep` de `deny` y `allow` en `template/.agents/commands/sdd-init.md`: sin coincidencias).

| Ruta | Capa | Qué tiene hoy |
|---|---|---|
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Producto (grupo A) | Líneas 53-79: deny con los cinco patrones (`commit`, `push`, `fetch`, `pull`, `sync`) y la advertencia de toda la sesión; allow genérico `Bash(comando de AGENTS.md › Comandos)`. Texto de la pieza. No se copia a `.claude/` ni a settings versionados. |
| `template/.agents/commands/sdd-init.md` | Producto (grupo A) | Entrevista, propuesta y aplicación. No sugiere los fragmentos. Si el proyecto ya está personalizado, avisa y pregunta; tampoco los muestra. |

**Nombres (deuda 007).** Los tres archivos siguen con el nombre viejo. `VERSION` contiene `0.5.0` y no tiene LF final. `LICENSE` empieza por `MIT License` y no cita su propio nombre.

Citas vivas comprobadas, que nombran el archivo y pasan al nombre nuevo:

| Ruta | Línea | Qué nombra |
|---|---|---|
| `template/.docs/002_specs/README.md` | 56 | Definición de clon: `VERSION` y `CHANGELOG.md` en la raíz. |
| `template/.agents/validar-estructura.md` | 48 | Ejemplo de BOM de E15: `CHANGELOG.md`. |
| `AGENTS.md` | 111 | Docs de la raíz: `CHANGELOG.md`, `VERSION`, `LICENSE`. |
| `AGENTS.md` | 120 | Cada versión actualiza `VERSION` y `CHANGELOG.md`. |
| `ACTUALIZAR.md` | 51, 57 | La raíz del kit o del destino tiene `VERSION`. |
| `ACTUALIZAR.md` | 184 | La versión destino es la línea del archivo `VERSION`. |
| `ACTUALIZAR.md` | 313 | `CHANGELOG.md` es la guía de lo agregado. |
| `INSTALAR.md` | 16 | Clon: hay `template/`, `VERSION` y `CHANGELOG.md`. |
| `INSTALAR.md` | 54, 56, 73 | La línea del archivo `VERSION` del kit. |
| `INSTALAR.md` | 97 | Raíz del clon: existen `template/`, `VERSION` y `AGENTS.md`. |
| `README.md` | 68 | Cada versión se anota en `CHANGELOG.md` y `VERSION`. |
| `README.md` | 73 | `MIT. Ver LICENSE`. |
| `CHANGELOG.md` | 21 | El chequeo E20 compara con el manifiesto de `VERSION`. |

`README.md` no estaba en la lista cerrada de la sesión. El `grep` lo encontró: son instrucciones de la raíz, del mismo tipo que las de `AGENTS.md`. Entran en el mismo cambio. La alternativa de dejarlas con el nombre viejo dejaría la portada apuntando a archivos que este cambio retira.

No se sustituyen, comprobado en el mismo `grep`:

- `manifiestos/<VERSION>.md` es el número de versión. Aparece en `template/.agents/validar-estructura.md` línea 37 (E20) y en `ACTUALIZAR.md` líneas 53 y 64. El marcador se queda.
- `ACTUALIZAR.md` línea 287: `.agents/skills/napkin/LICENSE`. Es la licencia de la skill napkin, no el `LICENSE` de la raíz. El archivo existe en `.agents/skills/napkin/LICENSE` y en `template/.agents/skills/napkin/LICENSE`.
- El índice de numeración de este clon (`.docs/002_specs/README.md` › « Numeración única ») no nombra `VERSION`, `CHANGELOG.md` ni `LICENSE`. La fila `007` del índice de planes los usa como palabras clave del feature: es registro de la fila, no una regla que abra el archivo.
- `template/` no cita `LICENSE`. El `grep` de `VERSION`, `CHANGELOG.md` y `LICENSE` en `template/` solo dio la definición de clon, el ejemplo de E15 y el marcador `<VERSION>` de E20.
- Carpetas `001` a `006`, deudas `001` a `004` y los ADR ya aceptados nombran los archivos viejos como registro. No se reescriben.

El punto 6 de la constitución ya nombra `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` (2026-10-25 21:28). Esta fase no lo reedita. Los tres archivos de la raíz todavía no se movieron.

**Numeración (deuda 008).**

| Ruta | Capa | Qué tiene hoy |
|---|---|---|
| `template/.docs/002_specs/README.md` › « Numeración única » | Producto (grupo C, fuente única) | Ancho válido: 3 o 4 dígitos, o los que el valor necesita sin ceros de relleno; 1 o 2 dígitos es inválido. El clon se detecta por `template/`, `VERSION` y `CHANGELOG.md`, y el ancho « es el que declara el índice de esa copia ». No dice que un ítem nuevo del clon se escriba con 4 dígitos. |
| `template/.agents/validar-estructura.md` fila E12 | Producto (grupo A) | `FALLA` por duplicado, por ancho inválido (ni 3 ni 4 ni los que el valor necesite) y, en el clon, por ancho distinto del declarado salvo desbordamiento. `AVISO` solo en un proyecto con mezcla válida. No dice que el clon no avisa, ni que `17` y `00000017` son el mismo entero, ni qué hacer con `abc_Mal`. |
| `.docs/002_specs/README.md` línea 53 | Mantenimiento del clon | « En este clon no se sube a 4. » |
| `AGENTS.md` líneas 51 y 67 | Mantenimiento del clon | El clon sigue en 3 dígitos y no se renumera a 4. `NNN` del clon es de 3 dígitos. |
| `.docs/004_adr/README.md` línea 3 | Mantenimiento del clon | « en este clon, 3 dígitos; no subirlos a 4 ». |
| `CHANGELOG.md` líneas 22-26 | Docs raíz | El clon « siguen en 3 » y E12 informa los anchos mezclados como `AVISO`, sin decir que el clon no entra en ese aviso. |
| `.docs/004_adr/014_ConvivenciaDeAnchosDeNumeracion.md` y `015_FuenteUnicaDeNumeracionEnGrupoC.md` | ADR aceptados | Conservan la regla y la fuente. No se reemplazan. |
| `ACTUALIZAR.md` › grupos y aviso de numeración | Docs raíz | El grupo C no se lee ni se reemplaza. El aviso detecta ítems de 3 dígitos por el nombre de la carpeta. Ese mecanismo se queda. |

`.docs/003_deuda/README.md` de este clon remite a « Numeración única » y no repite « no subir a 4 ». Los README de deuda y de ADR dentro de `template/` no declaran el ancho del clon.

La copia local `.agents/validar-estructura.md`, `.agents/sync-agents.md`, `.agents/commands/sdd-init.md` y `.agents/skills/sdd/restricciones-de-rol.md` repite al producto. E19 pide que `template/.agents` y `.agents` solo difieran en `napkin.md`. El refresco es el de `INSTALAR.md`; no se inventa otro.

### 2.4 Código legado o sin uso (no tocar, sí documentar)

- Nombres de riesgo bajo, que conservan el nombre: `AGENTS.md`, `.agents/`, `.claude/`, `.cursor/`, `.github/`, `.gemini/`, `CLAUDE.md`, `MEMORY.md`, `.gitignore`, `README.md` de la raíz (el archivo no se renombra; sí se actualizan sus dos citas), `template/`, `ACTUALIZAR.md`, `INSTALAR.md`, `manifiestos/`, `.docs/`, `SKILL.md`, `PREFERENCIAS.md` y `napkin.md`.
- Frontmatter de los cuatro agentes y el veredicto ya cerrado de Claude Code.
- ADR 014, ADR 015 y el grupo C de `ACTUALIZAR.md`. Un README de proyecto ya instalado no se lee ni se reemplaza.
- Carpetas de spec `001` a `006` y deudas `001` a `004`.
- Marcador `.agents/.kit-version` de un proyecto.
- El archivo `manifiestos/0.5.0.md` no se renombra. Se regenera al final si `template/` cambia.

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)

No aplica. `AGENTS.md` › Comandos declara la consulta de datos como « No aplica ». No hay base de datos.

Lo comprobado el 2026-10-25 21:36 es lectura de archivos y `grep`, citado en la sección 2. `VERSION` es `0.5.0`, sin LF final. El bloque de `CHANGELOG.md` sigue siendo `## 0.5.0 — 2026-10-24`. No existe encabezado `0.5.1`.

## 4. Flujo actual

1. Quien mira el registro de campos ve el veredicto de Claude Code. Cursor, Gemini y Copilot quedan en « no verificado ». Kiro no aparece en la tabla de enlaces. La columna `Garantizada` habla del mecanismo, no de la herramienta.
2. Quien corre `/sdd-init` personaliza el proyecto y no ve el deny ni el allow. Los fragmentos solo están en `restricciones-de-rol.md`. Copiarlos es un acto manual. El deny, si alguien lo copia, vale para toda la sesión.
3. Un plugin de release que escribe `VERSION`, `CHANGELOG.md` o `LICENSE` pisa el archivo de la raíz del clon. Las reglas de `AGENTS.md`, `ACTUALIZAR.md`, `INSTALAR.md` y `README.md` apuntan a esos nombres. El punto 6 de la constitución ya apunta a los nombres nuevos y los archivos no.
4. E12 cuenta mal un prefijo con ceros de relleno: lo trata como ancho inválido y es `FALLA`. `17`, `017`, `0017` y `00000017` no están dichos como el mismo entero. Una mezcla válida en el clon no produce `AVISO`, y la fila no lo dice. `abc_Mal` no está dicho. El índice del producto no declara los 4 dígitos del ítem nuevo del clon. El índice de este clon y `AGENTS.md` dicen que aquí no se sube a 4. Los ítems ya creados siguen en 3 (`001` a `007` en specs, deudas hasta `008`, ADR hasta `015`).

## 5. Reglas de negocio: conocidas vs código

| Regla de la spec | Hoy |
|---|---|
| Veredicto de `model`, `omitClaudeMd` y `tools` en las cuatro herramientas, con fecha y fuente (RF-1, RF-2, RNF-3) | Solo Claude Code tiene veredicto. Las otras tres dicen « no verificado ». Kiro no está. |
| La columna de garantía no afirma lo que el campo no sostiene (RF-3) | La columna no mira la herramienta. |
| Kiro tiene fila, otro formato descrito, o no aplica con el porqué (RF-4) | No tiene fila ni una frase que lo explique. |
| Claude Code se conserva (RF-5) | Ya cerrado en la `004`. El registro no dice nada de claves que viajen distinto. |
| `/sdd-init` sugiere deny y allow, y no los aplica ni los copia (RF-6, RF-7, RF-8, RNF-2) | No los sugiere. El texto de los fragmentos ya es genérico y ya trae la advertencia. |
| Solo tres renombres, y las citas vivas en el mismo cambio (RF-9, RF-10, RF-11) | Nada se ha movido. El punto 6 de la constitución ya usa los nombres nuevos. `LICENSE` no tiene cita en `template/`. |
| El entero ignora ceros de relleno; el duplicado es `FALLA` y no es aviso (RF-12) | E12 marca inválido el ancho que no es 3, 4 o los dígitos justos sin relleno. El duplicado por valor ya es `FALLA`. |
| El clon no emite `AVISO` por mezcla válida; el proyecto sí (RF-13) | El `AVISO` ya es de proyecto. Falta la frase de que el clon no entra. |
| `abc_Mal` no cuenta y no se renombra (RF-14) | La definición de ítem ya exige dígitos y `_`. E12 no lo dice. |
| Ítem nuevo del clon en 4 dígitos; los viejos no se renumeran; el índice del producto lo declara (RF-15) | El índice del producto no lo declara. El clon dice « no se sube a 4 ». |
| Siguen el ADR 014, el ADR 015, el grupo C y la frase mínima (RF-16, RNF-1) | Así está. Esta spec no abre otra fuente. |

## 6. Riesgos y casos borde

- Si la declaración del clon pasa a 4 dígitos y E12 sigue fallando « ancho distinto del declarado », los `001` ya creados quedan en `FALLA`. El chequeo tiene que usar los 4 dígitos solo para el ítem nuevo y dejar el ancho ya escrito.
- Un prefijo con ceros de relleno cuenta para el entero y para el duplicado. No debe fijar el ancho de la carpeta con su cantidad de dígitos, y no se reescribe.
- `1000` en una carpeta de ancho 3 sigue siendo desbordamiento del ADR 014. No es el borde de los ceros de relleno.
- Cambiar « Numeración única » en `template/` cambia la huella del texto que `ACTUALIZAR.md` ofrece. En un proyecto que marcó « mantener », el aviso puede volver a salir. No se cambia ese mecanismo, no se lee el grupo C y no se reemplaza el README del proyecto.
- Sustituir el marcador `<VERSION>` rompería E20 y el nombre `manifiestos/0.5.0.md`. Sustituir `.agents/skills/napkin/LICENSE` apuntaría la licencia de la skill al archivo de la raíz.
- Dejar `README.md` con los nombres viejos contradice a `AGENTS.md` en el mismo cambio.
- Copiar el YAML del deny dentro de `sdd-init.md` abre una segunda fuente. El comando muestra el fragmento que ya está en la pieza.
- Aplicar el deny, o copiarlo a `.claude/` o a un settings versionado, bloquea el Git de la sesión y sale del alcance.
- Un veredicto sin fecha y fuente no cumple RF-1. No se adelanta el resultado de Cursor, Gemini, Copilot ni Kiro.
- Si `tools` queda soportado en una herramienta y provoca error en otra, la columna `Garantizada` no puede seguir en una sola frase para las dos.
- Refrescar `.agents/` con un procedimiento distinto al de `INSTALAR.md` puede pisar `napkin.md` o borrar lo que solo está en la copia local.
- `VERSION` no tiene LF final. Normalizarlo cambia bytes que esta spec no pide.
- Regenerar el manifiesto a mitad de las ediciones de `template/` deja E20 en rojo hasta la última. Se regenera una vez, al final, y se repite si una corrección vuelve a tocar `template/`.
- El ADR 014 sigue diciendo que el ancho inválido es fallo. No se reemplaza. La regla viva queda en el índice y en E12. La diferencia es candidata a ADR, no un parche del 014.

## 7. Preguntas abiertas

Ninguna. La spec está aprobada y la sección Dudas abiertas dice « Ninguna ».
