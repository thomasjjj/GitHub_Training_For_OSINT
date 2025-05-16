# 8.4 Handling Merge Conflicts Together

Merge conflicts occur when Git can't automatically combine changes from different branches. Rather than being a source of frustration, resolving conflicts collaboratively can be an opportunity for knowledge sharing and code improvement. This lesson teaches you how to understand conflict markers and work together to resolve conflicts using PyCharm's powerful merge tools.

## Understanding Merge Conflicts

Merge conflicts typically happen in these scenarios:

1. **Parallel changes** - Two developers modify the same lines in a file
2. **Deleted file edits** - One developer modifies a file that another developer deleted
3. **Incompatible structural changes** - Developers make different changes to related code that Git can't reconcile

When Git encounters a conflict, it marks the problematic areas in the affected files and halts the merge process until you resolve these conflicts manually.

## Reading Conflict Markers

When a conflict occurs, Git modifies the affected files by inserting conflict markers that look like this:

```
<<<<<<< HEAD
Code from the branch you're merging into (usually your current branch)
=======
Code from the branch you're merging from
>>>>>>> feature-branch
```

These markers divide the conflicting section into two parts:

1. **Top section** (`<<<<<<< HEAD` to `=======`) - Contains the code from your current branch (often called "ours")
2. **Bottom section** (`=======` to `>>>>>>> feature-branch`) - Contains the code from the branch you're merging in (often called "theirs")

### Example Conflict

```python
<<<<<<< HEAD
def normalize_username(username):
    """Normalize usernames for consistent handling."""
    if username is None:
        return None
    return username.strip().lower().replace('@', '')
=======
def normalize_username(username):
    # Remove @ symbol and convert to lowercase
    if not username:
        return ""
    return username.strip().lstrip('@').lower()
>>>>>>> feature-branch
```

In this example:
- The current branch has a function that returns `None` for `None` inputs and removes all `@` symbols
- The feature branch returns an empty string for falsy inputs and only removes leading `@` symbols

## Resolving Conflicts Manually

Before looking at PyCharm's tools, it's important to understand how to resolve conflicts manually:

1. **Identify the conflict** by looking for conflict markers
2. **Decide which version to keep** or how to combine them
3. **Edit the file** to remove conflict markers and create the desired code
4. **Mark the conflict as resolved** by staging the file with `git add`

For the example above, a manual resolution might look like:

```python
def normalize_username(username):
    """Normalize usernames for consistent handling."""
    if username is None or not username:
        return ""
    return username.strip().lstrip('@').lower()
```

This combines the docstring from "ours", the empty string return from "theirs", and a compromise on the `@` symbol handling.

## Using PyCharm's Merge Tools

PyCharm provides powerful visual tools that make conflict resolution more intuitive and collaborative.

### Opening the Merge Tool

When you encounter a conflict in PyCharm, you'll typically see a notification. To open the merge tool:

1. **From a merge conflict notification**:
   - Click "Merge" in the notification popup

2. **From the Git menu**:
   - Go to "Git → Resolve Conflicts..."
   - Select the conflicting file(s)
   - Click "Merge"

3. **From the Git tool window**:
   - Open the "Git" tool window (Alt+9 or ⌘9)
   - Go to the "Log" tab
   - Right-click on the merge operation
   - Select "Resolve Conflicts..."

### Understanding PyCharm's Merge Interface

PyCharm's merge tool displays a three-panel interface:

1. **Left panel (Local Changes)**: Your current branch's version
2. **Right panel (Changes from Server)**: The incoming branch's version
3. **Center panel (Merged Result)**: The combined result that you're creating

Above these panels are controls for navigating between conflicts and applying changes from either side.

### Resolving Conflicts with PyCharm

To resolve conflicts using PyCharm:

1. **Navigate between conflicts** using the arrow buttons or the conflict overview bar
2. **Compare the changes** in the left and right panels
3. **Choose which version to use** with the buttons:
   - "Accept Yours" (←) to use your version
   - "Accept Theirs" (→) to use their version
   - Or manually edit the center panel to create a custom resolution
4. **Verify the merged result** in the center panel
5. **Click "Apply" or "Save and finish"** once all conflicts are resolved

## Pair Programming Approach to Conflict Resolution

Resolving conflicts is often more effective as a collaborative activity. Here's how to approach it as a pair:

### Before You Start

1. **Schedule a short meeting** with the other developer(s) involved
2. **Ensure everyone has the latest code** by fetching recent changes
3. **Identify which files have conflicts** and understand their purpose

### During the Pair Resolution Session

1. **Share screen** with the merge tool open
2. **Review each conflict together**:
   - Discuss the intent behind each change
   - Understand why the conflict occurred
   - Explain any domain knowledge relevant to the changes

3. **Decide on a resolution strategy**:
   - Take one version entirely
   - Combine elements from both versions
   - Rewrite the code to better accommodate both changes

4. **Document decisions** for complex resolutions:
   - Add comments explaining the rationale
   - Update related documentation if the resolution changes behavior

### After Resolution

1. **Test the merged code** together to ensure it works as expected
2. **Commit the resolved changes** with a clear message
3. **Discuss how to avoid similar conflicts** in the future

## Remote Collaboration on Merge Conflicts

If you can't meet in person, you can still resolve conflicts collaboratively:

1. **Use screen sharing** in a video call
2. **Use a collaborative IDE** like VS Code LiveShare (if not using PyCharm)
3. **Share a draft resolution** for review before committing
4. **Document your reasoning** in commit messages or PR comments

## Example: Resolving a Complex Conflict Together

Let's walk through a realistic example of resolving a conflict in an OSINT tool:

### The Conflict

```python
<<<<<<< HEAD
def search_social_profiles(username, platforms=None):
    """
    Search for user profiles across social platforms.
    
    Args:
        username: The username to search for
        platforms: List of platforms to search (default: all supported)
    
    Returns:
        Dictionary of platform -> profile URL
    """
    results = {}
    username = normalize_username(username)
    
    if not platforms:
        platforms = SUPPORTED_PLATFORMS
        
    for platform in platforms:
        if platform not in SUPPORTED_PLATFORMS:
            logger.warning(f"Unsupported platform: {platform}")
            continue
            
        try:
            url = PLATFORM_FUNCTIONS[platform](username)
            if url:
                results[platform] = url
        except Exception as e:
            logger.error(f"Error searching {platform}: {e}")
    
    return results
=======
def search_social_profiles(username, platforms=None, timeout=30, proxy=None):
    """Search for user profiles across social media platforms"""
    if not username:
        return {}
        
    normalized = normalize_username(username)
    results = {}
    
    # Use all platforms if none specified
    search_platforms = platforms or SUPPORTED_PLATFORMS
    
    # Set up session with proxy if provided
    session = requests.Session()
    if proxy:
        session.proxies = {"http": proxy, "https": proxy}
    
    for platform in search_platforms:
        try:
            finder = get_platform_finder(platform)
            url = finder.find_profile(normalized, session=session, timeout=timeout)
            if url:
                results[platform] = url
        except Exception as e:
            print(f"Error searching {platform}: {str(e)}")
    
    return results
>>>>>>> feature/proxy-support
```

### Pair Resolution Process

1. **Understand both changes**:
   - The current branch has better documentation and error logging
   - The feature branch adds proxy support and timeout parameters
   - Both have slightly different approaches to handling platforms

2. **Decide on a combined approach**:
   - Keep the better documentation from the current branch
   - Incorporate the new parameters and proxy support
   - Use the logger instead of print statements
   - Retain the more flexible platform handling

3. **Resolved version**:

```python
def search_social_profiles(username, platforms=None, timeout=30, proxy=None):
    """
    Search for user profiles across social platforms.
    
    Args:
        username: The username to search for
        platforms: List of platforms to search (default: all supported)
        timeout: Request timeout in seconds
        proxy: Optional proxy URL to use for requests
    
    Returns:
        Dictionary of platform -> profile URL
    """
    if not username:
        return {}
        
    results = {}
    normalized = normalize_username(username)
    
    # Use all platforms if none specified
    search_platforms = platforms or SUPPORTED_PLATFORMS
    
    # Set up session with proxy if provided
    session = requests.Session()
    if proxy:
        session.proxies = {"http": proxy, "https": proxy}
    
    for platform in search_platforms:
        if platform not in SUPPORTED_PLATFORMS:
            logger.warning(f"Unsupported platform: {platform}")
            continue
            
        try:
            finder = get_platform_finder(platform)
            url = finder.find_profile(normalized, session=session, timeout=timeout)
            if url:
                results[platform] = url
        except Exception as e:
            logger.error(f"Error searching {platform}: {e}")
    
    return results
```

## Common Conflict Scenarios and Solutions

### 1. Configuration and Constants Conflicts

**Scenario**: Two developers modify the same configuration constants.

**Collaborative approach**:
- Discuss the intention behind each change
- Understand which values are environment-specific vs. universal
- Consider refactoring to make constants more modular

### 2. Interface Changes

**Scenario**: One developer changes a function's parameters while another changes its return value.

**Collaborative approach**:
- Review function callers together to understand the impact
- Decide whether both changes can be accommodated
- Update documentation to reflect the new interface

### 3. Structural Conflicts

**Scenario**: Developers reorganize the same code area in different ways.

**Collaborative approach**:
- Draw diagrams of each approach
- Evaluate strengths and weaknesses together
- Choose one structure or create a hybrid approach

## Preventing Future Conflicts

After resolving conflicts, discuss how to prevent similar issues:

1. **Communicate about active work areas** in team meetings
2. **Create smaller, focused pull requests** that are easier to merge
3. **Merge from the main branch frequently** to reduce divergence
4. **Document complex code areas** that might need coordination
5. **Consider code ownership** for critical components

## Best Practices for OSINT Teams

For OSINT projects specifically:

1. **Be extra careful with data parsers** - Website scrapers often conflict
2. **Document API version dependencies** - Note which API version code works with
3. **Use feature flags** for experimental capabilities
4. **Maintain clear boundaries** between collection and analysis code
5. **Create tests that verify merged functionality** still works correctly

## Next Steps

Now that you understand how to handle merge conflicts collaboratively, you're prepared for effective team development. In the next module, we'll explore advanced topics that can further enhance your OSINT development toolkit.

Remember that merge conflicts are not a sign of failure but a natural part of collaborative development. Approaching them as an opportunity for communication and code improvement will make your team more effective in the long run.
