# Guide to add versioning to docker images

1. This guide is only for projects with `uv` setup because this uses `uv version` command.
1. Make sure that `pyproject.toml` has `version`. Do not use dynamic version because `uv version` does not support dynamic version.
1. In `taskfile.yml`, edit these.

```yml
build:
  cmds:
    - docker build . -t ghcr.io/sandbox-pokhara/PROJECT_NAME -t ghcr.io/sandbox-pokhara/PROJECT_NAME:{{.VERSION}}
  vars:
    VERSION:
      sh: uv version --short

publish:
  cmds:
    - docker image push ghcr.io/sandbox-pokhara/PROJECT_NAME --all-tags
```

1. Before building, it is mandatory to create a bump commit for updating version. So that it's easier to track version in `git log`.
