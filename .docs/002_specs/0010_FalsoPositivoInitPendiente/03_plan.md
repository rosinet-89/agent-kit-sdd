# 03 — Plan: Falso positivo de SDD-INIT-PENDIENTE tras /sdd-init
Spec: 01_spec.md (aprobada 2026-11-01 14:49)

## 1. Ubicación y capas
Todo es Markdown del kit, sin código. Capas: instrucciones globales (`AGENTS.md`), comandos, agente, skill y producto (`validar-estructura.md`) de `.agents/`. Los archivos de `.agents/` son iguales en `template/` y en la raíz: se edita `template/` y se replica con `cp` (el `AGENTS.md` de la raíz se edita aparte). En la raíz `.agents/*` está excluido de Git; solo se versionan `AGENTS.md` y `README.md`.

Regla de edición (por las colisiones de 02 §1.3): releer el archivo antes de cada Edit; `old_string` mínimo (la frase actual); nunca por número de línea. Orden de fusión del kit: 0008, 0010, 0009.

## 2. Archivos que se crean o modifican
Se crea un archivo: el ADR 0020 (fase ADR), que no es parte de `template/` ni entra al manifiesto. Los textos exactos van en 2.1 a 2.4; el implementer los copia.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/AGENTS.md` | Instrucciones globales | Paso 1 de «Cada sesión» (fuente única) y fila coordinator, sin literal | 1, 2, 3, 4, 7 |
| `AGENTS.md` (raíz) | Instrucciones globales | Mismas dos frases; grep da 0 | 1, 7 |
| `template/.agents/commands/sdd.md` y réplica | Comando | Líneas 8 y 14 remiten al paso 1 | 5 |
| `template/.agents/commands/sdd-init.md` y réplica | Comando | Línea 15 remite (la 69 no cambia) | 5 |
| `template/.agents/agents/coordinator.md` y réplica | Agente | Líneas 10 y 16 remiten | 5 |
| `template/.agents/skills/sdd/SKILL.md` y réplica | Skill | Línea 19 remite | 5 |
| `README.md` | Documentación | Línea 54 remite (la 48 no cambia) | 5 |
| `template/.agents/validar-estructura.md` y réplica | Producto | Fila E21, «Detalle de E21», rangos a E21 | 6, 6a |
| `template/.agents/commands/sdd-estructura.md` y réplica | Comando | Rangos a E21; no copia la cadena | 6a |

| `.docs/004_adr/0020_<nombre>.md` (nuevo) | Documentación del kit (raíz, versionada) | ADR de la decisión 7; fuera de `template/` y del manifiesto | — |
| `.docs/004_adr/README.md` | Documentación del kit | Fila nueva del ADR 0020 (el `template/` solo lleva el índice vacío; no se toca) | — |

Entregable adicional (RF-8): las líneas de Changelog 0.6.0 quedan en `05_avance.md` (F5), no en `ChangelogAgentKitSdd.md`.

No se tocan: `ACTUALIZAR.md`, `INSTALAR.md`, `reviewer.md` (lo corrige 0009), `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/`.

### 2.1 Frases de remisión (menores diferidos a y fijados aquí)
Ninguna lleva el literal ni `<!--`. La descripción no es un marcador.

**Paso 1 de «Cada sesión»** (idéntico en `template/AGENTS.md` y en `AGENTS.md` raíz; en la raíz sigue el paso 0 intacto). Sustituye las dos líneas actuales del paso 1:
```markdown
1. Estado de instalación: si la primera línea no vacía tras el título es el comentario HTML de
   instalación pendiente, idéntico al que define la plantilla del kit (AGENTS.md de su carpeta
   template/) y que `/sdd-init` borra, el proyecto aún no está personalizado. Dilo en una línea y
   sugiere correr `/sdd-init`. No ejecutes otros comandos `/sdd*` hasta entonces. Solo cuenta ese
   comentario exacto; esta descripción no es el marcador.
```
Sin comillas inversas en la ruta de la plantilla: un proyecto instalado no tiene template/ y E11 daría FALLA (02 §6). El mismo criterio rige en E21 y en sdd-estructura (decisión 5).

**Fila coordinator de «Qué lee cada rol»** (ambos `AGENTS.md`):
```markdown
| coordinator | Comprueba el estado de instalación con el paso 1 de «Cada sesión». También «Git» y «Reglas duras». |
```

### 2.2 Piezas que citan la regla (RF-5)
Cada una queda con una frase que remite al paso 1 y sin condición propia ("contiene"). Se edita en `template/.agents/` y se copia a `.agents/`.
- `commands/sdd.md` línea 8: «y de `AGENTS.md` solo si contiene `SDD-INIT-PENDIENTE`» pasa a «y de `AGENTS.md` solo el paso 1 de «Cada sesión» (estado de instalación)». Línea 14: «Si `AGENTS.md` contiene `SDD-INIT-PENDIENTE`, para y sugiere `/sdd-init`.» pasa a «Si el paso 1 de «Cada sesión» de `AGENTS.md` marca instalación pendiente, para y sugiere `/sdd-init`.»
- `agents/coordinator.md` línea 10: «compruebas si contiene `SDD-INIT-PENDIENTE`» pasa a «compruebas el estado de instalación con el paso 1 de «Cada sesión»»; el resto de la línea (incluido lo que añada 0008) no se toca. Línea 16: mismo cambio que `sdd.md` línea 14.
- `skills/sdd/SKILL.md` línea 19: «Si `AGENTS.md` contiene `SDD-INIT-PENDIENTE`, el proyecto no está personalizado: corre `/sdd-init` primero.» pasa a «Si el paso 1 de «Cada sesión» de `AGENTS.md` marca instalación pendiente, el proyecto no está personalizado: corre `/sdd-init` primero.»
- `commands/sdd-init.md` línea 15: «Si `AGENTS.md` ya no contiene `SDD-INIT-PENDIENTE`, el proyecto ya está personalizado:» pasa a «Si, según el paso 1 de «Cada sesión» de `AGENTS.md`, la instalación ya no está pendiente, el proyecto ya está personalizado:». La línea 69 («Quita el marcador `SDD-INIT-PENDIENTE` de `AGENTS.md`») queda: nombra lo que se borra (pieza exenta), no define una regla de detección.
- `README.md` línea 54: «Mientras exista `SDD-INIT-PENDIENTE` en `AGENTS.md`, los agentes piden» pasa a «Mientras el paso 1 de «Cada sesión» de `AGENTS.md` marque instalación pendiente, los agentes piden». La línea 48 queda por la misma razón.

### 2.3 E21 en `validar-estructura.md` (menor diferido b)
Criterio de rutas (E11): `validar-estructura.md` se instala en proyectos donde no existen template/AGENTS.md ni .agents/.kit-version (esta se escribe al instalar). Por eso, en las piezas nuevas esas rutas van SIN comillas inversas, como E20 con «template/» y «manifiestos/<VERSION>.md». Solo llevan comillas inversas las rutas que existen en un proyecto instalado (`AGENTS.md`, `validar-estructura.md`, `sdd-estructura.md`).

Fila nueva, justo tras E20 (bloque `sesión`, como E16 y E17):
```markdown
| E21 | sesión | **Marcador de instalación:** solo con .agents/.kit-version. Existe `AGENTS.md` y el literal `SDD-INIT-PENDIENTE` solo aparece dentro del marcador válido (cero veces en un proyecto personalizado, una en uno pendiente) | «Detalle de E21» |
```
Cambios de rango: línea 9 «E1 a E20» a «E1 a E21»; «Cómo responder» 3 «E1–E20» a «E1–E21».

Sección nueva «Detalle de E21», entre la tabla y «Cómo responder»:
```markdown
## Detalle de E21

Aplica solo si existe .agents/.kit-version. Si no existe (p. ej. la raíz del clon del kit), E21 no
aplica: ni `OK` ni `FALLA`. Lee `AGENTS.md` (un enlace simbólico se lee como su destino). Pasos, en
este orden; el primer `FALLA` termina E21:

1. `AGENTS.md` no existe: `FALLA`.
2. «La cadena» es el párrafo «Cadena del marcador» de este archivo (`validar-estructura.md`). Si no
   es un comentario HTML de una línea que contiene el literal una sola vez: `FALLA` (cadena ausente
   o alterada). Sin template/ este paso es lo único que se puede comprobar: la cadena se detecta
   solo por forma, no por contenido (diferencia ya declarada en la spec).
3. Si existe template/AGENTS.md: su primera línea no vacía tras el título, comparada como en el
   punto 5, debe ser igual a la cadena. Si no: `FALLA` (desincronizada). Sin template/, se omite.
4. Cuenta de ocurrencias del literal: `grep -o 'SDD-INIT-PENDIENTE' AGENTS.md | wc -l`. Si da 0: no
   hay marcador (proyecto personalizado); `OK`. Un archivo vacío cae aquí.
5. Si da 1 o más: extrae la primera línea no vacía tras el título con
   `sed 's/\r$//' AGENTS.md | awk 'f&&NF{print;exit} /^# /{f=1}' | sed 's/[[:space:]]*$//'`.
   Es el marcador válido si la cuenta es 1 y esa línea es igual a la cadena (solo se ignoran el fin
   de línea CRLF o LF y los espacios o tabuladores finales). Entonces `OK` (proyecto pendiente). En
   cualquier otro caso (cuenta mayor que 1, otra posición, comentario repetido, variante, BOM, `\r`
   suelto, multilínea): `FALLA`.

Cadena del marcador (copia literal del comentario de template/AGENTS.md; quien cambie uno cambia
el otro en el mismo cambio). Es la única copia fuera de la plantilla:

`<!-- SDD-INIT-PENDIENTE: este marcador lo quita /sdd-init al terminar la personalización. -->`

Mensajes de `FALLA` (columnas de «Cómo responder»; severidad ALTA):

| Caso | Hecho | Corrección propuesta |
|---|---|---|
| Sin `AGENTS.md` | `AGENTS.md` no existe | Restaurarlo desde Git o desde template/AGENTS.md; no se crea sin confirmación |
| Cadena (paso 2) | Cadena del marcador ausente o alterada en `validar-estructura.md` | Restaurarla desde el comentario de template/AGENTS.md |
| Desincronía (paso 3) | La cadena difiere del comentario de template/AGENTS.md | Igualar la cadena y el comentario en el mismo cambio |
| Literal (paso 5) | `AGENTS.md:N` contiene el literal fuera del marcador válido (cuenta C) | Reformular la frase sin el literal; si el proyecto ya está personalizado, incorporar el paso 1 nuevo (Changelog 0.6.0, grupo B) |
```
Orden justificado: primero lo que impide comparar (archivo, cadena), luego el control cruzado con la plantilla (la cadena debe ser fiable antes de usarla), por último la comprobación de `AGENTS.md`. `sdd-estructura.md` no muestra la cadena, por eso no hay comprobación entre las dos copias (RF-6 es vacío ahí); se verifica que `grep -c` del literal en `sdd-estructura.md` da 0.

### 2.4 `sdd-estructura.md`
Línea 10: «E1 a E20» a «E1 a E21». Línea 12: «E1–E20» a «E1–E21» y «ni E20» queda igual. Nada más (0009 quita «aplícala una por una» en su propia edición).

## 3. Base de datos
No aplica.

| Script `sql/` | Tipo (DDL/seed) | Rollback |
|---|---|---|
| — | — | — |

## 4. Seguridad
Sin credenciales ni datos. El chequeo es de solo lectura (`grep`, `sed`, `awk`). Las ediciones son idempotentes por `old_string`: si la frase ya cambió, el Edit falla sin efecto. La verificación usa copias en `mktemp -d` fuera del repo; nada se pisa (principio 4).

## 5. Liberación / piloto
No aplica piloto. Versión 0.6.0 única con 0008 y 0009. Cumplimiento del principio 6 sin crear la versión por cuenta propia:

- 0010 no crea `manifiestos/0.6.0.md`, no crea la entrada 0.6.0 del Changelog y no cambia `VersionAgentKitSdd` (0.5.0). Aporta sus líneas en su `05_avance.md` (F5), para que las pegue quien cierre 0009.
- El bump de `VersionAgentKitSdd` a 0.6.0, el Changelog y el manifiesto los hace una sola vez la F6 de 0009, después de fusionar 0008 y 0010 (así lo declara su plan, decisión 19). Eso cumple el criterio de la spec "`VersionAgentKitSdd` en 0.6.0 y nota aportada" en la fusión final, no en esta spec. Es la decisión 2 de §6.
- E20 no se verifica aquí (ventana aceptada). El manifiesto 0.6.0 tendrá las 43 filas esperadas (42 de hoy más la de `sdd-enlaces.md` de 0009), con las 7 de 0010 recalculadas.

Líneas para el Changelog 0.6.0 (grupo B; se dejan en `05_avance.md`, no en `ChangelogAgentKitSdd.md`):
```markdown
- Falso positivo tras `/sdd-init`: el paso 1 de «Cada sesión» y la fila coordinator de `AGENTS.md`
  describen el marcador de instalación pendiente sin escribir el literal y remiten al comentario de
  la plantilla. `/sdd`, el coordinator, la skill `sdd`, `/sdd-init` y el README remiten al paso 1.
- Nuevo chequeo E21 en `validar-estructura` (bloque sesión, solo con `.agents/.kit-version`): FALLA si
  `AGENTS.md` no existe, si contiene el literal fuera del marcador válido, o si la cadena del
  marcador falta o no coincide con la plantilla (con `template/` presente).
- `AGENTS.md` es grupo B: los proyectos existentes incorporan a mano el paso 1 y la fila coordinator
  nuevos. Hasta entonces, E21 da FALLA en un proyecto ya personalizado con una versión anterior
  (esperado).
```

## 6. Decisiones
| # | Decisión | Alternativa descartada | Por qué |
|---|---|---|---|
| 1 | El paso 1 describe el marcador por posición y exactitud, sin literal, y remite a la plantilla | Dejar "contiene" con el literal entre comillas; usar un archivo de estado | La spec lo fija; el mecanismo no cambia (fuera de alcance) |
| 2 | 0010 no sube `VersionAgentKitSdd`; la sube la F6 de 0009 con el manifiesto. Desvía de RF-8 y del Alcance de la spec, que dicen que 0010 sube la versión; aclarado en la spec | Subir 0.6.0 al cerrar 0010 | La misma spec exige crear todo una sola vez al final; E20 fallaría sin manifiesto, 0008 y 0009 ya lo excluyen y un solo bump evita choques. **Aprobada por el usuario 2026-11-01** |
| 3 | E21 en el bloque `sesión`, tras E20, corre sin argumento | Bloque propio o `kit` | `kit` es solo para el clon y E21 cubre proyectos instalados; un bloque propio cambia la lista de argumentos. No requiere aprobación (decisión de diseño dentro de la spec) |
| 4 | Orden de E21: archivo, cadena, control cruzado con plantilla, cuenta, posición | Cuenta primero | La cadena debe ser fiable antes de compararla. No requiere aprobación |
| 5 | Ninguna ruta inexistente en un proyecto instalado va entre comillas inversas en las piezas nuevas: paso 1, fila y detalle de E21, mensajes de FALLA y sdd-estructura (template/AGENTS.md, .agents/.kit-version, template/) | Citarlas entre comillas | E11 daría FALLA en proyectos instalados donde no existen. **Aprobada por el usuario 2026-11-01** (la spec escribe `template/AGENTS.md` entre comillas; aclarado en la spec) |
| 6 | `sdd-estructura.md` no copia la cadena: la cadena vive en 2 sitios (plantilla y `validar-estructura.md`) | Copiarla en 3 sitios | Decisión tomada: la spec ya lo permite («si la muestra»). Menos duplicación; la verificación cruzada de RF-6 queda vacía ahí |
| 7 | **ADR, aprobada por el usuario 2026-11-01; se crea en la fase ADR:** marcador sin literal fuera del comentario; regla del cero; paso 1 como fuente única; cadena copiada con E21 como control cruzado, excepción al principio 3. Alternativas: archivo de estado; grep laxo; quitar la regla de `AGENTS.md`. Efecto duradero: todo proyecto del kit y cada pieza nueva que cite la regla. Número: siguiente libre por valor numérico (0008 reserva 0019); se crea como 0020 (fase ADR): `.docs/004_adr/0020_MarcadorSinLiteralFueraDelComentario.md` | — | Alternativa real y efecto duradero |
| 8 | `reviewer.md` línea 21 («E1–E20») se deja a 0009 | Cambiarlo aquí | Fuera del alcance de la spec; 0009 ya lo lista |

## 7. Fases y estimado
Solo Markdown, un implementer por paso (MEMORY.md: el real ronda el estimado). Estimado en minutos; total 115.

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Preparación: releer el estado de 0008 y 0009, `git status`, `grep -rn` de las frases a cambiar, `diff -rq template/.agents .agents` previo y comprobar que `validar-estructura.md` aún dice E1 a E20 | — | 10 |
| F1 | `template/AGENTS.md` y `AGENTS.md` raíz: paso 1 y fila coordinator (2.1) | 1, 2, 3, 4, 7 | 15 |
| F2 | Piezas de RF-5 (2.2) en `template/.agents/` y `README.md`; copiar a `.agents/` (`cp`) y `diff` | 5 | 20 |
| F3 | E21: `validar-estructura.md` (fila, detalle, rangos) y `sdd-estructura.md` (2.3, 2.4); copiar a `.agents/` | 6, 6a | 25 |
| F4 | Verificación (sección 8) | 1 a 6a | 30 |
| F5 | Dejar las líneas de Changelog (sección 5) en `05_avance.md` y registrar la nota de merge | 8 | 5 |
| ADR | Al cerrar, crear el ADR 0020 con la skill `.agents/skills/adr/SKILL.md` (`0020_<nombre>.md`) y su fila en `.docs/004_adr/README.md`. Contenido: marcador sin literal fuera del comentario; regla del cero; paso 1 como fuente única; cadena copiada con E21 como control cruzado; excepción explícita al principio 3. Comprobar el número libre en ese momento (`ls .docs/004_adr`, por valor numérico): 0008 reserva 0019; se crea tras fusionar 0008, y si 0019 aún no existe se confirma con el usuario antes de usar 0020 | — | 10 |

Orden: F0, F1, F2, F3, F4, F5, ADR. Cada fase deja un estado revisable. F1 y F3 son independientes; F4 necesita ambas. Los commits los decide el usuario (un commit por módulo, `AGENTS.md` › Git).

## 8. Criterios de aceptación
Comandos de `AGENTS.md` › Comandos, sobre los archivos tocados: `file -i` da utf-8, `grep -l $'^\xEF\xBB\xBF'` vacío, rutas citadas existentes, sin `<<…>>` en `template/` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. Además:

1. Regla del cero: `grep -o 'SDD-INIT-PENDIENTE' template/AGENTS.md | wc -l` = 1 y la extracción del paso 5 de E21 igual a la cadena; el mismo comando en `AGENTS.md` raíz = 0 (única vigilancia de la raíz).
2. Piezas de RF-5: cada una (skill, `sdd.md`, coordinator, `sdd-init.md` línea 15, README, fila coordinator) tiene una frase que remite al paso 1 y ninguna condición "contiene". Lectura pieza por pieza. `grep -rn 'contiene `SDD-INIT' template .agents README.md AGENTS.md` vacío.
3. Ensayo de instalación, en `tmp=$(mktemp -d)` fuera del repo: `cp -R --update=none template/. "$tmp"/`. Caso base con marcador: `touch "$tmp/.agents/.kit-version"`; E21 da OK (pendiente, cuenta 1). Simular `/sdd-init` borrando la línea del marcador: cuenta 0, OK. Insertar el literal fuera del marcador: FALLA. Duplicar el marcador: FALLA. Borrar `AGENTS.md`: FALLA. `AGENTS.md` vacío: OK. CRLF y espacios finales: OK. Variantes (espacio inicial, BOM, `\r` suelto): FALLA. Segundo comentario idéntico en otra posición (marcador válido en su sitio más una copia más abajo): FALLA (cuenta 2). Marcador idéntico en otra posición con cuenta 1 (no es la primera línea no vacía tras el título): FALLA.
4. Con `template/` presente en la copia (`cp -R template "$tmp/template"`): editar el comentario de `$tmp/template/AGENTS.md` produce FALLA por desincronía; sin esa carpeta, ese caso no se detecta (declarado). Todo en la copia, nunca en el repo.
5. `grep -c '^| E[0-9]' .agents/validar-estructura.md` = 21 y `grep -rn 'E1 a E20\|E1–E20' .agents/validar-estructura.md .agents/commands/sdd-estructura.md` vacío; `grep -c SDD-INIT-PENDIENTE .agents/commands/sdd-estructura.md` = 0. Réplicas idénticas: `diff -rq template/.agents .agents` muestra solo `napkin.md` y `skills/caveman`.
6. Las líneas de Changelog de la sección 5 están en `05_avance.md`; `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `manifiestos/` no se tocaron (`git status` lo confirma).
