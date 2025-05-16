# 5.5 Opening Pull Requests (PRs)

Pull Requests (PRs) are a fundamental part of collaborative development on GitHub. They provide a way to propose changes, request reviews, and discuss modifications before integrating them into the main codebase. This lesson will explain what pull requests are, why they're important, and how to create them effectively.

## What is a Pull Request?

A Pull Request is essentially a request to have your code changes pulled into another branch or repository. It's much more than just a technical mechanism—it's a communication tool that:

1. **Notifies others** about your changes
2. **Facilitates code review** by showing exactly what's changed
3. **Enables discussion** through comments and suggestions
4. **Documents the reason** for changes
5. **Triggers integrations** like automated tests

## Why Use Pull Requests?

For OSINT professionals, pull requests offer several benefits:

1. **Quality control** - Get feedback before merging code
2. **Knowledge sharing** - Team members learn from reviewing each other's code
3. **Accountability** - Changes are tracked and attributed to specific people
4. **Documentation** - The PR description and comments explain why changes were made
5. **Integration safety** - Avoid breaking the main codebase

## The Pull Request Workflow

The typical workflow for contributing changes looks like this:

1. **Create a branch** in your local repository
2. **Make changes** and commit them
3. **Push the branch** to your GitHub repository
4. **Open a pull request** from your branch to the target branch
5. **Discuss and revise** based on feedback
6. **Merge the PR** when approved

## Creating a Pull Request on GitHub

After pushing your branch to GitHub, you can create a pull request directly through the web interface:

### Method 1: Automatic Prompt

1. After pushing a branch, GitHub often displays a prompt on the repository page
2. Click the "Compare & pull request" button
3. Fill in the PR details
4. Click "Create pull request"

### Method 2: Manual Creation

1. Navigate to the repository on GitHub
2. Click the "Pull requests" tab
3. Click the "New pull request" button
4. Select the base branch (where you want your changes to go)
5. Select your branch (where your changes are)
6. Click "Create pull request"
7. Fill in the details
8. Click "Create pull request" again

## Creating a Pull Request from PyCharm

PyCharm provides direct GitHub integration for creating pull requests:

1. Go to "VCS → Git → Create Pull Request"
2. Select the target repository and branch
3. Fill in the PR title and description
4. Add reviewers if desired
5. Click "Create Pull Request"

## Writing an Effective Pull Request Description

A good PR description helps reviewers understand your changes and makes the project history more valuable:

### Template for PR Descriptions

```
## Purpose
[Explain why you made these changes and what problem they solve]

## Changes Made
- [List the main changes or features added]
- [Be specific but concise]

## Testing Done
[Describe how you tested these changes]

## Screenshots (if applicable)
[Add screenshots for UI changes]

## Related Issues
Resolves #123
```

### Example for an OSINT Tool

```
## Purpose
Add support for analyzing Mastodon instances in our social media scraper

## Changes Made
- Add Mastodon API client in `src/clients/mastodon.py`
- Implement authentication with both OAuth and API keys
- Add user profile data extraction with rate limiting
- Integrate results into the unified output format

## Testing Done
- Tested against 3 different Mastodon instances
- Verified rate limiting with 1000+ test requests
- Compared output against manual exports

## Related Issues
Resolves #45 - Add Mastodon support
```

## Targeting Branches

When creating a pull request, you need to specify:

1. **Base branch** - Where you want your changes to go (e.g., `main`, `develop`)
2. **Compare branch** - Your branch containing the changes

Choose the appropriate target branch:
- For most changes, target the default branch (usually `main`)
- For long-term development, you might target a `develop` branch
- For changes to documentation only, some projects have a `docs` branch

## Adding Reviewers

Code review is a critical part of the pull request process:

1. **Assign specific reviewers** who are knowledgeable about the code
2. **Consider team assignments** to notify entire teams
3. **@mention people** in comments for specific questions

In GitHub, add reviewers from the sidebar of the PR page:
- Look for "Reviewers" or "Request review"
- Search for team members
- Selected reviewers will be notified

## Linking Related Issues

Connecting your PR to related issues helps maintain project organization:

### Automatic Linking with Keywords

GitHub supports keywords that automatically link PRs to issues:
- `Fixes #123` - Links and closes the issue when the PR is merged
- `Resolves #123` - Same as fixes
- `Closes #123` - Same as fixes
- `Relates to #123` - Links without closing

Include these keywords in your PR description or commit messages.

### Manual Linking

You can also link issues manually:
1. In the PR sidebar, look for "Linked issues"
2. Click "Link an issue"
3. Select the relevant issue

## Reviewing Your Own PR

Before asking others to review your pull request:

1. **Review the "Files changed" tab** yourself
2. **Look for unintended changes** like debugging code or sensitive data
3. **Check for coding standard violations**
4. **Verify tests pass** if the repository has CI/CD setup

## OSINT-Specific Considerations

When creating pull requests for OSINT tools:

1. **Sanitize data** - Remove any sensitive investigation data
2. **Document sources** - Clearly state where data or techniques come from
3. **Consider attribution** - Be mindful of revealing your interests or capabilities
4. **Note ethical considerations** - Document how the tool respects privacy and legal boundaries
5. **Test against real-world scenarios** - Mention how you validated with actual OSINT tasks

## Common Pull Request Issues and Solutions

### PR Shows More Changes Than Expected

**Solution**:
- Check if you created your branch from the latest main
- You might need to merge or rebase from the target branch first

### Conflicts Reported

**Solution**:
- Merge the target branch into your branch
- Resolve conflicts locally
- Push the updated branch

### CI/CD Tests Failing

**Solution**:
- Check the test logs
- Fix issues locally and push again
- Ask for help if you don't understand the failures

## Next Steps

Once your pull request is created, you'll need to respond to feedback and keep your branch up-to-date until it's merged. In the next section, we'll cover how to keep your branches synchronized with the main repository and handle any changes requested during review.
