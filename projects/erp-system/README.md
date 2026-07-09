# 🏢 ERP-SYSTEM Project

> **Libyan Holding Company — Multi-Tenant ERP**
> **Project-specific files for the ERP-SYSTEM work**
> **Part of Lab v2 architecture**

---

## 🎯 Project Overview

| | |
|---|---|
| **Client** | Libyan Holding Company |
| **Stack** | C# / .NET 9, Dapper, FluentMigrator, Npgsql |
| **Tenant** | Multi-tenant (AlFajr, AlBurj, AlNour, AlKhalij, AlKhalil) |
| **Modules** | Finance, HR, Payroll, Procurement, AR, Inventory, Projects |
| **Deployment** | Hugging Face Space |

---

## 📁 Structure

```
projects/erp-system/
├── README.md                    ← This file
├── execution-session/           ← Jimi (Tech Lead) workspace
│   ├── SESSION.md
│   ├── board.md
│   ├── tasks.md
│   └── decisions/               ← Per-session DECs
└── analytical-session/          ← Mavis (Lab) workspace
    └── decisions/               ← Per-session DECs
```

---

## 🔗 References to Templates

This project uses Lab v2 templates from `templates/`:

| Need | Template |
|---|---|
| Lab constitution | `templates/teams/analytical/SYSTEM.md` |
| 5 personas | `templates/teams/analytical/PERSONAS.md` |
| Session protocol | `templates/teams/analytical/SESSION-PROTOCOL.md` |
| Tech Lead role | `templates/teams/execution/TECH-LEAD-ROLE.md` |
| Worktree workflow | `templates/teams/execution/WORKTREE-WORKFLOW.md` |
| Cross-team comms | `templates/cross-team/COMMUNICATION-PROTOCOL.md` |
| Bridge protocol | `templates/cross-team/BRIDGE-V3-PROTOCOL.md` |
| Role boundaries | `templates/cross-team/ROLE-CLARIFICATION.md` |

---

## 📊 Current Sprint State

| Sprint | Status |
|---|---|
| Sprint-3 (JSON Migration) | ✅ COMPLETE (15 DECs, 40+ DLs) |
| Phase 2 (UI/API Integration) | ⏳ Queued (DEC-094+) |
| Sprint-4 (Marten re-eval) | 🚫 SUSPENDED |

---

## 🚀 Next Actions

1. **Phase 2**: UI/API integration (DEC-094)
2. **Smoke tests**: Per-session quality verification
3. **Production deployment**: Per-session deployment guide

---

*Last updated: 2026-07-09 (Lab v2 migration)*