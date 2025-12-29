# Project Management Rules

This directory contains Cursor rules for managing software projects using a spec-driven development approach, inspired by [Claude Code PM (ccpm)](https://github.com/automazeio/ccpm).

## Overview

These rules provide a complete workflow for:
- Creating Product Requirements Documents (PRDs)
- Managing epics and breaking down features
- Generating and tracking implementation tasks
- Executing work in parallel when possible
- Preserving context across sessions
- Maintaining full traceability from requirements to code
- Tracking project status and progress

## Workflow

The complete workflow follows this sequence:

```
1. Create PRD → 2. Create Epic → 3. Generate Tasks → 4. Execute (Parallel) → 5. Track Status
```

## Rules Documentation

### Core Workflow Rules

#### 1. **create-prd.mdc**
Creates Product Requirements Documents from user prompts.

**Usage**: Mention this rule when you want to create a new PRD for a feature.

**Process**:
- AI asks clarifying questions
- Generates structured PRD
- Saves to `/tasks/prd-[feature-name].md`

**Key Sections**:
- Introduction/Overview
- Goals
- User Stories
- Functional Requirements
- Non-Goals
- Success Metrics

#### 2. **epic-management.mdc**
Manages epics (large features) and their relationship to PRDs and tasks.

**Usage**: Mention this rule when creating or managing an epic.

**Process**:
- Create epic file from PRD
- Break epic into tasks
- Track epic status and progress
- Maintain epic context

**Epic Lifecycle**:
1. Epic Creation (from PRD)
2. Epic Breakdown (into tasks)
3. Epic Tracking (status updates)
4. Epic Completion (all tasks done)

#### 3. **generate-tasks.mdc**
Generates detailed task lists from PRDs.

**Usage**: Mention this rule when you have a PRD and want to create implementation tasks.

**Process**:
- Phase 1: Generate high-level parent tasks
- Wait for user confirmation
- Phase 2: Generate detailed sub-tasks
- Identify relevant files
- Save to `/tasks/tasks-[prd-name].md`

**Output Format**:
- Relevant Files section
- Hierarchical task list (parent → sub-tasks)

#### 4. **process-task-list.mdc**
Guidelines for executing tasks from task lists.

**Usage**: Automatically applied when working with task lists.

**Key Rules**:
- One sub-task at a time
- Mark completed tasks immediately
- Update task list as you work
- Wait for user approval before next task

### Advanced Features

#### 5. **parallel-execution.mdc**
Identifies tasks that can be executed in parallel.

**Usage**: Mention this rule when analyzing tasks for parallelization opportunities.

**Key Concepts**:
- One task ≠ One work stream
- Tasks can split into parallel streams (DB, API, UI, Tests)
- Mark tasks with `[parallel: true]`
- Respect dependencies

**Benefits**:
- 3-5x faster execution for well-structured tasks
- Better context management
- Reduced blocking

#### 6. **context-preservation.mdc**
Preserves project context across sessions and epics.

**Usage**: Mention this rule when setting up context management or when context is getting lost.

**Context Storage**:
- Epic-level: `.claude/context/[epic-name]/`
- Task-level: In task files
- Project-level: `CLAUDE.md`, `README.md`

**Protocol**:
- Load context before starting work
- Update context during work
- Finalize context after completing work

#### 7. **traceability.mdc**
Maintains full traceability from requirements to implementation.

**Usage**: Mention this rule when you need to track or verify traceability.

**Traceability Chain**:
```
PRD → Epic → Task → Issue (optional) → Code → Commit → Deployment
```

**Key Practices**:
- Explicit bidirectional links
- Reference epic/task in commit messages
- Update traceability as work progresses
- Verify chain completeness

#### 8. **project-status-tracking.mdc**
Tracks project status and generates status reports.

**Usage**: Mention this rule when you need status updates or reports.

**Status Levels**:
- Task status: `not-started`, `in-progress`, `blocked`, `review`, `completed`
- Epic status: `planning`, `ready`, `in-progress`, `blocked`, `review`, `completed`
- Project status: Aggregated metrics

**Report Types**:
- Daily standup reports
- Epic status reports
- Project dashboard

## Quick Start Guide

### Starting a New Feature

1. **Create PRD**:
   ```
   "Use create-prd.mdc to create a PRD for [feature name]"
   ```

2. **Create Epic**:
   ```
   "Use epic-management.mdc to create an epic from the PRD"
   ```

3. **Generate Tasks**:
   ```
   "Use generate-tasks.mdc to create tasks from prd-[feature-name].md"
   ```

4. **Analyze for Parallelization**:
   ```
   "Use parallel-execution.mdc to analyze tasks for parallel execution"
   ```

5. **Start Implementation**:
   ```
   "Start working on the first task from tasks-[name].md"
   ```
   (process-task-list.mdc will automatically apply)

### During Development

- **Update Status**: Tasks and epics update automatically as you work
- **Preserve Context**: Context is maintained in epic/task files
- **Track Progress**: Use project-status-tracking.mdc for reports

### Completing Work

- **Update Traceability**: Ensure all links are maintained
- **Finalize Context**: Update context files with final decisions
- **Generate Status Report**: Create completion report

## File Structure

```
/tasks/
  prd-[feature-name].md          # Product Requirements Document
  epic-[feature-name].md          # Epic tracking file
  tasks-[feature-name].md         # Task list file

.claude/
  context/
    [epic-name]/
      requirements.md             # Epic requirements
      decisions.md                 # Important decisions
      architecture.md              # Technical architecture
      dependencies.md              # Dependencies
      progress.md                  # Current progress
```

## Best Practices

### 1. Start with PRD
Always create a PRD before starting implementation. It provides clarity and prevents scope creep.

### 2. Break Down Properly
- PRD → Epic (large feature)
- Epic → Tasks (implementation units)
- Tasks → Sub-tasks (granular work)

### 3. Maintain Links
Keep explicit links between PRD, Epic, Tasks, and Code for full traceability.

### 4. Update Regularly
Update status and context files as work progresses, not just at the end.

### 5. Identify Parallelization
Look for opportunities to execute work in parallel to speed up delivery.

### 6. Preserve Context
Don't lose important decisions or context. Document them immediately.

### 7. Track Status
Regular status updates help identify blockers and maintain visibility.

## Integration with Existing Rules

These project management rules work alongside other Cursor rules:

- **Code Quality Rules**: Applied during implementation
- **Security Rules**: Considered during design and implementation
- **Documentation Rules**: Used for creating user documentation
- **Language-Specific Rules**: Applied based on the tech stack

## Example Workflow

```markdown
# Example: Implementing User Authentication

1. Create PRD
   → "Use create-prd.mdc for user authentication feature"
   → Creates: /tasks/prd-user-authentication.md

2. Create Epic
   → "Use epic-management.mdc to create epic from prd-user-authentication.md"
   → Creates: /tasks/epic-user-authentication.md

3. Generate Tasks
   → "Use generate-tasks.mdc from prd-user-authentication.md"
   → Creates: /tasks/tasks-user-authentication.md
   → Tasks: Database, Service, API, UI, Tests

4. Analyze Parallelization
   → "Use parallel-execution.mdc to analyze tasks-user-authentication.md"
   → Identifies: Database + Service can run parallel to UI design

5. Execute Tasks
   → Work through tasks one by one
   → Mark completed as you go
   → Update epic status

6. Track Progress
   → "Use project-status-tracking.mdc for epic status"
   → Generate status report

7. Complete
   → All tasks marked complete
   → Epic marked complete
   → Context finalized
   → Traceability verified
```

## Tips

- **Be Specific**: When mentioning rules, be specific about which one to use
- **One at a Time**: Focus on one phase at a time (PRD → Epic → Tasks → Execute)
- **Update Often**: Don't defer updates - do them as you work
- **Ask for Help**: If unsure which rule to use, ask for clarification
- **Review Regularly**: Periodically review traceability and status

## References

- Inspired by: [Claude Code PM (ccpm)](https://github.com/automazeio/ccpm)
- Original ccpm documentation: https://github.com/automazeio/ccpm

## Support

If you need help using these rules:
1. Check the specific rule file for detailed guidelines
2. Review the example workflow above
3. Ask the AI assistant to explain a specific rule

---

**Remember**: These rules are tools to help you work more effectively. Adapt them to your specific needs and project requirements.
