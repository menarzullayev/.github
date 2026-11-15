# `.github` — shared defaults for `menarzullayev/*`

Central home for community-health files and **reusable CI workflows** used across
my repositories. GitHub serves the health files automatically to any public repo
that doesn't define its own.

## What's here

| Path | Purpose |
|---|---|
| `SECURITY.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md` | Default community-health files for all public repos. |
| `.github/ISSUE_TEMPLATE/` | Structured bug / feature issue forms. |
| `.github/PULL_REQUEST_TEMPLATE.md` | Default PR checklist. |
| `.github/workflows/ci-python.yml` | **Reusable** Python CI (ruff + pytest, uv, cache). `on: workflow_call`. |
| `.github/workflows/ci-node.yml` | **Reusable** Node/TS CI (lint + typecheck + test, npm cache). `on: workflow_call`. |
| `workflow-templates/` | Starter workflows offered in the "Actions → New workflow" picker. |

## Use a reusable workflow from another repo

Python repo — `.github/workflows/ci.yml`:

```yaml
name: CI
on:
  push: { branches: [main, master] }
  pull_request:
jobs:
  ci:
    uses: menarzullayev/.github/.github/workflows/ci-python.yml@main
    with:
      python-version: "3.12"
    # secrets: inherit   # only if the called workflow needs repo secrets
```

Node/TypeScript repo:

```yaml
jobs:
  ci:
    uses: menarzullayev/.github/.github/workflows/ci-node.yml@main
    with:
      node-version: "20"
```

> Dummy env for configs that read `.env` at import time is passed via the `env-json`
> input (see `ci-python.yml`), so no more copy-pasting env blocks into every repo.
