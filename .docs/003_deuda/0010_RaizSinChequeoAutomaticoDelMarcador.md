# 0010 — La raíz del clon no tiene chequeo automático del marcador

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-11-03 |
| Origen | Decisión de requerimiento. Diferido en `0010_FalsoPositivoInitPendiente` (excepción declarada de HU-3; alta pedida por el usuario en el cierre) |
| Módulo | `/sdd-estructura` (E21), raíz del clon |
| Prioridad | Baja |
| Atendida por | `0017_PausaOrigenYContexto` (cierre 2026-11-22) |

## Descripción
E21 solo corre en proyectos con `.agents/.kit-version`. La raíz del clon (entorno de mantenimiento) no tiene ese marcador, así que nada vigila automáticamente que su `AGENTS.md` conserve la «regla del cero» del marcador de instalación pendiente. Solo la cubre el `grep` manual de RF-1 de la spec 0010. El comando `diff` de F3.3, que contrasta la cadena del marcador, solo se probó en el clon como ensayo.

## Por qué quedó pendiente
Darle `.kit-version` a la raíz la convertiría en un proyecto instalado, lo que cambia su papel de entorno de mantenimiento. La spec lo declaró como excepción de HU-3.

## Impacto si no se atiende
Una regresión en el `AGENTS.md` de la raíz (volver a escribir el marcador fuera de su comentario) pasa desapercibida hasta que alguien repita el `grep` a mano.

## Alcance (archivos, tablas)
`template/.agents/validar-estructura.md` (E20, E21) y su réplica en `.agents/`; posiblemente `AGENTS.md` de la raíz.

## Cómo se atendería (propuesta)
1. Abrir una spec propia por el flujo `sdd`.
2. Añadir un chequeo del kit, solo para el repo del kit (el que tiene `template/`), que cuente el marcador en el `AGENTS.md` de la raíz y compare la cadena con la plantilla.
3. Alternativa descartable: dar `.kit-version` a la raíz.

## Cómo se atendió (al resolver: qué se hizo y dónde)
RF-30 de `0017_PausaOrigenYContexto`: E24 de `template/.agents/validar-estructura.md` cuenta, en el repo del kit (con `template/` y sin `.agents/.kit-version`), el marcador de instalación pendiente del `AGENTS.md` de la raíz y compara su cadena con `template/AGENTS.md`. Da `AVISO` y nunca `FALLA` (decisión 24 del avance). La raíz no gana `.kit-version`.

## Historial
- 2026-11-03 — Alta, en el cierre de `0010_FalsoPositivoInitPendiente`, con el visto bueno del usuario.
- 2026-11-22 — Pasa a En atención y a Resuelta en el cierre de `0017_PausaOrigenYContexto`, con el visto bueno de la persona.
