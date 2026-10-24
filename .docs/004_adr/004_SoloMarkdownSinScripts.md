# ADR 004: El kit es solo Markdown, sin scripts de instalación

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; 0.4.0)
- **Origen:** CHANGELOG 0.4.0

## Contexto
Los scripts `install.ps1` e `install.sh` dependen del SO y del shell, y el kit es genérico.

## Decisión
Se eliminan los scripts. `INSTALAR.md` está escrito para que el agente detecte SO y shell, y genere un script temporal o dé el paso a paso. No hay código ejecutable versionado.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Mantener `install.ps1` e `install.sh` | Cubren dos entornos y fallan en los demás |

## Consecuencias
La instalación depende de un agente o de comandos manuales. La copia nunca sobrescribe (`cp -R --update=none` en Linux; `cp -n` avisa "non-portable" en GNU coreutils 9.x).
