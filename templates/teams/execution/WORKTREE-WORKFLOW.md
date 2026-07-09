# 🌳 Worktree Workflow (DEC-053)

> **Isolated development for clean branch management**
> **Part of Lab v2 architecture (templates/teams/execution/)**

---

## 🎯 Purpose

Every code change = in a worktree (not main directory directly).
Main directory stays on default branch (clean baseline).

---

## 🛠️ Setup (per DEC)

```bash
# 1. Create worktree from develop branch
git worktree add ../wt-<DEC-name> -b feature/<DEC-name> develop

# 2. Immediately rebase on develop (in case it changed)
cd ../wt-<DEC-name>
git fetch origin
git rebase origin/develop
```

---

## 📁 Naming Convention

| Pattern | Example |
|---|---|
| `wt-<DEC-name>` | `wt-dec094-ui-inventory` |
| Branch: `feature/<DEC-name>` | `feature/dec094-ui-inventory` |

---

## 🔄 Workflow

```bash
# Work in worktree
cd ../wt-<DEC-name>

# Make changes
# ... edit files ...

# Commit
git add .
git commit -m "feat(<scope>): DEC-NNN — <description>"

# Push branch
git push -u origin feature/<DEC-name>

# Open PR on GitHub
gh pr create --base develop --title "feat(<scope>): DEC-NNN — <description>"

# After PR merged, cleanup worktree
cd ../<main-repo>
git worktree remove ../wt-<DEC-name>
git branch -d feature/<DEC-name>
```

---

## 🛡️ Benefits

| Benefit | Description |
|---|---|
| **Isolation** | Each DEC in its own folder |
| **Parallel work** | Multiple DECs in parallel without conflicts |
| **Clean rollback** | Delete worktree = full rollback |
| **No main pollution** | Main directory always clean |

---

## ⚠️ Anti-Patterns

❌ **DON'T** work directly in main directory
❌ **DON'T** commit to develop without PR
❌ **DON'T** leave worktrees after merge

✅ **DO** create worktree per DEC
✅ **DO** use AGENTS.md in worktree
✅ **DO** cleanup after merge

---

## 📚 Related Files

- `TECH-LEAD-ROLE.md` — Tech Lead responsibilities
- `../cross-team/ROLE-CLARIFICATION.md` — Lab ↔ Tech boundaries

---

*Last updated: 2026-07-09 (Lab v2 migration)*