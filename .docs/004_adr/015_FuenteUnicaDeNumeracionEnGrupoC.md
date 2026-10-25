# ADR 015: La fuente única de la numeración vive en el grupo C

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `005_ConvivenciaNumeracion`

## Contexto
La regla de anchos (ADR 014) necesita un solo lugar, por el principio 3 de la constitución. El lugar natural es la sección « Numeración única » de `.docs/002_specs/README.md`. Ese README, como todo `.docs/`, es grupo C de `ACTUALIZAR.md` (ADR 007): si ya existe, la actualización no lo lee, no lo compara y no lo reemplaza. Un proyecto instalado con 0.4.0 conserva ahí la regla vieja mientras los comandos y las skills, que sí se actualizan, describen la nueva.

## Decisión
La fuente única sigue en « Numeración única » del índice de specs, dentro del grupo C. La actualización detecta ítems de 3 dígitos por el nombre de la carpeta, sin leer ningún README, y ofrece el texto de la regla; solo escribe si el usuario lo confirma. Cada pieza lleva una sola frase mínima — « Un ítem nuevo conserva el ancho de su carpeta » — y remite a esa sección. Esa frase es la única repetición permitida.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Mover la fuente a un archivo del grupo A | El usuario lo descartó: partiría la regla del índice que ya consulta quien numera |
| Leer o comparar el README durante la actualización | Enmienda el grupo C y el ADR 007, y obliga a definir qué texto cuenta como vigente |
| Copiar la regla completa en cada pieza | Es el desfase que el ADR 009 ya descartó |
| Remitir y no llevar la frase mínima | Si el README del proyecto sigue en 0.4.0, la pieza no sabe qué ancho usar |

## Consecuencias
No reemplaza al ADR 007, al ADR 009 ni al ADR 014: el primero fija que el grupo C no se reemplaza, el segundo fija una fuente por fase del flujo y el tercero fija la regla de anchos. La fuente puede quedar vieja en un proyecto ya instalado; la mitigación es el aviso y la frase mínima (RNF-2 de la `005`). Los bordes que esa mitigación no cubre quedan en la deuda `008_LimitacionesDeLaNumeracion`.
