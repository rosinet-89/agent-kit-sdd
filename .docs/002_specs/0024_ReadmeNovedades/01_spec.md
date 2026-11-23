# 01 — Spec: README con la versión actual y sus novedades
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
El `README.md` de la raíz se actualizó por última vez en 0.16.0 (commit `9117418`). Las versiones
0.17.0 (spec `0022_PlanContraRealEnVivo`) y 0.18.0 (spec `0023_BitacoraPorSubagente`) cambiaron
`template/` y el changelog, pero no el README. Su texto sigue siendo correcto en general, pero no
dice qué versión es la vigente ni qué cambió en esas dos versiones. La persona pide actualizarlo con
la versión y con lo que cambió. Objetivo: quien abre el README sabe qué versión está viendo, qué trae
de nuevo y dónde está el detalle, y las filas que describen `05_avance.md` y la bitácora reflejan
0.17.0 y 0.18.0.

## Requisitos funcionales (EARS)
- RF-1: EL README muestra, debajo de la descripción inicial, la versión vigente del kit (el valor de
  `VersionAgentKitSdd`, hoy `0.18.0`) y enlaza a `ChangelogAgentKitSdd.md` para el historial.
- RF-2: EL README tiene la sección «Novedades» con las versiones 0.18.0 y 0.17.0, la más reciente
  primero, en no más de 3 líneas de fuente de hasta 100 caracteres cada una. Cada versión dice qué cambia para quien usa el kit, nombra
  su spec y su ADR (`.docs/004_adr/`) y no copia las reglas: remite a la skill de `template/` que las
  define. Termina con
  un enlace al changelog para las versiones anteriores.
- RF-3: La fila de `05_avance.md` en la tabla de «Ciclo de un requerimiento» dice que el avance lleva
  la tabla Pasos con estimado, real y delta en vivo (0.17.0), además del estado y el punto de
  retomo. La línea de atajo de «Ejemplo de uso» dice que `/sdd-status` da también las horas.
- RF-4: La fila «Bitácora de uso (beta)» de la tabla de «Configurar» dice que los tokens se cuentan
  por subagente y por sesión, con la coordinación de la sesión principal aparte (0.18.0).

## Casos límite
- El README se lee en GitHub o en un visor local: la versión y las novedades son texto plano, sin
  insignias ni imágenes externas.
- Sale una versión nueva y nadie actualiza el README: la versión del README queda atrasada, pero el
  enlace al changelog sigue llevando al historial completo (supuesto 2).
- Una regla de 0.17.0 o 0.18.0 cambia después: como el README solo resume y remite, el desfase queda
  en la redacción, no en reglas copiadas.

## Fuera de alcance
- Cambiar `template/`, subir la versión del kit o tocar el manifiesto.
- Volver obligatorio actualizar el README en cada versión (sería una regla nueva en
  `AGENTS.md` › Reglas de negocio).
- Reescribir otras secciones del README.

## Criterios de finalización
- Lint de `AGENTS.md` › Comandos OK en los archivos tocados (UTF-8 sin BOM, rutas citadas existentes).
- Ninguna regla del kit se redefine en el README: solo resume y remite.
- La versión del README coincide con `VersionAgentKitSdd` y las novedades coinciden con las entradas
  0.17.0 y 0.18.0 de `ChangelogAgentKitSdd.md`.
- RF-1 a RF-4 verificados en validate.

## Modo
`ligero`. Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md):
1. Sí: toca `README.md` y el índice de specs (2 archivos).
2. Sí: no hay otra spec en curso (0001 a 0023 `Cerrado`).
3. Sí: no cambia Guardrails, constitución, numeración ni contratos; el README resume y remite.
4. Sí: sin ADR (supuestos 1 y 2: dónde va y sin regla nueva).
5. Sí: sin `sql/`.
6. Sí: solo documentación; ningún comando, agente ni chequeo cambia lo que hace.
7. Sí: estimado 15 min.
8. Sí, con lectura: el único archivo del producto es `README.md`; el índice se toca solo como
   registro del flujo (filas), como en toda spec (igual que `0018_ReadmeYFlujoGeneral`).

## Dudas abiertas
Ninguna. Supuestos explícitos (la persona los puede revertir al aprobar):
1. «Novedades» va en el propio `README.md`, justo después de la versión y antes de «Flujo general»,
   y cubre solo 0.17.0 y 0.18.0 (las que el README no reflejaba); lo anterior queda en el changelog.
2. Sin regla nueva: actualizar la versión y las novedades del README en cada versión queda como
   práctica, no como obligación de `AGENTS.md`. Si la persona la quiere obligatoria, es otra spec.
3. Sin subir versión: `template/` no cambia (como en la 0018).
