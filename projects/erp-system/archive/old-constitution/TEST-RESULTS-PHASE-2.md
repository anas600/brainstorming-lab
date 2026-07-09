# 🧪 Phase 2 — Smoke Test Results

> **Test execution date**: 2026-07-06
> **Test environment**: https://anas-assaket-erp-system.hf.space
> **Credentials**: admin@alfajr.local / Demo1234
> **Total tests run**: 18 (out of 39 in plan)
> **Pass rate**: 78% (14/18 — some bugs expected)

---

## ✅ Working Tests (14)

| Test | Endpoint | Result |
|---|---|---|
| T-HEALTH-LIVE | `/api/health/live` | ✅ 200 |
| T-HEALTH-STARTUP-DEEP | `/api/health/startup-deep` | ✅ 200, full state |
| T-AUTH-001 | `/api/auth/login` | ✅ 200, JWT returned |
| T-AUTH-003 | `/api/auth/me` | ✅ 200, user info |
| T-FIN-001 | `/api/finance/accounts` | ✅ 200, 17 accounts |
| T-FIN-002 | `/api/finance/journal-entries?take=N` | ✅ 200, 50+ entries |
| T-FIN-009 | `/api/finance/ledger/trial-balance` | ✅ 200, 47 accounts |
| T-INV-001 | `/api/inventory/items` | ✅ 200, items |
| T-INV-002 (found) | `/api/procurement/grs` (correct path) | ✅ 200, 50 GRs |
| T-PROJ-001 | `/api/projects` | ✅ 200, 3 projects |
| T-PROJ-002 | `/api/companies` | ✅ 200, 1 company |
| T-FIN-007 | `/api/procurement/bills` | ✅ 200, 50 bills |
| T-X (partial) | `/api/ar/customers` | ✅ 200 (empty) |
| T-X (partial) | `/api/ar/receipts` | ✅ 200 |

## ❌ Failing Tests (4) — Real Bugs

### T-FIN-006: ⭐ Create Vendor — **400 Bad Request**

**Request**:
```json
POST /api/procurement/vendors
{
  "name": "Test Vendor",
  "email": "test@vendor.com",
  "phone": "+218912345678",
  "taxId": "TAX123456",
  "address": "Tripoli",
  "contactPerson": "Ahmed"
}
```

**Response**: 
```json
{
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "Code": [
      "'Code' must not be empty.",
      "Code: حروف/أرقام/-/_ فقط."
    ]
  }
}
```

**Root Cause**: Form expects `Code` field (not just name/taxId). Frontend missing this field.

**Fix**: 
- Backend: keep validation (correct)
- Frontend: add `Code` field to form
- Make `Code` auto-generated if not provided (V-005, V-006, etc.)

### T-DATA-X: Bills have empty lines

**Issue**: `bills.lines = []` for all 50 bills  
**Root Cause**: BillGenerator not creating line items  
**Impact**: Cannot calculate bill totals accurately; bills = floating headers

### T-DATA-X: Future-dated Journal Entries

**Issue**: `entryDate: "2026-12-31"` (Dec 2026 — 5 months in future)  
**Root Cause**: Payroll seeder generating payroll for future months  
**Impact**: Confusing — system looks "in the future"

### T-DATA-X: Trial Balance Imbalanced

**Issue**: Account `1210 (Cash)` has 4,223,555.00 LYD CREDIT but 0 DEBIT  
**Root Cause**: Seed pays expenses (debit) without recording corresponding revenue/income  
**Impact**: Books don't balance (Trial Balance fails)

## ⚠️ Notable Findings

### Endpoint Path Confusion (Discovery)

After source code analysis, the actual API surface uses these prefixes:
- `/api/finance/...` — Chart of Accounts, Ledger, Journal Entries
- `/api/procurement/...` — Vendors, GRs, Bills, POs
- `/api/ar/...` — Accounts Receivable (Customers, Sales Invoices, Receipts)
- `/api/inventory/...` — Items, Stock
- `/api/projects/...` — Projects

### Date Range Issues (Seed Quality)

- **50 GRs all in 1-month window**: 2026-06-08 to 2026-07-05
- **No backdated entries** (cannot easily test "Q1 2026" reports)
- **All timestamps have full clock time** (10:36:49, etc.) — all within minutes of each other (seeded together)

### UI Patterns

- Long UUIDs displayed (e.g., `6d97b2e8-e4f4-46ad-9a54-d19f895183b4`)
- Frontend shows raw data model fields

## 📋 Bug Inventory (Confirmed)

| # | ID | Severity | Title | Phase |
|---|---|---|---|---|
| 1 | BUG-001 | 🔴 High | Vendor 400 error (missing Code field) | Phase 4 |
| 2 | BUG-002 | 🔴 High | GR dates unrealistic (1-month only) | Phase 3 (seed) |
| 3 | BUG-003 | 🟡 Medium | Bill lines empty | Phase 3 (seed) + 4 |
| 4 | BUG-004 | 🟡 Medium | Future-dated Journal Entries | Phase 3 |
| 5 | BUG-005 | 🟡 Medium | Trial Balance imbalanced | Phase 3 |
| 6 | BUG-006 | 🟡 Medium | Empty customers list | Phase 3 |
| 7 | BUG-007 | 🟡 Medium | Long UUIDs in UI | Phase 4 |
| 8 | BUG-008 | 🟡 Medium | Date locale not enforced | Phase 4 |
| 9 | BUG-009 | 🟢 Low | Discount column not modeled | Phase 4 |
| 10 | BUG-010 | 🟡 Medium | Customer screen shows nothing | Phase 3 |

## 🎯 Next Phases

### Phase 3 (Seed replacement) — Priority

The current seed has multiple data quality issues that affect user trust in the system.

Plan:
1. **Backup current state** (for rollback if needed)
2. **Design 2-year realistic scenario**:
   - 5 subsidiaries (matching Libyan holding)
   - Period: 2024-07 to 2026-07 (24 months)
   - Vendors: ~15 suppliers (realistic Libyan businesses)
   - Customers: ~20 clients
   - Projects: 5-10 active/completed projects
   - GRs: ~100 with proper distribution over 24 months
   - Bills: 100 with line items
   - Sales Invoices: 50
   - Journal Entries: 200+ including payroll, expenses, revenue
   - Trial Balance balances correctly

3. **Implementation**:
   - Backup current seed
   - New comprehensive seeder
   - Reset + re-seed database

### Phase 4 (Bug fixes) — After Phase 3

1. Add `Code` auto-generation for vendors
2. Format dates properly in UI (dd/MM/yyyy)
3. Hide UUIDs, show friendly codes
4. Improve vendor form UX
5. Test all UI flows
