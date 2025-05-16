# 8.1 Setting Up the First Shared Project

When moving from individual work to team collaboration, setting up a shared GitHub repository with proper permissions and protections is essential. This lesson guides you through creating an organization repository, adding team members with appropriate roles, and implementing branch protection rules to maintain code quality.

## Why Use GitHub Organizations

For collaborative OSINT projects, GitHub Organizations offer several advantages over personal repositories:

1. **Centralized ownership** - Repositories belong to the organization, not individual users
2. **Team management** - Create teams with different access levels for various projects
3. **Improved visibility** - Easier for members to find relevant repositories
4. **Streamlined permissions** - Manage access at both team and individual levels
5. **Enhanced security** - Additional security features like SAML SSO (in paid plans)

## Creating a GitHub Organization

Let's start by creating a new organization:

1. **Click the "+" icon** in the top-right corner of GitHub
2. **Select "New organization"** from the dropdown menu
3. **Choose a plan** - For most OSINT teams, the free plan is sufficient
4. **Fill in organization details**:
   - Name: Choose something professional like `osint-research-team`
   - Contact email: Use a shared or team email if possible
   - Verify your account if prompted

5. **Add organization members** (optional at this stage)
   - You can add team members now or later
   - Each person will receive an email invitation

## Setting Up Your First Organization Repository

Now, let's create a repository within the organization:

1. **Navigate to your organization** page
2. **Click the "New repository" button**
3. **Configure repository settings**:
   - Name: Choose a descriptive name like `social-media-analyzer`
   - Description: Add a concise summary of the project's purpose
   - Visibility: Select "Private" for sensitive OSINT projects or "Public" for open tools
   - Initialize with a README: Check this option
   - Add .gitignore: Select "Python"
   - Choose a license: Select an appropriate open source license (if public)

4. **Click "Create repository"**

## Repository Structure Setup

After creating the repository, establish the basic project structure (as covered in Lesson 7.1):

1. **Clone the repository locally**:
   ```bash
   git clone https://github.com/your-organization/repo-name.git
   cd repo-name
   ```

2. **Set up the basic directory structure**:
   ```bash
   mkdir -p src/package_name
   mkdir -p tests
   mkdir -p docs
   touch src/package_name/__init__.py
   ```

3. **Create essential files**:
   - `.env.example` - Template for environment variables
   - `pyproject.toml` - Project metadata and dependencies
   - `.pre-commit-config.yaml` - Code quality hooks
   - `CONTRIBUTING.md` - Guidelines for contributors

4. **Commit and push the structure**:
   ```bash
   git add .
   git commit -m "Set up initial project structure"
   git push origin main
   ```

## Adding Team Members

There are two approaches to adding members: individual collaborators or teams.

### Adding Individual Collaborators

For smaller projects with just a few members:

1. **Go to repository Settings**
2. **Click "Collaborators and teams"** in the sidebar
3. **Click "Add people"**
4. **Enter GitHub usernames or email addresses**
5. **Select appropriate permission level**:
   - **Read**: Can view and clone the repository (good for observers)
   - **Triage**: Can manage issues and pull requests without writing code (good for project managers)
   - **Write**: Can push to non-protected branches (good for most contributors)
   - **Maintain**: Can manage the repository without access to sensitive actions (good for team leads)
   - **Admin**: Full control of the repository (limit to key personnel)

### Creating Teams

For larger projects or organizations with multiple repositories:

1. **Go to your organization page**
2. **Click the "Teams" tab**
3. **Click "New team"**
4. **Set team details**:
   - Name: e.g., `developers`, `analysts`, or `admins`
   - Description: Explain the team's purpose/role
   - Parent team: Optionally nest under another team
   - Team visibility: Usually "Visible" unless there's a need for privacy

5. **Click "Create team"**
6. **Add members to the team**:
   - Click "Add a member" and enter GitHub usernames
   - Each person will receive an invitation

7. **Grant team access to repositories**:
   - Go to the team's page
   - Click the "Repositories" tab
   - Click "Add repository"
   - Select your repository
   - Choose permission level

## Best Practices for Team Roles

For OSINT projects, consider this role structure:

1. **Administrators** (1-2 people)
   - Manage repository settings and security
   - Establish branch protection rules
   - Control sensitive information

2. **Maintainers / Team Leads**
   - Review and merge pull requests
   - Manage issues and milestones
   - Guide project direction

3. **Contributors / Developers**
   - Write code and create pull requests
   - Report issues and bugs
   - Implement features and fixes

4. **Analysts / Researchers**
   - May need read-only access to the code
   - Focus on using the tools rather than developing them
   - Contribute insights and feature requests

## Setting Up Branch Protection Rules

Branch protection prevents accidental or unauthorized changes to important branches:

1. **Go to repository Settings**
2. **Click "Branches" in the sidebar**
3. **Click "Add branch protection rule"**
4. **Configure protection settings**:
   - Branch name pattern: `main` (or your default branch)
   - Check "Require a pull request before merging"
   - Check "Require approvals" (set to 1 or more reviewers)
   - Optionally check "Require status checks to pass before merging"
   - If using CI (from Lesson 7.7), add your test workflow as a required check

5. **Additional settings to consider**:
   - "Restrict who can push to matching branches" - Limit who can push directly
   - "Require linear history" - Prevents merge commits
   - "Include administrators" - Apply rules to everyone (recommended)

6. **Click "Create"**

## Creating a Helpful CONTRIBUTING.md

A CONTRIBUTING.md file guides new team members on how to contribute to the project:

```markdown
# Contributing to [Project Name]

Thank you for considering contributing to our OSINT tool! This document outlines the process for contributing and the standards we maintain.

## Development Environment Setup

1. Clone the repository:
   ```
   git clone https://github.com/organization/repository.git
   cd repository
   ```

2. Create a virtual environment:
   ```
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```
   pip install -e ".[dev]"
   ```

4. Install pre-commit hooks:
   ```
   pre-commit install
   ```

## Workflow

1. Create a new branch for your work:
   ```
   git checkout -b feature/your-feature-name
   ```

2. Make your changes and commit them:
   ```
   git add .
   git commit -m "Descriptive commit message"
   ```

3. Push your branch:
   ```
   git push origin feature/your-feature-name
   ```

4. Create a Pull Request through GitHub

## Code Standards

- Follow PEP 8 style guidelines
- Use docstrings for all functions, classes, and modules
- Write unit tests for new functionality
- Keep commits focused and related to a single topic
- Use descriptive commit messages

## Pull Request Process

1. Ensure your code passes all tests and pre-commit checks
2. Update documentation if needed
3. Request review from at least one team member
4. Address review comments and update your PR
5. Once approved, your changes will be merged

## Security Considerations

- Never commit API keys, tokens, or credentials
- Use environment variables for sensitive configuration
- Document any potential security implications
```

## Setting Up Project-Wide GitHub Settings

Additional settings to configure for your organization repository:

1. **Default branch name**:
   - In organization settings → Repository defaults
   - Set default branch to `main`

2. **GitHub Pages** (if using for documentation):
   - In repository settings → Pages
   - Configure source (e.g., `main` branch, `/docs` folder)

3. **GitHub Actions permissions**:
   - In repository settings → Actions → General
   - Configure workflow permissions and allowed actions

4. **Security policies**:
   - Consider adding a SECURITY.md file with vulnerability reporting guidelines
   - Enable security alerts in repository settings → Security & analysis

## OSINT-Specific Considerations

For OSINT projects, additional considerations for shared repositories:

1. **Data handling policies** - Document how to handle sensitive collected data
2. **Attribution guidelines** - Create clear rules for documenting intelligence sources
3. **Access compartmentalization** - Consider who needs access to sensitive code or results
4. **Privacy considerations** - Add guidelines for protecting both team members and investigation subjects
5. **Ethics framework** - Establish boundaries for OSINT activities

## Next Steps

With your shared repository established, team added, and branch protection enabled, you're ready to start collaborative development. In the next section, we'll cover code review etiquette to ensure productive feedback and maintain high-quality code.
