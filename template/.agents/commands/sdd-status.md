---
description: SDD · Dónde estamos - fase actual, avance, horas y siguiente paso de un feature
argument-hint: <prefijo_NombreFeature>
---

Informa el estado de `.docs/002_specs/$1/`. No modifica archivos.

Qué lee: la línea `Estado` de `01_spec.md`; la lista de documentos presentes en la carpeta; de `05_avance.md`, solo PUNTO DE RETOMO, Pasos, Pruebas y Tiempos; de `04_tasks.md`, solo el conteo de identificadores `Fn.m` por búsqueda, no el archivo entero.

Qué hace: con eso responde fase y `Estado`; punto de retomo; pasos hechos y pendientes (x de y); verificaciones manuales pendientes; horas acumuladas y filas con `pausa por confirmar`; el siguiente paso con su comando `/sdd-*`. Si no hay `05_avance.md`, deduce la fase por los documentos presentes y el `Estado` de la spec.

Qué no hace: no lee `01`–`06` completos ni `MEMORY.md`, salvo que el usuario pida el detalle de uno. No modifica archivos.
