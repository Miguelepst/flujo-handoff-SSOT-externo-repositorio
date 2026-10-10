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
git ls-remote --heads origin feat/pieza-N-nombre
git branch --merged main
git branch -d feat/pieza-N-nombre
git branch
```
Esperado: `ls-remote` sin salida (la rama no existe en el remoto); la rama
aparece en `--merged main`; `Deleted branch ... (was <hash>)`; la rama ya no
aparece en la lista final.

**Si falla.** Con "not fully merged", detenerse y revisar. No usar `-D`.
Si la rama sí está en el remoto, borrarla allí es otra decisión (comando
distinto) y se propone aparte, no se hace por inercia.

**No aplica.** Si la rama aún tiene trabajo sin fusionar.

**Evidencia.** 2026-10-09, pieza 1: tras fusionar, el chat nuevo marcó como no
comprobable si la rama seguía en el remoto. Los datos previos (`git status -sb`
sin seguimiento remoto; el único push fue a main) sugerían que no existía, pero
no se había comprobado.

**Estado.** Aplicada una vez (2026-10-09, pieza 1): ls-remote sin salida,
rama fusionada, borrada con -d (era 44e3f10). Se actualiza tras la pieza 2.


## Evidencia disponible (hasta 2026-10-09)
- Observado en este proyecto: un chat nuevo leyó el HANDOFF desde Context y desde
  URL raw; detectó y corrigió su propio error; pidió comprobaciones en lugar de
  fiarse del archivo; marcó como "no verificado" lo que no pudo comprobar.
- Observado una vez: la IA eliminó líneas del usuario al regenerar el HANDOFF.
- Una transición real (proyecto complete-android-roadmap-repositorio): migrada,
  con la lectura y la comparación de estado validadas por el chat nuevo.
- No verificado: límites de Context, fiabilidad del lector de URL raw, costo de
  arranque de cada chat. No existe medida del peso de un chat.

## H-01 Hipótesis: el proceso se desacopla de la conversación
Un proyecto puede durar meses sin que una conversación crezca, si el estado vive
en artefactos persistentes y cada chat parte de un resumen controlado.
- Correcto: el costo por mensaje depende del chat; lo que hay que recordar, del
  proceso. Se pueden separar.
- Demasiado optimista: el estado también crece (si el HANDOFF engorda, reaparece
  el chat gigante dentro de un archivo); cada chat paga un costo de arranque; el
  resumen pierde matices; la fiabilidad depende del usuario; archivar lo hecho no
  prueba lo aprendido.
- Estado: plausible, con una transición observada. No demostrada.

## R Reglas fundamentales
- R-01 Un solo SSOT (Git). Las copias se refrescan; no se editan en paralelo.
- R-02 La IA propone, el usuario revisa el diff y fusiona.
- R-03 Todo estado afirmado se puede verificar (Rev, git log, git status).
- R-04 HANDOFF acotado a una página; si crece, se divide en capas.
- R-05 La IA actualiza ESTADO y PLAN; CONTEXTO, DECISIONES y RESTRICCIONES
  cambian solo por decisión del usuario.
- R-06 Un chat, un propósito.
- R-07 Lo que debe sobrevivir nunca queda solo en el chat.

## E Errores peligrosos
- Aceptar un HANDOFF sin revisar el diff.
- Dos copias editadas a mano; cambiar el HANDOFF y olvidar re-subir Context.
- Decidir en el chat sin registrarlo: el siguiente chat revive lo anterior.
- Un HANDOFF que cuenta historia en lugar de estado.
- Cerrar el chat viejo antes de validar el nuevo.
- Cortar a mitad de un problema sin anotar hipótesis y pruebas hechas.
- Secretos o rutas personales en un repo público (lo publicado no se retira).
- Ejecutar bloques sin leerlos, o en la rama o carpeta equivocada.
- Reescribir lo ya publicado.

## L Límites: lo que el método no resuelve
- No sustituye la comprensión del usuario.
- No guarda el razonamiento (eso son los ADR).
- No evita que la IA se equivoque.
- No reemplaza pruebas ni CI.
- No sirve para trabajo en equipo (issues y PR).
- No mide la cuota.

## D Debilidades
- Pérdida de matiz al resumir.
- Costo de arranque en cada chat.
- Carga manual y dependencia de la disciplina del usuario.
- Herramientas variables (caché de la URL raw, sitios bloqueados, límites de Context).
- No hay medida fiable de "este chat ya pesa demasiado": no se inventan umbrales;
  se calibran señales con casos observados.
- Peor que otro mecanismo en: preguntas sueltas de un día, chats cortos que no
  cambian el estado, conocimiento que se consulta por búsqueda en docs.

## Dentro y fuera del chat
- Fuera: decisiones con su motivo, estado, plan, deudas, reglas, convenciones.
- Dentro, temporal: exploración, borradores, salidas de comandos, hipótesis sin validar.
- Hacer handoff: pieza terminada, PR fusionado, decisión tomada, o chat lento o confuso.
- No hace falta: pregunta puntual, o chat que no cambió el estado persistente.
