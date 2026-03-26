# Python Project Reference

## 1. Overview

This reference covers Python-specific project setup, configuration, and patterns for open-source libraries, CLI tools, and applications. Python's packaging landscape has converged around `pyproject.toml` as the single source of truth.

## 2. Project Structure

```
project-root/
├── src/
│   └── my_package/           # source package (src layout)
│       ├── __init__.py
│       ├── core.py
│       └── py.typed           # PEP 561 marker for type stubs
├── tests/
│   ├── __init__.py
│   ├── test_core.py
│   └── conftest.py            # pytest fixtures
├── docs/
├── scripts/
├── pyproject.toml             # project metadata, build config, tool config
├── LICENSE
├── README.md
├── CHANGELOG.md
└── .gitignore
```

### src Layout vs. Flat Layout

| Layout | Structure | Best For |
|---|---|---|
| **src layout** | `src/my_package/` | Libraries — prevents accidental import of uninstalled code |
| **flat layout** | `my_package/` at root | Small projects, scripts, applications |

**Recommended**: src layout for anything published to PyPI.

## 3. Configuration (pyproject.toml)

### Complete Example

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my-package"
version = "1.0.0"
description = "A clear, one-line description"
readme = "README.md"
license = "MIT"
requires-python = ">=3.10"
authors = [
    { name = "Your Name", email = "you@example.com" },
]
keywords = ["keyword1", "keyword2"]
classifiers = [
    "Development Status :: 4 - Beta",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
    "Programming Language :: Python :: 3.13",
    "Typing :: Typed",
]

[project.urls]
Homepage = "https://github.com/user/my-package"
Repository = "https://github.com/user/my-package"
Issues = "https://github.com/user/my-package/issues"
Documentation = "https://my-package.readthedocs.io"
Changelog = "https://github.com/user/my-package/blob/main/CHANGELOG.md"

[project.optional-dependencies]
dev = [
    "pytest>=8.0",
    "pytest-cov",
    "mypy",
    "ruff",
]

[project.scripts]
my-cli = "my_package.cli:main"
```

### Build Backends

| Backend | Config Style | Best For |
|---|---|---|
| **hatchling** | Minimal, convention-based | General-purpose, modern default |
| **setuptools** | Flexible, legacy-compatible | Existing projects, complex builds |
| **flit-core** | Ultra-minimal | Pure Python, simple packages |
| **maturin** | Rust + Python | Python bindings for Rust code |
| **pdm-backend** | PEP-compliant | pdm users |

**Recommended**: `hatchling` for new projects.

## 4. Modules & Types (Type Hints)

### Why Type Hints Matter for OSS

- Consumers get autocomplete and type checking in their IDEs.
- Type checkers (`mypy`, `pyright`) catch bugs before runtime.
- Increasingly expected in modern Python libraries.

### Making Your Package Typed (PEP 561)

1. Add a `py.typed` marker file in the package root:
   ```
   src/my_package/py.typed     # empty file
   ```

2. Use type annotations throughout your source code:
   ```python
   def add(a: int, b: int) -> int:
       return a + b
   ```

3. Ensure `py.typed` is included in the built package (most build backends do this automatically with src layout).

### Type Checking Configuration

```toml
# pyproject.toml
[tool.mypy]
strict = true
warn_return_any = true
warn_unused_configs = true

[tool.pyright]
typeCheckingMode = "strict"
```

## 5. Entry Points

### CLI Scripts

```toml
[project.scripts]
my-tool = "my_package.cli:main"
```

After installing, users can run `my-tool` directly from the command line.

### Plugin Systems

```toml
[project.entry-points."my_package.plugins"]
my_plugin = "my_plugin_package:Plugin"
```

## 6. Resources

- [Python Packaging User Guide](https://packaging.python.org) — authoritative packaging docs
- [pyproject.toml specification](https://packaging.python.org/en/latest/specifications/pyproject-toml/) — PEP 621 fields
- [PEP 561](https://peps.python.org/pep-0561/) — distributing type information
- [Hatch](https://hatch.pypa.io) — modern Python project management
- [uv](https://docs.astral.sh/uv/) — fast Python package manager
