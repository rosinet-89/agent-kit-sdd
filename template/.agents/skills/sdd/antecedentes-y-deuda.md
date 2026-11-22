# Antecedentes y deuda técnica

Fuente única de los procedimientos «Antecedentes» y «Deuda técnica». `AGENTS.md` conserva el
encabezado de cada uno con su condición y remite aquí.

## Antecedentes (obligatorio en análisis y desarrollo nuevo)
Índice de planes: `.docs/002_specs/README.md`.
1. Ábrelo y busca el módulo, pantalla, tabla o palabra clave de la petición.
2. Si hay coincidencia: dile al usuario qué carpeta es y lee SOLO esa (primero su `PUNTO DE RETOMO`).
3. Si no hay: di "sin antecedentes en el índice" y confírmalo con un `grep` antes de afirmarlo.
4. No recorras las carpetas de plan ni asumas que un proceso es nuevo, o que ya está resuelto, sin este paso.
5. Si la celda de la fila del índice y la etapa de la carpeta discrepan, los conteos siguen la celda,
   se avisa y no se escribe. Reglas en `.docs/002_specs/README.md` › «Leyenda de estatus».
6. Carpeta nueva o cambio de estatus actualiza la fila del índice. Quién escribe cada transición
   y la forma de la celda están en `.docs/002_specs/README.md` › «Leyenda de estatus».

## Deuda técnica
Índice: `.docs/003_deuda/README.md` (un `<prefijo>_<Nombre>.md` por deuda, con estado, origen y qué la resolvió).
1. Al hacer "Antecedentes", busca también ahí deuda abierta del módulo, pantalla o tabla y dila al usuario.
2. Si durante un requerimiento algo se difiere (por decisión del usuario o por recorte de alcance), o aparece un hallazgo que no se atiende ahora, PROPÓN darlo de alta; no la crees sin el visto bueno del usuario.
3. Al cerrar un requerimiento, indica qué deudas resolvió (estado `Resuelta` + "Atendida por" + "Cómo se atendió") y cuáles dejó. Lo hace `/sdd-cierre`.
4. Una deuda nunca se borra: pasa a `Resuelta` o `Descartada` (con motivo).
