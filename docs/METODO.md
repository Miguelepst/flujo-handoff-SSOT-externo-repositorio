# METODO — registro de prácticas del flujo de handoff con SSOT externo

Estado: borrador. Registra solo prácticas con evidencia o con una razón explícita.
El método completo se destilará después, con más casos. El estado del proyecto
vive en docs/HANDOFF.md, no en este archivo.

## Dónde va cada cosa
- Instructions del proyecto: reglas estables que el chat sigue siempre (una línea).
- docs/HANDOFF.md: estado actual, máximo una página.
- docs/METODO.md: motivo, procedimiento y evidencia de cada práctica.
Un detalle de rutina no entra en el HANDOFF: la regla va en Instructions y el
porqué, aquí.

## P-01 Cierre de pieza: limpieza de la rama

**Qué.** Tras fusionar y publicar la pieza, comprobar que su rama no existe en el
remoto y borrarla en local.

**Por qué.** Sus commits ya están en main por la fusión; la rama es solo una
etiqueta. Dejarlas acumula ruido y ya generó una duda abierta: un chat nuevo no
pudo saber si la rama seguía en el remoto.

**Cuándo.** Después de publicar main y comprobar la URL raw. Una vez por pieza.

**Cómo.**
```powershell
git branch -vv
git ls-remote --heads origin
git branch -d feat/pieza-N-nombre
git branch
```
Esperado: la rama en local sin `[origin/...]`; `ls-remote` solo con
`refs/heads/main`; `Deleted branch ... (was <hash>)`; la rama ya no aparece.

**Si falla.** Con "not fully merged", detenerse y revisar. No usar `-D`.
Si la rama sí está en el remoto, borrarla allí es otra decisión (comando
distinto) y se propone aparte, no se hace por inercia.

**No aplica.** Si la rama aún tiene trabajo sin fusionar.

**Evidencia.** 2026-10-09, pieza 1: tras fusionar, el chat nuevo marcó como no
comprobable si la rama seguía en el remoto. Los datos previos (`git status -sb`
sin seguimiento remoto; el único push fue a main) sugerían que no existía, pero
no se había comprobado.

**Estado.** Propuesta. Aún no probada: se completa tras la primera aplicación.
