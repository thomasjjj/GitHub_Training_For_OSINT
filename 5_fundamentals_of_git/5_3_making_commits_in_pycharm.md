# 5.3 Making Commits in PyCharm

Commits are the fundamental building blocks of your project's history in Git. A commit records a snapshot of your changes at a specific point in time, along with a message explaining what those changes represent. In this lesson, we'll learn how to make effective commits using PyCharm's Git integration.

## Understanding Commits

Before diving into the mechanics, let's understand what a commit represents:

- A commit is a **snapshot** of your project at a specific point in time
- Each commit has a unique identifier (a SHA-1 hash)
- Commits include **metadata** like author, date, and a descriptive message
- A commit stores the **differences** (or "diffs") between the previous version and the current version
- Commits build upon each other to form your project's **history**

Think of commits as save points in a video game - they allow you to track progress and return to previous states if needed.

## The Commit Process

Making a commit involves three main steps:

1. **Staging changes** - Selecting which files or changes to include in the commit
2. **Writing a commit message** - Documenting what the changes are about
3. **Creating the commit** - Finalizing the snapshot with your staged changes

Let's explore how to perform these steps in PyCharm.

## Viewing Changes in PyCharm

Before committing, you'll want to review what has changed. PyCharm makes this easy:

1. **Modified files** appear in a different color (usually blue) in the Project tool window
2. **The VCS tool window** shows pending changes (accessible via "View → Tool Windows → Version Control")
3. **The gutter** (margin next to the line numbers) shows colored markers for lines you've changed:
   - Green for added lines
   - Blue for modified lines
   - Grey for deleted lines

### Opening the Changes Tool Window

The Changes tool window is your central hub for managing commits:

1. Click "VCS → Commit..." or press `Ctrl+K` (Windows/Linux) or `⌘K` (macOS)
2. The Changes tool window will open, showing all modified files

## Staging Changes

Unlike the command line where you need to explicitly `git add` files to stage them, PyCharm automatically includes all modified files in the commit by default. However, you still have fine-grained control:

### Selecting Files to Commit

In the Changes tool window:

1. **Checkbox next to each file** - Check or uncheck to include or exclude files from the commit
2. **Right-click a file** - Additional options like "Move to Another Changelist" or "Rollback"

### Viewing and Selecting Specific Changes Within Files

For more precise control:

1. **Click on a file** in the Changes window to see its diff in the right panel
2. **Check/uncheck specific lines or chunks** by clicking the checkboxes in the gutter
3. **Use the gear icon** ⚙️ in the diff view for more options

This allows you to commit only some changes within a file, saving others for a later commit.

### Using Changelists for Organization

Changelists help organize changes into logical groups:

1. **Create a new changelist** by clicking the "+" icon in the Changes tool window
2. **Name it** based on its purpose (e.g., "Bug fix for login" or "New visualization feature")
3. **Drag files** between changelists to organize your work
4. **Commit each changelist** separately

## Writing Effective Commit Messages

A good commit message is crucial for understanding the project's history later. PyCharm provides a text field at the top of the Changes tool window for your commit message.

### Commit Message Structure

Follow this structure for clear commit messages:

```
Short summary of changes (50 chars or less)

More detailed explanation if necessary. Keep it wrapped around
72 characters. The blank line separating the summary from the body
is critical for tools to work correctly.

- Bullet points are okay
- Typically hyphen or asterisk is used
- Use a hanging indent

Relates to: #123
```

### Guidelines for Good Commit Messages

1. **Be specific** and descriptive
2. **Use imperative mood** ("Add feature" not "Added feature" or "Adds feature")
3. **Keep the first line short** (50 characters or less)
4. **Include the "why"** not just the "what" in the description
5. **Reference issue numbers** where applicable

### OSINT-Specific Commit Message Practices

For OSINT work, consider these additional guidelines:

1. **Avoid revealing sensitive investigation targets** in commit messages
2. **Use general descriptions** for sensitive changes
3. **Document data sources** where appropriate
4. **Note when adding new tool capabilities**
5. **Mention validation steps** for critical features

### Examples of Good Commit Messages

```
Add Twitter geolocation extraction feature

Implement functionality to extract and map geolocation data 
from Twitter posts. Uses the Twitter API to retrieve coordinates
when available and falls back to location string parsing.

- Handles both exact coordinates and place names
- Includes error handling for rate limits
- Adds caching to minimize API calls

Closes #45
```

## Creating the Commit

Once you've staged your changes and written a commit message:

1. **Click "Commit"** at the bottom of the Changes window

### Commit Options in PyCharm

The Commit button has a dropdown with additional options:

- **Commit and Push** - Creates the commit locally and immediately pushes to the remote repository
- **Create Patch** - Creates a patch file instead of a commit
- **Commit Message History** - Shows your recent commit messages for reuse

### Pre-Commit Features

PyCharm offers several options before committing:

- **Reformat code** - Automatically format code to match your style settings
- **Optimize imports** - Organize and clean up import statements
- **Perform code analysis** - Run inspections to check for problems
- **Check TODO** - Review TODO comments in changed files
- **Run tests** - Run unit tests before committing

These options appear at the bottom of the Commit window and help maintain code quality.

## Viewing Commit History

After making commits, it's important to be able to review the project history:

1. **Go to "VCS → Git → Show History"** or right-click a file and select "Git → Show History"
2. **The Log window** will open, showing commits, branches, and tags

### Features of the Log Window

- **Graph view** - Visual representation of branches and merges
- **Commit details** - Shows message, author, date, and changes
- **Search** - Find commits by message, author, or affected files
- **Filter** - Narrow down history by branch, user, date, or folder

### Exploring a Specific Commit

To examine a commit in detail:

1. **Click on a commit** in the Log window
2. **View the commit message and metadata** in the right panel
3. **See the list of changed files** below
4. **Click on a file** to see exactly what changed
5. **Use the "Show Diff"** button to compare with previous versions

### Working with Past Commits

The Log window allows you to:

- **Checkout a commit** - Right-click and select "Checkout Revision"
- **Create a branch** from a past commit
- **Revert changes** introduced by a commit
- **Cherry-pick commits** from one branch to another

## Amending Commits

If you notice an issue immediately after committing, you can amend the commit:

1. **Make the additional changes** to your files
2. **Open the Commit window** (Ctrl+K/⌘K)
3. **Check the "Amend commit"** checkbox at the bottom
4. **Modify the commit message** if needed
5. **Click "Commit"** to update the previous commit

> **Important**: Only amend commits that haven't been pushed to a shared repository, as it rewrites history.

## Common Commit Mistakes and Solutions

### Problem: Committed sensitive information

**Solution**: 
1. Remove the sensitive data
2. Commit the change
3. Use `git filter-branch` or BFG Repo-Cleaner to purge the data from history
4. Force-push the cleaned repository

### Problem: Committed to the wrong branch

**Solution**:
1. Note the commit hash
2. Switch to the correct branch
3. Use `git cherry-pick` to apply the commit
4. Go back to the original branch and reset or revert the commit

### Problem: Commit message has typos or is incorrect

**Solution**:
1. If not pushed: Use `git commit --amend` or PyCharm's "Amend commit" feature
2. If already pushed: Consider adding a new commit with a corrected message

## Best Practices for OSINT Projects

1. **Commit early and often** - Make small, focused commits rather than large batches of changes
2. **One logical change per commit** - Keep commits focused on a single purpose
3. **Test before committing** - Ensure your code works before creating a snapshot
4. **Review your changes** before committing to catch unintended modifications
5. **Be mindful of sensitive information** - Avoid committing API keys, credentials, or investigation details
6. **Use descriptive messages** that will make sense months later
7. **Reference issue trackers** in commit messages when applicable

## Next Steps

Now that you understand how to make effective commits in PyCharm, the next step is learning how to share those commits with others by pushing to GitHub. In the next section, we'll cover how to push your local commits to remote repositories, making your work available to teammates or the broader OSINT community.
