# Virtual Environments and Dependencies (Python)

## Rules

- Always use a virtual environment — never install packages globally for a project
- Pin all dependency versions — reproducible builds are non-negotiable
- Never commit the virtual environment folder (`venv/`, `.venv/`, `env/`)
- Commit the lock file (`poetry.lock`, `requirements.txt`) — this is the source of truth

## With Poetry (recommended)

```bash
# Create a new project
poetry new myproject

# Add a dependency
poetry add fastapi

# Add a dev dependency
poetry add --dev pytest black ruff

# Install all dependencies (including dev)
poetry install

# Install only production dependencies
poetry install --only main

# Run a command in the virtualenv
poetry run python manage.py runserver
poetry run pytest

# Activate the shell
poetry shell
```

```toml
# pyproject.toml — always pin minimum versions
[tool.poetry.dependencies]
python = "^3.11"
fastapi = "^0.110.0"
sqlalchemy = "^2.0.0"

[tool.poetry.group.dev.dependencies]
pytest = "^8.0.0"
ruff = "^0.3.0"
```

## With pip + venv

```bash
# Create virtual environment
python -m venv .venv

# Activate
source .venv/bin/activate        # macOS/Linux
.venv\Scripts\activate           # Windows

# Install pinned dependencies
pip install -r requirements.txt

# Generate pinned requirements from current environment
pip freeze > requirements.txt
```

```
# requirements.txt — always pin exact versions
fastapi==0.110.1
uvicorn==0.29.0
sqlalchemy==2.0.29
pydantic==2.6.4
```

## .gitignore

```
.venv/
venv/
env/
__pycache__/
*.pyc
*.pyo
.pytest_cache/
```

## Why

Unpinned dependencies lead to "works on my machine" failures. A `requirements.txt` with `fastapi` (no version) will install whatever is latest at the time — which may break existing code after a major version bump.
