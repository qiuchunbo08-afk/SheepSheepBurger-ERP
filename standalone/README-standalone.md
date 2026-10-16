# SheepSheep Burger ERP - 模块独立运行版

本目录是从主程序 `index.html` 拆分出的 **6 个模块独立运行文件**，每人/每模块一个文件，互不依赖、双击即用；数据库层统一引用上级目录的 `database.js`（单独提取的数据库文件）。

## 运行方式

直接双击任意 HTML 文件，用浏览器打开即可，无需安装、无需服务器。

登录账号（与原系统一致）：

| Username | Password | Role |
|----------|----------|------|
| admin | admin123 | Administrator |
| manager | manager123 | Store Manager |
| kitchen | kitchen123 | Kitchen Staff |
| finance | finance123 | Finance |

## 文件与模块对应

| 文件 | 模块 | 负责人对应 | 主要功能 |
|------|------|-----------|---------|
| `CO-Controlling.html` | 成本控制 (CO) | 成本负责人 | 产品成本分析表、利润率、成本结构堆叠图、KPI 卡片、BOM 参考、成本假设 |
| `SD-Sales.html` | 销售与分销 (SD) | 销售负责人 | 销售订单列表、新建订单、审批(Approve)、完成(Complete)、统计卡片 |
| `PP-Production.html` | 生产计划 (PP) | 生产负责人 | 生产订单列表、从已批准订单生成生产、物料检查、开始/完成生产、BOM |
| `MM-Inventory.html` | 物料管理 (MM) | 物料负责人 | 库存表、低库存预警、新建采购(PO)、收货入库(Goods Receipt) |
| `FI-Finance.html` | 财务会计 (FI) | 财务负责人 | 应收/应付/净现金流卡片、发票审批(Approve/Reject) |
| `HCM-HR.html` | 人力资本 (HCM) | 人事负责人 | 员工花名册、新增员工、删除员工、统计卡片 |

## 技术说明

- 每个文件是**独立可运行**的单页面应用（HTML + CSS + 原生 JavaScript），只包含本模块的业务代码。
- **数据库已单独提取**：所有模块统一通过 `<script src="../database.js"></script>` 引用上级目录的数据库层（`getDB` / `saveDB` / 种子数据），数据统一存放在 localStorage 键 `sheepsheep_erp_db`。六个模块共享同一份数据库——SD 新建/审批订单、PP 生成生产、MM 采购收货、FI 发票审批、HCM 员工管理等操作相互联动，数据保持一致。
- 请保持文件相对位置：`standalone/` 目录与根目录 `database.js` 同级（不要单独拷走某个 HTML 而不带 `database.js`）。
- 想重置演示数据：浏览器清除该站点的 localStorage（键 `sheepsheep_erp_db`）即可，下次打开自动重新生成种子数据。
- 原版中 HCM 的 "+ Add Employee" 只是弹窗提示，独立版已实现真正的员工新增/删除；原版 PP 的 "Generate Production from Orders" 只是提示，独立版已实现从已批准订单实际生成生产单。
