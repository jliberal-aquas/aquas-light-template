# Método

Cómo se construye. El qué y el orden están en `product/aquas/backlog.json`.

## Composición

- Iniciativa → feature → slice. Cada slice referencia su feature e iniciativa.
- Un bug referencia el slice que repara.
- Una necesidad (`need`) está en `to_define`: sin iniciativa, con la ruta a su spec y el prompt crudo.
- Cada ítem guarda la ruta a su spec en `product/aquas/work/requests/<id>.json`; los slices comparten el spec de su feature. El spec se lee solo cuando le toca a ese ítem.

## Estados y orden

- Estados: `to_define`, `ready`, `in_progress`, `standby`, `cancelled`, `done`.
- Solo los ítems de trabajo (slice, bug, necesidad en refinamiento) se ejecutan, y solo uno puede estar en `in_progress`. Iniciativas y features son contenedores. Todo ítem de trabajo termina, pasa a `standby` o se cancela antes de abrir otro.
- Cada alta o cambio del backlog se reordena con `aquas-ai backlog sort`. Bandas fijas: bugs, luego `to_define`, luego el resto por `rank`; empate, el más antiguo.
- `aquas-ai backlog move <id> --before <id> --reason "<por qué>"` mueve solo dentro de su banda y nunca antes de un ítem de su `depends_on`. El agente lo ejecuta solo con el «sí» del humano en el mismo turno.

## Refinamiento

Antes de actuar, clasifica cada mensaje del humano: comentario, pregunta, respuesta, necesidad, error u orden. Si mezcla tipos, sepáralo.

1. Una necesidad se refina con contexto: palabras del humano y fecha, escenario (quién, dónde, cómo arranca, qué observa al final) y qué falta preguntar.
2. Sin ítem en curso, interrógala en rondas con la skill `grill-with-docs` hasta que no quede duda. Recién ahí escríbela como feature y slices; el humano la aprueba.
3. Con un ítem en curso, guarda la necesidad o el error como ítem `to_define` con su prompt crudo literal. No la trabajes.
4. Un cambio de alcance vuelve a la iniciativa y la feature: decide si agranda un slice, crea uno nuevo o crea una feature.

## Slice

1. El implementador escribe la prueba técnica y la del escenario; las corre y deben fallar. La salida roja va al log.
2. Escribe el código hasta verde.
3. El QA, en contexto limpio: confirma verde, quita el código del slice y confirma rojo, y ataca con pruebas de tabla de muchos valores (límites, inválidos).
4. Si el QA rompe algo, el slice vuelve al implementador.
5. Declara `doc_impact` (`none`, `capability` o `decision`); si es `capability`, resella esas páginas en el mismo commit; si es `decision`, escribe el ADR nuevo y marca el anterior como reemplazado.
6. Commit en el Git del producto con la línea `Docs: reviewed=<path|none>; false=<yes|no>; promise=<yes|no>`.

## Bugs y cambios de alcance

- Un cambio de alcance reabre la aceptación de la feature y vuelve a pasar por el auditor.
- Las páginas de capacidad describen solo el presente y no mencionan IDs de ítems; el porqué de cada cambio vive en los ADR.

## Documentación

- Tres ámbitos que no se mezclan: gobierno (`docs/` del central), producto del central y producto de cada instancia (`product/docs/`). A una instancia solo viajan los controles para documentar, nunca la documentación (ADR 0005).

## Dependencias e iniciativa

- Las pruebas forman un árbol y cada nivel prueba solo lo que el nivel de abajo no puede ver:
  - Bug: su prueba de regresión, que falla antes del arreglo y pasa después.
  - Slice: sus pruebas unitarias, que incluyen las de regresión de sus bugs.
  - Feature: sus pruebas integrales, de punta a punta, que comprueban que sus slices combinan bien.
  - Iniciativa: su prueba integral, de punta a punta, que comprueba que sus features combinan bien.
  - Ej.: en una API, la feature «usuarios» prueba crear, leer y modificar juntos; la iniciativa prueba usuarios con roles.
- Al cerrar un bug se corren hacia arriba: su prueba, las del slice, las integrales de la feature y, si la feature ya cerró, la de la iniciativa.
- Cada feature lista en su spec (`details.dependencies`) las features de las que depende y por qué; el análisis de impacto las lee de ahí.
- Lo terminado sigue en el backlog hasta cerrar su feature; al cerrarla, la feature y sus slices se mudan juntos a `archive/<iniciativa>.json`. Un id archivado sigue siendo dependencia válida.

## Feature

1. Pruebas integrales de punta a punta de la feature.
2. Arquitecto: `go vet`, `golangci-lint` (incluye `dupl`), `go fix -diff ./...`, catálogo de `aquas-ai index` contra funciones duplicadas, guías `go-modern-guidelines`. Mientras una herramienta del método no exista, el paso se hace a mano y la excepción queda en la evidencia de la feature con su motivo.
3. Auditor: auditoría de cierre. También revisó el plan antes de la aprobación del humano.
4. Actualiza la página de cada capacidad tocada; la wiki nunca se quita.
5. Muda la feature al archivo con `closed_at` y `evidence`.
6. Di «Hay N candidatos para retrospectiva» o «Sin candidatos para retrospectiva».
7. Recomienda `/clear`: una sesión dura una feature.

## Controles y retrospectiva

- Un control nuevo nace de una falla observada, anotada en `product/aquas/work/defects.json`.
- La retrospectiva escribe candidatos en `product/aquas/work/retro.json`. Un candidato entra al backlog como `to_define` si su clase de falla se repitió dos o más veces, o si una sola bloqueó la entrega. El humano lo aprueba en el refinamiento.

## Memoria y telemetría

- `product/aquas/work/memory.json` se sobrescribe en cada paso del ítem en curso y se archiva al cerrarlo.
- Al cerrar cada turno, un gancho escribe una línea en `product/aquas/work/telemetry.jsonl` leyendo la sesión del arnés.
