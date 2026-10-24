---
name: migracion-bd
description: Reglas para escribir los scripts SQL de un feature en `.docs/002_specs/<NNN>_<Feature>/sql/` - nombre, orden, idempotencia, rollback, encabezado EJECUCIÓN MANUAL y verificación posterior. Usar al planear (`03_plan.md`), al implementar un paso que cambie datos o esquema (`/sdd-implement`) y al validar scripts (`/sdd-validate`). Los agentes generan el SQL, nunca lo ejecutan. Disparadores - "script SQL", "migración", "DDL", "seed", "rollback", "sql/", "cambio de esquema", "ALTER TABLE".
---

# Skill: Migración de base de datos (scripts `sql/`)

Complementa a `sdd` y al implementer: ellos fijan **que** el SQL se genera y no se ejecuta; esta
skill fija **cómo se escribe**. Es independiente del motor: el dialecto lo dicta `AGENTS.md`.

## Regla de oro

El agente **genera** DDL/DML en `sql/` y **nunca lo ejecuta**. Solo consultas de solo lectura.
Lo ejecuta el usuario, y `05_avance.md` › "Estado BD" registra qué script corrió y cuándo.

## Nombre y orden

- `NN_<tipo>_<desc>.sql`, con `NN` consecutivo de dos dígitos que **es** el orden de ejecución.
  `tipo`: `ddl`, `seed`, `data` (corrección de datos) o `idx`.
- Un script por cambio lógico; no mezclar esquema y datos en el mismo archivo.
- Rollback: `99_rollback_<desc>.sql`, que deshace en **orden inverso**. Si algo no se puede
  deshacer (borrado de datos, `DROP`), el encabezado lo dice y exige respaldo previo.

## Encabezado obligatorio

```sql
-- EJECUCIÓN MANUAL. Un agente no ejecuta este script.
-- Feature:   <NNN_Feature>      Paso: <Fn.m>
-- Propósito: <una línea>
-- Requiere:  <scripts previos NN, o "ninguno">
-- Rollback:  99_rollback_<desc>.sql   (o "irreversible: respaldo previo")
-- Verificar: <consulta de solo lectura que prueba que quedó bien>
```

## Idempotencia

- Todo script debe poder correrse dos veces sin error ni duplicados: `IF NOT EXISTS`/existencia
  previa en DDL, y `INSERT` condicionado o `MERGE`/`upsert` en seeds, según el motor.
- Todo `UPDATE`/`DELETE` lleva `WHERE` explícito y una consulta previa que cuenta las filas
  afectadas; si el número no es el esperado, se detiene.
- Sin `DROP` ni `TRUNCATE` sin respaldo y sin mención en la spec o el plan aprobados.

## Seguridad del cambio

- Cambios aditivos primero (columna nueva con valor por defecto o nula); lo destructivo, en un
  script aparte y posterior.
- Datos de prueba o de producción reales no se versionan. Sin credenciales ni datos personales.
- Transacción por script cuando el motor lo permita; si no, que el encabezado diga cuál es el
  estado intermedio seguro.
- Todo cambio de esquema que toque un módulo o tabla con deuda abierta (`.docs/003_deuda/`) se
  anota en `02_analisis_impacto.md`.

## Por rol

| Rol | Hace |
|---|---|
| planner | Lista en `03_plan.md` cada script con tipo y rollback; marca lo irreversible |
| implementer | Escribe los scripts con esta skill; no los ejecuta; los reporta como "pendiente de ejecutar" |
| reviewer | Revisa encabezado, orden, idempotencia, rollback y que la consulta "Verificar" sea de solo lectura |

## No hacer

- Ejecutar SQL de escritura desde un agente.
- SQL suelto fuera de `sql/`.
- Editar un script ya ejecutado por el usuario: se agrega uno nuevo con el siguiente `NN`.
