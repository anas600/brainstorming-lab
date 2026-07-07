# 📋 DECISION #004 — DEC-083 Strategic Analysis (Composite PK + JSONB)
**Date**: 2026-07-07 16:10 UTC
**Decision Authority**: Deep Researcher (Brainstorming Lab)
**Status**: ✅ ACTIVE — Strategic guidance for DEC-083 execution

---

## 🎯 Context

DEC-082 closed the JSON migration pattern for most entities (12 of 14 migrations). 9 tables remain in C# due to:
- Composite primary keys (PK on multiple columns)
- Complex JSONB fields (template_json, posting_rules)
- Read-model tables (stock_levels, stock_reservations, notifications)

DEC-083 (in execution by Tech) extends the DataTypeMigrator to support:
1. Composite primary keys
2. JSONB type (already in type mapping, needs INSERT/SELECT handling)
3. Conversion of 9 remaining tables

---

## 📊 Strategic Considerations

### 1. Composite Primary Key Design

**JSON syntax (proposed):**

```json
{
  "name": "UserRole",
  "table": "user_roles",
  "primary_key": ["user_id", "role_id"],
  "fields": [...]
}
```

**Implementation notes:**

- Migration runner must read `primary_key` as either string OR array
- Dapper `INSERT` with multiple PK columns needs explicit column list
- `ON CONFLICT (user_id, role_id) DO NOTHING` for upsert pattern (vs `ON CONFLICT DO NOTHING` for single PK)
- C# entity generation: `[Key, Column(Order = 0)]` for composite PK fields

**Pattern reference (ERPNext):**
- DocType `user_roles` has composite PK (parent + parenttype + parentfield)
- DocType `posting_rules` uses single PK + JSONB `conditions` field
- Same pattern we're adopting

---

### 2. JSONB Payload Strategy

**Two patterns for complex data:**

#### Pattern A: Explicit JSONB field
```json
{
  "name": "template_json",
  "type": "jsonb",
  "nullable": false,
  "default": "'{}'::jsonb"
}
```

- Used when JSON structure is arbitrary or unknown
- INSERT/SELECT: `JsonDocument` or `Dictionary<string, object>` in C#
- Dapper handles serialization automatically (string → JSONB)

#### Pattern B: Strongly-typed fields (preferred when structure is known)

```json
{
  "name": "amount",
  "type": "decimal(18,2)",
  "nullable": false
},
{
  "name": "currency",
  "type": "varchar(3)",
  "nullable": false
}
```

- Used when fields have known types
- Better for indexes, queries, joins

**Recommendation**: Use Pattern B wherever possible. Pattern A only for truly dynamic data (like `posting_rules.template_json` where rules are configurable at runtime).

---

### 3. Indexing Strategy for Composite PKs

**Each composite PK should have a unique index:**

```sql
CREATE UNIQUE INDEX ix_user_roles_user_role ON user_roles (user_id, role_id);
```

**JSON representation:**

```json
{
  "indexes": [
    {
      "name": "ix_user_roles_user_role",
      "columns": ["user_id", "role_id"],
      "unique": true
    }
  ]
}
```

**Other useful indexes for these tables:**

| Table | Index | Reason |
|---|---|---|
| `user_roles` | `(user_id, role_id)` UNIQUE | PK + lookup |
| `user_roles` | `(role_id)` | Reverse lookup (users in role) |
| `posting_rules` | `(tenant_id, account_id, posting_type)` | Common query pattern |
| `stock_levels` | `(item_id, warehouse_id)` UNIQUE | PK + lookup |
| `stock_reservations` | `(reference_id, reference_type)` | Lookup by source |
| `notifications` | `(tenant_id, user_id, is_read)` | User inbox query |
| `salary_structure_lines` | `(salary_structure_id, line_order)` UNIQUE | PK + ordering |
| `payroll_items` | `(payroll_run_id, employee_id)` UNIQUE | PK + per-employee lookup |
| `payslip_components` | `(payslip_id, component_type)` UNIQUE | PK + by-type |
| `payment_allocations` | `(payment_id, invoice_id)` UNIQUE | PK + per-invoice |

---

### 4. Rollback Plan (if DEC-083 fails)

#### Scenario A: Build fails
- Revert commit on develop
- Force-push to old state (no production impact)
- DEC-083 retried after fix

#### Scenario B: Migration runs but breaks existing data
1. **Immediate**: Set `JsonMigrationEnabled: false` in appsettings
   - DataTypeMigrator won't run on next deploy
   - Old C# migration (NoOp'd) still doesn't create tables (already exist)
   - System stays at current state

2. **If data corrupted**:
   - Restore from Neon DB backup (Neon provides PITR)
   - Redeploy old SHA (5032a02)
   - No data loss if PITR within reasonable window

3. **If specific table corrupted**:
   - DROP TABLE for affected table
   - Old C# migration (NoOp'd but actual CREATE) re-runs
   - Data repopulated from RealisticSeed (if enabled) or manually

#### Scenario C: Composite PK syntax causes unexpected behavior
- Detect via `PRAGMA table_info` (SQLite-style) or `INFORMATION_SCHEMA.COLUMNS` (Postgres)
- DataTypeMigrator logs full schema diff per table
- Easy to identify which table broke

---

## 🛡️ Defense Layer 30: Composite PK + JSONB Support

When DEC-083 completes:
- ✅ All 9 tables moved from C# to JSON
- ✅ DataTypeMigrator supports composite PK
- ✅ DataTypeMigrator handles JSONB type
- ✅ Idempotent on re-deploy
- ✅ Rollback path documented

**Adds Defense Layer 30: All-Tables-In-JSON** (modulo C# exceptions eliminated)

---

## 📊 Open Tables Conversion Plan

| # | Table | JSON File | Special Concern |
|---|---|---|---|
| 1 | `user_roles` | `user_roles.json` | Composite PK (user_id + role_id) |
| 2 | `posting_rules` | `posting_rules.json` | JSONB template + FK |
| 3 | `stock_levels` | `stock_levels.json` | Composite PK (item + warehouse) |
| 4 | `stock_reservations` | `stock_reservations.json` | Composite PK |
| 5 | `notifications` | `notifications.json` | JSONB metadata possible |
| 6 | `salary_structure_lines` | `salary_structure_lines.json` | Composite PK |
| 7 | `payroll_items` | `payroll_items.json` | Composite PK |
| 8 | `payslip_components` | `payslip_components.json` | Composite PK |
| 9 | `payment_allocations` | `payment_allocations.json` | Composite PK |

All 9 share the same pattern: composite PK with FK references.

---

## 🎯 Lab's Recommendations to Tech (DEC-083):

1. **Composite PK syntax**: Use array `["user_id", "role_id"]`
2. **JSONB**: Already in type mapping; verify Dapper serialization
3. **Indexes**: Add explicit unique indexes per table (above)
4. **Idempotency**: Skip CREATE TABLE if exists; skip ADD INDEX if exists
5. **Rollback**: Set `JsonMigrationEnabled: false` in appsettings to disable

---

## 📊 Methodology Notes:

- ✅ DEC-030: AGENTS.md read first
- ✅ DEC-053: Worktree workflow
- ✅ DEC-063: Don't disrupt network
- ✅ Token efficiency: focused per conversion
- ✅ Backward compat: Old migrations still run (NoOp)

---

## 🛡️ Cross-Team Coordination (DEC-052):

- Tech: Executes DEC-083 conversion (~2h)
- Lab: Provides this strategic analysis (done)
- Bridge: Surfaces PRs + Lab DECs
- User: Final Telegram updates

When DEC-083 complete, Tech publishes results + Lab publishes DEC-003 documenting outcomes.
