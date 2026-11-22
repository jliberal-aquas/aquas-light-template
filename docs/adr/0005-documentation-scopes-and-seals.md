# Documentación en tres ámbitos, con los mismos controles y páginas selladas

La documentación vive en tres ámbitos que nunca se mezclan: el gobierno (`docs/` del repositorio central), el producto del central (`product/docs/` del CLI) y el producto de cada instancia (su `product/docs/`). Lo que viaja a una instancia no es documentación, sino los controles para escribirla: la skill `aquas-document`, la estructura obligatoria de página y los gates de sello. Cada página de capacidad describe solo el presente; el porqué de cada cambio vive en los ADR del ámbito.

Status: accepted (2026-11-22)

## Considered Options

- Llevar la documentación del gobierno a cada instancia: rechazado, mezcla dos productos y crece sin control.
- Documentar por feature o por slice: rechazado, la página acumula historia y deja de explicar lo que el producto hace hoy.
- Tres ámbitos, controles compartidos, páginas en presente y sellos contra el código (elegida).

## Consequences

- `aquas-ai init` crea en la instancia solo el esqueleto vacío de `product/docs/`.
- Cada página de capacidad declara los archivos de código que describe y su `sha256`; si el código cambia y la página no se vuelve a sellar, `verify` falla.
- Una página de capacidad no menciona IDs de ítems (`S001`, `B001`, `F001`); `verify` lo rechaza.
- Un cambio que altera una decisión ya tomada (antes A, ahora B) exige un ADR nuevo que marca al anterior como reemplazado.
