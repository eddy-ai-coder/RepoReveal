# Repository Overview Analysis Prompt

## Context
You are analyzing the repository: `{REPOSITORY_NAME}` located at `{REPOSITORY_URL}`

## Task
Generate a comprehensive repository overview document.

## Instructions

1. **Basic Information**
   - Extract the repository name, description, and primary language
   - Document the main purpose and goals of the project
   - List key features and capabilities
   - Note the license type

2. **Repository Statistics**
   - Number of stars, forks, and watchers
   - Number of contributors
   - Last update date
   - Issue and PR activity levels

3. **Technology Stack**
   - Programming languages used (with percentages)
   - Major frameworks and libraries
   - Build tools and development dependencies
   - Database and storage solutions
   - External services and APIs

4. **Use Cases**
   - Primary use cases
   - Target audience
   - Real-world applications
   - Notable users or implementations

5. **Quick Summary**
   - One-paragraph executive summary
   - Key differentiators from similar projects
   - Maturity level (experimental, stable, production-ready)

## Output Format
```markdown
# {Repository Name}

## Overview
[Project description and purpose]

## Key Features
- Feature 1
- Feature 2
- ...

## Technology Stack
- **Languages**: ...
- **Frameworks**: ...
- **Tools**: ...

## Use Cases
[Description of main use cases]

## Statistics
- ⭐ Stars: X
- 🔀 Forks: Y
- 👥 Contributors: Z
- 📅 Last Updated: ...

## Quick Summary
[One paragraph summary]
```

## Notes
- Focus on what makes this repository unique
- Highlight innovative approaches or solutions
- Be objective and factual
