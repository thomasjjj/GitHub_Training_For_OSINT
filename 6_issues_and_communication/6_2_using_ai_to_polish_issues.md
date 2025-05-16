# 6.2 Using AI to Polish Issues

Artificial Intelligence tools like ChatGPT and GitHub Copilot Chat can help transform rough issue notes into clear, actionable descriptions. This lesson will show you how to leverage these AI assistants to create better GitHub issues with less effort.

## Why Use AI for Issue Creation

Even if you understand what makes a good issue, composing one can be time-consuming. AI assistants can help by:

1. **Improving clarity** - Restructuring jumbled thoughts into logical flows
2. **Enhancing completeness** - Prompting for missing information
3. **Refining language** - Suggesting clearer, more precise wording
4. **Maintaining consistency** - Following established patterns and templates
5. **Saving time** - Generating polished text from rough notes quickly

For OSINT professionals, this means less time documenting issues and more time on actual intelligence gathering and analysis.

## Available AI Tools for Issue Creation

### GitHub Copilot Chat

GitHub Copilot Chat is integrated directly into GitHub and optimized for development workflows. It can help with:

- Generating issue descriptions based on code or error messages
- Suggesting titles for issues
- Creating reproduction steps from verbal descriptions
- Analyzing error logs and suggesting solutions

### ChatGPT

ChatGPT is a versatile AI assistant available through OpenAI's website or API. Benefits include:

- Accessible without GitHub-specific subscriptions
- Useful for brainstorming and problem exploration
- Able to help with a wider range of content needs
- Multiple versions with different capabilities

### Other AI Tools

- **Claude** - Another AI assistant with strong writing capabilities
- **Bard** - Google's AI assistant
- **Microsoft Copilot** - Integrated into Microsoft products
- **IDE-integrated assistants** - Like the PyCharm AI Assistant

## Transforming Notes into Structured Issues

Let's look at the process of using AI to transform rough notes into polished issues.

### Step 1: Gather Your Initial Thoughts

Start by jotting down the key information about the issue, without worrying about structure or completeness:

```
twitter api keeps giving errors when searching for hashtags with special characters
tried things like #c++ and #c# and just get 403 errors
started happening yesterday, was working fine before
need to either fix or add some kind of escaping for these characters
```

### Step 2: Craft an Effective Prompt

The quality of the AI's response depends on your prompt. For issue creation, include:

1. **Context** - Tell the AI you're creating a GitHub issue
2. **Request** - Specify you want help formatting and improving your notes
3. **Template structure** - If applicable, mention the template to follow
4. **Specific needs** - Request any particular aspects to focus on

#### Example Prompt for ChatGPT

```
I'm creating a GitHub issue for an OSINT tool project. Please help me transform these rough notes into a well-structured issue that follows best practices. Include a clear title, description, steps to reproduce, and suggested solution:

"twitter api keeps giving errors when searching for hashtags with special characters
tried things like #c++ and #c# and just get 403 errors
started happening yesterday, was working fine before
need to either fix or add some kind of escaping for these characters"
```

#### Example Prompt for GitHub Copilot Chat

```
/createissue My Twitter API module is failing when searching hashtags with special characters like #c++ and #c#, resulting in 403 errors. This started yesterday and was working previously. We need to implement character escaping for these special cases.
```

### Step 3: Review and Refine the AI Output

AI tools typically provide a well-formatted issue like this:

```markdown
# Twitter API 403 Error When Searching Hashtags with Special Characters

## Description
The Twitter API integration is failing when searching for hashtags that contain special characters. Specifically, searches for hashtags like `#c++` and `#c#` are returning 403 Forbidden errors. This functionality was working correctly until yesterday.

## Steps to Reproduce
1. Configure API credentials
2. Attempt to search for hashtag `#c++` or `#c#`
3. Observe the 403 Forbidden error response

## Expected Behavior
The API should successfully search for hashtags containing special characters and return matching results.

## Actual Behavior
The API returns a 403 Forbidden error specifically when the hashtag contains special characters like `+` or `#`.

## Additional Information
- Issue started occurring: Yesterday (YYYY-MM-DD)
- Previously working: Yes
- Environment: [Please add your environment details]

## Possible Solution
Implement proper URL encoding/escaping for special characters in hashtags before sending requests to the Twitter API. Characters like `+`, `#`, and others likely need to be properly escaped according to Twitter's API requirements.
```

Always review the AI's output and:

1. **Verify accuracy** - Check that the AI hasn't introduced incorrect assumptions
2. **Add missing details** - Include environment information or other context
3. **Remove unnecessary content** - Delete irrelevant sections or AI-added disclaimers
4. **Personalize the tone** - Adjust to match your project's communication style

### Step 4: Iteratively Improve with Follow-up Prompts

If the first output isn't ideal, provide feedback to the AI:

```
That's a good start, but could you add more specific details about the Twitter API endpoint being used (v2 search API) and suggest we check for any recent Twitter API changes that might have affected this functionality?
```

## AI-Assisted Issue Enhancement Examples

Let's examine some before-and-after examples to see how AI can improve different types of issues.

### Example 1: Bug Report Enhancement

**Before (Raw Notes):**
```
pdf extractor crashes on large files. tried with 50MB report, got memory error. smaller ones work fine.
```

**After (AI-Enhanced):**
```markdown
# PDF Text Extractor Crashes with Memory Error on Large Files

## Description
The PDF text extraction module crashes when processing large PDF files, specifically throwing a memory error. This occurs with files around 50MB in size, while smaller files are processed successfully.

## Steps to Reproduce
1. Run the PDF extraction tool against a large (approximately 50MB) PDF file
2. Observe the tool crash with a memory error (specific error details below)

## Error Message
```
MemoryError: Unable to allocate 247.8 MiB for array with shape (259,842,034) and data type float64
```

## Environment
- PDF Extractor version: [version]
- Python version: [version]
- Operating system: [OS details]

## Possible Solutions
Consider implementing:
1. Chunked processing of large PDFs instead of loading entire files into memory
2. Memory usage optimization in the extraction algorithm
3. Clear error handling with user-friendly messages for files exceeding capabilities
```

### Example 2: Feature Request Enhancement

**Before (Raw Notes):**
```
need ability to export results as csv not just json. would help with excel analysis
```

**After (AI-Enhanced):**
```markdown
# Add CSV Export Option for Analysis Results

## Problem Statement
Currently, the tool only allows exporting results in JSON format, which limits interoperability with spreadsheet applications like Excel. Many analysts on our team need to perform further data analysis using Excel, and the current JSON-only export creates a workflow bottleneck.

## Proposed Solution
Add a CSV (Comma Separated Values) export option alongside the existing JSON export. This would allow direct import into Excel and other spreadsheet applications without requiring manual conversion or custom scripts.

## Implementation Details
- Add a new `--format` or `--output-format` parameter to the export command
- Support both `json` and `csv` as values (default remains `json` for backward compatibility)
- Ensure proper handling of nested data structures when flattening to CSV format
- Include headers in the CSV output for easy column identification

## User Story
As an OSINT analyst, I want to export my search results directly to CSV so that I can quickly analyze the data in Excel without additional conversion steps.

## Acceptance Criteria
- Command accepts `--format csv` parameter
- CSV files include appropriate headers
- All relevant data fields are included in the export
- Nested structures are appropriately flattened or expanded into multiple columns
- Documentation is updated to reflect the new export option
```

## Best Practices for AI-Assisted Issue Creation

### When to Use AI

AI assistance is most valuable when:

1. **You're short on time** but need a well-formatted issue
2. **The issue is complex** and needs clear organization
3. **English isn't your first language** and you want help with expression
4. **You're new to GitHub** and unfamiliar with issue conventions

### When to Avoid AI

Consider limiting AI use when:

1. **The issue contains sensitive information** you shouldn't share with AI services
2. **You're still gathering information** and don't have enough details yet
3. **The project has very specific, unique formatting requirements**
4. **You're working in a project that prohibits AI-generated content**

### Enhancing AI Output with Human Judgment

Always apply these human touches:

1. **Add personal context** that the AI wouldn't know
2. **Verify technical details** for accuracy
3. **Include project-specific terminology** where appropriate
4. **Mention relevant history** or previous related issues
5. **Sanitize any sensitive information** before posting

## OSINT-Specific AI Guidelines

When using AI for OSINT project issues:

1. **Redact sensitive targets** before submitting to AI tools
2. **Don't share actual API keys or credentials** in prompts
3. **Be mindful of revealing sources and methods** in public issues
4. **Consider data privacy implications** for any examples
5. **Verify AI doesn't add incorrect technical assumptions** about OSINT tools

## Next Steps

Once you've created a clear, well-structured issue using AI assistance, effective communication continues throughout the issue's lifecycle. In the next lesson, we'll cover best practices for commenting on issues and gracefully closing them when the work is complete.
