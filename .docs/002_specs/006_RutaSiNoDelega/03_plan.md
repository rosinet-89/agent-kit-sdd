# 03 — Plan: Ruta si no delega
Spec: 01_spec.md (aprobada 2026-10-25 15:37)

## 1. Ubicación y capas

El cambio es del kit. La regla de ruta vive en un solo sitio: la skill `sdd`, en `template/`. El contrato `avance-y-tiempos.md` solo dice cómo se anota la constancia y la fila de tiempos. Los comandos de fase siguen siendo la fuente de su propio procedimiento y enlazan la regla; no la copian. La constitución (capa única y kit genérico) se cumple: no hay nombres de herramienta ni de proyecto dentro de la regla.

La copia local `.agents/` de este clon se refresca, archivo por archivo, después de editar `template/`. `.cursor/` ya apunta a `.agents/`.

## 2. Archivos que se crean o modifican

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/sdd/SKILL.md` | Producto | Sustituye el párrafo de las líneas 75-79 por la regla de un solo paso, la otra ruta y el pedido del usuario. Enlaza el contrato para la constancia. No copia el formato de la fila. | RF-1, RF-2, RF-3, RF-4, RF-6, RF-7, RF-8, RF-11, RF-12 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Producto | La constancia es una decisión del avance, con hora: rol o pedido, porqué en una línea, ruta y quién hizo el trabajo. Una sola fila de la fase. Los minutos del intento que no delegó entran en esa fila y la nota lo dice. Si el agente no devolvió texto, la línea la escribe la sesión, con la causa que vio. | RF-5, RF-9, RF-10 |
| `template/.agents/agents/coordinator.md` | Producto | Si no puede abrir al rol, devuelve una línea con el porqué y suelta la fase. No redacta `01`–`04` ni `06`. No reintenta. | RF-2, RF-10 |
| `template/.agents/commands/sdd.md`, `sdd-spec.md`, `sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-change.md` | Producto | Una frase que enlaza la sección de la skill. No copia la regla. `sdd-flujo.md` conserva el orden planner y luego reviewer. | RF-2, RF-11 |
| `CHANGELOG.md` | Manual | Una viñeta en el bloque `0.5.0`: la fase sigue cuando no hay un solo paso, con constancia. | RF-1, RF-2 |
| `.agents/` (los mismos relativos de las filas de arriba) | Copia local | Refresco explícito desde `template/`, sin tocar `napkin.md` ni otros archivos. | RF-1 |

No se crean archivos nuevos. No hay `sql/`.

## 3. Base de datos

No aplica.

## 4. Seguridad

No aplica autenticación ni datos de usuario. La regla no pide secretos ni amplía herramientas. El coordinator sigue sin redactar los documentos de diseño para destrabar.

## 5. Liberación / piloto

No aplica un piloto. Entra en `0.5.0`, que sigue sin publicarse. No abre `0.5.1`. El manifiesto de `0.5.0` se regenera en el cierre, al final, después de la `004` y de la `005` si ellas también lo tocan. E20 se repite entonces. `/sdd-cierre` no es una fase de este plan.

## 6. Decisiones

| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| La regla de ruta vive en la skill `sdd`. El contrato solo describe la constancia y la fila. Los comandos enlazan. | Copiar la regla en cada comando. Dejarla solo en el coordinator. | Una sola fuente (constitución y la `003`). El chat tiene que verla aunque el coordinator no llegue a delegar. |
| Se implementa dentro de `0.5.0`. | Abrir `0.5.1`. | La versión no está publicada y la `004` ya dejó el mismo criterio. |
| El refresco local es la lista de archivos de esta spec. | Copiar todo `.agents/` desde `template/`. | `napkin.md` y el trabajo local no se pisan. |
| Marcar ADR: fuente única de la ruta alterna en la skill. | Crearlo ahora. | Tiene alternativas reales y efecto duradero. No se crea sin visto bueno. Se propone al cerrar. |

Registrado el 2026-10-25, con visto bueno del usuario: [ADR 010](../../004_adr/010_RutaSiNoHayUnSoloPaso.md). No reemplaza al 009.

## 7. Fases y estimado

| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | Releer los archivos compartidos con la `004` y la `005` y anotar que se parte de ese texto | — | 0,25 |
| F1 | Regla de un solo paso y otra ruta en la skill | RF-1, RF-2, RF-3, RF-4, RF-6, RF-7, RF-8, RF-11, RF-12 | 0,5 |
| F2 | Constancia y fila única en el contrato de tiempos | RF-5, RF-9, RF-10 | 0,25 |
| F3 | El coordinator suelta la fase en una línea | RF-2, RF-10 | 0,25 |
| F4 | Frase de enlace en los nueve comandos | RF-2, RF-11 | 0,5 |
| F5 | Viñeta en `CHANGELOG.md` de `0.5.0` | RF-1, RF-2 | 0,25 |
| F6 | Refresco de la copia local `.agents/` de esos archivos | RF-1 | 0,25 |

Total: 2,25 h.

## 8. Criterios de aceptación

- Con un solo paso posible, la fase usa la ruta diseñada.
- Si no hay un solo paso, la misma entrada sigue por la otra ruta, o se detiene en una línea cuando esa ruta tampoco puede lanzar y el usuario no pidió hacerla en el chat.
- La constancia y los minutos del intento van en el avance, en una fila.
- Cierre, estado y estructura siguen en la sesión principal.
- Ningún comando repite el procedimiento de la regla.
- `file -i` da utf-8 y no hay BOM en cada archivo tocado. En `template/` no aparecen marcadores `<<…>>` nuevos.
