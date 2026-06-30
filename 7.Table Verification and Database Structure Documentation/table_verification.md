# Smart Library Request Workflow in ServiceNow
## Section 7: Table Verification and Database Structure Documentation

## 1. Objective
The objective of this task is to verify the successful creation of the custom tables required for the Smart Library Request Workflow application and ensure that the database structure is ready for further configuration. This includes validating the Book and Borrow Request tables, confirming their naming conventions, and preparing them for field creation and relationship mapping.

## 2. Introduction
After creating custom tables in ServiceNow, it is important to verify that they have been created correctly before proceeding with additional configurations such as fields, relationships, workflows, and security settings.

The Book table acts as the master repository for all library books, while the Borrow Request table stores every borrowing transaction. Proper verification ensures that both tables are available in the system and follow ServiceNow's naming conventions.

---

## 3. Prerequisites
Before verification, ensure that:
* ServiceNow Personal Developer Instance (PDI) is active.
* Administrator (`admin`) privileges are available.
* The Book and Borrow Request tables have already been created (via Task 6).

---

## 4. Verification Steps

### Step 1: Open the Tables Module
1. Log in to your ServiceNow instance.
2. Click **All** in the Application Navigator.
3. Search for **Tables**.
4. Select **System Definition** ──> **Tables**.

#### UI Mockup 1: Open Tables Module Navigation
```
================================================================================
|  ServiceNow  |  Filter Navigator: [ tables         ]  | User Profile (Admin) |
================================================================================
|  All | Favorites | History | Developer                                       |
--------------------------------------------------------------------------------
|  ▼ System Definition                                                         |
|    - Dictionary                                                              |
|    * Tables   <=== (Select this to open the list of all tables)              |
================================================================================
```
*Figure 1: Opening the System Definition → Tables module.*

---

### Step 2: Verify the Book Table
Search the Tables list to confirm the Book table exists and is active.
* **Label**: `Book`
* **Table Name**: `u_book`
* **Active Status**: `true`

---

### Step 3: Verify the Borrow Request Table
Search the Tables list to confirm the Borrow Request table exists and is active.
* **Label**: `Borrow Request`
* **Table Name**: `u_borrow_request`
* **Active Status**: `true`

---

### Step 4: Confirm Database Structure
Search for both tables in a single view to confirm they coexist in the active scope.

#### UI Mockup 2: Tables verification list
```
================================================================================
|  Tables  [ New ]      | Search: [ Label ] [ Book* OR Borrow*               ] |
================================================================================
|  Label (▼)                 | Name                     | Extends Table        |
--------------------------------------------------------------------------------
|  Book                      | u_book                   | None                 |
|  Borrow Request            | u_borrow_request         | None                 |
================================================================================
```
*Figure 2: Book and Borrow Request tables successfully verified.*

---

## 5. Database Structure

| Table Label | Table Name | Scope Prefix | Description |
| :--- | :--- | :--- | :--- |
| **Book** | `u_book` | Custom/Scoped | Stores all library book records (title, author, copies). |
| **Borrow Request** | `u_borrow_request` | Custom/Scoped | Stores book borrowing transactions, request details, and states. |

---

## 6. Relationship Overview
The Borrow Request table references the Book table to map request transactions to inventory.
```
Book (u_book) ──────── 1 (One Book)
                      │
                      ▼
             Many Borrow Requests (u_borrow_request)
```
The Borrow Request table references the Book table using a **Reference** field, ensuring database integrity and preventing orphaned requests.

---

## 7. Verification Checklist

| Verification Item | Targeted Property | Status |
| :--- | :--- | :---: |
| **Book table created** | Table exists in system dictionary | ✔ Completed |
| **Borrow Request table created** | Table exists in system dictionary | ✔ Completed |
| **Naming convention verified** | Naming has proper prefixes (`u_` or scoped prefix) | ✔ Completed |
| **Tables visible in System Definition** | Reachable via Application Navigator | ✔ Completed |
| **Ready for field configuration** | Database structure is unlocked for columns | ✔ Completed |

---

## 8. Expected Outcome
After completing this task:
* Both custom tables are successfully verified.
* Database structure is confirmed.
* Tables are ready for field configuration.
* Foundation for workflow automation is established.

## 9. Benefits
* **Ensures Correct Table Creation**: Identifies naming or extension errors before schema lock-in.
* **Prevents Configuration Errors**: Prevents dependency issues during Flow or UI Policy definitions.
* **Supports Future Schema Upgrades**: Validates table baseline before adding fields and references.
* **Improves Application Reliability**: Confirms active status and namespace separation in ServiceNow.

## 10. Conclusion
The verification of the Book and Borrow Request tables confirms that the Smart Library Request Workflow database has been successfully established. With both tables available and correctly configured, the application is ready for the next phase, where fields, relationships, and business logic will be implemented.
