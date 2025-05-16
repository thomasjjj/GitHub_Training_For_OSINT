# GitHub & Python Workflow Training Outline  
*For OSINT professionals taking their very first steps into coding and collaborative development.*

---

## 1 · Orientation

### 1.1 What is GitHub?  
*Define GitHub as a cloud-based service built on the Git version-control system, letting individuals and teams host, track, and collaborate on code and data.*

### 1.2 Why Should OSINT Professionals Code?  
*Describe how even modest Python skills unlock data collection, enrichment, and automation workflows that dramatically scale open-source intelligence work.*

### 1.3 The End-to-End Workflow You’ll Learn  
*Preview the journey: create a GitHub account → set up Python → write code in PyCharm → use Git for version control → collaborate through issues, branches, and pull requests.*

---

## 2 · Account & Local Tooling

### 2.1 Creating a GitHub Account  
*Walk through signing up, enabling two-factor authentication, and personalizing your profile to showcase OSINT interests.*

### 2.2 Installing Git Locally  
*Show how to download Git for your OS, validate installation from the terminal, and set global username/email config.*

### 2.3 Generating and Adding an SSH Key  
*Explain why SSH keys avoid constant password prompts and how to create, copy, and register a key with GitHub.*

---

## 3 · Python Environment Setup

### 3.1 Installing Python 3.x (Official Builds)  
*Guide learners to the python.org installers, stressing 64-bit versions and the need to add Python to PATH.*

### 3.2 Creating & Activating Virtual Environments  
*Introduce `python -m venv` and why isolating dependencies keeps projects reproducible and conflict-free.*

### 3.3 Installing PyCharm Community Edition  
*Cover download, installation, and first-run settings—highlighting built-in Git integration and virtual-env detection.*

### 3.4 Configuring PyCharm for Best Practice  
*Demonstrate setting the default interpreter to a venv, enabling black + isort reformat on save, and syncing with Git.*

---

## 4 · Your First Python Script

### 4.1 Cloning a Starter Repo  
*Use `git clone` (or the PyCharm GUI) to copy a public example repository locally.*

### 4.2 Opening & Running the Script in PyCharm  
*Show how to open the folder, mark `src/` as Sources Root, and run the script with debug prints.*

### 4.3 Basic Troubleshooting  
*Teach reading Tracebacks and using breakpoints to inspect variables when something goes wrong.*

---

## 5 · Fundamentals of Git

### 5.1 Repositories, Forks, and Local Clones  
*Clarify the difference between the original repo, your fork on GitHub, and your cloned copy on disk.*

### 5.2 Branches: Why & How  
*Explain that branches create safe sandboxes for new work and walk through `git checkout -b` plus PyCharm’s branch menu.*

### 5.3 Making Commits in PyCharm  
*Demonstrate staging changes, writing meaningful commit messages, and viewing history.*

### 5.4 Pushing to GitHub  
*Show the push button / `git push` to publish local commits to your fork or the main repo (if you have rights).*

### 5.5 Opening Pull Requests (PRs)  
*Explain PRs as a request for review, how to target branches, add reviewers, and link related issues.*

### 5.6 Keeping Your Branch Up-to-Date  
*Teach `git pull --rebase` or PyCharm’s “Update” to integrate upstream changes before merging.*

---

## 6 · Issues & Communication (with AI Assistance)

### 6.1 Creating an Issue That Developers Love  
*Outline the “one problem per issue” rule, use of templates, and adding context, screenshots, or logs.*

### 6.2 Using AI to Polish Issues  
*Show how ChatGPT or GitHub Copilot Chat can rephrase raw notes into clear, actionable issue descriptions.*

### 6.3 Commenting and Closing Issues Gracefully  
*Explain respectful discussion, linking commits with “Fixes #123”, and closing once verified.*

---

## 7 · Project Structure & Code Style

### 7.1 Recommended Layout (`src/`, `tests/`, `docs/`)  
*Present the “src layout” pattern that keeps import paths clean and separates code from metadata.*

### 7.2 `config.py` and Constants  
*Encourage centralizing configurable values and magic numbers in one file for discoverability and reuse.*

### 7.3 Environment Variables & `.env.example`  
*Show how to load sensitive credentials via `python-dotenv`, keep them out of Git, and provide a template file.*

### 7.4 Creating a Solid `.gitignore`  
*List common Python, PyCharm, and OS artifacts to exclude, emphasizing that secrets must never be committed.*

### 7.5 Code Style & Pre-Commit Hooks  
*Introduce black, isort, flake8, and the `pre-commit` framework to enforce formatting before every commit.*

### 7.6 Writing Minimal Tests with `pytest`  
*Explain the value of tests for OSINT scripts that hit external APIs and show a “happy path” test example.*

### 7.7 Continuous Integration with GitHub Actions  
*Demonstrate adding a simple workflow that installs dependencies, runs tests, and lints on every push/PR.*

---

## 8 · Collaboration Workflow

### 8.1 Setting Up the First Shared Project  
*Guide the creation of an organization repo, adding teammates with proper roles, and enabling branch protection.*

### 8.2 Code Review Etiquette  
*Outline giving constructive feedback, requesting changes versus approving, and using suggestions blocks.*

### 8.3 Using GitHub Projects & Discussions  
*Show lightweight Kanban boards for task tracking and Discussions for knowledge-base Q&A separate from issues.*

### 8.4 Handling Merge Conflicts Together  
*Teach reading conflict markers and pair-resolving via PyCharm’s 3-way merge tool.*

---

## 9 · Leveling-Up the Toolkit (Optional Advanced Topics)

### 9.1 Semantic Versioning & Tags  
*Explain MAJOR.MINOR.PATCH semantics and how `git tag` plus GitHub Releases produce downloadable zips.*

### 9.2 Dependency Management with Poetry or pip-tools  
*Introduce pyproject.toml-based workflows that lock exact versions and build wheels.*

### 9.3 Packaging & Publishing to PyPI  
*Step through writing a minimal `pyproject.toml`, building, and uploading so others can `pip install` your tool.*

### 9.4 Automated Documentation with MkDocs  
*Show how to generate a docs site from Markdown (including this outline!) and deploy it with GitHub Pages.*

---

## 10 · Next Steps & Resources

### 10.1 Curated Reading List  
*Provide links to the Git book, GitHub Learning Lab, PyCharm docs, and OWASP’s OSINT cheat sheets.*

### 10.2 Community & Support Channels  
*Recommend relevant Discord/Slack groups, `/r/OSINT` on Reddit, and GitHub Discussions boards.*

### 10.3 Personal Roadmap Template  
*Invite learners to fork a Markdown checklist tracking what they have installed, read, and practiced.*

---

### Appendix A · Reference Commands Cheat-Sheet  
*Include a single-page table of the most common Git, Python, and PyCharm shortcuts covered in the course.*

---

**Consistency Mandate**  
> *Throughout the program, everyone codes in **Python 3**, uses **PyCharm** as the IDE, and hosts work on **GitHub**, following the style and structure rules established in Section 7. Deviations should be proposed through a pull request and agreed upon by the team.*

---
