# Aquas Light

Gobierno centralizado de la metodología Aquas Driven Development en su versión ligera y portable. Este repositorio es la única fuente de la verdad; cada producto lo importa desde Git.

## Composición del trabajo

**Iniciativa** (`initiative`):
Objetivo de negocio que agrupa una o más features. Toda necesidad aprobada pertenece a una iniciativa.
_Avoid_: épica, proyecto

**Feature** (`feature`):
Capacidad entregable dentro de una iniciativa, con una sola línea de aceptación y pruebas integrales propias.
_Avoid_: funcionalidad, historia

**Slice** (`slice`):
Unidad mínima completable de trabajo: se define, se prueba en rojo, se implementa, se prueba en verde y se cierra sola.
_Avoid_: tarea, ticket, «slide»

**Bug** (`bug`):
Ítem del backlog que repara un slice ya cerrado. Siempre referencia ese slice y va primero en prioridad.
_Avoid_: incidencia, defecto (el defecto es la falla del método, no del producto)

**Necesidad** (`need`):
Pedido del humano aún sin refinar, guardado con su prompt crudo y en estado `to_define`. No tiene iniciativa, feature ni slice todavía.
_Avoid_: idea, pendiente, inbox

**Spec** (`work/requests/<id>.json`):
Archivo con todo el detalle de un ítem: prompt crudo, escenario refinado, aceptación, pruebas y diagrama de flujo. El backlog guarda su ruta y solo se lee cuando le toca a ese ítem.
_Avoid_: ficha, ticket

**Backlog** (`backlog.json`):
Lista priorizada de lo pendiente. Solo contiene trabajo vivo; lo cerrado se muda al archivo. Cada slice referencia su feature e iniciativa; cada bug referencia su slice.
_Avoid_: tablero, lista de tareas, inbox

**Banda** (`band`):
Grupo fijo de orden en el backlog: primero los bugs, después las necesidades `to_define`, después el resto. Ningún movimiento cruza de banda.
_Avoid_: nivel, categoría

**Rango** (`rank`):
Posición de un ítem dentro de su banda. Lo recalcula `backlog sort` y lo cambia `backlog move`; el ID nunca cambia.
_Avoid_: prioridad (en el backlog), orden

**Archivo** (`archive/`):
Histórico de lo cerrado. No entra en contexto salvo para auditoría.
_Avoid_: histórico vivo

## Gobierno

**Canónicos** (`canon/`):
Fuente única de constituciones, reglas, roles y configuraciones. Los archivos de cada arnés se compilan desde aquí y nunca se editan a mano.
_Avoid_: plantillas, config

**Arnés** (`harness`):
Herramienta que ejecuta al agente (Claude Code, Codex, Oh My Pi). Lee archivos compilados desde los canónicos.
_Avoid_: IDE, cliente

**aquas-ai**:
CLI en Go que instala, actualiza, compila y verifica el gobierno en cada producto. Es el producto de este repositorio central.
_Avoid_: aquas-gov

**Repositorio central** (`aquas-light-template`):
Este repositorio. Único lugar donde se editan los canónicos; tiene Git propio.
_Avoid_: template, plantilla madre

**Instancia**:
Carpeta donde `aquas-ai` instaló el gobierno compilado para un producto. Su raíz no tiene Git y no se edita; solo `product/` cambia.
_Avoid_: copia, fork

**Producto** (`product/`):
Lo que construye cada repositorio que importa el gobierno. Vive en la carpeta `product/` con su propio Git.
_Avoid_: app, proyecto

**Contrato de traspaso** (`handoff`):
JSON tipado y validado contra su esquema que un agente envía a otro, y el informe tipado que vuelve. Única forma de comunicación entre agentes.
_Avoid_: mensaje, prompt al subagente

**Memoria caliente** (`work/memory.json`):
Estado del trabajo en curso para retomarlo tras un corte. Vive mientras dura el ítem activo y luego se archiva.
_Avoid_: contexto, notas
