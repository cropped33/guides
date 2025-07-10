# Migrating to `ruff, ty` from `black, isort, pyright`

## Migrating venv

1. Remove `venv` and create `.venv`
1. In `pre-commit.yml`, rename `venv` to `.venv`.

## Migrating tools

1. Remove `black, isort, pyright` from `pyproject.toml` and add these

```toml
[tool.ruff]
src = ["./backend"]
exclude = ["./backend/**/migrations/"]

[tool.ruff.lint]
extend-select = ["I"]

[tool.ty.environment]
root = ["./backend"]

[tool.ty.src]
exclude = ["./backend/**/migrations/"]
```

1.  Remove `black, isort, pyright` from `.pre-commit-config.yaml` and add these:

```yml
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.12.2
  hooks:
    - id: ruff-check
      args: [--fix]
    - id: ruff-format
- repo: local
  hooks:
    - id: ty-check
      name: ty check
      language: python
      entry: ty check
      pass_filenames: false
      args: [--python=.venv/]
      additional_dependencies: [ty]
```

1. Run `pre-commit run --all-files`, `ty` should pass without any change, `ruff` will fail the first time and pass the second time.
