# 0011 — `/sdd-constitucion` no figura en la tabla de fases ni en el ruteo del coordinator

| Dato | Valor |
|---|---|
| Estado | Abierta |
| Alta | 2026-11-03 |
| Origen | Hallazgo al validar `0009_EnlacesTrasPull` (aviso de E10, anterior a esa spec; alta pedida por el usuario en el cierre) |
| Módulo | Skill `sdd`, agente `coordinator` |
| Prioridad | Baja |
| Atendida por | — |

## Descripción
`/sdd-constitucion` existe como comando, pero no figura en la tabla de fases de `template/.agents/skills/sdd/SKILL.md` ni en el ruteo del `coordinator`. E10 lo reporta como AVISO desde antes de la spec 0009.

## Por qué quedó pendiente
No lo introdujo 0009 y quedaba fuera de su alcance (solo E1 y E7 del delta).

## Impacto si no se atiende
`/sdd <petición libre>` puede no ofrecer ni enrutar la fase de constitución, y E10 sigue dando AVISO en cada corrida de `/sdd-estructura`.

## Alcance (archivos, tablas)
`template/.agents/skills/sdd/SKILL.md` (tabla de fases), `template/.agents/agents/coordinator.md` (ruteo), y sus réplicas en `.agents/`.

## Cómo se atendería (propuesta)
1. Decidir si `/sdd-constitucion` es una fase del flujo o un comando auxiliar.
2. Si es fase, añadirla a la tabla y al ruteo; si es auxiliar, ajustar E10 para exentarla.

## Cómo se atendió (al resolver: qué se hizo y dónde)
—

## Historial
- 2026-11-03 — Alta, en el cierre de `0009_EnlacesTrasPull`, con el visto bueno del usuario.
