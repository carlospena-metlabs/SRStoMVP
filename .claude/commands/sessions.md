Analyze the project documentation and generate a development session plan.

---

## Workflow Context

This skill is part of a larger workflow:

1. `/spec` → Creates project specification
2. `/figma:tokens` → Extracts design tokens
3. `/figma:components` → Extracts components from Figma
4. **`/sessions`** → Creates development sessions (this skill)
5. Complete `Figma URL` and `Components` in sessions that require design

---

## Input Files (MANDATORY)

### 1. Stack Configuration
```
.claude/docs/stack.md
```
Read this to understand what technologies the project uses.

### 2. Project Specification
```
.claude/docs/projectSpec/projectSpec.md
```
Read this to understand what to build. This file is created by `/spec`.

---

## Phase 1: Analyze Stack & Recommend Tools

Before creating sessions, analyze the stack and recommend useful tools.

### Step 1: Read stack.md
Identify all technologies and services used in the project.

### Step 2: Generate dynamic tool recommendations
For each technology found, determine if a tool would be beneficial:

**Tool Types:**
- **MCP**: Connects to external services (APIs, browsers, design tools)
- **Skill**: Provides knowledge, patterns, and best practices

**Common mappings (use as reference, adapt to actual stack):**

| Technology | Tool | Type | Benefit |
|------------|------|------|---------|
| Next.js | context7 | MCP | Up-to-date documentation |
| React | context7 | MCP | Up-to-date documentation |
| Supabase | context7 | MCP | Up-to-date documentation |
| Prisma | context7 | MCP | Up-to-date documentation |
| Stripe | context7 | MCP | Up-to-date documentation |
| shadcn/ui | shadcn-ui | Skill | Component patterns and best practices |
| Tailwind CSS | context7 | MCP | Up-to-date documentation |
| Playwright | playwright | MCP | Browser automation for tests |
| Figma | figma | MCP | Direct design implementation |

### Step 3: Show recommendations (non-blocking)

Display recommendations before generating sessions:

```markdown
## Recommended Tools (Optional)

Based on your stack:

| Technology | Tool | Type | Benefit |
|------------|------|------|---------|
| [tech] | [tool] | [MCP/Skill] | [benefit] |

💡 You can continue without installing them. They will be used if available.
```

**DO NOT block the process.** Continue to Phase 2 regardless of installed tools.

---

## Phase 2: Session Generation

Generate development sessions based on the project specification.

### Session Structure

For each session, include:

```markdown
# Session [N]: [Title]

## Tools
- context7 (MCP) - Next.js, Supabase docs
- shadcn-ui (Skill) - Component patterns
- playwright (MCP) - Testing

## Objective
[Clear, specific goal]

## Scope
[What is included / excluded]

## Prerequisites
- Sessions that must be completed first
- External dependencies (env vars, API keys)

## Tasks
1. [ ] Task description
2. [ ] Task description
3. [ ] Task description

## Design (only if this session requires design)
Page: [page path]
Figma URL: [TO_ADD]
Components: [TO_ADD]

Interactions: (only non-intuitive behaviors from spec)
- [interaction] → [result]
- [interaction] → [result]

> Complete `Figma URL` and `Components` after running `/figma:tokens` and `/figma:components`

→ Use: /figma:implement-design

## Completion Criteria
- [ ] Functional test with Playwright
- [ ] [Specific acceptance criteria]
- [ ] [Specific acceptance criteria]

## Post-Session
After completing all tasks:
1. Run `/code-simplifier` on modified files
2. Update state.json to mark session completed
```

### Session Principles

1. **Tools explicit**: Every session lists which tools to use (with type)
2. **Self-contained**: Each session is independently executable
3. **Testable**: Every session has completion criteria
4. **Ordered**: Sessions follow logical dependency order
5. **Atomic**: One clear objective per session

---

## Phase 3: Output

### Session Files
Save each session to:
```
.claude/sessions/session-[NN]-[slug].md
```

Example:
```
.claude/sessions/session-01-auth-setup.md
.claude/sessions/session-02-database-schema.md
.claude/sessions/session-03-dashboard-page.md
```

### State File
Create a state tracking file:
```
.claude/sessions/state.json
```

Content:
```json
{
  "sessions": {
    "01-auth-setup": {
      "status": "pending"
    },
    "02-database-schema": {
      "status": "pending"
    },
    "03-dashboard-page": {
      "status": "pending"
    }
  }
}
```

**Status values:**
- `pending` - Not started
- `in_progress` - Currently working on it
- `completed` - All tasks done

---

## Phase 4: Working on a Session

When starting work on a session:

### Step 1: Update state
Set session status to `in_progress` in `state.json`.

### Step 2: Create tasks
Use `TaskCreate` for each task in the session:
```
TaskCreate: "Task description"
  - activeForm: "Working on [task]..."
```

### Step 3: Work through tasks
- Set task to `in_progress` when starting
- Set task to `completed` when done

### Step 4: Complete session
When all tasks are `completed`:
1. Run `/code-simplifier` on modified files
2. Update `state.json`: set session status to `completed`

---

## Execution Order

1. Read stack.md
2. Read projectSpec.md
3. Analyze stack and show tool recommendations (non-blocking)
4. Generate sessions
5. Create state.json
