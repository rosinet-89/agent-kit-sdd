# ADR 0028: Modelo por rol, rol en el chat salvo cambio de modelo y excepción de QA

- **Estado:** Aceptado
- **Fecha:** 2026-11-21
- **Origen:** `0013_ModelosPorAgente`

## Contexto
El ADR 013 dejó los cuatro roles en `model: inherit` porque no hubo comparación de modelos
previa. Con `tester` y `explore` como roles nuevos, el kit necesita un modelo por rol y una regla
de dónde corre cada uno. Además, `tester` ejecuta pantallas de QA, lo que choca con la regla 1 de
Guardrails (BD solo lectura, sin red). La persona decidió las tres cosas sin esperar a una
comparación previa.

## Decisión
1. **Modelo por rol.** Cada agente declara el modelo de la tabla del kit (`planner`, `coordinator`
   y `reviewer` en `opus`; `implementer` en `sonnet`; `tester` y `explore` en `haiku`). La tabla
   vive en `.agents/skills/sdd/restricciones-de-rol.md` › «Modelo por rol» y no se copia.
2. **Rol en el chat salvo cambio de modelo.** Un rol con el mismo modelo de la sesión corre en el
   chat, también el reviewer. Solo levanta subagente el rol que cambia de modelo o cuando la
   persona lo pide. La separación entre producir y validar es por rol: el chat cambia de rol, lo
   muestra y nunca valida con el rol con que produjo. Detalle en `.agents/skills/sdd/SKILL.md` ›
   «Dónde corre cada rol».
3. **Sonnet para el implementer sin comparación previa.** Se elige por decisión de la persona:
   implementa un diseño cerrado y lo revisa Opus. Se mide en uso real el tiempo por paso y las
   vueltas de validate, y se revisa tras 3 features.
4. **Excepción de QA a la regla 1.** `tester` puede ejecutar pantallas contra un entorno de QA que
   cada persona declare (no el proyecto), que se distinga de forma segura de producción. La
   excepción está escrita en `AGENTS.md` › Guardrails, regla 1, y solo vale para ese entorno.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| `inherit` en todos los roles (ADR 013) | Deja el costo atado a la sesión y no distingue roles que no necesitan el modelo mayor |
| Comparación previa del implementer en la spec | La persona decidió medir en uso real y revisar tras 3 features |
| `tester` y `explore` siempre como subagente | Si la sesión ya usa su modelo, el subagente solo agrega costo. Sus restricciones pasan a ser instrucción en el chat |
| Reviewer siempre como subagente | La separación es por rol, no por proceso: el chat cambia de rol y lo muestra |
| Sin excepción de QA (solo modo seguro o sacar `tester`) | `tester` no podría ejecutar pantallas de QA |
| QA sin red ni credenciales | Dejaría QA solo local y sin inicio de sesión |
| Aceptar RF-12 (encargo completo para un rol Haiku) como instrucción con deuda | Una prueba real mostró que Haiku trabaja igual con un encargo incompleto: la instrucción sola no basta, la comprobación debe hacerla quien lanza |

## Consecuencias
Baja el costo por feature en los roles de ejecución. El `tester` y el `explore` en el chat se
rigen por instrucción, no por un modelo distinto que los acote. La excepción de QA es la única que
admite el preámbulo de Guardrails y depende de que la persona declare bien su entorno. Queda
condicionado: la elección de Sonnet se revisa tras 3 features con el tiempo por paso y las vueltas
de validate. El encargo de un rol Haiku lo comprueba quien lo
lanza; el rol lo repite como segunda barrera, que es solo instrucción. Si no entrega, hay un solo
relanzamiento con el modelo del implementer y después la tarea pasa a la sesión principal. Detalle
en `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol». Reemplaza al ADR 013.
