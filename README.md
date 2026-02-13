# Claude PRD Workflow

Turn any Software Requirements Specification (SRS) into a working product using Claude Code.

---

## Overview

This workflow transforms your product idea into structured development sessions that Claude can execute. It handles everything from specification to implementation, including design integration with Figma.

```
Your Idea → /spec → /figma:tokens → /figma:components → /sessions → Working Product
```

---

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI installed
- Figma MCP (optional, for design integration)
- Context7 MCP (recommended, for up-to-date documentation)

---

## Quick Start

### 1. Define your stack

Create `.claude/docs/stack.md` with your tech choices:

```markdown
# Stack

## Platform
Next.js 14 (App Router)

## Database
Supabase (PostgreSQL)

## Auth
Supabase Auth

## UI
Tailwind CSS + shadcn/ui

## Hosting
Vercel

## Services
- Resend (email)

## Testing
Playwright
```

### 2. Run the workflow

```bash
# Step 1: Create project specification from your SRS/idea
/spec

# Step 2: Extract design tokens from Figma (optional)
/figma:tokens

# Step 3: Extract components from Figma (optional)
/figma:components

# Step 4: Generate development sessions
/sessions
```

---

## Workflow Steps

### Step 1: `/spec` - Create Specification

**Input:** Your product idea, SRS document, or feature description

**Output:** `.claude/docs/projectSpec/projectSpec.md`

This skill analyzes your requirements and creates a structured specification including:
- Features and user stories
- Pages and their interactions
- Data models
- API endpoints

**Example:**
```
/spec

"I want to build a SaaS dashboard for tracking customer subscriptions.
Users can sign up, view their subscription status, upgrade/downgrade plans,
and see billing history."
```

---

### Step 2: `/figma:tokens` - Design Tokens (Optional)

**Input:** Figma file URL

**Output:** Design tokens configuration

Extracts colors, typography, spacing, and other design tokens from your Figma file to ensure visual consistency.

**Skip this step if:** You don't have Figma designs or will use default styling.

---

### Step 3: `/figma:components` - Extract Components (Optional)

**Input:** Figma file URL

**Output:** Component specifications

Identifies reusable components in your Figma designs that will be implemented during development.

**Skip this step if:** You don't have Figma designs.

---

### Step 4: `/sessions` - Generate Development Sessions

**Input:**
- `.claude/docs/stack.md`
- `.claude/docs/projectSpec/projectSpec.md`

**Output:**
- `.claude/sessions/session-XX-name.md` (one per session)
- `.claude/sessions/state.json` (progress tracking)

This skill creates atomic, ordered development sessions. Each session:
- Has one clear objective
- Lists required tools (MCPs/Skills)
- Includes specific tasks
- Defines completion criteria

**Example output:**
```
.claude/sessions/
├── state.json
├── session-01-project-setup.md
├── session-02-auth-setup.md
├── session-03-database-schema.md
├── session-04-dashboard-page.md
└── session-05-billing-integration.md
```

---

## After Generating Sessions

### Complete Design Information

If your sessions require design, complete the placeholders:

```markdown
## Design
Page: /dashboard
Figma URL: [TO_ADD]        ← Add your Figma URL here
Components: [TO_ADD]       ← Add required components here

Interactions:
- Click export → opens modal
```

### Start Working

Pick a session and tell Claude to execute it:

```
Work on session-01-project-setup
```

Claude will:
1. Update `state.json` to `in_progress`
2. Create tasks for each item
3. Execute the tasks
4. Run code-simplifier when done
5. Update `state.json` to `completed`

---

## File Structure

```
your-project/
├── .claude/
│   ├── docs/
│   │   ├── stack.md                    # Your tech stack
│   │   └── projectSpec/
│   │       └── projectSpec.md          # Generated specification
│   ├── sessions/
│   │   ├── state.json                  # Progress tracking
│   │   ├── session-01-xxx.md
│   │   ├── session-02-xxx.md
│   │   └── ...
│   └── commands/
│       ├── spec.md                     # /spec skill
│       └── sessions.md                 # /sessions skill
└── README.md
```

---

## Session Structure

Each session file follows this structure:

```markdown
# Session [N]: [Title]

## Tools
- context7 (MCP) - Documentation
- shadcn-ui (Skill) - Component patterns

## Objective
Clear, specific goal for this session.

## Scope
What's included and excluded.

## Prerequisites
- Previous sessions that must be completed
- Required env vars or API keys


## Design (if applicable)
Page: /page-path
Figma URL: https://figma.com/...
Components: Button, Card, Modal

Interactions:
- Non-obvious behavior → result

→ Use: /figma:implement-design

## Completion Criteria
- [ ] All tasks done
- [ ] Tests passing
- [ ] Specific acceptance criteria

## Post-Session
1. Run `/code-simplifier` on modified files
2. Update state.json to mark session completed
```



**Status values:**
- `pending` - Not started
- `in_progress` - Currently working
- `completed` - Done

---

## Recommended Tools

The workflow recommends tools based on your stack:

| Technology | Tool | Type | Purpose |
|------------|------|------|---------|
| Next.js | context7 | MCP | Up-to-date docs |
| Supabase | context7 | MCP | Up-to-date docs |
| shadcn/ui | shadcn-ui | Skill | Component patterns |
| Playwright | playwright | MCP | Browser testing |
| Figma | figma | MCP | Design implementation |

**MCP** = Connects to external services
**Skill** = Provides knowledge and best practices

---

## Tips

1. **Start with a clear SRS** - The better your input, the better the output
2. **Review generated sessions** - Adjust scope if sessions are too large
3. **Complete design info early** - Add Figma URLs before starting design sessions
4. **Work sequentially** - Sessions are ordered by dependencies
5. **Use state.json** - Check progress across conversations

---

## Example: Full Workflow

```bash
# 1. Set up your stack
# Edit .claude/docs/stack.md with your tech choices

# 2. Create specification
/spec
"Build a task management app with teams, projects, and real-time updates"

# 3. (Optional) Extract design tokens
/figma:tokens
# Provide Figma URL when prompted

# 4. (Optional) Extract components
/figma:components
# Provide Figma URL when prompted

# 5. Generate sessions
/sessions

# 6. Review generated sessions in .claude/sessions/

# 7. Complete Figma URLs and Components in design sessions

# 8. Start working
"Work on session-01-project-setup"

# 9. Continue with next sessions
"Work on session-02-auth-setup"
```

---

## Troubleshooting

**Sessions not generating?**
- Ensure `.claude/docs/stack.md` exists
- Ensure `.claude/docs/projectSpec/projectSpec.md` exists (run `/spec` first)

**Missing tools warning?**
- Warnings are informational, not blocking
- Install recommended MCPs for better results

**Design section incomplete?**
- Add Figma URL and Components manually after running `/figma:components`

