# PyPI Publishing Reference

## Overview

This reference covers publishing Python packages to PyPI — from `pyproject.toml` metadata through building, uploading, trusted publishing, and versioning.

## pyproject.toml — Publishing Fields

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
authors = [{ name = "Your Name", email = "you@example.com" }]
keywords = ["keyword1", "keyword2"]
classifiers = [
    "Development Status :: 4 - Beta",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
]

[project.urls]
Homepage = "https://github.com/user/my-package"
Repository = "https://github.com/user/my-package"
Issues = "https://github.com/user/my-package/issues"
```

### Classifiers

Classifiers are PyPI's tagging system. Important categories:

| Category | Example |
|---|---|
| Development status | `Development Status :: 4 - Beta` |
| License | `License :: OSI Approved :: MIT License` |
| Python version | `Programming Language :: Python :: 3.12` |
| Typing | `Typing :: Typed` |
| Topic | `Topic :: Software Development :: Libraries` |
| Framework | `Framework :: Pytest` |

Full list: [pypi.org/classifiers](https://pypi.org/classifiers/)

## Building the Package

### Standard Build

```bash
pip install build
python -m build
```

This produces:
```
dist/
├── my_package-1.0.0-py3-none-any.whl   # wheel (preferred)
└── my_package-1.0.0.tar.gz             # source distribution
```

### With uv

```bash
uv build
```

### What Gets Included

The build backend determines what files are included. For `hatchling` with src layout:

- Everything under `src/my_package/` is included by default.
- `README.md`, `LICENSE` are included automatically.
- `tests/`, `docs/`, dotfiles are excluded by default.

To customize:

```toml
[tool.hatch.build.targets.sdist]
include = ["src/my_package", "LICENSE", "README.md"]

[tool.hatch.build.targets.wheel]
packages = ["src/my_package"]
```

### Validating Before Publish

```bash
# Check the built distributions
pip install twine
twine check dist/*

# Inspect wheel contents
unzip -l dist/my_package-1.0.0-py3-none-any.whl

# Install locally and test
pip install dist/my_package-1.0.0-py3-none-any.whl
```

## Publishing to PyPI

### Method 1: Trusted Publishing (Recommended)

No tokens needed. GitHub Actions authenticates directly with PyPI via OIDC.

**One-time setup on PyPI:**
1. Go to [pypi.org/manage/account/publishing](https://pypi.org/manage/account/publishing/)
2. Add a new "pending publisher" with your GitHub repo, workflow file name, and environment name.

**GitHub Actions workflow:**

```yaml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
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

### Method 2: API Token

```bash
# Upload with twine
twine upload dist/*

# twine will prompt for credentials:
#   username: __token__
#   password: pypi-AgEI... (your API token)
```

Store the token in `~/.pypirc`:

```ini
[pypi]
username = __token__
password = pypi-AgEI...
```

Or use environment variable:

```bash
TWINE_USERNAME=__token__ TWINE_PASSWORD=pypi-AgEI... twine upload dist/*
```

### Method 3: uv publish

```bash
uv publish
```

### Test PyPI

Always test with TestPyPI first for new packages:

```bash
# Upload to TestPyPI
twine upload --repository testpypi dist/*

# Install from TestPyPI
pip install --index-url https://test.pypi.org/simple/ my-package
```

## Versioning

### Single Source of Truth

Keep the version in one place. Options:

**Option A: In `pyproject.toml` (simplest)**

```toml
[project]
version = "1.0.0"
```

**Option B: Dynamic from `__init__.py`**

```toml
[project]
dynamic = ["version"]

[tool.hatch.version]
path = "src/my_package/__init__.py"
```

```python
# src/my_package/__init__.py
__version__ = "1.0.0"
```

**Option C: From Git tags**

```toml
[project]
dynamic = ["version"]

[tool.hatch.version]
source = "vcs"

[build-system]
requires = ["hatchling", "hatch-vcs"]
build-backend = "hatchling.build"
```

### Version Bumping

```bash
# With hatch
hatch version patch   # 1.0.0 → 1.0.1
hatch version minor   # 1.0.0 → 1.1.0
hatch version major   # 1.0.0 → 2.0.0
```

### Pre-release Versions

```
1.0.0a1    # alpha
1.0.0b1    # beta
1.0.0rc1   # release candidate
```

Per [PEP 440](https://peps.python.org/pep-0440/) — Python uses different pre-release notation than SemVer.

## Package Naming

- Use lowercase with hyphens: `my-package` (PyPI normalizes to `my-package`).
- The import name uses underscores: `import my_package`.
- Check availability: `pip index versions my-package`.

## Yanking

Yanking marks a version as unsuitable without fully removing it:

```bash
# Via PyPI web interface or:
# Not directly supported via CLI — use the PyPI web UI
```

Yanked versions:
- Won't be installed by default.
- Still available if explicitly requested by version.
- Preferable to full deletion (doesn't break pinned installs).

## Pre-Publish Checklist

- [ ] `python -m build` succeeds
- [ ] `twine check dist/*` passes
- [ ] `mypy src/` reports no errors
- [ ] `pytest` passes
- [ ] `pyproject.toml` has complete metadata (name, version, description, license, URLs)
- [ ] Classifiers are accurate
- [ ] `requires-python` is set correctly
- [ ] `py.typed` marker exists (if shipping type stubs)
- [ ] README renders correctly (PyPI uses the `readme` field)
- [ ] CHANGELOG is updated
- [ ] Tested on TestPyPI first (for new packages)
- [ ] Git tag matches the version

## Resources

- [Python Packaging User Guide](https://packaging.python.org) — authoritative docs
- [PyPI](https://pypi.org) — Python Package Index
- [TestPyPI](https://test.pypi.org) — testing registry
- [Trusted publishers](https://docs.pypi.org/trusted-publishers/) — OIDC-based publishing
- [PEP 440](https://peps.python.org/pep-0440/) — version identification
- [PEP 621](https://peps.python.org/pep-0621/) — pyproject.toml metadata
