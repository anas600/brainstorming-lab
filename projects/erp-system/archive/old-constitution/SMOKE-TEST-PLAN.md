# 🧪 ERP-SYSTEM Smoke Test Plan

> **Comprehensive end-to-end testing plan**
> **Goal**: Verify all critical user flows work correctly in the deployed system
> **Owner**: Analytical Team (Deep Researcher + 5 experts)
> **Test environment**: https://anas-assaket-erp-system.hf.space

---

## 🎯 Test Categories (5 experts designed):

### 1. Authentication & Multi-Tenant Tests
### 2. Finance Module Tests
### 3. Projects Module Tests
### 4. Inventory Module Tests
### 5. Cross-Module Integration Tests
### 6. UI/UX Quality Tests
### 7. Data Integrity Tests
### 8. Performance Baseline Tests
### 9. Audit & Events Verification

---

## 1️⃣ Authentication & Multi-Tenant Tests

### T-AUTH-001: Login with valid credentials
- **Action**: POST /api/auth/login with admin@alfajr.local / Demo1234
- **Expected**: 200 OK + JWT token
- **Risk if fails**: Critical (blocks all other tests)

### T-AUTH-002: Login with invalid credentials
- **Action**: POST /api/auth/login with wrong password
- **Expected**: 401 Unauthorized with clear error message

### T-AUTH-003: JWT Token validation
- **Action**: Use valid token on protected endpoint
- **Expected**: 200 OK

### T-AUTH-004: Expired/invalid token rejection
- **Action**: Use tampered token
- **Expected**: 401 Unauthorized

### T-AUTH-005: Tenant isolation in queries
- **Action**: Create second tenant, query data — verify no cross-tenant access
- **Expected**: Empty results for cross-tenant queries

---

## 2️⃣ Finance Module Tests

### T-FIN-001: List Chart of Accounts
- **Action**: GET /api/finance/accounts
- **Expected**: 17+ accounts in default CoA
- **Check**: Account codes (1100, 2100, etc.) present

### T-FIN-002: List Journal Entries
- **Action**: GET /api/finance/journal-entries
- **Expected**: Paged response, entries with valid status

### T-FIN-003: Create Draft Journal Entry
- **Action**: POST /api/finance/journal-entries
- **Payload**: Balanced debit/credit
- **Expected**: 201 Created + entry_number

### T-FIN-004: Post Journal Entry (Draft → Posted)
- **Action**: POST /api/finance/journal-entries/{id}/post
- **Expected**: Status = Posted, GL updated

### T-FIN-005: List Vendors
- **Action**: GET /api/procurement/vendors (or similar)
- **Expected**: Paged vendor list

### **T-FIN-006: ⭐ Create Vendor (PRIORITY — user's reported bug)**
- **Action**: POST /api/procurement/vendors
- **Payload**: { name: "Test Vendor", email: "valid@email.com", phone: "+218912345678", taxId: "TAX123" }
- **Expected**: 201 Created
- **Current bug**: 400 Bad Request (need to debug)

### T-FIN-007: List Bills
- **Action**: GET /api/finance/bills
- **Expected**: Bills with realistic dates and amounts

### T-FIN-008: Create Bill
- **Action**: POST /api/finance/bills
- **Expected**: 201 Created + bill_number

### T-FIN-009: Get Trial Balance
- **Action**: GET /api/finance/ledger/trial-balance
- **Expected**: Balanced totals (debits = credits)

---

## 3️⃣ Projects Module Tests

### T-PROJ-001: List Projects
- **Action**: GET /api/projects
- **Expected**: Project list with statuses

### T-PROJ-002: Get Project Cost (Sprint-4.5 T-013 future)
- **Action**: GET /api/projects/{id}/cost
- **Expected**: Actual cost = sum of invoices

### T-PROJ-003: Cross-module: invoice → project cost
- **Action**: Create invoice with ProjectId, verify project cost updated
- **Expected**: Project actual_cost reflects invoice

---

## 4️⃣ Inventory Module Tests

### T-INV-001: List Items
- **Action**: GET /api/inventory/items
- **Expected**: Item list with quantities

### T-INV-002: Goods Receipts (⭐ user's screenshot)
- **Action**: GET /api/inventory/goods-receipts
- **Expected**: GR list with REALISTIC dates (not random)
- **Current bug**: Random dates 04/07/2026, 02/07/2026, 20/06/2026 (no pattern)

### T-INV-003: List Customers
- **Action**: GET /api/sales/customers
- **Expected**: Customer list

---

## 5️⃣ Cross-Module Integration Tests

### T-X-001: Invoice affects Project cost
- **Action**: POST invoice with ProjectId → check project cost
- **Expected**: Cost auto-updated via domain event

### T-X-002: GR triggers PostingRule
- **Action**: POST goods-receipt → check Journal Entry created
- **Expected**: Stock received → JE (1300 Dr / 2100 Cr)

### T-X-003: Soft delete + restore
- **Action**: DELETE vendor (soft) → check excluded from list → RESTORE → check returned
- **Expected**: Soft delete hidden, restore returns

---

## 6️⃣ UI/UX Quality Tests (manual via browser)

### T-UI-001: Date format
- **Action**: Visual check on GR list
- **Expected**: dd/MM/yyyy format (Libyan standard)
- **Current bug**: Mixed/unclear dates

### T-UI-002: Column clarity
- **Action**: Visual check on GR list
- **Expected**: Clear column headers, readable data
- **Current bug**: Long UUIDs instead of human-friendly IDs

### T-UI-003: Discount column
- **Action**: Visual check on Bills list
- **Expected**: Either populated or hidden (not "0.00" everywhere)
- **Current bug**: All 0.00

### T-UI-004: Add Vendor form
- **Action**: Click "Add Vendor", fill form, submit
- **Expected**: Success message, vendor in list
- **Current bug**: 400 error

### T-UI-005: RTL support (Arabic)
- **Action**: Visual check
- **Expected**: Proper RTL layout, no broken text
- **Current bug**: Mixed text direction visible

### T-UI-006: Bilingual display
- **Action**: Check column headers
- **Expected**: Clear Arabic labels
- **Current bug**: Some headers unclear

---

## 7️⃣ Data Integrity Tests

### T-DATA-001: Account hierarchy
- **Action**: GET /api/finance/accounts/{id} for parent
- **Expected**: Returns children accounts

### T-DATA-002: Journal Entry balance
- **Action**: GET /api/finance/journal-entries/{id}
- **Expected**: Sum debit = Sum credit

### T-DATA-003: Tenant isolation
- **Action**: Login as different tenant, query data
- **Expected**: Different results per tenant

### T-DATA-004: Audit log (Sprint-4.5 T-009)
- **Action**: Create entity, query audit_log
- **Expected**: CREATE event logged

---

## 8️⃣ Performance Baseline Tests

### T-PERF-001: Login response time
- **Action**: Time POST /api/auth/login
- **Expected**: < 500ms

### T-PERF-002: List endpoint latency
- **Action**: Time GET /api/inventory/goods-receipts
- **Expected**: < 1s for 100 records

### T-PERF-003: Health endpoint latency
- **Action**: Time GET /api/health/live
- **Expected**: < 100ms

---

## 9️⃣ Audit & Events Verification

### T-AUDIT-001: Audit log after create
- **Action**: POST vendor → query audit_log
- **Expected**: Entry created with entity_type=Vendor

### T-AUDIT-002: Event firing (Sprint-4.5 T-010)
- **Action**: POST invoice → check event handler ran
- **Expected**: Project cost updated (cross-module event)

### T-AUDIT-003: Soft delete audit (Sprint-4.5 T-011)
- **Action**: DELETE entity → check audit shows DELETE action
- **Expected**: Audit entry with deleted_at = now

---

## 📊 Test Execution Plan

### Execution Order:
1. **API tests first** (T-AUTH to T-AUDIT) — using curl
2. **UI tests** (T-UI-*) — via Mavis (can interact with browser-like flows)
3. **Performance tests** (T-PERF-*) — backend latency

### Tools:
- **API tests**: curl + Python (in this session)
- **UI tests**: Mavis (interactive)
- **Reports**: Smoke test results document

### Success Criteria:
- All T-AUTH tests pass
- 90%+ of API tests pass
- All UI tests pass (with fixes)
- All performance tests within thresholds
- All audit/event tests pass

---

## 📋 Per-Phase Status Tracking

This plan will be updated as tests run:

- [x] Phase 1: Plan Designed ✅
- [ ] Phase 2: Test Execution (in progress)
- [ ] Phase 3: Bug Inventory (after tests)
- [ ] Phase 4: Fixes (Mavis)
- [ ] Phase 5: Re-test (analytical team)
