# Architecture Analysis Prompt

## Context
You are analyzing the architecture of: `{REPOSITORY_NAME}`

## Task
Create comprehensive architecture documentation including diagrams and component descriptions.

## Instructions

1. **High-Level Architecture**
   - Create a Mermaid diagram showing the main components
   - Describe the overall architecture pattern (MVC, microservices, layered, etc.)
   - Explain the rationale behind the architecture choices

2. **Component Breakdown**
   - List all major components/modules
   - Describe the responsibility of each component
   - Show dependencies between components
   - Identify public interfaces

3. **Data Flow**
   - Create data flow diagrams for key operations
   - Show how data moves through the system
   - Identify data transformations
   - Document data storage and persistence

4. **Design Patterns**
   - Identify design patterns used (Factory, Singleton, Observer, etc.)
   - Explain why these patterns were chosen
   - Show examples of pattern implementation

5. **System Boundaries**
   - External dependencies and integrations
   - API endpoints and interfaces
   - Third-party services
   - Data sources and sinks

## Output Format
```markdown
# Architecture

## Overview
[High-level architecture description]

## Architecture Diagram
\`\`\`mermaid
graph TD
    A[Component A] --> B[Component B]
    B --> C[Component C]
\`\`\`

## Components

### Component Name
- **Purpose**: ...
- **Responsibilities**: ...
- **Dependencies**: ...
- **Key Files**: ...

## Data Flow

### Operation: {Operation Name}
\`\`\`mermaid
sequenceDiagram
    participant A
    participant B
    A->>B: Request
    B->>A: Response
\`\`\`

## Design Patterns

### Pattern Name
- **Type**: ...
- **Purpose**: ...
- **Implementation**: ...
- **Location**: ...

## External Integrations
- Integration 1: Description
- Integration 2: Description
```

## Guidelines
- Use Mermaid syntax for diagrams
- Keep diagrams clear and not overly complex
- Focus on the most important architectural aspects
- Provide file/directory references for each component
