# ADR 008: Formato de manifiestos, huella y marcador

- **Estado:** Aceptado
- **Fecha:** 2026-10-25 (registrada; decidida en `001_ActualizarKit`)
- **Origen:** `.docs/002_specs/001_ActualizarKit/` (decisiones 2, 4 y 13)

## Contexto
La actualización de un proyecto ya instalado tiene que saber qué versión tiene y qué archivos del kit cambiaron, sin depender de Git ni de tags. El ADR 007 exige reconocer un archivo KIT-VIEJO antes de reemplazarlo. El contenido de `template/` no debe llevar esos datos a los proyectos.

## Decisión
La identidad de un archivo es una huella SHA-256 en hexadecimal minúsculo, calculada sobre los bytes ya normalizados. El algoritmo vive en `ACTUALIZAR.md`. Cada versión del kit tiene un manifiesto en `manifiestos/<versión>.md`, en la raíz del kit y fuera de `template/`: no se distribuye. La tabla es `Ruta | Huella`. La versión instalada en un proyecto es una sola línea numérica en `.agents/.kit-version`. Lo que el usuario pide mantener queda en `.agents/.kit-mantener`, una línea por decisión, y se vuelve a proponer solo si cambia la huella del destino guardada.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Comparar solo con `git show` o con el kit nuevo | Sin historial no hay base, y contra el kit nuevo nunca aparece un reemplazo |
| Poner el marcador dentro de `AGENTS.md` o dejarlo solo en la máquina | El primero se fusiona a mano; el segundo diverge entre quienes trabajan el mismo proyecto |
| Guardar «mantener» dentro del marcador, en `napkin.md` o en un `.md` | Rompe la línea única, lo cura el agente o lo revisan chequeos que no deben fallar por una ruta retirada a propósito |
| Distribuir los manifiestos dentro de `template/` | Llevarían al proyecto rutas retiradas y el registro de versiones del kit |

## Consecuencias
El ADR 007 puede clasificar KIT-VIEJO comparando la huella del proyecto con la de la base y la del destino. E20, solo en el repositorio del kit, comprueba que `template/` coincide con `manifiestos/<VERSION>.md`. Un cambio de `template/` después de congelar la versión obliga a regenerar ese manifiesto. El algoritmo de la huella queda fijo: cambiarlo invalidaría los manifiestos ya publicados.
