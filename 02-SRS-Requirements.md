# SheepSheep Burger ERP - Software Requirements Specification (SRS)

**Project**: ESE302 Capstone Project  
**Version**: 1.0  
**Date**: 2026-10-12  
**Team**: SheepSheep Burger ERP Team  

---

## 1. Introduction

### 1.1 Purpose
This document specifies the functional and non-functional requirements for the SheepSheep Burger ERP system, a web-based ERP prototype for a burger fast-food restaurant covering six modules: SD (Sales & Distribution), PP (Production Planning), MM (Materials Management), FI (Financial Accounting), CO (Controlling), and HCM (Human Capital Management).

### 1.2 Scope
- **Industry**: Fast Food / Burger Restaurant
- **Modules**: SD, PP, MM, FI, CO, HCM (6 modules, 4-5 interconnected as required)
- **Business Processes**: 
  1. Order-to-Cash (SD → PP → MM → FI → CO)
  2. Procure-to-Pay (MM → FI)
- **User Roles**: Admin, Store Manager, Kitchen Staff, Finance Staff (4 roles)

### 1.3 Definitions
- **BOM**: Bill of Materials - recipe for each burger type
- **SD**: Sales & Distribution
- **PP**: Production Planning
- **MM**: Materials Management
- **FI**: Financial Accounting
- **CO**: Controlling (cost accounting)
- **HCM**: Human Capital Management

---

## 2. Overall Description

### 2.1 Product Perspective
The system is a standalone web-based ERP prototype built with HTML/CSS/JavaScript, using browser localStorage for data persistence. It simulates the core workflows of a burger restaurant's operations.

### 2.2 User Classes
| Role | Description | Permissions |
|------|-------------|--------------|
| Administrator | Full system access | All modules, user management |
| Store Manager | Oversees daily operations | SD, PP, MM, CO view + approve |
| Kitchen Staff | Production and inventory | PP, MM operations |
| Finance Staff | Payments and cost analysis | FI, CO operations |

### 2.3 Operating Environment
- Browser: Chrome 90+, Firefox 88+, Edge 90+
- No server required; runs locally via file:// protocol
- Data stored in browser localStorage

---

## 3. Functional Requirements

### 3.1 Authentication & Access Control

**FR-01: User Login**
- **Description**: Users must log in with username and password to access the system.
- **Priority**: High
- **Acceptance Criteria**: 
  - Given valid credentials, when user submits login form, then system grants access and shows role-specific dashboard.
  - Given invalid credentials, when user submits login form, then system displays "Invalid username or password" error.
  - Session persists until user clicks Logout.

**FR-02: Role-Based Access Control (RBAC)**
- **Description**: Different roles see only authorized modules and actions.
- **Priority**: High
- **Acceptance Criteria**:
  - Kitchen staff cannot access Finance module (FI).
  - Finance staff cannot create sales orders (SD).
  - Each role sees only permitted navigation items.

---

### 3.2 Sales & Distribution (SD) Module

**FR-03: Create Sales Order**
- **Description**: Cashier/Manager can create a new sales order with customer info, channel, and burger quantities.
- **Priority**: High
- **Acceptance Criteria**:
  - Given a customer order, when user selects burger types and quantities, then system calculates total price automatically.
  - Prices: Classic Burger = $8.50, Cheese Burger = $10.50, Double Burger = $13.50.
  - Order ID auto-generated in format ORD-XXX.
  - Order status starts as "pending".

**FR-04: Approve Sales Order**
- **Description**: Store Manager can approve pending orders, triggering production.
- **Priority**: High
- **Acceptance Criteria**:
  - Given a pending order, when manager clicks "Approve", then order status changes to "processing".
  - System automatically creates corresponding production orders in PP module.
  - System checks material availability and flags low-stock items.

**FR-05: Complete Sales Order**
- **Description**: Order marked as completed after production and delivery.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Given a processing order, when user clicks "Complete", then order status changes to "completed".
  - Revenue is added to daily total on dashboard.

---

### 3.3 Production Planning (PP) Module

**FR-06: Production Order Generation**
- **Description**: Production orders are auto-created from approved sales orders based on BOM.
- **Priority**: High
- **Acceptance Criteria**:
  - Each burger type in an approved order generates one production order.
  - Production ID format: PROD-XXX.
  - Material availability checked: "Sufficient" or "Low Stock" badge shown.

**FR-07: Start Production (Material Issue)**
- **Description**: Kitchen staff starts production, deducting raw materials from inventory.
- **Priority**: High
- **Acceptance Criteria**:
  - Given a pending production order with sufficient materials, when kitchen staff clicks "Start", then:
    - Order status changes to "processing".
    - Raw materials deducted from MM inventory based on BOM.
    - BOM: Classic = 1 bun + 1 patty + 50g veg + 20g sauce; Cheese adds 1 cheese slice; Double = 2 patties.

**FR-08: Complete Production**
- **Description**: Kitchen marks production order as completed after cooking.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Given a processing production order, when kitchen clicks "Finish", then status becomes "completed".

**FR-09: BOM Reference Display**
- **Description**: System displays the Bill of Materials for each burger type.
- **Priority**: Low
- **Acceptance Criteria**:
  - BOM table shown on PP page with ingredients per burger type.

---

### 3.4 Materials Management (MM) Module

**FR-10: Inventory List Display**
- **Description**: System shows all materials with stock quantity, reorder point, and unit price.
- **Priority**: High
- **Acceptance Criteria**:
  - Table displays: Material Name, Unit, In Stock, Reorder Point, Status, Unit Price.
  - Low stock items (stock < reorder point) highlighted with red "Low" badge.

**FR-11: Create Purchase Order (Procurement)**
- **Description**: Manager can create purchase orders for materials.
- **Priority**: High
- **Acceptance Criteria**:
  - User selects supplier, material, and quantity.
  - System calculates total = unit price × quantity.
  - PO ID format: PO-XXX.
  - Auto-creates corresponding payable invoice in FI module.

**FR-12: Goods Receipt**
- **Description**: When PO is approved, receiving goods updates inventory.
- **Priority**: High
- **Acceptance Criteria**:
  - Given an approved PO, when user clicks "Goods Receipt", then:
    - PO status becomes "completed".
    - Inventory stock increased by received quantity.
    - Three-way match concept simulated (PO = Goods Receipt = Invoice).

---

### 3.5 Financial Accounting (FI) Module

**FR-13: Invoice List Display**
- **Description**: System shows all invoices (receivables and payables) with approval status.
- **Priority**: High
- **Acceptance Criteria**:
  - Table displays: Invoice ID, Type (Receivable/Payable), Party, Amount, Status, Action.

**FR-14: Invoice Approval / Rejection**
- **Description**: Finance staff can approve or reject pending invoices.
- **Priority**: High
- **Acceptance Criteria**:
  - Given a pending invoice, when finance clicks "Approve", status becomes "approved".
  - When "Reject" clicked, status becomes "cancelled".
  - Only Finance role can perform this action.

**FR-15: Dashboard Financial Summary**
- **Description**: FI summary cards show Receivables, Payables, Net Cash Flow.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Three stat cards displayed on FI page with current totals.

---

### 3.6 Controlling (CO) Module

**FR-16: Product Cost Analysis**
- **Description**: System calculates cost and profit per burger type.
- **Priority**: High
- **Acceptance Criteria**:
  - Table shows: Selling Price, Material Cost, Labor Cost, Overhead, Total Cost, Profit/Unit, Margin %.
  - Margin = (Profit / Selling Price) × 100%.

---

### 3.7 Human Capital Management (HCM) Module

**FR-17: Employee Roster**
- **Description**: System displays all employees with role, department, wage, and hours.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Table shows: ID, Name, Role, Department, Hourly Wage, Monthly Hours, Status.

---

### 3.8 Dashboard & Reporting

**FR-18: Dashboard KPI Cards**
- **Description**: Home dashboard shows 4 key metrics.
- **Priority**: High
- **Acceptance Criteria**:
  - Today's Revenue, Pending Orders, Low Stock Items, Monthly Cost.
  - Each card shows trend indicator (up/down arrow).

**FR-19: Sales Trend Chart**
- **Description**: Bar chart showing last 7 days sales.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Bar chart on dashboard with 7 bars, each labeled by day.

**FR-20: Profit Margin Chart**
- **Description**: Bar chart showing profit margin by burger type.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Bar chart comparing Classic, Cheese, Double burger margins.

---

## 4. Non-Functional Requirements

**NFR-01: Response Time**
- **Description**: System page load and data update response time.
- **Priority**: Medium
- **Acceptance Criteria**:
  - All page transitions and data updates complete within 1 second on a modern browser with <1000 records.

**NFR-02: Browser Compatibility**
- **Description**: System works on major modern browsers.
- **Priority**: High
- **Acceptance Criteria**:
  - Tested on Chrome 90+, Edge 90+, Firefox 88+.
  - Layout consistent across browsers.

**NFR-03: Data Persistence**
- **Description**: Data survives browser refresh and close.
- **Priority**: High
- **Acceptance Criteria**:
  - All data stored in localStorage; persists after browser restart.
  - Maximum localStorage usage < 5MB.

**NFR-04: Usability**
- **Description**: System is usable by restaurant staff with basic computer skills.
- **Priority**: Medium
- **Acceptance Criteria**:
  - Each function reachable within 3 clicks from dashboard.
  - Chinese/English interface labels clear and intuitive.

**NFR-05: Security**
- **Description**: Basic access control prevents unauthorized module access.
- **Priority**: Medium
- **Acceptance Criteria**:
  - User must login to access any page.
  - Role-based navigation hides unauthorized modules.

**NFR-06: Deployability**
- **Description**: System can be deployed on any computer without installation.
- **Priority**: High
- **Acceptance Criteria**:
  - Single HTML file; double-click to open in browser.
  - No server, database, or software installation required.

---

## 5. Requirements Traceability Matrix (RTM)

| Req ID | Module | Design Element | Test Case | Status |
|--------|--------|----------------|------------|--------|
| FR-01 | Auth | Login page, user model | TC-01 | ✅ |
| FR-02 | Auth | RBAC navigation | TC-02 | ✅ |
| FR-03 | SD | Order form, order model | TC-03 | ✅ |
| FR-04 | SD | Approve button, auto-PP | TC-04 | ✅ |
| FR-05 | SD | Complete order action | TC-05 | ✅ |
| FR-06 | PP | Auto production gen | TC-06 | ✅ |
| FR-07 | PP | Start production, material deduct | TC-07 | ✅ |
| FR-08 | PP | Finish production | TC-08 | ✅ |
| FR-09 | PP | BOM table | TC-09 | ✅ |
| FR-10 | MM | Inventory table | TC-10 | ✅ |
| FR-11 | MM | Procurement form | TC-11 | ✅ |
| FR-12 | MM | Goods receipt action | TC-12 | ✅ |
| FR-13 | FI | Invoice table | TC-13 | ✅ |
| FR-14 | FI | Approve/reject invoice | TC-14 | ✅ |
| FR-15 | FI | Financial summary cards | TC-15 | ✅ |
| FR-16 | CO | Cost analysis table | TC-16 | ✅ |
| FR-17 | HCM | Employee roster table | TC-17 | ✅ |
| FR-18 | Dashboard | KPI stat cards | TC-18 | ✅ |
| FR-19 | Dashboard | Sales bar chart | TC-19 | ✅ |
| FR-20 | Dashboard | Profit margin chart | TC-20 | ✅ |

---

## 6. Assumptions and Constraints

### 6.1 Assumptions
- The restaurant has 6 employees across 4 departments.
- Three standard burger types only (Classic, Cheese, Double).
- Initial inventory data provided by management.
- Browser localStorage is sufficient for demo-scale data.

### 6.2 Constraints
- No backend server; data is browser-side only.
- No real payment gateway integration.
- No multi-restaurant / multi-branch support.
- English-language interface for demo purposes.
