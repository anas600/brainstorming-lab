# 🔍 Tech Stack Discovery — Major Finding
**Date**: 2026-07-05
**Session**: 002 — ERP MVP Monitoring
**Trigger**: User questioned assumptions about EF Core
**Status**: Awaiting user decision on Marten

---

## 📋 User's Question

User noted the README says the project uses **Dapper, FluentMigrator, MartenDB** (NOT EF Core) and asked to verify.

User also mentioned:
- 2 databases: `erp_system` + `erp_events`
- `erp_events` for MartenDB events
- Connection strings as env vars in HF

---

## ✅ Verified Findings (by Mavis)

### 1. Tech Stack — Actual Code

```
<PackageReference Include="Npgsql" Version="8.0.5" />
<PackageReference Include="Dapper" Version="2.1.35" />
<PackageReference Include="FluentMigrator" Version="5.0.0" />
<PackageReference Include="FluentMigrator.Runner.Postgres" Version="5.0.0" />
<PackageReference Include="Marten" Version="7.34.0" />
```

| Package | Status |
|---|---|
| Dapper | ✅ ACTIVE (heavy usage) |
| FluentMigrator | ✅ ACTIVE (14 migrations) |
| **Marten** | ⚠️ **DEAD DEPENDENCY** |
| EF Core | ❌ NONE |

### 2. ⚠️ Critical Finding: Marten Not Wired Up

| Test | Result |
|---|---|
| `grep "AddMarten"` in Program.cs | ❌ Not found |
| `grep "IDocumentSession"` in src/ | ❌ Not found |
| `grep "IQuerySession"` in src/ | ❌ Not found |
| `grep "WithMarten\|WireupMarten"` | ❌ Not found |

**Conclusion**: Marten is a "shadow dependency" — installed but never used. Likely planned for future Sprint.

### 3. Database Configuration

| Section | Value | Status |
|---|---|---|
| `ConnectionStrings:Postgres` | `Database=erp_system` | ✅ ACTIVE |
| `Marten:ConnectionString` | `Database=erp_events` | ⚠️ Configured but unused |
| `Marten:SchemaName` | `mt_events` | ⚠️ Default, never created |

### 4. DROP SCHEMA Impact

**Question**: Was DROP SCHEMA applied to both databases?

**Answer**: 
- `erp_system` → ✅ Reset (the only one we knew about)
- `erp_events` → ❓ State unknown (but unused anyway since Marten disabled)

---

## 🤔 Implications

### Misunderstanding:
- Earlier I (Deep Researcher) assumed EF Core was used
- README confirmed Dapper + FluentMigrator + Marten
- User was right to correct

### Documentation Drift:
- README mentions MartenDB but doesn't mention it's disabled
- Future developers might think event sourcing is implemented

### Cost:
- ⚠️ `erp_events` database in Neon = reserved but unused
- ⚠️ Marten package = build time + memory waste

---

## 🎯 Decision Required from User

### Option A: Enable Marten (Event Sourcing)
- **Time**: 1-2 sprints of work
- **Value**: Real event sourcing + projections
- **Risk**: Implementation complexity
- **Need**: Wire up `AddMarten()`, define events, projections

### Option B: Remove Marten (Cleanup)
- **Time**: 1-2 hours
- **Value**: Cleaner code, faster build
- **Risk**: None
- **Need**: Remove from csproj, delete Marten config, drop `erp_events` database

### Option C: Keep + Document
- **Time**: 30 minutes
- **Value**: Future-proof for event sourcing
- **Risk**: Dead dependency remains
- **Need**: Add `// TODO: Enable Marten in Sprint-X` comment

---

## 📊 Recommendations from 5 Experts

### [CFO أحمد]:
- 🟢 **Option B** — save costs, clean code
- ❌ Option A — too much work for current MVP

### [م. عمر]:
- 🟡 **Option C** — keep flexibility for future
- Document the disabled state clearly

### [CTO ليلى]:
- 🟡 Depends on roadmap
- If event sourcing planned → **Option A**
- If not → **Option B**

### [PO سارة]:
- 🟡 Need to know product roadmap
- What's the value of events to users?

### [أ. سامي]:
- 🟢 **Option C** initially (with clear docs)
- Schedule Option A or B for Sprint planning

---

## 📝 Lessons Learned (Add to existing doc)

### L7: Verify Tech Stack from Code, Not Just Docs

> **READMEs can drift from actual implementation.**

- Always verify package usage with `grep` + `using` statements
- Cross-check README claims against csproj + Program.cs
- Update README when tech stack changes

### L8: Dead Dependencies are Technical Debt

> **Marten installed but unused = waste.**

- Build time increases
- Memory usage increases
- Confusion for new developers
- Audit dependencies regularly

---

*Documented by: Deep Researcher (Brainstorming Lab)*
*Date: 2026-07-05*
*Status: Awaiting user decision*