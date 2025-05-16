# 7.5 Code Style & Pre-Commit Hooks

Maintaining consistent code style across a project is essential for readability and collaboration. This lesson introduces code formatting tools like Black, isort, and flake8, and shows how to enforce style standards automatically using pre-commit hooks.

## Why Code Style Matters

A consistent coding style offers several advantages for OSINT projects:

1. **Readability** - Consistent code is easier to understand and maintain
2. **Collaboration** - Team members can work together more efficiently
3. **Reduced conflicts** - Fewer merge conflicts due to formatting differences
4. **Focus on substance** - Spend time on functionality, not formatting debates
5. **Onboarding** - New contributors learn the codebase faster

## Essential Python Code Formatting Tools

These tools automate code formatting and quality checks:

### 1. Black - The Uncompromising Code Formatter

[Black](https://black.readthedocs.io/) automatically formats your code to a consistent style:

- Takes the decision-making out of formatting
- Has few configuration options (intentionally)
- Produces deterministic output
- Has become the de facto standard in many Python communities

### 2. isort - Import Statement Organizer

[isort](https://pycqa.github.io/isort/) sorts your imports alphabetically and separates them into sections:

- Standard library imports
- Third-party imports
- Local application imports
- Makes imports consistent and organized
- Works well with Black

### 3. flake8 - Style Guide Enforcer

[flake8](https://flake8.pycqa.org/) checks your code against style guides and common mistakes:

- Enforces PEP 8 style guidelines
- Catches logical errors
- Identifies complexity issues
- Customizable to your project's needs

## Setting Up the Tools

### Installation

Install these tools in your development environment:

```bash
pip install black isort flake8 pre-commit
```

Add them to your project's development dependencies in `pyproject.toml` or `setup.py`:

```toml
[project.optional-dependencies]
dev = [
    "black>=23.1.0",
    "isort>=5.12.0",
    "flake8>=6.0.0",
    "pre-commit>=3.1.1",
]
```

### Configuration Files

Create configuration files in your project root:

#### `.flake8` (for flake8)

```ini
[flake8]
max-line-length = 88
extend-ignore = E203
exclude = .git,__pycache__,build,dist,.venv,venv
```

#### `pyproject.toml` (for Black and isort)

```toml
[tool.black]
line-length = 88
target-version = ["py39"]
include = '\.pyi?$'
exclude = '''
/(
    \.git
  | \.venv
  | venv
  | _build
  | build
  | dist
)/
'''

[tool.isort]
profile = "black"
line_length = 88
```

> Note: Configuring isort with `profile = "black"` ensures it works harmoniously with Black.

## Using the Tools Manually

Before setting up automation, you can run these tools manually:

### Black

Format a single file:
```bash
black path/to/file.py
```

Format all Python files in a directory:
```bash
black src/
```

Check what would change without making changes:
```bash
black --check src/
```

### isort

Sort imports in a file:
```bash
isort path/to/file.py
```

Sort imports in a directory:
```bash
isort src/
```

Check what would change:
```bash
isort --check src/
```

### flake8

Check a file for style issues:
```bash
flake8 path/to/file.py
```

Check an entire directory:
```bash
flake8 src/
```

## Introduction to Pre-Commit Hooks

[Pre-commit](https://pre-commit.com/) is a framework for managing and maintaining Git hooks that run before each commit. These hooks can automatically:

1. Format your code
2. Check for style issues
3. Run tests
4. Validate syntax
5. Check for sensitive information

Pre-commit hooks prevent you from committing code that doesn't meet your project's standards.

## Setting Up Pre-Commit

### 1. Create a `.pre-commit-config.yaml` File

```yaml
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
    -   id: check-toml
    -   id: detect-private-key

-   repo: https://github.com/psf/black
    rev: 23.1.0
    hooks:
    -   id: black

-   repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
    -   id: isort

-   repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
    -   id: flake8
```

### 2. Install the Pre-Commit Hooks

```bash
pre-commit install
```

This adds the hooks to your local `.git/hooks/pre-commit` file.

### 3. Optional: Run Against All Files

To format all files in your repository (not just the ones you're committing):

```bash
pre-commit run --all-files
```

## How Pre-Commit Works

1. When you run `git commit`, pre-commit hooks execute first
2. Each hook runs in order against staged files
3. If any hook fails, the commit is aborted
4. You fix the issues and try again
5. Once all hooks pass, the commit proceeds

## Creating an OSINT-Specific Pre-Commit Config

For OSINT projects, consider these additional hooks:

```yaml
repos:
# ... [previous hooks]

# Check for secrets
-   repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
    -   id: detect-secrets

# Additional security checks
-   repo: https://github.com/Lucas-C/pre-commit-hooks-safety
    rev: v1.3.1
    hooks:
    -   id: python-safety-dependencies-check

# Check for broken links in documentation
-   repo: https://github.com/tcort/markdown-link-check
    rev: v3.10.3
    hooks:
    -   id: markdown-link-check
        args: [--config=.markdown-link-check.json]
```

## OSINT Security Pre-Commit Hooks

For OSINT work, security is crucial. These hooks help prevent leaking sensitive information:

### 1. detect-secrets

Finds secrets, tokens, and credentials accidentally committed:

```yaml
-   repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
    -   id: detect-secrets
        exclude: .*/tests/.*|.*\.example
```

### 2. Custom Sensitive Information Check

Create a custom hook to check for OSINT-specific sensitive data:

```yaml
-   repo: local
    hooks:
    -   id: check-osint-sensitive-data
        name: Check for OSINT sensitive data
        entry: python scripts/check_sensitive_data.py
        language: python
        files: \.(py|json|csv|md)$
```

With a custom script like:

```python
# scripts/check_sensitive_data.py
import re
import sys

PATTERNS = [
    r'target_name\s*=\s*["\'](.+?)["\']',
    r'investigation_id\s*=\s*["\'](.+?)["\']',
    # Add other patterns specific to your OSINT work
]

def check_file(filename):
    with open(filename, 'r', encoding='utf-8', errors='ignore') as f:
        content = f.read()
        
    for pattern in PATTERNS:
        matches = re.findall(pattern, content)
        if matches:
            print(f"Found sensitive data in {filename}: {pattern}")
            return 1
    
    return 0

if __name__ == "__main__":
    exit_code = 0
    for filename in sys.argv[1:]:
        exit_code |= check_file(filename)
    sys.exit(exit_code)
```

## Common Pre-Commit Configurations

### Skipping Hooks Temporarily

Sometimes, you need to commit without running hooks (e.g., during urgent fixes):

```bash
git commit -m "Emergency fix" --no-verify
```

> ⚠️ Use this sparingly, as it bypasses your quality checks!

### Configuring Specific Files to Check

Control which files pre-commit processes:

```yaml
-   id: black
    files: ^src/
    exclude: ^tests/fixtures/
```

### Creating Hook Environments

For hooks with dependencies:

```yaml
-   repo: local
    hooks:
    -   id: pylint
        name: pylint
        entry: pylint
        language: python
        types: [python]
        additional_dependencies: [pylint==2.15.10]
```

## IDE Integration

Most IDEs can be configured to run these tools automatically:

### PyCharm

1. **For Black**: Install the "Black Connect" plugin
2. **For isort**: Configure in "Settings → Editor → Code Style → Python → Imports"
3. **For flake8**: Configure in "Settings → Editor → Inspections"

### VS Code

1. Install the "Python" extension
2. Add to your `settings.json`:
   ```json
   {
       "python.formatting.provider": "black",
       "python.formatting.blackArgs": ["--line-length", "88"],
       "editor.formatOnSave": true,
       "python.linting.flake8Enabled": true,
       "python.linting.enabled": true,
       "[python]": {
           "editor.codeActionsOnSave": {
               "source.organizeImports": true
           }
       }
   }
   ```

## Team Adoption Strategy

Getting your team to adopt these tools requires a gradual approach:

1. **Start with documentation** - Add a coding standards document
2. **Implement incrementally** - Begin with one tool (usually Black)
3. **Run in check mode first** - Show changes without enforcing them
4. **Format existing codebase** - Apply to all files before enforcing
5. **Add to CI pipeline** - Check style in pull requests
6. **Finally enforce locally** - Add pre-commit hooks for all team members

## OSINT-Specific Style Guidelines

Consider these additional guidelines for OSINT code:

1. **Security comments** - Mark sensitive sections with `# SECURITY: ...` comments
2. **Attribution** - Include source comments for data processing logic
3. **Error handling** - Always use proper exception handling for external API calls
4. **Request limits** - Enforce rate limiting in code through style checks
5. **Documentation** - Require docstrings for public functions and classes

## Next Steps

Now that you've set up automated code formatting and quality checks, the next step is to implement testing for your OSINT tools. In the next section, we'll explore writing minimal but effective tests with `pytest` to ensure your code works reliably, especially when interacting with external APIs and data sources.
