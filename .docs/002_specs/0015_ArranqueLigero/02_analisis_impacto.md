# 02 — Análisis de impacto: Arranque ligero de sesión

## 1. Resumen del requerimiento
Bajar lo que el kit carga al arrancar una sesión por dos vías: el archivo siempre cargado
(`AGENTS.md`, que `CLAUDE.md` inyecta) y las lecturas del protocolo «Cada sesión». No cambia
«Guardrails», «Comandos», «Stack» ni «Reglas de negocio», y no cambia el resultado del arranque.
Alcance: las dos capas (raíz y `template/`), con las réplicas `.agents/` iguales.

### 1.1 Antecedentes
- `003_OptimizarArnes` (cerrada): redujo la lectura de agentes y comandos, con línea base en bytes.
  Se reutiliza su método (`wc -c` antes y después, tokens solo como referencia). No tocó el arranque
  de la sesión principal, así que no contradice esta spec.
- `0013_ModelosPorAgente` y `0014_ConfiguracionGuiada` (cerradas el 2026-11-21, kit 0.9.0 y 0.10.0):
  ya están implementadas. El bloqueo «implementar después de la 0013» del avance queda cumplido.
  Engordaron las piezas que mide esta spec (ver § 3): la 0014 agregó el punto 5 del chequeo de
  arranque y la configuración por dev; la 0013 agregó roles `tester` y `explore`.
- `0016` (cerrada, kit 0.11.0): cierre en una corrida. No toca el arranque.
- ADR 009 (`FuenteUnicaPorFase`) cita «Qué lee cada rol» como sección que «nombra secciones; no es
  otra copia». Es un registro histórico: no se edita. El ADR nuevo de esta spec (ver `03_plan.md`
  § 6) dice que la tabla salió.
- ADR 0021 y 0022 (estado «visto» y arranque sin réplicas): fijan qué hace el chequeo. Esta spec no
  cambia qué hace, solo qué lee.

### 1.2 Deuda técnica relacionada
Deuda abierta: 0009, 0010, 0011, 0012 y 0013.
- 0010 (`RaizSinChequeoAutomaticoDelMarcador`): toca el paso 1 de «Cada sesión» y E21. Esta spec no
  cambia el paso 1 ni E21, así que no la resuelve ni la empeora.
- 0011 (`/sdd-constitucion` fuera de la tabla de fases y del ruteo): roza «Roles y comandos», que se
  mueve, pero el hallazgo es sobre la skill `sdd` y el coordinator. Se deja.
- 0009, 0012 y 0013: sin relación.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica (el kit es Markdown). Entradas: el arranque de la sesión principal (`AGENTS.md` ›
«Cada sesión»), `/sdd-enlaces`, `/sdd-estructura`, `/sdd-init` y la actualización (`ACTUALIZAR.md`).

### 2.2 Tablas (existentes y nuevas)
No aplica: sin base de datos.

### 2.3 Archivos
| Archivo (las dos capas salvo que se diga) | Qué tiene hoy que importa | Uso |
|---|---|---|
| `AGENTS.md` (raíz) y `template/AGENTS.md` | «Cada sesión» pasos 4, 7 y 8; secciones «Roles y comandos», «Qué lee cada rol» y «Formato y numeración» | Siempre cargado |
| `.agents/sync-agents.md` | Tabla de estados dentro de «Qué hace el agente», paso 1; «Estado «visto»»; «Chequeo de arranque»; definición de `OK` por archivo dentro de «Carpeta de agentes por archivo» y «OpenCode» | Paso 7 y `/sdd-enlaces` |
| `.agents/README.md` | Layout, altas de skill, agente y comando, anti-patrones; cita «Roles y comandos» de `AGENTS.md` (línea 78) | Paso 4, `/sdd-init`, E17 |
| `.agents/validar-estructura.md` | E8 compara roles con «Roles y comandos» de `AGENTS.md`; E9 exige que todo comando exista **y esté citado en `AGENTS.md`** | `/sdd-estructura` |
| `.agents/commands/sdd-enlaces.md` | Lista lo que lee de `sync-agents.md` («estados, tabla de enlaces, …») | `/sdd-enlaces` |
| `.agents/configuracion.ejemplo.md` | Cita `sync-agents.md` › «Chequeo de arranque» | `/sdd-config` |
| `.agents/agents/*.md` | Cada rol ya declara qué secciones de `AGENTS.md` lee | Todos los roles |
| `ACTUALIZAR.md` | Grupo B: `AGENTS.md` nunca se reemplaza; faltantes a mano | Actualización |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.12.0.md` | Versión 0.11.0 | Liberación |

### 2.4 Código legado o sin uso
- La tabla «Qué lee cada rol» duplica lo que declara cada `.agents/agents/<rol>.md` en su «Qué lee».
  Ya divergen: la fila de `explore` dice «Comandos» y `explore.md` dice «De `AGENTS.md`, solo
  "Guardrails"». Manda el archivo del rol.
- «Formato y numeración» duplica `.docs/002_specs/README.md` › «Numeración única»: ancho de 3
  dígitos de las carpetas de sección, ancho de un ítem nuevo, 4 dígitos en el clon y la regla de
  «conserva el número la que ya está en la rama remota» (punto 3 de la revisión de duplicados). Su
  primer punto («toda carpeta nace en formato SDD») duplica la skill `sdd` y la nota de «Skills de
  proyecto».

## 3. Datos reales (medición de solo lectura, `wc -c`, 2026-11-21 16:05, este clon en `main`, 8c9681b)
Sin base de datos. La medición sustituye a las consultas.

| Pieza | Línea base congelada (spec) | Hoy (tras 0013, 0014 y 0016) |
|---|---|---|
| `AGENTS.md` raíz | 13 314 | 14 279 |
| `template/AGENTS.md` | 11 859 | 12 990 |
| `.agents/sync-agents.md` (entero, las dos capas) | 10 689 | 15 336 |
| de él, «Chequeo de arranque» | 2 063 | 2 853 |
| `.agents/README.md` | 4 927 | 5 475 |
| `MEMORY.md` | 3 415 | 3 779 |
| `formato-respuesta/SKILL.md` | 2 443 | 2 480 |
| `.agents/napkin.md` | 1 579 | 1 579 |
| constitución | 1 133 | 1 133 |
| **Lectura del arranque bajo control del kit (raíz)** | **24 186** | **29 782** |

Bytes de hoy por sección de `AGENTS.md` (raíz / `template/`): Roles y comandos 1 095 / 1 095 ·
Qué lee cada rol 1 009 / 1 009 · Formato y numeración 726 / 643 · Deuda técnica 676 / 682.
Secciones de `sync-agents.md`: encabezado 458 · Qué hace el agente 3 518 (de ella, tabla de estados
386) · Estado «visto» 1 517 · Chequeo de arranque 2 853 · Tabla de enlaces 5 749 · Notas 1 241.

Dato clave del escenario base: en este clon no existe `.agents/.kit-version` y sí `template/`, así que
el punto 1 del chequeo de arranque lo omite. Hoy la sesión igual abre los 15 336 bytes de
`sync-agents.md` para llegar a esa regla.

## 4. Flujo actual
1. `CLAUDE.md` inyecta `AGENTS.md` entero (14 279 bytes en la raíz).
2. «Cada sesión»: paso 2 napkin, paso 3 `MEMORY.md`, paso 4 `.agents/README.md` (siempre), paso 5
   constitución, paso 7 «sigue `sync-agents.md` › «Chequeo de arranque»» (en la práctica se abre el
   archivo entero: la sección remite a «Estado «visto»», a los estados del paso 1 de «Qué hace el
   agente» y a la tabla), paso 9 `formato-respuesta`, paso 10 `PREFERENCIAS.md`.
3. Paso 8: revisar prefijos repetidos «(ver "Formato y numeración")».

## 5. Reglas de negocio: conocidas vs código
| Regla | Dónde vive hoy | Comprobación |
|---|---|---|
| Un subagente no repite el arranque | `AGENTS.md` › «Qué lee cada rol» | Si sale la sección, la frase debe quedarse en `AGENTS.md` (caso límite de la spec) |
| «Guardrails» no cuenta contra el tope de `sdd-implement` ni contra la lista cerrada de `reviewer` | Solo en «Qué lee cada rol» | No está en `sdd-implement.md` ni en `reviewer.md`: hay que conservarla |
| Todo comando existente está citado en `AGENTS.md` | E9 | Si «Roles y comandos» sale, E9 daría `FALLA`: E9 se apunta al nuevo lugar |
| Los roles coinciden con «Roles y comandos» de `AGENTS.md` | E8 | Igual que E9 |
| El chequeo de arranque se omite en el clon del kit | `sync-agents.md` › «Chequeo de arranque», punto 1 | Se conserva; hoy se lee tras abrir el archivo entero |

## 6. Riesgos y casos borde
- **E8 y E9 dan `FALLA`** si «Roles y comandos» sale sin mover su verificación. Se cambian en el mismo
  paso (RF-5, RF-11).
- **Proyecto con `AGENTS.md` viejo y `.agents/` nuevo** (RF-13): conserva «Roles y comandos» y
  «Formato y numeración» propios y su paso 7 viejo. El paso 7 viejo cita `sync-agents.md` › «Chequeo
  de arranque», que se conserva con ese nombre y es la que dice qué más leer. E8 y E9 nuevos leen
  `.agents/README.md`, que llega con el `.agents/` nuevo. Riesgo bajo; se comprueba con el ensayo de
  actualización.
- **Lectura parcial de un archivo:** el paso 7 pide leer secciones, no el archivo. Si la herramienta lo
  abre entero, el ahorro se pierde, pero el resultado no cambia. Mitigación: las secciones del
  arranque quedan juntas y al inicio de `sync-agents.md`, así basta un `Read` con límite de líneas.
- **Meta del 35 %:** la medición de hoy (29 782) ya supera la línea base congelada (24 186) por lo que
  sumaron la 0013 y la 0014. La meta se mide contra la línea base congelada, como dice la spec. Ver
  estimado en `03_plan.md` § 8.
- **«Formato y numeración» en proyectos:** la fuente que queda es `.docs/002_specs/README.md`, que es
  del proyecto. Un proyecto actualizado con un índice viejo conserva su `AGENTS.md` viejo, que todavía
  tiene la sección. Un proyecto nuevo trae los dos archivos nuevos.
- **Divergencia de `explore`** (§ 2.4): al salir la tabla, rige `explore.md`. No cambia la conducta de
  `explore`, que ya sigue su archivo.

## 7. Preguntas abiertas
Ninguna que bloquee. Queda pendiente el visto bueno para registrar el ADR del criterio (ver `03_plan.md`
§ 6).
