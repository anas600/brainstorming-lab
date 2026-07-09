# 📡 Channel 5 Protocol — Tech → Lab Direct Communication

**Version**: 1.0
**Date**: 2026-07-09
**Status**: ✅ ACTIVE
**Author**: Mavis (Lab) — Root Session

---

## 🎯 Purpose

Establish a **direct, event-driven communication channel** from Tech Lead (Jimi) to Lab (Mavis Root), eliminating the dependency on periodic bridge cron reports.

---

## 📋 Channel 5 vs Bridge

| | Channel 5 (NEW) | Bridge (Legacy) |
|---|---|---|
| **Trigger** | Event-driven (task complete) | Time-based (every 3-6h) |
| **Direction** | Tech → Lab direct | Bridge → Lab |
| **Latency** | Instant (within seconds) | Up to 3-6h delay |
| **Token cost** | Low (per event) | High (periodic) |
| **Coverage** | Task completion | Always-on monitoring |

**Decision**: Bridge becomes **alert mode** (only CRITICAL). Channel 5 handles routine task completions.

---

## 🔄 Trigger Conditions

Jimi sends a Channel 5 message when:

1. **Task complete** — A DEC, PR, or feature is shipped
2. **Blocker encountered** — Cannot proceed (Lab needs to decide)
3. **Question** — Needs clarification on directive
4. **Status update** — Mid-task progress (e.g., halfway done)

---

## 📋 Standard Format

```
✅ Task Complete: [DEC-NNN] — [short title]

📊 Summary:
- PR: #XX (SHA: abc1234)
- Files: N (added) / M (modified)
- Defense Layers: DL X-Y
- ETA vs actual: [actual time]

🔍 Self-verification:
- Build: ✅/❌
- Tests: ✅/❌
- HF deploy: ✅/❌ (HTTP code)
- TB: balanced ✅/❌ (if applicable)

🤔 Open issues (if any):
- [issue 1]
- [issue 2]

⏳ Awaiting: Lab verification
```

### Example (Real Use):

```
✅ Task Complete: DEC-096 — Financial pages UI

📊 Summary:
- PR: #88 (SHA: 8261b5c)
- Files: 8 added / 1 modified
- Defense Layers: DL 47-50
- ETA vs actual: 2h vs 1h (faster)

🔍 Self-verification:
- Build: ✅
- Tests: ✅
- HF deploy: ✅ (200)
- TB: 11.24M = 11.24M ✅

🤔 Open issues: None

⏳ Awaiting: Lab verification
```

---

## 🤖 Lab Workflow on Channel 5

When Lab receives Channel 5 message:

1. **Parse** the message (PR + SHA + DLs + self-verification)
2. **Independent verify**:
   - Read GitHub PR via API
   - Read commit details
   - HF probe (HTTP 200/404)
   - Read TB if financial change
3. **Publish** Lab DEC (success or failure)
4. **Notify** user via Telegram
5. **Send directive** for next task (if applicable)

---

## 🛡️ Failure Modes & Recovery

| Scenario | Behavior |
|---|---|
| Jimi sends without complete info | Lab requests missing data |
| Lab cannot verify (GitHub 4xx) | Lab escalates to user via Telegram |
| TB unbalanced | Lab rejects PR, asks Jimi to fix |
| HF probe fails | Lab escalates CRITICAL |
| Jimi silent > 6h | Bridge alert fires (fallback) |

---

## 📊 Metrics (Targets)

- Latency: < 5 min from PR merge to Lab DEC publish
- Bridge alerts: < 1 per day (only CRITICAL)
- Channel 5 messages: 2-4 per day (per active sprint)

---

## 📂 Document Locations

- Master: `/workspace/.mavis/CHANNEL-5-PROTOCOL.md`
- Templates: `brainstorming-lab/templates/cross-team/CHANNEL-5-PROTOCOL.md` (to add)
- Constitution (Lab): `/workspace/.constitution/lab/CHANNEL-5-PROTOCOL.md`
- Constitution (Tech): `/workspace/.constitution/team/CHANNEL-5-PROTOCOL.md`

---

**Status**: ✅ ACTIVE. First use: DEC-096 (2026-07-09 12:13 UTC).
