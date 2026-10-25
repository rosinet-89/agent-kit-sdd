# 05 — Avance: Actualizar el kit en un proyecto existente
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 12:53 |
| Fase actual | `cierre` hecho |
| Último paso cerrado | `cierre` el 2026-10-25 12:50; las tres trampas quedaron en `MEMORY.md` a las 12:53 |
| Siguiente paso | Ninguno en el plan. Los commits siguen sin hacerse |
| Bloqueos abiertos | Ninguno |
| Coordinación | 2026-10-25: otra sesión cerró la deuda 001 (refresco del clon) y la deuda 002 (spec retroactiva `002_Kit050`). No editar `INSTALAR.md` desde "## Modo mantenimiento" hasta el final, ni la fila E19 de `validar-estructura.md` (en `.agents/` y en `template/.agents/`), ni `.docs/003_deuda/001_ActualizarCloneDelKit.md`, ni `.docs/002_specs/002_Kit050/`. El número 002 de specs ya está usado. El clon y el registro de 0.5.0 ya publicado siguen fuera de esta spec. El resto de `INSTALAR.md` (modo proyecto y el enlace al procedimiento de actualización) y el changelog de 0.5.0 para este requerimiento siguen en este plan. Numeración de proyectos: specs, deudas y ADR pasan a 4 dígitos en `template/` (`0001`); no renumerar las carpetas de este clon (siguen en 3, ADR incluidos) ni la fila E12 más allá de ese ancho. |
| Traspaso (Claudio → Corlos) | Claudio documentó spec, análisis, plan y tareas y no implementó. **Pendiente al retomar:** (1) tras tu trabajo de 4 dígitos en `template/`, `03_plan.md`/`04_tasks.md` siguen válidos pero hay que releer los pasos F2.2, F2.3 y F3.x que editan `template/AGENTS.md`, `.agents/README.md`, `validar-estructura.md` e `INSTALAR.md` (parten del contenido actual; no revertir). (2) `02_analisis_impacto.md` tomó sus cifras (41 archivos en `template/`, oráculo 6 NUEVO / 8 KIT-VIEJO / 19 IGUAL) el 2026-10-24; con los cambios de hoy hay que recontarlas antes de F0. (3) Hecho 2026-10-25 11:55: en KIT-VIEJO se compara el archivo del proyecto (P = B) con `template/`; en PERSONALIZADO con cambio del kit solo se muestran las huellas y el archivo destino, sin inventar el texto de la base. (4) F5 (3,5 h) y F5.4 son los pasos más densos. (5) Hecho 2026-10-25 10:57: la spec y el análisis ya dicen la deuda 001 resuelta y fuera de alcance. (6) 0.5.0 sigue sin subirse (7 commits locales): este feature sale dentro de 0.5.0. Decisiones 1 a 16 de abajo siguen vigentes. |
| Estado BD | No aplica |
| Código | Ninguno (cambios ajenos sin registrar: sección "Refrescar la copia local" en `INSTALAR.md` y E19 en `validar-estructura.md`; ver deuda 001) |

## Pasos
(Se llena al tener `04_tasks.md`.)

## Decisiones
1. **[2026-10-24 16:33] Se usa el flujo SDD para este feature, con carpeta propia.**
   Por qué: el trabajo de 0.5.0 se hizo sin spec, plan ni avance y contradijo el principio 2 de la
   constitución. Alternativa descartada: seguir editando `template/` directo.
2. **[2026-10-24 16:39] Versión base por manifiesto de versión dentro del kit** (ruta + huella o resumen de cambios), sin depender de Git ni tags; el de 0.4.0 se genera desde el commit 861bb99. Alternativas descartadas: solo `git show` (sin tags, falla sin historial) y comparar solo contra el kit nuevo (nunca propone reemplazos).
3. **[2026-10-24 16:39] Sin marcador se asume 0.4.0 con confirmación del usuario**; lo anterior a 0.4.0 se trata como PERSONALIZADO. Alternativas descartadas: preguntar la versión (el usuario suele no saberla) y todo PERSONALIZADO.
4. **[2026-10-24 16:39] Marcador de una línea en la carpeta de agentes del proyecto, versionado.** Alternativas descartadas: en `AGENTS.md` (se fusiona a mano) y local (bases distintas por dev).
5. **[2026-10-24 16:39] Procedimiento como documento propio enlazado desde el de instalación**, sin comando nuevo. Alternativa descartada: `/sdd-actualizar` (no existe en proyectos 0.4.0, justo el caso a resolver).
6. **[2026-10-24 16:39] Actualizar el clon del kit en modo mantenimiento queda fuera**; se propone como deuda.
7. **[2026-10-24 16:45] Este feature sale dentro de 0.5.0** (commits locales, sin push; ningún proyecto real la tiene). 0.5.0 lleva marcador y su manifiesto se genera con el estado final. Alternativas descartadas: 0.5.1 y 0.6.0 (un proyecto con 0.5.0 se asumiría 0.4.0).
8. **[2026-10-24 16:45] Excepción a "la copia nunca sobrescribe"** solo para archivos KIT-VIEJO, con confirmación explícita y diff. Requiere enmendar el principio 5 y el ADR 007. Alternativa descartada: nunca reemplazar (no resuelve skills sin conectar).
9. **[2026-10-24 16:45] El marcador avanza solo si cada archivo quedó aplicado o con "mantener" explícito.** Alternativas descartadas: avanzar siempre (se pierde lo no fusionado) y dos marcadores (un archivo más).
10. **[2026-10-24 16:47] Spec aprobada por el usuario** (`Estado: aprobada`), con 25 RF, 9 RNF y sin dudas abiertas.
11. **[2026-10-25 10:39] El modo mantenimiento no escribe marcador en el clon del kit.** La deuda 001 cubre el clon y E19 detecta el desfase. Alternativa descartada: marcador también en el clon (amplía alcance, se mezcla con las exclusiones locales).
12. **[2026-10-25 10:39] Faltantes del grupo B sin Git: lista conservadora** que el usuario resuelve con incorporar o "mantener". Alternativa descartada: exigir Git (choca con RNF-7).
13. **[2026-10-25 10:39] Se registrará un ADR 008 (formato de manifiestos, huella y marcador) en F6**, propuesto por `/sdd-cierre` y con visto bueno.
14. **[2026-10-25 10:39] El hueco 2026-10-24 16:49 a 2026-10-25 10:29 (≈17 h 40 min) se declara pausa completa** por el usuario; el relanzamiento del planner (16:49) no se cuenta.
15. **[2026-10-25 10:47] Solo documentación: plan y tareas NO se aprueban por ahora y no se implementa.** La implementación queda para Corlos (otro agente, sesión del usuario). Claudio (esta sesión) cierra aquí. Por qué: decisión del usuario. Alternativa descartada: aprobar y seguir con `/sdd-implement F0.1`.
16. **[2026-10-25 10:47] Se confirmó que la sesión que cerró las deudas 001 y 002, creó `002_Kit050` y pasó la numeración de proyectos a 4 dígitos es Corlos**; se respetan sus límites (ver fila Coordinación).
17. **[2026-10-25 10:57] El usuario aprobó `03_plan.md` y `04_tasks.md`.** Se arranca `/sdd-implement 001_ActualizarKit F0.1`. Alternativa descartada: seguir sin aprobar. La cita de la deuda 001 en la spec pasa a `Resuelta` y sigue fuera de alcance; no cambia requisitos.
18. **[2026-10-25 11:07] El entorno de trabajo es Ubuntu; por eso no hay PowerShell.** El ejemplo de PowerShell en `ACTUALIZAR.md` queda como texto no probado en esta máquina. Alternativa descartada: instalar PowerShell para probarlo.
19. **[2026-10-25 12:07] El usuario aprobó `ACTUALIZAR.md`**, con el índice de 25 secciones añadido a su pedido. F1 queda cerrado. Alternativa descartada: seguir a F2 sin ese visto bueno.
20. **[2026-10-25 12:16] El usuario dio VoBo a `INSTALAR.md`.** F3 queda cerrado. El modo mantenimiento no se editó y no escribe marcador.
21. **[2026-10-25 12:27] El usuario aprobó el informe de F5.** Las cuentas medidas (NUEVO 6, KIT-VIEJO 15, IGUAL 10, PERSONALIZADO 1, RETIRADO 0, conflicto 1) sustituyen al oráculo 6 / 8 / 19. F5 queda cerrado.
22. **[2026-10-25 12:29] Las deudas 001 y 002 siguen Resueltas.** El texto de F6.3 pedía citarlas como abiertas; no se reabren (decisión 17 y la coordinación). No se crea el ADR 008 ni se hace commit hasta el visto bueno.
23. **[2026-10-25 12:30] El usuario confirmó el cierre y rechazó los commits.** F6 y el plan quedan cerrados. El ADR 008 no se crea: no hubo visto bueno. Alternativa descartada: commitear igual.
24. **[2026-10-25 12:31] El usuario pidió crear el ADR 008.** Quedó aceptado en `.docs/004_adr/008_FormatoManifiestosHuellaMarcador.md`. Los commits siguen sin hacerse.
25. **[2026-10-25 12:50] El usuario aprobó `06_flujo.md`.** El reviewer había pedido tres correcciones y ya estaban aplicadas. Siguiente fase: `/sdd-cierre`.
26. **[2026-10-25 12:50] Cierre aplicado.** `01_spec.md` pasa a `implementada`. Las deudas 001 y 002 siguen Resueltas y no las resolvió esta spec. No se abre deuda nueva. Los commits siguen sin hacerse. Las entradas de `MEMORY.md` se proponen y no se escriben.
27. **[2026-10-25 12:53] El usuario aprobó las tres trampas.** Quedaron en `MEMORY.md`. Los commits siguen sin hacerse.

## Pruebas
- **F0.1 (2026-10-25 11:05).** Misma huella para LF y para BOM+CRLF+espacios finales: `fa0bc48477b5b4d064f39b799e388c78e64c80fe29f08aee6686a8c842112630`. Palabra distinta: `6dcdde67a08228b75fac8acbe91cf6653b1a7679025e2493f5d9d8c3190db398`. Vacío: `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`. Reprobado en esta sesión con la misma función. PowerShell no está en el entorno (no probado). `shasum -a 256` del Linux coincidió sobre bytes ya normalizados; el `sed` de macOS no se probó. `python3 -I` coincidió en las tres. Un archivo que termina en CR sin LF no forma parte de la prueba: `sed` lo quita y Python lo conserva.

- **F0.2 (2026-10-25 11:11).** `git ls-tree` y el listado calculado: 35 rutas, `diff` vacío. Contraprueba `python3 -I` coincidió en `.docs/003_deuda/README.md`, `.agents/skills/napkin/SKILL.md` y `.agents/skills/sdd/avance-y-tiempos.md`. Esta sesión recalculó `AGENTS.md` y obtuvo `2e7dd27a81c95ecddc7ae22f821c422f6f38a56a7cd6fcacee8e7daa07a8d352`. Tabla para F0.3 (ruta, tabulador, huella):

```
.agents/README.md	aa9556a9f6b4cc1073af0ed64371f842b7552e028c7f926142bd0ac9a304a292
.agents/agents/coordinator.md	aeeec987154ee02f95c6345c47fde96441a831fffb7a111c61e04bdf7580b451
.agents/agents/implementer.md	86ab23ed0cc9c3a10eff968f9695fd44405ae1f190fe59f9ef3d44a706a742b0
.agents/agents/planner.md	a54efccae895a853620ce5a4981826eb77e0fd2fea1c63825cdb78e6e0277a68
.agents/agents/reviewer.md	c4a9796437b3ed89d6d4625ef384de6f7d78edc734db6ba5e75699e1fc43753e
.agents/commands/sdd-change.md	a32be7e3506ef52ee41795d29a50c601984691e17d24b9b09ceca40270af3f2e
.agents/commands/sdd-cierre.md	b7f2c09ed6208b1f8f166456e9dc692530633c917e5d6840f99b16001c2ceba3
.agents/commands/sdd-clarify.md	b343a800f0afa6661c63b432478ed9434b76e30d17a8311544498f4727b28dfc
.agents/commands/sdd-constitucion.md	f64c56510b80ae67282b62f6e765a1b1e5408ec1ad5ebf6d2727613bb8eaafb1
.agents/commands/sdd-flujo.md	55acf683303ecef0b74ccee641dd0b73f6e2cf1559a3fe8ed300ab9c75a25e8a
.agents/commands/sdd-implement.md	aa7bd5369e3e7ef606a8b87f6a28ca868acfb7ffdd485949163ad1147357fb4d
.agents/commands/sdd-init.md	9bea26e7f51675531eb3c1aa48b93ba982b008c1883b4af687ba31fa721b4b55
.agents/commands/sdd-plan.md	66396cdcff604b2b2719d4f34132878635f06347f2aa600e3a856aaeeb3f078d
.agents/commands/sdd-spec.md	91dc708e24a076831803165bd70feeb4f0e695e72a974a7c147db021989e0b14
.agents/commands/sdd-status.md	b39e39393318c504fec63db609e17e7983c84cfccb4c4232624196ff096d7901
.agents/commands/sdd-tasks.md	cc9d290f12118d2cff9e335d10173f82434ba5bdb13d40494e8900342ed47765
.agents/commands/sdd-validate.md	2df9b76c792c5c78873c491d4bd2b239558f2a35f1eb5aa116e624538dfc5ead
.agents/commands/sdd.md	d10aa283be4d6f87616adc8b79ad7e2057156c3716aa21c939260de6a6d34fb8
.agents/napkin.md	5ff1b10c8a93c24915e435122a511c3729ce2788a4f79ad0c2bd0a533220956f
.agents/preferencias.ejemplo.md	62c927fffcc1a407f5c09a32f31144bc8d6b4351cc79be25c1a848d79a99bbea
.agents/skills/formato-respuesta/SKILL.md	8755f2a346cabb242ff1f41e9856bc37665cf088285af709fb6ca17e5ae127ce
.agents/skills/git-commit/SKILL.md	6bae6cf8d99bd6a79107a70179ba822fb6fa251bf062c36d803a6936fd16b1e3
.agents/skills/napkin/LICENSE	311df79b86fcebfe76d3267fb4f786888b6263aa028488b1aa1e9bea3cb95836
.agents/skills/napkin/SKILL.md	d5966178cbbe53c05905fada7c690ef20d5b640f7ec75965a0adf58bf1a5fd6a
.agents/skills/sdd/SKILL.md	b8198a4b06ed68e392c58e7fc7c63aaf991f17a0d1851a4459ae62600e12cf28
.agents/skills/sdd/avance-y-tiempos.md	f976c39d046e1a20b0767e396b0db9219926346a188becc8d81e95e394dabbdc
.agents/skills/sdd/plantillas.md	46354f609859578c9d1700d79ce61dfc631e8a94613812c0c43ff76442ce6063
.agents/sync-agents.md	98006accfae21b57cf752c3f52b98be82319f3de3b17b952095b1e35bbcb7962
.docs/001_constitucion/constitution.md	f75685495c792991d30e027d77a6a0f9b4b1cce3abe9ca33beff621834dec1b1
.docs/002_specs/README.md	8370a85ab4d359cd5ea79241ed6f3557eda284327d80934f971ed0b804ca4340
.docs/003_deuda/README.md	159d9be47abda39740e5d1832d1f70e485d241acfa6f675be565327f5876b679
.gitignore	f1131a8cef9da0c94790052ed7df4088626fbad1c38baeed565fddb91b6a76c0
AGENTS.md	2e7dd27a81c95ecddc7ae22f821c422f6f38a56a7cd6fcacee8e7daa07a8d352
CLAUDE.md	336cc4fbf19beaada7ccf9986414fa91851a8d7a07dfb3ccbe800a69eed0ab49
MEMORY.md	879e24fb0d354f825747b1a3e89580c42dda3ef6963e80ac86818c043fcf618b
```

- **F0.3 (2026-10-25 11:16).** `manifiestos/0.4.0.md`: utf-8, sin BOM, 35 filas, 35 huellas de 64 hex, `sort -c` sin error, `diff` vacío contra `git ls-tree` de `861bb99`. Huella de `AGENTS.md` igual a F0.2. Formato confirmado por el usuario a las 11:18.
- **F1.1 (2026-10-25 11:27).** `ACTUALIZAR.md` creado. Prueba de humo copiada del documento: mismas tres huellas de F0.1. utf-8, sin BOM. Rutas `template/`, `VERSION`, `manifiestos/` e `INSTALAR.md` existen.
- **F1.2 (2026-10-25 11:44).** Tabla de 5 clases en `ACTUALIZAR.md` con las mismas condiciones que `01_spec.md`. Siete casos del marcador con instrucción. utf-8, sin BOM.
- **F1.3 (2026-10-25 11:50).** Regla por ruta en `ACTUALIZAR.md`. Conteo sobre `template/`: A=32, B=3 (`.gitignore`, `AGENTS.md`, `CLAUDE.md`), C=6, D=0. utf-8, sin BOM.

- **F1.4 (2026-10-25 11:55).** `ACTUALIZAR.md` utf-8, sin BOM. «Reemplazar KIT-VIEJO es la única excepción» aparece una sola vez, en `## Aplicación`. `grep -n OCUPADO template/.agents/sync-agents.md` devolvió las líneas 20, 23, 29, 79 y 83. F1.5 sigue sin marcar.

- **F1.5 (2026-10-25 12:02).** `ACTUALIZAR.md` utf-8, sin BOM. «única excepción» sigue en 1. Existen `git-commit`, `seguridad-basica` y `sync-agents.md`. Los 21 casos de la spec apuntan a un encabezado del documento; el 12 y el 20 se escriben en `## Casos límite y errores frecuentes`. F1.6 sigue sin marcar.

- **F1.6 (2026-10-25 12:07).** Índice de 25 secciones, mismo orden que los encabezados. utf-8, sin BOM. «única excepción» sigue en 1. El usuario respondió «yes» a la pregunta de aprobación.

- **F2.1 (2026-10-25 12:08).** `diff` contra la copia en `/tmp` muestra una sola línea añadida en Reglas duras. utf-8, sin BOM. Los `<<` que quedan son `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. `ACTUALIZAR.md` y `.agents/.kit-version` van en texto plano, sin comillas inversas. `AGENTS.md` de la raíz no se tocó.

- **F2.2 (2026-10-25 12:10).** `diff` muestra solo las dos líneas del árbol. utf-8, sin BOM, sin `<<`. Van en el bloque del árbol, sin comillas inversas. La copia local de `.agents/` no se refrescó (eso es F2.4).

- **F2.3 (2026-10-25 12:11).** `diff` contra la copia previa: solo la aclaración de E11, la fila E20 y el cierre `E1–E20`. E19 igual. `grep -c E20` = 2. No queda `E1–E19`. utf-8, sin BOM, sin frontmatter. `<<` solo los de E18, que ya estaban (`<<NOMBRE_PROYECTO>>`, `<<IDIOMA>>`). Rutas nuevas entre comillas inversas: `template/` y `ACTUALIZAR.md`, ambas existen. manifiestos/<VERSION>.md va en texto plano. La copia local no se refrescó.

- **F2.4 (2026-10-25 12:12).** Diff previo: solo `README.md`, `validar-estructura.md` y `napkin.md`. Tras copiar y restaurar, `diff -rq` muestra solo `napkin.md`. `cmp` del napkin guardado coincidió. `git status --short -- .agents` vacío.

- **F3 (2026-10-25 12:15).** Modo mantenimiento de `INSTALAR.md` idéntico al recorte previo. `cp -Rn` y `robocopy` sin cambio. `grep 'compara su' README.md` vacío. `VERSION` = `0.5.0`. ADR 007 existe. utf-8, sin BOM, en los cuatro archivos. El modo mantenimiento no escribe marcador (decisión 11, ya confirmada; no se dejó pendiente).

- **F3.1 (2026-10-25 12:16).** El usuario respondió «VoBo» a la detección, el marcador solo en Modo proyecto y el enlace a `ACTUALIZAR.md`.

- **F4 (2026-10-25 12:19).** 41 archivos en `template/` (39 `.md`). Lint: utf-8, sin BOM; `name` de skills y agentes coincide; comandos no traen `name` (E9 no lo pide). El único `<<` aparte de `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>` es la frase de E18 que nombra el patrón `<<…>>`; no es un marcador sin llenar. `manifiestos/0.5.0.md`: 41 filas, rutas iguales al `find`, 64 hex, utf-8, sin BOM. Contraste con 0.4.0: solo en 0.5.0 = 6, solo en 0.4.0 = 0, huella distinta = 19, huella igual = 16 (el oráculo 3.4 decía 6 / 0 / 8 / 19). E20 en `mktemp`: limpia OK; editar `AGENTS.md` = diferencia; restaurar = OK; borrar `.gitignore` = fila sin archivo; añadir un archivo = archivo sin fila. `git status --short template` igual que la línea base (20 archivos). Línea base:

```
 M template/.agents/README.md
 M template/.agents/commands/sdd-change.md
 M template/.agents/commands/sdd-cierre.md
 M template/.agents/commands/sdd-clarify.md
 M template/.agents/commands/sdd-flujo.md
 M template/.agents/commands/sdd-implement.md
 M template/.agents/commands/sdd-plan.md
 M template/.agents/commands/sdd-spec.md
 M template/.agents/commands/sdd-status.md
 M template/.agents/commands/sdd-tasks.md
 M template/.agents/commands/sdd-validate.md
 M template/.agents/skills/adr/SKILL.md
 M template/.agents/skills/git-commit/SKILL.md
 M template/.agents/skills/migracion-bd/SKILL.md
 M template/.agents/skills/sdd/SKILL.md
 M template/.agents/validar-estructura.md
 M template/.docs/002_specs/README.md
 M template/.docs/003_deuda/README.md
 M template/.docs/004_adr/README.md
 M template/AGENTS.md
```

- **F5 (2026-10-25 12:24).** Todo en `mktemp`. 59 comprobaciones y E19: `diff -rq` solo difiere en `napkin.md` (el mensaje salió en inglés). `template/` igual a la línea base de F4. Personalizaciones: `implementer.md` editado, `coordinator.md` borrado, `AGENTS.md` de 0.4.0 sin fusionar, huérfano `notas-locales.md`, `planner.md` en CRLF, `adr/SKILL.md` ocupado por un directorio. Limpio grupo A: NUEVO 5, KIT-VIEJO 16, IGUAL 11. Con personalización: NUEVO 6, KIT-VIEJO 15, IGUAL 10, PERSONALIZADO 1, RETIRADO 0, conflicto 1. Faltantes de `AGENTS.md`: adr, migracion-bd, seguridad-basica, `/sdd-estructura`, paso 7, línea de no reinstalar. `CLAUDE.md` y `.gitignore` iguales. Reporte al aplicar: nuevos 7 (los 6 más `adr` ya resuelto), reemplazados 15, IGUAL 10, respetados 7, pendientes 0, retirados 0, huérfanos 2 (el archivo y el contenido dentro del directorio que ocupaba la ruta). Marcador `0.5.0`. Sin commit.

- **F5.9 (2026-10-25 12:27).** El usuario respondió «yes» al informe de cuentas y al cierre de las pruebas.

- **F6 (2026-10-25 12:29).** `git status --short template` igual a la línea base de F4. E20 OK; no se regeneró el manifiesto. `seguridad-basica`: las coincidencias son la palabra «secreto» o «token» en la documentación de la skill, no valores. Sin rutas personales. Prefijos sin repetir. Índice 001 pasado a Implementado. Deudas 001 y 002 siguen Resueltas. Sin commit.

- **F6.3 (2026-10-25 12:30).** El usuario confirmó el cierre y dijo que no a los commits. ADR 008 no creado.

- **ADR 008 (2026-10-25 12:31).** El usuario pidió crearlo. Aceptado y enlazado desde el índice y desde `03_plan.md`. Sin commit.

- **flujo (2026-10-25 12:50).** El usuario respondió «yes» a `06_flujo.md`. El contraste dejó tres diferencias y el planner las corrigió antes de mostrarlo.

- **cierre (2026-10-25 12:50).** Tareas en `[x]`. Sin verificación manual pendiente en Pruebas. Flujo aprobado. Sin `pausa por confirmar`. Deudas 001 y 002 sin cambio. Sin commit.

Función probada, para copiar en F1.1:

```bash
kit_huella() {
  (
    LC_ALL=C
    export LC_ALL
    sed -e $'1s/^\xef\xbb\xbf//' -e $'s/\r$//' -e $'s/[ \t]*$//' -- "$1" \
      | awk 'NF { for (i = 1; i <= n; i++) print ""; n = 0; print; next } { n++ }' \
      | sha256sum \
      | awk 'NR==1 { print $1 }'
  )
}
```

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-24 16:33 | 2026-10-24 16:39 | 6 | 0 | entrevista (5 dudas) + 01_spec; incluye abrir carpeta |
| clarify | 2026-10-24 16:40 | 2026-10-24 16:47 | 7 | 0 | reviewer (27 hallazgos), 3 decisiones, ajustes del planner, ADR 007 y deuda 001/002 |
| plan | 2026-10-25 10:32 | 2026-10-25 10:39 | 7 | 0 | retoma tras pausa de ≈17 h 40 min declarada; planner (relanzado 16:49) escribió 02 y 03; 3 preguntas y revisión |
| tasks | 2026-10-25 10:41 | 2026-10-25 10:47 | 6 | 0 | planner escribió 04_tasks.md; revisión y cierre sin aprobar por decisión del usuario |
| F0.1 | 2026-10-25 10:57 | 2026-10-25 11:05 | 8 | 0 | prueba de humo en /tmp; solo se marcó 04_tasks.md |
| F0.2 | 2026-10-25 11:07 | 2026-10-25 11:11 | 4 | 0 | 35 huellas desde 861bb99; diff vacío; python3 en 3 archivos |
| F0.3 | 2026-10-25 11:13 | 2026-10-25 11:16 | 3 | 0 | manifiestos/0.4.0.md; formato confirmado por el usuario a las 11:18 |
| F1.1 | 2026-10-25 11:18 | 2026-10-25 11:27 | 9 | 0 | ACTUALIZAR.md cabecera; humo reejecutado desde el documento |
| F1.2 | 2026-10-25 11:39 | 2026-10-25 11:44 | 5 | 0 | marcador y tabla de 5 clases; condiciones iguales a la spec |
| F1.3 | 2026-10-25 11:45 | 2026-10-25 11:50 | 5 | 0 | grupos A=32 B=3 C=6 D=0 sobre template/ |
| F1.4 | 2026-10-25 11:51 | 2026-10-25 11:55 | 4 | 0 | plan, confirmación, aplicación, .kit-mantener y conflicto; única excepción en ## Aplicación |
| F1.5 | 2026-10-25 11:56 | 2026-10-25 12:02 | 6 | 0 | marcador al cerrar, reporte, enlaces, commit, idempotencia; 21 casos localizables |
| F1.6 | 2026-10-25 12:05 | 2026-10-25 12:07 | 2 | 0 | índice pedido y añadido; el usuario aprobó el documento |
| F2.1 | 2026-10-25 12:08 | 2026-10-25 12:08 | 0 | 0 | una línea en template/AGENTS.md; diff de una sola línea |
| F2.2 | 2026-10-25 12:09 | 2026-10-25 12:10 | 1 | 0 | dos líneas en el árbol; sin comillas inversas |
| F2.3 | 2026-10-25 12:10 | 2026-10-25 12:11 | 1 | 0 | E20, aclaración E11, E1–E20; E19 intacto |
| F2.4 | 2026-10-25 12:12 | 2026-10-25 12:12 | 0 | 0 | diff previo solo F2.2, F2.3 y napkin; al final solo napkin |
| F3.1 | 2026-10-25 12:13 | 2026-10-25 12:16 | 2 | 0 | escrito en el tramo de F3; VoBo del usuario a las 12:16 |
| F3.2 | 2026-10-25 12:13 | 2026-10-25 12:15 | 0 | 0 | mismo tramo; minutos contados en F3.1 |
| F3.3 | 2026-10-25 12:13 | 2026-10-25 12:15 | 0 | 0 | mismo tramo; minutos contados en F3.1 |
| F3.4 | 2026-10-25 12:13 | 2026-10-25 12:15 | 0 | 0 | mismo tramo; VERSION 0.5.0 sin editar |
| F4.1 | 2026-10-25 12:17 | 2026-10-25 12:19 | 2 | 0 | mismo tramo que F4.2 y F4.3; 41 archivos; línea base en Pruebas |
| F4.2 | 2026-10-25 12:17 | 2026-10-25 12:19 | 0 | 0 | mismo tramo; contraste 6 / 0 / 19 / 16 |
| F4.3 | 2026-10-25 12:17 | 2026-10-25 12:19 | 0 | 0 | mismo tramo; E20 en mktemp; template real igual |
| F5.1 | 2026-10-25 12:22 | 2026-10-25 12:24 | 2 | 0 | mismo tramo que F5.2 a F5.9; todo en mktemp |
| F5.2 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo |
| F5.3 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo |
| F5.4 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo; conteos medidos, no el oráculo 8/19 |
| F5.5 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo |
| F5.6 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo |
| F5.7 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo |
| F5.8 | 2026-10-25 12:22 | 2026-10-25 12:24 | 0 | 0 | mismo tramo |
| F5.9 | 2026-10-25 12:22 | 2026-10-25 12:27 | 0 | 0 | mismo tramo de ejecución; visto del usuario a las 12:27 |
| F6.1 | 2026-10-25 12:28 | 2026-10-25 12:29 | 1 | 0 | mismo tramo que F6.2 y F6.3; E20 OK; template igual a F4 |
| F6.2 | 2026-10-25 12:28 | 2026-10-25 12:29 | 0 | 0 | mismo tramo; sin valores de secreto |
| F6.3 | 2026-10-25 12:28 | 2026-10-25 12:30 | 0 | 0 | mismo tramo de ejecución; visto del usuario a las 12:30; sin ADR ni commits |
| flujo | 2026-10-25 12:36 | 2026-10-25 12:50 | 14 | 0 | planner, reviewer y tres correcciones; el usuario aprobó a las 12:50 |
| cierre | 2026-10-25 12:50 | 2026-10-25 12:50 | 0 | 0 | plan contra real, estado implementada, índice; sin commit |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| Documentación SDD | sin estimado | 0.67 h | — |
| F0 | 1.5 h | 0.25 h | -1.25 h |
| F1 | 4.0 h | 0.52 h | -3.48 h |
| F2 | 1.5 h | 0.03 h | -1.47 h |
| F3 | 2.0 h | 0.03 h | -1.97 h |
| F4 | 1.0 h | 0.03 h | -0.97 h |
| F5 | 3.5 h | 0.03 h | -3.47 h |
| F6 | 1.0 h | 0.02 h | -0.98 h |

El real sale de la tabla de tiempos: spec 6, clarify 7, plan 7, tasks 6 y flujo 14 suman 40 min (0.67 h); el cierre quedó en el mismo minuto. F0 a F6 suman 55 min (0.92 h) frente a 14.5 h.

En el siguiente plan de un feature que es solo documentación, estima cada fase en fracciones de hora. F1 (4.0 h) y F5 (3.5 h) se cerraron en 31 min y 2 min. Una fase del plan no sustituye a `/sdd-cierre`.
