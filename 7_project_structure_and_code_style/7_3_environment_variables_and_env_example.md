# 7.3 Environment Variables & `.env.example`

When developing OSINT tools, you'll need to handle sensitive credentials like API keys, tokens, and passwords. Environment variables provide a secure way to manage these secrets without hardcoding them in your source code. This lesson covers how to use the `python-dotenv` package to load credentials from `.env` files while keeping them out of your Git repository.

## Why Use Environment Variables?

Hardcoding sensitive values directly in your code creates several security risks:

1. **Accidental exposure** - Secrets might be committed to public repositories
2. **Credential sharing** - Everyone with code access gets the same credentials
3. **Limited flexibility** - Changing credentials requires modifying code
4. **Deployment complexity** - Each environment needs different credentials

Environment variables solve these problems by:

- Keeping secrets separate from code
- Allowing different credentials per environment
- Supporting easy updates without code changes
- Preventing accidental commits of sensitive data

## Understanding Environment Variables

Environment variables are key-value pairs that exist outside your application but are accessible within it. They're commonly used for:

- API keys and tokens
- Database credentials
- Service endpoints with embedded authentication
- Feature flags and environment indicators
- File paths specific to the current system

## Using `python-dotenv` to Manage Environment Variables

The `python-dotenv` package makes it easy to:
1. Store environment variables in a file (`.env`)
2. Load these variables when your application starts
3. Provide default values for missing variables

### Installing `python-dotenv`

```bash
pip install python-dotenv
```

### Creating a `.env` File

A `.env` file contains key-value pairs in a simple format:

```
# API Credentials
TWITTER_API_KEY=your_twitter_api_key_here
TWITTER_API_SECRET=your_twitter_api_secret_here

# Database Configuration
DATABASE_URL=postgresql://username:password@localhost/dbname

# Feature Flags
ENABLE_ADVANCED_SEARCH=true
```

### Loading Environment Variables in Your Code

Here's how to load variables from a `.env` file:

```python
import os
from dotenv import load_dotenv

# Load environment variables from .env file
load_dotenv()

# Access variables
twitter_api_key = os.getenv("TWITTER_API_KEY")
twitter_api_secret = os.getenv("TWITTER_API_SECRET")
database_url = os.getenv("DATABASE_URL")

# Provide default values for optional variables
enable_advanced_search = os.getenv("ENABLE_ADVANCED_SEARCH", "false").lower() == "true"
```

### Where to Place the Loading Code

Typically, you'll load environment variables early in your application:

```python
# In src/your_package/__init__.py
from dotenv import load_dotenv
load_dotenv()

# Or in a dedicated module like src/your_package/environment.py
import os
from dotenv import load_dotenv

load_dotenv()

# Export variables with proper typing
TWITTER_API_KEY = os.getenv("TWITTER_API_KEY", "")
TWITTER_API_SECRET = os.getenv("TWITTER_API_SECRET", "")
ENABLE_ADVANCED_SEARCH = os.getenv("ENABLE_ADVANCED_SEARCH", "false").lower() == "true"
REQUEST_TIMEOUT = int(os.getenv("REQUEST_TIMEOUT", "30"))
```

## Creating a `.env.example` File

Since your actual `.env` file contains sensitive information and should **never** be committed to Git, it's a best practice to provide a template file called `.env.example`:

1. Create a file named `.env.example` in your project root
2. Include all the required environment variables with dummy values
3. Add comments explaining each variable's purpose
4. Commit this template to your repository

Example `.env.example` file:

```
# Twitter API Credentials
# Create an account at https://developer.twitter.com to obtain these
TWITTER_API_KEY=your_twitter_api_key_here
TWITTER_API_SECRET=your_twitter_api_secret_here
TWITTER_ACCESS_TOKEN=your_access_token_here
TWITTER_ACCESS_SECRET=your_access_secret_here

# Proxy Configuration (optional)
# Format: protocol://host:port
HTTP_PROXY=http://proxy.example.com:8080
HTTPS_PROXY=http://proxy.example.com:8080

# Feature Flags
# Set to "true" to enable advanced search capabilities
ENABLE_ADVANCED_SEARCH=false

# Performance Settings
# Maximum number of concurrent requests
MAX_CONCURRENT_REQUESTS=5
# Request timeout in seconds
REQUEST_TIMEOUT=30
```

## Keeping Secrets Out of Git

To ensure sensitive credentials never end up in your Git repository:

1. **Add `.env` to your `.gitignore` file**:
   ```
   # .gitignore
   .env
   .env.local
   .env.*.local
   ```

2. **Never commit actual credentials** - even in examples or documentation

3. **Check your Git history** if you suspect credentials were committed:
   ```bash
   git log -p | grep -A 2 -B 2 "API_KEY"
   ```

4. **Use Git pre-commit hooks** to prevent accidental commits (covered in a later section)

## Environment Variables in Different Environments

Different environments (development, testing, production) often need different credentials:

### Development

- Local `.env` file on each developer's machine
- Usually points to development/sandbox API endpoints
- May include debugging flags enabled

### Testing

- Environment variables set in CI/CD systems
- Often uses test accounts or sanitized data
- May include mocked services for isolation

### Production

- Set in deployment platform (Heroku, AWS, etc.)
- Uses production credentials with appropriate restrictions
- Should be managed by secure secrets management systems

## Accessing Environment Variables in Different Contexts

### Using environment variables in scripts

```python
import os
from dotenv import load_dotenv

load_dotenv()

def get_twitter_client():
    from tweepy import Client
    
    return Client(
        bearer_token=os.getenv("TWITTER_BEARER_TOKEN"),
        consumer_key=os.getenv("TWITTER_API_KEY"),
        consumer_secret=os.getenv("TWITTER_API_SECRET"),
        access_token=os.getenv("TWITTER_ACCESS_TOKEN"),
        access_token_secret=os.getenv("TWITTER_ACCESS_SECRET")
    )
```

### Using environment variables in CI/CD workflows

In GitHub Actions:

```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    env:
      # Set common environment variables
      PYTHONPATH: ${{ github.workspace }}
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
          
      - name: Install dependencies
        run: pip install -e ".[dev]"
        
      - name: Run tests
        env:
          # Set sensitive credentials as secrets
          TWITTER_API_KEY: ${{ secrets.TWITTER_API_KEY }}
          TWITTER_API_SECRET: ${{ secrets.TWITTER_API_SECRET }}
        run: pytest
```

## Validating Required Environment Variables

To ensure your application fails fast if required variables are missing:

```python
import os
from dotenv import load_dotenv

load_dotenv()

# List of required environment variables
REQUIRED_ENV_VARS = [
    "TWITTER_API_KEY",
    "TWITTER_API_SECRET",
    "DATABASE_URL"
]

# Check for missing variables
missing_vars = [var for var in REQUIRED_ENV_VARS if not os.getenv(var)]
if missing_vars:
    raise EnvironmentError(
        f"Required environment variables are missing: {', '.join(missing_vars)}\n"
        f"Please check your .env file or environment settings."
    )
```

## Type Conversion for Environment Variables

Remember that environment variables are always strings. You'll need to convert them to appropriate types:

```python
# Boolean conversion
DEBUG = os.getenv("DEBUG", "false").lower() in ("true", "1", "yes")

# Integer conversion
TIMEOUT = int(os.getenv("TIMEOUT", "30"))

# Float conversion
RATE_LIMIT = float(os.getenv("RATE_LIMIT", "0.5"))

# List conversion (comma-separated)
ALLOWED_DOMAINS = os.getenv("ALLOWED_DOMAINS", "").split(",")
```

## OSINT-Specific Environment Variable Recommendations

For OSINT tools, consider these additional recommendations:

1. **API key rotation** - Include support for multiple API keys to rotate through
2. **Proxy configuration** - Environment variables for proxy settings
3. **User identity protection** - Variables to control attribution reduction
4. **Feature flags** - Enable/disable capabilities based on operational needs
5. **Output controls** - Settings for verbosity and logging

Example:

```
# API Keys with rotation capability
TWITTER_API_KEY_1=key1
TWITTER_API_KEY_2=key2
TWITTER_API_KEY_3=key3

# Proxy settings
USE_TOR_PROXY=true
PROXY_ROTATION_INTERVAL=600

# Identity protection
RANDOMIZE_USER_AGENT=true
OBFUSCATE_REQUEST_FINGERPRINT=true

# Feature flags
ENABLE_SOCIAL_MEDIA_COLLECTION=true
ENABLE_DOMAIN_ENUMERATION=false

# Output controls
LOG_LEVEL=INFO
SAVE_RAW_RESPONSES=false
```

## Best Practices for Environment Variables

1. **Name variables clearly** - Use uppercase and underscores by convention
2. **Group related variables** with common prefixes (e.g., `TWITTER_*`)
3. **Document each variable** in your `.env.example` file
4. **Provide default values** for non-critical variables
5. **Validate at startup** to fail fast if required variables are missing
6. **Never log credentials** - Mask sensitive values in logs and error reports
7. **Rotate credentials regularly** - Design your system to make this easy

## Next Steps

Now that you understand how to manage sensitive credentials using environment variables, the next step is to ensure these sensitive files don't accidentally get committed to your repository. In the next section, we'll explore creating a solid `.gitignore` file to prevent accidental exposure of secrets and exclude unnecessary files from version control.
