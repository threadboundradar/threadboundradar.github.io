# Sitio de documentación

Fuente del sitio público de documentación del plugin, hecho con [MkDocs](https://www.mkdocs.org/)
y el tema [Material](https://squidfunk.github.io/mkdocs-material/).

Carpeta temporal: se moverá a su propio repositorio de GitHub cuando se cree, para servirlo con GitHub Pages.

## Ver el sitio en local

```bash
python -m pip install mkdocs-material
python -m mkdocs serve
```

Queda en `http://127.0.0.1:8000` y recarga solo al guardar.

## Generar el sitio estático

```bash
python -m mkdocs build
```

Escribe el HTML en `site/`, que está ignorado por git. En GitHub Pages no hace falta generarlo a mano:
un workflow puede correr `mkdocs gh-deploy`.

## Pendiente

- Nombre del sitio y URL (`site_name` y `site_url` en `mkdocs.yml`) — dependen del nombre comercial.
- Las páginas de referencia por regla, que dependen de traducir `rules_db.json` al inglés.
