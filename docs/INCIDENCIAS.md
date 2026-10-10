# INCIDENCIAS — registro de fallos del flujo de handoff

Registro de hechos con fecha, solo se añade: si una entrada cambia, se agrega otra.
Las prácticas viven en docs/METODO.md; el estado, en docs/HANDOFF.md.
Campos: qué pasó / causa / detección / respuesta / estado.

## I-01 2026-10-09 La IA eliminó líneas del usuario al regenerar el HANDOFF
- Qué pasó: el HANDOFF de cierre de la pieza 1 llegó sin la línea "URL raw HANDOFF"
  ni la del editor, que el usuario había añadido.
- Causa: el chat regeneró el archivo desde la versión que había leído, anterior a
  las ediciones manuales aún no publicadas.
- Detección: el usuario comparó ambas versiones.
- Respuesta: git diff antes de cada commit; la IA conserva CONTEXTO, DECISIONES y
  RESTRICCIONES (METODO.md R-05).
- Estado: mitigada. Observada una vez.

## I-02 2026-10-09 Ruta local de disco en la primera línea del HANDOFF
- Qué pasó: el archivo comenzaba con una ruta local, en un repo público.
- Causa: origen no determinado; aparece en el archivo del usuario y en el HANDOFF
  regenerado por el chat.
- Detección: revisión del git diff antes del commit (44e3f10).
- Respuesta: línea eliminada y comprobación con Select-String de rutas y correos.
- Estado: cerrada en main. La ruta persiste en commits anteriores del historial
  público; no contenía secretos y no se reescribe la historia.

## I-03 2026-10-09 Estado afirmado sin verificar
- Qué pasó: se redactó el HANDOFF dando la pieza 1 por fusionada en main.
- Causa: se tomó el reporte de un chat como hecho.
- Detección: git log y git status antes del commit mostraron que la rama no estaba
  fusionada ni publicada.
- Respuesta: ESTADO y SIGUIENTE ACCIÓN corregidos antes del commit (METODO.md R-03).
- Estado: cerrada.

## I-04 2026-10-09 Push incompleto de METODO.md
- Qué pasó: se publicó METODO.md sin parte del contenido y con una etiqueta
  perdida ("Cómo.") en P-01.
- Causa: no determinada (edición manual).
- Detección: el rango del push (892fec9..7c1e634) mostró commits no reportados.
- Respuesta: commit nuevo de reparación (2478b37), sin amend por estar publicado.
- Estado: cerrada. Verificadas las 8 etiquetas de P-01.

## I-05 2026-10-09 Base del HANDOFF sin punto de comparación exacto
- Qué pasó: en tres arranques, el chat o el especialista esperaban un hash de main
  que ya no era el real y juzgaron por coherencia.
- Causa: la Base no traía hash; no puede contenerse a sí misma.
- Respuesta: P-02 en METODO.md (bloque de arranque v1).
- Estado: en evaluación; pendiente de verificar con una pieza fusionada.

## I-06 2026-10-10 Hora del HANDOFF escrita a mano con fecha errónea
- Qué pasó: la línea Actualizado del Rev 3 decía 2026-10-09 00:30; el commit se
  hizo el 2026-10-10 a las 00:18, con una hora anterior a la del Rev 2.
- Causa: fecha escrita a mano.
- Detección: la fecha del commit (git show --stat HEAD).
- Respuesta: Rev 4 (bfb281e) con la hora obtenida de Get-Date.
- Estado: cerrada.

## I-07 2026-10-10 Bloque de arranque entregado en una sola línea con ;
- Qué pasó: el chat entregó los comandos en una línea separada por ;, mientras
  METODO.md los tiene en líneas.
- Causa: en las Instructions el bloque está en prosa.
- Efecto: menos legible; con ; un fallo intermedio no detiene los siguientes.
- Respuesta: ninguna todavía; opción v2, primero en METODO.md y luego en las
  Instructions con la misma etiqueta.
- Estado: abierta.
