---
description: Execute implementation plan from conversation context
agent: build
model: anthropic/claude-haiku-4-5
---

Execute the implementation plan from the conversation above.

## Plan Loading

Look back in the conversation history to find the most recent message containing:

- A section starting with "# IMPLEMENTATION PLAN:" or "# Implementation Plan:"
- Status marked as "READY_FOR_EXECUTION" or "PENDING"
- Task list with TASK-1, TASK-2, etc.

If no plan is found in recent conversation:

```
ERROR: No implementation plan found in conversation context.
Please run /plan first to create a plan, then run /execute.
```

Parse the plan from conversation to extract:

- Technology stack
- Task list with all details
- Commands reference (build, test, lint, etc.)
- Execution order
- Acceptance criteria for each task

## Pre-Execution Checks

Before starting, verify:

1. **Environment readiness**

   - Required tools are available
   - Dependencies are installed
   - Working directory is clean: `git status --short 2>/dev/null || echo "Not a git repo"`

2. **Plan validity**

   - All tasks have clear descriptions
   - Dependencies form valid graph (no circular deps)
   - Commands reference is complete from plan

3. **Backup current state**
   - Current git status: `git status`
   - Suggest creating git commit or stash before proceeding

## Execution Loop

For each task in execution order:

### Step 1: Task Initialization

```
TASK-{number}: {title}
Type: {type}
Complexity: {complexity}
Files: {files}
Dependencies: {deps}
Status: EXECUTING
```

- Display full task details
- Verify dependencies marked as COMPLETED
- For XL complexity: Ask for user confirmation
- For OPTIONAL tasks: Ask if user wants to execute

### Step 2: Implementation

Execute based on task type:

**For CREATE tasks:**

- Create new files with appropriate structure
- Follow project conventions and patterns
- Add necessary imports/dependencies
- Include inline documentation
- Implement error handling

**For MODIFY tasks:**

- Load existing file(s)
- Make precise changes as specified
- Preserve existing functionality
- Maintain code style consistency
- Update related documentation

**For DELETE tasks:**

- Verify files/code are safe to remove
- Check for dependencies on what's being deleted
- Remove files or code sections
- Clean up related imports/references

**For REFACTOR tasks:**

- Analyze current implementation
- Apply refactoring while preserving behavior
- Update tests if needed
- Ensure no functionality breaks

**For TEST tasks:**

- Create or update test files
- Cover all acceptance criteria
- Include edge cases and error scenarios
- Follow testing conventions of the project

**For DOCUMENT tasks:**

- Update README, API docs, or inline comments
- Add usage examples
- Document configuration options
- Update changelog if applicable

### Step 3: Verification

After implementation, verify:

1. **Syntax and compilation**

   - Run project build command from plan
   - Check for compilation errors
   - Verify no syntax issues

2. **Code quality**

   - Run linter if specified in plan
   - Check for code style violations
   - Verify formatting is consistent

3. **Tests**

   - Run test suite using command from plan
   - Verify existing tests still pass
   - Check new tests are passing
   - Review test coverage if available

4. **Acceptance criteria**
   - Verify each criterion from task is met
   - Test functionality manually if needed
   - Check edge cases work correctly

### Step 4: Task Completion

If verification passes:

```
TASK-{number}: COMPLETED
Files modified: {list}
Time taken: {duration}
```

Mark task as completed in memory and proceed to next task.

If verification fails:

```
TASK-{number}: FAILED
Error: {error description}
Files affected: {list}
```

**Failure handling:**

- Document the error clearly
- Suggest potential fixes
- Ask user: [RETRY|SKIP|ABORT]
  - RETRY: Try implementation again with fixes
  - SKIP: Mark as SKIPPED, continue to next task
  - ABORT: Stop execution, preserve changes made

### Step 5: Progress Update

After each task:

- Show progress: `Tasks: {completed}/{total} ({percentage}%)`
- Display estimated time remaining
- List next pending task

## Execution Rules

Follow these principles:

1. **Sequential execution**: Execute tasks in dependency order
2. **Atomicity**: Complete each task fully before moving to next
3. **Verification**: Always verify before marking complete
4. **Documentation**: Update plan file with actual results
5. **Safety**: Stop on critical failures, ask on warnings
6. **Transparency**: Show what's being changed and why

## Error Recovery

If a task fails:

1. **Analyze the error**

   - Determine if it's fixable automatically
   - Check if it's due to missing context
   - Identify if plan needs adjustment

2. **Recovery options**

   - Auto-fix: Simple issues (imports, syntax)
   - Request help: Need user input or clarification
   - Adjust plan: Task needs to be split or modified
   - Rollback: Undo changes and restart task

3. **Communication**
   - Explain what went wrong clearly
   - Suggest concrete next steps
   - Ask for user decision when needed

## Post-Execution Report

After all tasks completed (or stopped):

```

EXECUTION SUMMARY

Status: {COMPLETED|PARTIAL|FAILED}
Tasks Completed: {count}/{total}
Tasks Failed: {count}
Tasks Skipped: {count}
Total Time: {duration}

Files Created: {count}
  - {list of files}

Files Modified: {count}
  - {list of files}

Files Deleted: {count}
  - {list of files}

Build Status:
  {output of build command}

Next Steps:
  - {recommended next actions}
  - {manual testing needed}
  - {documentation to review}
```

## Final Verification

Run complete verification:

1. Full build: {build command from plan}
2. Full test suite: {test command from plan}
3. Linter: {lint command from plan}
4. Git status: !`git status`

Suggest commit message if all passed:

```
feat: {feature name from plan}

Implemented:
- {task 1 summary}
- {task 2 summary}
...

Tasks completed: {count}
Tests: All passing
```

## Rollback Instructions

If user wants to rollback:

1. Show what would be reverted
2. Confirm before executing
3. Use git to restore: `git checkout -- {files}`
4. Or restore from stash: `git stash pop`
5. Verify rollback was successful

## Execution Summary

The execution report serves as the final record in the conversation:

- All tasks attempted and their outcomes
- Files changed during execution
- Test results and verification status
- Time taken and any issues encountered
- Recommendations for next steps

This summary becomes part of the conversation history and can be referenced later.
