# 03 — Plan: Optimizar el arnés SDD
Spec: 01_spec.md (aprobada 2026-10-25 13:07)

## 1. Ubicación y capas
El kit es solo Markdown (constitución, principios 1, 3 y 6; ADR 004). El cambio vive en el
producto `template/` y en los docs de la raíz que el kit ya versiona (`AGENTS.md`, `CHANGELOG.md`,
`manifiestos/`). No hay capas de aplicación, pantallas ni base de datos.

Patrón: cada fase del flujo tiene una sola secuencia, la de su comando `/sdd-<fase>`. La skill
`sdd` solo asocia fase, documento y rol. El coordinator solo elige el comando. El arranque de
sesión sigue siendo de `AGENTS.md`. El contenido de cada documento sigue en `plantillas.md`. Las
reglas de tiempos siguen en `avance-y-tiempos.md`.

`AGENTS.md` es grupo B (ADR 007): se editan `template/AGENTS.md` y el de la raíz con el mismo texto
genérico. La única diferencia que se conserva es la de E12: 4 dígitos en un proyecto, 3 en este
clon. Esos cambios llegan a un proyecto ya instalado por fusión manual, no por reemplazo.

La deuda 003 (modelo y permisos del frontmatter) no se implementa. El archivo ya existe; no se
crea otro.

## 2. Archivos que se crean o modifican
Ningún archivo nuevo de agente o comando. La lista con ruta y el RF que obliga a tocarlo está en
`02_analisis_impacto.md` §2.3. Resumen por capa:

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/commands/sdd-implement.md` | Producto | Procedimiento del paso: lectura mínima, ampliaciones, respaldo de RF-1 | RF-1, RF-2, RF-16 |
| `template/.agents/agents/implementer.md` | Producto | Qué lee, qué hace y qué no hace. No reescribe el procedimiento | RF-1, RF-2, RF-12, RF-13, RF-21 |
| `template/.agents/commands/sdd-status.md` | Producto | Lee solo Estado, lista de la carpeta, cuatro secciones de `05_avance.md` y el conteo por búsqueda | RF-5 |
| `template/.agents/commands/sdd.md` | Producto | Elige el comando sin cargar skill ni contrato si la fase no los usa. La lista incluye `sdd-estructura` | RF-8, RF-15 |
| `template/.agents/commands/sdd-estructura.md` | Producto | Lo ejecuta la sesión principal. No lanza subagente | RF-18 |
| `template/.agents/validar-estructura.md` | Producto | Una frase en Alcance: la sesión principal corre E1–E20. E20 no se redefine | RF-18 |
| `template/.agents/commands/sdd-spec.md`, `sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-cierre.md`, `sdd-change.md` | Producto | Cada uno conserva su procedimiento y deja de copiarlo en otras piezas. Clarify muestra el cambio sin depender de `git diff`. Cierre localiza pasos no `[x]` en `05_avance.md`. Validate y flujo acotan el alcance. Plan y tasks apuntan a la plantilla | RF-4, RF-6, RF-9, RF-11, RF-14, RF-15, RF-16, RF-17, RF-19, RF-20 |
| `template/.agents/commands/sdd-init.md`, `sdd-constitucion.md` | Producto | Tope de 30 archivos o 150 KB, declarado en init y referenciado por constitucion. Sin meta del 70 % | RF-10 |
| `template/.agents/agents/coordinator.md` | Producto | Enruta. No lee constitución ni `MEMORY.md` para dirigir. No reescribe fases | RF-7, RF-14, RF-15, RF-16 |
| `template/.agents/agents/planner.md` | Producto | Por documento, solo el contexto que ese documento usa. La cadena de spec no relee lo que la sesión ya retuvo | RF-6, RF-9, RF-12, RF-13, RF-17 |
| `template/.agents/agents/reviewer.md` | Producto | Checklist del paso y criterios de los RF citados. No corre E1–E20. Alcance de RF-19. Clasifica el modo de seguridad | RF-3, RF-11, RF-12, RF-18, RF-19, RF-21 |
| `template/.agents/skills/sdd/SKILL.md` | Producto | Tabla fase → documento → rol, con enlace al comando. Sin la secuencia. La fila de estructura dice sesión principal | RF-14, RF-15, RF-18 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Producto | Dueño de pausa, formato de fila, quién escribe `05_avance.md`, leyenda de estados, mínimo 1 minuto y la frase exacta "sin trabajo real" | RF-16, RF-22 |
| `template/.agents/skills/seguridad-basica/SKILL.md` | Producto | Declara el modo "solo secretos" y cuándo aplica la skill completa. Una sola vez | RF-11 |
| `template/AGENTS.md` y `AGENTS.md` (raíz) | Producto (grupo B) y clon | Cada rol declara sus secciones. Los subagentes no repiten el arranque. Lo genérico no diverge | RF-12, RF-13, RF-15 |
| `CHANGELOG.md` | Docs raíz | Una línea en 0.5.0 | Criterio de finalización |
| `manifiestos/0.5.0.md` | Docs raíz | Regenerado al congelar `template/`, mismo algoritmo de huella | Criterio de finalización; no cambia ADR 008 |
| `.docs/002_specs/003_OptimizarArnes/05_avance.md` | Esta carpeta | Línea base y confirmación de coordinación. Lo escribe la sesión principal | Definición de línea base; requisito previo |

`plantillas.md` no se reescribe: los comandos de plan y tasks la referencian. `001_ActualizarKit`
no se reescribe. ADR 007, ADR 008, `ACTUALIZAR.md` y la definición de E20 no se editan.

## 3. Base de datos
No aplica. Sin `sql/`, sin DDL ni seed, sin rollback.

## 4. Seguridad
El cambio es documentación de agentes. No hay datos de proyecto, sesión ni scripts versionados.

- La búsqueda de secretos sigue en toda validación (RF-11). Si el cambio es solo documentación, el
  reviewer aplica el modo "solo secretos" de `seguridad-basica` y lo declara. Si toca código
  ejecutable (definición de la spec), datos, accesos o secretos, aplica la skill completa. Ante
  duda, la skill completa.
- No se modifican `mode`, `permissions` ni `model`. Eso queda en la deuda 003. RF-21 se verifica
  por lectura cruzada con `AGENTS.md` › Git y reglas duras.
- Cada edición parte del contenido actual del archivo. Prohibido `git checkout` y `git restore`
  sobre archivos con trabajo ajeno.
- Al regenerar el manifiesto no se cambia el algoritmo (quitar BOM, CRLF a LF, espacios finales,
  líneas vacías finales, un LF, SHA-256 en minúsculas). La definición sigue en `ACTUALIZAR.md` y
  en ADR 008.

## 5. Liberación / piloto
Dentro de 0.5.0, sin `push` y sin distribuir a ningún proyecto. Sin feature flag.

Orden, después de que las fases de edición estén hechas: congelar `template/`, regenerar
`manifiestos/0.5.0.md`, E20, contraste de nuevo contra `manifiestos/0.4.0.md` (se anota lo medido;
no se exigen las cifras viejas), chequeos de `AGENTS.md` › Comandos y medición de RF-23 y RF-24
sobre ese contenido. Si `template/` cambia después, se repite el manifiesto y la medición.

`/sdd-cierre` no es una fase de este plan. Corre cuando el usuario lo pida, sobre el resultado ya
medido.

## 6. Decisiones
Las once primeras están tomadas. No se reabren. La alternativa es la que ya descartó el avance.

| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| (1) Flujo SDD; no editar el kit directo | Aplicar los cambios sin carpeta | Principio 2 de la constitución. Avance, decisión 1 |
| (2) Modelo por rol y permisos del frontmatter fuera. La deuda 003 ya está creada; el cierre la cita y no crea el archivo | Incluirlos en este feature, o solo el permiso del implementer | Sin verificar la herramienta sería un cambio a ciegas. Avance, decisiones 2 y 14 |
| (3) Medición en bytes de lo que la instrucción manda leer, caso `001_ActualizarKit`. RNF-1: ≥ 70 % en la mediana del implementer y en `/sdd-status`. Menor y mayor no suben | Re-ejecutar un feature, o no poner cifra | Avance, decisión 3. Cifras en `02` §3 |
| (4) Sale en 0.5.0, sin push. Al congelar se regenera `manifiestos/0.5.0.md` y E20 pasa. No se cambia E20 ni ADR 007/008 ni el procedimiento de actualización | Versión 0.5.1 | Avance, decisión 4 |
| (5) Antes de editar, confirmación en `05_avance.md`: con quién se coordinó, quién commitea 001/002/003 y el árbol ajeno. F1 no la da por hecha. No se revierte trabajo ajeno | Que lo commitee esta sesión, o implementar sobre el árbol sucio | Avance, decisión 5. El punto de retomo actual no es esa confirmación |
| (6) En features abiertos desde esta versión: mínimo 1 minuto por paso real; prohibido el lote de filas con 0 minutos; la única excepción es la frase exacta "sin trabajo real" en Nota. `001_ActualizarKit` no se reescribe | Sin excepción, o aplicar la prohibición también a los features ya cerrados | Avance, decisiones 6 y 8 |
| (7) No suponer que la herramienta inyecta `AGENTS.md`. Cada subagente lee solo las secciones que usa | Suponer herencia, o no tocar esa lectura | Avance, decisión 9 |
| (8) Cambios de `AGENTS.md`: `template/AGENTS.md` y el de la raíz, sin divergir en lo genérico. Llegan por fusión manual (grupo B) | Sacar esos cambios de `AGENTS.md` | Avance, decisión 10 |
| (9) `/sdd-estructura` lo ejecuta la sesión principal, sin subagente. El reviewer conserva `validate` | Dejar la estructura en el reviewer | Avance, decisión 11 |
| (10) Documentos de menos de 4 096 bytes se leen enteros. Tope de `/sdd-init` y `/sdd-constitucion`: 30 archivos o 150 KB. Modo "solo secretos" una sola vez en `seguridad-basica`. Esos dos comandos no tienen meta del 70 %. El 70 % es el paso de mediana. La lectura cuenta el texto del agente y del comando | Medir solo lo que una corrida leyó, o ponerles también el 70 % | Avance, decisión 12 |
| (11) Fuente única por fase: el procedimiento vive en `/sdd-<fase>`. La skill solo asocia fase, documento y rol. El coordinator solo rutea. Registrado en [ADR 009](../../004_adr/009_FuenteUnicaPorFase.md) | Dejar la secuencia en la skill, o dejarla en el coordinator | La spec ya lo fijó (definición "Fuente única por fase") |
| (12) Mediana = `F5.1` (posición 16 de 32). Menor = `F6.2`. Mayor = `F1.5`. Línea base del implementer = 89 004, y 92 280 en `F6.2`. `/sdd-status` = 122 731 | Promediar los dos centrales (`F2.1` tiene 15 215 y no se usa). No hubo empate | Método en `02` §3.3–3.5, para no volver a medir con otra regla |

## 7. Fases y estimado
F0 no edita `template/`. F1 es el primer paso de la implementación y no está hecha. Ninguna fase se
llama de forma que sustituya a `/sdd-cierre`.

Quien edita relee el archivo, toca solo la parte de este feature y, si el archivo cambió desde la
lectura anterior, para y avisa. Los números de F0 salen de `02` §3. Si un `wc -c` de `template/`
ya no coincide, se vuelve a medir y se anota la cifra nueva antes de editar.

| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | La sesión principal copia en `05_avance.md` la línea base de `02` §3 (piezas, caso congelado, los tres pasos con su mínima y su línea base, `/sdd-status`, arranque de los otros roles y comandos). No edita `template/` ni docs de la raíz | RF-23 | 0,25 |
| F1 | Puerta de coordinación. Sin una entrada fechada del usuario —con quién se coordinó, quién commitea `001`/`002`/`003`, y el árbol ajeno— no hay F2. No se da por hecha. No se revierte nada | RNF-4 | 0,25 |
| F2 | Contrato de tiempos (RF-16 reglas 1 a 4, y RF-22) en `avance-y-tiempos.md`. La skill `sdd` queda en tabla de asociación, con enlace al comando, y la fila de estructura pasa a la sesión principal | RF-14, RF-15, RF-16, RF-18, RF-22 | 0,5 |
| F3 | Comandos de lectura y de tope: `sdd.md`, `sdd-status.md`, `sdd-implement.md`, `sdd-estructura.md`, `sdd-init.md`, `sdd-constitucion.md`. Cada uno dice qué lee, qué hace y qué no hace | RF-1, RF-2, RF-5, RF-8, RF-10, RF-15, RF-18 | 0,5 |
| F4 | El resto de comandos (`sdd-spec`, `sdd-clarify`, `sdd-plan`, `sdd-tasks`, `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-change`) se quedan como fuente de su fase y dejan de repetir plantillas, tiempos y arranque. La sesión principal anota la tabla fase → fuente → piezas que solo enlazan | RF-4, RF-6, RF-9, RF-11, RF-14, RF-16, RF-17, RF-19, RF-20 | 0,5 |
| F5 | Los cuatro agentes: leen lo que su rol declara, no reescriben el comando, no repiten el arranque. `implementer.md` + `sdd-implement.md` tienen que caber en el margen de RNF-1 (abajo) | RF-1, RF-2, RF-3, RF-6, RF-7, RF-12, RF-13, RF-21 | 0,5 |
| F6 | `template/AGENTS.md` y `AGENTS.md` de la raíz, alineados en lo genérico. Modo "solo secretos" en `seguridad-basica`. Una frase de Alcance en `validar-estructura.md` | RF-11, RF-12, RF-13, RF-15, RF-18 | 0,5 |
| F7 | Una línea en la entrada 0.5.0 de `CHANGELOG.md` | Criterio de finalización | 0,25 |
| F8 | Congelar `template/`. Regenerar `manifiestos/0.5.0.md` con el algoritmo vigente. E20. Contraste contra 0.4.0, anotado de nuevo. Chequeos de Comandos y ensayo de instalación en `mktemp -d`. Medición RF-23 y RF-24 sobre ese contenido. Proponer el refresco de la copia local si E19 difiere; hacerlo solo con confirmación | RF-23, RF-24, RNF-1, RNF-3 | 0,75 |
| | **Total** | | **4,0** |

Dependencias: F0 antes de cualquier edición. F1 antes de F2. F2 antes de F3 y F4 (los comandos
enlazan un contrato y una tabla que ya existen). F5 después de F3 y F4 (el agente enlaza el
comando). F6 puede ir junto a F5 y antes de F8. F7 antes de F8. F8 al final. Una edición de
`template/` posterior a F8 obliga a repetir F8.

Comprobación de RNF-1 al terminar F5 y de nuevo en F8, porque la medición vale sobre el contenido
final:

- Mediana: bytes de `implementer.md` + bytes de `sdd-implement.md` + 15 181 ≤ 26 701.
- `/sdd-status`: el comando nuevo + la línea `Estado` + las cuatro secciones de `05_avance.md` del
  caso + las líneas que devuelva la búsqueda de identificadores en `04_tasks.md` ≤ 36 819. No entra
  el archivo de tareas entero ni `MEMORY.md`.
- `F6.2` y `F1.5` no superan 92 280 y 89 004. En `F6.2` y `F5.9` se cuenta el respaldo de RF-1, no
  solo la mínima de la tabla.

Si F5 no cumple el tope de la mediana, se para y se vuelve al usuario. No se cambia RNF-1 ni la
línea base para hacerla pasar.

## 8. Criterios de aceptación
| Criterio | Cómo se acepta |
|---|---|
| RF-1, RF-2 | Un paso del implementer lee la mínima de `02` §3.3. Ampliación solo dentro de la lista cerrada (tope 3). Respaldo con aviso si no hay RF o no hay sección de plan |
| RF-3 | El reviewer lee el "Hecho cuando" del paso y los criterios de esos RF |
| RF-4 | `/sdd-cierre` localiza en `05_avance.md` los pasos que no están `[x]` |
| RF-5, RNF-1 (status) | `/sdd-status` sobre `001_ActualizarKit` ≤ 36 819 bytes con el método de §3.5, y entrega los mismos cinco puntos |
| RF-6, RF-7, RF-9 | Planner y coordinator leen lo declarado para ese documento o para enrutar. La cadena de spec no relee constitución ni memoria |
| RF-8 | `/sdd` para `status` o `estructura` no carga la skill ni el contrato |
| RF-10 | Init declara 30 archivos o 150 KB; constitucion lo referencia. Sin meta del 70 % |
| RF-11 | La skill declara el modo "solo secretos"; validate y reviewer la referencian |
| RF-12, RF-13 | Cada rol nombra sus secciones de `AGENTS.md`. Los dos `AGENTS.md` coinciden en lo genérico |
| RF-14 | Tabla fase → comando → piezas que solo enlazan, sin otra secuencia |
| RF-15 | E12 intacto. El cierre, en el comando y en quien lo describa, revisa deuda y ADR. `sdd.md` incluye `sdd-estructura` |
| RF-16, RF-22 | Las cinco reglas multi-fase tienen un dueño. El contrato exige inicio, fin y al menos 1 minuto, y solo admite "sin trabajo real" en Nota. `001` sigue válido |
| RF-17 | Plan y tasks apuntan a `plantillas.md` |
| RF-18 | Sesión principal corre E1–E20. El reviewer no. E20 dice lo mismo que antes |
| RF-19, RF-20 | Validate y flujo acotan al feature. Clarify no depende de `git diff` |
| RF-21 | Cada pieza dice qué no hace, y coincide con Git y reglas duras. Sin cambio de frontmatter |
| RF-23, RNF-1 (mediana) | Tabla antes/después con las cifras de F0. Mediana ≤ 26 701. Menor y mayor no suben |
| RF-24 | Cada verificación de la spec sigue localizable en una pieza |
| RNF-2, RNF-3, RNF-5, RNF-6 | No se elimina un documento ni un chequeo. UTF-8 sin BOM, español, sin scripts. Features viejos siguen válidos. Cada pieza tocada declara qué lee, qué hace y qué no hace |
| RNF-4 | F1 registrada. Diffs sin revertir trabajo ajeno |
| Versión 0.5.0 | Manifiesto 0.5.0 regenerado, E20 pasa, contraste anotado, chequeos de Comandos pasan, ensayo de instalación en `mktemp -d` no pisa. `CHANGELOG.md` tiene la línea. `/sdd-cierre` queda fuera de estas fases |
