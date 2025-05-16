# 7.7 Continuous Integration with GitHub Actions

Continuous Integration (CI) automatically tests your code whenever changes are pushed to your repository. This lesson demonstrates how to set up GitHub Actions to run tests, check code style, and ensure quality in your OSINT projects.

## Why Use Continuous Integration?

CI provides several benefits for OSINT development:

1. **Automatic quality checks** - Tests run on every push and pull request
2. **Early bug detection** - Find issues before they reach production
3. **Consistent environments** - Tests run in clean, reproducible environments
4. **Reduced manual effort** - No need to remember to run tests
5. **Project health visibility** - See build status at a glance

For OSINT tools, CI helps ensure reliability as APIs change and dependencies evolve.

## Introduction to GitHub Actions

GitHub Actions is a CI/CD platform integrated directly into GitHub repositories:

- **Workflows** - YAML files defining when and how to run automated jobs
- **Jobs** - Sets of steps that execute on the same runner
- **Steps** - Individual tasks that run commands or actions
- **Actions** - Reusable units of code for common tasks
- **Runners** - Servers that run your workflows

## Creating Your First GitHub Actions Workflow

To set up a basic CI workflow:

1. Create a `.github/workflows` directory in your repository
2. Add a YAML file, such as `ci.yml`, with your workflow configuration

Here's a simple example for an OSINT Python project:

```yaml
name: Python Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'
        
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -e ".[dev]"
        
    - name: Run tests
      run: pytest
```

This workflow will:
1. Run whenever code is pushed to the main branch or a pull request is opened
2. Set up an Ubuntu environment with Python 3.10
3. Install your package and its development dependencies
4. Run your pytest tests

## Adding Code Style Checks

Extend your workflow to include code style checks using the tools we set up in Lesson 7.5:

```yaml
name: Python Tests and Style Checks

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'
        
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -e ".[dev]"
        
    - name: Check code style
      run: |
        black --check src tests
        isort --check src tests
        flake8 src tests
        
    - name: Run tests
      run: pytest
```

## Adding Test Coverage Reports

To track test coverage in your CI workflow:

```yaml
- name: Run tests with coverage
  run: |
    pytest --cov=src --cov-report=xml
    
- name: Upload coverage report
  uses: codecov/codecov-action@v3
  with:
    file: ./coverage.xml
    fail_ci_if_error: false
```

This requires setting up a free account on [Codecov](https://codecov.io/) or a similar service.

## Matrix Testing for Multiple Python Versions

To ensure your OSINT tool works across different Python versions:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.8', '3.9', '3.10', '3.11']
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    
    # ... rest of your steps
```

This will run your tests on Python 3.8, 3.9, 3.10, and 3.11 in parallel.

## Handling Dependencies with Caching

Speed up your CI runs by caching dependencies:

```yaml
- name: Set up Python
  uses: actions/setup-python@v4
  with:
    python-version: '3.10'
    
- name: Cache pip packages
  uses: actions/cache@v3
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('**/pyproject.toml') }}
    restore-keys: |
      ${{ runner.os }}-pip-
      
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install -e ".[dev]"
```

## Managing Secrets for API Testing

For OSINT tools that need API access during tests:

1. Add your API keys as repository secrets in GitHub:
   - Go to repository settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Add your secret (e.g., `TWITTER_API_KEY`)

2. Access the secrets in your workflow:

```yaml
- name: Run API tests
  env:
    TWITTER_API_KEY: ${{ secrets.TWITTER_API_KEY }}
    TWITTER_API_SECRET: ${{ secrets.TWITTER_API_SECRET }}
  run: pytest tests/test_twitter_api.py
```

## Skipping Integration Tests in CI

For tests that shouldn't run in CI (e.g., those that access real APIs):

```python
# tests/test_twitter_live.py
import os
import pytest

# Skip this test if the SKIP_INTEGRATION_TESTS environment variable is set
@pytest.mark.skipif(
    os.environ.get("SKIP_INTEGRATION_TESTS") == "true",
    reason="Skipping integration tests in CI"
)
def test_live_twitter_api():
    # Test that would access the real Twitter API
    pass
```

Then in your GitHub Actions workflow:

```yaml
- name: Run unit tests
  env:
    SKIP_INTEGRATION_TESTS: "true"
  run: pytest
```

## Creating a Complete CI Workflow

Here's a comprehensive workflow for OSINT projects:

```yaml
name: OSINT Toolkit CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 0'  # Run weekly to catch API changes

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.9', '3.10']
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
        
    - name: Cache pip packages
      uses: actions/cache@v3
      with:
        path: ~/.cache/pip
        key: ${{ runner.os }}-pip-${{ hashFiles('**/pyproject.toml') }}
        restore-keys: |
          ${{ runner.os }}-pip-
          
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -e ".[dev]"
        
    - name: Check code style
      run: |
        black --check src tests
        isort --check src tests
        flake8 src tests
        
    - name: Run security checks
      run: |
        pip install safety
        safety check
        
    - name: Run tests with coverage
      env:
        SKIP_INTEGRATION_TESTS: "true"
      run: |
        pytest --cov=src --cov-report=xml
        
    - name: Upload coverage report
      uses: codecov/codecov-action@v3
      with:
        file: ./coverage.xml
        fail_ci_if_error: false
        
  integration:
    needs: test
    runs-on: ubuntu-latest
    if: github.event_name == 'schedule' || github.event_name == 'workflow_dispatch'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'
        
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -e ".[dev]"
        
    - name: Run integration tests
      env:
        TWITTER_API_KEY: ${{ secrets.TWITTER_API_KEY }}
        TWITTER_API_SECRET: ${{ secrets.TWITTER_API_SECRET }}
      run: |
        pytest tests/integration/
```

This workflow:
1. Runs tests on multiple Python versions
2. Checks code style and performs security scans
3. Runs a separate job for integration tests only on a schedule or manual trigger
4. Uses secrets for API access in integration tests

## Adding Workflow Status Badges

Add a status badge to your README.md to show your project's CI status:

```markdown
# My OSINT Tool

![CI Status](https://github.com/username/repo-name/actions/workflows/ci.yml/badge.svg)

A tool for collecting and analyzing OSINT data.
```

## Common Workflow Problems and Solutions

### Tests Pass Locally But Fail in CI

**Problem**: Your tests work on your machine but fail in GitHub Actions.

**Solutions**:
- Check for environment differences (Python version, OS)
- Look for hardcoded paths or environment-specific code
- Add more debug output to tests
- Try running tests in a clean virtual environment locally

### Secret Management Issues

**Problem**: Tests requiring API keys fail in CI.

**Solutions**:
- Verify secrets are correctly added to repository settings
- Check that secret names match in workflow and code
- Consider using mock objects for API calls in CI

### Workflow Taking Too Long

**Problem**: CI runs are slow and delay feedback.

**Solutions**:
- Use caching for dependencies
- Run only a subset of tests in PR checks
- Split your workflow into parallel jobs
- Optimize test execution order to fail fast

## OSINT-Specific CI Considerations

For OSINT tools in particular:

1. **API rate limits** - Be mindful of how often CI runs tests against real APIs
2. **Credential rotation** - Regularly rotate any API keys used in CI
3. **Test data privacy** - Don't include sensitive test data in your repository
4. **External dependency monitoring** - Schedule weekly tests to catch API changes
5. **Documentation builds** - Add a step to build and validate documentation

## Next Steps

With continuous integration set up, your OSINT project now has automated quality checks. This completes the basics of project structure and code style. From here, you can move on to more advanced topics:

- Building command-line interfaces for your tools
- Packaging your code for distribution
- Setting up comprehensive documentation
- Implementing advanced testing for complex scenarios

These topics are covered in the "Leveling-Up the Toolkit" section of the course.
