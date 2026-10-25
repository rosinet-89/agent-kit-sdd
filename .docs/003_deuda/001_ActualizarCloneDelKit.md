# 001 — Actualizar el clon del kit (modo mantenimiento)

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-10-24 |
| Origen | Recorte de alcance en `001_ActualizarKit` (duda 5, decisión del usuario, 2026-10-24) |
| Módulo | Instalación / actualización del kit |
| Prioridad | Media |
| Atendida por | Documentación en `INSTALAR.md` (sesión paralela a `001_ActualizarKit`; no entra en esa spec) |

## Descripción
La copia local de `.agents/` en el clon del kit (modo mantenimiento) se desactualiza respecto de `template/` cuando se edita el producto. Hoy se refresca a mano con `cp -R template/.agents/. .agents/` conservando `napkin.md`.

## Por qué quedó pendiente
`001_ActualizarKit` resuelve proyectos externos. El clon del kit es un caso aparte: sus exclusiones son locales y parte de su contenido es conocimiento propio versionado (ADR 001).

## Impacto si no se atiende
Quien mantiene el kit trabaja con skills y comandos viejos sin darse cuenta; `/sdd-estructura` (chequeo E19) ya lo detecta, pero no lo corrige.

## Alcance (archivos, tablas)
`INSTALAR.md` (modo mantenimiento), `.agents/validar-estructura.md` (E19), `.git/info/exclude` local.

## Cómo se atendería (propuesta)
Reutilizar la mecánica de `001_ActualizarKit` para el clon, o documentar el refresco como paso del modo mantenimiento.

## Cómo se atendió (al resolver: qué se hizo y dónde)
No se reutilizó la mecánica de `001_ActualizarKit` (sigue en fase `plan` y el clon está fuera de
esa spec). Se documentó el refresco como paso del modo mantenimiento:

- `INSTALAR.md` › Modo mantenimiento › "Refrescar la copia local": sobrescribe `.agents/` desde
  `template/.agents/`, restaura `napkin.md` y no borra lo que solo está en la copia local. Se
  ejecuta solo con confirmación.
- `template/.agents/validar-estructura.md` y `.agents/validar-estructura.md`, fila E19: si el
  `diff` muestra algo más que `napkin.md`, la corrección propuesta es ese refresco, sin ejecutarlo
  solo.
- `.git/info/exclude` ya tenía las exclusiones ancladas con `/` y la negación de `napkin.md`. No
  se modificó.

La constitución y el ADR 007 no se tocaron: los lleva `001_ActualizarKit`.

## Historial
- 2026-10-24 — Alta.
- 2026-10-25 — Resuelta. Refresco documentado en el modo mantenimiento; el clon no entra en `001_ActualizarKit`.
