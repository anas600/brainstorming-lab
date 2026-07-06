# ERP-SYSTEM Task Tracker

> **Live task tracking across both teams**
> **Format**: Markdown (Excel export available on request)
> **Last updated**: 2026-07-06

---

## In Progress

| ID | Task | Owner | Started | Status | Notes |
|---|---|---|---|---|---|
| — | — | — | — | — | No tasks in progress |

---

## Completed (Recent)

| ID | Task | Owner | Completed | DEC | Commit |
|---|---|---|---|---|---|
| T-2026-07-06-010 | In-app domain events + handlers (Sprint-4.5 T-010) | Mavis | 2026-07-06 | DEC-057 | `2d3255c` |
| T-2026-07-06-009 | audit_log table + IAuditLogger (Sprint-4.5 T-009) | Mavis | 2026-07-06 | DEC-056 | `6d27b69` |
| T-2026-07-06-008 | Cross-Team Awareness (AGENTS.md) | Mavis | 2026-07-06 | New | `49d37ed` |
| T-2026-07-06-007 | DEC-054 followups (issues #20-22) | Mavis | 2026-07-06 | DEC-054 FU | `995ab30` |

---

## Blocked

| ID | Task | Owner | Blocked Since | Blocker |
|---|---|---|---|---|
| — | — | — | — | No blockers |

---

## Pending / Waiting

| ID | Task | Owner | Waiting For | Priority |
|---|---|---|---|---|
| T-2026-07-05-001 | Session 001 Stage 3 (Synthesis) | Analytical | User (Anas) signal | Medium |
| T-2026-07-05-002 | Improvement 5: Security Scanning | Mavis | Sprint-4.5 finish | Low |
| T-2026-07-05-003 | DB Rename (erp-events → erp_events) | Mavis | Sprint-5 kickoff (currently DEFERRED) | Medium |
| T-2026-07-05-004 | JWT Secret rotation | Mavis | User action | Low |
| T-2026-07-05-005 | JWT Issuer/Audience update | Mavis | User action | Low |

---

## Recommended Next Tasks (Sprint-4.5)

### High Priority (Sprint-4.5 — Stability & Shipping):

| ID | Task | ETA | Dependencies | Status |
|---|---|---|---|---|
| DONE T-009 | audit_log table (Finance + Projects) | 2h | None | Merged |
| DONE T-010 | In-app event publishers (cross-module) | 4h | None | Merged |
| T-2026-07-06-011 | Soft deletes (deleted_at on invoices + projects) | 1h | None | NEXT |
| T-2026-07-06-012 | End-to-end tests (Finance flow) | 2 days | T-009, T-010 | Pending |
| T-2026-07-06-013 | Improvement 5: Security Scanning | 1 day | None | Parallel |
| T-2026-07-06-014 | Deployment guide + User training | 2 days | After tests | Pending |

### ~~T-NEXT-001 Sprint-5 (Marten) — DEFERRED~~

**Status**: DEFERRED (96% consensus)
**Reason**: ROI not justified at MVP stage
**Alternative**: audit_log + simpler patterns (8h vs 160h)
**Re-evaluation**: Quarterly (after 30 days of production use)

---

## Task Statistics

| Metric | Value |
|---|---|
| Total tasks completed | 10+ |
| Tasks in progress | 0 |
| Blocked tasks | 0 |
| Pending tasks | 5 |
| Recommended next | 3 (Sprint-4.5) |

---

## Update Protocol

### When to update:

- Task completed → mark Done, move to Completed section
- Task started → move from Pending to In Progress
- Task blocked → mark with blocker reason
- Task reactivated → move back to In Progress

### Who updates:

- **Mavis**: After PR merge, after Sprint day complete
- **Analytical team**: After DEC approval, after strategic decision

### Format:

```
| T-YYYY-MM-DD-NNN | Task name | Owner | YYYY-MM-DD | DEC-XXX | commit SHA |
```

---

*Last update: 2026-07-06 (after DEC-057 merge)*
*Next update: After T-011 (soft deletes) or next PR merge*
