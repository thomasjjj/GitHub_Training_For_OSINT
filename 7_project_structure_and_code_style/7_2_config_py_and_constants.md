# 7.2 `config.py` and Constants

Centralizing configuration values in a single location makes your OSINT tools more maintainable, flexible, and easier to use. This lesson explains why a dedicated `config.py` file is important and how to implement it effectively in your projects.

## Why Centralize Configuration?

When developing OSINT tools, you'll encounter many values that might need to change:

- API endpoints and version numbers
- Request parameters and limits
- File paths and format specifications
- Default search terms or filters
- Timeouts and retry attempts

Scattering these values throughout your code creates several problems:

1. **Duplication** - The same value may be repeated in multiple places
2. **Hidden dependencies** - It's not clear which components share configuration
3. **Difficult updates** - Changing a value requires finding all occurrences
4. **Unclear intent** - "Magic numbers" lack context about their purpose

A central `config.py` file solves these issues and provides additional benefits.

## Benefits of a Central Configuration Module

### 1. Single Source of Truth

When all configurable values live in one place:
- You only need to update a value once
- There's never confusion about which value is "correct"
- Documentation can focus on one location

### 2. Improved Code Readability

Compare these two approaches:

**Without centralized config:**
```python
# In twitter_collector.py
response = requests.get(
    "https://api.twitter.com/2/tweets/search/recent",
    params={"max_results": 100},
    timeout=30
)

# In twitter_analyzer.py
response = requests.get(
    "https://api.twitter.com/2/tweets/search/recent",
    params={"max_results": 100},
    timeout=30
)
```

**With centralized config:**
```python
# In config.py
TWITTER_API_URL = "https://api.twitter.com/2/tweets/search/recent"
TWITTER_MAX_RESULTS = 100
REQUEST_TIMEOUT = 30

# In twitter_collector.py
from mypackage.config import TWITTER_API_URL, TWITTER_MAX_RESULTS, REQUEST_TIMEOUT

response = requests.get(
    TWITTER_API_URL,
    params={"max_results": TWITTER_MAX_RESULTS},
    timeout=REQUEST_TIMEOUT
)

# In twitter_analyzer.py
from mypackage.config import TWITTER_API_URL, TWITTER_MAX_RESULTS, REQUEST_TIMEOUT

response = requests.get(
    TWITTER_API_URL,
    params={"max_results": TWITTER_MAX_RESULTS},
    timeout=REQUEST_TIMEOUT
)
```

The second approach makes it immediately clear which values are configurable.

### 3. Easier Maintenance

With centralized configuration:
- New team members can quickly see all configurable values
- Updates for API changes are simplified 
- You can provide comments explaining each value's purpose
- Constants with similar purposes are grouped together

### 4. Simplified Testing

Centralized configuration makes testing easier:
- Test fixtures can override config values
- Tests can run with modified settings without changing code
- You can catch issues related to configuration changes

## Creating a Well-Structured `config.py`

Let's look at how to design an effective configuration module for an OSINT tool.

### Basic Structure

Place your `config.py` file within your package:

```
src/
└── osint_toolkit/
    ├── __init__.py
    ├── config.py         # Configuration module
    ├── collectors/
    └── analyzers/
```

### Constants Naming Conventions

Use these naming conventions for configuration constants:

- **ALL_UPPERCASE** for constants
- **Descriptive names** that indicate purpose
- **Namespacing prefixes** for related constants
- **Underscores** to separate words

### Example Content

Here's an example `config.py` for an OSINT toolkit:

```python
"""
Configuration settings for the OSINT toolkit.

This module contains all configurable constants used throughout the package.
"""

# API endpoints
TWITTER_API_BASE_URL = "https://api.twitter.com/2"
TWITTER_SEARCH_ENDPOINT = f"{TWITTER_API_BASE_URL}/tweets/search/recent"
REDDIT_API_BASE_URL = "https://oauth.reddit.com"

# Request parameters
DEFAULT_REQUEST_TIMEOUT = 30  # seconds
DEFAULT_REQUEST_HEADERS = {
    "User-Agent": "OSINT-Toolkit/1.0 (Research Project)"
}
MAX_RETRIES = 3
RETRY_BACKOFF_FACTOR = 0.5

# Rate limiting
TWITTER_RATE_LIMIT_WAIT = 15 * 60  # 15 minutes in seconds
REQUESTS_PER_MINUTE = 60

# Data collection defaults
MAX_RESULTS_PER_PAGE = 100
DEFAULT_RESULT_LIMIT = 1000

# File paths
DEFAULT_DATA_DIR = "data"
DEFAULT_RESULTS_DIR = "results"
DEFAULT_CACHE_DIR = "cache"

# Analysis parameters
TEXT_MIN_WORD_COUNT = 3
NETWORK_MIN_EDGE_WEIGHT = 2
SIMILARITY_THRESHOLD = 0.75
```

### Organizing by Categories

For larger projects, group related constants with comments:

```python
#
# API Configuration
#
TWITTER_API_BASE_URL = "https://api.twitter.com/2"
REDDIT_API_BASE_URL = "https://oauth.reddit.com"

#
# Request Parameters
#
DEFAULT_REQUEST_TIMEOUT = 30
MAX_RETRIES = 3
```

## Advanced Configuration Techniques

As your OSINT tools grow more complex, you may need more sophisticated configuration approaches.

### Environment-Specific Configuration

To handle different environments (development, testing, production):

```python
import os

# Determine environment
ENV = os.getenv("OSINT_ENVIRONMENT", "development")

# Base configuration
DEFAULT_REQUEST_TIMEOUT = 30
MAX_RESULTS_PER_PAGE = 100

# Environment-specific overrides
if ENV == "testing":
    DEFAULT_REQUEST_TIMEOUT = 5
    MAX_RESULTS_PER_PAGE = 10
elif ENV == "production":
    DEFAULT_REQUEST_TIMEOUT = 60
    MAX_RESULTS_PER_PAGE = 500
```

### Configuration Classes

For complex applications, a class-based approach can provide better organization:

```python
class TwitterConfig:
    API_BASE_URL = "https://api.twitter.com/2"
    SEARCH_ENDPOINT = f"{API_BASE_URL}/tweets/search/recent"
    MAX_RESULTS = 100
    RATE_LIMIT_WAIT = 15 * 60


class RedditConfig:
    API_BASE_URL = "https://oauth.reddit.com"
    SEARCH_ENDPOINT = f"{API_BASE_URL}/search"
    MAX_RESULTS = 100
    RATE_LIMIT_WAIT = 60


class Config:
    DEBUG = False
    REQUEST_TIMEOUT = 30
    DATA_DIR = "data"
    
    # Nested configurations
    TWITTER = TwitterConfig
    REDDIT = RedditConfig


# Usage:
# from mypackage.config import Config
# response = requests.get(Config.TWITTER.SEARCH_ENDPOINT, timeout=Config.REQUEST_TIMEOUT)
```

### Configuration from Files

For user-configurable settings, consider loading from a configuration file:

```python
import yaml
import os
from pathlib import Path

# Default configuration
DEFAULT_CONFIG = {
    "api": {
        "twitter": {
            "base_url": "https://api.twitter.com/2",
            "max_results": 100
        }
    },
    "request": {
        "timeout": 30,
        "max_retries": 3
    }
}

# Load custom configuration from file
config_path = Path(os.getenv("CONFIG_PATH", "config.yaml"))
CONFIG = DEFAULT_CONFIG.copy()

if config_path.exists():
    with open(config_path) as f:
        custom_config = yaml.safe_load(f)
        # Deep update the configuration
        # (simplified - use a proper deep update function in practice)
        for section, values in custom_config.items():
            if section in CONFIG:
                CONFIG[section].update(values)
            else:
                CONFIG[section] = values

# Usage:
# TWITTER_API_URL = CONFIG["api"]["twitter"]["base_url"]
# REQUEST_TIMEOUT = CONFIG["request"]["timeout"]
```

## The Difference Between Constants and Secrets

It's important to distinguish between configuration constants and sensitive credentials:

- **Constants** (in `config.py`):
  - API URLs and endpoints
  - Default parameters
  - Timeouts and limits
  - Non-sensitive defaults

- **Secrets** (in environment variables or `.env` file):
  - API keys and tokens
  - Passwords and passphrases
  - Account identifiers
  - Private URLs or endpoints

We'll cover managing secrets in the next lesson on environment variables.

## OSINT-Specific Considerations

For OSINT tools, consider these additional recommendations:

1. **User agent rotation** - Define multiple user agents for web scraping
2. **Multiple APIs** - Store fallback API endpoints when primary ones fail
3. **Tool signatures** - Be mindful of identifiable fingerprints in your configuration
4. **Proxy configuration** - Include settings for proxies or Tor usage
5. **Collection controls** - Set reasonable defaults to avoid aggressive scraping

Example:

```python
# User agent rotation
USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36"
]

# API fallbacks
SEARCH_APIS = [
    "https://primary-api.example.com/search",
    "https://backup-api.example.com/search"
]

# Collection controls
MIN_REQUEST_INTERVAL = 2.0  # seconds between requests
MAX_PAGES_PER_SESSION = 25  # maximum pagination depth
RANDOMIZE_INTERVALS = True  # add random delay between requests
```

## Best Practices

Follow these guidelines when working with a `config.py` file:

1. **Document every constant** with a comment explaining its purpose
2. **Group related constants** for better organization
3. **Use descriptive names** to make the purpose clear
4. **Avoid hard-coding values** elsewhere in your codebase
5. **Update comments** when values change
6. **Provide default values** for all configurable options
7. **Separate sensitive information** into environment variables

## Next Steps

Now that you understand how to centralize configuration constants, the next step is to learn about managing sensitive credentials through environment variables. In the next section, we'll explore using `.env` files with the `python-dotenv` package to securely handle API keys and other secrets.
