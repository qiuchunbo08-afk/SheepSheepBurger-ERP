# SheepSheep Burger ERP - Diagrams

## 1. Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    USER ||--o{ ORDER : creates
    USER ||--o{ EMPLOYEE : manages
    ORDER ||--|{ ORDER_ITEM : contains
    ORDER_ITEM }o--|| BURGER_TYPE : refers_to
    ORDER ||--o{ PRODUCTION_ORDER : triggers
    PRODUCTION_ORDER }o--|| BURGER_TYPE : produces
    PRODUCTION_ITEM }o--|| MATERIAL : consumes
    PRODUCTION_ORDER ||--|{ PRODUCTION_ITEM : has
    MATERIAL ||--o{ PURCHASE_ORDER_ITEM : ordered_in
    PURCHASE_ORDER ||--|{ PURCHASE_ORDER_ITEM : contains
    PURCHASE_ORDER ||--|| INVOICE : generates
    ORDER ||--o{ INVOICE : billing
    EMPLOYEE ||--o{ PAYROLL : receives
    BURGER_TYPE ||--|| BOM : has
    BOM }o--|| MATERIAL : uses

    USER {
        int user_id PK
        string username
        string password
        string role
        string full_name
    }
    ORDER {
        int order_id PK
        string order_number
        string customer_name
        string channel
        date order_date
        string status
        float total_amount
        int user_id FK
    }
    ORDER_ITEM {
        int item_id PK
        int order_id FK
        int burger_id FK
        int quantity
        float unit_price
    }
    BURGER_TYPE {
        int burger_id PK
        string name
        float selling_price
        string description
    }
    PRODUCTION_ORDER {
        int prod_id PK
        string prod_number
        int burger_id FK
        int quantity
        string status
        string material_check
        date created_date
    }
    PRODUCTION_ITEM {
        int prod_item_id PK
        int prod_id FK
        int material_id FK
        float required_qty
    }
    MATERIAL {
        int material_id PK
        string name
        string unit
        float stock_qty
        float reorder_point
        float unit_price
    }
    BOM {
        int bom_id PK
        int burger_id FK
        int material_id FK
        float qty_per_unit
    }
    PURCHASE_ORDER {
        int po_id PK
        string po_number
        string supplier
        date order_date
        string status
        float total_amount
    }
    PURCHASE_ORDER_ITEM {
        int po_item_id PK
        int po_id FK
        int material_id FK
        int quantity
        float unit_price
    }
    INVOICE {
        int invoice_id PK
        string invoice_number
        string type
        string party_name
        float amount
        string status
        date invoice_date
    }
    EMPLOYEE {
        int emp_id PK
        string employee_number
        string name
        string role
        string department
        float hourly_wage
        int monthly_hours
        string status
    }
    PAYROLL {
        int payroll_id PK
        int emp_id FK
        date period
        float gross_pay
        float deductions
        float net_pay
    }
```

---

## 2. BPMN Business Process - Order-to-Cash

```mermaid
flowchart LR
    subgraph Customer
        A([Customer Order])
    end
    subgraph "SD - Cashier"
        B[Create Sales Order]
        C{Manager Approval}
        D[Order Processing]
        E[Delivery / Pickup]
    end
    subgraph "PP - Kitchen"
        F[Generate Production Order]
        G{Material Check}
        H[Start Production]
        I[Cook Burgers]
        J[Complete Production]
    end
    subgraph "MM - Warehouse"
        K[Deduct Inventory]
        L{Stock < Reorder?}
        M[Create Purchase Requisition]
    end
    subgraph "FI - Finance"
        N[Issue Invoice]
        O[Receive Payment]
    end
    subgraph "CO - Controlling"
        P[Calculate Cost]
        Q[Update Profit Margin]
    end

    A --> B --> C
    C -->|Reject| R([Order Cancelled])
    C -->|Approve| F --> G
    G -->|Insufficient| M
    G -->|Sufficient| H --> I --> K
    K --> L
    L -->|Yes| M
    L -->|No| J
    M --> J
    J --> D --> E --> N --> O
    O --> P --> Q
    Q --> S([Order Completed])
```

---

## 3. BPMN Business Process - Procure-to-Pay

```mermaid
flowchart LR
    subgraph "MM - Warehouse"
        A([Low Stock Alert])
        B[Create Purchase Requisition]
        C[Purchase Order]
        D[Goods Receipt]
    end
    subgraph "FI - Finance"
        E[Three-Way Match]
        F{Match OK?}
        G[Approve Invoice]
        H[Process Payment]
    end
    subgraph Supplier
        I([Deliver Goods])
        J([Receive Payment])
    end

    A --> B --> C --> I
    I --> D --> E
    E --> F
    F -->|No| K([Reject - Discrepancy])
    F -->|Yes| G --> H --> J
    J --> L([Procurement Complete])
```

---

## 4. System Architecture Diagram

```mermaid
flowchart TB
    subgraph "User Layer"
        U1[Admin]
        U2[Store Manager]
        U3[Kitchen Staff]
        U4[Finance Staff]
    end

    subgraph "Presentation Layer - Browser"
        UI[HTML/CSS/JS UI]
        NAV[Navigation & RBAC]
        FORMS[Forms & Validation]
    end

    subgraph "Application Layer"
        SD[SD Module<br/>Order Management]
        PP[PP Module<br/>Production & BOM]
        MM[MM Module<br/>Inventory & Procurement]
        FI[FI Module<br/>Invoices & Payments]
        CO[CO Module<br/>Cost Analysis]
        HCM[HCM Module<br/>Employee Roster]
    end

    subgraph "Data Layer"
        LS[(Browser<br/>localStorage)]
        DB[(JSON Data Store<br/>Users, Orders, Inventory, etc.)]
    end

    U1 & U2 & U3 & U4 --> UI
    UI --> NAV & FORMS
    NAV & FORMS --> SD & PP & MM & FI & CO & HCM
    SD & PP & MM & FI & CO & HCM --> LS
    LS --> DB
```

---

## 5. Module Interaction Map

```mermaid
flowchart LR
    SD[SD<br/>Sales Orders] -->|Order triggers| PP[PP<br/>Production]
    PP -->|BOM check| MM[MM<br/>Inventory]
    MM -->|Low stock| MM2[Procurement]
    MM2 -->|Goods receipt| FI[FI<br/>AP Invoice]
    PP -->|Production cost| CO[CO<br/>Costing]
    HCM[HCM<br/>Payroll] -->|Labor cost| CO
    HCM -->|Salary payment| FI
    SD -->|Customer billing| FI2[FI<br/>AR Invoice]
    CO -->|Pricing feedback| SD
```
