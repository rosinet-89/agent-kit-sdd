# 03 — Plan: Actualizar un proyecto a una versión nueva del kit
Spec: 01_spec.md (aprobada 2026-10-24; hora no registrada en el índice)

## 1. Ubicación y capas
El kit es solo Markdown (ADR 004) y se organiza en capas (constitución, principio 3; ADR 001 y 002):

| Capa | Qué entra aquí |
|---|---|
| Docs de la raíz (manual del kit, no se distribuye) | `ACTUALIZAR.md` (procedimiento), `manifiestos/<versión>.md`, `INSTALAR.md`, `README.md`, `CHANGELOG.md`, `VERSION` |
| `template/` (producto, se copia a los proyectos) | `AGENTS.md`, `.agents/README.md`, `.agents/validar-estructura.md` (ajustes mínimos) |
| Conocimiento propio de la raíz (versionado) | `AGENTS.md` raíz, `.docs/` (spec, plan, constitución y ADR 007 **ya existen**) |
| Datos que escribe el procedimiento en cada proyecto (no son del kit) | `.agents/.kit-version` (marcador) y `.agents/.kit-mantener` (registro "mantener") |

No hay código, comando nuevo ni base de datos. El "patrón" es: el documento de procedimiento le dice al
agente qué calcular, mostrar y confirmar; los manifiestos son datos; el chequeo E20 mantiene los datos
honestos.

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `ACTUALIZAR.md` (nuevo) | Docs raíz | Procedimiento completo: precondiciones (kit disponible, destino con manifiesto, entorno con SHA-256, no correr sobre el clon), definición exacta de la huella y prueba de humo, versión base/destino, tabla de clasificación, grupos A/B/C/D por ruta, plan agrupado y confirmación, aplicación, "mantener", marcador, reporte, enlaces, commit propuesto, idempotencia y retoma, conflictos de ruta, ejemplos de comandos por SO | RF-1 a RF-12, RF-14 a RF-16, RF-18 a RF-21, RF-23 a RF-25 |
| `manifiestos/0.4.0.md` (nuevo) | Docs raíz | Rutas y huellas de los 35 archivos de 0.4.0, generado desde `861bb99` | RF-17, RF-19 |
| `manifiestos/0.5.0.md` (nuevo) | Docs raíz | Rutas y huellas de los 41 archivos de 0.5.0, generado lo último | RF-17 |
| `INSTALAR.md` (modifica) | Docs raíz | Paso previo de detección de kit ya instalado que remite a `ACTUALIZAR.md`; paso que escribe el marcador con la versión de `VERSION`; paso manual del marcador en "A mano"; enlace a `ACTUALIZAR.md` | RF-13, RF-18, RF-22 |
| `README.md` (modifica) | Docs raíz | Árbol (`ACTUALIZAR.md`, `manifiestos/`), sección Instalación con enlace a actualizar, reescribe la regla "Para actualizar un proyecto existente, compara..." | RF-18 |
| `CHANGELOG.md` (modifica) | Docs raíz | Entradas de 0.5.0: actualización, manifiestos, marcador, E20 | RF-17, RF-18 |
| `VERSION` | Docs raíz | Sin cambio (ya es 0.5.0); se verifica en el cierre | — |
| `AGENTS.md` raíz (modifica) | Propio versionado | Regla de negocio: la instalación no sobrescribe; la actualización solo reemplaza KIT-VIEJO confirmados (ADR 007); manifiestos fuera de `template/`; excepción RNF-9 en la línea de lint | RF-8, RNF-9 |
| `template/AGENTS.md` (modifica) | Producto | Una línea en "Reglas duras": no reinstalar encima, el paso a una versión nueva usa el procedimiento de actualización del kit y la versión instalada está en el marcador. Sin comillas inversas en la ruta del documento (E11 correría en el proyecto) | RF-18, RF-22 |
| `template/.agents/README.md` (modifica) | Producto | Árbol: `.kit-version` y `.kit-mantener` como archivos del procedimiento, versionables | RF-13, RF-21 |
| `template/.agents/validar-estructura.md` (modifica) | Producto | Nuevo E20 (kit): huellas de `template/` = `manifiestos/<VERSION>.md`, sin archivos de más ni de menos; E11 aclara que no aplica a manifiestos; texto final "E1–E20" | RF-17, RNF-9 |
| `.agents/validar-estructura.md` y `.agents/README.md` raíz (copia local) | Local, no versionado | Se refrescan con `cp -R template/.agents/. .agents/` conservando `napkin.md`; no es entregable | — |
| `.docs/002_specs/README.md` (modifica) | Propio versionado | Fila 001: estatus al cerrar | — |
| `template/.agents/sync-agents.md` | Producto | **Sin cambios**: `ACTUALIZAR.md` lo cita para el paso de enlaces y los estados `OCUPADO` | RF-14, RF-23 |
| `.docs/001_constitucion/constitution.md`, `.docs/004_adr/007_*.md` | Propio versionado | **Ya hechos** (enmienda del principio 5 y ADR 007) | criterio de finalización |

Cobertura: RF-1 a RF-12, RF-14 a RF-16, RF-19 a RF-21, RF-23 a RF-25 → `ACTUALIZAR.md`; RF-13 y RF-22 →
`INSTALAR.md` (más `template/.agents/README.md`); RF-17 → manifiestos más E20; RF-18 → `ACTUALIZAR.md`,
`INSTALAR.md`, `README.md`. RNF-1 a RNF-8 se cumplen por diseño (ver sección 6); RNF-9 en `AGENTS.md`
raíz y E11/E20.

## 3. Base de datos
No aplica (sin `sql/`, sin DDL ni seed).

## 4. Seguridad
- **No pisar trabajo:** solo se reemplaza KIT-VIEJO (P = B exacta), con diferencia visible y
  confirmación explícita (por archivo o por grupo ya mostrado). Ante duda, PERSONALIZADO (RNF-4).
- **Nada sin confirmar:** ni borrar RETIRADO, ni tocar grupo B/C/D, ni crear enlaces, ni hacer commit.
- **Proyecto con Git:** aviso de árbol sucio y commit aparte con solo los archivos tocados
  (skill `git-commit`; revisión de secretos con `seguridad-basica` antes de proponerlo). **Sin Git:**
  respaldo opcional de lo reemplazado en una carpeta temporal fuera del proyecto.
- **Sin secretos:** ni el marcador, ni el registro "mantener", ni los manifiestos llevan datos del
  proyecto ni credenciales (RNF-5). Antes del commit del kit se busca con `seguridad-basica`.
- **Idempotencia y retoma:** el marcador solo avanza sin pendientes (RF-12); repetir da IGUAL (RF-15).
- **Scripts:** solo temporales, fuera de los repos, generados por el agente (RNF-8); no se versiona ninguno.
- **Precondición de integridad:** si `template/` no coincide con el manifiesto de la versión destino, no
  se modifica nada (evita clasificar contra un manifiesto que miente).

## 5. Liberación / piloto
Va dentro de 0.5.0, que aún no se publica (sin piloto ni flag). Todas las pruebas, incluida la
actualización simulada, se hacen en `mktemp -d`, nunca en un proyecto real ni en el kit real. Los
comandos `git archive` extraen fuera del repo.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| **(1) Manifiestos** en `manifiestos/<versión>.md` en la raíz del kit (fuera de `template/`), Markdown con una tabla `Ruta \| Huella` ordenada por ruta, rutas relativas a la raíz del proyecto (sin `template/`), una fila por archivo de `template/` (grupos A, B y C; el grupo lo da la ruta, no el manifiesto) | Dentro de `template/` (se copiarían a los proyectos y habría que clasificarlos); un JSON/YAML o un `.sha256`; una columna "grupo" | Fuera de `template/` evita circularidad y mantiene el kit genérico; Markdown es legible y consistente con ADR 004; el grupo vive en una sola fuente (`ACTUALIZAR.md`) |
| **(2) Huella:** SHA-256 en hexadecimal minúscula (64 caracteres) sobre los bytes de esta normalización, en orden: (a) quitar BOM inicial `EF BB BF`; (b) cambiar CRLF por LF; (c) quitar espacios y tabuladores al final de cada línea; (d) quitar líneas vacías al final y terminar con **un** LF (archivo vacío = 0 bytes). Sin decodificar el texto, sin considerar permisos ni mayúsculas del nombre. El agente la calcula con lo que haya (ejemplos en `ACTUALIZAR.md`): `sed` + `awk` + `sha256sum` en Linux (ya probado, ver análisis 3.5), `shasum -a 256` en macOS, PowerShell o un script temporal de `python3` fuera de los repos en Windows. Siempre antes: **prueba de humo** con un texto en LF, el mismo con BOM y CRLF, y uno distinto | MD5 o SHA-1 (más débiles sin ventaja); hash del archivo crudo (CRLF y BOM darían falsos PERSONALIZADO); comparar contenido contra Git; un script versionado | SHA-256 está en todos los SO; la normalización cumple RF-19; la prueba de humo cumple RF-25 sin degradar a suposiciones; sin script versionado (RNF-1, RNF-8) |
| **(3) Marcador** `.agents/.kit-version`: una línea con la versión (`0.5.0`), copiada del archivo `VERSION` del kit usado al instalar (no escrita a mano); versionado en Git del proyecto; fuera de `template/` y de los manifiestos | Archivo en la raíz del proyecto (ensucia la raíz); campo dentro de `AGENTS.md` (grupo B, lo edita el usuario); ponerlo en `template/` (se distribuiría con la versión equivocada y se clasificaría) | `.agents/` ya es la carpeta de configuración de agentes y sale en Git; no lo confunde el `.gitignore` del kit (no ignora `.agents/`); el nombre oculto indica que no se edita |
| **(4) Registro "mantener"** `.agents/.kit-mantener`: archivo de texto sin extensión, una línea por decisión `ruta o ruta › elemento \| clase \| huella del destino al decidir \| versión destino \| AAAA-MM-DD`. Se vuelve a proponer solo si la huella del destino actual difiere de la guardada. Para elementos del grupo B la huella es la del texto del elemento tomado de `template/`. Incluye NUEVO borrados a propósito | Guardarlo dentro del marcador (rompe "una sola línea", RF-2); en `napkin.md` (grupo C, lo cura el agente y tiene tope de 10 ítems); en `MEMORY.md` (solo con visto bueno); un `.md` (E11 y E15 lo revisarían y rutas borradas a propósito harían fallar E11) | Es del procedimiento, versionable y compartido; la huella responde "¿cambió el kit?" sin Git; sin extensión queda fuera de E11/E15 |
| **(5) Documento** `ACTUALIZAR.md` en la raíz, junto a `INSTALAR.md`. En `INSTALAR.md`: un paso previo "¿ya tiene el kit?" (marcador o `.agents/skills/sdd/SKILL.md`) que remite y detiene la instalación (RF-22); paso 6 escribe el marcador desde `VERSION` solo en **Modo proyecto** (RF-13); "A mano" agrega `printf '%s\n' <versión> > .agents/.kit-version` y su equivalente en PowerShell; enlace en el encabezado | Anexarlo como sección de `INSTALAR.md` (mezcla dos procedimientos con reglas opuestas sobre sobrescribir); un comando `/sdd-actualizar` (fuera de alcance, RF-18) | Un documento por procedimiento evita que el agente aplique la regla equivocada |
| **(6) Otros documentos.** `template/AGENTS.md`: una línea en "Reglas duras". `AGENTS.md` raíz: regla de negocio y excepción RNF-9 en la línea de lint. `README.md`: árbol, Instalación y regla vieja. `CHANGELOG.md`: entradas dentro de 0.5.0 (sin versión nueva). `VERSION`: sin cambio. `sync-agents.md`: sin cambio. `validar-estructura.md`: E20 (kit) más E11 aclarado. E20 recalcula las huellas de `template/` y las compara con `manifiestos/<VERSION>.md` (falla por diferencia, archivo sin fila o fila sin archivo) | No tocar `template/AGENTS.md` (el ADR 007 pide distinguir instalar de actualizar); subir la versión a 0.5.1 (0.5.0 no se publicó); ignorar el desfase de manifiestos | El chequeo vuelve verificable el criterio "el manifiesto es consistente"; mínimo cambio en el grupo B para no sumar faltantes a los proyectos |
| **(7) Generación de manifiestos.** 0.4.0: `git archive 861bb99 template` extraído en `mktemp -d` fuera del repo, huellas con el algoritmo (2) y verificación contra `git ls-tree` (35 archivos). 0.5.0: al final, con `template/` congelado, sobre el árbol de trabajo; se verifica con E20. Todo template editado después obliga a regenerar | Generar 0.5.0 al empezar (queda obsoleto con cada edición); manifiesto de 0.4.0 a mano; guardar el contenido de 0.4.0 en el kit | La 0.4.0 solo se puede reconstruir desde ese commit y se hace una única vez; la 0.5.0 se hace cuando `template/` ya no cambia |
| **(8) Constitución y ADR 007** | — | **Hecho:** principio 5 enmendado y ADR 007 aceptado antes de este plan. ADR 008 aceptado el 2026-10-25: [formato de manifiestos, huella y marcador](../../004_adr/008_FormatoManifiestosHuellaMarcador.md) |
| **(9) No correr sobre el clon del kit** | Soportar el clon como destino | Es la deuda 001 (fuera de alcance). El procedimiento se detiene si el destino tiene `template/`, `VERSION` y `manifiestos/`. Modo mantenimiento no escribe marcador (pendiente de confirmar, análisis 7.1) |
| **(10) Faltantes del grupo B** por identidad del elemento contra el `template/` del destino, con el `CHANGELOG.md` como guía; lista conservadora | Reconstruir `AGENTS.md` de la base (sin contenido en los manifiestos) | Sin Git no existe la base; sobrereportar es seguro y el usuario lo resuelve con "mantener" (pendiente de confirmar, análisis 7.2) |
| **(11) Grupo de una ruta nueva sin regla**: se trata como grupo C (solo si falta) | Tratarla como A | RNF-4: lo desconocido se respeta |

## 7. Fases y estimado
| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | Preparación: fijar la huella y la prueba de humo (Linux ya probado; revisar macOS y Windows con los ejemplos). Generar `manifiestos/0.4.0.md` desde `861bb99` (extracción fuera del repo, 35 filas verificadas contra `git ls-tree`) | RF-17, RF-19, RF-25 | 1.5 |
| F1 | Redactar `ACTUALIZAR.md` completo (precondiciones, huella, base/destino, clasificación, grupos, plan y confirmación, aplicación, mantener, marcador, reporte, enlaces, commit, idempotencia, conflictos, ejemplos por SO) | RF-1 a RF-12, RF-14 a RF-16, RF-18 a RF-21, RF-23 a RF-25 | 4.0 |
| F2 | Cambios en `template/`: `AGENTS.md`, `.agents/README.md`, `validar-estructura.md` (E20, E11). Refrescar copia local de `.agents/` | RF-13, RF-17, RF-18, RF-21, RF-22, RNF-9 | 1.5 |
| F3 | Docs de la raíz: `INSTALAR.md`, `README.md`, `AGENTS.md` raíz, `CHANGELOG.md` | RF-8, RF-13, RF-17, RF-18, RF-22, RNF-9 | 2.0 |
| F4 | Congelar `template/` y generar `manifiestos/0.5.0.md` (41 filas); correr E20 y verificar que detecta una edición y la restauración | RF-17 | 1.0 |
| F5 | Validación en `mktemp -d`: (a) ensayo de instalación sin pisar nada y marcador escrito; (b) proyecto simulado 0.4.0 con `git archive 861bb99` fuera del repo, sin marcador y con personalizaciones (un A editado, un A borrado, `AGENTS.md` sin fusionar, un huérfano, un archivo en CRLF, un archivo en una ruta ocupada); (c) actualización a 0.5.0 con una copia del kit sin `.git`; resultado esperado del análisis 3.4; (d) segunda corrida "al día"; aprobación parcial y rechazo total sin mover el marcador; interrupción y retoma; marcador malformado, mayor y destino sin manifiesto; entorno sin SHA-256; "mantener" y re-propuesta tras un cambio; RETIRADO sintético; (e) chequeos de lint de `AGENTS.md` › Comandos en cada archivo tocado; `/sdd-estructura` E1–E20 | todos | 3.5 |
| F6 | Cierre: regenerar manifiesto si algo de `template/` cambió en F5, `seguridad-basica`, propuesta de ADR 008, índices (`002_specs`), deudas 001 y 002 (se citan, siguen abiertas), propuesta de commits por módulo | RF-16, RF-17 | 1.0 |
| | **Total** | | **14.5** |

Dependencias: F0 antes de F1 (el algoritmo se documenta tal como se probó); F4 después de F2 y F3 (el
manifiesto es lo último); F5 sobre F4; cualquier edición de `template/` posterior a F4 obliga a repetir F4
(E20 la detecta).

## 8. Criterios de aceptación
Mapeo a los criterios de finalización de la spec:

| Criterio de la spec | Cómo se acepta |
|---|---|
| Proyecto 0.4.0 sin marcador se actualiza a 0.5.0 | F5 (b)(c): 6 NUEVO, 8 KIT-VIEJO reemplazados tras confirmación, 19 IGUAL, personalizados intactos, `AGENTS.md` con faltantes listados, marcador `0.5.0` de una línea (cuentas del análisis 3.4, ajustadas a lo que quede en F2/F3) |
| Segunda ejecución "al día" | F5 (d): sin cambios y sin escribir nada |
| Instalación nueva deja marcador; parcial o rechazo no lo mueven | F5 (a) y (d) |
| Existen manifiestos 0.4.0 (desde `861bb99`) y 0.5.0, consistentes y verificables | F0, F4: 35 y 41 filas; E20 sin fallos |
| Huella y normalización definidas; CRLF y LF dan la misma | F0 y F5: prueba de humo; texto distinto da otra |
| Procedimiento documentado aparte y enlazado desde la instalación | F1, F3: `ACTUALIZAR.md`, enlace y paso previo en `INSTALAR.md` |
| Constitución enmendada y ADR existe | Hecho (principio 5; ADR 007) |
| `VERSION` y `CHANGELOG.md` de 0.5.0 reflejan el requerimiento | F3, F6 |
| El ensayo de instalación no pisa nada | F5 (a) |
| Chequeos de `AGENTS.md` › Comandos pasan; manifiestos exceptuados de rutas citadas | F5 (e); excepción escrita en `AGENTS.md` raíz y E11/E20 |
| Probado en directorio temporal y sin historial de Git del kit | F5 (b)(c): copia del kit sin `.git` como fuente del destino |
