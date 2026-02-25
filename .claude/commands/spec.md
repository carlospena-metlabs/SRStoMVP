# AI-SRS Architect

Transform PRD documents into a projectSpec optimized for AI agent consumption.

---

## Inputs

- **PRD**: `.claude/docs/prd/*.md`
- **Stack**: `.claude/docs/stack.md` (read for context, do NOT include in output)

---

## Outputs

1. `.claude/docs/projectSpec/projectSpec.md`
2. `.env.example` (project root)

---

## Core Principle

> Include only what agents CANNOT infer from stack.md or data model.

---

## Pre-check (STOP & ASK)

### If stack.md does NOT exist:

STOP. Ask user to confirm:
- Frontend (Next.js, React Native, Vue...)
- Backend/BaaS (Supabase, Firebase, Node...)
- Database (PostgreSQL, MongoDB...)
- Third-party services (Stripe, Resend, OpenAI...)

Generate `.claude/docs/stack.md` first, then proceed.

### If stack.md exists:

Read PRD and identify ambiguities (max 5 questions). If any exist, ASK before generating:
- Unclear user flows
- Missing business rules
- Undefined edge cases
- Ambiguous permissions

If no ambiguities, proceed directly.

---

## Workflow

1. Run Pre-check
2. Read stack.md and all PRD files
3. Generate projectSpec.md
4. Generate .env.example

---

## projectSpec.md Structure

### Product Overview

2-3 lines. What it is, for whom, what problem it solves. No marketing.

---

### Pages & Routes

For each route, define auth, data, and interactions together:

#### /route-path
| Auth | Data |
|------|------|
| Protected | entity1, entity2 |

**Interactions:**
- Action → System consequence → Integration (if any) → Error handling
- Action → System consequence → Error handling

Example:

#### /checkout
| Auth | Data |
|------|------|
| Protected | cart, user |

**Interactions:**
- Click "Pay" → Create order record → Stripe.createPaymentIntent() → Error: Toast "Card declined"
- Click "Apply coupon" → Validate coupon in DB → Error: Toast "Invalid coupon"

---

#### /dashboard
| Auth | Data |
|------|------|
| Protected | user, projects |

**Interactions:**
- Click "New project" → Insert project → Redirect /project/[id]

---

Notes:
- **Auth**: Public / Protected / Admin
- **Data**: Entity names from Data Model
- **Interactions**: Only include non-obvious flows (skip basic navigation, standard CRUD)

---

### Data Model

#### EntityName
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| owner_id | uuid | FK → User |
| status | enum | values: draft, published |

Types: uuid, string, number, boolean, timestamp, date, enum, json
Constraints: PK, FK → Table, required, unique, optional, default: value

---

### Business Logic

#### Permissions

Only non-obvious rules:

| Action | Rule |
|--------|------|
| Delete project | Owner only |
| Invite member | Owner + admins |

#### Constraints

| Constraint |
|------------|
| Max 10 projects per user |
| Task title: 1-200 chars |

---

### Open Questions

Ambiguities that could not be resolved during Pre-check:

1. **[Topic]**: [Question]

If none: "No unresolved ambiguities."

---

## .env.example

Generate from stack.md services:

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=

# Resend
RESEND_API_KEY=
```

---

## Do NOT Include

- Stack details (in stack.md)
- Standard auth/CRUD flows
- UI/UX details
- Folder structure
- Marketing language
