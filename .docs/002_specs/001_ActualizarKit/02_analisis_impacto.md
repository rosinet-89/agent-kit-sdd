# 02 — Análisis de impacto: Actualizar un proyecto a una versión nueva del kit

## 1. Resumen del requerimiento
Hoy el kit solo sabe instalarse (copia lo que falta, nunca sobrescribe). Este requerimiento agrega un
procedimiento de **actualización** de un proyecto que ya tiene el kit: marcador de versión en el
proyecto, manifiestos de huellas por versión en el kit, clasificación de cada archivo (NUEVO, RETIRADO,
IGUAL, KIT-VIEJO, PERSONALIZADO) y reemplazo confirmado solo de KIT-VIEJO. Va dentro de 0.5.0 (sin
publicar). Primer salto soportado: 0.4.0 a 0.5.0. Spec aprobada (`01_spec.md`, 25 RF, 9 RNF).

### 1.1 Antecedentes
- Índice `.docs/002_specs/README.md`: la única fila es esta misma carpeta (`001_ActualizarKit`).
  Sin otros antecedentes en el índice. Confirmación:
  `grep -rn "actualiz\|manifiest\|kit-version" .docs/002_specs/README.md` solo devuelve la fila 001 y una línea de reglas del propio índice (verbo "se actualiza"), sin otra carpeta.
- ADR vigentes que condicionan el diseño: 001 (modo mantenimiento: la parte local del clon puede
  desactualizarse), 004 (solo Markdown, sin scripts; un script temporal fuera del repo sí se permite) y
  007 (reemplazo solo de KIT-VIEJO con confirmación; **ya existe**). La enmienda del principio 5 de la
  constitución **ya está hecha** (`.docs/001_constitucion/constitution.md`, principio 5).

### 1.2 Deuda técnica relacionada
- `.docs/003_deuda/001_ActualizarCloneDelKit.md` (Resuelta, 2026-10-25, en `INSTALAR.md` › Modo
  mantenimiento): el clon sigue **fuera de alcance** de esta spec. Lo que sí hereda: el procedimiento no
  debe correr sobre el propio clon y `/sdd-estructura` E19 sigue siendo el detector de desfase del clon.
- `.docs/003_deuda/002_FlujoSddNoAplicadoEn050.md` (Resuelta, 2026-10-25, por `002_Kit050`): 0.5.0 se
  registró en una spec retroactiva. Este requerimiento no la resolvió. Dato útil: las diferencias 0.4.0
  a hoy en `template/` (sección 3) son el material de la prueba.
- Sin otra deuda abierta del módulo (`.docs/003_deuda/README.md`).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica. No hay comando nuevo (RF-18): el usuario pide al agente "actualiza el proyecto X siguiendo
`ACTUALIZAR.md`", igual que hoy pide instalar con `INSTALAR.md`.

### 2.2 Tablas (existentes y nuevas)
No aplica (sin base de datos).

### 2.3 Archivos de código (ruta, capa)
El kit es solo Markdown. Capas según constitución (principio 3) y ADR 001/002:

| Archivo | Capa | Acción |
|---|---|---|
| `ACTUALIZAR.md` (raíz del kit) | Docs de la raíz (manual del kit) | Crear: procedimiento, huella, clasificación |
| `manifiestos/0.4.0.md`, `manifiestos/0.5.0.md` (raíz del kit) | Docs de la raíz, **no se distribuyen** | Crear |
| `INSTALAR.md`, `README.md`, `CHANGELOG.md`, `VERSION` | Docs de la raíz | Modificar (VERSION ya dice 0.5.0) |
| `AGENTS.md` (raíz) | Conocimiento propio versionado | Modificar (regla de copia, excepción RNF-9) |
| `template/AGENTS.md` | Producto | Modificar (una regla sobre actualizar) |
| `template/.agents/README.md` | Producto | Modificar (árbol: marcador y registro "mantener") |
| `template/.agents/validar-estructura.md` | Producto | Modificar (E20, E11, texto "E1–E20") |
| `.docs/002_specs/README.md` | Conocimiento propio | Actualizar fila al cerrar |

### 2.4 Código legado o sin uso (no tocar, sí documentar)
- `README.md` raíz, sección "Reglas del kit": dice "compara su `.agents/` con el del kit; la copia
  nunca sobrescribe". Queda obsoleta con este requerimiento y se reescribe.
- `INSTALAR.md` › "A mano": usa `cp -Rn` (el napkin ya advierte que en GNU coreutils 9.x avisa
  "non-portable"; se deja como está, fuera de alcance, salvo agregar el paso del marcador).
- La copia local de `.agents/` de la raíz (excluida en `.git/info/exclude`) no es entregable; se refresca
  a mano tras editar `template/` (deuda 001).

## 3. Datos reales (consultas de solo lectura, sin base de datos)
Todas el 2026-10-24, en `/home/cromero/dev/agent-kit-sdd`, con `find`, `git` y `comm` (solo lectura).

**3.1** `find template -type f | sort` → **41 archivos** (hoy). `git ls-tree -r --name-only 861bb99 -- template`
→ **35 archivos** en 0.4.0. Son 6 nuevos y ninguno eliminado
(`comm -13` / `comm -23` de ambas listas).

**3.2** `git diff --stat 861bb99 HEAD -- template/` → 15 archivos, +331 −8:
- Nuevos (A en Git): `sdd-estructura.md`, `skills/adr/SKILL.md`, `skills/migracion-bd/SKILL.md`,
  `skills/seguridad-basica/SKILL.md`, `validar-estructura.md`, `.docs/004_adr/README.md`.
- Modificados: `.agents/README.md`, `agents/coordinator.md`, `agents/reviewer.md`, `commands/sdd-cierre.md`,
  `commands/sdd-plan.md`, `skills/git-commit/SKILL.md`, `skills/sdd/SKILL.md`, `sync-agents.md`, `AGENTS.md`.

**3.3** Clasificación por grupo de los 41 archivos de hoy (regla por ruta):

| Grupo | Archivos | N |
|---|---|---|
| A (kit puros: `.agents/**` salvo `napkin.md`) | 4 agentes, 14 comandos, `README.md`, `preferencias.ejemplo.md`, `sync-agents.md`, `validar-estructura.md`, 10 de skills (`adr`, `formato-respuesta`, `git-commit`, `migracion-bd`, `napkin/LICENSE`, `napkin/SKILL.md`, `sdd/avance-y-tiempos.md`, `sdd/plantillas.md`, `sdd/SKILL.md`, `seguridad-basica`) | 32 |
| B (fusión manual) | `AGENTS.md`, `CLAUDE.md`, `.gitignore` | 3 |
| C (propios, solo si faltan) | `MEMORY.md`, `.agents/napkin.md`, `.docs/001_constitucion/constitution.md`, `.docs/002_specs/README.md`, `.docs/003_deuda/README.md`, `.docs/004_adr/README.md` | 6 |
| D (personales) | `PREFERENCIAS.md`: no existe en `template/` (solo la plantilla `preferencias.ejemplo.md`, que es A) | 0 |

32 + 3 + 6 = 41. En 0.4.0 eran 27 A + 3 B + 5 C = 35.

**3.4** Resultado esperado del salto 0.4.0 a 0.5.0 sobre un proyecto sin tocar (sirve de oráculo para la
validación): NUEVO = 6 (5 de grupo A + `.docs/004_adr/README.md`); KIT-VIEJO = 8 (los A modificados);
IGUAL = 19 (A sin cambios); RETIRADO = 0; grupo B: `AGENTS.md` con faltantes (3 filas de skills,
`/sdd-estructura`, paso 7 ampliado, y lo que agregue este requerimiento), `CLAUDE.md` y `.gitignore`
sin cambios.

**3.5** Prueba de la huella (en `/tmp`, fuera del repo): con
`sed` (quita BOM, `\r` final y espacios/tabuladores finales) + `awk` (quita líneas vacías finales) +
`sha256sum`, un archivo con LF y espacios finales y otro con BOM + CRLF dieron **la misma huella**
(`911169dd…59a2`) y uno con contenido distinto dio otra (`b72cf6d7…2750`). Confirma que la normalización
de la spec es calculable con herramientas estándar de Linux; falta confirmarla en macOS y Windows.
`python3` está disponible aquí; `pwsh` no.

## 4. Flujo actual
- **Instalar:** `INSTALAR.md` elige modo (proyecto o mantenimiento), copia `template/` sin sobrescribir,
  fusiona a mano `AGENTS.md`, `CLAUDE.md`, `MEMORY.md` y `.gitignore`, y manda al usuario a `/sdd-init`.
  No deja ningún dato de versión en el proyecto. Si el destino ya tiene el kit, lo trata como "proyecto
  existente": copia lo que falta y omite lo demás (queda a medias).
- **Actualizar:** no existe. `README.md` raíz dice "compara su `.agents/` con el del kit".
- **Versión:** `VERSION` (0.5.0) y `CHANGELOG.md` en la raíz del kit; sin etiquetas Git.
- **Chequeos:** `/sdd-estructura` (`validar-estructura.md`) con E1 a E19; E11 valida rutas citadas en
  `AGENTS.md`, `.agents/**/*.md` y `.docs/**/README.md`; E19 compara `template/.agents` con `.agents`.

## 5. Reglas de negocio: conocidas vs código
| Regla (spec / ADR) | Estado en el código |
|---|---|
| La copia nunca sobrescribe | Vigente en `INSTALAR.md`; la constitución ya tiene la excepción (ADR 007); `AGENTS.md` raíz › Reglas de negocio dice "La instalación copia sin sobrescribir nunca": sigue cierto, pero hay que precisar que la actualización es otra cosa |
| Manifiestos no se distribuyen | Cumplible: `manifiestos/` en la raíz, fuera de `template/`. `AGENTS.md` raíz ya dice que lo distribuible vive en `template/` y docs de la raíz |
| Marcador "no pertenece al kit" | Cumplible: lo escribe el procedimiento, no está en `template/` (`find` de 3.1) |
| E11 "rutas citadas existentes" | Alcance de E11 = `AGENTS.md`, `.agents/**/*.md`, `.docs/**/README.md`. `manifiestos/` y `ACTUALIZAR.md` quedan **fuera** de ese alcance por su ubicación; RNF-9 se cumple documentándolo, no cambiando la ruta |
| E19 `diff -rq template/.agents .agents` solo muestra `napkin.md` | Sigue válido si el marcador **no** se escribe en el clon (ver pregunta abierta 1) |
| Sin scripts versionados (ADR 004) | Cumplible: los comandos de huella van como ejemplos en `ACTUALIZAR.md`, igual que `INSTALAR.md` ya trae ejemplos de `cp`/`robocopy` |
| El kit no dice dónde queda el entorno con 0.1.0–0.3.0 | Sin contenido recuperable (spec); se trata con "B no existe" |

## 6. Riesgos y casos borde
1. **Huella con normalización.** Si el agente la calcula distinto en cada SO, un archivo idéntico sale
   PERSONALIZADO (falso positivo, seguro) o, peor, un archivo cambiado sale KIT-VIEJO (falso negativo,
   pisa trabajo). Mitigación: definición única y exacta en `ACTUALIZAR.md`; **prueba de humo obligatoria**
   antes de calcular (CRLF vs LF vs BOM deben dar la misma huella, y un texto distinto otra); si falla, se
   detiene (RF-25). Pendiente: probar los ejemplos de macOS y PowerShell (aquí solo hay Linux).
2. **Clasificación errónea que pise trabajo.** Reemplazar exige P = B exacta, diferencia mostrada y
   confirmación (ADR 007). Ante duda, PERSONALIZADO (RNF-4). Mitigación extra: en proyecto con Git, aviso
   de árbol sucio (RF-16) y el cambio entra en un commit aparte, reversible; sin Git, respaldo opcional de
   lo reemplazado en una carpeta temporal fuera del proyecto.
3. **Manifiestos desactualizados.** Quien edite `template/` después de generar `manifiestos/0.5.0.md`
   deja un manifiesto que miente (un archivo cambiado figuraría como IGUAL). Detección: nuevo chequeo
   **E20** (solo en el kit): recalcula la huella de cada archivo de `template/` y la compara con el
   manifiesto de la versión de `VERSION`; también detecta archivos de `template/` sin fila y filas sin
   archivo. Además el procedimiento hace la misma verificación como **precondición** del destino antes
   de proponer nada (si no coincide, avisa y no modifica). El manifiesto de 0.5.0 se genera lo último,
   tras congelar `template/`.
4. **Interacción con el modo mantenimiento (ADR 001).** El procedimiento no debe correr sobre el propio
   clon (tiene `template/`, `VERSION`, `manifiestos/`): se detiene y remite a la deuda 001. En modo
   mantenimiento no se escribe el marcador (propuesta; ver pregunta 1). `ACTUALIZAR.md` y `manifiestos/`
   no están en `template/`, así que no llegan a la copia local `.agents/` ni a proyectos.
5. **Chequeos de `/sdd-estructura`.** E11: sin riesgo por ubicación, pero se deja escrito que no aplica a
   manifiestos (RNF-9). Si `template/AGENTS.md` cita el marcador o el documento de actualización con
   comillas inversas, E11 corre en el proyecto y `ACTUALIZAR.md` no existe allí: **no citarlo con comillas
   inversas** en archivos que se distribuyen. E15 (BOM): los manifiestos son `.md` y entran. E19: el
   registro "mantener" y el marcador viven en el proyecto, no en `template/`.
6. **Rutas retiradas en manifiestos (RNF-9).** Ningún archivo se retiró entre 0.4.0 y 0.5.0 (3.1), así
   que hoy no hay caso real; el caso RETIRADO solo se prueba con un archivo sintético en la validación.
7. **Grupo B sin contenido de la base.** Los manifiestos guardan huellas, no contenido; sin Git no hay
   forma de ver cómo era `AGENTS.md` en 0.4.0. Por eso "faltante = agregado desde la base" no se puede
   calcular exacto. Mitigación: se compara el destino contra el proyecto por identidad del elemento, se
   usa el `CHANGELOG.md` como guía de qué se agregó y, ante duda, se lista como faltante (el usuario lo
   incorpora o lo marca "mantener"). Puede sobrereportar; nunca oculta.
8. **"Mantener" y cambios del kit.** Registrar solo el nombre no basta: hay que saber si el kit cambió el
   elemento. Se guarda la huella del destino al decidir; si en una versión posterior cambia, se vuelve a
   proponer. Para elementos de grupo B (una fila, un paso) la huella es la del texto del elemento tomado
   del `template/` disponible.
9. **Marcador mal manejado.** Si se avanza con pendientes, el proyecto queda "al día" sin estarlo.
   Mitigación: solo avanza si no hay pendientes (RF-12); un proyecto con marcador y sin pendientes
   registrados sale "al día" sin tocar nada.
10. **Rutas ocupadas, enlaces simbólicos y mayúsculas/minúsculas.** `.claude/skills` suele ser un enlace
    a `.agents/skills`: las huellas del proyecto se leen siempre por la ruta real de `.agents/`, nunca
    por las carpetas de herramienta. Conflictos de tipo (RF-23) se informan según los estados de
    `sync-agents.md`, sin tocar.
11. **Cumplimiento de la spec en la propia 0.5.0.** Quien reciba 0.5.0 por primera vez ya tendrá el
    marcador solo si instala de cero; los que vienen de 0.4.0 pasan por RF-3 (suposición confirmada).
12. **Entorno sin sha256.** RF-25: se informa y no se modifica nada; el agente puede generar un script
    temporal fuera de los repos (RNF-8).

## 7. Preguntas abiertas
1. ¿El **modo mantenimiento** también escribe el marcador en el clon? Propuesta: **no** (el clon se
   actualiza con la deuda 001 y E19 ya detecta su desfase; escribirlo daría una versión que puede mentir).
2. Faltantes del grupo B: dado que sin Git no se conoce el contenido de la base, ¿se acepta la lista
   conservadora (todo lo que el destino tiene y el proyecto no, resuelto por el usuario con incorporar o
   "mantener")? Propuesta: **sí**.
3. ¿Se registra un ADR 008 (formato de manifiestos, huella y marcador) al cerrar? Propuesta: **sí**,
   solo propuesto; no se crea sin visto bueno.
