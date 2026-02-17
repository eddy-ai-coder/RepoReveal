# Quick Start: Using AI Agents to Generate Documentation

This guide shows you how to use the AI agents and prompts in this repository to generate comprehensive documentation for any repository.

## Prerequisites

- Access to an AI coding assistant (GitHub Copilot, Claude, ChatGPT, etc.)
- The target repository cloned locally
- Basic understanding of the repository you're documenting

## Step-by-Step Process

### Step 1: Set Up Your Environment

1. Clone the target repository you want to document
2. Explore the codebase to understand its structure
3. Read the existing README and documentation

### Step 2: Use the Main Agent

Open `agents/doc-generator-agent.md` and provide it to your AI assistant as context. This sets the overall guidelines and objectives.

**Example prompt to your AI**:
```
I want to generate comprehensive documentation for the [REPO_NAME] repository.
Please follow the guidelines in doc-generator-agent.md.

Repository: [URL]
Purpose: [Brief description]
```

### Step 3: Generate Each Documentation Section

Use each prompt in `agents/prompts/` sequentially:

#### 3.1 Repository Overview
```
Use the prompt in agents/prompts/01-overview-prompt.md to analyze [REPO_NAME].

Please generate the repository overview section, including:
- Basic information
- Statistics
- Technology stack
- Use cases
```

#### 3.2 Architecture
```
Use the prompt in agents/prompts/02-architecture-prompt.md to document [REPO_NAME]'s architecture.

Focus on:
- High-level architecture diagram (Mermaid)
- Component breakdown
- Data flow
- Design patterns
```

#### 3.3 Technical Details
```
Use the prompt in agents/prompts/03-technical-details-prompt.md to document [REPO_NAME]'s implementation.

Cover:
- Directory structure
- Core modules
- Configuration
- Dependencies
```

#### 3.4 In-Depth Guide
```
Use the prompt in agents/prompts/04-indepth-guide-prompt.md to create practical guides for [REPO_NAME].

Include:
- Getting started
- Common use cases with examples
- Best practices
- Troubleshooting
```

### Step 4: Review and Refine

1. **Check completeness**: Ensure all sections are filled out
2. **Verify accuracy**: Cross-check technical details with the codebase
3. **Test examples**: Run any code examples to ensure they work
4. **Validate diagrams**: Make sure Mermaid diagrams render correctly
5. **Review links**: Verify all file paths and references are correct

### Step 5: Format and Finalize

1. Use the templates in `templates/` as a reference for structure
2. Compare your documentation with the example in `examples/sample-web-framework/`
3. Ensure consistent formatting throughout
4. Add cross-references between documentation files

## Tips for Best Results

### Working with AI Assistants

1. **Be specific**: Provide exact file paths and line numbers when asking about code
2. **Iterate**: Ask the AI to refine sections that aren't clear
3. **Validate**: Don't trust AI-generated content blindly - verify everything
4. **Context**: Give the AI access to key files from the repository

### Getting Quality Output

1. **Start broad, then narrow**: Begin with overview, then dive into details
2. **Use examples**: Point AI to similar well-documented repositories
3. **Ask for diagrams**: Explicitly request Mermaid diagrams for complex concepts
4. **Request code examples**: Always ask for working, runnable code samples

### Common Pitfalls to Avoid

- ❌ Don't skip verification - AI can make mistakes
- ❌ Don't document outdated versions - use the latest stable release
- ❌ Don't copy-paste without understanding - know what you're documenting
- ❌ Don't ignore existing docs - supplement, don't duplicate

## Example Workflow

Here's a complete example using GitHub Copilot Chat:

```
You: I want to document the 'axios' repository (https://github.com/axios/axios).
Here's the agent configuration: [paste doc-generator-agent.md content]

Please analyze the repository and tell me if you're ready to proceed.

AI: [Confirms understanding]

You: Great! Use the overview prompt [paste 01-overview-prompt.md] and generate
the repository overview section.

AI: [Generates overview]

You: Excellent! Now use the architecture prompt [paste 02-architecture-prompt.md]
and create the architecture documentation with Mermaid diagrams.

AI: [Generates architecture docs]

You: Perfect! Continue with technical details using [paste 03-technical-details-prompt.md]

AI: [Generates technical details]

You: Finally, create the in-depth guide using [paste 04-indepth-guide-prompt.md]

AI: [Generates guides]

You: Now review all sections and ensure they're complete and accurate.
```

## Advanced Techniques

### Combining Multiple Repositories

If documenting a related set of repositories:
1. Document each repository separately
2. Create a comparative analysis
3. Cross-reference related documentation

### Updating Documentation

When the repository changes:
1. Identify changed components
2. Re-run relevant prompts for those sections
3. Update diagrams if architecture changed
4. Verify examples still work

### Custom Prompts

Feel free to create custom prompts for specific needs:
- API reference documentation
- Performance benchmarks
- Security analysis
- Migration guides

## Validation Checklist

Before submitting your documentation:

- [ ] All 4 required files are present
- [ ] Mermaid diagrams render correctly
- [ ] Code examples have been tested
- [ ] File paths are accurate
- [ ] No placeholders remain (e.g., `{REPOSITORY_NAME}`)
- [ ] Cross-references between files work
- [ ] Formatting is consistent
- [ ] Spelling and grammar are correct
- [ ] Information is current and accurate

## Getting Help

If you encounter issues:

1. **Check the example**: Review `examples/sample-web-framework/` for reference
2. **Review templates**: Ensure you're following the template structure
3. **Ask in Discussions**: Post questions in the repository discussions
4. **Open an Issue**: Report problems or request clarification

## Resources

- **Agent Configuration**: `agents/doc-generator-agent.md`
- **Prompts**: `agents/prompts/01-*.md` through `04-*.md`
- **Templates**: `templates/`
- **Example**: `examples/sample-web-framework/`
- **Contributing Guide**: `CONTRIBUTING.md`

---

Happy documenting! 🚀
