# 3.1 Installing Python 3.x (Official Builds)

Python is the programming language we'll use throughout this course for OSINT scripts and tools. This guide will walk you through installing the official Python distribution on your system.

## Why Use the Official Python Distribution?

While many operating systems come with Python pre-installed or offer Python through package managers, we recommend using the official builds from python.org because:

- You'll get the most current version with all the latest features
- The installation is consistent across operating systems
- The official installer includes pip (Python's package manager)
- You have control over exactly which version you install

## Choosing the Right Python Version

For OSINT work in 2024-2025:

- Install **Python 3.10 or newer** (ideally the latest stable release)
- Always choose **64-bit** versions unless you have a specific reason not to
- Avoid Python 2.x as it's no longer maintained

## Installation by Operating System

### Windows Installation

1. **Download the installer**
   - Visit [https://www.python.org/downloads/](https://www.python.org/downloads/)
   - Click "Download Python" for the latest version
   - Alternatively, scroll down to choose a specific version
   - Select the **Windows 64-bit installer**

2. **Run the installer**
   - Open the downloaded .exe file
   - **IMPORTANT**: Check "Add Python to PATH" before clicking "Install Now"
     ![Add Python to PATH](https://docs.python.org/3/_images/win_installer.png)

3. **Verify installation**
   - Open Command Prompt
   - Type `python --version` and press Enter
   - You should see the version number (e.g., `Python 3.11.4`)
   - Also verify pip with `pip --version`

### macOS Installation

1. **Download the installer**
   - Visit [https://www.python.org/downloads/](https://www.python.org/downloads/)
   - Click "Download Python" for the latest version
   - Select the **macOS 64-bit universal2 installer**

2. **Run the installer**
   - Open the downloaded .pkg file
   - Follow the installation wizard
   - The installer will guide you through the process

3. **Verify installation**
   - Open Terminal
   - Type `python3 --version` and press Enter
   - You should see the version number
   - Check pip with `pip3 --version`

> **Note**: On macOS, you might need to use `python3` and `pip3` commands rather than `python` and `pip` to avoid using the system Python.

### Linux Installation

While you can use your distribution's package manager, the official builds ensure you have the latest version:

1. **Download the source tarball**
   - Visit [https://www.python.org/downloads/](https://www.python.org/downloads/)
   - Select the latest version
   - Download the **XZ compressed source tarball**

2. **Install dependencies**
   
   For Debian/Ubuntu:
   ```bash
   sudo apt update
   sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
   ```
   
   For Fedora:
   ```bash
   sudo dnf groupinstall "Development Tools"
   sudo dnf install zlib-devel bzip2-devel openssl-devel ncurses-devel
   ```

3. **Extract and build Python**
   ```bash
   tar -xf Python-3.x.x.tar.xz  # Replace with actual version
   cd Python-3.x.x
   ./configure --enable-optimizations
   make -j$(nproc)  # Parallel build
   sudo make altinstall  # Use altinstall to avoid replacing system Python
   ```

4. **Verify installation**
   ```bash
   python3.x --version  # Replace x with your version number
   pip3.x --version
   ```

## Alternative: Using Linux Package Managers

If building from source seems complicated, you can use your package manager:

For Ubuntu/Debian:
```bash
sudo apt update
sudo apt install python3 python3-pip
```

For Fedora:
```bash
sudo dnf install python3 python3-pip
```

For Arch Linux:
```bash
sudo pacman -S python python-pip
```

## Verifying Your PATH Configuration

The PATH environment variable tells your system where to look for executable programs. Python needs to be in your PATH.

### Windows
1. Open Command Prompt
2. Type `echo %PATH%` and press Enter
3. Verify you see the Python installation directory
4. If not, you may need to reinstall with "Add Python to PATH" checked

### macOS/Linux
1. Open Terminal
2. Type `echo $PATH` and press Enter
3. Verify you see the Python installation directory
4. If not, you may need to add it:
   ```bash
   echo 'export PATH="$HOME/Library/Python/3.x/bin:$PATH"' >> ~/.zshrc  # For macOS Catalina+
   echo 'export PATH="$HOME/Library/Python/3.x/bin:$PATH"' >> ~/.bash_profile  # For older macOS
   # For Linux
   echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bashrc
   ```

## Installing Python with Version Managers (Advanced)

For users who need multiple Python versions, consider using a version manager:

- **pyenv** (macOS/Linux): Allows installing and switching between multiple Python versions
- **conda** (All platforms): Environment and package manager, useful for data science
- **Python Launcher** (Windows): Installed with Python, allows specifying versions with `py -3.10` syntax

## Common Installation Issues and Solutions

### Windows: "Python is not recognized as an internal or external command"
- Reinstall with "Add Python to PATH" option checked
- Or manually add Python to PATH through System Properties → Environment Variables

### macOS: Multiple Python versions
- Use explicit `python3` and `pip3` commands
- Consider using aliases in your `.zshrc` or `.bash_profile`

### Linux: Missing SSL support
- Install OpenSSL development packages before building Python
- For Debian/Ubuntu: `sudo apt install libssl-dev`

## OSINT-Specific Considerations

When setting up Python for OSINT work:

1. **Privacy**: Consider using a dedicated Python environment for OSINT work to isolate it from personal projects
2. **Security**: Keep Python updated regularly to protect against vulnerabilities
3. **Documentation**: Document your Python version in your project's README for collaboration
4. **Compatibility**: Some OSINT tools may require specific Python versions, so note any constraints

## Next Steps

Now that you have Python installed, the next step is to learn about virtual environments. These will allow you to create isolated environments for different projects, ensuring that dependencies for one project don't conflict with another.

In the next section, we'll guide you through creating and activating virtual environments for your OSINT projects.
