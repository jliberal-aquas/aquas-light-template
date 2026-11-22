# La raíz de una instancia no tiene Git; todo lo mutable vive en `product/`

En una instancia, la raíz contiene solo el gobierno compilado (canónicos, ganchos, archivos del arnés) y no tiene Git, para que nadie pueda modificar ni empujar el gobierno desde un producto. Todo lo que cambia (código, backlog, memoria caliente, specs, telemetría, archivo) vive dentro de `product/` y se versiona con el Git del producto.

Status: accepted (2026-11-22)

## Considered Options

- Raíz con Git propio: rechazado, el producto podría alterar el gobierno.
- Raíz sin Git y estado de gobierno en la raíz: rechazado, el backlog y la auditoría quedarían sin historial.
- Raíz sin Git, inmutable, y estado en `product/aquas/` (elegida).

## Consequences

- El agente escribe solo dentro de `product/`; un gancho bloquea todo lo demás.
- Sin Git en la raíz, la integridad se comprueba con hashes: `aquas.lock.json` guarda el `sha256` de cada archivo instalado y `aquas-ai verify` detecta cualquier edición.
- `aquas-ai update` puede reinstalar la raíz entera sin perder nada.
- En el repositorio central la raíz sí tiene Git, porque ahí se edita el gobierno.
