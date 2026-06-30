# Smart Library Request Workflow in ServiceNow
## Section 11: Create Flow in Flow Designer Documentation

## 1. Objective
The objective of this task is to automate the library borrowing process using Flow Designer in ServiceNow. The flow automatically sends a borrow request for librarian approval, updates the book status when approved, updates the borrow request status, and notifies the student via email. This automation reduces manual work, improves efficiency, and ensures accurate tracking of library resources.

## 2. Introduction
Flow Designer is a low-code automation tool in ServiceNow that enables users to create workflows without writing scripts. In the Smart Library Request Workflow application, Flow Designer automates the complete borrow request approval process.

When a student submits a borrow request with the status Requested, the flow sends an approval request to a librarian. Once approved, the book status changes to Issued, the borrow request status changes to Approved, and an email notification is sent to the student.

This automation ensures a consistent approval process while minimizing manual intervention.

---

## 3. Prerequisites
Before creating the flow, ensure that:
* ServiceNow Personal Developer Instance (PDI) is active.
* Administrator (`admin`) access is available.
* The Book (`u_book`) and Borrow Request (`u_borrow_request`) tables are created.
* Student and Librarian roles are available.
* The Borrow Request table contains the fields: Book, Requested By, and Status.

---

## 4. Flow Overview

| Flow Element / Step | ServiceNow Action | Configuration details |
| :--- | :--- | :--- |
| **Flow Name** | `Borrow Request Approval Flow` | Primary workflow definition |
| **Trigger** | `Created or Updated` | Runs on `u_borrow_request` when `u_status` is `Requested` |
| **Action 1** | `Ask For Approval` | Route request to user group/user with `librarian` role |
| **Action 2 (If Approved)**| `Update Record` | Update referencing book field `u_status` in `u_book` to `Issued` |
| **Action 3 (If Approved)**| `Update Record` | Update current request field `u_status` in `u_borrow_request` to `Approved` |
| **Action 4 (If Approved)**| `Send Email` | Notify student user (`u_requested_by.email`) of checkout success |
| **Action 5 (If Rejected)**| `Update Record` | Update current request field `u_status` in `u_borrow_request` to `Rejected` |

---

## 5. Implementation Steps

### Step 1 – Open Flow Designer
1. Log in to your ServiceNow instance.
2. Click **All** in the Application Navigator.
3. Search for **Flow Designer** and open it.
4. Click **New** ──> **Flow** in the upper right.
5. In the Flow properties pop-up, enter:
   * **Flow Name**: `Borrow Request Approval Flow`
   * **Application**: `Global` (or scoped app)
6. Click **Submit**.

#### UI Mockup 1: Flow Creation Dialog
```
================================================================================
|  Flow Properties                                                             |
================================================================================
|  * Flow name:   [ Borrow Request Approval Flow                             ] |
|  Description:   [ Automates student book approvals and status adjustments. ] |
|  * Application: [ Global                                                 |▼] |
|  Run As:        [ System User                                            |▼] |
--------------------------------------------------------------------------------
|                                                    [ Cancel ] [ Submit ]     |
================================================================================
```
*Figure 1: Creating the Borrow Request Approval Flow.*

---

### Step 2 – Add the Trigger
1. Click **+ Add Trigger**.
2. Configure as follows:
   * **Trigger**: `Created or Updated`
   * **Table**: `Borrow Request [u_borrow_request]`
   * **Condition**: `Status [u_status] is Requested`
3. Click **Done**.

#### UI Mockup 2: Trigger Condition Panel
```
================================================================================
|  TRIGGER: Created or Updated                                                 |
================================================================================
|  * Table:     [ Borrow Request [u_borrow_request]                        |▼] |
|  Conditions:                                                                 |
|     [ Status           ] [ is            ] [ Requested                  |▼]  |
================================================================================
```
*Figure 2: Configuring the flow trigger.*

---

### Step 3 – Add Approval Action
1. Under **Actions**, click **+ Add an Action, Flow Logic, or Subflow**.
2. Click **Action** and search for **Ask for Approval** (ServiceNow core).
3. Configure the parameters:
   * **Record**: Drag and drop the `Borrow Request Record` data pill from the Trigger data panel.
   * **Table**: `Borrow Request [u_borrow_request]`
   * **Rules**: Choose `Anyone approves` and assign to users with the **Librarian** role.
4. Click **Done**.

#### UI Mockup 3: Ask for Approval Action Block
```
================================================================================
|  ACTION 1: Ask For Approval                                                  |
================================================================================
|  * Record:        [ Trigger - Record -> Borrow Request Record             ]  |
|  * Table:         [ Borrow Request [u_borrow_request]                     ]  |
|  Rules:                                                                      |
|    - [ Approve ] when [ Anyone approves                                  |▼] |
|      User: [ Librarians Group / users with x_library.librarian role       ]  |
================================================================================
```
*Figure 3: Configuring the approval action for the Librarian.*

---

### Step 4 – Branching Flow Logic & Book Update
1. Add a **Flow Logic** step: Select **If (Decision)**.
2. Label it `If Approved`. Condition: `Ask For Approval -> Approval State is Approved`.
3. Inside the `If Approved` block, click **+** and choose **Action** ──> **Update Record**.
4. Configure as follows:
   * **Record**: Drag the `Book` reference data pill from the trigger: `Trigger -> Borrow Request Record -> Book`.
   * **Table**: `Book [u_book]`
   * **Fields**: `Status = Issued`
5. Click **Done**.

#### UI Mockup 4: Update Book Record Status
```
================================================================================
|  ACTION 2: Update Record (Inside "If Approved" branch)                       |
================================================================================
|  * Record:        [ Trigger -> Borrow Request Record -> Book              ]  |
|  * Table:         [ Book [u_book]                                         ]  |
|  Fields:                                                                     |
|    [ Status           ] = [ Issued                                      |▼]  |
================================================================================
```
*Figure 4: Updating the Book status to Issued after approval.*

---

### Step 5 – Update Borrow Request
1. Click **+** inside the `If Approved` block.
2. Select **Action** ──> **Update Record**.
3. Configure:
   * **Record**: Drag the `Borrow Request Record` data pill from the trigger.
   * **Table**: `Borrow Request [u_borrow_request]`
   * **Fields**: `Status = Approved`
4. Click **Done**.

#### UI Mockup 5: Update Borrow Request Status
```
================================================================================
|  ACTION 3: Update Record (Inside "If Approved" branch)                       |
================================================================================
|  * Record:        [ Trigger -> Borrow Request Record                      ]  |
|  * Table:         [ Borrow Request [u_borrow_request]                     ]  |
|  Fields:                                                                     |
|    [ Status           ] = [ Approved                                    |▼]  |
================================================================================
```
*Figure 5: Updating the Borrow Request status to Approved.*

---

### Step 6 – Send Email Notification
1. Click **+** inside the `If Approved` block.
2. Select **Action** ──> **Send Email**.
3. Configure properties:
   * **To**: Drag data pill `Trigger -> Borrow Request Record -> Requested By -> Email`.
   * **Subject**: `Your Borrow Request has been Approved`
   * **Body**:
     `Hello, the book "[Trigger.Book.Title]" has been issued to you. Please collect it from the library.`
4. Click **Done**.

#### UI Mockup 6: Send Email Action Panel
```
================================================================================
|  ACTION 4: Send Email (Inside "If Approved" branch)                          |
================================================================================
|  To:      [ Trigger -> Borrow Request Record -> Requested By -> Email     ]  |
|  Subject: [ Your Borrow Request has been Approved                         ]  |
|  Body:                                                                       |
|  [ The book [Trigger.Book.Title] has been issued to you. Please collect    ] |
|  [ it from the library.                                                    ] |
================================================================================
```
*Figure 6: Configuring the email notification.*

---

### Step 7 – Add Reject Branch
1. Add a **Flow Logic** step: Select **Else**.
2. Inside the `Else` block, click **+** and choose **Action** ──> **Update Record**.
3. Configure:
   * **Record**: Drag the `Borrow Request Record` data pill from the trigger.
   * **Table**: `Borrow Request [u_borrow_request]`
   * **Fields**: `Status = Rejected`
4. Click **Done**.

### Step 8 – Save and Activate
1. Click **Save** in the top right corner.
2. Click **Activate** to enable the automated execution.

#### UI Mockup 7: Completed Flow Summary Canvas
```
================================================================================
|  Borrow Request Approval Flow  |   [ Save ] [ Deactivate ]  |  ● Active      |
================================================================================
|  TRIGGER: Created or Updated (Borrow Request u_borrow_request)               |
|                                                                              |
|  ▼ 1 - Ask For Approval (Librarian Approves)                                 |
|                                                                              |
|  ▼ 2 - IF APPROVED (Approval State == Approved)                              |
|     ├── 3 - Update Book Record (Status = Issued)                             |
|     ├── 4 - Update Borrow Request Record (Status = Approved)                 |
|     └── 5 - Send Email Notification (To: Student, Subj: Approved)            |
|                                                                              |
|  ▼ 6 - ELSE (Approval State == Rejected)                                     |
|     └── 7 - Update Borrow Request Record (Status = Rejected)                 |
================================================================================
```
*Figure 7: Borrow Request Approval Flow activated successfully.*

---

## 6. Flow Execution Sequence Diagram
The execution logic transitions through these tasks when triggered:
```mermaid
sequenceDiagram
    autonumber
    actor Student as Student Portal
    participant SN as Flow Designer
    actor Lib as Librarian Console
    database DB as ServiceNow tables
    
    Student->>DB: Submit Borrow Request (Status: Requested)
    Note over DB: Trigger Condition met
    DB->>SN: Start Borrow Request Approval Flow
    activate SN
    SN->>Lib: Task: Ask For Approval (Notify Librarians)
    activate Lib
    Lib-->>SN: Decision: Approved (or Rejected)
    deactivate Lib
    
    alt Decision is Approved
        SN->>DB: Update Book Record (u_status = 'Issued')
        SN->>DB: Update Borrow Request (u_status = 'Approved')
        SN->>Student: Send Confirmation Email (SMTP Notification)
    else Decision is Rejected
        SN->>DB: Update Borrow Request (u_status = 'Rejected')
    end
    deactivate SN
```

---

## 7. Testing the Flow
To verify correct flow automation, test cases are run in a sandbox instance:

| Test Case | Inputs | Expected Output | Verification Status |
| :--- | :--- | :--- | :---: |
| **1. Auto Trigger** | Create Request with Status = `Requested` | Flow activates; context log shows "Running" | ✔ Checked |
| **2. Approval Execution** | Click "Approve" in Librarian dashboard | Book Status changes to `Issued` | ✔ Checked |
| **3. Request Status Change** | Click "Approve" in Librarian dashboard | Borrow Request status updates to `Approved` | ✔ Checked |
| **4. SMTP Check** | Click "Approve" in Librarian dashboard | Outbound mail queue records approval email | ✔ Checked |
| **5. Rejection Path** | Click "Reject" in Librarian dashboard | Borrow Request status updates to `Rejected` | ✔ Checked |

---

## 8. Expected Outcome
After completing this task:
* Borrow Requests trigger automatically.
* Librarian receives approval requests.
* Book status updates to Issued upon approval.
* Borrow Request status updates to Approved.
* Students receive email notifications.
* Manual approval work is minimized.

## 9. Benefits
* **Automates Request Management**: Eliminates manual checklist routines.
* **Prevents Process Looping**: Standardizes the sequence of approvals.
* **Maintains Stock Accuracy**: Immediately blocks books as "Issued" once checked out.
* **Improves Communication**: Auto-sends emails, keeping students informed without librarian email drafting.
* **Integrates with System Records**: Provides visual debug details and execution history.

## 10. Conclusion
The Borrow Request Approval Flow successfully automates the core library borrowing process within the Smart Library Request Workflow application. By integrating approval, record updates, and email notifications into a single automated workflow, the system improves operational efficiency, reduces manual intervention, and ensures accurate tracking of books and borrow requests. This implementation demonstrates the power of ServiceNow Flow Designer in streamlining business processes through low-code automation.
