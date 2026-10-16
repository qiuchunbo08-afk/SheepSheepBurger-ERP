# SheepSheep Burger ERP - Capstone Project

**ESE302 Capstone Project | Group: SheepSheep Burger | Version 1.0**

---

## 🚀 Quick Start

1. **Double-click `index.html`** - the system opens directly in your browser.
2. **Login** with any demo account below.
3. No installation, no server, no database needed.

---

## 👤 Demo Accounts

| Username | Password | Role | Access |
|----------|----------|------|--------|
| admin | admin123 | Administrator | All modules |
| manager | manager123 | Store Manager | SD, PP, MM, CO (view + approve) |
| kitchen | kitchen123 | Kitchen Staff | PP, MM (production & inventory) |
| finance | finance123 | Finance | FI, CO (payments & costing) |

---

## 📁 Project Files

| File | Description |
|------|-------------|
| `index.html` | **Main ERP prototype** - double-click to run |
| `02-SRS-Requirements.md` | Software Requirements Specification (20 FRs + 6 NFRs) |
| `03-Diagrams-ERD-BPMN-Architecture.md` | ERD, BPMN process flows, system architecture |
| `04-Test-Report.md` | Test report (20 test cases, UAT results) |

---

## 🧩 Modules Overview

### SD (Sales & Distribution)
- Create, approve, and track sales orders
- 3 burger types: Classic ($8.5), Cheese ($10.5), Double ($13.5)
- Order status flow: pending → processing → completed

### PP (Production Planning)
- Auto-generates production orders from approved sales
- BOM (Bill of Materials) reference for each burger
- Start production (deducts materials) → Finish production

### MM (Materials Management)
- Real-time inventory tracking (6 materials)
- Low-stock alerts (red badge when stock < reorder point)
- Purchase order creation → Goods receipt → inventory update

### FI (Financial Accounting)
- Accounts receivable & payable tracking
- Invoice approval / rejection workflow
- Three-way match simulation (PO = GR = Invoice)

### CO (Controlling)
- Per-burger cost breakdown: material + labor + overhead
- Profit margin calculation for each product
- Pricing decision support

### HCM (Human Capital Management)
- Employee roster with roles, departments, wages
- 6 employees across 4 departments

---

## 🔄 Core Business Processes

### Process 1: Order-to-Cash
```
Customer Order → SD Create Order → Manager Approval
→ PP Generate Production → MM Material Check
→ Kitchen Production → MM Deduct Inventory
→ Delivery → FI Invoice → CO Cost Calculation
```

### Process 2: Procure-to-Pay
```
Low Stock Alert → MM Create PO → Supplier Delivery
→ Goods Receipt → FI Three-Way Match
→ Approve Invoice → Process Payment
```

---

## 🛠️ Technology Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML5 + CSS3 + Vanilla JavaScript |
| Data Storage | Browser localStorage (JSON) |
| Architecture | Single-page application (SPA) |
| Deployment | Static file - open in any modern browser |

---

## 📊 Key KPIs on Dashboard

- Today's Revenue
- Pending Orders
- Low Stock Items
- Monthly Cost
- 7-day sales trend chart
- Profit margin by burger type

---

## ✅ Feature Checklist (vs. Capstone Requirements)

- [x] 4 user roles with authentication & RBAC
- [x] 6 core business entities with CRUD
- [x] 2 cross-module business processes (Order-to-Cash, Procure-to-Pay)
- [x] Approval / rejection decision node (invoice approval)
- [x] 4+ reports / KPIs on dashboard
- [x] Exception handling (low stock, invalid input, empty order)
- [x] Re-deployable (just open index.html)
- [x] Seed data included for demo

---

## 🎓 Presentation Tips

1. **Start with admin login** to show all modules.
2. **Demo flow**: Create order → Approve → Watch production auto-generate → Start production → See inventory decrease → Complete order.
3. **Show integration**: Go to MM after production, see stock reduced. Go to FI, see auto-generated invoice.
4. **Show CO**: Cost analysis table - explain margin calculation.
5. **Show RBAC**: Login as kitchen, show they can't see FI module.
6. **Demo exception**: Create an order for Double Burger when Patty is low → see "Low Stock" warning.

---

## 📞 Contact

SheepSheep Burger ERP Team | ESE302 Capstone | 2026
