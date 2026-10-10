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

## P-02 Arranque de chat: comprobar el estado sin hash en el archivo

**Qué.** Al abrir un chat, calcular H (el último commit que tocó docs/HANDOFF.md) y
listar lo que hay en main después de H. Esperado: nada, o solo el merge de la pieza.

**Por qué.** El HANDOFF no puede contener el hash del commit que lo guarda (se
contendría a sí mismo), ni el del merge --no-ff (aún no existe). Un hash de main
escrito en el archivo queda viejo con el primer commit de docs. Calcular H en el
repo no requiere mantener ningún hash y funciona con cualquier tipo de fusión.

**Cuándo.** Al abrir cualquier chat, con la pieza ya fusionada y publicada.

**Cómo.** Bloque v1 (referencia versionada; el chat usa la copia pegada en Instructions, CONTINUIDAD, con la misma etiqueta):
```powershell
git fetch
git status -sb
$h = git log -1 --format=%h -- docs/HANDOFF.md
$h
git log --oneline "${h}..main"
git show main:docs/HANDOFF.md | Select-String '^Actualizado'
```
Esperado: sin ahead/behind ni cambios; `$h` es un commit "docs: HANDOFF Rev N";
log vacío o solo "Merge ..."; la línea Actualizado igual a la de Context.

**Si falla.** Si `${h}..main` trae commits, `git diff --name-only $h main` y
decidir con el usuario si afectan a lo descrito en ESTADO. Preguntar antes de actuar.

**Límites.**
- Todo cambio del HANDOFF sube el Rev; si no, H avanza y oculta un desfase previo.
- Los commits de docs posteriores a H salen como ruido: el HANDOFF va en el último
  commit de una tanda.
- No detecta ediciones manuales con el mismo Rev ni valida el contenido.
- Descartado como comprobación principal: `merge-base --is-ancestor <Base>`, porque
  da 0 mientras main solo avance (detecta reescritura, no desfase).
- El bloque también vive en Instructions, que están fuera de Git y no dejan rastro:
  se cambia primero aquí y se copia con la misma etiqueta (v1, v2...). Si las
  etiquetas difieren, manda la de este archivo.

**Evidencia.** 2026-10-09, la Base sin hash obligó a juzgar por coherencia en tres
arranques. Ninguna decisión resultó errónea, pero ninguna se apoyó en un dato:
pieza 1 (el chat juzgó por coherencia); arranque con Rev 2 (esperaba main = f5d2b62,
hash del estado, con main en 7c1e634; lo aceptó por razonamiento); y la consulta
que originó P-02 (partió de la misma expectativa, f5d2b62). Misma causa en los tres.

**Estado.** Propuesta. Bloque ejecutado una vez en main (2026-10-09, antes del
Rev 3): salida como la esperada, sin merge real de por medio. Sin verificar con una
pieza fusionada; se actualiza tras la pieza 2.

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
