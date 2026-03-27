# Python Tooling Reference

## 1. Overview

Common tooling options for Python open-source projects. The ecosystem has modernized significantly, but projects should choose tools based on compatibility, contributor familiarity, and release requirements.

## 2. Dependency Management & Virtual Environments

| Tool | Purpose | Speed | Notes |
|---|---|---|---|
| **uv** | Install, venv, lock, run | ⚡ Fastest | Rust-based, drop-in pip replacement, recommended |
| **pip** | Install packages | Moderate | Built-in, universal |
| **pip-tools** | Pin dependencies | Moderate | `pip-compile` for reproducible installs |
| **hatch** | Project management | Moderate | Build, env, test, publish — all-in-one |
| **pdm** | PEP-compliant manager | Moderate | Lock file, workspaces |
| **poetry** | Project management | Moderate | Popular, opinionated (non-standard lock) |

### Example: `uv`

```bash
# Create virtual environment
uv venv

# Install dependencies
uv pip install -e ".[dev]"

# Lock dependencies
uv lock

# Run a script in the project environment
uv run pytest
```

### Virtual Environments & Lock Files

- Always use a virtual environment to isolate project dependencies.
- Use `uv.lock` to track deterministic installs, or `requirements.txt` (via `pip-tools`).

### Specifying Dependencies (pyproject.toml)

```toml
[project]
dependencies = ["httpx>=0.27", "pydantic>=2.0"]

[project.optional-dependencies]
dev = ["pytest>=8.0", "mypy", "ruff"]
```

## 3. Testing

| Tool | Style | Best For |
|---|---|---|
| **pytest** | Fixture-based, assertion rewriting | Everything — the standard |
| **pytest-cov** | Coverage plugin | Coverage reporting |
| **pytest-xdist** | Parallel plugin | Speeding up large suites |
| **hypothesis** | Property-based | Edge-case discovery |
| **tox / nox** | Multi-env runner | Testing across Python versions |

### Example: `pytest`

```toml
# pyproject.toml
[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-ra -q --strict-markers"
markers = [
    "slow: marks tests as slow",
    "integration: marks integration tests",
]
```

```python
# tests/test_core.py
import pytest
from my_package.core import add

def test_add():
    assert add(1, 2) == 3

def test_add_negative():
    assert add(-1, 1) == 0

@pytest.fixture
def sample_data():
    return {"key": "value"}

def test_with_fixture(sample_data):
    assert sample_data["key"] == "value"
```

### Multi-Version Testing with nox

```python
# noxfile.py
import nox

@nox.session(python=["3.10", "3.11", "3.12", "3.13"])
def tests(session):
    session.install(".[dev]")
    session.run("pytest")
```

Run with: `nox`

## 4. Linting and Formatting

| Tool | Purpose | Speed | Notes |
|---|---|---|---|
| **ruff** | Linter + formatter | ⚡ Fastest | Rust-based, replaces flake8 + isort + black |
| **mypy** | Static type checker | Moderate | Most established, strict mode recommended |
| **pyright** | Static type checker | Fast | Microsoft, better inference, used by Pylance |
| **black** | Formatter | Moderate | Legacy — ruff format is the modern replacement |
| **isort** | Import sorter | Moderate | Legacy — ruff handles this |

### Example pairing: `ruff` + `mypy`

```toml
# pyproject.toml
[tool.ruff]
target-version = "py310"
line-length = 100

[tool.ruff.lint]
select = [
    "E",    # pycodestyle errors
    "W",    # pycodestyle warnings
    "F",    # pyflakes
    "I",    # isort
    "UP",   # pyupgrade
    "B",    # flake8-bugbear
    "SIM",  # flake8-simplify
    "TCH",  # flake8-type-checking
    "RUF",  # ruff-specific rules
]

[tool.ruff.lint.isort]
known-first-party = ["my_package"]

[tool.mypy]
strict = true
warn_return_any = true
```

```json
// npm-style scripts via pyproject.toml won't work natively,
// use these commands directly or via a Makefile / taskfile:
```

```bash
ruff check .                    # lint
ruff check . --fix              # lint + auto-fix
ruff format .                   # format
ruff format . --check           # format check (CI)
mypy src/                       # type check
```

## 5. CI/CD (GitHub Actions)

### Basic CI Workflow

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12", "3.13"]
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v5
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - run: uv pip install -e ".[dev]"
      - run: ruff check .
      - run: ruff format . --check
      - run: mypy src/
      - run: pytest --cov
```

### Release Workflow (Trusted Publishing)

```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      id-token: write  # trusted publishing
    environment:
      name: pypi
      url: https://pypi.org/p/my-package
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install build
      - run: python -m build
      - uses: pypa/gh-action-pypi-publish@release/v1
```

## 6. Documentation

| Tool | Output | Best For |
|---|---|---|
| **Sphinx** | HTML, PDF, ePub | Full documentation sites, the standard |
| **MkDocs** | Static HTML | Markdown-based docs |
| **MkDocs Material** | Static HTML | Beautiful Markdown docs |
| **pdoc** | HTML | Quick API reference |

### Sphinx + Furo Theme

```bash
pip install sphinx furo sphinx-autodoc-typehints
sphinx-quickstart docs/
```

### MkDocs Material

```bash
pip install mkdocs-material
mkdocs new .
mkdocs serve
```

### Hosting

- **Read the Docs** — free for OSS, auto-builds from Git pushes
- **GitHub Pages** — `mkdocs gh-deploy` or Sphinx + CI

## 7. Task Runners

Python doesn't have `npm scripts`. Common alternatives:

| Tool | Style | Notes |
|---|---|---|
| **Makefile** | `make test`, `make lint` | Universal, no install |
| **just** | `just test`, `just lint` | Modern, Rust-based, cross-platform |
| **nox** | Python-based | Multi-env, flexible |
| **hatch** | Built-in scripts | Part of hatch project management |

### Makefile Example

```makefile
.PHONY: test lint format typecheck

test:
	pytest --cov

lint:
	ruff check .

format:
	ruff format .

typecheck:
	mypy src/
```
