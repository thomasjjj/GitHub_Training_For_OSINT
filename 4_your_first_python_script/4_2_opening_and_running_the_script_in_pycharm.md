# 4.2 Opening & Running the Script in PyCharm

Now that you've cloned the starter repository, it's time to open the project in PyCharm and run your first OSINT script. This lesson will walk you through opening the project properly, configuring source roots, and executing the script with debug prints.

## Opening the Project in PyCharm

### If You Used PyCharm to Clone the Repository

If you cloned the repository directly from PyCharm as covered in the previous lesson, the project should already be open. You can skip to the "Exploring the Project Structure" section.

### Opening an Existing Repository

If you cloned the repository using Git from the command line, follow these steps to open it in PyCharm:

1. **Launch PyCharm** from your applications menu or desktop shortcut

2. **From the welcome screen**:
   - Click "Open"
   - Navigate to the location where you cloned the repository
   - Select the root folder of the repository (e.g., `osint-starter`)
   - Click "OK"

3. **Wait for indexing**:
   - PyCharm will index the project files
   - This may take a few moments depending on the project size
   - You'll see a progress bar at the bottom of the window

4. **Trust the project** if prompted:
   - PyCharm may ask if you trust the project
   - Since we're using a known repository for this course, click "Trust Project"

## Exploring the Project Structure

Once the project is open, let's examine its structure in the Project tool window (usually on the left side of the interface).

The OSINT starter repository should have a structure similar to:

```
osint-starter/
├── .git/               # Git repository data (hidden)
├── src/                # Source code directory
│   ├── __init__.py
│   ├── osint_tools.py  # OSINT utility functions
│   └── sample_script.py # Example script to run
├── data/               # Data files directory
│   └── sample_data.csv
├── tests/              # Test files directory
│   └── test_osint_tools.py
├── .gitignore          # Git ignore file
├── README.md           # Project documentation
└── requirements.txt    # Python dependencies
```

## Marking Source Directories

To help PyCharm understand the project structure and provide better code completion, we need to mark certain directories as source roots.

### Why Mark Source Roots?

Marking a directory as a "Sources Root" tells PyCharm that:
- Python packages and modules are located here
- Import statements should be resolved relative to this directory
- Code completion should prioritize modules found here

### Marking the `src/` Directory as Sources Root

1. **Locate the `src/` directory** in the Project tool window

2. **Right-click on the `src/` directory**

3. **Select "Mark Directory as" from the context menu**

4. **Choose "Sources Root"**

You'll notice the `src` directory icon changes color (usually to blue), indicating it's now recognized as a sources root.

### Marking the `tests/` Directory as Test Sources Root (Optional)

Similarly, we should mark the tests directory:

1. **Right-click on the `tests/` directory**

2. **Select "Mark Directory as" from the context menu**

3. **Choose "Test Sources Root"**

The `tests` directory icon should change color (usually to green).

## Setting Up a Virtual Environment

Before running any code, we should ensure we're using a virtual environment with the required dependencies.

### Creating a Virtual Environment

1. **Open settings/preferences**:
   - Windows/Linux: "File → Settings"
   - macOS: "PyCharm → Preferences"

2. **Navigate to "Project: osint-starter → Python Interpreter"**

3. **Click the gear icon** in the top-right corner and select "Add..."

4. **Choose "Virtualenv Environment" → "New environment"**

5. **Set the location** to a directory inside your project (e.g., `venv`)

6. **Select the base interpreter** (your installed Python version)

7. **Click "OK"** to create the virtual environment

### Installing Dependencies

The repository should include a `requirements.txt` file listing the necessary packages. Install them:

1. **Open the Terminal** in PyCharm (from the bottom toolbar or "View → Tool Windows → Terminal")

2. **Ensure your virtual environment is activated** (the terminal prompt should show the environment name)

3. **Run the pip install command**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Wait for the installation to complete**

## Running Your First Script

Now that your environment is set up, let's run the sample script.

### Opening the Script

1. **Navigate to the `src/` directory** in the Project tool window

2. **Double-click on `sample_script.py`** to open it in the editor

3. **Review the code** to understand what it does
   - The sample script likely demonstrates some basic OSINT functionality
   - Look for comments explaining the purpose of different code sections

### Adding Debug Prints

Before running the script, let's add some debug prints to see what's happening during execution:

1. **Add `print` statements** at key points in the code to show variable values:
   ```python
   # Example: After loading data
   print(f"Loaded {len(data)} records from the data file")
   
   # Example: Before processing data
   print(f"Processing data with parameters: {params}")
   
   # Example: After processing
   print(f"Processing complete. Results: {results}")
   ```

2. **Save the file** (Ctrl+S or ⌘S)

### Running the Script

There are multiple ways to run the script in PyCharm:

#### Method 1: Using the Run Button

1. **Right-click anywhere in the editor** with the script open

2. **Select "Run 'sample_script'"**

#### Method 2: Using the Run Menu

1. **Open the Run menu** from the top menu bar

2. **Select "Run... "** or press Alt+Shift+F10 (Windows/Linux) or Ctrl+Alt+R (macOS)

3. **Select "sample_script"** from the list

#### Method 3: Using the Keyboard Shortcut

With the script file open, press Shift+F10 (Windows/Linux) or Ctrl+R (macOS)

### Viewing the Output

When the script runs, a Run tool window will appear at the bottom of the IDE, showing:

1. **The command used to run the script**

2. **Output from your print statements**

3. **Any errors or exceptions** that occurred

4. **The exit code** (0 indicates success, non-zero indicates failure)

## Running with Different Configurations

You can create different run configurations for different scenarios:

### Creating a New Run Configuration

1. **Click "Run → Edit Configurations..."**

2. **Click the "+" button** and select "Python"

3. **Name your configuration** (e.g., "Sample Script with Test Data")

4. **Set the Script path** to the path of the script

5. **Add parameters** if needed (e.g., input file paths, options)

6. **Set the Python interpreter** to your virtual environment

7. **Click "OK"** to save the configuration

### Selecting and Running Configurations

1. **Select your configuration** from the dropdown menu in the top-right toolbar

2. **Click the Play button** next to it to run

## Running Scripts with Input Arguments

If the script accepts command-line arguments (like most OSINT tools do), you can provide them in PyCharm:

1. **Edit the run configuration** as described above

2. **In the "Parameters" field**, add your arguments:
   ```
   --input data/custom_data.csv --output results.json --verbose
   ```

3. **Save and run** the configuration

## Dealing with Working Directories

Sometimes scripts need to run from a specific directory to locate resources properly:

1. **In the run configuration**, set the "Working directory" field to the appropriate path

2. **Common options include**:
   - Project root directory (the default)
   - The directory containing the script
   - The `data` directory (if the script primarily works with data files)

## Next Steps

Now that you've run your first OSINT script, you're ready to start making changes and dealing with any issues that arise. In the next section, we'll cover basic troubleshooting techniques including how to read tracebacks and use breakpoints to inspect variables when something goes wrong.

As you become more comfortable running scripts in PyCharm, you'll appreciate how the IDE streamlines the development workflow, making it easier to focus on writing effective OSINT tools rather than wrestling with execution details.
