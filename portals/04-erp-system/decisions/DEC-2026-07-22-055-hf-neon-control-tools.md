# 📋 DEC-055 — HF Space & Neon Control Tools (Quick Win)

> **Date**: 2026-07-22 00:43 UTC
> **Status**: 🟡 Active
> **Author**: Mavis (Coordinator) on behalf of Anas
> **Audience**: Jimi (Tech Lead) + Lab (Analytical Team)

## 🎯 Goal

Pass HF Space control tools + Neon DB connection management to Jimi (Tech Lead) so he can orchestrate deployment, testing, and database operations from his sandbox.

## 🛠️ Tools to Pass to Jimi

### 1. HF Space Control
- **Token location**: `/workspace/.mavis/secrets/hf.token` (chmod 600)
- **Capabilities**: Deploy, start, stop, restart, view logs, view status
- **SDK**: `huggingface_hub` Python lib (v1.24.0+)
- **CLI**: `hf` (v1.24.0+) — `hf auth login --token <token>`

### 2. Neon DB Control
- **Token location**: `/workspace/.mavis/secrets/neon.key` (chmod 600)
- **Capabilities**: Query, schema inspection, run migrations, view logs
- **MCP**: `https://mcp.neon.tech/mcp` via `Authorization: Bearer napi_*`
- **Connection string**: stored in `/workspace/.mavis/secrets/neon.url` (chmod 600)

## 🔒 Connection Lifecycle Policy (CRITICAL)

**RULE**: Every connection must follow: **open → use → close**. Never leave connections open.

```python
# Correct pattern
conn = psycopg2.connect(os.environ['NEON_URL'])
try:
    # use connection
    cursor.execute("...")
finally:
    conn.close()  # ALWAYS close
```

**Why**: Idle connections consume Neon compute units. Open = paid. Closed = free.

## 📂 Two-Copy Documentation

This DEC lives in:
1. **Mavis internal**: `/workspace/.mavis/DEC-2026-07-22-055-...`
2. **Brainstorming Lab portal**: `portals/04-erp-system/decisions/` (4th portal)
3. **ERP-SYSTEM repo**: `AGENTS.md` (Jimi's responsibility to commit)

## 📊 Permissions Matrix

| Action | Anas | Mavis | Jimi | Lab |
|---|---|---|---|---|
| Read docs | ✅ | ✅ | ✅ | ✅ |
| Deploy to HF | ⚠️ | ✅ | ✅ | ❌ |
| Run Neon queries | ⚠️ | ✅ | ✅ | ⚠️ |
| Modify DEC files | ⚠️ | ✅ | ❌ | ✅ |
| Push to repos | ✅ | ✅ | ✅ | ❌ |

⚠️ = requires explicit approval per case

## 🚀 Action Items

- [x] Mavis creates this DEC
- [x] Mavis communicates tools to Jimi (via session 408773242015948)
- [ ] Jimi reads + acknowledges
- [ ] Jimi documents in ERP-SYSTEM AGENTS.md
- [ ] Lab updates brainstorming-lab portal with this DEC
- [ ] Both repos cross-link

## 📎 Related DECs

- DEC-043 — Neon API Key Received
- DEC-044 — ERD Generated (uses Neon)
- DEC-046 — Jimi تحليلي (new session)
- DEC-055 — THIS (HF + Neon control)

## 💬 Anas's words (verbatim):

> "جيمي قائد القائد، يقوم بعملية التطوير وكذا برنامج.. ودبلومنت ويشغل المساحة، النظام يشغله"
> "المفروض يكون عنده التوكن ويتحكم هو.. ويسكر الـ connection لما.. يقدر يتحكم في الـ neon ويسكر الـ connection"
