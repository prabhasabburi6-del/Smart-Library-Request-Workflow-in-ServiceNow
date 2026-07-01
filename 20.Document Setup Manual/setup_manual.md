# Smart Library Request Workflow in ServiceNow
## Section 20: Setup & Re-creation Manual

## 1. Objective
The objective of this setup manual is to provide a complete step-by-step guide for recreating the Smart Library Request Workflow application in a new ServiceNow Personal Developer Instance (PDI). This document enables developers and administrators to install, configure, and validate the solution from scratch.

## 2. Introduction
The Smart Library Request Workflow is a role-based ServiceNow application that automates library operations such as book management, borrow requests, approvals, security, and reporting. This manual outlines the installation sequence, required configurations, and validation steps to ensure a successful deployment.

---

## 3. Prerequisites
Before beginning the setup, ensure the following are active:
* Active ServiceNow Personal Developer Instance (PDI) (any recent release).
* Administrator (`admin`) privileges.
* ServiceNow Flow Designer and Reporting engine enabled.
* System SMTP Email configurations enabled.

---

## 4. Setup Overview

| Step | Setup Milestone | Execution Area | Status |
| :--- | :--- | :--- | :---: |
| **1** | Create Roles | User Administration ──> Roles | ✅ Completed |
| **2** | Create Tables | System Definition ──> Tables | ✅ Completed |
| **3** | Configure Fields | Columns (Dictionary) Related Lists | ✅ Completed |
| **4** | Configure Reference Qualifier | Dictionary Entry Advanced Settings | ✅ Completed |
| **5** | Configure Flow Designer | Flow Designer Studio Canvas | ✅ Completed |
| **6** | Configure UI Policy | System UI ──> UI Policies | ✅ Completed |
| **7** | Configure ACLs | System Security ──> Access Control | ✅ Completed |
| **8** | Create Report | Reports ──> View / Run | ✅ Completed |
| **9** | Insert Test Data | u_book & u_borrow_request Tables | ✅ Completed |
| **10**| Validate Workflow | Impersonation Test Scripts | ✅ Completed |

---

## 5. Step-by-Step Setup Guide

### Step 1 – Create Roles
1. Navigate to **All** ──> **User Administration** ──> **Roles**.
2. Click **New** and configure roles:
   * Role 1: `x_library.student` (Description: *Borrow books*)
   * Role 2: `x_library.librarian` (Description: *Manage library operations*)
3. Click **Submit** for each role.

#### Figure 1: Custom roles registered in the ServiceNow user directory
<img width="1376" height="768" alt="10" src="https://github.com/user-attachments/assets/bdc69632-11a6-4442-abb2-c794e477160c" />


---

### Step 2 – Create Custom Tables
1. Navigate to **All** ──> **System Definition** ──> **Tables**.
2. Click **New** and configure:
   * Book Table: Label = `Book`, Name = `u_book`.
   * Borrow Request Table: Label = `Borrow Request`, Name = `u_borrow_request`.
3. Click **Submit**.

#### Figure 2: Custom tables created and verified in the ServiceNow instance
<img width="1376" height="768" alt="11" src="https://github.com/user-attachments/assets/0d78e548-9423-4a77-b820-cfea48509dd9" />


---

### Step 3 – Configure Table Fields
Open the respective table configurations and add columns via the **Columns** related list:

#### Book Table (`u_book`) Columns:
* `Title` (String)
* `Author` (String)
* `ISBN` (String)
* `Status` (Choice): Set default as `Available`. Add Choices: `Available`, `Issued`, `Lost`.

#### Borrow Request Table (`u_borrow_request`) Columns:
* `Requested By` (Reference ──> `sys_user`)
* `Book` (Reference ──> `u_book`)
* `Borrow Date` (Date)
* `Return Date` (Date)
* `Status` (Choice): Set default as `Requested`. Add Choices: `Requested`, `Approved`, `Rejected`, `Issued`, `Returned`.

#### Figure 3: Book form fields successfully added and laid out
<img width="1376" height="768" alt="12" src="https://github.com/user-attachments/assets/065ed5f0-45e8-4e0e-b1a7-1bd4b1337894" />

---

### Step 4 – Configure Reference Qualifier
1. Open the Dictionary Entry for the **Book** field on the Borrow Request table.
2. Toggle the **Advanced View** link.
3. On the **Reference Specification** tab, configure:
   * **Use reference qualifier**: `Simple`
   * **Reference qual condition**: `Status is Available`.
4. Click **Update**.

#### UI Mockup 4: Dictionary Entry filter condition builder
```
================================================================================
|  Reference Specification (Advanced View)                                     |
================================================================================
|  * Use reference qualifier: [ Simple                                     |▼] |
|  * Reference qual condition:                                                 |
|    ------------------------------------------------------------------------  |
|    [ Status           ] [ is            ] [ Available                   |▼]  |
|    ------------------------------------------------------------------------  |
================================================================================
```
*Figure 4: Reference Qualifier configuration.*

---

### Step 5 – Configure Flow Designer
1. Navigate to **All** ──> **Flow Designer**.
2. Create a new flow: `Borrow Request Approval Flow`.
3. Configure the trigger and actions:
   * **Trigger**: Created or Updated `u_borrow_request` where `Status = Requested`.
   * **Action 1**: Ask for Approval (Assign to Librarians group).
   * **If Approved**:
     * Action 2: Update Book Record (Set `Status = Issued`).
     * Action 3: Update Borrow Request Record (Set `Status = Approved`).
     * Action 4: Send Email Notification.
   * **Else (If Rejected)**:
     * Action 5: Update Borrow Request Record (Set `Status = Rejected`).
4. Click **Save** and **Activate**.

#### UI Mockup 5: Active Borrow Request Approval Flow canvas layout
```
================================================================================
|  Borrow Request Approval Flow  |   [ Save ] [ Deactivate ]  |  ● Active      |
================================================================================
|  TRIGGER: Created or Updated (Borrow Request u_borrow_request)               |
|  ▼ 1 - Ask For Approval (Librarian Approves)                                 |
|  ▼ 2 - IF APPROVED (Approval State == Approved)                              |
|     ├── 3 - Update Book Record (Status = Issued)                             |
|     ├── 4 - Update Borrow Request Record (Status = Approved)                 |
|     └── 5 - Send Email Notification (To: Student, Subj: Approved)            |
================================================================================
```
*Figure 5: Borrow Request Approval Flow.*

---

### Step 6 – Configure UI Policy
1. Navigate to **System UI** ──> **UI Policies**.
2. Click **New** and configure:
   * **Table**: `Borrow Request [u_borrow_request]`
   * **Short Description**: `Make Return Date mandatory when Issued`
   * **Conditions**: `Status is Issued`
3. Save the UI Policy.
4. Scroll to the **UI Policy Actions** related list, click **New** and add action:
   * **Field name**: `Return Date`
   * **Mandatory**: `True`
   * **Visible**: `True`
5. Click **Submit**.

#### UI Mockup 6: Return Date marked mandatory and visible
```
================================================================================
|  Borrow Request  |  BR0001004                                 [ Update ] [ < ] |
================================================================================
|  Status:        [ Issued                                                 |▼] |
| *Return Date:   [ 2026-07-14 17:00:00                                  |🗓️] |
================================================================================
```
*Figure 6: UI Policy configuration.*

---

### Step 7 – Configure Access Control (ACLs)
1. Navigate to **System Security** ──> **Access Control (ACL)**.
2. Click **New** and construct the database access rules:

| Table | Operation | Role Required |
| :--- | :--- | :--- |
| `u_book` | `read` | `student`, `librarian` |
| `u_book` | `create` / `write` / `delete` | `librarian` |
| `u_borrow_request` | `create` | `student` |
| `u_borrow_request` | `read` | `student`, `librarian` |
| `u_borrow_request` | `write` / `delete` | `librarian` |

#### Figure 7: Create ACL configured for student borrow requests
<img width="1376" height="768" alt="13" src="https://github.com/user-attachments/assets/5a6fea53-cf43-4ef7-8d9c-dbd9458be2fb" />


---

### Step 8 – Create Report
1. Navigate to **Reports** ──> **View / Run**.
2. Click **New** and configure:
   * **Report Name**: `Most Borrowed Books`
   * **Source Type**: `Table`
   * **Table**: `Borrow Request [u_borrow_request]`
   * **Type**: `Bar`
   * **Group By**: `Book`
   * **Aggregate**: `Count`
   * **Filter**: `Status is Approved`
3. Click **Save**.

#### Figure 8: Most Borrowed Books bar chart visualization report
<img width="1376" height="768" alt="14" src="https://github.com/user-attachments/assets/c068a699-0cf7-4ee7-9ac0-f17655f245f4" />

---

### Step 9 – Insert Test Data & Validate

#### 1. Books Inventory Setup
Create three sample book records in `u_book` table:
* *Java Programming* (Status: `Available`)
* *Python Basics* (Status: `Available`)
* *Database Systems* (Status: `Available`)

#### 2. Create Borrow Request
Impersonate a student user, open the `u_borrow_request` list, and click **New**.
1. Verify that only the 3 books are visible in the lookup.
2. Select *Java Programming* and submit.

#### 3. Approve Borrow Request
Impersonate a librarian user, locate the pending request in the task list.
1. Click **Approve**.
2. Verify the book status has transitioned to `Issued` in the `u_book` table.
3. Verify the borrow request status has transitioned to `Approved`.

#### UI Mockup 9: Active check-out database log verification
```
================================================================================
|  Borrow Requests  [ New ]                                                   |
================================================================================
|  Number    | Requested By | Book             | Status     | Return Date      |
--------------------------------------------------------------------------------
|  BR0001004 | John Doe     | Java Programming | Approved   | 2026-07-14       |
================================================================================
```
*Figure 9: Test data validation.*

---

### Step 10 – Validate Complete Workflow
Impersonate a Student user again.
1. Try to open the approved request and update the status or title.
2. Verify that the form is completely locked (read-only) and the "Save" and "Delete" actions are hidden.

#### Figure 10: Impersonation test verifying read-only access for student accounts
<img width="1376" height="768" alt="15" src="https://github.com/user-attachments/assets/486425ce-ab5f-4242-b9d5-ec3caf50f4d0" />


---

## 6. Setup Workflow
```
[Start] ──> [Create Roles] ──> [Create Tables] ──> [Configure Fields] ──> [Reference Qualifier] ──> [Flow Designer] ──> [UI Policy] ──> [ACL Configuration] ──> [Create Report] ──> [Insert Test Data] ──> [Validate Workflow] ──> [Setup Completed]
```

---

## 7. Expected Outcome
After completing this setup:
* The Smart Library Request Workflow application is successfully recreated.
* Role-based access control is enforced.
* Borrow requests are automated.
* Reports are generated correctly.
* The end-to-end workflow is validated.
* The application is ready for demonstration and deployment.

## 8. Benefits
* **Easy Deployment**: Steps enable recreation in a new PDI from scratch.
* **Standardized Configuration**: Outlines exact field names, types, and logic.
* **Lower Setup Time**: Prevents database design circular dependencies.
* ** upgrade-safe structure**: Adheres strictly to OOTB low-code parameters.

## 9. Conclusion
This setup manual provides a structured procedure for recreating the Smart Library Request Workflow application in a new ServiceNow Personal Developer Instance. Following the documented sequence—creating roles, tables, fields, workflows, UI policies, ACLs, reports, and validating with test data—ensures a consistent, secure, and fully functional deployment. The manual also serves as a valuable reference for future maintenance, upgrades, and knowledge transfer.
