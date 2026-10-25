# 06 — Flujo operativo: Actualizar el kit en un proyecto existente

## 1. ¿Qué es y para qué sirve?

Sirve para llevar un proyecto que ya tiene el kit a la versión del kit disponible en disco, conservando lo que el equipo personalizó. El responsable ve el plan antes de que se toque un archivo. Si el proyecto ya está al día, repetir el proceso no escribe nada.

El procedimiento que se sigue es `ACTUALIZAR.md`, junto a `INSTALAR.md`. El acceso es el agente en el proyecto. No hay pantallas, endpoints ni comando nuevo.

Instalar (`INSTALAR.md`, Modo proyecto) copia lo que falta y deja en paz lo que ya existe. Actualizar es el otro camino. La única excepción a «la copia nunca sobrescribe» es reemplazar un archivo KIT-VIEJO ya confirmado (ADR 007). Los manifiestos viven en `manifiestos/<versión>.md`, fuera de `template/`, y no se copian al proyecto (ADR 008). La huella de cada archivo es la de `ACTUALIZAR.md`.

## 2. ¿Quién lo usa? (roles y permisos)

No hay permisos de aplicación ni roles de sistema. Quien habla con el agente en el proyecto puede pedir la actualización. Aplicar un cambio exige la confirmación explícita del responsable en esa conversación.

- **Responsable del proyecto.** Decide qué se copia, qué se reemplaza y qué se mantiene.
- **Agente.** Sigue `ACTUALIZAR.md`, presenta el plan y espera la confirmación. No decide por el responsable.
- **Mantenedor del kit.** Publica cada versión con su manifiesto, para que un proyecto pueda actualizarse. Refrescar el clon del kit es `INSTALAR.md`, sección Modo mantenimiento, y ese modo no escribe marcador.

## 3. ¿Cuándo aplica?

Aplica cuando el destino ya tiene el kit: existe `.agents/.kit-version` o `.agents/skills/sdd/SKILL.md`. En ese caso se sigue `ACTUALIZAR.md` y no se reinstala.

La versión destino es la línea de `VERSION` del kit local. El kit trae manifiesto de 0.4.0 y de 0.5.0. Si la versión base es igual a la destino y no hay pendientes registrados, el proyecto está al día y no se modifica nada.

El agente se detiene sin modificar nada cuando ocurre cualquiera de estas situaciones:

- La raíz del destino tiene `template/`, `VERSION` y `manifiestos/`. Ese destino es el clon del kit. Su refresco es `INSTALAR.md`, sección Modo mantenimiento, y ese modo no escribe marcador.
- No existe el manifiesto de la versión destino.
- El entorno no puede calcular la huella, o la prueba de humo de `ACTUALIZAR.md` no coincide.
- Las huellas de `template/` no coinciden con el manifiesto de la versión destino.
- El marcador es mayor que la versión destino.

Si el proyecto todavía no tiene el kit, el camino es instalar (`INSTALAR.md`, Modo proyecto). Esa instalación escribe `.agents/.kit-version` con la línea de `VERSION`. El modo mantenimiento no lo escribe.

## 4. Flujo paso a paso (acceso → pantalla → acción → resultado)

No hay pantalla. El acceso es el agente abierto en el proyecto, con el kit en local (`template/`, `VERSION`, `manifiestos/`) y `ACTUALIZAR.md` junto a `INSTALAR.md`. La «pantalla» es esa conversación. El responsable pide actualizar.

1. **Precondiciones.** El agente comprueba el kit local, el manifiesto destino, que pueda calcular la huella y que el destino no sea el clon. Corre la prueba de humo de `ACTUALIZAR.md` en un directorio temporal, fuera del kit y del proyecto, y contrasta `template/` con `manifiestos/<VERSION>.md`. Si algo falla, informa y no modifica nada. Si pasa, la versión destino es la línea de `VERSION`. No propone cambios hasta conocer también la versión base.

2. **Versión base.** Lee `.agents/.kit-version`.
   - Una sola línea numérica es la base. Se toleran BOM, espacios y el salto de línea final.
   - Si no hay marcador, asume 0.4.0, dice que es un supuesto y pide confirmación antes de seguir. Si el responsable indica otra versión, usa esa. Si no la sabe o no responde, la trata como base sin manifiesto o se detiene sin modificar nada.
   - Vacío, varias líneas, solo espacios o no numérico es malformado: se trata como ausente, se informa y el archivo no se modifica hasta el final.
   - Marcador mayor que el destino: aviso y no se cambia nada.
   - Base sin manifiesto (anterior a 0.4.0, o una versión como 0.4.1): cero KIT-VIEJO. Se clasifica con «B no existe» y se avisa.

3. **Grupos.** La ruta es relativa al proyecto. Solo el grupo A entra en la clasificación.
   - **A.** `.agents/**`, salvo exactamente `.agents/napkin.md`.
   - **B.** Exactamente `AGENTS.md`, `CLAUDE.md` y `.gitignore`. No se reemplazan ni se clasifican. Solo se informan los faltantes, por identidad del elemento (nombre de la skill, propósito del paso, regla), no por la redacción. `CHANGELOG.md` guía lo que se agregó. El responsable los incorpora a mano o los marca «mantener».
   - **C.** `MEMORY.md`, `.agents/napkin.md`, todo `.docs/` y cualquier ruta sin otra regla. Si falta, se ofrece como NUEVO. Si existe, se respeta entero.
   - **D.** Exactamente `PREFERENCIAS.md`. No se toca ni se muestra. No está en `template/`. Si falta, lo crea el paso 10 de `AGENTS.md` durante la instalación, no esta actualización.
   - Un archivo de B o C que el proyecto eliminó se ofrece como NUEVO, con el aviso de que pide personalización posterior, o queda en «mantener».

4. **Clasificación del grupo A.** B es la huella en el manifiesto de la base, D la del destino y P la del archivo en el proyecto. Se toma la primera fila que cumple.
   1. Falta en el proyecto y existe en el destino: **NUEVO**. Se propone copiar.
   2. Falta en el destino y existen proyecto y base: **RETIRADO**. Solo se informa. Si P difiere de B, se avisa que además fue modificado.
   3. Existe en el destino y P = D: **IGUAL**. Sin acción.
   4. Existe en el destino, P ≠ D, existe la base y P = B: **KIT-VIEJO**. Se muestra el archivo del proyecto contra el de la misma ruta en `template/` y se propone reemplazar.
   5. El resto con destino y P ≠ D: **PERSONALIZADO**. Si B existe y B ≠ D, no hay texto de la base: se muestran las huellas B y D y el contenido del archivo de esa ruta en `template/`, sin inventar el texto viejo. El responsable lo fusiona a mano o lo marca «mantener». Si B = D o B no existe, no hay cambio del kit que mostrar: se respeta y se informa.
   Un archivo del proyecto que no está en el manifiesto de la base ni en el del destino es huérfano: no se clasifica, no se toca y se informa aparte. Ante duda, se trata como personalizado.

5. **Plan y confirmación.** Antes de modificar cualquier archivo, el agente muestra el plan completo agrupado por clase y espera. La confirmación es por archivo o por grupo de clase cuyas diferencias ya se mostraron; confirmar el grupo confirma cada archivo de ese grupo. Cualquier ítem se puede excluir con «mantener». Sin confirmación no se aplica nada. Un rechazo total deja el proyecto y el marcador como estaban.

6. **Aplicación.** Se copia cada NUEVO confirmado desde `template/` y se reemplaza cada KIT-VIEJO confirmado. Ese reemplazo es la única excepción a «la copia nunca sobrescribe». PERSONALIZADO queda igual. RETIRADO se conserva. Si la ruta de un NUEVO está ocupada por un directorio, un enlace o una carpeta real de herramienta donde iría un enlace, queda pendiente: no se sobrescribe ni se mueve. El estado `OCUPADO` de `template/.agents/sync-agents.md` no se borra ni se mueve.

7. **«Mantener».** Cada decisión agrega una línea a `.agents/.kit-mantener`:

   `ruta o ruta › elemento | clase | huella del destino al decidir | versión destino | AAAA-MM-DD`

   En el grupo B la huella es la del texto de ese elemento en `template/`. El ítem se vuelve a proponer solo si cambia esa huella. Incluye un NUEVO que el proyecto borró a propósito.

8. **Marcador.** `.agents/.kit-version` se escribe con la versión destino solo al final, cuando cada ítem quedó aplicado o en «mantener» explícito y no quedan pendientes: PERSONALIZADO con cambio del kit sin resolver, faltantes del grupo B sin incorporar ni mantener, y conflictos. Una aprobación parcial o un rechazo total no lo mueven. Una segunda corrida al día, con el proyecto al día, no escribe nada. Si la corrida se cortó después de reemplazar y antes del marcador, al repetir lo ya aplicado sale IGUAL y solo se propone lo que falta.

9. **Reporte, enlaces y commit.** El reporte separa nuevos, reemplazados, IGUAL, respetados, pendientes y retirados. Respetados reúne el PERSONALIZADO sin cambio del kit que mostrar y lo marcado «mantener». Pendientes reúne el PERSONALIZADO con cambio del kit sin resolver, los faltantes del grupo B sin resolver y los conflictos. Los huérfanos van aparte y no entran en esas cuentas. Después se verifican los enlaces según `template/.agents/sync-agents.md`. Si faltan o están mal, se propone corregirlos. Si todo está bien, no se dice nada. Si el proyecto usa Git, propone un commit aparte, solo con los archivos que la actualización tocó, y antes avisa si el árbol ya tenía cambios sin guardar. No se crean enlaces ni el commit sin confirmación. Sin Git, el commit no aplica y el marcador sí puede escribirse. El respaldo de un archivo reemplazado, si se hace, queda fuera del proyecto.

Las cuentas dependen del proyecto. El ensayo de F5, en un directorio temporal y nunca en un proyecto real, midió esto: limpio en el grupo A, NUEVO 5, KIT-VIEJO 16, IGUAL 11; con las seis personalizaciones de ese ensayo, NUEVO 6, KIT-VIEJO 15, IGUAL 10, PERSONALIZADO 1, RETIRADO 0, conflicto 1.

## 5. Estatus y transiciones

No hay máquina de estados de negocio: los únicos estados del procedimiento son la clase de cada archivo (NUEVO, KIT-VIEJO, IGUAL, PERSONALIZADO o RETIRADO) y el marcador `.agents/.kit-version`, que pasa a la versión destino solo cuando cada ítem quedó aplicado o en «mantener» y no hay pendientes.

## 6. Notificaciones (quién recibe qué)

No hay notificaciones: el agente muestra el plan, pide la confirmación y entrega el reporte en la misma conversación.

## 7. Piloto / liberación (si aplica)

No hay piloto ni liberación por usuarios: el procedimiento entra en la versión 0.5.0 del kit y el ensayo se hizo en un directorio temporal, nunca en un proyecto real.

## 8. Errores frecuentes y qué hacer

| Situación | Qué hacer |
|---|---|
| El destino es el clon (`template/`, `VERSION` y `manifiestos/` en la raíz) | Detenerse. El refresco del clon es `INSTALAR.md`, Modo mantenimiento, y ese modo no escribe marcador. |
| No hay manifiesto de la versión destino, o `template/` no coincide con ese manifiesto | Avisar y no modificar nada. |
| El entorno no calcula la huella, o la prueba de humo no da las tres huellas de `ACTUALIZAR.md` | Avisar y no modificar nada. |
| No hay marcador | Asumir 0.4.0, decir que es un supuesto y pedir confirmación. Sin respuesta o sin versión conocida: base sin manifiesto, o detenerse. |
| Marcador vacío, de varias líneas, solo espacios o no numérico | Tratarlo como ausente. No modificar ese archivo hasta el cierre. BOM y espacios alrededor de una versión numérica siguen siendo válidos. |
| Marcador mayor que la versión destino | Avisar y no cambiar nada. |
| Base sin manifiesto | Cero KIT-VIEJO. Clasificar con «B no existe» y avisarlo. |
| Ruta de un NUEVO ocupada por directorio, enlace o carpeta real de herramienta | Conflicto pendiente. No sobrescribir ni mover. `OCUPADO` no se borra. |
| El kit cambió un PERSONALIZADO y no hay texto de la base | Mostrar las dos huellas y el archivo de esa ruta en `template/`. No inventar el texto viejo. El responsable fusiona a mano o marca «mantener». |
| Aprobación parcial, rechazo total, o corte antes de escribir el marcador | El marcador se queda. Lo ya aplicado, al repetir, sale IGUAL. |
| Segunda corrida al día, ya al día y sin pendientes | No escribe nada. |
| El proyecto no usa Git | El commit no aplica. El marcador sí puede escribirse al cerrar sin pendientes. |
| El árbol de Git ya tenía cambios | Avisar antes de proponer el commit. El commit no se crea sin confirmación. |
| Fin de línea, BOM o espacios finales distintos | La huella coincide. No se trata como personalización. |
| El nombre solo cambia en mayúsculas, en un sistema que no las distingue | Avisar y no duplicar el archivo. |
| El proyecto movió o renombró una carpeta del kit | La ruta original se propone como NUEVO y la carpeta en el sitio nuevo se informa como huérfano. No se adivina el renombre. |

## 9. Fuera de alcance

- Fusión automática de archivos del grupo B o PERSONALIZADO.
- Retroceder (downgrade) a una versión anterior del kit.
- Etiquetar o publicar versiones del kit más allá de entregar el manifiesto de cada una.
- Capa de usuario o configuración personal fuera del proyecto.
- Reconstruir el contenido de las versiones 0.1.0 a 0.3.0.
- Actualizar el propio clon del kit. Sigue en `INSTALAR.md`, Modo mantenimiento.
- Un comando nuevo para actualizar.
- Actualizar varios proyectos en una sola ejecución.

## 10. Referencias (01_spec, 03_plan, pantallas)

- Spec: `.docs/002_specs/001_ActualizarKit/01_spec.md` (implementada).
- Plan: `.docs/002_specs/001_ActualizarKit/03_plan.md`.
- Procedimiento que este flujo opera: `ACTUALIZAR.md` (huella, prueba de humo y ejemplos por sistema).
- Instalación, para distinguirla de actualizar: `INSTALAR.md`. Modo proyecto escribe el marcador; si el destino ya tiene el kit, remite a `ACTUALIZAR.md`. El refresco del clon queda en la sección Modo mantenimiento de ese archivo.
- Decisiones: `.docs/004_adr/007_ActualizarSoloKitViejo.md` y `.docs/004_adr/008_FormatoManifiestosHuellaMarcador.md`.
- Ensayo y cuentas medidas: `.docs/002_specs/001_ActualizarKit/05_avance.md` (prueba F5).
- Enlaces y estado `OCUPADO`: `template/.agents/sync-agents.md`.
- Pantallas: no hay.
