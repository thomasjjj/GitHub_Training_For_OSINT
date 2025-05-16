# 4.3 Basic Troubleshooting

Even the most experienced programmers encounter errors and unexpected behavior in their code. The ability to effectively troubleshoot issues is as important as writing the code itself, especially for OSINT professionals who often work with external APIs and data sources that can be unpredictable. This lesson will teach you how to understand Python error messages, use debugging techniques, and resolve common issues.

## Understanding Python Error Messages

When your Python script encounters an error, it generates a "traceback" - a report that shows the sequence of function calls leading to the error. Learning to read tracebacks is the first step in effective troubleshooting.

### Anatomy of a Traceback

Let's look at a typical error message:

```
Traceback (most recent call last):
  File "/path/to/src/sample_script.py", line 42, in <module>
    result = process_data(input_file)
  File "/path/to/src/osint_tools.py", line 23, in process_data
    data = json.loads(content)
  File "/usr/lib/python3.9/json/__init__.py", line 346, in loads
    return _default_decoder.decode(s)
  File "/usr/lib/python3.9/json/decoder.py", line 337, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
  File "/usr/lib/python3.9/json/decoder.py", line 355, in raw_decode
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```

Breaking this down:

1. **The header**: `Traceback (most recent call last)` indicates the start of the error report
2. **The call stack**: Shows the sequence of function calls, with the most recent at the bottom
   - Each line shows the file, line number, and function name
3. **The error type and message**: `JSONDecodeError: Expecting value: line 1 column 1 (char 0)`
   - This tells you what went wrong (in this case, invalid JSON data)

### Key Information to Look For

When analyzing a traceback, focus on:

1. **The last line**: Shows the specific error type and message
2. **The line in your code**: Look for the first reference to your own code
3. **The function call chain**: Understand how the program reached the error
4. **File paths**: Ensure they're correct and accessible

### Common Python Error Types

Understanding common error types helps you diagnose issues faster:

#### SyntaxError
Indicates a problem with the structure of your code, often a missing parenthesis, quotation mark, or colon.
```
SyntaxError: invalid syntax
```

#### NameError
Occurs when you try to use a variable or function that hasn't been defined.
```
NameError: name 'data_file' is not defined
```

#### TypeError
Happens when an operation is applied to a value of the wrong type.
```
TypeError: can only concatenate str (not "int") to str
```

#### IndexError / KeyError
Appears when you try to access an element that doesn't exist in a list or dictionary.
```
IndexError: list index out of range
KeyError: 'username'
```

#### FileNotFoundError
Occurs when a script tries to open a file that doesn't exist.
```
FileNotFoundError: [Errno 2] No such file or directory: 'data.csv'
```

#### ImportError / ModuleNotFoundError
Happens when Python can't find a module you're trying to import.
```
ModuleNotFoundError: No module named 'beautifulsoup'
```

## Using Breakpoints and the Debugger

While print statements are useful for quick checks, the debugger is a more powerful tool for understanding what's happening in your code.

### What is a Breakpoint?

A breakpoint is a marker you place in your code that tells the debugger to pause execution at that line. When the program pauses, you can inspect variables, step through code line by line, and understand the program's state.

### Setting Breakpoints in PyCharm

1. **Click in the gutter**: Click in the margin to the left of the line number where you want to set a breakpoint
   - A red circle appears, indicating a breakpoint
   
2. **Alternative method**: Right-click on the line number and select "Toggle Breakpoint"

3. **Conditional breakpoints**: For more control, right-click an existing breakpoint and select "Edit Breakpoint"
   - Add a condition (e.g., `len(data) > 100`) to only pause when that condition is true

### Starting the Debugger

1. **Right-click in the editor** and select "Debug 'sample_script'"

2. **Alternative**: Click "Run → Debug..." or press Shift+F9 (Windows/Linux) or Ctrl+D (macOS)

3. **The script will run** until it hits a breakpoint or finishes

### Using the Debug Tool Window

When the debugger pauses at a breakpoint, the Debug tool window appears, showing:

1. **Variables view**: Shows all local and global variables and their values
   - Expand objects to see their attributes
   - Right-click a variable to add it to "Watches" for continuous monitoring

2. **Call stack**: Shows the sequence of function calls that led to the current line

3. **Console**: Allows you to execute Python commands in the current context
   - Useful for testing expressions without modifying your code

### Navigating While Debugging

Use these controls to move through your code:

1. **Step Over** (F8): Executes the current line and moves to the next line
   - If the current line calls a function, the entire function is executed

2. **Step Into** (F7): Moves into the function being called on the current line
   - Use this to debug inside function calls

3. **Step Out** (Shift+F8): Completes the current function and returns to the caller
   - Useful when you're done examining a function

4. **Resume Program** (F9): Continues execution until the next breakpoint or the end

5. **Stop** (Ctrl+F2): Terminates the debugging session

### Inspecting Variables

To understand what's happening in your code:

1. **Hover over a variable** in the editor while debugging to see its value

2. **Use the Variables view** to see all available variables

3. **Add a Watch** for expressions you want to monitor throughout debugging:
   - Click the "+" in the Watches panel
   - Enter an expression, such as `len(results)` or `data["users"][0]`

4. **Evaluate expressions** on demand:
   - Right-click in the editor and select "Evaluate Expression"
   - Enter a Python expression to evaluate in the current context

## Common OSINT Script Issues

OSINT scripts often face specific challenges. Here are common issues and solutions:

### Network Connection Problems

**Symptoms**: Connection timeouts, connection refused errors

**Troubleshooting**:
1. Check internet connectivity
2. Verify URL is correct
3. Test if the target site is accessible in a browser
4. Add debugging to see request headers and response codes:
   ```python
   response = requests.get(url)
   print(f"Status code: {response.status_code}")
   print(f"Headers: {response.headers}")
   ```

### Rate Limiting and IP Blocking

**Symptoms**: HTTP 429 errors, sudden 403 Forbidden responses

**Troubleshooting**:
1. Add delays between requests: `time.sleep(2)`
2. Implement exponential backoff for retries
3. Use rotating user agents or proxies
4. Check the site's terms of service and robots.txt

### Data Parsing Errors

**Symptoms**: JSON decode errors, KeyError, IndexError

**Troubleshooting**:
1. Print the raw response before parsing: `print(response.text[:500])`
2. Check if the API response format has changed
3. Add error handling for missing keys:
   ```python
   username = data.get("user", {}).get("username", "unknown")
   ```
4. Validate data before processing

### API Authentication Issues

**Symptoms**: 401 Unauthorized, 403 Forbidden

**Troubleshooting**:
1. Verify API keys and tokens are correct
2. Check if tokens have expired
3. Confirm authentication headers are formatted correctly
4. Test API credentials with a simple request

### Path and File Issues

**Symptoms**: FileNotFoundError, permission errors

**Troubleshooting**:
1. Print the absolute path where the script is looking:
   ```python
   import os
   print(f"Looking for file at: {os.path.abspath(file_path)}")
   ```
2. Check working directory: `print(os.getcwd())`
3. Verify file existence: `print(os.path.exists(file_path))`
4. Check file permissions

## Best Practices for Easier Debugging

Follow these practices to make troubleshooting easier:

### 1. Use Try-Except Blocks

Wrap risky operations in try-except blocks to handle errors gracefully:

```python
try:
    data = json.loads(response.text)
    result = process_data(data)
except json.JSONDecodeError as e:
    print(f"Failed to parse JSON: {e}")
    print(f"Response text: {response.text[:500]}")
except KeyError as e:
    print(f"Missing expected key: {e}")
```

### 2. Add Verbose Logging

Use Python's logging module instead of print statements:

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logging.debug(f"Sending request to {url}")
response = requests.get(url)
logging.debug(f"Received status code {response.status_code}")
```

### 3. Validate Inputs and Assumptions

Check assumptions before processing data:

```python
def process_user_data(user_dict):
    # Validate input
    if not isinstance(user_dict, dict):
        raise TypeError(f"Expected dict, got {type(user_dict)}")
    
    if "name" not in user_dict:
        raise ValueError("User data missing required 'name' field")
    
    # Process the data
    ...
```

### 4. Create Isolated Test Cases

When facing complex issues, create simplified test cases:

```python
# Test if the API connection works
def test_api_connection():
    response = requests.get(API_BASE_URL + "/status")
    print(f"Status code: {response.status_code}")
    print(f"Response: {response.text}")

# Test if JSON parsing works
def test_json_parsing():
    sample_data = '{"user": {"id": 123, "name": "test_user"}}'
    parsed = json.loads(sample_data)
    print(f"Parsed data: {parsed}")
```

### 5. Use PyCharm's Code Inspection

PyCharm highlights potential issues before you run the code:

1. Pay attention to underlined code in the editor
2. Hover over warnings to see explanations
3. Use "Code → Inspect Code" to run a complete analysis
4. Address warnings even if the code seems to work

## Advanced Debugging Techniques

As you grow more comfortable with basic debugging, explore these advanced techniques:

### Remote Debugging

Debug scripts running on remote servers:

1. Configure PyCharm for remote debugging
2. Run scripts with the PyCharm debug egg
3. Connect to the remote process from your local PyCharm instance

### Post-Mortem Debugging

Analyze a program after it has crashed:

```python
try:
    # Your code here
except Exception:
    import pdb
    pdb.post_mortem()
```

### Debugging with ipdb

The ipdb package provides enhanced debugging:

```bash
pip install ipdb
```

Then add breakpoints in your code:

```python
import ipdb

def process_data(data):
    ipdb.set_trace()  # Debugger will stop here
    # Rest of function
```

## Next Steps

Now that you understand how to read error messages, use the debugger, and troubleshoot common issues, you're equipped to tackle problems in your OSINT scripts. In the next section, we'll build on these skills as we explore the fundamentals of Git for version control.

Remember that debugging is a skill that improves with practice. Each error you resolve teaches you something new about Python, your tools, or the systems you're interacting with. Don't be discouraged by errors—they're a normal part of development and often lead to the most valuable learning experiences.
