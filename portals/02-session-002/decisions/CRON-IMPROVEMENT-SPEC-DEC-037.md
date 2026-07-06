# 📊 Cron Improvement Spec — DEC-037

> **Bridge Reporter: Brainstorming Lab ↔ ERP Tech Team**

---

## 🚨 Current Bug:

The cron `erp-3h-report` (ID `416137089306693`) is currently set to:
- **Schedule**: `43 * * * *` (every hour — NOT every 3 hours!)
- **Original intent**: `0 */3 * * *` (every 3 hours)

This was likely modified accidentally somewhere. The bug means:
- 24 reports/day instead of 8
- Wasted tokens (~$5/day)
- Reports duplicate content
- Doesn't bridge between teams effectively

---

## 🎯 New Cron Spec

### Schedule:
```
0 */3 * * *
```
(Truly every 3 hours — 8 reports/day)

### Agent:
```
Mavis
```

### Cron Name:
```
erp-bridge-report
```

### Active Hours (optional):
```json
{
  "start": "07:00",
  "end": "23:00"
}
```
(Optional: skip 23:00-07:00 to reduce overnight reports)

---

## 📝 Full Prompt:

```
You are the ERP-SYSTEM Bridge Reporter — connecting two teams:

TEAM 1: Brainstorming Lab Experts (5 experts) — analytical/strategic
TEAM 2: Mavis's Tech Team — implementation/execution

Your job: BRIDGE INSIGHTS between them every 3 hours.

STEPS:

1. Quick State Check (parallel):
   - HF Space: curl -s "https://huggingface.co/api/spaces/Anas-Assaket/erp-system"
   - Last commit: cd /workspace/erp-system && git log --oneline -5
   - Last DEC: ls -t /workspace/.mavis/labs/brainstorming-lab-portals/portals/02-session-002/feedback-loop/decisions/*.md | head -1

2. Delta Detection:
   - Compare to PREVIOUS run state (stored in /tmp/erp-bridge-state.json)
   - If unchanged: brief "no activity" (≤ 30 words)
   - If changed: detailed report

3. Bridge Report Format (≤ 200 words):

🟢 Status:
- Tech: [SHA] [HF state]
- Analytical: [last DEC] [open items]

🚀 Deltas (only if any):
- Tech: [new commits since last run]
- Analytical: [new DECs/feedback]

⚠️ Blockers (only if any):
- Tech → Analytical: [...]
- Analytical → Tech: [...]
- External (user): [...]

🎯 Next Steps (smart recommendation):
- Based on current state
- One clear action

4. Persist state:
echo '{"last_sha": "...", "last_dec": "..."}' > /tmp/erp-bridge-state.json

5. Send to bridge session 406067545768199 via communicate tool

Output: "Bridge report sent."

---

ESCALATION RULES:
- If "BREAKING" in commit message → escalate immediately
- If HF Space stage = "CRASHED" or runtime error → escalate
- If new DEC mentions user action needed → escalate
- If no activity for 24h AND user asked for action → escalate

---

5-Expert Lens (apply briefly):
- CFO أحمد: cost implications
- م. عمر: practical/scheduling
- CTO ليلى: technical quality
- PO سارة: user impact
- أ. سامي: methodology/process
```

---

## 🎯 Benefits:

| Aspect | Old | New |
|---|---|---|
| Frequency | 24/day | 8/day |
| Tokens/report | ~300 words | ~150 words |
| Daily tokens | ~7,200 | ~1,200 |
| Reports duplicate | Yes | No (delta) |
| Bridges teams | No | Yes |
| Actionable | No | Yes |

**Token savings**: ~80% reduction

---

## 🚀 Implementation Options:

### Option A: Mavis updates via bash (recommended)
```bash
# Mavis has direct bash access — try:
mavis cron update --task_id=416137089306693 \
  --schedule="0 */3 * * *"
```

### Option B: Delete + Create
```bash
# Delete old
mavis cron delete --task_id=416137089306693

# Create new
mavis cron create --agent_name=Mavis \
  --cron_name=erp-bridge-report \
  --schedule="0 */3 * * *" \
  --prompt="..."
```

### Option C: Manual update via Web UI
- Login to archon platform
- Navigate to Mavis's cron tasks
- Find `erp-3h-report`
- Edit schedule to `0 */3 * * *`
- Edit prompt (paste above)

### Option D: Document + Wait
- Save this spec to repo (already done)
- Wait for tool fix

---

## 📍 Status:

- ✅ Spec documented (this file)
- ⏳ Awaiting Mavis to execute Option A/B
- 📋 Reference: DEC-037

---

## 🔗 Related Decisions:

- **DEC-017**: Marten disabled (Sprint-5 reference)
- **DEC-030**: AGENTS.md reading rule (cross-project)
- **DEC-032**: Plan B direct execution
- **DEC-037**: This improvement spec

---

*Documented by: Deep Researcher (Brainstorming Lab)*
*Date: 2026-07-06*
*Status: Awaiting implementation*