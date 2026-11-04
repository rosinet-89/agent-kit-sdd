# 02 — Análisis de impacto: Falso positivo de SDD-INIT-PENDIENTE tras /sdd-init

## 1. Resumen del requerimiento
Tras `/sdd-init`, `AGENTS.md` sigue conteniendo el literal en dos frases que describen la regla (paso 1 de «Cada sesión» y fila coordinator de «Qué lee cada rol»). Un agente que compruebe "contiene" lo encuentra, sugiere `/sdd-init` otra vez y bloquea los `/sdd*`. El cambio: describir la regla sin el literal, dejar el paso 1 como única fuente, que las demás piezas remitan a él y que un chequeo nuevo (E21) vigile la regresión. Solo Markdown.

### 1.1 Antecedentes (índice de planes)
- `004_ModeloYPermisosAgentes` (02 §3.3 y 04 F1.1): vio el falso positivo en la raíz y lo esquivó con una copia; no lo resolvió. Se reutiliza su hallazgo; nada lo contradice.
- `003_OptimizarArnes` y `007_DeudasAbiertas`: tocaron `/sdd-init` sin abordar esto. Se reutiliza el criterio de fuente única de 003 (ADR 009).
- `002_Kit050` / `001_ActualizarKit`: definen `validar-estructura.md` (E1 a E20) y E20 con manifiestos. E21 se añade sin tocar E20.
- Colisiones con specs en vuelo (versión 0.6.0 compartida; orden de fusión 0008, luego 0010, luego 0009). Ver 1.3.

### 1.2 Deuda técnica relacionada
Ninguna abierta (`.docs/003_deuda/README.md`: 001 a 008 resueltas).

### 1.3 Colisiones con 0008 y 0009 (leídas en `01`, `02`, `03` y `04` de cada una)
Se leyeron los planes en el estado del 2026-11-01 14:50. Ninguna está implementada aún (`tasks` cerrada; en espera).

| Archivo | 0008 (Guardrails) | 0009 (Enlaces tras pull) | 0010 (esta spec) | Cómo integrar |
|---|---|---|---|---|
| `template/AGENTS.md` y `AGENTS.md` raíz | Sección nueva «Guardrails» antes de «Cada sesión»; nota tras la tabla «Qué lee cada rol»; línea «Consulta de datos» en Comandos | Paso 7 de «Cada sesión» y «Roles y comandos» (cita `/sdd-enlaces`) | Paso 1 de «Cada sesión» y fila coordinator de la tabla | Zonas disjuntas, pero los números de línea se mueven. Edit con `old_string` mínimo (la frase actual), releyendo el archivo antes. Nunca por número de línea |
| `.agents/commands/sdd-init.md` (+ réplica) | Línea 38 (tema de pregunta de BD) | Pasos 4.2 y 4.3 (sugerir `/sdd-enlaces`) | Línea 15 («Si `AGENTS.md` ya no contiene…») | Líneas distintas; `old_string` por frase |
| `agents/coordinator.md` (+ réplica) | Línea 10 («Qué lee»): añade «y Guardrails» y una frase tras «"Git" y "Reglas duras"» | Fila de ruteo de `/sdd-enlaces`; «E1–E20» a «E1–E21» donde aparezca | Línea 10 (la frase «compruebas si contiene…») y línea 16 | Misma línea 10 que 0008. Como 0008 va primero, 0010 relee la línea y reemplaza solo la subcadena «compruebas si contiene `…`»; no toca lo que 0008 añadió |
| `commands/sdd.md`, `skills/sdd/SKILL.md` (+ réplicas) | Solo `SKILL.md` líneas 127 y 136 | Fila `/sdd-enlaces`; lista de comandos | `sdd.md` líneas 8 y 14; `SKILL.md` línea 19 | Líneas distintas |
| `validar-estructura.md` (+ réplica) | No lo toca | E1, E19 (`local/`), «Cómo responder» 4 | Fila E21, sección «Detalle de E21», «E1 a E20» y «E1–E20» a E21 | 0010 va antes que 0009: ambas editan este archivo en zonas distintas. 0009 debe partir de la rama con 0010 (ya lo declara en su F0) |
| `commands/sdd-estructura.md` (+ réplica) | No lo toca | Quita «aplícala una por una» | «E1 a E20» y «E1–E20» a E21 (líneas 10 y 12) | Línea 10 la comparten 0009 (frase final) y 0010 (el rango): `old_string` mínimo distinto |
| `README.md` | No | Posible (comando nuevo) | Frase de la línea 54 | Una sola frase |
| `agents/reviewer.md` línea 21 («E1–E20») | Líneas 10 y 21 (Guardrails) | «E1–E20» a «E1–E21» | No lo toca | Queda en 0009; ver riesgos |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.6.0.md` | No toca; entrega líneas en su `05_avance.md` | Los crea una sola vez (su F6) | No toca; entrega líneas en su `05_avance.md` | Ver 03 §5 |

Numeración de chequeos: 0008 no añade ninguno. 0009 declara "Cualquier otro cambio a E1–E21 queda fuera" y espera que E21 exista tras 0010. Verificado: ningún plan reclama un E distinto de E21. Para no chocar: E21 es de 0010, se añade como fila inmediatamente después de E20 y 0009 solo cambia sus textos "E1 a E20" a "E1 a E21" donde 0010 aún no lo haya hecho (releer antes: tras 0010 deben quedar 0 coincidencias en `validar-estructura.md` y `sdd-estructura.md`).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica (kit de Markdown). Comandos afectados: `/sdd-init`, `/sdd` y `/sdd-estructura` (E21).

### 2.2 Tablas
No aplica. Sin base de datos.

### 2.3 Archivos de código (ruta, capa)
Todos los archivos ya existen. No se crea ninguno. Capa: instrucciones globales, comando, agente, skill, producto (`validar-estructura`), documentación. En la raíz, `.agents/*` está excluido de Git (`.git/info/exclude:8`); solo se versiona `AGENTS.md`, `README.md` y lo demás fuera de `.agents/`.

| Archivo | Capa | Qué cambia |
|---|---|---|
| `template/AGENTS.md` | Instrucciones globales | Paso 1 y fila coordinator (sin literal; el marcador de la línea 3 no se toca) |
| `AGENTS.md` (raíz) | Instrucciones globales | Paso 1 y fila coordinator (grep da 0) |
| `template/.agents/commands/sdd.md` y réplica | Comando | Líneas 8 y 14 remiten al paso 1 |
| `template/.agents/commands/sdd-init.md` y réplica | Comando | Línea 15 remite; la línea 69 (nombra lo que borra) no cambia |
| `template/.agents/agents/coordinator.md` y réplica | Agente | Líneas 10 y 16 remiten |
| `template/.agents/skills/sdd/SKILL.md` y réplica | Skill | Línea 19 remite |
| `README.md` | Documentación | Línea 54 remite; la línea 48 (nombra lo que se quita) no cambia |
| `template/.agents/validar-estructura.md` y réplica | Producto | Fila E21, «Detalle de E21» con la cadena del marcador, rangos E1 a E21 |
| `template/.agents/commands/sdd-estructura.md` y réplica | Comando | Rangos E1 a E21; no copia la cadena |

Total: 7 archivos de `template/` (`AGENTS.md` y 6 de `template/.agents/`), sus 6 réplicas en `.agents/` de la raíz, más `AGENTS.md` raíz y `README.md`: 15 archivos.

### 2.4 Código legado o sin uso
- `ACTUALIZAR.md` no cita el literal y no se toca (la spec lo deja fuera de las piezas exentas).
- Historiales de `.docs/` (004, 003, 0010) contienen el literal; son piezas exentas, no se tocan.
- `reviewer.md` línea 21 dice «E1–E20»: fuera del alcance de esta spec; lo corrige 0009.

## 3. Datos reales
Mediciones de solo lectura (`grep`, `sed`, `awk`, `find`, `diff`) sobre el árbol de trabajo, 2026-11-01 14:50, repo local `/home/cromero/dev/agent-kit-sdd` (HEAD `3d72e10`, más el árbol sin confirmar de 0008, 0009 y 0010). No hay BD.

- Ocurrencias del literal en `AGENTS.md`: `template/AGENTS.md` = 3 (línea 3 marcador, línea 9 paso 1, línea 84 fila coordinator). `AGENTS.md` raíz = 2 (línea 10 paso 1, línea 82 fila coordinator); no tiene marcador. Con `grep -o 'SDD-INIT-PENDIENTE' <archivo> | wc -l`. Objetivo: 1 y 0.
- Otras piezas con el literal (fuera de `.docs/`), todas exentas: `README.md` (48, 54); `.agents/` y `template/.agents/`: `commands/sdd-init.md` (15, 69), `commands/sdd.md` (8, 14), `agents/coordinator.md` (10, 16), `skills/sdd/SKILL.md` (19). Todas con réplica idéntica en la raíz. `ACTUALIZAR.md`, `INSTALAR.md`, `validar-estructura.md` y `sdd-estructura.md` no lo citan.
- Texto exacto del marcador (`template/AGENTS.md` línea 3, primera línea no vacía tras el título de la línea 1; línea 2 en blanco; sin BOM ni CR): `<!-- SDD-INIT-PENDIENTE: este marcador lo quita /sdd-init al terminar la personalización. -->`
- Chequeos hoy: `grep -c '^| E[0-9]' .agents/validar-estructura.md` = 20 (E1 a E20), igual en `template/`. `validar-estructura.md` (57 líneas) y `sdd-estructura.md` (12 líneas) son idénticos a sus réplicas (`diff` sin salida). «E1 a E20» aparece en `validar-estructura.md` líneas 9 y 51 (esta como «E1–E20») y en `sdd-estructura.md` líneas 10 y 12, y «E1–E20» en `reviewer.md` línea 21 (los tres con réplica). `sdd-estructura.md` no muestra el marcador.
- `diff -rq template/.agents .agents`: solo `napkin.md` y `Sólo en .agents/skills: caveman`.
- Manifiesto: `manifiestos/0.5.0.md` tiene 42 filas; `find template -type f | wc -l` = 42; las rutas coinciden una a una. Es decir, todos los archivos de `template/` entran en E20. De esos, 0010 toca 7 (`AGENTS.md`, `validar-estructura.md`, `sdd-estructura.md`, `sdd.md`, `sdd-init.md`, `coordinator.md`, `skills/sdd/SKILL.md`). No existe `.agents/.kit-version` ni en la raíz ni en `template/` (se escribe al instalar).
- Versión: `VersionAgentKitSdd` = 0.5.0; último manifiesto `0.5.0.md`.
- Comandos de los Criterios, probados en una carpeta temporal de scratchpad con variantes de `template/AGENTS.md` (quitadas las dos frases del literal): el marcador intacto, con CRLF y con espacios o tabulador finales da "válido" (cuenta 1 y línea idéntica); con espacio inicial, BOM, `\r` suelto antes de `-->`, `:` alterado, comentario repetido o segundo comentario en otra posición da cuenta ≥ 1 y línea distinta (FALLA); sin marcador da cuenta 0; archivo vacío da cuenta 0; enlace simbólico a un archivo válido se lee como su destino. Hoy, en el estado actual, la cuenta da 3 y 2 (el bug). Los comandos funcionan y no necesitan ajuste. Detalle: con BOM el `awk` no encuentra el título (`/^# /`) y devuelve vacío, lo que también cae en FALLA, como pide la spec.

## 4. Flujo actual
1. `/sdd-init` § 4, paso 2: borra el comentario HTML con el marcador.
2. El paso 1 de «Cada sesión» y la fila coordinator siguen diciendo «si este archivo contiene `SDD-INIT-PENDIENTE`». El literal queda dentro de esas frases.
3. Un agente que aplica la regla con grep o "contiene" lo encuentra → "no personalizado" → sugiere `/sdd-init` y bloquea `/sdd*`.
4. `skills/sdd/SKILL.md`, `commands/sdd.md`, `coordinator.md` y `sdd-init.md` repiten la misma condición "contiene" (cinco definiciones de la regla), contra el principio 3.
5. `validar-estructura` no vigila nada de esto.

## 5. Reglas de negocio: conocidas vs código
| Regla | Fuente | Estado en el código |
|---|---|---|
| Regla del cero: el literal solo dentro del marcador válido (1 en plantilla/pendiente, 0 en personalizado y en la raíz) | Spec 01 | No se cumple (3 y 2) |
| Fuente única de la regla: paso 1 de «Cada sesión» | Spec 01, constitución 3 | Cinco redacciones paralelas |
| Marcador válido: primera línea no vacía tras el título, una vez, coincidencia exacta salvo fin de línea y espacios finales | Spec 01 | No hay chequeo |
| E21 solo con `.agents/.kit-version` | Spec 01 | Inexistente |
| Excepción al principio 3: la cadena del marcador en `validar-estructura.md` (más el comentario de la plantilla) con E21 como control cruzado | Spec 01 | Nueva |
| Cada versión actualiza `VersionAgentKitSdd` y Changelog (principio 6); versión única 0.6.0 al final | Constitución, specs 0008/0009 | Pendiente de la fusión final |
| Un proyecto existente incorpora el cambio a mano (grupo B) | Spec 01, `ACTUALIZAR.md` | A consignar en el Changelog |

## 6. Riesgos y casos borde
- Choque de edición en `template/AGENTS.md`, `AGENTS.md` raíz, `coordinator.md` (línea 10 compartida con 0008) y `validar-estructura.md`/`sdd-estructura.md` (0009). Mitigación: orden 0008, 0010, 0009; Edit con `old_string` mínimo (la frase del literal); releer antes de cada edición; F0 comprueba el estado de 0008 y 0009.
- Ventana de E20 (heredada de 0008): al tocar archivos de `template/` sin manifiesto nuevo, E20 puede no pasar hasta el cierre de 0009. Aceptada por el usuario; 0010 no verifica E20.
- Ruta citada en un proyecto instalado (E11): las rutas citadas con comillas inversas deben existir. En un proyecto instalado no existen `template/` ni `.agents/.kit-version` (se escribe al instalar), y `validar-estructura.md` sí se instala allí. Criterio para TODAS las piezas nuevas (paso 1, fila y detalle de E21, mensajes de FALLA, sdd-estructura): esas rutas van sin comillas inversas, como hace E20 con «template/» y «manifiestos/<VERSION>.md». Solo llevan comillas las rutas que existen en el proyecto instalado (`AGENTS.md`, `validar-estructura.md`). Es la decisión 5 del plan.
- `reviewer.md` línea 21 queda en «E1–E20» entre la fusión de 0010 y la de 0009. Es solo un rango escrito; 0009 lo corrige y su F0 hace `grep` de "E1 a E20" y "E1–E20".
- `/sdd-estructura` con argumento: los bloques seleccionables no incluyen `sesión`. E21 va en el bloque `sesión` (como E16 y E17) y corre solo sin argumento. Alternativa: bloque propio; no se añade para no cambiar la lista de argumentos.
- "Cadena alterada" sin `template/`: no hay referencia con la que compararla. E21 la comprueba por forma (comentario HTML de una línea que contiene el literal una vez). La alteración fina solo se detecta con `template/` presente (ya lo declara la spec).
- Proyectos existentes: su `AGENTS.md` conserva las dos frases; E21 da FALLA hasta que incorporen el cambio. Esperado y documentado (grupo B).
- La raíz del clon no tiene `.kit-version`: E21 no aplica; la vigila solo el grep manual de RF-1 en la validación (HU-3).
- La cadena se copia a mano. Si un tercero cambia el comentario de la plantilla, E21 con `template/` lo detecta; sin `template/` no (declarado).
- Se evita reintroducir el literal: la descripción del paso 1 no lleva `<!--` ni el literal, para que no se confunda con el marcador ni se cuente.

## 7. Preguntas abiertas
Ninguna bloquea el plan. Decisiones de 03 §6 que el usuario debe aprobar: 2 (quién sube `VersionAgentKitSdd`; desvía de RF-8 y Alcance de la spec), 5 (rutas inexistentes en proyecto instalado sin comillas inversas; la spec escribe una con comillas) y 7 (candidata a ADR). No requieren aprobación: 3 (E21 en bloque `sesión`), 4 (orden de pasos), 6 (cadena en 2 sitios: la spec ya lo permite con «si la muestra») y 8 (`reviewer.md` queda para 0009).
