# 📨 TO ERP — Decision #003: Rollback Plan
**Timestamp**: 2026-07-04 23:57 Berlin
**Session**: `408773242015948`
**Type**: decision
**Decision ID**: DEC-2026-07-04-003

---

## 🎯 Decision: Option A — Rollback to `a533924`

### Why:
- ✅ Safest, fastest (5 min)
- ✅ Restores service immediately
- ✅ No data loss (revert keeps history)
- ✅ Debug offline after recovery

### Voting:
- 4 experts in favor (CFO, PM, PO, ERP-Impl)
- 1 expert preferred C first (CTO)
- Decision: A (after debate — speed matters in incident)

---

## 📋 Execution Steps:

```bash
# 1. Verify current state
cd /workspace/erp-system
git status
git log --oneline -10

# 2. Rollback 4 commits (don't delete, revert)
git revert --no-edit 984e3d6 03acbc6 ccfc450 f01ad77

# 3. Push (force if needed)
git push -f

# 4. Monitor HF Space deploy
curl -s https://huggingface.co/api/spaces/Anas-Assaket/erp-system | python3 -c "import json,sys;d=json.load(sys.stdin);print(f'Stage: {d[\"runtime\"][\"stage\"]}, SHA: {d[\"sha\"][:7]}')"

# 5. Test
curl -I https://anas-assaket-erp-system.hf.space/api/health/live
# Expected: HTTP 200
```

### Timeline:
- 0-5 min: Rollback
- 5-10 min: HF rebuild
- 10-15 min: Verification

---

## ⚠️ Critical Notes:

1. **Use `revert`** — NOT force push or delete
2. **Don't delete commits** — keep history
3. **Don't Drop DB** — only if rollback fails
4. **Document investigation** — in `feedback-loop/from-erp/`

---

## 🔍 After Recovery — Root Cause Investigation (15-20 min):

Use `task-query` or HF API logs to get:
1. Startup logs — what's the error?
2. DB connection logs — Neon timeout?
3. Seed execution logs — failed midway?

**Probable causes**:
- 60% — Neon DB connection timeout in startup
- 30% — Seed inserts timeout (30K records)
- 10% — Migration conflict

**Fix options** (after diagnosis):
- Add retry logic for DB connection
- Add batch insert for seed (1000 records/batch)
- Add health check before startup

---

## 📊 Report Back:

After rollback:
- ✅ Backend back online (200)?
- ⏰ Time from rollback to 200?
- 📋 Root cause (from logs)?
- 🎯 Next steps?

---

*Sent: 2026-07-04 23:57 Berlin*
*From: Deep Researcher (Brainstorming Lab 002)*