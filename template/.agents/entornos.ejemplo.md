# Entornos de QA (ejemplo)

Este archivo es un ejemplo. Cópialo a `.agents/local/entornos.md` y edítalo ahí. Esa copia es de
cada persona y no se versiona (`.agents/local/` está fuera de Git). Este ejemplo sí viaja con el kit.

Lo lee el rol `tester` antes de cada caso. La excepción de QA a la regla 1 de Guardrails está en
`AGENTS.md` › Guardrails, regla 1 (sub-viñeta «Excepción de QA»); allí está la fuente, aquí no se
repite.

## Formato

- Una sección `## QA`.
- Dentro, una línea por destino con la forma `- esquema://host[:puerto]`.
- Un destino coincide solo si esquema, host y puerto son iguales. Sin puerto cuenta el del esquema.
  La ruta no cuenta.
- Una línea distinta de ese formato, o un destino repetido, se toma como formato roto. Entonces rige
  el modo seguro: la regla 1 sin excepción, y `tester` deja los casos como `manual`.
- Sin este archivo en `.agents/local/entornos.md` rige también el modo seguro.

## Advertencia (S-8)

Declarar un destino afirma que su base de datos, y todo lo que su página llama en segundo plano
(APIs de terceros, un API productivo u otros hosts), es de QA. `tester` solo comprueba sus propias
navegaciones y llamadas, no lo que la página hace por dentro. Si el destino usa una BD productiva o
compartida, no lo declares: el riesgo lo asumes tú al declararlo.

## Credenciales

Este archivo no lleva credenciales, ni valores ni nombres: solo destinos. Una línea de credencial
dentro de `## QA` rompe el formato. Si un caso las necesita, el caso nombra la variable de entorno en
«Datos de partida» (`.agents/skills/sdd/plantillas.md` › `## 07_casos_prueba.md`), por ejemplo
`QA_USUARIO` y `QA_CLAVE`. La herramienta de apoyo del entorno lee el valor; el agente nunca lo lee,
imprime ni copia.

## Ejemplo

```
## QA
- https://qa.ejemplo.test
- https://qa.ejemplo.test:8443
- http://localhost:3000
```
