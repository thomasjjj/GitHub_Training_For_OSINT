# 9.2 Dependency Management with Poetry or pip-tools

Managing Python dependencies effectively is crucial for creating reliable, reproducible OSINT tools. This lesson introduces modern dependency management approaches using Poetry and pip-tools, both of which leverage the `pyproject.toml` standard to precisely control package versions and build distribution packages.

## The Challenge of Dependency Management

Python projects typically rely on external libraries, but managing these dependencies presents several challenges:

1. **Version conflicts** - Different packages requiring incompatible versions
2. **Reproducibility** - Ensuring everyone has identical environments
3. **Dependency hell** - Complex dependency trees with hidden incompatibilities
4. **Transitive dependencies** - Dependencies of your dependencies
5. **Build requirements** - Managing development vs. runtime dependencies

For OSINT tools, these issues are especially important because:
- You often integrate with many different services and APIs
- Tools may be deployed in various environments (analyst workstations, servers, CI/CD)
- Consistent results are critical for investigations

## Modern Python Dependency Solutions

Two popular tools have emerged to solve these challenges:

1. **Poetry** - A complete packaging and dependency management tool
2. **pip-tools** - A lightweight solution focused on generating precise dependency lockfiles

Both tools:
- Use the modern `pyproject.toml` standard
- Generate lockfiles that pin exact versions for reproducibility
- Support separate development and production dependencies
- Build wheels (pre-compiled packages) for faster installation

## Poetry: All-in-One Dependency Management

Poetry provides a complete solution for Python project management:

```
├── pyproject.toml      # Dependencies and project metadata
├── poetry.lock         # Locked dependencies with exact versions
└── src/
    └── your_package/   # Your source code
```

### Getting Started with Poetry

1. **Installation**:
   ```bash
   curl -sSL https://install.python-poetry.org | python3 -
   ```

2. **Creating a new project**:
   ```bash
   poetry new osint-toolkit
   cd osint-toolkit
   ```

3. **Using with an existing project**:
   ```bash
   cd your-project
   poetry init
   ```

### Poetry Project Configuration

Poetry stores all configuration in `pyproject.toml`:

```toml
[tool.poetry]
name = "osint-toolkit"
version = "0.1.0"
description = "Tools for OSINT data collection and analysis"
authors = ["Your Name <your.email@example.com>"]
readme = "README.md"
packages = [{include = "osint_toolkit", from = "src"}]

[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.28.1"
beautifulsoup4 = "^4.11.1"
tweepy = "^4.10.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.2.0"
black = "^22.10.0"
flake8 = "^6.0.0"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

### Managing Dependencies with Poetry

#### Adding Dependencies

```bash
# Add regular dependencies
poetry add requests beautifulsoup4

# Add development dependencies
poetry add --group dev pytest black flake8

# Specify exact versions
poetry add requests==2.28.1

# Specify version constraints
poetry add "requests>=2.28.0,<3.0.0"
```

#### Version Specifiers in Poetry

Poetry uses caret (`^`) as the default version constraint:

- `^1.2.3` means `>=1.2.3,<2.0.0` (compatible with 1.x.x)
- `^0.2.3` means `>=0.2.3,<0.3.0` (compatible with 0.2.x)
- `^0.0.3` means `>=0.0.3,<0.0.4` (only the exact version)

You can also use:
- `~1.2.3` for `>=1.2.3,<1.3.0` (patch-level compatibility)
- `*` for any version
- Exact version: `1.2.3`

#### The poetry.lock File

When you add dependencies, Poetry creates or updates `poetry.lock`:

```bash
poetry update
```

This lockfile contains:
- Exact versions of all dependencies (including transitive ones)
- Hashes for verification
- Package metadata
- Dependency markers (e.g., OS or Python version restrictions)

**Always commit your `poetry.lock` file to Git** to ensure everyone uses identical dependencies.

### Working with Virtual Environments

Poetry automatically creates and manages virtual environments:

```bash
# Create/activate the virtual environment and run a command
poetry run python script.py

# Spawn a shell in the virtual environment
poetry shell

# Show the path to the virtual environment
poetry env info
```

### Installing from Lock File

To install dependencies from `poetry.lock`:

```bash
poetry install

# Install only production dependencies (no dev)
poetry install --no-dev
```

This ensures everyone gets the exact same packages, regardless of when they install.

## pip-tools: Lightweight Alternative

If Poetry seems too heavy, pip-tools provides a more minimalist approach:

```
├── pyproject.toml      # Dependencies and project metadata
├── requirements.txt    # Locked production dependencies
├── requirements-dev.txt # Locked development dependencies
└── src/
    └── your_package/   # Your source code
```

### Getting Started with pip-tools

1. **Installation**:
   ```bash
   pip install pip-tools
   ```

2. **Setup with pyproject.toml**:
   Create a `pyproject.toml` file:

   ```toml
   [build-system]
   requires = ["setuptools>=42", "wheel"]
   build-backend = "setuptools.build_meta"

   [project]
   name = "osint-toolkit"
   version = "0.1.0"
   description = "Tools for OSINT data collection and analysis"
   readme = "README.md"
   authors = [
       {name = "Your Name", email = "your.email@example.com"}
   ]
   requires-python = ">=3.9"
   dependencies = [
       "requests>=2.28.0",
       "beautifulsoup4>=4.11.0",
       "tweepy>=4.10.0",
   ]

   [project.optional-dependencies]
   dev = [
       "pytest>=7.2.0",
       "black>=22.10.0",
       "flake8>=6.0.0",
   ]
   ```

### Generating Lock Files with pip-tools

Use `pip-compile` to generate precise requirements files:

```bash
# Generate requirements.txt from pyproject.toml
pip-compile pyproject.toml -o requirements.txt

# Generate dev requirements including primary dependencies
pip-compile pyproject.toml --extra=dev -o requirements-dev.txt
```

The generated files will look something like:

```
# requirements.txt
beautifulsoup4==4.11.1
    # via osint-toolkit (pyproject.toml)
certifi==2022.12.7
    # via requests
charset-normalizer==2.1.1
    # via requests
idna==3.4
    # via requests
requests==2.28.1
    # via
    #   osint-toolkit (pyproject.toml)
    #   tweepy
soupsieve==2.3.2.post1
    # via beautifulsoup4
tweepy==4.12.0
    # via osint-toolkit (pyproject.toml)
urllib3==1.26.13
    # via requests
```

Note how pip-compile:
- Pins exact versions for all dependencies
- Shows the dependency chain (what requires what)
- Includes transitive dependencies (dependencies of dependencies)

### Installing Dependencies with pip-tools

To install the locked dependencies:

```bash
# Install main dependencies
pip-sync requirements.txt

# Install all dependencies including dev
pip-sync requirements.txt requirements-dev.txt
```

### Updating Dependencies

To update your locked dependencies:

```bash
# Update all packages
pip-compile --upgrade pyproject.toml -o requirements.txt

# Update specific packages
pip-compile --upgrade-package requests pyproject.toml -o requirements.txt
```

## Dependency Management for OSINT Projects

OSINT tools have specific dependency management concerns:

### 1. API Client Versioning

Many OSINT tools depend on API clients that change frequently:

```toml
# Using Poetry
[tool.poetry.dependencies]
tweepy = "^4.12.0"  # Twitter API client
praw = "^7.6.1"     # Reddit API client
shodan = "^1.28.0"  # Shodan API client
```

When APIs change, pin to the last working version:

```toml
# Fixed version due to API changes
tweepy = "4.10.1"  # Last version that works with Twitter API v1.1
```

### 2. Handling Optional Dependencies

For features that require heavy or specialized dependencies:

```toml
# Poetry optional groups
[tool.poetry.group.geo.dependencies]
geopandas = "^0.12.2"
folium = "^0.14.0"

[tool.poetry.group.viz.dependencies]
matplotlib = "^3.6.2"
seaborn = "^0.12.1"
```

Users can install only what they need:

```bash
# Install with geographic features
poetry install --with geo

# Install with visualization features
poetry install --with viz
```

### 3. Managing Security-Critical Dependencies

For dependencies with security implications:

```toml
[tool.poetry.dependencies]
# Security-critical packages should pin minor versions at minimum
cryptography = "^39.0.0"
requests = "^2.28.0"

[tool.poetry.group.dev.dependencies]
# Regular security scans
safety = "^2.3.5"
bandit = "^1.7.4"
```

Add security scanning to your workflow:

```bash
# Check for vulnerable dependencies
poetry run safety check

# Scan code for security issues
poetry run bandit -r src/
```

## Building Wheels with Poetry and pip-tools

Both tools support building distribution packages (wheels):

### Building with Poetry

```bash
# Build both source distribution and wheel
poetry build

# Install the built package (for testing)
pip install dist/osint_toolkit-0.1.0-py3-none-any.whl
```

The built packages will be in the `dist/` directory:
- `osint_toolkit-0.1.0.tar.gz` (source distribution)
- `osint_toolkit-0.1.0-py3-none-any.whl` (wheel)

### Building with pip-tools and build

With pip-tools, use the standard `build` package:

```bash
# Install build
pip install build

# Build both source distribution and wheel
python -m build

# Install the built package (for testing)
pip install dist/osint_toolkit-0.1.0-py3-none-any.whl
```

## Choosing Between Poetry and pip-tools

| Feature | Poetry | pip-tools |
|---------|--------|-----------|
| Learning curve | Steeper | Simpler |
| Command complexity | More commands | Fewer commands |
| Project management | Complete solution | Dependencies only |
| Installation | External installer | pip install |
| Virtual env management | Built-in | Use with venv |
| Build system | Built-in | Requires build |
| Project scaffolding | Yes | No |
| Community adoption | Growing quickly | Widespread |

**Use Poetry if:**
- You want an all-in-one solution
- You're starting a new project
- You prefer a polished CLI experience
- You need built-in virtual environment management

**Use pip-tools if:**
- You prefer minimalism
- You're adding to an existing workflow
- You want to use standard pip
- You need maximum compatibility

## Best Practices for Dependency Management

Regardless of which tool you choose:

1. **Always use a lockfile** - Commit `poetry.lock` or `requirements.txt` to Git
2. **Pin versions appropriately** - Use caret (`^`) for most, exact versions for problematic packages
3. **Separate dev dependencies** - Keep testing and development tools separate from runtime dependencies
4. **Update regularly** - Schedule regular dependency updates to stay current with security fixes
5. **Use CI/CD to verify** - Test that locked dependencies work across environments
6. **Document dependency decisions** - Note why you pinned certain versions

## Next Steps

With your dependencies properly managed, the next step is to package and publish your OSINT tool to PyPI. This will allow others to install your tool with a simple `pip install` command. In the next section, we'll walk through creating a minimal `pyproject.toml` file, building your package, and uploading it to the Python Package Index.
