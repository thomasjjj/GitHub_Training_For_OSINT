# 8.3 Using GitHub Projects & Discussions

Beyond code and issues, GitHub offers powerful collaboration tools for project management and knowledge sharing. This lesson covers how to use GitHub Projects to track tasks with Kanban boards and GitHub Discussions to create a knowledge base separate from issues.

## GitHub Projects: Task Tracking Made Simple

GitHub Projects provides flexible project management tools directly integrated with your repository. The latest version, Projects (beta), offers improvements over the classic Projects view.

### Why Use GitHub Projects?

Projects offer several advantages for OSINT teams:

1. **Visibility** - See all tasks and their status at a glance
2. **Integration** - Direct connection to issues and pull requests
3. **Flexibility** - Customize views and workflows to match your team's needs
4. **Automation** - Automatically update task status based on activity
5. **Documentation** - Create a record of project progress

### Creating Your First Project Board

To set up a project board:

1. **Navigate to your organization or repository**
2. **Click on the "Projects" tab**
3. **Click "New project"**
4. **Select "Board" as the template**
5. **Name your project** (e.g., "OSINT Tool Development")
6. **Click "Create"**

You'll now have a basic Kanban board with three columns:
- **Todo** - Tasks that haven't been started
- **In progress** - Work that's actively being done
- **Done** - Completed tasks

### Customizing Your Project Board

Projects are highly customizable to fit your workflow:

1. **Add custom columns** by clicking the "+" button:
   - "Backlog" for future tasks
   - "Review" for items waiting for review
   - "Blocked" for tasks that can't proceed

2. **Customize fields** by clicking "..." menu → "Manage fields":
   - Add "Priority" field (High, Medium, Low)
   - Add "Effort" estimation (Small, Medium, Large)
   - Add "Type" (Bug, Feature, Research)

3. **Create custom views** by clicking "New view":
   - Table view for detailed information
   - Gantt chart for timeline visualization
   - Calendar view for scheduling

### Adding Items to Your Project

There are several ways to add items to your project:

1. **Create a new item** directly on the board:
   - Click "+" button in a column
   - Enter a title and optional details
   - These become "draft" items that can be converted to issues later

2. **Add existing issues**:
   - Click "Add items" on your project
   - Search for and select issues from your repositories
   - The issues will appear in your project view

3. **Convert draft items to issues**:
   - Click on a draft item
   - Click "Convert to issue"
   - Select the repository where the issue should be created

### Setting Up Automation

Automate routine project management tasks:

1. **Go to project settings** (click "..." menu → "Settings")
2. **Click "Workflows" in the sidebar**
3. **Set up built-in workflows**:
   - Move newly added items to "Todo"
   - Move items with new pull requests to "In progress"
   - Move closed issues and merged pull requests to "Done"

4. **Create custom workflows** to match your team's process:
   - Move issues with "blocked" label to "Blocked" column
   - Assign priority based on labels
   - Set due dates for specific types of tasks

### Using Projects for OSINT Investigations

For OSINT teams, project boards can track both development and investigation tasks:

1. **Investigation tracking**:
   - Columns for "Leads", "Researching", "Verifying", "Confirmed", "Documented"
   - Custom fields for source reliability and confidence levels
   - Classification or sensitivity markings

2. **Tool development tracking**:
   - Columns for traditional software development workflow
   - Custom fields for associated data sources or techniques
   - Priority based on investigation needs

### Project Views for Different Team Members

Different team members may need different views of the same project:

1. **For developers**:
   - Board view filtered to code-related tasks
   - Grouped by component or repository

2. **For analysts**:
   - Table view with detailed fields
   - Grouped by investigation or target

3. **For managers**:
   - Timeline view showing deadlines
   - Summary view with status counts

To create custom views:
1. Click "New view" button
2. Select the view type
3. Configure filters and grouping
4. Name the view appropriately

### Best Practices for Projects

1. **Keep boards focused** - Create separate boards for distinct workflows
2. **Use consistent naming** - Establish conventions for items and fields
3. **Clean up regularly** - Archive completed items to maintain visibility
4. **Link related items** - Use mentions to connect related work
5. **Document your process** - Add a README to your project explaining the workflow

## GitHub Discussions: Building a Knowledge Base

GitHub Discussions provides a forum-like space for questions, ideas, and conversations that don't fit within issues or pull requests.

### Why Use GitHub Discussions?

Discussions offer advantages over issues for certain types of communication:

1. **Persistent knowledge base** - Long-lived resource unrelated to specific tasks
2. **Community engagement** - More conversational format for team interaction
3. **Structured categories** - Organize topics by purpose or subject
4. **Marked solutions** - Highlight answers to common questions
5. **Separation of concerns** - Keep general discussions apart from actionable issues

### Enabling Discussions for Your Repository

To activate GitHub Discussions:

1. **Go to your repository**
2. **Click "Settings"**
3. **Scroll down to "Features"**
4. **Check the box next to "Discussions"**
5. **Return to your repository and click the "Discussions" tab**

### Setting Up Discussion Categories

Customize categories to organize different types of conversations:

1. **Click "Get started with discussions"** (first time) or **"New discussion"**
2. **Click "Categories" in the left sidebar**
3. **Click "New category"**
4. **Configure each category**:
   - Name and emoji icon
   - Description explaining its purpose
   - Format (Discussion, Q&A, Announcement)
   - Who can create discussions in this category

For OSINT projects, consider these categories:

1. **Announcements** - Important team updates
2. **Techniques & Methods** - Discussion of OSINT approaches
3. **Tool Usage Q&A** - Help with using the team's tools
4. **Data Sources** - Information about APIs and websites
5. **Ethics & Legal** - Discussions about boundaries and compliance
6. **General** - Miscellaneous conversations

### Starting a New Discussion

To begin a discussion:

1. **Click "New discussion"**
2. **Select the appropriate category**
3. **Add a clear title**
4. **Write your initial post using Markdown**
5. **Click "Start discussion"**

### Using Q&A Format Effectively

Q&A-format discussions have special features:

1. **Marking answers as solutions**:
   - Discussion creator or maintainers can mark a reply as the solution
   - Solutions appear at the top for easy reference

2. **Following up on answers**:
   - Reply threads allow detailed follow-up
   - Complex questions can have multiple solutions for different aspects

Example OSINT Q&A discussion:

```
Title: Best approaches for archiving social media posts?

Body:
We need to archive social media posts for later analysis while maintaining metadata. 
What are the best tools and approaches the team has used for this?

Specific requirements:
- Must capture timestamps and user information
- Should preserve images/videos when possible
- Needs to work with Twitter, Facebook, and Instagram
- Should generate evidence-grade archives

What has worked well for others on the team?
```

### Converting Between Issues and Discussions

Sometimes a discussion reveals an actionable task, or an issue turns into a broader conversation:

1. **Convert issue to discussion**:
   - Open the issue
   - Click "..." menu
   - Select "Convert to discussion"
   - Choose the appropriate category

2. **Create issue from discussion**:
   - When a specific action item is identified
   - Use the discussion link in the issue for context
   - Focus the issue on the specific task

### Building a Knowledge Base with Pinned Discussions

Pin important discussions to make them easily accessible:

1. **Find an important discussion** with valuable information
2. **Click "..." menu**
3. **Select "Pin discussion"**

Good candidates for pinning include:
- Onboarding information for new team members
- Standard operating procedures for investigations
- Commonly referenced resources and tools
- Important policy decisions

### Using Discussions for OSINT Knowledge Sharing

For OSINT teams, discussions can catalog valuable knowledge:

1. **Data source documentation**:
   - Access methods and authentication requirements
   - Known limitations and restrictions
   - Example queries and responses

2. **Tool usage guides**:
   - Step-by-step instructions
   - Configuration options
   - Troubleshooting help

3. **Technique sharing**:
   - Novel approaches to common problems
   - Workflow documentation
   - Case studies (with appropriate privacy protection)

### Integration with Projects and Issues

Link discussions to other GitHub elements:

1. **Reference discussions in issues**:
   - Link to relevant knowledge when creating tasks
   - Cite established practices in pull requests

2. **Add discussion links to project items**:
   - Include knowledge base references in project cards
   - Create custom fields for related discussions

3. **Create issues from actionable points**:
   - When a discussion identifies necessary work
   - Link back to the original discussion for context

## Combining Projects and Discussions for OSINT Workflows

For maximum effectiveness, use both tools together:

### Example: New Investigation Workflow

1. **Create a discussion** outlining the investigation goals and parameters
2. **Start a project board** with the investigation stages
3. **Create issues** for specific research tasks
4. **Link issues to the project** for tracking
5. **Document findings in discussions** by category
6. **Update the project** as tasks progress
7. **Summarize results** in a concluding discussion

### Example: Tool Development Cycle

1. **Start discussions** about required capabilities
2. **Create a project board** for feature development
3. **Convert specific feature requests to issues**
4. **Track development progress** on the board
5. **Document usage guidelines** in discussions
6. **Link completed features** to their documentation

## Security Considerations

When using Projects and Discussions for sensitive OSINT work:

1. **Be mindful of public vs. private repositories**:
   - Projects and discussions inherit repository visibility
   - Private repositories keep content restricted to members

2. **Consider information compartmentalization**:
   - Use code names for sensitive subjects
   - Keep personally identifiable information out of discussions
   - Reference investigation numbers rather than specific details in project boards

3. **Document security practices**:
   - Create a pinned discussion outlining security requirements
   - Include security review in project workflows
   - Define what information can be shared in which locations

## Next Steps

With GitHub Projects and Discussions, you now have powerful tools to organize work and share knowledge. In the next section, we'll learn how to handle merge conflicts collaboratively, ensuring smooth integration of code changes even when multiple team members modify the same files.
