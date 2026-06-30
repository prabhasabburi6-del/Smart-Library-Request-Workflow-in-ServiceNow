# Smart Library Request Workflow in ServiceNow
## Section 5: Creation of Roles Documentation

## 1. Objective
The objective of this task is to create user roles in the ServiceNow platform for the Smart Library Request Workflow application. Role-based access control (RBAC) ensures that users have only the permissions required to perform their responsibilities. In this project, two primary roles are created: Student and Librarian.

## 2. Introduction
Role-Based Access Control (RBAC) is a security mechanism in ServiceNow that restricts access to system resources based on user roles. Each role contains a set of permissions that determine what actions a user can perform.

For the Smart Library Request Workflow, two custom roles are required:
1. **Student** (`x_library.student`) – Can search books, submit borrow requests, and track request status.
2. **Librarian** (`x_library.librarian`) – Can manage books, approve or reject requests, update book status, and generate reports.

Creating these roles ensures secure access to application features while preventing unauthorized modifications.

---

## 3. Prerequisites
Before creating roles, ensure that:
* ServiceNow Personal Developer Instance (PDI) is active.
* Administrator (`admin`) access is available.
* You are logged into the ServiceNow instance.

---

## 4. Implementation Steps

### Step 1: Open the Roles Module
1. Log in to the ServiceNow instance.
2. Click on **All** in the Application Navigator.
3. Type **Roles** in the search box.
4. Select **User Administration** ──> **Roles**.

#### UI Mockup 1: Navigating to User Administration ──> Roles
```
================================================================================
|  ServiceNow  |  Filter Navigator: [ roles          ]  | User Profile (Admin) |
================================================================================
|  All | Favorites | History | Developer                                       |
--------------------------------------------------------------------------------
|  ▼ User Administration                                                       |
|    - Users                                                                   |
|    - Groups                                                                  |
|    * Roles    <=== (Select this to open the roles list)                      |
|    - User Preferences                                                        |
================================================================================
```
*Figure 1: Navigating to User Administration → Roles from the Application Navigator.*

---

### Step 2: Create the Librarian Role
1. Click the **New** button on the Roles list header.
2. In the **Name** field, enter: `x_library.librarian` (or `librarian` depending on scope prefix).
3. In the **Description** field, enter:
   `Role for managing library books and approving borrow requests.`
4. Click **Submit**.

#### UI Mockup 2: Role Creation Form (Librarian)
```
================================================================================
|  Role  |  New Record                                         [ Submit ] [ < ] |
================================================================================
|  * Name:        [ x_library.librarian                                    ]   |
|  Description:   [ Role for managing library books and approvals.         ]   |
================================================================================
```

---

### Step 3: Create the Student Role
1. Click the **New** button again.
2. In the **Name** field, enter: `x_library.student` (or `student`).
3. In the **Description** field, enter:
   `Role for requesting books and tracking borrow requests.`
4. Click **Submit**.

---

### Step 4: Verify the Roles
After submitting both records:
1. Search for `student` in the name search box.
2. Search for `librarian` in the name search box.
3. Verify that both roles appear in the Roles list.

#### UI Mockup 3: Roles List Verification
```
================================================================================
|  Roles  [ New ]       | Search: [ Name ] [ *student OR *librarian          ] |
================================================================================
|  Name (▼)                 | Description                                      |
--------------------------------------------------------------------------------
|  x_library.librarian      | Role for managing library books and approvals.   |
|  x_library.student        | Role for requesting books and tracking requests.  |
================================================================================
```
*Figure 2: The student and librarian roles successfully created and visible in the Roles list.*

---

## 5. Roles Created

| Role Name | Scope Prefix | Description / Purpose |
| :--- | :--- | :--- |
| **student** | `x_library.student` | Request books, track requests, and view personal borrowing history. |
| **librarian** | `x_library.librarian` | Manage books, approve/reject requests, update book status, and generate reports. |

---

## 6. Role Responsibilities

### Student Role (`x_library.student`)
* **Can**:
  * View available books.
  * Submit borrow requests.
  * View request status.
  * View personal borrow history.
* **Cannot**:
  * Add or edit books.
  * Approve requests.
  * Delete records.
  * Configure system tables or workflows.

### Librarian Role (`x_library.librarian`)
* **Can**:
  * Add new books.
  * Edit book information.
  * Delete books.
  * Approve or reject borrow requests.
  * Update book status.
  * Generate reports.
  * Maintain library inventory.
* **Cannot**:
  * Modify table schemas.
  * Edit ServiceNow Flows or core scripts.

---

## 7. Expected Outcome
After completing this task:
1. The `student` role is successfully created.
2. The `librarian` role is successfully created.
3. Both roles are available for assignment to users.
4. The foundation for role-based security is established.

## 8. Benefits
* **Implements Secure Access Control**: Establishes Role-Based Access Control (RBAC).
* **Prevents Unauthorized Access**: Shields sensitive library admin configurations from student accounts.
* **Separates Responsibilities**: Restricts critical workflow steps (approving requests) to verified librarians.
* **Improves Security Audits**: Simplifies permission tracing across ServiceNow audits.

## 9. Conclusion
The successful creation of the student and librarian roles establishes the security foundation for the Smart Library Request Workflow application. These roles ensure that students can perform only borrowing-related activities, while librarians have the necessary privileges to manage library operations. This role separation enhances security, improves operational efficiency, and supports effective access control throughout the application.
