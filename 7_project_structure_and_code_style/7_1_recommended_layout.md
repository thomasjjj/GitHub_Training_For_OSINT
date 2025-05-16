# 7.1 Recommended Layout (`src/`, `tests/`, `docs/`)

A well-organized project structure is the foundation of maintainable, collaborative code. This lesson introduces the recommended "src layout" pattern that keeps import paths clean, separates code from metadata, and provides a scalable structure for your OSINT tools.

## Why Project Structure Matters

The way you organize your code has significant impacts:

1. **Maintainability** - New contributors can quickly understand where to find things
2. **Import clarity** - Prevents complex, error-prone import paths
3. **Testing ease** - Allows for proper test isolation
4. **Documentation organization** - Keeps docs close to but separate from code
5. **Scalability** - A good structure grows with your project's complexity

For OSINT tools, where projects often start small but expand as data sources and capabilities grow, having a scalable structure from the beginning is particularly valuable.

## The "src Layout" Pattern

The "src layout" is a widely recommended pattern that separates your actual package code from project metadata, tests, and documentation:

```
project_name/              # Root directory
├── src/                   # Source code directory
│   └── package_name/      # Actual Python package
│       ├── __init__.py
│       ├── module1.py
│       ├── module2.py
│       └── subpackage/
│           ├── __init__.py
│           └── module3.py
├── tests/                 # Test directory
│   ├── test_module1.py
│   ├── test_module2.py
│   └── test_subpackage/
│       └── test_module3.py
├── docs/                  # Documentation directory
│   ├── index.md
│   ├── usage.md
│   └── examples.md
├── .gitignore             # Git ignore file
├── README.md              # Project overview
├── pyproject.toml         # Project metadata and dependencies
├── LICENSE                # License file
└── .env.example           # Template for environment variables
```

## Benefits of the src Layout

### 1. Clean Import Paths

With the src layout, imports are always from the package root:

```python
# Good (absolute imports from the package root)
from package_name.module1 import function1
from package_name.subpackage.module3 import Class1

# Avoid (relative imports can get confusing in larger projects)
from ..module1 import function1
```

### 2. Installation for Development

You can install your package in "development mode" while working on it:

```bash
pip install -e .
```

This makes imports work correctly regardless of which directory you're in.

### 3. Test Isolation

Tests are completely separate from your source code, ensuring:
- Tests don't accidentally become part of your package
- Tests must import your package the same way end users would
- Tests can have their own utilities and fixtures without polluting your package

### 4. Documentation Organization

Keeping documentation in its own directory:
- Makes it easier to convert to a documentation site with tools like MkDocs
- Prevents doc files from cluttering your code
- Makes it clear where to look for high-level information

## Directory Details

### `/src/package_name/`

This is where your actual code lives:

- The outer `src/` directory is a container that isn't imported directly
- The inner `package_name/` is your actual Python package
- `__init__.py` files are required to make directories importable as packages
- Main functionality goes in module files (`.py`)
- Group related functionality in subpackages

**OSINT Example Structure**:
```
src/
└── osint_toolkit/
    ├── __init__.py                 # Package version and exports
    ├── collectors/                 # Data collection modules
    │   ├── __init__.py
    │   ├── twitter_collector.py
    │   └── web_scraper.py
    ├── analyzers/                  # Data analysis modules
    │   ├── __init__.py
    │   ├── text_analyzer.py
    │   └── network_analyzer.py
    ├── visualizers/                # Data visualization modules
    │   ├── __init__.py
    │   └── network_graph.py
    └── utils/                      # Utility functions
        ├── __init__.py
        └── formatting.py
```

### `/tests/`

The tests directory mirrors your package structure:

- Test files are named `test_*.py` by convention
- Each module typically has a corresponding test file
- Tests can be organized in subdirectories matching your package structure
- Test utilities and fixtures can be in their own modules

**OSINT Test Example**:
```
tests/
├── conftest.py                     # Shared pytest fixtures
├── test_collectors/
│   ├── test_twitter_collector.py
│   └── test_web_scraper.py
├── test_analyzers/
│   ├── test_text_analyzer.py
│   └── test_network_analyzer.py
└── utils/                          # Test utilities
    └── test_data_generator.py
```

### `/docs/`

Documentation files are typically written in Markdown or reStructuredText:

- `index.md` - Main entry point / overview
- Files for each major component or feature
- Tutorials, API reference, etc.

**OSINT Docs Example**:
```
docs/
├── index.md                        # Overview and introduction
├── installation.md                 # Installation instructions
├── usage/
│   ├── twitter_collection.md
│   ├── web_scraping.md
│   └── network_analysis.md
├── api/                            # API documentation
│   └── reference.md
└── development/                    # Developer documentation
    ├── contributing.md
    └── testing.md
```

### Other Root Files

- `README.md` - Brief overview, installation, and quick start
- `pyproject.toml` - Modern Python project metadata and dependencies
- `LICENSE` - Open source license
- `.gitignore` - Specifies files Git should ignore
- `.env.example` - Template for environment variables
- `CHANGELOG.md` - Document version changes

## Setting Up This Structure for a New Project

To set up a new project with this structure:

1. **Create the directory skeleton**:
   ```bash
   mkdir -p my_osint_tool/src/my_osint_tool
   mkdir -p my_osint_tool/tests
   mkdir -p my_osint_tool/docs
   touch my_osint_tool/src/my_osint_tool/__init__.py
   touch my_osint_tool/README.md
   ```

2. **Initialize Git**:
   ```bash
   cd my_osint_tool
   git init
   ```

3. **Create a basic pyproject.toml**:
   ```toml
   [build-system]
   requires = ["setuptools>=61.0"]
   build-backend = "setuptools.build_meta"

   [project]
   name = "my_osint_tool"
   version = "0.1.0"
   authors = [
     {name = "Your Name", email = "your.email@example.com"},
   ]
   description = "A tool for OSINT data collection and analysis"
   readme = "README.md"
   requires-python = ">=3.8"
   classifiers = [
     "Programming Language :: Python :: 3",
     "License :: OSI Approved :: MIT License",
     "Operating System :: OS Independent",
   ]
   dependencies = [
     "requests>=2.28.0",
     "beautifulsoup4>=4.11.0",
   ]

   [project.optional-dependencies]
   dev = [
     "pytest>=7.0.0",
     "black>=22.3.0",
     "isort>=5.10.0",
   ]
   ```

## Transitioning an Existing Project

If you have an existing project with a different structure:

1. **Create the new directories**:
   ```bash
   mkdir -p src/package_name
   mkdir -p tests
   mkdir -p docs
   ```

2. **Move your existing code**:
   - Move your package code into `src/package_name/`
   - Move your tests into `tests/`
   - Move your documentation into `docs/`

3. **Update imports**:
   - Change any relative imports to absolute ones
   - Update import paths to reflect the new structure

4. **Install in development mode**:
   ```bash
   pip install -e .
   ```

## OSINT-Specific Structure Considerations

For OSINT projects, consider these additional recommendations:

1. **Data directory** - Include a `data/` directory in your `.gitignore` for storing sample data (but not sensitive data)
2. **Results separation** - Keep a `results/` directory for outputs, also in `.gitignore`
3. **Credential isolation** - Use a separate module for API client configuration
4. **Collection vs. Analysis** - Clearly separate data collection from analysis functionality
5. **Documentation privacy** - Be careful not to document sensitive techniques in public repositories

## Next Steps

Now that you understand the recommended project structure, the next step is to learn about centralizing configuration values. In the next section, we'll explore how to create a `config.py` file to manage constants and settings in your OSINT tools.
