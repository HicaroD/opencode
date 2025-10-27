---
description: Execute implementation plan from conversation context
agent: build
model: anthropic/claude-haiku-4-5
---

Execute the implementation plan from the conversation context.

## Plan Loading

Find the most recent plan in conversation with:

- Section: "# IMPLEMENTATION PLAN:" or "# Implementation Plan:"
- Status: "READY_FOR_EXECUTION" or "PENDING"
- Tasks: TASK-1, TASK-2, etc.

**If no plan found:**

```
ERROR: No implementation plan found.
Run /plan first, then /execute.
```

Extract from plan:

- Technology stack and commands (build, test, lint)
- Task list with dependencies and acceptance criteria
- Execution order

## Execution Loop

For each task in dependency order:

### 1. Initialize Task

Display:

```
TASK-{n}: {title}
Type: {type} | Complexity: {complexity} | Files: {files}
Dependencies: {deps} | Status: EXECUTING
```

- Verify dependencies are COMPLETED
- Ask confirmation for XL complexity or OPTIONAL tasks

### 2. Implement by Type

**CREATE**: New files with structure, imports, docs, error handling
**MODIFY**: Precise changes preserving functionality and style
**DELETE**: Verify safety, remove files/code, clean references
**REFACTOR**: Preserve behavior, update tests if needed
**TEST**: Cover criteria, edge cases, follow conventions
**DOCUMENT**: Update README, API docs, examples, changelog

### 3. Verify

Check in order:

1. **Build**: Run build command, check compilation (if applicable)
2. **Quality**: Run linter, verify style consistency
3. **Tests**: Run test suite, verify pass/coverage
4. **Criteria**: Verify each acceptance criterion met

### 4. Complete or Handle Failure

**Success:**

```
TASK-{n}: COMPLETED
Files: {list} | Time: {duration}
```

**Failure:**

```
TASK-{n}: FAILED
Error: {description} | Files: {list}

Options: [RETRY|SKIP|ABORT]
```

- RETRY: Fix and try again
- SKIP: Mark SKIPPED, continue
- ABORT: Stop, preserve changes

### 5. Progress

Show after each task:

- `Tasks: {done}/{total} ({%}%)`
- Estimated time remaining
- Next task preview

## Execution Rules

1. **Sequential**: Execute in dependency order
2. **Atomic**: Complete fully before next
3. **Verify**: Always check before marking complete
4. **Safe**: Stop on critical failures
5. **Transparent**: Show changes and reasoning

## Error Recovery

When task fails:

1. **Analyze**: Auto-fixable? Missing context? Plan adjustment needed?
2. **Recover**: Auto-fix simple issues OR request help OR adjust plan OR rollback
3. **Communicate**: Explain clearly, suggest next steps

## Post-Execution Report

```
EXECUTION SUMMARY

Status: {COMPLETED|PARTIAL|FAILED}
Completed: {n}/{total} | Failed: {n} | Skipped: {n}
Time: {duration}

Files Created: {n}
  - {list}

Files Modified: {n}
  - {list}

Files Deleted: {n}
  - {list}

Build Status: {output}

Next Steps:
  - {actions}
  - {manual testing}
  - {review items}
```

## Final Verification

Run complete check:

1. Build: {command from plan}
2. Tests: {command from plan}
3. Lint: {command from plan}
4. Git: `git status`

## Rollback

If rollback requested:

1. Show what reverts
2. Confirm action
3. Execute: `git checkout -- {files}`
4. Verify success
