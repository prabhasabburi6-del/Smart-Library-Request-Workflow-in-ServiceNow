# Smart Library Request Workflow in ServiceNow
## Section 6: Create Tables Documentation

## 1. Objective
The objective of this task is to create the core database tables required for the Smart Library Request Workflow application in ServiceNow. These tables will store book information and borrow request details, forming the foundation of the library management system.

## 2. Introduction
In ServiceNow, a Table is a database object used to store records related to a specific business process. Every application is built around tables that organize and manage data efficiently.

For the Smart Library Request Workflow, two custom tables are created:
1. **Book** (`u_book`) – Stores all information about books available in the library.
2. **Borrow Request** (`u_borrow_request`) – Stores borrowing requests submitted by students.

These tables establish the data structure required for managing books, tracking requests, and implementing workflow automation.

---

## 3. Prerequisites
Before creating the tables, ensure that:
* ServiceNow Personal Developer Instance (PDI) is active.
* Administrator (`admin`) access is available.
* You are logged into the ServiceNow instance.

---

## 4. Implementation Steps

### Step 1: Open the Tables Module
1. Log in to your ServiceNow instance.
2. Click on **All** in the Application Navigator.
3. Type **Tables** in the search box.
4. Select **System Definition** ──> **Tables**. This opens the list of existing tables in ServiceNow.

#### UI Mockup 1: Navigating to System Definition ──> Tables
```
================================================================================
|  ServiceNow  |  Filter Navigator: [ tables         ]  | User Profile (Admin) |
================================================================================
|  All | Favorites | History | Developer                                       |
--------------------------------------------------------------------------------
|  ▼ System Definition                                                         |
|    - Dictionary                                                              |
|    * Tables   <=== (Select this to open the list of all tables)              |
|    - Table Columns                                                           |
|    - Number Maintenance                                                      |
================================================================================
```
*Figure 1: Navigating to the Tables module in ServiceNow.*

---

### Step 2: Create the Book Table
1. Click the **New** button at the top of the Tables list.
2. In the **Label** field, enter: `Book`.
3. ServiceNow will automatically generate the database name in the **Name** field: `u_book` (or scoped equivalent like `x_snc_library_book`).
4. Leave other settings (such as Auto-number, Extends Table, etc.) as default.
5. Click **Submit**.

This creates the Book table, which will store book inventory information.

---

### Step 3: Create the Borrow Request Table
1. Click the **New** button again.
2. In the **Label** field, enter: `Borrow Request`.
3. ServiceNow will automatically generate the database name: `u_borrow_request` (or scoped equivalent).
4. Click **Submit**.

This table will store borrowing requests submitted by students.

---

### Step 4: Verify the Tables
After submitting both tables:
1. Search for `Book` in the table list.
2. Search for `Borrow Request` in the table list.
3. Verify that both tables appear in the System Definition -> Tables list.

#### UI Mockup 2: Tables Module Search Verification
```
================================================================================
|  Tables  [ New ]      | Search: [ Label ] [ Book* OR Borrow*               ] |
================================================================================
|  Label (▼)                 | Name                     | Extends Table        |
--------------------------------------------------------------------------------
|  Book                      | u_book                   | None (Base Table)    |
|  Borrow Request            | u_borrow_request         | None (Base Table)    |
================================================================================
```
*Figure 2: Successfully created Book and Borrow Request tables.*

---

## 5. Tables Created

| Table Label | Table Name | Scope Prefix | Primary Purpose |
| :--- | :--- | :--- | :--- |
| **Book** | `u_book` | Custom/Scoped | Stores library book information (master catalog). |
| **Borrow Request** | `u_borrow_request` | Custom/Scoped | Stores student borrow requests and workflow states. |

---

## 6. Relationship Between Tables
The Borrow Request table references the Book table via a **Reference** field.
```
[Book Table (u_book)] 1 ─────── 0..* [Borrow Request Table (u_borrow_request)]
```
This relationship is defined as a one-to-many relationship. It allows multiple borrowing transactions to be associated with a single book record over time, preserving historical loan logs.

---

## 7. Benefits of Creating Tables
* **Provides a Structured Database**: Organizes key properties (ISBN, author, dates) into strongly-typed columns.
* **Stores Library Information Centrally**: Librarians and students query the same source database.
* **Supports Workflow Automation**: ServiceNow Flow Designer binds to these tables to trigger automated flows.
* **Enables Reporting and Analytics**: Dashboards draw statistics directly from database logs.
* **Improves Data Consistency**: Avoids data duplication by referencing master book entries.

---

## 8. Expected Outcome
After completing this task:
1. The Book (`u_book`) table is created successfully.
2. The Borrow Request (`u_borrow_request`) table is created successfully.
3. Both tables are visible in the Tables module.
4. The database structure is ready for adding fields and configuring relationships.

## 9. Advantages
* Centralized data storage.
* Better record management.
* Easy integration with Flow Designer.
* Supports Access Control List (ACL) implementation.
* Enables scalable application development.
* Improves data organization.

## 10. Conclusion
Creating the Book and Borrow Request tables is a fundamental step in developing the Smart Library Request Workflow application. These tables provide the primary data repository for library operations and enable future configurations such as fields, reference relationships, workflows, UI policies, and reports. With these tables established, the system is now ready for detailed schema configuration.
