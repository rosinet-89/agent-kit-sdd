# 04 — Tareas: Cierre en una corrida con sus commits
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## F1 — Leyenda y comando (estimado: 5 min)
- [x] **F1.1 Leyenda: criterio 2 y tres resultados.** RF-1, RF-7
  - Hecho cuando:
    - [x] El criterio 2 dice «no queda un cambio del requerimiento fuera de los commits confirmados en la corrida del cierre» y remite el orden a `sdd-cierre`
    - [x] La frase de los tres resultados dice que el criterio 2 se decide en la misma corrida con los commits
    - [x] `diff` de la leyenda entre `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md` vacío
    - [x] Lint de `AGENTS.md` › Comandos OK en los dos archivos
- [x] **F1.2 `sdd-cierre.md`: orden y puntos 7, 8 y 16.** RF-1 a RF-6
  - Hecho cuando:
    - [x] El orden del cierre hacia `Cerrado` termina en los commits del cierre
    - [x] El punto 16 cubre propuesta (RF-2), confirmación (RF-3), sin confirmación (RF-4), fallo (RF-5), sin cambios del requerimiento (RF-6) y la corrida cortada
    - [x] La réplica `.agents/commands/sdd-cierre.md` igual a `template/`
    - [x] Lint OK; rutas citadas existentes

## F2 — Publicación (estimado: 5 min)
- [x] **F2.1 Versión 0.11.0, changelog y manifiesto.** Criterio 3
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` = `0.11.0`; entrada en `ChangelogAgentKitSdd.md`
    - [x] `manifiestos/0.11.0.md` con la huella de `ACTUALIZAR.md`; cada fila coincide con `template/`
- [x] **F2.2 Ensayo de instalación y lint final.** Criterios de finalización; CP-01
  - Hecho cuando:
    - [x] `cp -R --update=none template/. "$tmp"/` sin error en `mktemp -d`; `diff -rq` vacío
    - [x] Lint de cada archivo tocado
