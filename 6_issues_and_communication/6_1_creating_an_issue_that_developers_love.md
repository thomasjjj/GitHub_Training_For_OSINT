# 6.1 Creating an Issue That Developers Love

Issues are the primary way to track bugs, feature requests, and other tasks in GitHub repositories. Well-written issues help developers understand problems quickly, prioritize effectively, and implement solutions correctly. This lesson will teach you how to create issues that developers appreciate and can act on efficiently.

## The Purpose of GitHub Issues

GitHub Issues serve several important functions:

1. **Bug tracking** - Document errors, unexpected behaviors, or crashes
2. **Feature requests** - Propose new functionality or improvements
3. **Task management** - Track work that needs to be done
4. **Discussion forum** - Discuss implementations or design decisions
5. **Documentation** - Create a searchable record of problems and solutions

For OSINT projects, issues help track data collection challenges, API integration problems, and feature requests for new intelligence sources.

## The "One Problem Per Issue" Rule

The most important principle for effective issues: **focus on a single, clearly defined problem or request in each issue.**

### Why One Problem Per Issue?

1. **Easier to assign** - Different team members may handle different aspects
2. **Clearer progress tracking** - Each issue can be in only one state (open/closed)
3. **Focused discussion** - Conversations stay on topic
4. **Bite-sized work** - Developers can make progress in small increments
5. **Better searchability** - Future team members can find specific solutions

### Examples of Splitting Issues

**Bad (Multiple Problems):**
"Search functionality is broken and we also need to add Twitter integration"

**Good (Split into Two Issues):**
- Issue 1: "Search functionality returns no results for exact matches"
- Issue 2: "Add Twitter as a data source for social media monitoring"

## Using Issue Templates

Many repositories provide issue templates to ensure necessary information is included. If available, always use these templates.

### Creating Your Own Templates

For your OSINT repositories, consider creating templates for common issue types:

#### Bug Report Template

```markdown
---
name: Bug report
about: Report an error or unexpected behavior
---

## Bug Description
[Clear, concise description of the bug]

## Steps to Reproduce
1. [First step]
2. [Second step]
3. [And so on...]

## Expected Behavior
[What you expected to happen]

## Actual Behavior
[What actually happened]

## Environment
- OS: [e.g., Windows 10, macOS Monterey]
- Browser/Python version: [e.g., Chrome 96, Python 3.9]
- Tool version: [e.g., v1.2.0 or commit hash]

## Additional Context
[Any other relevant information, such as when it started occurring]
```

#### Feature Request Template

```markdown
---
name: Feature request
about: Suggest a new feature or enhancement
---

## Problem Statement
[Describe the problem this feature would solve]

## Proposed Solution
[Describe your proposed feature or enhancement]

## Alternative Solutions
[Any alternative solutions you've considered]

## OSINT Value
[How this feature would enhance OSINT capabilities]

## Additional Context
[Any other relevant information or screenshots]
```

## Writing Clear Issue Titles

The title is the first thing developers see, so make it count:

### Title Guidelines

1. **Be specific** - Include key information for quick understanding
2. **Be concise** - Aim for 5-10 words that capture the essence
3. **Include context** - Add prefixes like [API], [UI], or [Doc] if helpful
4. **Use actionable wording** - Start with verbs for feature requests

### Examples of Good vs. Bad Titles

**Bad:** "Search doesn't work"  
**Good:** "Search returns no results for exact phrase matches in quotation marks"

**Bad:** "Add more sources"  
**Good:** "Add Mastodon as a social media data source"

**Bad:** "Error message"  
**Good:** "[API] 403 Forbidden error when accessing Twitter API with valid credentials"

## Providing Crucial Context

The body of your issue should provide all the information a developer needs to understand and solve the problem.

### Essential Elements for Bug Reports

1. **Clear description** of what's happening
2. **Precise steps to reproduce** the issue
3. **Expected vs. actual behavior**
4. **Environment details** (OS, browser, versions)
5. **When it started** happening (if known)
6. **Impact level** - How severely does this affect users?

### Essential Elements for Feature Requests

1. **Problem statement** - What need exists?
2. **Proposed solution** - How would you solve it?
3. **User stories** - Who benefits and how?
4. **Acceptance criteria** - How will we know when it's done correctly?
5. **OSINT value** - How does this improve intelligence gathering capabilities?

## Including Visual Evidence

Visual evidence dramatically improves issue comprehension:

### Screenshots

- Capture the entire relevant window, not just a small portion
- Use arrows or circles to highlight the specific area of concern
- Blur or redact any sensitive information

### Screen Recordings

For issues that involve a sequence of events or are hard to reproduce:

- Keep recordings short (under 30 seconds if possible)
- Focus on the specific issue
- Add narration in the issue description about what to look for
- Consider GIFs for simple interactions

### Example Code

When relevant, include minimal code examples:

```python
# This results in the error:
search_results = client.search("example phrase")
# TypeError: client.search() takes 1 positional argument but 2 were given
```

## Including Logs and Error Messages

Logs and error messages provide crucial diagnostic information:

1. **Use code blocks** for proper formatting:
   ```
   Traceback (most recent call last):
     File "osint_tool.py", line 42, in <module>
       results = api.search(query)
   ApiError: Rate limit exceeded
   ```

2. **Include relevant context** before and after the error

3. **Remove sensitive information** like API keys or personal data

4. **Format as plain text** (not screenshots of text) to allow searching

## OSINT-Specific Considerations

When creating issues for OSINT projects:

1. **Sanitize data** - Remove personal information, investigation details, or sensitive targets
2. **Be cautious with API information** - Don't include API keys or access details
3. **Consider operational security** - Avoid revealing investigative techniques in public repositories
4. **Document data source changes** - Note when APIs or websites change that affect collection

## Best Practices Checklist

Before submitting an issue, verify:

✅ Issue addresses a single problem or feature request  
✅ Title is clear, specific, and describes the issue well  
✅ Description includes all relevant details  
✅ Steps to reproduce are clear and complete (for bugs)  
✅ Screenshots or screen recordings are included if relevant  
✅ Logs or error messages are formatted properly  
✅ Sensitive information has been removed  
✅ Environment details are provided  
✅ Impact or value is clearly stated  

## Next Steps

Once you've mastered creating clear, detailed issues, you can enhance them further using AI tools to polish your descriptions. In the next section, we'll explore how to use AI assistants like ChatGPT or GitHub Copilot Chat to transform rough notes into polished, actionable issue descriptions.
