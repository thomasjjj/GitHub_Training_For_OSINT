# 7.6 Writing Minimal Tests with `pytest`

Testing is crucial for OSINT tools, especially those that interact with external APIs or parse complex data. This lesson introduces the basics of testing with `pytest` and shows how to write minimal yet effective tests for your OSINT scripts.

## Why Test OSINT Scripts?

OSINT tools face unique testing challenges:

1. **External dependencies** - APIs change, websites get updated
2. **Complex data parsing** - Extracting structured data from messy sources
3. **Network issues** - Connectivity problems, rate limiting, IP blocking
4. **Credential management** - Testing with API keys securely
5. **Reproducibility** - Ensuring consistent results

Even minimal testing can prevent major issues and save hours of debugging.

## Benefits of Testing for OSINT Work

Well-tested OSINT tools provide:

1. **Reliability** - Scripts continue working despite external changes
2. **Confidence** - Know your tool produces accurate results
3. **Maintainability** - Easier to update or extend functionality
4. **Documentation** - Tests demonstrate how components should work
5. **Validation** - Verify data processing logic is correct

## Introduction to pytest

[pytest](https://docs.pytest.org/) is a testing framework that makes it easy to write simple tests while also supporting complex functional testing:

- **Simple syntax** - No boilerplate required
- **Powerful fixtures** - Reusable test components
- **Extensive plugins** - For every testing need
- **Detailed reports** - Clear failure explanations
- **Autodiscovery** - Automatically finds test files

## Setting Up pytest

### Installation

Install pytest in your virtual environment:

```bash
pip install pytest pytest-cov
```

Add it to your development dependencies:

```toml
[project.optional-dependencies]
dev = [
    "pytest>=7.3.1",
    "pytest-cov>=4.1.0",
    # other dev dependencies
]
```

### Test Directory Structure

Follow the structure we set up in Lesson 7.1:

```
project_name/
├── src/
│   └── package_name/
│       ├── __init__.py
│       └── module.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py              # Shared fixtures
│   ├── test_module.py           # Tests for module.py
│   └── data/                    # Test data files
│       └── sample_response.json
```

## Writing Your First Tests

Let's start with a simple example. Assume we have an OSINT utility function:

```python
# src/osint_toolkit/utils/data_cleaner.py
def normalize_username(username):
    """
    Normalize social media usernames by removing '@' prefix and lowercasing.
    
    Args:
        username (str): Raw username
        
    Returns:
        str: Normalized username
    """
    if not username:
        return ""
    
    # Remove @ prefix and whitespace, convert to lowercase
    return username.strip().lstrip('@').lower()
```

Here's a minimal test for this function:

```python
# tests/test_data_cleaner.py
from osint_toolkit.utils.data_cleaner import normalize_username

def test_normalize_username():
    # Test basic normalization
    assert normalize_username('@UserName') == 'username'
    
    # Test without @ symbol
    assert normalize_username('UserName') == 'username'
    
    # Test with whitespace
    assert normalize_username('  @UserName  ') == 'username'
    
    # Test empty string
    assert normalize_username('') == ''
    
    # Test None value
    assert normalize_username(None) == ''
```

## Running Tests

To run the tests:

```bash
# Run all tests
pytest

# Run a specific test file
pytest tests/test_data_cleaner.py

# Run a specific test function
pytest tests/test_data_cleaner.py::test_normalize_username
```

## Testing API Clients

OSINT tools often interact with external APIs. Here's how to test an API client:

### The API Client

```python
# src/osint_toolkit/clients/twitter_client.py
import requests

class TwitterClient:
    def __init__(self, api_key, api_secret):
        self.api_key = api_key
        self.api_secret = api_secret
        self.base_url = "https://api.twitter.com/2"
    
    def search_tweets(self, query, max_results=10):
        """Search for tweets containing the query string."""
        url = f"{self.base_url}/tweets/search/recent"
        headers = self._get_auth_headers()
        
        params = {
            "query": query,
            "max_results": max_results,
            "tweet.fields": "created_at,author_id,public_metrics"
        }
        
        response = requests.get(url, headers=headers, params=params)
        response.raise_for_status()
        
        return response.json()
    
    def _get_auth_headers(self):
        # Implementation of authentication...
        return {"Authorization": f"Bearer {self.token}"}
```

### Testing Strategy for API Clients

For API clients, we want to test:
1. **Request formatting** - Correct URLs, headers, parameters
2. **Response handling** - Proper parsing of API responses
3. **Error handling** - Graceful handling of API errors

But we don't want to:
1. **Make actual API calls** - Slow, unreliable, consumes quota
2. **Expose credentials** - Keep API keys out of tests
3. **Depend on external state** - Tests should be deterministic

## Mocking External Services

The solution is to use "mocking" to simulate API responses:

```python
# tests/test_twitter_client.py
import pytest
from unittest.mock import patch, MagicMock
from osint_toolkit.clients.twitter_client import TwitterClient

@pytest.fixture
def twitter_client():
    """Create a TwitterClient with dummy credentials."""
    return TwitterClient(api_key="fake_key", api_secret="fake_secret")

def test_search_tweets(twitter_client):
    # Create a mock response
    mock_response = MagicMock()
    mock_response.json.return_value = {
        "data": [
            {
                "id": "1234567890",
                "text": "This is a test tweet",
                "created_at": "2023-05-20T12:00:00Z",
                "author_id": "987654321"
            }
        ],
        "meta": {
            "result_count": 1
        }
    }
    mock_response.raise_for_status = MagicMock()
    
    # Patch the requests.get method to return our mock response
    with patch('requests.get', return_value=mock_response) as mock_get:
        # Call the method being tested
        result = twitter_client.search_tweets("test query")
        
        # Assert that requests.get was called with the right parameters
        mock_get.assert_called_once()
        args, kwargs = mock_get.call_args
        
        # Check the URL
        assert kwargs['url'] == "https://api.twitter.com/2/tweets/search/recent"
        
        # Check the params
        assert kwargs['params']['query'] == "test query"
        assert kwargs['params']['max_results'] == 10
        
        # Check that we got the expected result
        assert 'data' in result
        assert len(result['data']) == 1
        assert result['data'][0]['text'] == "This is a test tweet"
```

## Using Test Fixtures with Real API Responses

For more realistic tests, you can use saved API responses:

### Sample Response File

```json
// tests/data/sample_twitter_response.json
{
  "data": [
    {
      "id": "1234567890",
      "text": "This is a test tweet",
      "created_at": "2023-05-20T12:00:00Z",
      "author_id": "987654321",
      "public_metrics": {
        "retweet_count": 5,
        "reply_count": 2,
        "like_count": 10,
        "quote_count": 1
      }
    }
  ],
  "meta": {
    "result_count": 1
  }
}
```

### Test Using the Sample Response

```python
import json
import os
import pytest
from unittest.mock import patch, MagicMock
from osint_toolkit.clients.twitter_client import TwitterClient

@pytest.fixture
def sample_response():
    """Load sample API response from file."""
    current_dir = os.path.dirname(os.path.abspath(__file__))
    file_path = os.path.join(current_dir, 'data', 'sample_twitter_response.json')
    
    with open(file_path, 'r') as f:
        return json.load(f)

def test_search_tweets_with_sample(twitter_client, sample_response):
    # Create a mock response using the sample data
    mock_response = MagicMock()
    mock_response.json.return_value = sample_response
    mock_response.raise_for_status = MagicMock()
    
    with patch('requests.get', return_value=mock_response):
        result = twitter_client.search_tweets("test query")
        
        # Verify the result matches our sample
        assert result == sample_response
        
        # Test processing of the response
        tweet = result['data'][0]
        assert tweet['public_metrics']['like_count'] == 10
```

## Testing Data Parsers

OSINT tools often parse complex data from websites or APIs. Here's how to test a parser:

### HTML Parser Module

```python
# src/osint_toolkit/parsers/profile_parser.py
from bs4 import BeautifulSoup

def extract_profile_links(html_content):
    """
    Extract social media profile links from HTML content.
    
    Args:
        html_content (str): HTML content to parse
        
    Returns:
        dict: Dictionary of platform names to profile URLs
    """
    soup = BeautifulSoup(html_content, 'html.parser')
    results = {}
    
    # Find social media links
    social_patterns = {
        'twitter': ['twitter.com/', 't.co/'],
        'linkedin': ['linkedin.com/in/', 'linkedin.com/company/'],
        'facebook': ['facebook.com/', 'fb.com/'],
        'instagram': ['instagram.com/', 'instagr.am/'],
        'github': ['github.com/']
    }
    
    # Find all links
    links = soup.find_all('a', href=True)
    
    for link in links:
        href = link['href'].lower()
        for platform, patterns in social_patterns.items():
            if any(pattern in href for pattern in patterns):
                results[platform] = href
                break
    
    return results
```

### Testing the Parser

```python
# tests/test_profile_parser.py
from osint_toolkit.parsers.profile_parser import extract_profile_links

def test_extract_profile_links():
    # Sample HTML content
    html = """
    <html>
    <body>
        <div class="contact-info">
            <a href="https://twitter.com/johndoe">Twitter</a>
            <a href="https://linkedin.com/in/johndoe">LinkedIn</a>
            <a href="https://github.com/johndoe">GitHub</a>
            <a href="https://example.com">Website</a>
        </div>
    </body>
    </html>
    """
    
    # Extract links
    profile_links = extract_profile_links(html)
    
    # Check that we found the expected platforms
    assert 'twitter' in profile_links
    assert 'linkedin' in profile_links
    assert 'github' in profile_links
    assert 'facebook' not in profile_links
    
    # Check the correct URLs were extracted
    assert profile_links['twitter'] == 'https://twitter.com/johndoe'
    assert profile_links['linkedin'] == 'https://linkedin.com/in/johndoe'
    assert profile_links['github'] == 'https://github.com/johndoe'
```

## Testing Error Handling

It's important to test how your code handles errors:

```python
# tests/test_twitter_client.py
def test_search_tweets_handles_http_error(twitter_client):
    # Create a mock response that raises an HTTPError
    mock_response = MagicMock()
    mock_response.raise_for_status.side_effect = requests.exceptions.HTTPError(
        "429 Client Error: Too Many Requests"
    )
    
    with patch('requests.get', return_value=mock_response):
        # Check that the error is propagated
        with pytest.raises(requests.exceptions.HTTPError):
            twitter_client.search_tweets("test query")
```

## Fixture Patterns for OSINT Tests

Fixtures are a powerful pytest feature for setting up test dependencies. Here are some useful fixtures for OSINT testing:

### Mock Web Response Fixture

```python
# tests/conftest.py
import pytest
from unittest.mock import MagicMock

@pytest.fixture
def mock_response():
    """Create a mock requests.Response object."""
    response = MagicMock()
    response.raise_for_status = MagicMock()
    return response

@pytest.fixture
def mock_html_response(mock_response):
    """Mock response with HTML content."""
    mock_response.text = """
    <html>
    <body>
        <h1>Test Page</h1>
        <div class="content">Test Content</div>
    </body>
    </html>
    """
    mock_response.headers = {'Content-Type': 'text/html'}
    return mock_response
```

### HTTP Test Server Fixture

For more complex tests, setting up a local HTTP server can be helpful:

```python
# tests/conftest.py
import pytest
from http.server import HTTPServer, BaseHTTPRequestHandler
import threading
import socket

class MockHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/api/data':
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.end_headers()
            self.wfile.write(b'{"result": "success"}')
        else:
            self.send_response(404)
            self.end_headers()
    
    # Silence server logs
    def log_message(self, format, *args):
        pass

@pytest.fixture
def http_server():
    """Start a local HTTP server for testing."""
    # Find an available port
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.bind(('localhost', 0))
        port = s.getsockname()[1]
    
    server = HTTPServer(('localhost', port), MockHandler)
    thread = threading.Thread(target=server.serve_forever)
    thread.daemon = True
    thread.start()
    
    yield f"http://localhost:{port}"
    
    server.shutdown()
    server.server_close()
```

## Measuring Test Coverage

Code coverage shows how much of your code is tested. To measure coverage:

```bash
# Run tests with coverage
pytest --cov=osint_toolkit tests/

# Generate an HTML report
pytest --cov=osint_toolkit --cov-report=html tests/
```

### Minimal Coverage Goals for OSINT Projects

For OSINT tools, focus your testing efforts on:

1. **Core data processing logic** - Aim for high coverage
2. **API client request formatting** - Test parameter construction
3. **Error handling paths** - Verify graceful failure
4. **Parser edge cases** - Test with malformed or unexpected input

Don't worry about 100% coverage—focus on the critical components.

## Test-Driven Development for OSINT Tools

Test-driven development (TDD) can be particularly valuable for OSINT tools:

1. **Write a test** for a feature you want to add
2. **Run the test** and watch it fail
3. **Write the code** to make the test pass
4. **Refactor** your code while keeping tests passing

This approach works well when:
- Building parsers for specific data formats
- Developing API clients for well-documented services
- Implementing data transformation functions

## Next Steps

Now that you know how to write effective tests for your OSINT tools, the next step is to automate testing using Continuous Integration. In the next section, we'll learn how to set up GitHub Actions to automatically run tests, check code style, and ensure your project stays in good shape as it evolves.
