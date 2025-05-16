# 9.1 Semantic Versioning & Tags

Versioning your OSINT tools properly helps users understand changes between releases and manages expectations about compatibility. This lesson explains Semantic Versioning (SemVer) principles, how to use Git tags to mark releases, and how GitHub Releases can distribute your tools as downloadable packages.

## Understanding Semantic Versioning

Semantic Versioning (SemVer) is a versioning scheme that communicates meaning about the underlying changes in each release. A SemVer version number has three parts:

```
MAJOR.MINOR.PATCH
```

Each component has a specific meaning:

1. **MAJOR** - Incremented when you make incompatible API changes
2. **MINOR** - Incremented when you add functionality in a backward-compatible manner
3. **PATCH** - Incremented when you make backward-compatible bug fixes

### Examples of SemVer in Action

| Version Change | Meaning |
|----------------|---------|
| 1.0.0 → 1.0.1  | Bug fixes without changing functionality |
| 1.0.1 → 1.1.0  | New features added, everything still works with existing code |
| 1.1.0 → 2.0.0  | Breaking changes that will require users to modify their code |

### Pre-release and Build Metadata

SemVer also supports additional labels for pre-release versions and build metadata:

- **Pre-release**: Append a hyphen and identifiers (e.g., `1.0.0-alpha.1`)
- **Build metadata**: Append a plus sign and identifiers (e.g., `1.0.0+20130313144700`)

For OSINT tools, a typical version progression might look like:

```
0.1.0  - Initial development release
0.2.0  - Added new data source
0.2.1  - Fixed API rate limiting issue
1.0.0  - First stable release
1.1.0  - Added export functionality
2.0.0  - Restructured API for better extendability
```

## When to Increment Each Version Component

### Increment MAJOR when:
- Changing the function signature of public APIs
- Removing or renaming public functions or classes
- Changing how results are structured or returned
- Dropping support for a Python version
- Any change that would break existing scripts using your tool

### Increment MINOR when:
- Adding new functionality without breaking existing code
- Deprecating (but still supporting) a function or parameter
- Adding new data sources or collection methods
- Significant performance improvements
- Adding new output formats

### Increment PATCH when:
- Fixing bugs that don't change the API
- Improving error messages
- Updating documentation
- Making small performance optimizations
- Fixing typos in code or docs

## Using Git Tags for Versioning

Git tags allow you to mark specific commits in your repository's history, typically used to mark release points. 

### Creating a Version Tag

To tag the current commit with a version:

```bash
# Create a lightweight tag
git tag 1.0.0

# Create an annotated tag (recommended for releases)
git tag -a 1.0.0 -m "Version 1.0.0 - Initial stable release"
```

Annotated tags store extra metadata including:
- Tagger name and email
- Date of tagging
- A message explaining the tag

### Pushing Tags to GitHub

By default, `git push` doesn't transfer tags to remote repositories. To push tags:

```bash
# Push a specific tag
git push origin 1.0.0

# Push all tags
git push origin --tags
```

### Viewing Tags

To list all tags in your repository:

```bash
git tag
```

To see details of a specific tag:

```bash
git show 1.0.0
```

### Checking Out Tags

To examine code at a specific version:

```bash
git checkout 1.0.0
```

This puts your repository in a "detached HEAD" state at the tagged commit.

## Creating GitHub Releases

GitHub Releases expand on Git tags by allowing you to:
- Provide extensive release notes
- Attach binary files or compiled versions
- Automatically generate source code archives (zip/tar.gz)
- Create a user-friendly release page

### Creating a Release on GitHub

1. **Navigate to your repository** on GitHub
2. **Click "Releases"** in the right sidebar
3. **Click "Create a new release"** or "Draft a new release"
4. **Choose a tag** (existing or create new)
5. **Write a release title** (typically the version number)
6. **Add release notes** describing changes
7. **Optionally upload binary assets**
8. **Choose the release type** (regular release, pre-release, or draft)
9. **Click "Publish release"**

### Writing Effective Release Notes

Good release notes help users understand what's changed. Include:

1. **Summary** of the main changes
2. **New features** with brief explanation and examples
3. **Bug fixes** with issue references
4. **Breaking changes** with migration guidance
5. **Dependencies** that were updated
6. **Acknowledgments** to contributors

Example:

```markdown
## Twitter Collector 1.2.0

This release adds support for Twitter API v2 while maintaining compatibility
with existing code. Performance improvements reduce API rate limit usage.

### New Features
- Added support for Twitter API v2 endpoints
- Added `--format` parameter for CSV/JSON output options
- Improved rate limiting with automatic backoff

### Bug Fixes
- Fixed timezone handling in date filters (#42)
- Fixed encoding issues with non-Latin usernames (#45)
- Improved error messages for authentication failures

### Dependencies
- Updated requests to 2.28.0
- Added tweepy 4.10.0 as an optional dependency

### Contributors
Thanks to @username1 and @username2 for their contributions to this release!
```

## Automatic Archive Generation

When you create a GitHub Release, GitHub automatically generates:

1. **ZIP archive** of the source code at that tag
2. **TAR.GZ archive** of the source code at that tag

These archives are useful for users who:
- Want to download your tool without Git
- Need a specific version for reproducibility
- Are deploying in environments without Git access

The download URLs follow a predictable format:
```
https://github.com/username/repo/archive/refs/tags/VERSION.zip
https://github.com/username/repo/archive/refs/tags/VERSION.tar.gz
```

## Version Management for OSINT Projects

OSINT tools have some special versioning considerations:

1. **API changes** - External APIs frequently change, which may force version bumps
2. **Data format evolution** - How you handle changes in collected data formats
3. **Compatibility with intelligence platforms** - Version based on integration needs
4. **Security patches** - May require out-of-sequence releases

### Recommendations for OSINT Tool Versioning

1. **Start at 0.1.0** for initial development
2. **Move to 1.0.0** when your tool is stable and used in real investigations
3. **Use pre-release versions** for testing breaking changes (e.g., `2.0.0-beta.1`)
4. **Document version compatibility** with external APIs and services
5. **Include last compatible API version** in release notes

## Automating Version Management

For more mature projects, consider automating version management:

### Version in Code

Keep your version in a single location in your code:

```python
# src/your_package/__init__.py
__version__ = "1.2.3"
```

### Using bumpversion

The `bumpversion` tool helps manage version numbers:

```bash
# Install
pip install bumpversion

# Configure (creates .bumpversion.cfg)
bumpversion --init

# Bump patch version (1.2.3 → 1.2.4)
bumpversion patch

# Bump minor version (1.2.4 → 1.3.0)
bumpversion minor

# Bump major version (1.3.0 → 2.0.0)
bumpversion major
```

## Best Practices for Versioning OSINT Tools

1. **Be conservative with MAJOR versions** - Avoid breaking changes when possible
2. **Document breaking changes clearly** - Provide migration guides
3. **Test thoroughly before tagging** - Tags should point to stable commits
4. **Use consistent tag format** - Prefix with 'v' or not, but be consistent (e.g., `v1.0.0` or `1.0.0`)
5. **Sign your tags** if security is important:
   ```bash
   git tag -s 1.0.0 -m "Signed release 1.0.0"
   ```

## Next Steps

Now that you understand semantic versioning and Git tags, the next section will explore dependency management with Poetry or pip-tools. These tools help manage the libraries your project depends on, ensuring reproducible builds and easier deployment.
