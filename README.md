# 🧠 مختبر العصف الذهني · Brainstorming Lab Hub

> **منصة موحدة** تجمع جميع المواقع/البوابات/اللوحات من مشروع "مختبر العصف الذهني" في رابط GitHub Pages واحد ثابت.

## 🎯 الهدف

مختبر العصف الذهني هو نظام لتوليد حلول عبر فرق خبراء افتراضيين. كل مشروع/جلسة ينتج:
- لوحات (HTML dashboards)
- ملفات تحليل (Markdown)
- تقارير (PDF/MD)

هذا الـ Hub **يجمعها كلها** في رابط ثابت واحد على GitHub Pages.

## 🌐 الرابط العام

> **`https://anas600.github.io/brainstorming-lab/`**

(رابط ثابت — لن يتغير مع التحديثات، فقط المحتوى يتحدث داخله)

## 📂 البنية (Repository Structure)

```
brainstorming-lab/
├── index.html              ← Hub الموحّد (نقطة الدخول الرئيسية)
├── README.md               ← هذا الملف
└── portals/
    ├── 00-erp-frd/         ← بوابة مشروع ERP-FRD (شركة قابضة ليبيرية)
    │   ├── index.html
    │   ├── readme.html
    │   ├── methodology.html
    │   ├── media-analysis-recs/index.html
    │   ├── telegram-integration-recs/index.html
    │   └── telegram-mystery/index.html
    └── 01-session-001/     ← جلسة 001 - FRD MVP (Brainstorming Lab)
        ├── home.html
        ├── session.html
        ├── cycle-1-ideation.html
        ├── analysis-holding-deep-dive.html
        └── cycle-2-critique.html
```

## 🔐 كلمات المرور

| البوابة | كلمة المرور |
|---|---|
| **ERP-FRD** | `work-pilot-pilot-316` |
| **Session 001** | `blab-001-frd-mvp-2026` |

## 🗂️ البوابات الحالية

### 📋 Portal #00 · ERP-FRD (v1.0)
- **التاريخ**: 30 يونيو 2026
- **المحتوى**: 6 صفحات (~70 KB)
- **الوصف**: تحليل متطلبات نظام ERP لشركة قابضة ليبيرية
- **الصفحات**: الرئيسية + README + Methodology + Telegram Mystery + 2 صفحات توصيات

### 🎯 Portal #01 · Session 001 (v3.0)
- **التاريخ**: 4 يوليو 2026
- **المرحلة**: Stage 2/5 من بروتوكول العصف الذهني
- **المحتوى**: 5 صفحات (~190 KB)
- **الفريق**: 5 خبراء (CFO، PM، Tech Lead، PO، ERP Implementation)
- **المخرجات**: 56 فكرة + 8 جداول + 10 ثغرات حرجة
- **الصفحات**:
  - `home.html` — Dashboard (مع كلمة مرور)
  - `session.html` — سجل الجلسة + Q&A + Decisions
  - `cycle-1-ideation.html` — 56 فكرة من 14 عملية
  - `analysis-holding-deep-dive.html` — تحليل عميق + 8 جداول
  - `cycle-2-critique.html` — نقد من 5 خبراء

## 🧠 بروتوكول مختبر العصف الذهني

### المهارات الأساسية:
1. **Analysis** (تحليل + SWOT)
2. **Discussion** (شريك حوار ذكي)
3. **Execution** = مختبر العصف الذهني (الأهم)

### 5 مراحل لكل جلسة:
| # | المرحلة | الحالة |
|---|---|---|
| 0 | توضيحات + Q&A → SESSION.md | ✅ مكتملة |
| 0.5 | لوحة الجلسة التفاعلية | ✅ مكتملة |
| 1 | الدورة 1 (Ideation) + Deep Dive | ✅ مكتملة |
| 2 | الدورة 2 (Critique) | ✅ مكتملة |
| 3 | الدورة 3 (Synthesis) | ⏳ مُعلّقة |
| 4 | التقرير النهائي | ⏳ مُعلّق |

## 🚀 آلية العمل (Dynamic Updates)

### عند إضافة ملف جديد للجلسة:
```bash
cd /workspace/.mavis/labs/brainstorming-lab/sessions/001-frd-accounting-mvp
bash rebuild.sh  # يُعيد بناء الـ portal
```

### عند إضافة بوابة جديدة:
1. أنشئ مجلد جديد تحت `portals/XX-name/`
2. ضع ملفات الـ HTML داخله
3. أضف بطاقة جديدة في `index.html` (Hub)
4. push للـ GitHub → GitHub Pages يحدث تلقائياً

## 🔧 كيف تشتغل كلمة المرور؟

كل بوابة فيها `index.html` أو `home.html` فيها:
- **Overlay gate** مع SHA-256 password check
- **Auth guards** على الصفحات الفرعية (تحوّل للبوابة إذا لم يكن مصرّح)

**ملاحظة**: في GitHub Pages، الـ password gate يعمل على **client-side** فقط — هذا للأمان الأساسي (يمنع الـ casual visitors)، ليس للأمان المؤسسي الحقيقي.

## 🤖 Credits

- **Built by**: Deep Researcher (Facilitator)
- **Powered by**: 
  - `analysis-portal` skill (توليد HTML)
  - `website_deploy` tool (نشر سابق على space.minimax.io)
  - **GitHub Pages** (نشر نهائي ثابت)
- **Hosted by**: GitHub Pages (anas600/brainstorming-lab)

## 📅 سجل الإصدارات

| التاريخ | الحدث |
|---|---|
| 2026-06-30 | ✅ Portal #00 (ERP-FRD) — أول بوابة |
| 2026-07-02 | ✅ Portal #01 v1 (Session 001 Setup + Cycle 1) |
| 2026-07-04 | ✅ Portal #01 v2 (+ Deep Dive) |
| 2026-07-04 | ✅ Portal #01 v3 (+ Cycle 2 Critique) |
| 2026-07-04 | ✅ **Hub الموحّد على GitHub Pages** |

## ⚖️ الترخيص

استخدام داخلي — Anas Assaket © 2026