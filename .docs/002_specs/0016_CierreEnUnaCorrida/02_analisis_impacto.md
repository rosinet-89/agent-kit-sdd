# 02 — Análisis de impacto: Cierre en una corrida con sus commits

## 1. Resumen del requerimiento
El cierre hacia `Cerrado` decide el criterio 2 (commits) en la misma corrida: escribe el estatus,
propone los commits del requerimiento y del cierre, y con la confirmación queda `Cerrado`; sin ella,
baja a `Implementado`.

### 1.1 Antecedentes
- `0012_EstatusDeSpecYCierre`: define la leyenda, los cinco criterios y `/sdd-cierre`. Se reutiliza
  todo; solo cambia el criterio 2 y la frase de los tres resultados.
- `0013_ModelosPorAgente` (decisión 36) y `0014_ConfiguracionGuiada` (decisiones 26 y 27): los dos
  casos reales del doble ciclo. La 0014 ya cerró con la regla nueva por decisión de la persona.
- ADR 0024 (la leyenda vive en el índice) y ADR 0027 (copia en el índice del clon): siguen vigentes.

### 1.2 Deuda técnica relacionada
- 0012 (celdas de fases intermedias): no la toca.
- 0013 (pulido de `sdd-cierre.md`): toca el mismo archivo, pero no sus cuatro puntos; queda abierta.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
`/sdd-cierre`.
### 2.2 Tablas (existentes y nuevas)
No aplica.
### 2.3 Archivos de código (ruta, capa)
- `template/.docs/002_specs/README.md` › «Leyenda de estatus»: criterio 2 y frase de los tres resultados.
- `.docs/002_specs/README.md` › «Leyenda de estatus»: la misma copia (ADR 0027).
- `template/.agents/commands/sdd-cierre.md`: orden del cierre hacia `Cerrado`, puntos 7, 8 y 16;
  réplica en `.agents/commands/sdd-cierre.md`.
- Publicación: `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.11.0.md`.
### 2.4 Código legado o sin uso
Ninguno.

## 3. Datos reales
No aplica (sin BD).

## 4. Flujo actual
Previa → fila `cierre` y plan contra real → criterios 2 a 5 → celda y línea `Estado:`. Con trabajo sin
commit, el criterio 2 falla: `Implementado`. Commits aparte, segunda corrida y otro commit para `Cerrado`.

## 5. Reglas de negocio: conocidas vs código
- Nunca `git commit` sin confirmación explícita (`AGENTS.md` › Git): se mantiene; el cierre pide una
  confirmación para el conjunto.
- `Cerrado` es terminal y se versiona en el índice: no puede quedar publicado sin sus commits.

## 6. Riesgos y casos borde
- Sesión cortada entre escribir `Cerrado` y confirmar: queda `Cerrado` solo en el árbol local; la
  siguiente corrida lo retoma (caso límite de la spec). No llega a Git sin commit.
- Un commit que falla a mitad: RF-5 baja a `Implementado` sin commitear los archivos del cierre.
- Leer un `Cerrado` sin commit desde `/sdd-status`: se lee como `Cerrado` bien formado; el riesgo es
  local y transitorio.

## 7. Preguntas abiertas
Ninguna.
