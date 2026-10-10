# HANDOFF — Flujo de handoff con SSOT externo (fuente de verdad: este archivo, no el chat)

Actualizado: 2026-10-09 00:30 (UTC-5) · Rev 3. Comprobación de arranque: Instructions y METODO.md P-02.

## CONTEXTO
Proyecto en dos capas. (1) Experimento de referencia: app web mínima HTML + JS
puro para practicar el flujo handoff con SSOT en Git. Repo:
https://github.com/Miguelepst/flujo-handoff-SSOT-externo-repositorio
URL raw HANDOFF: https://raw.githubusercontent.com/Miguelepst/flujo-handoff-SSOT-externo-repositorio/main/docs/HANDOFF.md
URL raw METODO: https://raw.githubusercontent.com/Miguelepst/flujo-handoff-SSOT-externo-repositorio/main/docs/METODO.md
(2) Terminado el experimento, este proyecto será el consejero del método: una
duda por chat, con su aprendizaje incorporado al estado persistente.
Meta de fondo: aplicar el método a proyectos largos, primero
complete-android-roadmap-repositorio, en plan gratuito.

## OBJETIVO ACTUAL
Terminar el experimento de referencia (piezas 2 y 3) y luego la Fase B.
No diseñar todavía el método definitivo.

## ESTADO
- Pieza 1 (index.html "Hola Mundo"): completada y fusionada en main (merge f5d2b62).
- Piezas 2 y 3: pendientes.
- Evidencia observada, hipótesis (H-01) y práctica P-02 en evaluación: ver docs/METODO.md.
- Proyecto real (complete-android-roadmap-repositorio): migrado al método;
  su HANDOFF Rev 1 está commiteado en local y el chat nuevo validó la
  lectura y la Base. El push de ci-prueba estaba propuesto; no consta aquí.

## DECISIONES VIGENTES
- SSOT = docs/HANDOFF.md en Git; Context es una copia que se re-sube tras cada cambio.
- HTML + JS puro (4 GB de RAM, sin Android Studio).
- Rama por pieza (feat/pieza-N-...), Conventional Commits, un chat por pieza.
- Actualizar el HANDOFF en la rama de la pieza, antes de fusionar, redactado
  como estado posterior; la IA conserva CONTEXTO, DECISIONES y RESTRICCIONES;
  Rev+1; el usuario revisa git diff.
- Lo no publicado se puede corregir con amend; lo publicado, no.
- Piezas 1-3 quedan como control sin cambios. Fase B aparte, en el mismo repo:
  pieza 4 con CI mínima, protección de main por PR, check requerido y sin bypass.
- Artefactos nuevos solo cuando aporten (README, ROADMAP, ADR...).
- EMA queda fuera de la práctica: el SSOT vive solo en el repo (Opción A).

## RESTRICCIONES
- Plan gratuito, sin RAG; este archivo en una página.
- Windows, PowerShell 7, CudaText (UTF-8). Leer cada bloque antes de ejecutarlo.
- Repo público: sin secretos, rutas personales ni datos privados.
- No hay medida fiable del peso del chat: no inventar umbrales.

## DEUDAS / PENDIENTES
- Definir señales de alerta de chat pesado, a partir de casos observados.
- Registro de incidencias del método (crearlo al tener 3 o más).
- Verificar límites de Context y del lector de URL raw en este plan.

## PLAN
1. Pieza 2 (botón) y pieza 3 (contador), cada una en un chat nuevo.
2. Fase B (pieza 4).
3. Primera consulta al especialista, con una duda real surgida de las piezas.
4. Decidir si la protección de main se practica aquí antes de configurarla en
   el proyecto real (es recuperable, por lo que es recomendable pero opcional).

## SIGUIENTE ACCIÓN
Comprobar que main contiene index.html (`git ls-files index.html`; esperado: `index.html`) y crear feat/pieza-2-boton-cambia-texto
desde main; agregar el botón que cambie el texto del h1.
