# Smart Library Request Workflow in ServiceNow
## Section 17: Adherence to Timelines Documentation

## 1. Objective
The objective of this task is to ensure that the Smart Library Request Workflow project was completed according to the planned schedule and milestones. Following a structured timeline helped organize development activities, monitor progress, complete tasks on time, and deliver a fully functional ServiceNow application within the internship duration.

## 2. Introduction
Effective project management requires proper planning and timely execution of each development phase. The Smart Library Request Workflow project was divided into multiple milestones, covering requirements analysis, backend configuration, workflow automation, security implementation, reporting, testing, and validation.

By completing each milestone according to the planned schedule, the project maintained consistency, minimized delays, and ensured successful completion.

---

## 3. Project Timeline

| Phase | Milestone / Activity | Target Duration | Completion Status |
| :--- | :--- | :---: | :---: |
| **Phase 1** | Creation of Roles (`student`, `librarian`) | Day 1-2 | ✅ Completed |
| **Phase 2** | Creation of Tables (`u_book`, `u_borrow_request`) | Day 3-5 | ✅ Completed |
| **Phase 3** | Adding Fields to Book Table | Day 6-7 | ✅ Completed |
| **Phase 4** | Adding Fields to Borrow Request Table | Day 8-9 | ✅ Completed |
| **Phase 5** | Reference Qualifier Configuration | Day 10 | ✅ Completed |
| **Phase 6** | Flow Designer Implementation (Borrow Request Approval Flow) | Day 11-13| ✅ Completed |
| **Phase 7** | UI Policy Configuration (Return Date Rules) | Day 14 | ✅ Completed |
| **Phase 8** | Access Control (ACL) Configuration (Book/Borrow Request CRUD) | Day 15-16| ✅ Completed |
| **Phase 9** | Report Creation (Most Borrowed Books Bar Chart) | Day 17 | ✅ Completed |
| **Phase 10**| End-to-End Testing & Validation (User Impersonation) | Day 18-20| ✅ Completed |

---

## 4. Project Execution Milestones

### Milestone 1 – Creation of Roles
User roles were created to implement Role-Based Access Control (RBAC) to separate student activities from librarians.
* **Activities Completed**: Created Student role (`x_library.student`) and Librarian role (`x_library.librarian`), verified role registry.

#### Figure 1: Student and Librarian roles successfully created and visible in the Roles list
![ServiceNow Custom User Roles Verification](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_roles_list_view_1782821056814.jpg)

---

### Milestone 2 – Creation of Tables
Created the core database tables representing physical assets and transactions.
* **Activities Completed**: Created Book (`u_book`) and Borrow Request (`u_borrow_request`) tables.

#### Figure 2: Custom tables created successfully and visible in the System Definition list
![ServiceNow Custom Tables List Verification](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_tables_list_view_1782821000904.jpg)

---

### Milestone 3 – Adding Fields
Configured essential dictionary entries and options on both tables.
* **Activities Completed**: Added Title, Author, ISBN, and Status choice fields (Book table); Added Requested By, Book reference, Request Date, and Status choice fields (Borrow Request table).

#### Figure 3: Required fields added to Book table and verified on new record form
![ServiceNow Book Form Verified Layout](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_completed_book_form_1782820975182.jpg)

---

### Milestone 4 – Reference Qualifier
Configured reference filtering rules to prevent requesting books that are out of stock.
* **Activities Completed**: Configured Simple Reference Qualifier on Book field to enforce `Status = Available`.

#### UI Mockup 4: Reference Qualifier filter condition set to Status = Available
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
*Figure 4: Reference Qualifier filtering available books.*

---

### Milestone 5 – Flow Designer
Implemented request workflow automation.
* **Activities Completed**: Formulated trigger, Ask for Approval rule (Librarians), record updates (Book status to `Issued`, Request status to `Approved`), and SMTP email notification dispatch.

#### UI Mockup 5: Active Borrow Request Approval Flow canvas
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

### Milestone 6 – UI Policy
Exposed field behaviors dynamically in the user's web browser layout.
* **Activities Completed**: Added UI Policy to set `Return Date` visible and mandatory when `Status = Issued`.

#### UI Mockup 6: Return Date field displaying with mandatory asterisk (*)
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

### Milestone 7 – Access Control
Applied table-level and column-level database security.
* **Activities Completed**: Created Read, Create, Write, and Delete ACL records restricting book records and check-out tracking controls.

#### Figure 7: Role-Based Access Control Create ACL for Borrow Request table
![ServiceNow Borrow Request Table ACLs](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_borrow_request_create_acl_1782834434011.jpg)

---

### Milestone 8 – Report Creation
Created live visual representations of checked-out inventory.
* **Activities Completed**: Grouped approved borrowing requests by book to render a horizontal bar chart.

#### Figure 8: Most Borrowed Books report preview
![ServiceNow Most Borrowed Books Bar Chart Report](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_most_borrowed_books_report_1782834870049.jpg)

---

### Milestone 9 – Testing & Validation
Conducted comprehensive verification test suites using Student and Librarian personas.
* **Activities Completed**: User impersonation testing, email logs verification, and boundary validation checks.

#### Figure 9: Student view impersonation testing confirming read-only fields
![ServiceNow Impersonated Student View Verification](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_borrow_request_student_view_1782834455057.jpg)

---

## 5. Timeline Workflow
```
[Project Planning] ──> [Create Roles] ──> [Create Tables] ──> [Add Fields] ──> [Reference Qualifier] ──> [Flow Designer] ──> [UI Policy] ──> [Access Control] ──> [Create Reports] ──> [Testing & Validation] ──> [Project Completed]
```

---

## 6. Project Progress Summary

| Work Package / Task | Scope Target | Completion Status |
| :--- | :--- | :---: |
| **Requirement Analysis** | Map ServiceNow platform potentials to library requirements | ✅ Completed |
| **Business Objectives** | Identify core metrics: automation, tracking, UX | ✅ Completed |
| **Functional Scope** | Define module bounds and workflows | ✅ Completed |
| **Stakeholder Mapping** | Map personas: Students, Librarians, Admin, Faculty, IT | ✅ Completed |
| **Execution Roadmap** | Build step-by-step 10-phase sequence roadmap | ✅ Completed |
| **Role Creation** | Create `student` and `librarian` roles | ✅ Completed |
| **Table Creation** | Create `u_book` and `u_borrow_request` schemas | ✅ Completed |
| **Field Configuration** | Create columns, choices, and defaults | ✅ Completed |
| **Reference Qualifier** | Filter reference lookups to available items only | ✅ Completed |
| **Flow Designer** | Set up approval workflow, automated emails, and updates | ✅ Completed |
| **UI Policy** | Enforce Return Date input for active checkouts | ✅ Completed |
| **Access Control** | Limit Write/Delete actions to Librarian role | ✅ Completed |
| **Report Creation** | Build Most Borrowed Books horizontal bar chart | ✅ Completed |
| **Testing & Validation** | Run test cases on sandbox PDI environment | ✅ Completed |

---

## 7. Expected Outcome
After following the project timeline:
* All milestones were completed successfully.
* No major delays occurred.
* Each development phase was validated before moving to the next.
* The project was completed within the planned internship schedule.
* The application was ready for deployment and demonstration.

## 8. Benefits
* **Improved Project Organization**: Clear milestones prevented configuration blocks.
* **Timely Completion**: Work Packages were delivered within target duration bounds.
* **Resource Optimization**: Minimized database redesigns by completing roles and tables first.
* **Mitigated Risks**: Automated tests and verification checklists confirmed security ACL rules before shipping.

## 9. Conclusion
The Smart Library Request Workflow project was executed according to the planned milestones and development schedule. Each phase, from role creation and table configuration to workflow automation, security implementation, reporting, and testing, was completed in sequence and validated before proceeding to the next stage. Adhering to the defined timeline ensured efficient project management, minimized delays, and resulted in the successful delivery of a secure and fully functional ServiceNow application.
