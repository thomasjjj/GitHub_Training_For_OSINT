# 9.3 Packaging & Publishing to PyPI

Making your OSINT tools available to the wider community through PyPI (the Python Package Index) allows others to easily install and use your work with a simple `pip install` command. This lesson guides you through creating a minimal `pyproject.toml` file, building your package, and publishing it to PyPI.

## Why Publish to PyPI?

Publishing your OSINT tools to PyPI offers several advantages:

1. **Easy installation** - Users can install with `pip install your-package`
2. **Dependency management** - pip automatically handles dependencies
3. **Discoverability** - Others can find your tool through PyPI search
4. **Version management** - Users can install specific versions
5. **Professional presentation** - Establishes your tool as a proper package

For OSINT professionals, sharing tools via PyPI helps build community resources and establishes your technical expertise.

## Understanding Python Packaging

Before diving in, let's understand some key packaging concepts:

### Package vs. Module

- **Module**: A single Python file (`.py`)
- **Package**: A directory containing related modules and a special `__init__.py` file
- **Distribution package**: A versioned archive file that can be installed using pip

### Package Configuration

Modern Python packaging uses `pyproject.toml` as the standard configuration file, which defines:

- Metadata (name, version, description, author)
- Dependencies
- Build system requirements
- Optional features
- Entry points (command-line scripts)

## Step 1: Setting Up Your Project Structure

Start with a well-organized project structure:

```
osint_toolkit/                # Project root
├── pyproject.toml           # Package configuration
├── README.md                # Documentation
├── LICENSE                  # License file
└── src/                     # Source directory
    └── osint_toolkit/       # Package directory
        ├── __init__.py      # Package initialization
        ├── collector.py     # Module file
        └── analyzer.py      # Module file
```

The "src layout" (with code in `src/package_name/`) is recommended for packaging, as it:
- Prevents accidental imports from the project directory
- Makes testing against the installed version easier
- Separates package code from project files

## Step 2: Creating a Minimal pyproject.toml

The `pyproject.toml` file defines your package. Here's a minimal example:

```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "osint-toolkit"
version = "0.1.0"
description = "Tools for open source intelligence collection and analysis"
readme = "README.md"
authors = [
    {name = "Your Name", email = "your.email@example.com"}
]
license = {text = "MIT"}
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Information Technology",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
]
keywords = ["osint", "intelligence", "security"]
dependencies = [
    "requests>=2.28.0",
    "beautifulsoup4>=4.11.0",
]
requires-python = ">=3.9"

[project.urls]
Homepage = "https://github.com/yourusername/osint-toolkit"
Issues = "https://github.com/yourusername/osint-toolkit/issues"

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=22.3.0",
]

[project.scripts]
osint-search = "osint_toolkit.cli:search_command"
```

### Key Sections Explained

- **build-system**: Specifies the build tools needed
- **project**: Core package metadata
  - **name**: Package name on PyPI (must be unique)
  - **version**: Package version (follow Semantic Versioning)
  - **description**: One-line summary
  - **readme**: Path to README file
  - **authors**: List of author information
  - **license**: License information
  - **classifiers**: PyPI classifiers for categorization
  - **dependencies**: Required packages
- **project.urls**: Links to project resources
- **project.optional-dependencies**: Extra dependencies for specific features
- **project.scripts**: Command-line entry points

## Step 3: Writing Package Code

### Package __init__.py

The `__init__.py` file defines what's exported from your package:

```python
"""OSINT Toolkit - Tools for open source intelligence collection and analysis."""

__version__ = "0.1.0"

from .collector import TwitterCollector, RedditCollector
from .analyzer import TextAnalyzer, NetworkAnalyzer

__all__ = [
    "TwitterCollector",
    "RedditCollector",
    "TextAnalyzer",
    "NetworkAnalyzer"
]
```

### Including Version Information

It's good practice to include your version number in the package code, which lets users check which version they're using:

```python
# In your code
import osint_toolkit
print(f"Using OSINT Toolkit version {osint_toolkit.__version__}")
```

### Creating Command-Line Scripts

If your tool should be usable from the command line, create CLI entry points:

```python
# src/osint_toolkit/cli.py

import argparse
import sys
from . import __version__
from .collector import TwitterCollector

def search_command():
    """Command-line interface for the search functionality."""
    parser = argparse.ArgumentParser(description="Search for OSINT data")
    parser.add_argument("query", help="Search query")
    parser.add_argument("--platform", choices=["twitter", "reddit"], default="twitter",
                        help="Platform to search (default: twitter)")
    parser.add_argument("--limit", type=int, default=100,
                        help="Maximum number of results (default: 100)")
    parser.add_argument("--version", action="version", 
                        version=f"OSINT Toolkit {__version__}")
    
    args = parser.parse_args()
    
    if args.platform == "twitter":
        collector = TwitterCollector()
        results = collector.search(args.query, limit=args.limit)
        # Process and display results
        for result in results:
            print(f"{result['date']} - {result['user']}: {result['text']}")
    
    return 0

if __name__ == "__main__":
    sys.exit(search_command())
```

## Step 4: Building Your Package

Now you can build distribution packages for your project:

### Using build (recommended)

```bash
# Install build
pip install build

# Build source distribution and wheel
python -m build
```

### Using setuptools directly

```bash
# Install setuptools and wheel
pip install setuptools wheel

# Build the package
python -m pip wheel -w dist .
```

After building, you'll find two files in the `dist/` directory:
- `osint_toolkit-0.1.0.tar.gz` - Source distribution
- `osint_toolkit-0.1.0-py3-none-any.whl` - Wheel (binary distribution)

## Step 5: Testing Your Package Locally

Before publishing, test that your package installs and works correctly:

```bash
# Create a new virtual environment for testing
python -m venv test-env
source test-env/bin/activate  # On Windows: test-env\Scripts\activate

# Install your package from the wheel
pip install dist/osint_toolkit-0.1.0-py3-none-any.whl

# Try importing and using it
python -c "import osint_toolkit; print(osint_toolkit.__version__)"

# Test the command-line interface
osint-search "test query" --limit 10
```

## Step 6: Publishing to PyPI

### Setting Up PyPI Accounts

To publish packages, you need accounts on:

1. **PyPI** (https://pypi.org) - The main Python Package Index
2. **TestPyPI** (https://test.pypi.org) - A sandbox for testing package uploads

Register on both sites and enable two-factor authentication for security.

### Creating API Tokens

Instead of using your password, create API tokens:

1. Log in to PyPI/TestPyPI
2. Go to Account Settings → API tokens
3. Create a new token with "Upload" scope
4. Save the token securely - it won't be shown again

### Creating a .pypirc File

Create a `~/.pypirc` file to store your repository configurations:

```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
username = __token__
password = pypi-AgEIcHlwaS5vcmcCJDYzNmM3MmM5LWZmZjMtNGFkYS1hZWVmLTkyZGY1MzgxN...

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-AgEIcHlwaS5vcmcCJDYzNmM3MmM5LWZmZjMtNGFkYS1hZWVmLTkyZGY1MzgxN...
```

Make sure this file is only readable by your user:
```bash
chmod 600 ~/.pypirc
```

### Installing Twine

Twine is the recommended tool for uploading packages to PyPI:

```bash
pip install twine
```

### Uploading to TestPyPI First

Always test your package on TestPyPI before publishing to the main PyPI:

```bash
# Upload to TestPyPI
twine upload --repository testpypi dist/*

# Test installation from TestPyPI
pip install --index-url https://test.pypi.org/simple/ osint-toolkit
```

### Uploading to PyPI

Once you've verified everything works on TestPyPI, upload to the real PyPI:

```bash
twine upload dist/*
```

Congratulations! Your package is now available on PyPI, and users can install it with:

```bash
pip install osint-toolkit
```

## OSINT-Specific Package Considerations

When publishing OSINT tools, consider these special considerations:

### 1. Privacy and Security

- Don't include API keys or credentials in your package
- Use environment variables or configuration files for sensitive settings
- Document security implications clearly

### 2. Rate Limiting and Terms of Service

- Include built-in rate limiting for API calls
- Document applicable terms of service for data sources
- Make it easy for users to configure appropriate limits

### 3. Documentation for OSINT Users

Include documentation that covers:
- Ethical and legal considerations
- Data source attribution requirements
- Privacy implications of collected data
- Recommended operational security practices

### 4. API Versioning and Stability

- Document which API versions your tool works with
- Consider using extras for different API versions:
  ```toml
  [project.optional-dependencies]
  twitter-v1 = ["tweepy<4.0.0"]
  twitter-v2 = ["tweepy>=4.0.0"]
  ```

### 5. Attribution and Licensing

- Choose an appropriate license (MIT, Apache 2.0, etc.)
- Include attribution requirements in documentation
- Consider how your tool might be used by others

## Maintenance and Updates

Once your package is published, you'll need to maintain it:

### 1. Version Updates

When releasing new versions:
1. Update the version number in `pyproject.toml` and `__init__.py`
2. Create a new Git tag matching the version
3. Build new distribution packages
4. Upload to PyPI

### 2. Handling Deprecations

When APIs change or features are deprecated:
1. Document deprecations with clear migration paths
2. Use deprecation warnings:
   ```python
   import warnings

   def old_function():
       warnings.warn(
           "old_function() is deprecated and will be removed in version 2.0.0. "
           "Use new_function() instead.",
           DeprecationWarning,
           stacklevel=2
       )
       # Function implementation
   ```

### 3. README Badges

Add badges to your README to show package status:

```markdown
# OSINT Toolkit

[![PyPI version](https://img.shields.io/pypi/v/osint-toolkit.svg)](https://pypi.org/project/osint-toolkit/)
[![Python versions](https://img.shields.io/pypi/pyversions/osint-toolkit.svg)](https://pypi.org/project/osint-toolkit/)
[![License](https://img.shields.io/pypi/l/osint-toolkit.svg)](https://github.com/yourusername/osint-toolkit/blob/main/LICENSE)
```

## Best Practices for OSINT Packages

1. **Keep the API simple** - Prioritize ease of use for investigative workflows
2. **Document everything** - OSINT tools often have complex requirements and interactions
3. **Handle errors gracefully** - External APIs and data sources can be unreliable
4. **Provide examples** - Include realistic usage examples in documentation
5. **Support export formats** - Make it easy to get data out (CSV, JSON, etc.)
6. **Version carefully** - Follow semantic versioning strictly

## Next Steps

Now that you know how to package and publish your OSINT tools to PyPI, the next section will cover how to create comprehensive documentation using MkDocs. This will allow you to provide detailed guides, reference material, and examples to help others use your tools effectively.
