# In-Depth Guide Generation Prompt

## Context
You are creating in-depth guides for: `{REPOSITORY_NAME}`

## Task
Generate comprehensive guides that go beyond basic documentation to help developers deeply understand and use the repository.

## Instructions

1. **Getting Started Guide**
   - Prerequisites and system requirements
   - Step-by-step installation
   - Initial configuration
   - First-run tutorial
   - Verification steps

2. **Common Use Cases**
   - Identify 3-5 common scenarios
   - Provide complete, working examples
   - Explain each step
   - Show expected output
   - Include troubleshooting

3. **Advanced Features**
   - Document advanced capabilities
   - Provide real-world use cases
   - Show configuration options
   - Explain trade-offs and best practices

4. **Customization and Extension**
   - Plugin/extension mechanisms
   - Custom configuration examples
   - Adding new features
   - Integration with other tools

5. **Best Practices**
   - Performance optimization tips
   - Security considerations
   - Error handling patterns
   - Testing strategies
   - Code organization recommendations

6. **Troubleshooting Guide**
   - Common errors and solutions
   - Debugging techniques
   - Log analysis
   - Performance issues
   - FAQ

7. **Migration and Upgrade**
   - Version compatibility
   - Migration guides
   - Breaking changes
   - Upgrade procedures

## Output Format
```markdown
# In-Depth Guides

## Getting Started

### Prerequisites
- Requirement 1
- Requirement 2

### Installation
\`\`\`bash
# Step-by-step commands
\`\`\`

### Quick Start
1. Step 1
2. Step 2

## Common Use Cases

### Use Case 1: {Title}
**Scenario**: [Description]

**Implementation**:
\`\`\`language
// Complete working example
\`\`\`

**Explanation**:
[Step-by-step explanation]

**Output**:
\`\`\`
Expected output
\`\`\`

## Advanced Features

### Feature: {name}
- **Purpose**: ...
- **When to use**: ...
- **Configuration**: ...
- **Example**: ...

## Best Practices

### {Category}
- ✅ Do: ...
- ❌ Don't: ...
- 💡 Tip: ...

## Troubleshooting

### Error: {error message}
**Cause**: ...
**Solution**: ...
\`\`\`bash
# Fix commands
\`\`\`

## FAQ

**Q: {Question}**
A: {Answer}
```

## Guidelines
- Provide complete, runnable examples
- Include actual commands and code
- Show both good and bad practices
- Address common pain points
- Link to related sections
- Use clear, actionable language
