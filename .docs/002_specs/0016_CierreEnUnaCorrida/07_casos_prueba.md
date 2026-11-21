# 07 — Casos de prueba: Cierre en una corrida con sus commits

Todos los casos corren en un temporal (`tmp=$(mktemp -d)`) o sobre el propio clon en solo lectura.
Herramienta de apoyo: «No aplica» (`AGENTS.md` › Comandos).

## Casos
### CP-01 Instalación copia sin pisar
- RF: criterios de finalización
- Datos de partida: temporal vacío.
- Destino: `local`
- Pasos:
  1. `cp -R --update=none template/. "$tmp"/`; `diff -rq template "$tmp"`
- Resultado esperado: `cp` sin error; `diff` vacío.
- Herramienta: a mano
- Escribe: no

### CP-02 Las dos leyendas son iguales
- RF: RF-7
- Datos de partida: clon tras F1.1.
- Destino: `local`
- Pasos:
  1. `diff <(sed -n '/^## Leyenda de estatus/,/^## Índice/p' .docs/002_specs/README.md) <(sed -n '/^## Leyenda de estatus/,/^## Índice/p' template/.docs/002_specs/README.md)`
- Resultado esperado: salida vacía.
- Herramienta: a mano
- Escribe: no

### CP-03 El cierre de esta spec llega a `Cerrado` en una corrida
- RF: RF-1 a RF-3; criterio 1
- Datos de partida: 0016 implementada y validada, sin commits.
- Destino: `local`
- Pasos:
  1. Correr `/sdd-cierre 0016_CierreEnUnaCorrida` y confirmar los commits propuestos.
- Resultado esperado: una sola corrida y una sola confirmación; celda `Cerrado`, línea `cerrada`, una fila `cierre`; `git status` sin cambios de la 0016.
- Herramienta: a mano
- Escribe: no

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-21 | ok | `cp` sin error; `diff -rq` vacío (F2.2) |
| CP-02 | 2026-11-21 | ok | `diff` de las dos leyendas vacío (F1.1) |
| CP-03 | 2026-11-21 | ok | Cierre en una corrida: `Cerrado`, `cerrada`, una fila `cierre` y 3 commits con una sola confirmación (decisión 7 del avance) |

## Evidencia formal (opcional)
No aplica.
