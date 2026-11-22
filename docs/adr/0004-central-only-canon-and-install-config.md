# Canónicos solo del central y configuración de instalación detectada

Los canónicos tienen una parte que viaja a toda instancia (`canon/`) y otra que solo se compila en el repositorio central (`canon/central/`): sus reglas, ganchos y gates nunca salen de ahí. Cada instalación escribe `aquas.install.json` con el rol (`central` o `instance`), el sistema operativo, la arquitectura, la shell y las rutas de la raíz y de `product/`, detectados en el momento; los gates leen ese archivo para saber dónde se puede escribir.

Status: accepted (2026-11-22)

## Considered Options

- Archivo marcador `aquas.central.json`: rechazado, separa en dos lugares lo que es un solo dato (el rol de la instalación).
- Rol y entorno en un único `aquas.install.json` generado, y canónicos centrales en su propia carpeta (elegida).

## Consequences

- El compilador excluye `canon/central/` cuando el rol es `instance`; `verify` falla si un archivo central aparece en una instancia.
- Si la carpeta se mueve, la ruta guardada no coincide y `verify` pide `aquas-ai update`.
- `aquas.install.json` es generado y depende de la máquina: no se edita a mano ni se versiona en el producto.
