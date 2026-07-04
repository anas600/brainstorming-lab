# 🔄 Feedback Loop Protocol
## Session 002 ↔ ERP MVP Session

> **Protocol Name**: Closed-Loop Steering (التوجيه بالحلقات المغلقة)
> **Effective Date**: 2026-07-04
> **Parties**:
> - **Sender (this side)**: Brainstorming Lab Session 002 — Deep Researcher + 5 Expert Team
> - **Receiver (other side)**: ERP MVP Work Session `408773242015948` — Mavis (Coordinator) + 3 Agents

---

## 📜 Purpose (الهدف)

إنشاء حلقة وصل دائمة بين:
- **التحليل** (Brainstorming Lab) — 5 خبراء يراجعون
- **التنفيذ** (ERP MVP) — Mavis + 3 agents يبنون

**بحيث**:
- الـ decisions تتطوّر بناءً على real-world data
- الـ blockers تُكتشف مبكراً
- الـ spec adjustments تحصل بسرعة

---

## 📡 Communication Rules (قواعد الاتصال)

### 1️⃣ Exclusive Channel Rule
> **نتواصل حصرياً مع Mavis (المنسق/القائد في ERP Session)** — لا نتجاوزه للتحدث مع الـ sub-agents.
>
> **السبب**: Mavis يعمل وفق "Team Mavis Rules" — الـ sub-agents يتبعون orkestrator أعلى. تجاوزه = اختراق للـ hierarchy.

### 2️⃣ Message Format (قالب الرسالة)

```
## 📡 FROM [sender] — [type] #[N] (HH:MM)
**Session**: [id]
**Cycle**: [name]
**Priority**: 🟢 Normal | 🟡 Watch | 🔴 Critical | ⚫ Halt

### Topic:
[موضوع الرسالة]

### Body:
[المحتوى]

### Action Required:
[الإجراء المطلوب]

### Next:
[ما سيحدث]
```

### 3️⃣ Cadence (التردد)

| النوع | التردد | الـ Trigger |
|---|---|---|
| **Hourly Status Report** | كل ساعة | من ERP Session → إلينا |
| **Decision Request** | عند الحاجة | منا → ERP Session |
| **Blocker Alert** | فوري | من ERP Session → إلينا |
| **Cycle Closing Summary** | عند إغلاق كل دورة | منّا → ERP Session |

---

## 📊 Data Types (أنواع البيانات)

### ✅ Default (افتراضي):
- **Text reports** (status, blockers, decisions, summaries)
- **MD files** (markdown documentation) — كل التقارير على GitHub
- **Agents' docs** — التوثيق الذاتي الذي يكتبه كل agent في مجلده

### 🟡 Limited Cases (حالات محدودة):
- **Code diffs** — عند الحاجة لتغيير review
- **Log excerpts** — عند debugging

### 🔴 Rare (نادر جداً):
- **Direct chat with sub-agents** — فقط إذا:
  - Mavis نفسه يطلب ذلك
  - أو حالة طوارئ لا يستطيع Mavis حلها

---

## 🚨 Escalation Rules (قواعد التصعيد)

### Decision Conflicts (تعارض في القرارات):
- **القاعدة**: تصويت بين الفريقين (Brainstorming + ERP)
- **إذا تعادل**: المستخدم (أنس) يحسم
- **Deadline للتصويت**: 24 ساعة (بعدها escalation تلقائي)

### HALT Scenarios (حالات التعطل):

| السيناريو | الإجراء | المدة قبل التدخل |
|---|---|---|
| **Agents FAILED** | نُرسل rescue plan | 2 ساعات |
| **HALT بعد rescue** | فتح session جديدة للإنقاذ | 3 محاولات فاشلة |
| **Token Plan exhausted** | نطلب من المستخدم ترقية | فوري |
| **Communication break** | Re-verify session state | 1 ساعة |

---

## 📂 Documentation Structure (الهيكل التوثيقي)

```
feedback-loop/
├── protocol.md                    # هذا الملف
├── from-erp/                      # الرسائل الواردة من ERP
│   └── YYYY-MM-DD-HHMM-type.md
├── to-erp/                        # الرسائل الصادرة إلى ERP
│   └── YYYY-MM-DD-HHMM-type.md
└── decisions/                     # القرارات المتخذة
    └── DEC-YYYY-MM-DD-NNN-topic.md
```

### File Naming Convention (معايير التسمية):
- **Timestamps**: `YYYY-MM-DD-HHMM` (24-hour format)
- **Types**: `status`, `decision`, `blocker`, `cycle-summary`, `rescue-plan`
- **Decisions**: `DEC-NNN` (sequential numbering)

---

## 🎯 Success Criteria (معايير النجاح)

| المؤشر | الهدف |
|---|---|
| **Response Time** | < 1 ساعة من الطلب |
| **Decision Cycle Time** | < 24 ساعة من الطلب |
| **Documentation Coverage** | 100% (كل قرار موثّق) |
| **Cross-Reference** | 100% (كل دورة مرتبطة بالقرار) |

---

## 📊 Current Status (الحالة الحالية)

| البند | الحالة |
|---|---|
| Protocol | ✅ Active |
| ERP Session Reachable | ⚠️ لم يرد على ping |
| Latest Inbound Message | ⚠️ Hourly report @ 21:02 UTC |
| Latest Outbound Message | Ping @ 21:02 UTC |

---

*⚙️ Protocol Version 1.0 — 2026-07-04*
*Last Updated: 2026-07-04 23:30 Berlin*