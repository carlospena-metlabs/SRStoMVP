# projectSpec.md

## Product Overview

APPCHULA is a private institutional capital management platform for Family Offices, VCs, and high-net-worth investors. Users are pre-qualified externally; the platform provides portfolio visibility, yield tracking, and controlled withdrawal management. All critical operations require admin validation.

---

## Pages & Routes

### / (redirect)
| Auth | Data |
|------|------|
| Public | - |

**Interactions:**
- Unauthenticated → Redirect /login
- Authenticated → Redirect /dashboard

---

### /login
| Auth | Data |
|------|------|
| Public | - |

**Interactions:**
- Submit credentials → Validate → If new device/location: trigger 2FA via email (OTP expires 10 min) → Error: Toast "Invalid credentials"
- Click "Forgot password" → Redirect /forgot-password

---

### /forgot-password
| Auth | Data |
|------|------|
| Public | - |

**Interactions:**
- Submit email → Send reset link (24h expiry) → Success: Toast "Check your email"

---

### /reset-password/[token]
| Auth | Data |
|------|------|
| Public | - |

**Interactions:**
- Submit new password → Validate token → Update password → Invalidate all sessions → Redirect /login → Error: Toast "Invalid or expired link"

---

### /activate/[token]
| Auth | Data |
|------|------|
| Public | - |

**Interactions:**
- Submit password → Validate token (24h default) → Activate account → Redirect /login → Error: Toast "Invalid or expired invitation"

---

### /dashboard
| Auth | Data |
|------|------|
| Protected | user, balance, yields, withdrawals, agent |

**Interactions:**
- View financial summary: capital, balance, accumulated yield (USD, 2 decimals)
- View balance evolution chart (monthly, closed months only)
- View yield history table (month, %, amount, resulting balance)
- View withdrawal history (date, amount, status, execution date)
- Click "Request withdrawal" → Validate window + 10-day advance + 3-month tenure → Open withdrawal modal → Error: Toast "Withdrawal window closed" or "Minimum tenure not met"
- Export yields/withdrawals → Download CSV/Excel

---

### /dashboard/withdrawal/new
| Auth | Data |
|------|------|
| Protected | user, balance |

**Interactions:**
- Select partial/total → If partial: enter amount → Trigger 2FA → Submit → Create withdrawal (status: requested) → Redirect /dashboard → Error: Toast "Insufficient balance"

---

### /profile
| Auth | Data |
|------|------|
| Protected | user |

**Interactions:**
- View personal info (read-only)
- Click "Change password" → Trigger 2FA → Submit → Update password → Invalidate sessions

---

### /backoffice
| Auth | Data |
|------|------|
| Admin/Operator | - |

**Interactions:**
- Redirect to /backoffice/users

---

### /backoffice/users
| Auth | Data |
|------|------|
| Admin/Operator | users |

**Interactions:**
- View user list with filters (type, status, agent)
- Click user → Navigate /backoffice/users/[id]
- Admin: Click "New user" → Navigate /backoffice/users/new

---

### /backoffice/users/new
| Auth | Data |
|------|------|
| Admin | agents, superAgents |

**Interactions:**
- Fill user form (email, name, type, role, agent assignment, capital, guaranteed %) → Save → Generate activation token → Send invitation email → Redirect /backoffice/users/[id]

---

### /backoffice/users/[id]
| Auth | Data |
|------|------|
| Admin/Operator | user, balance, yields, withdrawals, commissions, notes, auditLogs |

**Interactions:**
- View complete user profile and financial data
- Admin: Edit user info → Confirm → Log change
- Admin: Adjust capital → Trigger 2FA → Confirm → Log change
- Admin: Block/unblock user → Confirm → Log change
- Admin: Resend invitation / Copy activation link
- Admin: Invalidate previous invitation → Generate new token
- Operator/Admin: Add internal note → Save with author + timestamp
- View audit log for this user

---

### /backoffice/users/[id]/withdrawals
| Auth | Data |
|------|------|
| Admin/Operator | withdrawals |

**Interactions:**
- View user's withdrawal requests
- Admin: Approve withdrawal → Log change
- Admin: Mark as executed → Trigger 2FA → Deduct from balance → Log change

---

### /backoffice/withdrawals
| Auth | Data |
|------|------|
| Admin/Operator | withdrawals |

**Interactions:**
- View all withdrawal requests with filters (status, date, user)
- Admin: Bulk approve selected
- Admin: Mark individual as executed

---

### /backoffice/monthly-close
| Auth | Data |
|------|------|
| Admin | users, balances |

**Interactions:**
- Select month/year → Enter yield % → Calculate preview for all eligible users
- View validation list: user, previous balance, applied %, yield amount, new balance
- Toggle individual users (pre-selected by default)
- Exclude users below 50k USD capital
- Apply guaranteed minimum % where applicable (user's guaranteed % if higher than entered %)
- Apply 50% yield for mid-month entries (capital entered day 1-14)
- Click "Execute close" → Trigger 2FA → Update balances → Generate yield records → Log → Success: Toast "Monthly close completed"

---

### /backoffice/commissions
| Auth | Data |
|------|------|
| Admin/Operator | commissions, agents, superAgents |

**Interactions:**
- View commission summary by agent/superAgent
- View pending agent commissions (accumulated, awaiting quarter-end)
- Admin: Execute quarterly agent payout → Validate Super Agent balance sufficient → If insufficient: block payout, show warning → If sufficient: credit Agent balance, debit Super Agent balance → Log both operations

---

### /backoffice/commissions/superagent/[id]
| Auth | Data |
|------|------|
| Admin/Operator | superAgent, clients, commissions |

**Interactions:**
- View Super Agent's monthly commission breakdown
- View associated clients (direct + via agents)
- Admin: Trigger monthly commission accrual → Calculate on previous month's client balances (clients >1 month tenure) → Credit to Super Agent balance → Log

---

### /backoffice/commissions/agent/[id]
| Auth | Data |
|------|------|
| Admin/Operator | agent, clients, commissions |

**Interactions:**
- View Agent's accumulated commissions
- View direct clients and their contribution
- View payout history (quarterly)

---

### /backoffice/audit
| Auth | Data |
|------|------|
| Admin | auditLogs |

**Interactions:**
- View all system logs with filters (action type, user, date range)
- Export logs

---

## Data Model

### User
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| email | string | required, unique |
| password_hash | string | required |
| name | string | required |
| phone | string | optional |
| type | enum | values: client, agent, superAgent |
| role | enum | values: none, operator, admin |
| status | enum | values: active, blocked, unverified |
| agent_id | uuid | FK → User, optional (for clients) |
| super_agent_id | uuid | FK → User, optional (for agents) |
| guaranteed_yield_pct | enum | values: 2.5, 3.25, 4.25, 5.0, required for clients |
| capital | decimal | required, default: 0 |
| balance | decimal | required, default: 0 |
| yield_start_date | date | optional (when yields begin) |
| kyc_status | enum | values: pending, verified, reserved for future |
| kyc_provider | string | optional, reserved for future |
| kyc_verified_at | timestamp | optional |
| created_at | timestamp | required |
| updated_at | timestamp | required |

### Yield
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User, required |
| month | number | required (1-12) |
| year | number | required |
| percentage | decimal | required |
| amount | decimal | required |
| balance_before | decimal | required |
| balance_after | decimal | required |
| is_prorated | boolean | default: false |
| created_at | timestamp | required |
| created_by | uuid | FK → User (admin) |

### Withdrawal
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User, required |
| amount | decimal | required |
| type | enum | values: partial, total |
| status | enum | values: requested, approved, executed |
| requested_at | timestamp | required |
| approved_at | timestamp | optional |
| approved_by | uuid | FK → User, optional |
| executed_at | timestamp | optional |
| executed_by | uuid | FK → User, optional |
| window_date | date | required (quarter-end date) |

### Commission
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| beneficiary_id | uuid | FK → User (agent/superAgent) |
| source_user_id | uuid | FK → User (client) |
| month | number | required |
| year | number | required |
| base_balance | decimal | required |
| percentage | decimal | required |
| amount | decimal | required |
| status | enum | values: pending, accrued, paid |
| accrued_at | timestamp | optional |
| paid_at | timestamp | optional |
| paid_by | uuid | FK → User, optional |
| created_at | timestamp | required |

### ActivationToken
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User, required |
| token | string | required, unique |
| expires_at | timestamp | required |
| used_at | timestamp | optional |
| invalidated_at | timestamp | optional |
| created_at | timestamp | required |

### OtpCode
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User, required |
| code | string | required |
| purpose | enum | values: login, withdrawal, password_change, financial_edit |
| expires_at | timestamp | required (10 min from creation) |
| used_at | timestamp | optional |
| created_at | timestamp | required |

### TrustedDevice
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User, required |
| device_fingerprint | string | required |
| expires_at | timestamp | required (30 days) |
| created_at | timestamp | required |

### Note
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User (subject), required |
| author_id | uuid | FK → User (admin/operator), required |
| content | string | required |
| created_at | timestamp | required |

### AuditLog
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| actor_id | uuid | FK → User, required |
| action | string | required |
| entity_type | string | required |
| entity_id | uuid | required |
| old_value | json | optional |
| new_value | json | optional |
| ip_address | string | optional |
| created_at | timestamp | required |

---

## Business Logic

### Permissions

| Action | Rule |
|--------|------|
| Access backoffice | role = admin OR role = operator |
| Create/edit/block users | role = admin |
| Execute monthly close | role = admin |
| Approve/execute withdrawals | role = admin |
| Execute commission payouts | role = admin |
| Add internal notes | role = admin OR role = operator |
| View audit logs | role = admin |
| Request withdrawal | type = client AND status = active AND tenure >= 3 months |

### Constraints

| Constraint |
|------------|
| Capital < 50,000 USD generates no yields or commissions |
| Withdrawal requests require 10+ days before window date |
| Withdrawal windows: Mar 31, Jun 30, Sep 30, Dec 31 |
| Minimum client tenure for withdrawal: 3 months |
| Minimum client tenure for commission generation: 1 month |
| Guaranteed yield options: 2.5%, 3.25%, 4.25%, 5.0% only |
| OTP codes expire after 10 minutes |
| Activation tokens expire after 24 hours (configurable) |
| Trusted devices expire after 30 days |
| Session timeout: 20 minutes of inactivity |
| One Agent per Client (strict) |
| Agent commission payout blocked if Super Agent has insufficient balance |
| Total withdrawal applies guaranteed minimum % for final month (not standard %) |
| Capital entered day 1-14 → yields start day 15 (50% prorated) |
| Capital entered day 15+ → yields start 1st of next month |

---

## Open Questions

No unresolved ambiguities.
