# 02 — Análisis de impacto: Optimizar el arnés SDD

## 1. Resumen del requerimiento
La spec aprobada (`Estado: aprobada`, 2026-10-25 13:07, sin dudas abiertas) pide bajar la lectura que
hoy mandan los 4 agentes y los 14 comandos, sin quitar verificaciones. La métrica de RNF-1 es el paso
de mediana del `implementer` sobre `001_ActualizarKit` y la lectura de `/sdd-status`: al menos 70 %
menos. El paso de menor y el de mayor no pueden subir.

Medido el 2026-10-25 con `wc -c` (sección 3):

| Qué | Identificador | Lectura mínima | Línea base de hoy |
|---|---|---:|---:|
| Menor | `F6.2` | 834 | 92 280 |
| Mediana (posición 16 de 32) | `F5.1` | 15 181 | 89 004 |
| Mayor | `F1.5` | 21 248 | 89 004 |
| `/sdd-status` | — | — | 122 731 |

`F5.1` y `F1.5` comparten línea base: el texto del `implementer` y de `/sdd-implement` nombra los
mismos archivos en todo paso. `F6.2` suma la skill `seguridad-basica` porque el paso manda aplicarla.
Bytes ÷ 4 es solo referencia (cociente entero): mediana 15 181 → 3 795; su línea base 89 004 → 22 251;
`/sdd-status` 122 731 → 30 682.

### 1.1 Antecedentes (índice de planes)
Índice leído entero (`.docs/002_specs/README.md`). Tres filas, prefijos distintos:

| Carpeta | Estatus en el índice | Uso en este análisis |
|---|---|---|
| `003_OptimizarArnes` | Análisis, spec aprobada | Esta carpeta. Se redactan `02` y `03`. |
| `001_ActualizarKit` | Implementado (2026-10-25) | Caso de referencia. Se miden sus documentos. No se modifican. |
| `002_Kit050` | Implementado, retroactivo | Registro de 0.5.0 ya publicado en local. Sin `02`–`04` ni `06`. Fuera de alcance. |

`001` aporta el procedimiento de actualización, los manifiestos y la definición de E20. Este feature
no cambia qué se clasifica, cómo se reemplaza, ni el texto de E20 (ADR 007 y 008 siguen como están).
`06_flujo.md` de `001` existe: entra en la lectura de `/sdd-status`.

### 1.2 Deuda técnica relacionada
Índice `.docs/003_deuda/README.md`:

| Deuda | Estado | Qué hace este feature |
|---|---|---|
| 001 Actualizar el clon del kit | Resuelta (`INSTALAR.md` › Modo mantenimiento) | Fuera de alcance. No se reabre. |
| 002 Versión 0.5.0 sin spec | Resuelta (`002_Kit050`) | Fuera de alcance. No se reabre. |
| 003 Modelo por rol y permisos del frontmatter | Abierta (alta 2026-10-25, decisión 14 de este avance) | La deja abierta. El archivo ya existe (`.docs/003_deuda/003_ModeloYPermisosDeAgentes.md`). No se crea otro. El cierre la cita como dejada. |

La 003 cubre `model`, `mode`, `permissions`, el `Write` del reviewer y el shell del implementer.
RF-21 se queda en documentación.

## 2. Módulos, menú, tablas y archivos involucrados
El producto es `template/` (constitución, principio 1 y 3; ADR 002). La medición de RF-23 es sobre
`template/`, no sobre la copia local `.agents/`. Los documentos del caso y la constitución están en
`.docs/` de este clon.

### 2.1 Menú / opciones / rutas de acceso
No aplica. No hay pantallas. El acceso es el comando `/sdd*` y el agente que lo ejecuta.

### 2.2 Tablas (existentes y nuevas)
No aplica. Sin base de datos ni `sql/`.

### 2.3 Archivos de código (ruta, capa)
Solo Markdown. Estos archivos de `template/.agents/agents/` y `template/.agents/commands/` se editan
(existen hoy; no se crean piezas nuevas):

| Archivo | Por qué entra |
|---|---|
| `template/.agents/agents/coordinator.md` | RF-7, RF-8, RF-14, RF-15, RF-16. Hoy relee constitución, memoria e índice para enrutar, y la sección `Fases` reescribe los procedimientos. |
| `template/.agents/agents/planner.md` | RF-6, RF-9, RF-12, RF-13, RF-17. Hoy lee el contexto entero del proyecto para cualquier documento. |
| `template/.agents/agents/implementer.md` | RF-1, RF-2, RF-12, RF-13, RF-21. |
| `template/.agents/agents/reviewer.md` | RF-3, RF-11, RF-12, RF-18, RF-19, RF-21. Conserva `validate`. No ejecuta E1–E20. |
| `template/.agents/commands/sdd.md` | RF-8, RF-15. Carga la skill y el contrato antes de elegir, y la lista del punto 1 no incluye `sdd-estructura`. |
| `template/.agents/commands/sdd-spec.md` | RF-9, RF-14, RF-16. |
| `template/.agents/commands/sdd-clarify.md` | RF-20. El diff no puede ser el único medio si no hay Git. |
| `template/.agents/commands/sdd-plan.md` | RF-17. |
| `template/.agents/commands/sdd-tasks.md` | RF-17. |
| `template/.agents/commands/sdd-implement.md` | RF-1, RF-2, RF-16. |
| `template/.agents/commands/sdd-validate.md` | RF-3, RF-11, RF-19. |
| `template/.agents/commands/sdd-flujo.md` | RF-6, RF-19. |
| `template/.agents/commands/sdd-cierre.md` | RF-4, RF-15, RF-16. Ya revisa deuda y ADR; pasa a localizar pasos sin leer las tareas enteras. |
| `template/.agents/commands/sdd-change.md` | RF-14, RF-16. El procedimiento del cambio vive aquí; deja de repetir el formato de tiempos. |
| `template/.agents/commands/sdd-status.md` | RF-5. |
| `template/.agents/commands/sdd-init.md` | RF-10. Declara el tope de 30 archivos o 150 KB. |
| `template/.agents/commands/sdd-constitucion.md` | RF-10. Remite a ese tope. |
| `template/.agents/commands/sdd-estructura.md` | RF-18. Hoy delega en el reviewer. |

Otras piezas que el feature edita, fuera de esas dos carpetas:

| Archivo | Capa | Rol en el cambio |
|---|---|---|
| `template/.agents/skills/sdd/SKILL.md` | Producto | La tabla solo asocia fase, documento y rol (RF-14, RF-15, RF-18). |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Producto | Dueño de las reglas de tiempo (RF-16, RF-22). |
| `template/.agents/skills/seguridad-basica/SKILL.md` | Producto | Modo "solo secretos", una sola vez (RF-11). |
| `template/AGENTS.md` | Producto, grupo B | Secciones que cada rol lee. Llega a los proyectos por fusión manual (RF-12). |
| `AGENTS.md` (raíz del clon) | Conocimiento propio | El mismo texto genérico que `template/AGENTS.md`. La frase del ancho (3 en este clon, 4 en un proyecto) sigue la regla de E12 y es la diferencia admitida. |
| `template/.agents/validar-estructura.md` | Producto | Una frase en Alcance: la sesión principal corre E1–E20. No se toca la definición de E20 ni el qué de cada chequeo. |
| `CHANGELOG.md` | Docs de la raíz | Una línea en la entrada 0.5.0. |
| `manifiestos/0.5.0.md` | Docs de la raíz | Se regenera después de congelar `template/`, con el algoritmo ya definido. No se edita a mano. |
| `.docs/002_specs/003_OptimizarArnes/05_avance.md` | Esta carpeta | La sesión principal registra la línea base (F0) y la confirmación de coordinación (F1). |

No se editan `plantillas.md` (dueño del contenido de cada documento), `ACTUALIZAR.md`, `INSTALAR.md`,
ADR 007, ADR 008, la constitución, la carpeta `001_ActualizarKit` ni el archivo de la deuda 003.

### 2.4 Código legado o sin uso (no tocar, sí documentar)
La sección `Fases` de `coordinator.md` y el cuerpo de varios comandos repiten la secuencia que ya
está en otro sitio. No se borra el agente ni el comando: se deja la secuencia en el comando de la
fase y el resto solo enlaza (RF-14). El frontmatter de permisos se documenta en la deuda 003 y no
se reescribe.

## 3. Datos reales (consultas de solo lectura, con fecha y origen)
Fecha: 2026-10-25. Origen: `wc -c` sobre el archivo, sin modificarlo. No hay consulta de base de
datos (`AGENTS.md` › Comandos: no aplica). Umbral de "menos de 4 KB": 4 096 bytes.

### 3.1 Piezas del kit que hoy manda leer un rol o un comando

| Archivo | Bytes |
|---|---:|
| `template/.agents/agents/coordinator.md` | 6 679 |
| `template/.agents/agents/planner.md` | 3 510 |
| `template/.agents/agents/implementer.md` | 2 074 |
| `template/.agents/agents/reviewer.md` | 3 175 |
| `template/.agents/commands/sdd.md` | 1 361 |
| `template/.agents/commands/sdd-spec.md` | 1 630 |
| `template/.agents/commands/sdd-clarify.md` | 924 |
| `template/.agents/commands/sdd-plan.md` | 1 350 |
| `template/.agents/commands/sdd-tasks.md` | 903 |
| `template/.agents/commands/sdd-implement.md` | 1 142 |
| `template/.agents/commands/sdd-validate.md` | 1 064 |
| `template/.agents/commands/sdd-flujo.md` | 884 |
| `template/.agents/commands/sdd-cierre.md` | 1 614 |
| `template/.agents/commands/sdd-change.md` | 783 |
| `template/.agents/commands/sdd-status.md` | 658 |
| `template/.agents/commands/sdd-constitucion.md` | 1 254 |
| `template/.agents/commands/sdd-init.md` | 3 013 |
| `template/.agents/commands/sdd-estructura.md` | 900 |
| `template/AGENTS.md` | 7 798 |
| `template/.agents/skills/sdd/SKILL.md` | 8 551 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | 4 907 |
| `template/.agents/skills/sdd/plantillas.md` | 3 970 |
| `template/.agents/skills/seguridad-basica/SKILL.md` | 3 276 |
| `.docs/001_constitucion/constitution.md` | 1 111 |

`template/AGENTS.md` (7 798) está por encima de 4 096: en la lectura mínima entran secciones, no el
archivo. La constitución (1 111) es una sola sección y está por debajo: cuando un paso la cita, entra
entera. `plantillas.md` (3 970) está por debajo; el `implementer` de hoy no la nombra.

Secciones de `template/AGENTS.md` que el texto del `implementer` usa hoy (`Stack y convenciones de
código` 216, `Comandos` 289): **505**. El `AGENTS.md` de la raíz mide 8 667 y no entra en la línea
base: RF-23 mide el producto.

### 3.2 Tamaños congelados del caso `001_ActualizarKit`
`06_flujo.md` existe. `02_analisis_impacto.md` y `MEMORY.md` no estaban en la lista de congelados de
la spec y sí entran en `/sdd-status`; se congelan aquí para que esa suma se pueda repetir.

| Archivo | Bytes |
|---|---:|
| `.docs/002_specs/001_ActualizarKit/01_spec.md` | 22 394 |
| `.docs/002_specs/001_ActualizarKit/02_analisis_impacto.md` | 13 954 |
| `.docs/002_specs/001_ActualizarKit/03_plan.md` | 16 330 |
| `.docs/002_specs/001_ActualizarKit/04_tasks.md` | 29 604 |
| `.docs/002_specs/001_ActualizarKit/05_avance.md` | 24 928 |
| `.docs/002_specs/001_ActualizarKit/06_flujo.md` | 13 796 |
| `MEMORY.md` | 1 067 |

Los seis documentos que el párrafo de contexto de la spec agrupaba (spec, plan, tareas,
`template/AGENTS.md`, skill `sdd`, constitución) suman **85 788**. Ese es el "~85 KB" (~21 447 en el
cociente entero). La línea base del `implementer` le suma el agente y el comando.

### 3.3 Método de la lectura mínima (para repetirlo)
1. Un paso es el bloque `Fn.m` de `04_tasks.md`: desde su viñeta `- [x] **Fn.m` hasta la viñeta del
   paso siguiente, con su "Hecho cuando". No incluye el encabezado de fase ni "Reglas comunes".
2. RF: cada token `RF-n` del bloque (un rango `RF-1 a RF-12` nombra `RF-1` y `RF-12`). Se suma el
   texto de esa viñeta en `01_spec.md` y, una vez, cada viñeta de "Casos límite" que nombra ese
   `RF-n`. Los `RNF-n` no entran.
3. Plan: cada sección `##` de `03_plan.md` que contiene el identificador `Fn.m`, un `RF-n` del paso
   o el archivo que el paso crea o modifica. El preámbulo (141 bytes, antes de `## 1`) no es sección.
   Archivos por paso: `F0.3` `manifiestos/0.4.0.md`; `F1.1`–`F1.6` `ACTUALIZAR.md`; `F2.1`
   `template/AGENTS.md`; `F2.2` `template/.agents/README.md`; `F2.3`
   `template/.agents/validar-estructura.md`; `F2.4` la copia local (`.agents/README.md` y
   `.agents/validar-estructura.md`); `F3.1` `INSTALAR.md`; `F3.2` `README.md`; `F3.3` `AGENTS.md` de
   la raíz; `F3.4` `CHANGELOG.md`; `F4.2` y `F6.1` `manifiestos/0.5.0.md`; `F6.3`
   `.docs/002_specs/README.md`. El resto no crea ni modifica un archivo del repo.
4. En todo paso: las dos secciones de `template/AGENTS.md` (505).
5. Constitución y skill `sdd`: solo la sección que el paso o esas secciones del plan citan.
   Constitución: si el texto trae `constitución`, `constitution.md` o `principio` más un número, entra
   el archivo (1 111). Skill: un encabezado nombrado suma esa sección; una ruta sin encabezado, con
   el archivo en 8 551 (≥ 4 096), suma 0. En los 32 pasos la skill sumó 0.
6. Orden: bytes ascendente y, a igual bytes, identificador (`F0.1` < `F0.2` < … < `F6.3`). Menor =
   posición 1. Mayor = posición 32. Mediana = posición n/2 si n es par (el menor de los dos
   centrales). Aquí n = 32, posición 16. No hubo empate en esas tres posiciones. El otro central
   (posición 17) es `F2.1` con 15 215 y no se usa.
7. El respaldo de RF-1 (fase `Fn` si no hay RF; plan entero si no hay sección) no se suma a esta
   tabla. `F5.9` y `F6.2` quedan sin RF y sin sección de plan.

| Pos | Paso | Mínima | Paso | RF | Casos | Plan | AGENTS | Const. | Secciones del plan |
|---:|---|---:|---:|---:|---:|---:|---:|---:|---|
| 1 | F6.2 | 834 | 329 | 0 | 0 | 0 | 505 | 0 | — |
| 2 | F5.9 | 1 452 | 947 | 0 | 0 | 0 | 505 | 0 | — |
| 3 | F5.7 | 2 918 | 497 | 443 | 300 | 1 173 | 505 | 0 | 4 |
| 4 | F6.1 | 8 196 | 244 | 393 | 0 | 5 943 | 505 | 1 111 | 2, 7 |
| 5 | F4.1 | 8 458 | 506 | 393 | 0 | 5 943 | 505 | 1 111 | 2, 7 |
| 6 | F4.3 | 8 690 | 738 | 393 | 0 | 5 943 | 505 | 1 111 | 2, 7 |
| 7 | F2.3 | 8 906 | 954 | 393 | 0 | 5 943 | 505 | 1 111 | 2, 7 |
| 8 | F2.4 | 9 298 | 748 | 0 | 0 | 6 934 | 505 | 1 111 | 1, 2, 7 |
| 9 | F6.3 | 9 587 | 1 027 | 859 | 142 | 5 943 | 505 | 1 111 | 2, 7 |
| 10 | F5.2 | 9 949 | 794 | 987 | 609 | 5 943 | 505 | 1 111 | 2, 7 |
| 11 | F5.5 | 10 144 | 341 | 990 | 81 | 7 116 | 505 | 1 111 | 2, 4, 7 |
| 12 | F5.8 | 10 859 | 905 | 1 601 | 794 | 5 943 | 505 | 1 111 | 2, 7 |
| 13 | F5.4 | 11 975 | 916 | 2 840 | 660 | 5 943 | 505 | 1 111 | 2, 7 |
| 14 | F5.6 | 12 040 | 673 | 2 286 | 349 | 7 116 | 505 | 1 111 | 2, 4, 7 |
| 15 | F2.2 | 15 144 | 548 | 574 | 140 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 16 | F5.1 | 15 181 | 732 | 567 | 0 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 17 | F2.1 | 15 215 | 743 | 590 | 0 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 18 | F4.2 | 15 328 | 693 | 662 | 91 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 19 | F0.2 | 15 483 | 848 | 662 | 91 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 20 | F3.2 | 15 615 | 529 | 213 | 0 | 13 257 | 505 | 1 111 | 1, 2, 6, 7 |
| 21 | F0.1 | 15 629 | 1 170 | 407 | 170 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 22 | F0.3 | 15 782 | 1 147 | 662 | 91 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 23 | F1.6 | 17 018 | 505 | 213 | 0 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 24 | F5.3 | 17 326 | 893 | 1 660 | 891 | 12 266 | 505 | 1 111 | 2, 6, 7 |
| 25 | F3.4 | 17 352 | 446 | 606 | 0 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 26 | F3.3 | 17 497 | 652 | 545 | 0 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 27 | F1.3 | 17 823 | 898 | 528 | 97 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 28 | F3.1 | 18 310 | 1 230 | 780 | 0 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 29 | F1.1 | 18 719 | 1 274 | 803 | 342 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 30 | F1.4 | 19 689 | 1 341 | 1 840 | 208 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 31 | F1.2 | 19 962 | 1 061 | 1 880 | 721 | 14 684 | 505 | 1 111 | 1, 2, 6, 7, 8 |
| 32 | F1.5 | 21 248 | 1 098 | 2 028 | 649 | 15 857 | 505 | 1 111 | 1, 2, 4, 6, 7, 8 |

Composición de los tres (líneas 1-based del bloque en `04_tasks.md`):

- `F6.2` (líneas 325–328): 329 + 505 = 834. Cita `RNF-5`, ningún `RF-n`. No hay sección de plan.
- `F5.1` (líneas 252–259): 732 + RF-13 y RF-22 (567) + plan secciones 2, 6 y 7 (12 266) + 505 + 1 111 = 15 181.
- `F1.5` (líneas 122–133): 1 098 + cinco RF (2 028) + cinco casos límite (649) + plan 1, 2, 4, 6, 7 y 8 (15 857) + 505 + 1 111 = 21 248.

### 3.4 Línea base del `implementer` (lo que hoy manda leer)
Fórmula, igual para todo paso salvo la skill de dominio: el archivo del agente (2 074) +
`sdd-implement.md` (1 142) + los documentos que esa instrucción nombra, enteros:
`01_spec.md` (22 394), `03_plan.md` (16 330), `04_tasks.md` (29 604), constitución (1 111),
`template/AGENTS.md` (7 798) y `template/.agents/skills/sdd/SKILL.md` (8 551). Suma **89 004**.
No entran `avance-y-tiempos.md` ni `plantillas.md`: el texto no los nombra. El acotado "el paso" y
el paréntesis de `AGENTS.md` definen la lectura mínima (3.3), no esta línea base.

| Paso | Skill de dominio | Línea base |
|---|---|---:|
| `F6.2` | `seguridad-basica` (3 276): el paso dice aplicarla | 92 280 |
| `F5.1` | ninguna | 89 004 |
| `F1.5` | ninguna (nombra rutas para comprobar que existen) | 89 004 |

Tope para RNF-1 en la mediana: la lectura posterior ≤ 26 701 (30 % de 89 004). La mínima ya ocupa
15 181; quedan **11 520** para el texto final de `implementer.md` más `sdd-implement.md`. Hoy esos
dos archivos suman 3 216, así que el 70 % cabe sin achicarlos, siempre que la redacción nueva no
mande leer archivos fuera de esa mínima.

El menor y el mayor no tienen que bajar un 70 %. Con la mínima ya quedan debajo de su línea base
(834 y 21 248 frente a 92 280 y 89 004). El respaldo de RF-1 en `F6.2` (plan entero 16 330 + fase F6
1 635 + secciones de AGENTS 505 + constitución 1 111 + `seguridad-basica` 3 276) sigue debajo de
92 280.

### 3.5 Línea base de `/sdd-status`
El comando (658) manda leer los `01`–`06` que existan y `MEMORY.md`. En el caso existen los seis.
No hay subagente.

658 + 22 394 + 13 954 + 16 330 + 29 604 + 24 928 + 13 796 + 1 067 = **122 731**.

Tope del 70 %: ≤ 36 819. Las cuatro secciones que RF-5 conserva de `05_avance.md` miden hoy
PUNTO DE RETOMO 2 478, Pasos 45, Pruebas 11 541 y Tiempos 4 234 (suma 18 298), más la línea `Estado`
de la spec (21). Eso cabe. Leer `04_tasks.md` entero (29 604) no cabe: el conteo tiene que ser la
búsqueda de identificadores, y la medición posterior cuenta esas líneas, no el archivo.

### 3.6 Arranque de los otros roles y comandos (misma fecha, mismo `wc -c`)
Sirve para la tabla de RF-23. Cada fila es el archivo del rol o del comando más los archivos que su
texto nombra, enteros, con el caso `001` cuando la ruta es de la carpeta. `git diff` y la inspección
de código no se congelan.

| Pieza | Bytes | Qué se sumó |
|---|---:|---|
| `coordinator` | 34 533 | agente + skill `sdd` + contrato + constitución + `template/AGENTS.md` + `MEMORY.md` + índice de specs |
| `planner` | 35 063 | agente + constitución + `template/AGENTS.md` + `MEMORY.md` + `template/.agents/napkin.md` + índices de specs y deuda + `SKILL.md` + `plantillas.md`. Skills de dominio y código: no congelados |
| `reviewer` (validate, caso 001) | 75 284 | agente + `01` + `03` + `04` + `seguridad-basica` + secciones Stack y Comandos (505). `git diff`: no congelado |
| `/sdd` | 21 498 | comando + `coordinator.md` + skill + contrato. El comando elegido va aparte |
| `/sdd-spec` | 21 686 | comando + skill + contrato + constitución + `MEMORY.md` + índice de specs |
| `/sdd-implement` (solo el comando) | 64 536 | comando + plan + constitución + `template/AGENTS.md` + skill + tareas. La fila de RNF-1 es la de 3.4, que además trae el agente y la spec |
| `/sdd-estructura` | 12 194 | comando + `validar-estructura.md` + `sync-agents.md` |
| `/sdd-constitucion` | 47 364 | comando + `template/AGENTS.md` + `MEMORY.md` + napkin de `template/` + los `.md` de `template/.agents/skills/` (36 672). El código del proyecto no se congela |
| `/sdd-init` | 11 564 | comando + skill `sdd`. La inspección de código no se congela; no tiene meta del 70 % |
| `/sdd-plan`, `/sdd-tasks`, `/sdd-clarify`, `/sdd-validate`, `/sdd-flujo`, `/sdd-cierre`, `/sdd-change` | 1 350, 903, 924, 1 064, 884, 1 614, 783 | Solo el archivo del comando: delega y la lectura del rol está en la fila del rol |

## 4. Flujo actual
Un paso de implementación llega así: `/sdd-implement` (o el coordinator, que antes leyó skill,
contrato, constitución, `AGENTS.md`, `MEMORY.md` y el índice) comprueba que las tareas estén
aprobadas, delega el paso y le pide seguir el plan, la constitución, `AGENTS.md` y la skill `sdd`.
El `implementer` añade la spec, el paso dentro de las tareas y la skill de dominio que aplique.
Eso es la línea base de 3.4. Al terminar, la sesión principal escribe `05_avance.md`. El subagente
no la escribe.

`/sdd-status` abre los `01`–`06` presentes y `MEMORY.md` y responde en cinco puntos (fase y Estado,
punto de retomo, pasos x de y, horas y pausas, siguiente comando). En este caso son 122 731 bytes.

`/sdd` carga coordinator, skill y contrato aunque la petición sea `status` o `estructura`, y su
lista de comandos no nombra `sdd-estructura`. `/sdd-estructura` delega la revisión en el reviewer.
La tabla de la skill `sdd` también asigna esa fase al reviewer. El checklist E1–E20 vive en
`validar-estructura.md`; E20 compara huellas de `template/` con `manifiestos/<VERSION>.md` y no se
va a redefinir.

El mismo procedimiento está escrito en el coordinator (`Fases`), en la tabla de la skill, en el
comando y en el agente. Ya no coinciden: el cierre del coordinator no revisa deuda ni ADR; el
comando `/sdd-cierre` sí. La numeración de E12 (4 dígitos en un proyecto, 3 en este clon) está en
`template/` y en el `AGENTS.md` de la raíz con esa diferencia; no hay que unificarla en un solo
ancho.

El contrato de tiempos no exige todavía el mínimo de 1 minuto ni prohíbe el lote de filas en 0.
`001_ActualizarKit` conserva sus filas actuales.

## 5. Reglas de negocio: conocidas vs código
| Regla ya decidida | Dónde se cumple hoy | Dónde no |
|---|---|---|
| Una fuente por fase: el comando | El comando existe y delega | Coordinator `Fases`, tabla de la skill y el agente reescriben la secuencia |
| `/sdd-estructura` lo corre la sesión principal | El checklist no dice "reviewer" fila por fila | `sdd-estructura.md` delega en el reviewer; la tabla de la skill también |
| El reviewer conserva `validate` | `sdd-validate.md` y `reviewer.md` | — |
| No suponer que la herramienta inyecta `AGENTS.md` | — | Los subagentes no declaran qué secciones leen; el arranque de sesión sigue implícito |
| `AGENTS.md` es grupo B | ADR 007 y `ACTUALIZAR.md` | Los dos `AGENTS.md` (template y raíz) hay que editarlos juntos en lo genérico |
| Tiempos: mínimo 1 minuto; lote en 0 prohibido; excepción "sin trabajo real" | — | `avance-y-tiempos.md` aún no lo dice. `001` no se reescribe |
| Seguridad: skill completa si hay código ejecutable; si no, solo secretos | La búsqueda de secretos está en la sección 1 | No existe el modo "solo secretos" |
| Cierre revisa deuda y ADR | `/sdd-cierre` | El paso 9 de `Fases` en el coordinator no lo dice |
| Lista de comandos del punto de entrada incluye `sdd-estructura` | `template/AGENTS.md` › Roles y comandos sí lo nombra | `sdd.md` punto 1 no |
| E20 y el algoritmo de huella | `validar-estructura.md`, ADR 008, `ACTUALIZAR.md` | No se tocan |
| Deuda 003 fuera | Archivo de deuda ya creado | El frontmatter sigue igual, a propósito |

## 6. Riesgos y casos borde
- Otra sesión (Corlos) tiene cambios sin commit en archivos que este feature edita. F1 no empieza a
  editar hasta la confirmación. Cada edición relee el archivo y no revierte lo ajeno.
- Si `template/` cambia entre esta medición y F0, la cifra de `wc -c` de esta sección queda vieja.
  Manda la medición nueva (trampa ya anotada en `MEMORY.md`). Los documentos del caso se usan con
  los bytes de 3.2 aunque `001` cambie después.
- `F6.2` y `F5.9` no tienen RF ni sección de plan. La lectura posterior usa el respaldo de RF-1.
  El identificador de menor sigue siendo `F6.2` por la tabla 3.3. El respaldo cabe bajo su línea base.
- RNF-1 de la mediana cabe (margen 11 520 bytes para agente + comando). Si la redacción nueva manda
  leer la skill completa (8 551) o las tareas enteras, el margen se acaba.
- `/sdd-status` pierde el 70 % si la implementación lee `04_tasks.md` o `05_avance.md` enteros.
- `AGENTS.md` es grupo B: un cambio de redacción en un elemento que ya existe no se detecta como
  faltante. La línea del `CHANGELOG.md` es lo que avisa a los proyectos.
- Tras editar `template/.agents/`, E19 marcará la copia local hasta que se refresque. El refresco
  pide confirmación. Los demás chequeos de réplicas tienen que seguir pasando.
- `/sdd-init` y `/sdd-constitucion` no tienen meta del 70 %. Su tope (30 archivos o 150 KB) puede
  quedarse corto en un repo grande: se pide confirmación, no se lee sin límite.
- Dos sesiones en el mismo archivo: se relee, se edita solo la parte propia y, si cambió, se avisa.

## 7. Preguntas abiertas
Ninguna sobre el qué: la spec está aprobada y las decisiones 1 a 11 de la sección 6 del plan ya
están tomadas. Quedan tres puertas que el plan no cierra solo:

1. Coordinación (F1): con quién se acordó la edición en paralelo, quién commitea `001`, `002` y
   `003`, y qué archivos del árbol son ajenos. La nota actual del punto de retomo no es esa
   confirmación.
2. Si la fuente única por fase se registra como ADR. El plan la marca. El archivo no se crea aquí.
3. Si se refresca la copia local de `.agents/` cuando E19 difiera. Se propone al final; se hace
   solo con confirmación.
