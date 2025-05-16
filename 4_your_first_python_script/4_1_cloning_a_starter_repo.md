# 4.1 Cloning a Starter Repo

Now that you have your development environment set up, it's time to get your first OSINT project onto your local machine. In this section, we'll cover how to clone a GitHub repository using both the command line and PyCharm's GUI interface.

## What is Cloning?

Cloning is the process of creating a local copy of a remote repository on your computer. When you clone a repository, you download all the files, commit history, branches, and other metadata associated with the project.

## Why Clone an Existing Repository?

For OSINT professionals learning to code, starting with existing code offers several advantages:

1. **Learning from examples** - See how experienced developers structure their code
2. **Building on established tools** - Add your own features to working OSINT tools
3. **Avoiding setup mistakes** - Start with a properly configured project
4. **Contributing to open source** - Help improve the tools you use in your work

## Preparing to Clone

Before cloning, you'll need:

1. **The repository URL** - Usually in the format `https://github.com/username/repository.git` or `git@github.com:username/repository.git`
2. **Git installed** and configured on your machine (covered in section 2.2)
3. **SSH keys set up** for secure authentication (covered in section 2.3, if using SSH)

## The OSINT-Starter Repository

For this course, we've created a simple OSINT starter repository with basic Python scripts for common tasks. The repository URL is:

```
https://github.com/osint-training/osint-starter.git
```

> **Note**: If the repository above doesn't exist or you're working with different materials, substitute the URL provided by your instructor.

## Method 1: Cloning Using Git Command Line

### Step 1: Open a Terminal or Command Prompt

- On Windows: Open Git Bash or Command Prompt
- On macOS/Linux: Open Terminal

### Step 2: Navigate to Your Projects Directory

Choose where you want to store your projects, for example:

```bash
# Windows
cd C:\Users\YourUsername\Documents\Projects

# macOS/Linux
cd ~/Documents/Projects
```

If the directory doesn't exist, create it:

```bash
# Windows
mkdir C:\Users\YourUsername\Documents\Projects

# macOS/Linux
mkdir -p ~/Documents/Projects
```

### Step 3: Clone the Repository

For HTTPS cloning (username/password authentication):

```bash
git clone https://github.com/osint-training/osint-starter.git
```

For SSH cloning (SSH key authentication):

```bash
git clone git@github.com:osint-training/osint-starter.git
```

You should see output showing the cloning progress:

```
Cloning into 'osint-starter'...
remote: Enumerating objects: 74, done.
remote: Counting objects: 100% (74/74), done.
remote: Compressing objects: 100% (54/54), done.
remote: Total 74 (delta 20), reused 67 (delta 13), pack-reused 0
Receiving objects: 100% (74/74), 25.96 KiB | 3.24 MiB/s, done.
Resolving deltas: 100% (20/20), done.
```

### Step 4: Navigate into the Cloned Repository

```bash
cd osint-starter
```

### Step 5: Verify the Clone

List the contents of the repository:

```bash
# Windows
dir

# macOS/Linux
ls -la
```

You should see the project files, including a `.git` directory, which stores all the version control information.

## Method 2: Cloning Using PyCharm GUI

PyCharm provides a user-friendly interface for cloning repositories directly into the IDE.

### Step 1: Open PyCharm

Launch PyCharm from your applications menu or desktop shortcut.

### Step 2: Access the Clone Dialog

There are several ways to access the clone dialog:

- From the welcome screen, select "Get from VCS"
- From the main menu, select "File → New → Project from Version Control → Git"
- From the main menu, select "VCS → Get from Version Control"

### Step 3: Enter Repository URL and Directory

1. In the "URL" field, enter the repository URL:
   ```
   https://github.com/osint-training/osint-starter.git
   ```

2. In the "Directory" field, choose where you want to store the project locally:
   - Click the folder icon to browse for a location
   - Select or create a folder for your projects

3. Click "Clone"

### Step 4: Wait for Indexing

After cloning, PyCharm will index the project files for code navigation and completion. This may take a few moments, especially for larger projects.

### Step 5: Configure Python Interpreter

PyCharm will usually prompt you to set up a Python interpreter. If it doesn't:

1. Go to "File → Settings" (Windows/Linux) or "PyCharm → Preferences" (macOS)
2. Navigate to "Project: osint-starter → Python Interpreter"
3. Click the gear icon and select "Add..."
4. Choose "Virtualenv Environment" → "New environment"
5. Select the base interpreter (your installed Python)
6. Click "OK"

## Method 3: Cloning and Opening in Two Steps

You can also combine the command line and PyCharm methods:

1. Clone using the command line (Method 1)
2. Open the cloned project in PyCharm:
   - From the welcome screen, select "Open"
   - Navigate to the cloned repository folder
   - Click "OK"

## Forking vs. Cloning

If you plan to contribute to a public repository or make significant changes, consider **forking** before cloning:

1. **Forking** creates your own copy of the repository on GitHub
2. **Cloning** downloads a repository to your local machine

To fork a repository:
1. Visit the repository page on GitHub
2. Click the "Fork" button in the top right
3. Select your GitHub account as the destination
4. Then clone your forked repository instead of the original

## Common Cloning Issues and Solutions

### Authentication Failed

**Problem**: "Permission denied" or "Authentication failed" errors

**Solutions**:
- For HTTPS: Ensure your GitHub username and password are correct
- For SSH: Verify your SSH key is properly added to GitHub (see Section 2.3)
- Check you have read access to the repository

### Repository Not Found

**Problem**: "Repository not found" error

**Solutions**:
- Double-check the repository URL for typos
- Ensure the repository exists and is public (or you have access)
- Verify your GitHub account has access if it's a private repository

### SSL Certificate Error

**Problem**: "SSL certificate problem" or "server certificate verification failed"

**Solutions**:
- Update Git to the latest version
- Check your system date and time are correct
- If on a corporate network, check with your IT department about SSL inspection

## Next Steps

Now that you've successfully cloned your first repository, you're ready to open and run the scripts in PyCharm. In the next section, we'll explore how to open the project, mark the source directories correctly, and run your first OSINT Python script.

Remember that cloning is just the first step in the Git workflow. As you progress, you'll learn how to make changes, commit them, create branches, and even submit pull requests to contribute back to open-source OSINT tools.
