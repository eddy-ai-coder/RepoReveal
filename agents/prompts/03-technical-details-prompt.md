# Technical Details Analysis Prompt

## Context
You are documenting the technical implementation details of: `{REPOSITORY_NAME}`

## Task
Create detailed technical documentation covering code structure, key implementations, and configuration.

## Instructions

1. **Directory Structure**
   - Create a tree view of important directories
   - Explain the purpose of each major directory
   - Note any special organization patterns

2. **Core Modules**
   - Identify 5-10 most important modules/files
   - Document their purpose and functionality
   - Show key functions/classes with signatures
   - Provide usage examples

3. **Key Algorithms**
   - Identify important algorithms used
   - Explain their purpose and complexity
   - Document input/output formats
   - Show pseudocode or simplified examples

4. **Configuration**
   - List all configuration options
   - Document environment variables
   - Explain configuration files
   - Show example configurations

5. **Dependencies**
   - List major dependencies with versions
   - Explain why each dependency is needed
   - Note any critical or security-sensitive dependencies
   - Document dependency installation

6. **Build and Deployment**
   - Build process and commands
   - Deployment options
   - Environment requirements
   - Performance considerations

## Output Format
```markdown
# Technical Details

## Directory Structure
\`\`\`
/
├── src/
│   ├── core/          # Core functionality
│   ├── utils/         # Utility functions
│   └── config/        # Configuration
├── tests/             # Test files
└── docs/              # Documentation
\`\`\`

## Core Modules

### Module: {module_name}
**Location**: `path/to/module`
**Purpose**: [Description]

**Key Functions**:
\`\`\`language
function functionName(param1: type): returnType
\`\`\`

**Usage Example**:
\`\`\`language
// Example code
\`\`\`

## Key Algorithms

### Algorithm: {name}
- **Purpose**: ...
- **Time Complexity**: O(...)
- **Space Complexity**: O(...)
- **Implementation**: ...

## Configuration

### Environment Variables
| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| VAR_NAME | Yes/No   | value   | Description |

### Configuration Files
- **File**: `config.json`
  - Purpose: ...
  - Format: ...
  - Example: ...

## Dependencies

### Production Dependencies
- `package-name` (version): Description
- ...

### Development Dependencies
- `package-name` (version): Description
- ...

## Build Process
\`\`\`bash
# Build commands
\`\`\`

## Deployment
[Deployment instructions and considerations]
```

## Guidelines
- Include code snippets with proper syntax highlighting
- Reference specific files with relative paths
- Document actual values and examples
- Focus on information not in README
