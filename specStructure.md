## projectSpec.md Structure

### Product Overview

2-3 lines. What it is, for whom, what problem it solves. No marketing.

---

### Pages & Routes

For each route, define actors, data, and interactions together:

#### /route-path
| Actors | Data |
|--------|------|
| User | entity1, entity2 |

**Interactions:**
- Action → System consequence → Integration (if any) → Error handling
- Action → System consequence → Error handling

Example:

#### /login
| Actors | Data |
|--------|------|
| Guest | - |

**Interactions:**
- Submit credentials → Validate → If new device: trigger 2FA via email (OTP 10 min) → Error: Toast "Invalid credentials"
- Click "Forgot password" → Redirect /forgot-password

---

#### /dashboard
| Actors | Data |
|--------|------|
| User | user, balance, yields, withdrawals |

**Interactions:**
- View financial summary: capital, balance, accumulated yield
- Click "Request withdrawal" → Validate window + tenure → Open modal → Error: Toast "Withdrawal window closed"
- Export yields → Download CSV

---

#### /backoffice/users
| Actors | Data |
|--------|------|
| Admin, Operator | users |

**Interactions:**
- View user list with filters (type, status)
- Click user → Navigate /backoffice/users/[id]
- Admin: Click "New user" → Navigate /backoffice/users/new

---

Notes:
- **Actors**: Guest / User / Admin (roles that access this route)
- **Data**: Entity names from Data Model
- **Interactions**: Only include non-obvious flows (skip basic navigation, standard CRUD)

---

### Data Model

#### EntityName
| Field | Type | Constraints |
|-------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK → User, required |
| amount | decimal | required |
| status | enum | values: requested, approved, executed |
| requested_at | timestamp | required |
| approved_by | uuid | FK → User, optional |

Types: uuid, string, number, boolean, decimal, timestamp, date, enum, json
Constraints: PK, FK → Table, required, unique, optional, default: value

---

### Business Logic

#### Permissions

Only non-obvious rules:

| Action | Rule |
|--------|------|
| Access backoffice | role = admin OR operator |
| Execute withdrawals | role = admin |
| Request withdrawal | status = active AND tenure >= 3 months |

#### Limits

Caps and boundaries not obvious from the Data Model:

| Entity | Limit |
|--------|-------|
| OTP codes | expire after 10 minutes |
| Activation tokens | expire after 24 hours |
| Session | timeout after 20 min inactivity |
| Minimum capital for yields | 50,000 USD |
