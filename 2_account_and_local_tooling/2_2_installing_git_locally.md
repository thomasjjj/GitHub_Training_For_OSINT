# 2.2 Installing Git Locally

Git is the underlying version control system that powers GitHub. To work effectively with GitHub, you'll need to install Git on your local machine. This guide walks you through downloading, installing, and configuring Git for your operating system.

## What is Git (A Brief Reminder)

Git is a distributed version control system that:
- Tracks changes to files over time
- Allows you to revert to previous versions
- Supports collaborative work through branches
- Functions independently from GitHub (which is a cloud service built on Git)

## Installing Git by Operating System

### Windows Installation

1. **Download the installer**
   - Visit [https://git-scm.com/download/win](https://git-scm.com/download/win)
   - The download should start automatically (64-bit Git for Windows Setup)
   - If not, click on the appropriate download link for your system

2. **Run the installer**
   - Open the downloaded .exe file
   - Accept the license agreement and click "Next"

3. **Choose components**
   - Use the default selection
   - Ensure "Git Bash" and "Git GUI" are selected
   - Click "Next"

4. **Choose default editor**
   - Select the text editor you prefer for Git operations
   - If you plan to use PyCharm (which we'll set up later), you can select "Use Visual Studio Code" as a lightweight alternative for simple edits
   - Click "Next"

5. **Adjust your PATH environment**
   - Select "Git from the command line and also from 3rd-party software"
   - This ensures Git is accessible from both the command prompt and PyCharm
   - Click "Next"

6. **Configure line ending conversions**
   - Choose "Checkout Windows-style, commit Unix-style line endings"
   - This is the recommended setting for Windows users
   - Click "Next"

7. **Configure terminal emulator**
   - Choose "Use Windows' default console window"
   - Click "Next"

8. **Configure extra options**
   - Enable file system caching
   - Enable Git Credential Manager
   - Click "Install"

9. **Complete installation**
   - Click "Finish" once the installation completes

### macOS Installation

1. **Check if Git is already installed**
   - Open Terminal (Applications → Utilities → Terminal)
   - Type `git --version` and press Enter
   - If Git is installed, you'll see the version number
   - If not, you'll be prompted to install it

2. **Install using package manager (recommended)**
   
   Using Homebrew:
   - If you don't have Homebrew, install it first:
     ```
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```
   - Then install Git:
     ```
     brew install git
     ```

3. **Alternative: Download installer**
   - Visit [https://git-scm.com/download/mac](https://git-scm.com/download/mac)
   - Follow the instructions to download and install the latest version

### Linux Installation

For Debian/Ubuntu-based distributions:
```
sudo apt update
sudo apt install git
```

For Fedora:
```
sudo dnf install git
```

For CentOS/RHEL:
```
sudo yum install git
```

For Arch Linux:
```
sudo pacman -S git
```

## Validating Your Installation

After installing Git, verify it's working correctly:

1. **Open a terminal or command prompt**
   - Windows: Open "Git Bash" or "Command Prompt"
   - macOS/Linux: Open "Terminal"

2. **Check Git version**
   ```
   git --version
   ```
   
   You should see output similar to:
   ```
   git version 2.39.2
   ```
   
   The exact version number may differ, but as long as you see a version displayed, Git is installed correctly.

## Configuring Git Global Settings

Before using Git, you should configure your identity. This information will be attached to your commits.

### Setting Your Username and Email

1. **Set your name** (use quotes if your name contains spaces)
   ```
   git config --global user.name "Your Name"
   ```

2. **Set your email** (use the same email as your GitHub account)
   ```
   git config --global user.email "your.email@example.com"
   ```

> **OSINT Privacy Tip**: If you're concerned about privacy, GitHub offers email protection. You can use your GitHub-provided no-reply email address (available in your GitHub email settings) instead of your personal email.

### Additional Helpful Configurations

These configurations are optional but can improve your Git experience:

1. **Set default branch name** (GitHub now uses "main" as the default)
   ```
   git config --global init.defaultBranch main
   ```

2. **Set default text editor** (if you didn't select during installation)
   ```
   git config --global core.editor "notepad"  # Windows
   git config --global core.editor "nano"     # macOS/Linux
   ```

3. **Enable colorful output**
   ```
   git config --global color.ui auto
   ```

### Checking Your Configuration

To verify your settings:
```
git config --list
```

This will display all your Git configuration settings.

## Common Installation Issues and Solutions

### Windows: 'git' is not recognized as an internal or external command

**Solution**: The Git installation directory needs to be added to your PATH.
1. Search for "Environment Variables" in Windows search
2. Click "Edit the system environment variables"
3. Click "Environment Variables"
4. Under "System variables", find "Path" and click "Edit"
5. Add the Git installation path (typically `C:\Program Files\Git\cmd`)
6. Click "OK" on all dialogs
7. Restart your command prompt

### macOS: Command Line Developer Tools are not installed

**Solution**: Install the required tools when prompted, or run:
```
xcode-select --install
```

### Linux: Permission denied

**Solution**: Make sure you have sufficient permissions:
```
sudo apt install git
```

## Next Steps

Now that you have Git installed and configured on your local machine, the next step is to set up SSH keys for secure authentication with GitHub. SSH keys will allow you to connect to GitHub without entering your password each time, which is both more convenient and more secure.

In the next section, we'll guide you through generating and adding an SSH key to your GitHub account.
