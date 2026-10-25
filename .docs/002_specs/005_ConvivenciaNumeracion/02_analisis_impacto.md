# 02 — Análisis de impacto: Convivencia de la numeración de 3 y 4 dígitos

## 1. Resumen del requerimiento
Definir cómo conviven los ítems de 3 y 4 dígitos en un proyecto que actualiza a 0.5.0: un ítem nuevo conserva el ancho de su carpeta, los números se comparan por valor, nunca se renumera, la regla vive en « Numeración única » y la actualización avisa (por nombres de carpeta, sin leer los README) y ofrece el texto sin escribir sin confirmación. Spec aprobada: `01_spec.md` (RF-1 a RF-20, RNF-1 a RNF-8).

### 1.1 Antecedentes
- Consulta del índice de planes (`.docs/002_specs/README.md`, 2026-10-25 15:34, `grep -n -i "numeraci\|dígitos"`): ninguna spec trata la convivencia de anchos. La fila 005 es esta carpeta. `002_Kit050` y `003_OptimizarArnes` mencionan la numeración como contexto (la 003 dejó la diferencia 4 dígitos en un proyecto y 3 en el clon como única divergencia entre los dos `AGENTS.md`). No hay nada que reutilizar ni que contradecir.
- Coordinación (no son antecedentes de numeración):
  - `004_ModeloYPermisosAgentes` (spec aprobada, con `02`, `03` y `04` ya escritos): comparte `CHANGELOG.md`, `template/AGENTS.md`, `template/.agents/skills/sdd/SKILL.md`, `AGENTS.md` de la raíz y `manifiestos/0.5.0.md` (detalle en la sección 3.3).
  - `006_RutaSiNoDelega` (en análisis): su alcance aún no está definido; si toca comandos o el coordinator, aplica la misma regla de ediciones en serie.

### 1.2 Deuda técnica relacionada
- `.docs/003_deuda/004_ConvivenciaNumeracion3y4Digitos.md` (estado En atención, atendida por esta spec). La resuelve: sus cuatro puntos de « Cómo se atendería » quedan cubiertos (regla, fuente única, E12 por valor, prueba en proyecto simulado). Al cerrar, `/sdd-cierre` la pasa a `Resuelta`.
- Ninguna otra deuda abierta toca numeración (consultado en `.docs/003_deuda/README.md`). La deuda 003 es de la `004_ModeloYPermisosAgentes`.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica. El kit es solo Markdown: no hay pantallas ni menú.

### 2.2 Tablas
No aplica. Sin base de datos.

### 2.3 Archivos (ruta, capa)
Capa Producto = `template/` (se distribuye). Capa Docs raíz = manual del kit (no se distribuye). Lista cerrada; el detalle de líneas está en la sección 3.1.

| Archivo | Capa | Qué cambia |
|---|---|---|
| `template/.docs/002_specs/README.md` | Producto (grupo C en un proyecto) | Sección « Numeración única » (fuente única) y encabezado `NNN` de la tabla |
| `template/.docs/003_deuda/README.md` | Producto (grupo C) | Remite a la fuente; `NNN`/`NNNN` unificados |
| `template/.docs/004_adr/README.md` | Producto (grupo C) | Remite a la fuente; `NNN`/`NNNN` unificados |
| `template/AGENTS.md` | Producto (grupo B) | « Deuda técnica » (placeholder), « Formato y numeración », nota de `sdd` |
| `template/.agents/README.md` | Producto (grupo A) | Árbol de carpetas |
| `template/.agents/skills/sdd/SKILL.md` | Producto (grupo A) | Descripción, árbol, línea de ancho y ejemplo |
| `template/.agents/skills/adr/SKILL.md` | Producto (grupo A) | Descripción, paso 2, rutas y plantilla |
| `template/.agents/skills/git-commit/SKILL.md` | Producto (grupo A) | Fase 2, punto 4 |
| `template/.agents/skills/migracion-bd/SKILL.md` | Producto (grupo A) | Descripción y encabezado del SQL |
| `template/.agents/commands/sdd-spec.md` | Producto (grupo A) | `argument-hint` y paso 2 |
| `template/.agents/commands/sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-cierre.md`, `sdd-change.md`, `sdd-status.md` | Producto (grupo A) | Solo el `argument-hint` |
| `template/.agents/validar-estructura.md` | Producto (grupo A) | Fila E12 y pasos 1 a 3 (estado AVISO) |
| `ACTUALIZAR.md` | Docs raíz | Aviso de numeración, registro «mantener», marcador al cerrar, índice |
| `CHANGELOG.md` | Docs raíz | Dos líneas del bloque 0.5.0 |
| `manifiestos/0.5.0.md` | Docs raíz | Regenerado al final |
| `AGENTS.md` (raíz) y `.docs/002_specs/README.md`, `.docs/004_adr/README.md` (raíz) | Mantenimiento del clon | Frases que afirman « en un proyecto el ancho es de 4 » |

Cambian 20 de los 41 archivos de `template/` (10 de piezas con texto + 10 comandos, 9 de ellos solo en el `argument-hint`).

### 2.4 Código legado o sin uso (no tocar, sí documentar)
- Los README de `.docs/` de un proyecto ya instalado (grupo C) conservan la regla de 0.4.0 y no se tocan por la actualización; solo se ofrece el texto.
- Los ítems existentes (`017_…`, `003_…`) no se renombran.
- `template/.agents/skills/sdd/plantillas.md` (`COM-NNN`, ticket) y `NN_<tipo>_<desc>.sql` (orden de scripts) no son numeración de ítems: no se tocan.
- `ACTUALIZAR.md`: no cambia la clasificación de grupos ni E20 (spec, « Fuera de alcance »).

## 3. Datos reales (consultas de solo lectura, 2026-10-25 15:34)
Origen: el árbol de trabajo de `/home/cromero/dev/agent-kit-sdd`. Sin base de datos.

### 3.1 Piezas con ancho de numeración
Comando: `grep -rnE "NNNN?|0001|[34] dígitos|dos dígitos" template --include=*.md` (se descartó `NN_<tipo>` del SQL). Resultado agrupado:

| Archivo | Líneas | Qué afirma hoy |
|---|---|---|
| `template/.docs/002_specs/README.md` | 25, 46, 49, 53-55, 59 | Encabezado `NNN`; secciones en 3; « consecutivo de 4 dígitos en un proyecto »; ejemplos `0001_…` |
| `template/.docs/003_deuda/README.md` | 5, 7, 43, 49, 58, 73-74 | `NNNN_<Nombre>.md`, « 4 dígitos en un proyecto »; tabla y plantilla con `NNN` |
| `template/.docs/004_adr/README.md` | 3, 13, 16, 20 | `NNNN_<Titulo>.md`, « 4 dígitos en un proyecto »; `Reemplazado por NNN`, tabla con `NNN` |
| `template/AGENTS.md` | 41, 50, 51, 69 | `NNNN_<Nombre>.md`; secciones en 3; « prefijo de 4 dígitos (`0001`) »; `<NNNN>` y « 4 dígitos, `0001` » en la nota de `sdd` |
| `template/.agents/README.md` | 29, 31, 33 | Árbol con `<NNNN>` y « 4 dígitos en un proyecto » |
| `template/.agents/skills/sdd/SKILL.md` | 7, 35, 47, 48 | `<NNNN>`; « 4 dígitos en un proyecto (`0001`) »; ejemplo `0017_ReporteVentasPorZona` |
| `template/.agents/skills/adr/SKILL.md` | 3, 35-37, 44, 46 | `NNNN_<Titulo>.md`, « 4 dígitos en un proyecto, `0001` »; plantilla con `NNNN` |
| `template/.agents/skills/git-commit/SKILL.md` | 49 | « 4 dígitos en un proyecto (`0001`) » (Fase 2, punto 4) |
| `template/.agents/skills/migracion-bd/SKILL.md` | 3, 28 | `<NNN>_<Feature>` en la descripción y `<NNNN_Feature>` en el encabezado: dos anchos en la misma pieza |
| `template/.agents/commands/sdd-spec.md` | 3, 13 | `argument-hint` con `<NNNN_…>`; « En un proyecto el ancho es de 4 dígitos (`0001`) » |
| `template/.agents/commands/sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-cierre.md`, `sdd-change.md`, `sdd-status.md` | 3 en cada uno | `argument-hint` con `<NNNN_NombreFeature>` |
| `template/.agents/validar-estructura.md` | 29 | E12: « Ancho: 4 dígitos en un proyecto (`0001`)… 3 dígitos en el clon » |

Raíz del kit (comando: `grep -nE "NNNN?|0001|[34] dígitos|dígitos" ACTUALIZAR.md INSTALAR.md README.md CHANGELOG.md AGENTS.md`):
- `CHANGELOG.md:22-25`: la línea « Numeración en un proyecto… pasa a 4 dígitos… esa regla de convivencia no está definida todavía ».
- `AGENTS.md` (raíz) `:51` y `:67`: « en un proyecto el ancho es de 4 (`0001`) » y « en un proyecto, 4 ». Son del clon y siguen en 3; solo esas frases afirman el ancho del proyecto.
- `ACTUALIZAR.md`, `INSTALAR.md`, `README.md`: sin mención de ancho de numeración.
- `.docs/002_specs/README.md:52-53` y `.docs/004_adr/README.md:3,12` del clon: repiten « en un proyecto 4 dígitos ».

### 3.2 Estado de 0.5.0 y de E20
- `VERSION` = `0.5.0`; `CHANGELOG.md` tiene el bloque `## 0.5.0 — 2026-10-24` (sin publicar).
- `manifiestos/0.5.0.md`: 41 filas. `template/` tiene 41 archivos.
- E20 recalculado a mano con el algoritmo de `ACTUALIZAR.md` (BOM, CRLF, espacios finales, líneas vacías finales; SHA-256), con un script de Python de solo lectura: 41 archivos, 41 filas, 0 diferencias, 0 filas sin archivo. `git status --short template manifiestos CHANGELOG.md ACTUALIZAR.md` no lista cambios: hoy E20 pasa y todo cambio a `template/` lo deja en rojo hasta regenerar.

### 3.3 Archivos que comparte con la `004_ModeloYPermisosAgentes`
Fuente: `004_…/03_plan.md` §2 (tabla de archivos) y `01_spec.md`.

| Archivo | Qué toca la 004 | Qué toca esta spec | Solapa |
|---|---|---|---|
| `template/AGENTS.md` | Solo « Qué lee cada rol » | « Deuda técnica » (l. 41), « Formato y numeración » (l. 50-51), nota de `sdd` en « Skills de proyecto » (l. 69) | No: secciones distintas. El riesgo es de reordenamiento |
| `AGENTS.md` (raíz) | « Qué lee cada rol » | L. 51 y 67 (alineación mínima) | No, secciones distintas |
| `template/.agents/skills/sdd/SKILL.md` | Una frase junto al enlace de `avance-y-tiempos.md` | Descripción (l. 7), árbol (l. 35), línea de ancho (l. 47-48) | Mismo archivo, líneas distintas |
| `CHANGELOG.md` | Un ítem nuevo en 0.5.0 | Reescribe las líneas 22-25 de 0.5.0 | Mismo bloque, líneas distintas |
| `manifiestos/0.5.0.md` | Regenerado entero (F7 de su plan) | Regenerado entero | Sí: quien cierre segundo lo regenera |
| `template/.agents/commands/sdd-validate.md` | Paso 5 | Solo `argument-hint` (l. 3) | Mismo archivo, líneas distintas |

Los agentes (`coordinator`, `planner`, `implementer`, `reviewer`) y la nueva pieza `restricciones-de-rol.md` son solo de la 004: esta spec no los toca.

## 4. Flujo actual
1. Un agente asigna un número: `sdd-spec`, la skill `adr` o la regla de deuda calculan « prefijo mayor + 1 » mirando la copia local y la rama remota ya descargada (`git ls-tree --name-only @{u}`), con « 4 dígitos en un proyecto ». No dicen cómo comparar anchos distintos.
2. Revisión de duplicados: `git-commit` (Fase 2), el inicio de sesión y E12 comparan prefijos; el criterio de coincidencia es « mismo prefijo » y « otro nombre ».
3. Actualización de un proyecto de 0.4.0 a 0.5.0 (`ACTUALIZAR.md`): los grupos A y B se actualizan; los README de `.docs/` (grupo C) no se comparan ni se reemplazan. El proyecto queda con comandos y skills que dicen 4 dígitos y README que dicen la regla de 0.4.0.
4. No hay aviso al usuario ni regla de convivencia. El `CHANGELOG.md` de 0.5.0 lo declara como no definido.

## 5. Reglas de negocio: conocidas vs código
| Regla | Conocida (spec) | Estado real hoy | Brecha |
|---|---|---|---|
| Ancho del ítem nuevo | Conserva el ancho de su carpeta (RF-1) | Se manda 4 en un proyecto | Reescribir en la fuente y poner la frase mínima en las piezas |
| Comparación | Por valor numérico (RF-4) | No se dice | Redactar en « Numeración única » |
| Carpeta vacía | Ancho del ítem de mayor valor entre las otras; 4 si ninguna (RF-2) | Se manda 4 | Redactar |
| Duplicado | Por valor; mismo nombre = probable mismo ítem (RF-7, RF-8) | Por prefijo y « otro nombre » | Reescribir paso 1 de la revisión de duplicados |
| Sin renumerar | Nunca por cambio de regla (RF-10) | No está escrito | Redactar |
| E12 | FALLA/AVISO por valor y ancho (RF-14) | Solo OK/FALLA con ancho 4 | Tercer estado en el procedimiento del reviewer |
| Aviso al actualizar | Por nombres de carpeta (RF-16, RF-17) | No existe | Sección nueva en `ACTUALIZAR.md` |
| CHANGELOG | Sin « no está definida todavía » (RF-18) | Frase vigente | Reescribir 2 líneas |

## 6. Riesgos y casos borde
| Riesgo | Efecto | Mitigación |
|---|---|---|
| Renumerar por error (un agente ve `017` junto a `0018` y propone « normalizar ») | Rompe citas desde otras specs, deudas y ADR | RF-10 en la fuente y en la frase mínima; la prueba cubre « mezcla sin renombrar » con `ls` idéntico antes y después |
| E12 con anchos mezclados | Falso fallo que bloquea cierres, o falso OK que oculta un duplicado por valor | Comparar por valor; AVISO para mezcla válida; FALLA para duplicado y ancho inválido; probar los tres |
| Frase mínima repetida que se desfasa | Seis piezas dicen casi lo mismo y una cambia | Redacción única y literal (decisión 1); verificación por `grep` de la frase exacta; sin cifras de ancho |
| Conflicto de edición con la 004 | Pisar líneas ajenas o dejar un manifiesto viejo | Ediciones en serie; releer el archivo en el momento de editar; ubicar por título de sección; manifiesto al final por quien cierre segundo |
| Manifiesto de 0.5.0 desactualizado | E20 en rojo, o un proyecto recibe huellas viejas | Regenerar al final y repetir E20; el conteo de 20 archivos cambiados se confirma contra la medición |
| `.docs/` de un proyecto (grupo C) no se actualiza | El README conserva la regla de 0.4.0 y contradice las piezas | Aviso por nombres con texto ofrecido; frase mínima en cada pieza; limitación declarada en RNF-2 |
| Aviso ruidoso | El usuario lo ve en cada actualización | Se oculta con «mantener» mientras no cambie el texto ofrecido |
| Proyecto de 0.4.0 sin ningún ítem | Sin aviso y con README viejo | Limitación aceptada en la spec (caso límite 12) |
| Remoto simulado en la prueba | Una prueba con `git` puede tocar el repo real | Todo en `mktemp -d`, con rutas absolutas y sin `cd` al repo real |
| Cambio de `template/` por la `006` mientras tanto | Edición cruzada | Releer el archivo antes de editarlo; la sesión principal serializa |

Casos borde de la spec (1 a 16) se cubren en la validación; ninguno requiere código.

## 7. Preguntas abiertas
Ninguna. El usuario resolvió: (1) esta spec se implementa después de que la `004` termine sus ediciones de `template/`, y la `006` se coordina aparte; (2) los placeholders son `<prefijo>` y los ejemplos no llevan número; (3) las frases del clon que afirman el ancho de un proyecto se alinean con un cambio mínimo que remite a la fuente.
