# 📚 Brainstorming Lab Templates

> **Reusable templates for cross-team analytical work**
> **Lab v2 architecture**

---

## 🎯 Purpose

This `templates/` directory contains **reusable** rules, protocols, and personas that get applied to every new project. Unlike per-project files (in `projects/`), these templates evolve slowly and are referenced by multiple projects.

---

## 📁 Structure

```
templates/
├── teams/
│   ├── analytical/              ← Lab-specific (Deep Researcher)
│   │   ├── SYSTEM.md           ← Lab constitution
│   │   ├── PERSONAS.md         ← 5 expert personas
│   │   └── SESSION-PROTOCOL.md ← How to run sessions
│   └── execution/              ← Tech-specific (Tech Lead)
│       ├── TECH-LEAD-ROLE.md   ← Tech Lead responsibilities
│       └── WORKTREE-WORKFLOW.md ← DEC-053 worktree workflow
└── cross-team/                 ← Shared between teams
    ├── COMMUNICATION-PROTOCOL.md ← Lab ↔ Tech communication
    ├── BRIDGE-V3-PROTOCOL.md     ← Bridge cron spec
    └── ROLE-CLARIFICATION.md     ← Cross-team boundaries
```

---

## 🛡️ Who Reads What

### Lab (Deep Researcher) reads:
- `teams/analytical/*` (always)
- `cross-team/*` (when communicating with Tech)
- **Does NOT read** `teams/execution/*` (not their domain)

### Tech (Tech Lead) reads:
- `teams/execution/*` (always)
- `cross-team/*` (when receiving from Lab)
- **Does NOT read** `teams/analytical/*` (not their domain)

---

## 📊 Token Savings

Before Lab v2:
- Each agent read all 9 files (~55 KB)
- Every decision required re-reading

After Lab v2:
- Each agent reads only their team's files (~5-10 KB)
- Templates cached after first read
- **Token savings: 70%+**

---

## 🆕 Adding New Templates

When adding a new template:
1. Determine which team it belongs to (or cross-team)
2. Create file in appropriate folder
3. Update this README's structure diagram
4. Document in `LAB-DEC-XXX.md`

---

## 🔄 Versioning

- **v1** (deprecated): flat file structure in `portals/`
- **v2** (current): team-based templates
- **v3** (future): per-project isolated templates

---

## 📚 Related Files

- `../projects/<project-name>/` — Per-project files (session-specific)
- `../README.md` — Top-level repo README

---

*Last updated: 2026-07-09 (Lab v2 migration)*