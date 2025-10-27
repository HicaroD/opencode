---
description: Create detailed implementation plan for any technology stack
agent: plan
model: anthropic/claude-sonnet-4-5
---

Create a comprehensive implementation plan for:

$ARGUMENTS

## Analysis Phase

Analyze the project:

- Technology stack and framework
- Project structure and conventions
- Fundamental commands (Build/test)
- Code style and patterns

## Plan Structure

### 1. Overview & Requirements

- Feature scope and acceptance criteria
- Business rules and constraints
- Non-functional requirements

### 2. Technical Design

- Architecture approach
- Components to create/modify
- Data models and integration points
- Applicable design patterns

### 3. Tasks

Format each task as:

```
TASK-{n}: {Action-oriented title}
Type: CREATE|MODIFY|DELETE|REFACTOR|TEST|DOCUMENT
Files: {file paths}
Description: {what and how}
Acceptance Criteria: {testable criteria}
Dependencies: [TASK-X] or [None]
Complexity: XS|S|M|L|XL
Status: PENDING
```

### 4. Execution Order

Show dependency graph and optimal sequence.

### 5. Risk Analysis

- Technical risks and unknowns
- Breaking changes impact
- Performance/security considerations
- Mitigation strategies

### 6. Testing Strategy

- Unit/Integration/E2E test requirements
- Manual verification steps
- Required test data

### 7. Rollback Plan

How to safely revert changes if needed.

## Output Template

```markdown
# IMPLEMENTATION PLAN: {Feature Name}

**Status**: READY_FOR_EXECUTION
**Stack**: {technology}
**Est. Time**: {total}

## 1. OVERVIEW

{What will be implemented}

## 2. REQUIREMENTS

{Acceptance criteria}

## 3. DESIGN

{Architecture and patterns}

## 4. TASKS

### TASK-1: {Title}

- **Type**: {type}
- **Files**: {paths}
- **Description**: {details}
- **Acceptance**: {criteria}
- **Dependencies**: {tasks}
- **Complexity**: {size}

## 5. EXECUTION ORDER

1. TASK-1
2. TASK-2 (→ TASK-1)
3. TASK-3, TASK-4 (→ TASK-2)

## 6. RISKS

- {Risk} → {Mitigation}

## 7. TESTING

- **Unit**: {coverage}
- **Integration**: {tests}
- **Manual**: {steps}

## 8. COMMANDS

- Build: {cmd}
- Test: {cmd}
- Run: {cmd}

## 9. ROLLBACK

{Undo strategy}

---

**Ready for execution with `/execute`**
```

## Best Practices

- Make tasks atomic and testable
- Include actual commands for the stack
- Keep plan self-contained
- Analyze existing codebase patterns
- Stay in conversation for `/execute` parsing
