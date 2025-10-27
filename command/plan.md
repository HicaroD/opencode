---
description: Create detailed implementation plan for any technology stack
agent: plan
model: anthropic/claude-sonnet-4-5
---

Create a comprehensive implementation plan for: $ARGUMENTS

## Project Context

Please provide or I will analyze:

1. Technology stack and framework
2. Project structure and conventions
3. Build and test commands
4. Code style and patterns used

Project structure:
!`find . -type f -name "*.json" -o -name "*.yaml" -o -name "*.toml" -o -name "*.md" | grep -E "(package|composer|requirements|Cargo|go.mod|pom|build|README)" | head -20`

Directory overview:
!`tree -L 2 -I 'node_modules|vendor|dist|build|target|bin|obj|coverage|.git|__pycache__|venv|env'`

Recent changes:
!`git log --oneline -5 2>/dev/null || echo "Not a git repository"`

## Planning Requirements

Based on the provided context and task, create a structured plan with:

### 1. Requirements Analysis

- Feature description and scope
- User stories or acceptance criteria
- Business rules and constraints
- Non-functional requirements (performance, security, etc.)

### 2. Technical Design

- High-level architecture approach
- Components/modules to create or modify
- Data structures and models
- Integration points and dependencies
- Design patterns applicable

### 3. Implementation Tasks

Create a numbered task list where each task is:

- **Atomic**: Can be completed independently
- **Clear**: Has explicit inputs and outputs
- **Testable**: Has verifiable completion criteria

Format each task as:

```
TASK-{number}: {Clear, action-oriented title}
Type: [CREATE|MODIFY|DELETE|REFACTOR|TEST|DOCUMENT]
Files: {list of file paths}
Description: {detailed what and how}
Acceptance Criteria:
  - {criterion 1}
  - {criterion 2}
Dependencies: [TASK-X, TASK-Y] or [None]
Complexity: [XS|S|M|L|XL]
Estimated Time: {rough estimate}
Status: PENDING
```

### 4. Dependency Graph

Show task dependencies and suggest optimal execution order:

```
TASK-1 (no deps)
  ├─> TASK-2 (depends on 1)
  └─> TASK-3 (depends on 1)
        └─> TASK-4 (depends on 3)
```

### 5. Risk Analysis

Identify potential issues:

- **Technical risks**: Complex implementations, unknowns
- **Breaking changes**: Impact on existing functionality
- **Performance considerations**: Bottlenecks, scalability
- **Security implications**: Vulnerabilities, data exposure
- **Mitigation strategies**: How to address each risk

### 6. Testing Strategy

Define testing approach:

- **Unit tests**: What needs unit test coverage
- **Integration tests**: System interaction tests needed
- **E2E tests**: User flow tests required
- **Manual testing**: Steps for manual verification
- **Test data**: Required fixtures or mock data

### 7. Rollback Strategy

- How to undo changes if something goes wrong
- Safe points for incremental rollback
- Database migration reversal (if applicable)

## Output Format

Output the plan directly in the conversation in a structured format that the `/execute` command can parse and follow.

The plan should be:

- **Self-contained**: All info needed for execution
- **Technology-specific**: Include actual commands for the project
- **Actionable**: Executor can follow without ambiguity
- **Traceable**: Each task has unique ID for tracking
- **In-context**: Stays in conversation history for `/execute` to reference

## Plan Structure Template

Use this exact structure so `/execute` can parse it:

```markdown
# IMPLEMENTATION PLAN: {Feature Name}

**Created**: {timestamp}
**Status**: READY_FOR_EXECUTION
**Technology Stack**: {detected or specified}
**Estimated Total Time**: {sum of task estimates}

---

## 1. OVERVIEW

{High-level description of what will be implemented}

## 2. REQUIREMENTS

{Detailed requirements and acceptance criteria}

## 3. TECHNICAL DESIGN

{Architecture approach, patterns, and design decisions}

## 4. TASKS

### TASK-1: {Clear, action-oriented title}

- **Type**: [CREATE|MODIFY|DELETE|REFACTOR|TEST|DOCUMENT]
- **Files**: {list of file paths}
- **Description**: {detailed what and how}
- **Acceptance Criteria**:
  - {criterion 1}
  - {criterion 2}
- **Dependencies**: [None] or [TASK-X, TASK-Y]
- **Complexity**: [XS|S|M|L|XL]
- **Estimated Time**: {estimate}

### TASK-2: {title}

{... repeat for all tasks ...}

## 5. EXECUTION ORDER

Based on dependencies:

1. TASK-1 (no dependencies)
2. TASK-2 (depends on TASK-1)
3. TASK-3, TASK-4 (can run after TASK-2)
4. TASK-5 (depends on TASK-3, TASK-4)

## 6. RISKS & MITIGATION

- **Risk 1**: {description} → Mitigation: {strategy}
- **Risk 2**: {description} → Mitigation: {strategy}

## 7. TESTING STRATEGY

- **Unit Tests**: {what needs coverage}
- **Integration Tests**: {what needs testing}
- **Manual Testing**: {steps to verify}

## 8. COMMANDS REFERENCE

- **Build**: {command to build project}
- **Test**: {command to run tests}
- **Lint**: {command to run linter}
- **Format**: {command to format code}
- **Run**: {command to run application}

## 9. ROLLBACK PLAN

{How to safely undo changes if needed}

---

**IMPORTANT**: This plan is now ready for execution.
To execute, simply run: `/execute`
The executor will read this plan from the conversation context.
```

## Additional Analysis

Before creating the plan, analyze:

- Existing codebase patterns and conventions
- Similar features already implemented
- Team coding standards (if documented)
- Configuration files for technology hints

Provide recommendations on:

- Code organization best practices for this stack
- Potential refactoring opportunities
- Technical debt considerations
