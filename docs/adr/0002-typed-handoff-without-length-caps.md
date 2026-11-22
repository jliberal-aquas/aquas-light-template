# El traspaso entre agentes se controla por tipo, no por largo

Todo traspaso entre agentes es un JSON validado contra su esquema en `canon/schemas/`. No hay techos de caracteres: un capítulo de libro o un informe largo es legítimo, y un techo lo rompería. Lo que se controla es la forma, los campos obligatorios y los valores de listas cerradas.

Status: accepted (2026-11-22)

## Considered Options

- Techos de caracteres por campo (anexo original): rechazado, rompe entregas grandes legítimas y obliga a parches.
- Validación de tipo sin techos (elegida).

## Consequences

- Reemplaza la tabla de techos del anexo «Delegar».
