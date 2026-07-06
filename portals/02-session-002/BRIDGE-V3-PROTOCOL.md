# Bridge v3 Protocol Specification

> **Single source for v3 bridge protocol — apply when cron update tool bug fixed**

## 🎯 What is Bridge v3?

Bridge v3 = Code monitoring + Strategic context awareness

While v2 only detects code-level deltas (commits), v3 ALSO reads STATUS.md from the implementation repo for strategic state.

## 📋 v3 Steps

### Step 0 (NEW): Strategic Context from STATUS.md

```bash
STATUS=$(curl -sf --max-time 10 "https://raw.githubusercontent.com/anas600/ERP-SYSTEM/develop/STATUS.md" 2>/dev/null)
if [ -n "$STATUS" ]; then
  PARSED_STRATEGY=$(echo "$STATUS" | python3 -c "
import sys
content = sys.stdin.read()
# Parse sections, extract key fields
print('---STRATEGIC CONTEXT---')
# Extract sprint, progress, next task, deferred
# (use regex or simple parsing)
")
fi
```

If $STATUS is empty → graceful fallback to v2 (no strategic context in output)

### Step 1: Quick state check (HTTP, parallel)
- HF Space status (unchanged from v2)
- GitHub ERP commits (unchanged)
- GitHub Lab DECs (unchanged)

### Step 2: Delta detection (unchanged from v2)
- Persist /tmp/erp-bridge-state-v2.json
- Skip if no changes

### Step 3: Bridge report v3 (≤ 250 words)

```
🟢 Status:
- Tech: [SHA] [HF stage]
- Analytical: [last DEC file] [open items count]

📊 Strategic Context (from STATUS.md):
- Sprint: [current sprint + progress %]
- Production readiness: [%]
- Next action: [from STATUS.md]
- Deferred: [from STATUS.md]

🚀 Deltas (last 3h):
- Tech: [new commits with 1-line description each]
- Analytical: [new DECs/feedback]

⚠️ Blockers (if any):
- Tech → Analytical: [blockers]
- Analytical → Tech: [blockers]
- User-facing: [action needed]

🎯 Next steps (smart recommendation):
- ONE clear action aligned with STATUS.md
```

### Step 4-5: Same as v2

## 🛡️ Fallback Strategy

If STATUS.md fetch fails:
- Log warning
- Continue with v2 protocol
- Output omits strategic context block (graceful degradation)

## 🎯 Why v3

- v2: detects code changes only
- v3: detects code + strategic changes
- Future bridges (or this one after bug fix): always have context

## 🔧 How to Apply

When cron `update` tool bug is fixed:
1. Get current v2 cron task_id (416151197184106)
2. Replace prompt with v3 prompt (in /tmp/bridge-v3-prompt.txt)
3. Or create new cron as v3 and disable old v2
