# 05 — Avance: README del repo con flujo general del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `ligero`, confirmado por la persona (decisión 4). Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): las 8 se cumplen; la 8 con lectura (único archivo del producto `README.md`; el índice solo como registro). Lista en `01_spec.md` › «Modo» |
| Última sesión | 2026-11-22 15:27 |
| Fase actual | `cierre`: `Cerrado` (decisión 11), con el criterio 2 en los commits |
| Último paso cerrado | Cierre (decisión 11). Validación del conjunto aprobada (decisiones 6 y 9) |
| Siguiente paso | Ninguno en esta spec. La persona publica con `git push` |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | `README.md` y `.docs/002_specs/README.md`, en 2 commits locales sin push |

## Pasos
Detalle en `04_tasks.md`.

| Fase | Pasos | Estado |
|---|---|---|
| F1 Índice | F1.1 | `[x]` |
| F2 README | F2.1 | `[x]` |
| F2 README | F2.2 (RF-9) | `[x]` |

## Decisiones
1. **[2026-11-22 15:12] Origen y alcance.** La persona pide: (1) quitar del índice los enlaces a deudas
   resueltas de las filas `0012` y `0015`, (2) un flujo general del kit y su configuración, (3)
   actualizar el `README.md` (ejemplo de uso, instalar, actualizar, configuración), (4) los commits y
   (5) publicar. Se agrupan 1 a 3 en esta spec (memoria «flujo SDD obligatorio en el kit»). El punto 5
   no lo hace el agente: `AGENTS.md` › Git prohíbe `push`; se entrega `! git push` a la persona.
   «procede» de la persona a las 15:12 cubre los pasos en cascada hasta aprobar la spec.
2. **[2026-11-22 15:12] Antecedentes:** sin antecedentes en el índice (grep de `readme`, `diagrama`,
   `mermaid`, `flujo general` en `.docs/002_specs/README.md` sin filas). Deuda: ninguna abierta
   (14 de 14 `Resuelta`).
3. **[2026-11-22 15:14] Clarify, pasada 1 de 1** (reviewer en el chat, modo automático): 3 hallazgos,
   corregidos por el planner sin decisiones de alcance. (a) RF-5 citaba rutas locales
   (`.agents/local/…`, `PREFERENCIAS.md`) que no existen en el repo y harían fallar el lint de rutas:
   se enlaza la plantilla `template/.agents/*.ejemplo.md` y se añade el caso límite. (b) RF-8 decía
   «cuando se cierre» y la limpieza se hace al implementar: cambia a «cuando se implemente». (c) RF-1
   omitía `/sdd-enlaces` tras actualizar: se añade.
4. **[2026-11-22 15:16] Spec aprobada** por la persona («Aprobar tal cual»): modo ligero confirmado y
   supuestos 1 a 3 aceptados (flujo dentro de `README.md`, sin bump, español).
5. **[2026-11-22 15:17] Tareas aprobadas** por la persona; cascada hasta el cierre. F1.1 y F2.1 van a
   `implementer` como subagentes en segundo plano (rol en Sonnet, preferencia 5), en paralelo por
   tocar archivos disjuntos (índice y `README.md`).
6. **[2026-11-22 15:19] Validate aprobado** (reviewer en el chat, claude-opus-5-5; produjo el
   implementer en Sonnet). Una vuelta de corrección pedida al implementer: enlace de `topes-y-modos.md`
   a `template/`, fila `07_casos_prueba.md` y la constitución en «Quién lee qué». Evidencia: RF-1
   (Mermaid y lista de 6 pasos), RF-2 (ciclo con 3 puertas y `sql/`, completo contra ligero, tabla
   01–07), RF-3 (Instalar remite a `INSTALAR.md`), RF-4 (`/sdd-version`, KIT-VIEJO, `/sdd-enlaces`,
   `ACTUALIZAR.md`), RF-5 (7 piezas, locales sin enlace y plantillas enlazadas, Mermaid), RF-6
   (ejemplo de 8 líneas), RF-7 (18 comandos y 6 roles = `ls template/.agents/…`, diff vacío), RF-8
   (diff del índice: solo las 2 celdas y la fila 0018). Lint: UTF-8 sin BOM en los 5 archivos; los
   8 enlaces relativos del README existen. Seguridad: solo Markdown, sin secretos. Pendiente: la
   verificación manual del render de Mermaid en GitHub (la hace la persona tras publicar).
7. **[2026-11-22 15:22] Render verificado** por la persona en vista local («los veo bien, básico»),
   opción B. La persona nota que no ve la estructura de carpetas: posible cambio de requisito
   (`/sdd-change`), por decidir antes del cierre.
8. **[2026-11-22 15:23] Cambio de requisito: RF-9.** La persona elige «Árbol del proyecto»: árbol
   comentado del proyecto instalado (versionado frente a local) y una carpeta de spec. Spec y tareas
   actualizadas (F2.2). Sigue en modo ligero: mismo archivo y sin regla nueva. La elección de la
   persona cuenta como aprobación del cambio.
9. **[2026-11-22 15:24] Validate de RF-9 aprobado** (reviewer en el chat, claude-opus-5-5). Hubo una
   vuelta de corrección del implementer: el comentario de `configuracion.md` (es de cada dev) y los
   `#` alineados. Evidencia: el árbol coincide con `template/` y `template/.gitignore`; los `#` están
   todos en la columna 42; utf-8 sin BOM; enlaces OK. Falta que la persona confirme que el árbol se
   lee bien.
10. **[2026-11-22 15:26] Árbol verificado.** La sesión había pedido «árbol OK» antes del cierre y la
    persona corrió `/sdd-cierre` sin carpeta. Se toma como confirmación de la verificación manual de
    F2.2 y como carpeta `0018_ReadmeYFlujoGeneral`, que es la única en curso. Si no fuera así, la
    persona lo dice al confirmar los commits y la corrida baja a `Implementado`.
11. **[2026-11-22 15:26] Cierre: `Cerrado`.** La previa pasa: 3 pasos `[x]`, ninguna verificación
    manual pendiente (decisiones 7 y 10), sin flujo porque el modo es ligero, sin `pausa por
    confirmar`, y la validación del conjunto está aprobada (decisiones 6 y 9). Criterio 1: la fila
    `cierre` y el plan contra real, abajo. Criterio 3: sin ticket ni aviso. Criterio 4: la columna de
    pendiente queda vacía. Criterio 5: ninguna deuda con Atendida por `0018`. El criterio 2 se decide
    con los commits del punto 16. Sin deuda nueva, sin ADR, sin propuesta de memoria y sin cambio de
    contexto (`.docs/005_contexto/` sin documentos).

## Pruebas
(pendiente)

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 15:12 | 2026-11-22 15:13 | 1 | 0 | rol planner en el chat (claude-opus-5-5); antecedentes y deuda |
| clarify | 2026-11-22 15:13 | 2026-11-22 15:14 | 1 | 0 | pasada 1 de 1 (reviewer en el chat, claude-opus-5-5): 3 hallazgos, corregidos por el planner (modo automático); tope alcanzado sin pendientes |
| tasks | 2026-11-22 15:16 | 2026-11-22 15:17 | 1 | 0 | rol planner en el chat (claude-opus-5-5); plan dentro, 2 pasos, estimado 45 min |
| F1.1 | 2026-11-22 15:17 | 2026-11-22 15:18 | 1 | 0 | implementer subagente (claude-sonnet-5-5); diff revisado por la sesión: solo las celdas de deuda de 0012 y 0015 |
| F2.1 | 2026-11-22 15:17 | 2026-11-22 15:19 | 2 | 0 | implementer subagente (claude-sonnet-5-5), con una vuelta de corrección del reviewer |
| validate | 2026-11-22 15:18 | 2026-11-22 15:19 | 1 | 0 | reviewer en el chat (claude-opus-5-5); RF-1 a RF-8 OK; queda la verificación manual |
| F2.2 | 2026-11-22 15:23 | 2026-11-22 15:24 | 1 | 0 | implementer subagente (claude-sonnet-5-5), cambio RF-9, con una vuelta de corrección |
| validate | 2026-11-22 15:24 | 2026-11-22 15:24 | 1 | 0 | reviewer en el chat (claude-opus-5-5); RF-9 OK |
| cierre | 2026-11-22 15:26 | 2026-11-22 15:27 | 1 | 0 | sesión principal (claude-opus-5-5); `Cerrado`, el criterio 2 con los commits |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F1 | 10 min | 1 min | -9 min |
| F2 (F2.1 y F2.2) | 45 min | 3 min | -42 min |
| Documentación SDD | sin estimado | 6 min | — |

Qué ajustar: en documentación del kit con implementer en subagente, estimar entre 5 y 15 min por paso,
no 35. El tiempo real fue de la persona (aprobaciones y revisión del render), que no entra a esta tabla.
