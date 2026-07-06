# 📋 ERP-SYSTEM Task Tracker

> **Live task tracking across both teams**
> **Format**: Markdown (Excel export available on request)
> **Last updated**: 2026-07-06

---

## 🟢 In Progress

| ID | Task | Owner | Started | Status | Notes |
|---|---|---|---|---|---|
| — | — | — | — | — | No tasks in progress |

---

## ✅ Completed (Recent)

| ID | Task | Owner | Completed | DEC | Commit |
|---|---|---|---|---|---|
| T-2026-07-06-008 | Cross-Team Awareness (AGENTS.md) | Mavis | 2026-07-06 | New | `49d37ed` |
| T-2026-07-06-007 | DEC-054 followups (issues #20-22) | Mavis | 2026-07-06 | DEC-054 FU | `995ab30` |
| T-2026-07-06-006 | DEC-054 Test Workflow | Mavis | 2026-07-06 | DEC-054 | `2e43c42` |
| T-2026-07-06-005 | DEC-052 Git Workflow | Mavis | 2026-07-06 | DEC-052 | `4b4520a` |
| T-2026-07-06-004 | DEC-051 Auto-Rollback | Mavis | 2026-07-06 | DEC-051 | `19a1780` |
| T-2026-07-06-003 | DEC-050 Workflows Consolidation | Mavis | 2026-07-06 | DEC-050 | `339d26a` |
| T-2026-07-06-002 | Sprint-4 Day 4 (README + tests) | Mavis | 2026-07-06 | DEC-046 | `884951a` |
| T-2026-07-06-001 | Sprint-4 Day 3 (Logging + Middleware) | Mavis | 2026-07-06 | DEC-045 | `d373c9b`, `108b16c`, `db7d1ea` |
| T-2026-07-05-005 | Sprint-4 Day 2 (Admin + Batch + Retry) | Mavis | 2026-07-05 | DEC-042 | `83e8a45`, `c496395`, `f489ec6` |
| T-2026-07-05-004 | Sprint-4 Day 1 (Health + Seeder flags) | Mavis | 2026-07-05 | DEC-032/034 | `14723bd` |

---

## 🔴 Blocked

| ID | Task | Owner | Blocked Since | Blocker |
|---|---|---|---|---|
| — | — | — | — | No blockers |

---

## ⏳ Pending / Waiting

| ID | Task | Owner | Waiting For | Priority |
|---|---|---|---|---|
| T-2026-07-05-001 | Session 001 Stage 3 (Synthesis) | Analytical | User (Anas) signal | 🟡 Medium |
| T-2026-07-05-002 | Improvement 5: Security Scanning | Mavis | User approval | 🟢 Low |
| T-2026-07-05-003 | DB Rename (`erp-events` → `erp_events`) | Mavis | Sprint-5 kickoff | 🟡 Medium |
| T-2026-07-05-004 | JWT Secret rotation | Mavis | User action | 🟢 Low |
| T-2026-07-05-005 | JWT Issuer/Audience update | Mavis | User action | 🟢 Low |

---

## 🎯 Recommended Next Tasks (Sprint-4.5)

### High Priority (Sprint-4.5 — Stability & Shipping):

| ID | Task | ETA | Dependencies | Status |
|---|---|---|---|---|
| T-2026-07-06-009 | audit_log table (Finance + Projects) | 2h | None | ⏳ New |
| T-2026-07-06-010 | In-app event publishers | 4h | None | ⏳ New |
| T-2026-07-06-011 | End-to-end tests | 2 days | T-009, T-010 | ⏳ New |
| T-2026-07-06-012 | Improvement 5: Security Scanning | 1 day | None | ⏳ |
| T-2026-07-06-013 | Deployment guide + User training | 2 days | None | ⏳ |

### ~~T-NEXT-001 Sprint-5 (Marten) — DEFERRED~~

**Status**: DEFERRED (96% consensus)
**Reason**: ROI not justified at MVP stage
**Alternative**: audit_log + simpler patterns (8h vs 160h)
**Re-evaluation**: Quarterly (after 30 days of production use)

### Medium Priority:

| ID | Task | ETA | Dependencies |
|---|---|---|---|
| T-NEXT-003 | Session 001 Stage 3 (Synthesis) | 1-2 days | User signal |

### Low Priority (Optional):

| ID | Task | ETA | Dependencies |
|---|---|---|---|
| T-NEXT-004 | Improvement 5 (Security Scanning) | 1 hour | User approval |
| T-NEXT-005 | Issue #23 (Pre-commit hook) | 30 min | None |
| T-NEXT-006 | Issue #24 (Coverage threshold) | 30 min | None |

---

## 📊 Task Statistics

| Metric | Value |
|---|---|
| Total tasks completed | 9+ (Sprint-4 + follow-ups) |
| Tasks in progress | 0 |
| Blocked tasks | 0 |
| Pending tasks | 5 |
| Recommended next | 2-3 |

---

## 🔄 Update Protocol

### When to update:

- ✅ Task completed → mark ✅ Done, move to Completed section
- ✅ Task started → move from Pending to In Progress
- ✅ Task blocked → mark 🔴 with blocker reason
- ✅ Task reactivated → move back to In Progress

### Who updates:

- **Mavis**: After PR merge, after Sprint day complete
- **Analytical team**: After DEC approval, after strategic decision

### Format:

```
| T-YYYY-MM-DD-NNN | Task name | Owner | YYYY-MM-DD | DEC-XXX | commit SHA |
```

---

*Last update: 2026-07-06 09:30 UTC*
*Next update: After next PR merge or DEC approval*
