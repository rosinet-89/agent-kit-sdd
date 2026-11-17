# ADR 0026: Antecedentes remite a la leyenda

- **Estado:** Aceptado
- **Fecha:** 2026-11-16
- **Origen:** `0012_EstatusDeSpecYCierre`

## Contexto
Los puntos 5 y 6 de Antecedentes decían que manda la carpeta y que cualquier estatus lo hacen `/sdd-spec` y `/sdd-cierre`. Esas frases contradicen el desfase de `## Leyenda de estatus` y a la persona, que es quien escribe `Diferido`. `AGENTS.md` es grupo B: un proyecto ya instalado no lo recibe al actualizar.

## Decisión
Los puntos 5 y 6 de Antecedentes remiten a la leyenda, en `template/AGENTS.md` y en el `AGENTS.md` del clon. No copian la tabla.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Dejar «manda la carpeta» y «lo hacen `/sdd-spec` y `/sdd-cierre`» | Contradicen el desfase y a la persona que escribe `Diferido`. |

## Consecuencias
Antecedentes aplica `.docs/002_specs/README.md` › «Leyenda de estatus» (en el kit, la de `template/.docs/002_specs/README.md`) y no vuelve a redactar el desfase ni quién escribe cada transición. El changelog avisa que un proyecto ya instalado incorpora esos dos puntos a mano. No se crea un comando para `Diferido`.
