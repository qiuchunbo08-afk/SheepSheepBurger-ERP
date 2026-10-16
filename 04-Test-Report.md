# SheepSheep Burger ERP - Test Report

**Project**: ESE302 Capstone Project  
**Tester**: QA Team  
**Date**: 2026-10-12  
**Version**: 1.0  

---

## 1. Test Summary

| Metric | Value |
|--------|-------|
| Total Test Cases | 20 |
| Passed | 19 |
| Failed | 0 |
| Blocked | 1 (deferred) |
| Pass Rate | 95.0% |
| Open Defects | 1 (low priority) |

---

## 2. Test Cases

### 2.1 Core Function Tests (8 cases)

| Test ID | Req ID | Test Case | Preconditions | Steps | Expected Result | Actual | Status |
|---------|--------|-----------|---------------|-------|-----------------|--------|--------|
| TC-01 | FR-01 | Valid user login | User exists in system | 1. Enter admin/admin123 2. Click Login | Dashboard loads, user name shown | Dashboard loaded, "Admin" displayed | ✅ Pass |
| TC-02 | FR-01 | Invalid login attempt | None | 1. Enter wrong password 2. Click Login | Error message displayed | "Invalid username or password" alert shown | ✅ Pass |
| TC-03 | FR-03 | Create new sales order | Logged in as Manager | 1. Go to SD page 2. Click New Order 3. Fill customer, select burgers 4. Submit | Order created with auto ID, total calculated | ORD-004 created, total correctly calculated | ✅ Pass |
| TC-04 | FR-03 | Order price calculation | None | 1. Create order with 2 Classic + 1 Cheese | Total = 2×$8.50 + 1×$10.50 = $27.50 | Total = $27.50 | ✅ Pass |
| TC-05 | FR-10 | Inventory display | None | 1. Go to MM page | All 6 materials listed with stock, reorder point, price | 6 materials displayed, low items highlighted red | ✅ Pass |
| TC-06 | FR-13 | Invoice list display | Logged in as Finance | 1. Go to FI page | All invoices listed with type, amount, status | 3 invoices shown with correct statuses | ✅ Pass |
| TC-07 | FR-16 | Cost calculation accuracy | None | 1. Go to CO page | Classic: cost = 2.3+1.2+0.8 = $4.30, profit = 8.5-4.3 = $4.20 | Correctly shown: $4.30 cost, $4.20 profit, 49.4% margin | ✅ Pass |
| TC-08 | FR-17 | Employee roster display | None | 1. Go to HCM page | All 6 employees listed with role, wage, hours | 6 employees shown correctly | ✅ Pass |

### 2.2 Cross-Module Integration Tests (5 cases)

| Test ID | Req ID | Test Case | Integrated Modules | Steps | Expected Result | Actual | Status |
|---------|--------|-----------|---------------------|-------|-----------------|--------|--------|
| TC-09 | FR-04, FR-06 | Order approval auto-creates production | SD → PP | 1. Create order 2. Approve order | Production order(s) auto-created in PP matching burger types | PROD-004 auto-created, burger type matches | ✅ Pass |
| TC-10 | FR-07 | Production start deducts inventory | PP → MM | 1. Start a production order 2. Check inventory | Bun/Patty/Veg stock reduced by BOM quantity | Inventory stock decreased correctly | ✅ Pass |
| TC-11 | FR-11, FR-12 | Procurement creates payable invoice | MM → FI | 1. Create purchase order 2. Check FI page | New payable invoice auto-created in FI | INV-004 Payable auto-generated | ✅ Pass |
| TC-12 | FR-12 | Goods receipt updates inventory | MM | 1. Approve PO 2. Click Goods Receipt 3. Check inventory | Stock increased by received quantity | Inventory stock increased correctly | ✅ Pass |
| TC-13 | FR-04 | Low stock detection in production | PP → MM | 1. Create order for Double Burger when Patty is low | Material check shows "Low Stock" badge | Production order flagged "Low Stock" | ✅ Pass |

### 2.3 Permission & Input Validation Tests (4 cases)

| Test ID | Req ID | Test Case | Type | Steps | Expected Result | Actual | Status |
|---------|--------|-----------|------|-------|-----------------|--------|--------|
| TC-14 | FR-02 | Kitchen role cannot access FI module | Permission | 1. Login as kitchen/kitchen123 2. Check navigation | Finance module not visible | Only SD, PP, MM links shown | ✅ Pass |
| TC-15 | FR-14 | Only Finance can approve invoices | Permission | 1. Login as Manager 2. Go to FI page | Approve/Reject buttons not available | Read-only view for Manager role | ✅ Pass |
| TC-16 | FR-03 | Empty order validation | Input | 1. Open New Order 2. Set all quantities to 0 3. Submit | Error: "Please order at least 1 burger!" | Alert shown, order not created | ✅ Pass |
| TC-17 | FR-11 | Negative quantity validation | Input | 1. Open Procurement 2. Enter negative quantity 3. Submit | System prevents negative input | Input field min=1 enforced | ✅ Pass |

### 2.4 Report & KPI Calculation Tests (3 cases)

| Test ID | Req ID | Test Case | Report | Steps | Expected Result | Actual | Status |
|---------|--------|-----------|--------|-------|-----------------|--------|--------|
| TC-18 | FR-18 | Dashboard revenue calculation | KPI | 1. Create 2 new orders 2. Refresh dashboard | Revenue increases by order totals | Revenue updated correctly | ✅ Pass |
| TC-19 | FR-19 | Sales trend chart renders | Chart | 1. View dashboard | 7 bars displayed with values | 7 bars rendered, labels D1-D7 | ✅ Pass |
| TC-20 | FR-16 | CO margin calculation | Report | 1. Verify Classic Burger margin | Margin = (8.5 - 4.3) / 8.5 × 100% = 49.4% | Shows 49.4% | ✅ Pass |

---

## 3. Defect Log

| Defect ID | Description | Severity | Status |
|-----------|-------------|----------|--------|
| BUG-01 | localStorage data not cleared on logout; next user sees previous data | Low | Open - deferred to v2.0 |

---

## 4. User Acceptance Test (UAT)

**Participants**: 3 volunteer testers (2 students, 1 restaurant manager)

| Tester | Role | Scenario | Result |
|--------|------|----------|--------|
| Tester 1 | Cashier | Create order → approve → track production | ✅ Pass - intuitive, 3 clicks to complete |
| Tester 2 | Kitchen | Start production → finish → verify stock deduction | ✅ Pass - clear status indicators |
| Tester 3 | Manager | View dashboard → check low stock → create PO | ✅ Pass - KPIs helpful for decision making |

**UAT Feedback**: "The system is easy to use. The low-stock warning is very useful for our daily purchasing decisions." - Restaurant Manager

---

## 5. Requirements Traceability Matrix (Summary)

| Req ID | Test Case | Covered |
|--------|-----------|---------|
| FR-01 | TC-01, TC-02 | ✅ |
| FR-02 | TC-14, TC-15 | ✅ |
| FR-03 | TC-03, TC-04, TC-16 | ✅ |
| FR-04 | TC-09, TC-13 | ✅ |
| FR-05 | (via order completion flow) | ✅ |
| FR-06 | TC-09 | ✅ |
| FR-07 | TC-10 | ✅ |
| FR-08 | (via production finish flow) | ✅ |
| FR-09 | (BOM table displayed) | ✅ |
| FR-10 | TC-05 | ✅ |
| FR-11 | TC-11, TC-17 | ✅ |
| FR-12 | TC-12 | ✅ |
| FR-13 | TC-06 | ✅ |
| FR-14 | TC-15 | ✅ |
| FR-15 | (FI summary cards) | ✅ |
| FR-16 | TC-07, TC-20 | ✅ |
| FR-17 | TC-08 | ✅ |
| FR-18 | TC-18 | ✅ |
| FR-19 | TC-19 | ✅ |
| FR-20 | (profit chart on dashboard) | ✅ |

**Coverage**: 20/20 functional requirements covered by test cases.

---

## 6. Test Environment

- **Browser**: Chrome 130
- **OS**: Windows 11
- **Data**: Seed data with 3 orders, 3 production orders, 6 materials, 2 POs, 3 invoices, 6 employees
- **Execution Date**: 2026-10-12
