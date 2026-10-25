# 02 — Análisis de impacto: Ruta si no delega

## 1. Resumen del requerimiento

Si quien atiende una fase puede lanzar en un solo paso al rol que esa fase nombra, se usa la ruta diseñada. Si no puede, o si el camino insertaría un agente que a su vez tendría que delegar, la misma fase sigue en esa entrada por la otra ruta, con el mismo trabajo y las mismas puertas, y deja constancia en el avance. Si el usuario pide que la fase se haga en el chat, esa entrada se hace en la sesión. La regla es del kit, para cualquier herramienta de agentes.

### 1.1 Antecedentes (índice de planes: carpetas que coinciden, qué se reutiliza y qué contradice; o "sin antecedentes en el índice" + grep que lo confirma)

- `003_OptimizarArnes` (Implementado, cierre 2026-10-25). Ahí se fijó que el chat lanza al coordinator y no ejecuta las fases, y que cada comando es la fuente de su fase. Esta spec completa esa regla cuando el lanzamiento no se puede hacer o insertaría un segundo paso. No se reabre la `003`.
- `004_ModeloYPermisosAgentes` (spec aprobada, plan en curso en paralelo). Cambia el frontmatter de los cuatro agentes, entre ellos `coordinator.md`, y una pieza de restricciones en la skill `sdd`. No cambia esta regla de ruta. La implementación de la `006` no edita esos archivos hasta partir del texto que deje la `004`.
- `005_ConvivenciaNumeracion` (spec aprobada). Toca numeración, `template/AGENTS.md` en otra sección, el changelog y el manifiesto. No define la delegación.
- No había carpeta previa de ruta alterna. El índice, leído el 2026-10-25 al abrir la `006`, no tenía otra fila con esas palabras. La `006` es la carpeta de este caso.

### 1.2 Deuda técnica relacionada (`.docs/003_deuda/README.md`: deudas abiertas del módulo y si este feature las resuelve o las deja; o "ninguna")

Ninguna deuda abierta describe este atasco. La deuda 003 la atiende la spec 004. La deuda 004 la atiende la spec 005. Esta spec no las resuelve ni las reabre.

## 2. Módulos, menú, tablas y archivos involucrados

### 2.1 Menú / opciones / rutas de acceso

No aplica. El acceso es el flujo `sdd` (`/sdd` y los comandos de fase).

### 2.2 Tablas (existentes y nuevas)

No aplica.

### 2.3 Archivos de código (ruta, capa)

Medido el 2026-10-25 15:37 sobre `template/` (el producto) y la copia local `.agents/` (la herramienta de este clon). `.cursor/agents`, `.cursor/commands` y `.cursor/skills` son enlaces a `.agents/`. `.agents/` y `template/.agents/` son dos copias, no el mismo archivo.

| Ruta | Capa | Qué tiene hoy |
|---|---|---|
| `template/.agents/skills/sdd/SKILL.md` | Producto | Líneas 75-79: el chat lanza al coordinator y no ejecuta las fases. No hay salida si ese lanzamiento no puede delegar. |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Producto | Quién escribe el avance y el formato de la fila. No dice que el intento fallido entre en la misma fila ni el texto de la constancia. |
| `template/.agents/agents/coordinator.md` | Producto | Delega y, si lo invocan como subagente, devuelve la fase. No dice que suelte la fase en una línea cuando no puede abrir al rol. |
| `template/.agents/commands/sdd.md` | Producto | Elige un comando y lo sigue. No tiene la salida. |
| `template/.agents/commands/sdd-spec.md`, `sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-change.md` | Producto | Cada uno delega en planner, reviewer o implementer. `sdd-flujo.md` delega en dos roles, en ese orden. Ninguno dice qué hacer si el lanzamiento no se puede. |
| `CHANGELOG.md` | Manual del kit | `0.5.0` ya dice, en la última viñeta, que la skill manda lanzar al coordinator y no ejecutar las fases. `VERSION` es `0.5.0`. |
| `.agents/` (mismos relativos que arriba) | Copia local de este clon | Es la que leen las sesiones de aquí. Hay que refrescarla desde `template/` al implementar, archivo por archivo. No se toca `.agents/napkin.md`. |

No entran `sdd-cierre.md`, `sdd-status.md`, `sdd-estructura.md`, `sdd-init.md` ni `sdd-constitucion.md`: el cierre, el estado y la estructura ya se hacen en la sesión principal, y los otros dos no son fases de un feature.

### 2.4 Código legado o sin uso (no tocar, sí documentar)

La frase de las líneas 75-79 de la skill queda sustituida por la regla de un solo paso. No se borra el diseño de agentes y subagentes.

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)

No aplica base de datos. Lectura de archivos el 2026-10-25 15:37:

- Nueve comandos delegan: los ocho de fase listados arriba más `sdd.md`, que lanza el enrutado.
- `sdd-flujo.md` es el único con dos roles (planner y luego reviewer).
- `VERSION` = `0.5.0`. El changelog de esa versión ya está escrito y la versión no está publicada como paquete aparte en este clon: el trabajo de la `004` y de la `005` también entra en ese bloque.

## 4. Flujo actual

1. El chat lee la skill y lanza al coordinator.
2. El coordinator elige el comando y debe lanzar al rol de la fase.
3. Si el coordinator va como subagente, en Cursor no puede abrir a otro subagente. Se detiene. La fase no se hace. El avance, si se escribe, no cierra la fase.
4. Solo sigue si alguien, fuera de la regla, lanza el rol desde el chat.

## 5. Reglas de negocio: conocidas vs código

| Regla de la spec | Hoy |
|---|---|
| Un solo paso, o la otra ruta en la misma entrada (RF-1, RF-2) | La skill obliga al segundo paso y no tiene salida. |
| Constancia en una línea y tiempos en la misma fila (RF-5, RF-9, RF-10) | El contrato no lo pide. |
| Varios roles en el orden de la fase; si uno posterior no arranca, se conserva lo ya devuelto (RF-11) | `sdd-flujo.md` tiene el orden y no dice qué hacer si el segundo no arranca. |
| Pedido del usuario de hacer la fase en el chat (RF-12) | No está escrito. |
| Cierre, estado y estructura siguen en la sesión principal (RF-7) | Ya está así. No se mueven. |

## 6. Riesgos y casos borde

- La `004` y esta spec escriben `template/.agents/agents/coordinator.md` y `template/.agents/skills/sdd/SKILL.md`. La `005` escribe el changelog y el manifiesto. Quien edite parte del texto que haya en ese momento y no revierte líneas ajenas. El manifiesto de `0.5.0` y E20 los regenera quien cierre último.
- Copiar la regla en cada comando rompería la fuente única que dejó la `003`. La regla vive en la skill. El contrato de tiempos solo dice cómo se anota. Cada comando lleva una frase que enlaza, no el procedimiento.
- Refrescar `.agents/` con una copia amplia pisaría `napkin.md` y trabajo local. El refresco es la lista de archivos de esta spec.
- Un subagente que ya arrancó no se relanza, aunque el resultado venga incompleto. Eso ya es un caso límite de la spec.

## 7. Preguntas abiertas

Ninguna.
