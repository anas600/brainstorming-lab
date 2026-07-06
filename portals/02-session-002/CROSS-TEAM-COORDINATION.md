# 🤝 Cross-Team Coordination Protocol

> **Reusable pattern for Brainstorming Lab + Implementation Team collaboration**
> **Established**: 2026-07-06 (Session 002 — ERP MVP)

---

## 🎯 Purpose

Define **how the Brainstorming Lab (analytical team)** and the **Implementation Team (Mavis / Tech Lead)** collaborate efficiently:

- Share documentation without burning tokens
- Maintain a single source of truth (this repo)
- Avoid duplication and confusion
- Make the pattern reusable for future projects

---

## 🏛️ The Two Teams

### 🟢 Team A: Implementation Team (Mavis / Tech Lead)

- **Workspace**: `/workspace/<project>/` (e.g., `erp-system/`)
- **Repo**: e.g., `https://github.com/anas600/ERP-SYSTEM`
- **Role**: Execute, code, deploy, test, merge
- **Tools**: `team`, `team-mavis-plan`, git, worktrees, CI/CD

### 🔵 Team B: Brainstorming Lab (Analytical Team)

- **Workspace**: `/workspace/.mavis/labs/brainstorming-lab/`
- **Repo**: `https://github.com/anas600/brainstorming-lab` (this repo)
- **Role**: Analyze, plan, review, document, decide
- **Tools**: 5-expert personas, web_search, web_fetch, file editing

---

## 📂 Repository Structure (this repo = the hub)

```
brainstorming-lab/
├── SYSTEM.md                          # Constitution (7 sections)
├── portals/
│   └── NN-session-name/
│       ├── SESSION.md                 # Session context + Output Rules
│       ├── ROLE-CLARIFICATION-FOR-MAVIS.md  # Mavis's role (if relevant)
│       ├── CROSS-TEAM-COORDINATION.md       # This file
│       ├── SYSTEM.md                  # Copy of constitution (per-session)
│       ├── board.md                   # Live system progress
│       ├── tasks.md                   # Task tracking
│       ├── cycles/                    # Brainstorming cycles
│       ├── feedback-loop/
│       │   ├── decisions/             # DEC-YYYY-MM-DD-NNN-name.md
│       │   ├── from-erp/              # From implementation team
│       │   └── to-erp/                # To implementation team
│       └── (other deliverables)
└── sessions/                          # Legacy (older sessions)
```

---

## 🔄 Communication Protocols

### 1️⃣ **Direct Messaging** (`communicate` tool)

**Real-time, fast, conversational**

```
Mavis (Tech Lead) ←────communicate────→ Deep Researcher (Root + 5 experts)
```

**Use cases**:
- Directives (Mavis receives tasks)
- Status updates
- Blocker alerts
- Quick clarifications

### 2️⃣ **GitHub PRs** (Async, versioned, reviewable)

**Slow but high-fidelity**

```
Implementation Repo (PR) ←─────→ Brainstorming Lab (review/comment)
```

**Use cases**:
- Code reviews (5-expert team reviews PRs)
- Merge decisions
- Issue creation (non-blocking follow-ups)
- Documentation updates

### 3️⃣ **Bridge Cron v2** (Every 3h, automated)

**Scheduled, low-noise, delta-only**

```
Tech Team → Bridge Cron → Root → Telegram → User
```

**Use cases**:
- Status reports (only on deltas)
- Blockers escalation
- HF Space health check
- No standups needed

### 4️⃣ **DECs** (Persistent decisions)

**Forever, cross-cutting**

```
DECs stored in: brainstorming-lab/portals/NN-session/feedback-loop/decisions/
                + user memory (cross-project rules)
```

**Use cases**:
- Decisions affecting both teams
- Methodology changes
- Cross-project rules

---

## 🚦 Token Efficiency Rules (CRITICAL)

### ❌ DON'T:
- Don't paste `https://raw.githubusercontent.com/...` URLs in every directive
- Don't ask Mavis to read SYSTEM.md + ROLE-CLARIFICATION.md + SESSION.md every task
- Don't repeat context that Mavis already knows

### ✅ DO:
- Add cross-team awareness **once** in AGENTS.md (Mavis's project)
- Reference this repo URL **once** in AGENTS.md
- Tell Mavis to read **specific file** only when needed:
  ```
  📖 READ: brainstorming-lab/portals/02-session-002/decisions/DEC-054.md
  ```
- Trust that Mavis knows HIS project conventions (AGENTS.md)
- Trust that you (analytical team) know the lab conventions (SYSTEM.md)

### Token Math:

| Approach | Tokens / Task |
|---|---|
| ❌ Paste URLs + re-explain context | ~500 tokens |
| ✅ Reference specific file | ~50 tokens |
| ✅ Save 90% | 450 tokens saved |

---

## 📋 Workflow Pattern (Reusable)

### Standard Task (Mavis's perspective):

```
1. Receive directive from analytical team
2. Read AGENTS.md (project conventions) — once per session
3. Work in worktree (per worktree-management skill)
4. Commit + push to feature branch
5. Open PR (or report URL for analytical team to create)
6. Wait for analytical team's review
7. If approved → merge
8. Cleanup worktree
9. Report back
```

### Standard Review (Analytical team's perspective):

```
1. Receive PR URL (or fetch via API)
2. Wait for CI checks (auto)
3. Run 5-expert review
4. Decide: APPROVE / REQUEST CHANGES / COMMENT
5. If APPROVE → merge (via API)
6. Close related issues
7. Update board.md + tasks.md
8. Communicate decision to Mavis
9. Bridge cron detects delta → reports to user
```

---

## 🤝 Cross-Team Awareness (in AGENTS.md)

When a project has a Brainstorming Lab partner, add this section to **AGENTS.md**:

```markdown
## 🤝 Cross-Team Coordination (Brainstorming Lab)

This project has an analytical team connected via the Brainstorming Lab.

- **Hub repo**: https://github.com/anas600/brainstorming-lab
- **When to read from there**: ONLY when explicitly instructed by the analytical team
- **Default**: Work from local context (AGENTS.md, RUNBOOK.md, source code)
- **Pattern**: Read specific file → work → push → wait for review

**Don't** read SYSTEM.md + ROLE-CLARIFICATION.md + SESSION.md every task (token waste).
```

This adds awareness **once** without ongoing token cost.

---

## 🔄 Bridge Cron Pattern (DEC-037, DEC-039)

The bridge cron v2 is the **scheduled reporter** between teams:

```yaml
Schedule: 0 */3 * * *  # Every 3 hours
Mode: HTTP-only (no local FS)
Endpoints:
  - HF Space API: status check
  - GitHub commits: delta detection
  - GitHub issues: DEC tracking
Output: communicate to root session (no spam, delta-only)
```

**Established**: Sprint-4 follow-ups (DEC-037)

---

## 📊 Documentation Update Rules

### When to update `board.md`:

- After every PR merge
- After every DEC approved
- After Sprint-4 / Sprint-5 kickoffs
- When status changes (RUNNING / BLOCKED / DONE)

### When to update `tasks.md`:

- Task started
- Task completed
- Task blocked
- Task reopened

### Who updates?

| File | Owner |
|---|---|
| `board.md` | Analytical team (after each significant event) |
| `tasks.md` | Both teams (analytical after DEC, Mavis after merge) |
| `decisions/` | Analytical team (DEC creator) |
| `cycles/` | Analytical team (facilitator) |

---

## 🛠️ Reusability (Future Projects)

This pattern applies to ANY project with a Brainstorming Lab + Implementation Team:

1. Create new session folder: `portals/03-new-project/`
2. Copy template files:
   - SESSION.md (with project context)
   - SYSTEM.md (or link to root)
   - CROSS-TEAM-COORDINATION.md (this file or simplified version)
   - board.md (empty template)
   - tasks.md (empty template)
3. Both teams reference this hub
4. Same protocols apply

**No need to recreate from scratch** — just clone and adapt.

---

## 📚 Related Documents

- **SYSTEM.md** — Constitution (7 sections)
- **SESSION.md** — Session 002 context
- **ROLE-CLARIFICATION-FOR-MAVIS.md** — Mavis's role definition
- **board.md** — Live system progress
- **tasks.md** — Task tracking
- **decisions/** — Persistent decisions (DECs)

---

*Established: 2026-07-06 by Deep Researcher + User (Anas)*
*Reusable for: All future Brainstorming Lab + Implementation Team projects*
