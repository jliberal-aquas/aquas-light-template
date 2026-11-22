# Reglas solo del repositorio central

Se compilan solo cuando `aquas.install.json` dice `"role": "central"`. Nunca viajan a una instancia (ADR 0004).

1. Además de `product/`, en el central puedes escribir en `canon/`, `docs/adr/` y `GLOSSARY.md`.
2. Todo cambio en `canon/` obliga a recompilar los archivos del arnés antes del commit.
3. Antes de publicar una versión del gobierno: `aquas-ai verify` en verde, ADR aceptados y glosario al día.
