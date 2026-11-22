# El gobierno se distribuye con el CLI `aquas-ai`, no con submódulos

Este repositorio es la fuente única del gobierno. Cada producto lo recibe con `aquas-ai init` y `aquas-ai update`, que descargan una versión fija, compilan los canónicos para el arnés y dejan `aquas.lock.json` con el commit exacto. `aquas-ai verify` comprueba que nada compilado se editó a mano.

Status: accepted (2026-11-22)

## Considered Options

- Submódulo de Git: frágil en Windows y editable por el agente sin aviso.
- Clonar y copiar a mano: la versión se desvía sin que nadie lo note.
- CLI con versión fija (elegida): reproducible y agnóstica del arnés.

## Consequences

- El CLI `aquas-ai` es el producto del repositorio central y vive en `product/`, con su propio Git (`aquas-light-template-cli`). El Git del central ignora `product/`.
- Se escribe en Go 1.27 o superior, en un solo binario para Windows y Linux.
