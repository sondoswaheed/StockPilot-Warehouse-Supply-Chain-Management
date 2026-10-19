# StockPilot API 

An enterprise-grade Warehouse Management System (WMS) Backend API engineered to streamline inventory tracking, procurement workflows, automated sales order fulfillments, and strict audit traceability.

---

## 🏗️ Architecture & Tech Stack

This project strictly adheres to **Clean Architecture** principles and implements the **CQRS** (Command Query Responsibility Segregation) pattern using MediatR.

* **Platform:** .NET Core / C#
* **Design Pattern:** Clean Architecture + CQRS + Result Pattern
* **Database & ORM:** Entity Framework Core (SQL Server / PostgreSQL)
* **Authentication & Authorization:** JWT Bearer Tokens with Role-Based Access Control (RBAC)
* **Validation & Logging:** FluentValidation & Serilog
* **Testing:** xUnit, Moq, FluentAssertions

---

## 👥 Core Features & Modules

1. **Identity & Platform Security:**
   * Secure user registration and JWT-based authentication.
   * Role-Based Authorization across 5 distinct roles: `Admin`, `WarehouseManager`, `Procurement`, `Sales`, and `Auditor`.

2. **Inventory & Batch Management (FEFO):**
   * Automated **FEFO** (First-Expired, First-Out) stock allocation algorithm prioritizing nearest batch expiry dates while ignoring expired stock.
   * Immutable, append-only **Inventory Movement Ledger** tracking every receipt and issue.

3. **Concurrency Safety & Transactions:**
   * Optimistic Concurrency Control using `RowVersion` tokens combined with automatic retry logic to prevent overselling during high-traffic sales surges.
   * All-or-Nothing transaction boundaries for order confirmations.

4. **Procurement & Sales Workflows:**
   * Full lifecycle management for Purchase Orders (Draft → Submit → Approve/Reject → Receive).
   * Sales Order processing with automated inventory reservation, cancellation hooks, and shipment processing.

5. **Governance & Audit Trail:**
   * Automated `DbContext` interceptor generating JSON snapshots of pre/post entity modifications for complete compliance tracking.
   * Soft-deletion safeguards preventing the removal of active inventory items.

---

## 🛡️ Security Access Matrix

| Role | User Management | Warehouses & Products | Purchase Orders | Sales Orders | Stock & Movements | Reports & Audit Logs |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Admin** | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| **WarehouseManager**| – | ✔ | Approve/Receive | Ship | ✔ | ✔ (Reports) |
| **Procurement** | – | – | Create/Submit | – | ✔ | – |
| **Sales** | – | – | – | Create/Confirm/Ship| ✔ | – |
| **Auditor** | – | – | – | – | ✔ | Read-only |

---

