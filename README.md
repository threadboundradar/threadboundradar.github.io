# Threadbound Radar — sitio de documentación

Fuente de <https://threadboundradar.github.io/>, la documentación pública del plugin de diagnóstico
de rendimiento para Unreal Engine. Hecho con [MkDocs](https://www.mkdocs.org/) y el tema
[Material](https://squidfunk.github.io/mkdocs-material/).

## Ver el sitio en local

```bash
python -m pip install mkdocs-material
python -m mkdocs serve
```

Queda en `http://127.0.0.1:8000` y recarga solo al guardar.

## Generar el sitio estático

```bash
python -m mkdocs build --strict
```

Escribe el HTML en `site/`, que está ignorado por git.

## Publicación

Cada push a `main` dispara `.github/workflows/deploy.yml`, que compila con `--strict` y publica en
GitHub Pages. `--strict` convierte cualquier advertencia de MkDocs en error, así que un enlace roto
corta el deploy en vez de publicarse.

## Pendiente

- Las páginas de referencia por regla, que dependen de traducir `rules_db.json` al inglés.
- El campo `regimes` de las reglas se va a renombrar a `editor_states`; la guía de reglas propias
  todavía documenta el nombre viejo porque es el que sigue vigente en el código.
