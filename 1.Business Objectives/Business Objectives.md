# Smart Library Request Workflow & Prism Library Management System

## Task 1 – Business Objectives

### Introduction
Traditional library management systems often suffer from manual overhead, slow request fulfillment times, high rates of book misplacement, and poor user experiences. Students and faculty frequently face delays in borrowing or returning books, while administrators struggle with paper-based tracking, incorrect stock counts, and inefficient rack organization.

The **Smart Library Request Workflow** aims to automate and streamline these operations by leveraging ServiceNow's robust workflow capabilities. By replacing outdated manual methods with an automated, state-tracked request engine, the system ensures seamless book acquisition, tracking, and returns.

### System Architecture Overview
The system is divided into two primary environments: the request fulfillment engine in ServiceNow, and the core inventory and member tracker in the Prism Library Management System.

#### Figure 1: Conceptual illustration of an automated Smart Library Request Workflow in ServiceNow
<img width="1024" height="411" alt="media__1782810909775" src="https://github.com/user-attachments/assets/6e6b602f-49b1-4bd1-b979-8995c7c76554" />


This workflow represents the ServiceNow Service Catalog Request execution model:
- **Begin State**: The workflow begins immediately upon a student submitting a library request through the Service Catalog.
- **Parallel Subtask Execution**: To optimize fulfillment time, the workflow simultaneously creates and runs two subtasks:
  - **Task 1 (Create Task)**: Handles verification of the member's profile and catalog eligibility.
  - **Task 2 (Create Task)**: Initiates inventory checking and physical book location tracing.
- **Synchronization and Consolidation**: Once both Task 1 and Task 2 transition to a completed state, the flow triggers **Task 3 (Create Task)** to bundle the request for final librarian sign-off or physical pickup.
- **End State**: Upon completion of the fulfillment steps, the request transitions to the End node, archiving the transaction and updating the database.

---

#### Figure 2: Prism Library Management System Modular Structure
<img width="1024" height="593" alt="media__1782810920068" src="https://github.com/user-attachments/assets/cb3cb5d0-4be3-4395-95be-340b29a3b7b8" />


The back-end and operations management are handled by the **Prism Library Management System**, which is structured into five distinct operational modules:
1. **Member Management**:
   - **New Member Added**: Registers students, faculty, and community members.
   - **Member List**: Displays and searches registered members, their roles, and borrow privileges.
2. **Author Management**:
   - **Author Added**: Registers authors in the database.
   - **Author List**: Maintains detailed records of authors and their corresponding catalogs.
3. **Rack Management**:
   - **Add Rack**: Configures physical layout elements in the library.
   - **Add Book to Rack**: Associates books with specific shelf/rack locations.
   - **Rack Wise Book View**: Provides librarians with a visual directory of where books are stored to speed up physical collection.
4. **Book Management**:
   - **Add Genre & Add Book**: Adds new publications to the catalog, indexed by category/genre.
   - **Book List**: Central index of all available physical and digital items.
   - **Lending**: Coordinates book lending flows.
     - **Lending List**: Tracks currently checked-out materials.
     - **Upcoming Book Return List**: Generates automated alerts for books due shortly.
   - **Borrow**: Registers student requests to check out materials.
     - **Borrow Request Created & Borrow Request List**: Manages incoming student requests.
5. **Book Receive**:
   - Handles the return check-in procedure, verifying book condition and updating the repository.

---

### Business Objectives
1. **Automate Library Operations**: Eliminate manual processing by introducing automated state changes in ServiceNow. This reduces the administrative burden on librarians and speeds up processing times.
2. **Improve Book Tracking**: Maintain real-time logs of book locations (Rack Management) and lending states (Lending/Borrow lists) to drastically reduce book misplacement and inventory loss.
3. **Enhance Student Experience**: Provide students with an intuitive Service Catalog interface to request, renew, or return books, along with self-service status updates and automatic return reminders.

---

### Transaction Flow Logic
To illustrate how a student interacts with the system, the activity diagram below details the step-by-step logic for borrowing and returning physical books.

#### Figure 3: Borrowing and Returning Activity Diagram
<img width="1024" height="676" alt="media__1782810932240" src="https://github.com/user-attachments/assets/e68815a4-929c-491a-b092-25b7d009933f" />


The system distinguishes requests based on transaction types:
- **Borrowing Flow**:
  1. Student requests to borrow a book.
  2. Student presents their physical or digital Library ID.
  3. The system queries availability:
     - **If Book is Available**:
       - Issue the book to the student.
       - Update book inventory records (reducing available count).
       - Update student profile records (logging the active loan).
       - Display the success message: *"Book issued successfully"*.
     - **If Book is Not Available**:
       - Display failure message: *"Book not available"*.
- **Returning Flow**:
  1. Student requests to return a book.
  2. Student presents their Library ID.
  3. Physical books are returned to the library counter.
  4. The system updates book inventory records (increasing available count).
  5. The system updates student profile records (clearing the active loan).
  6. Display success message: *"Book returned successfully"*.
- **Fallback Flow**:
  - If the student's request is neither a borrow nor a return, the system displays: *"Invalid request"*.
- All branches converge at a merge node before terminating at the final activity state.

---

### Expected Outcome
By integrating ServiceNow Catalog Workflows with the Prism Library Management System:
- **Zero Manual Log Sheets**: All transactions are digitally tracked.
- **Fulfillment SLA Reductions**: Request-to-handover time is reduced from days to minutes.
- **High Inventory Accuracy**: Immediate inventory database updates upon borrowing or receiving.
- **Proactive Notifications**: Automated upcoming return alerts lower overdue rates by an estimated 40%.

### Benefits
- **For Students**: A transparent, fast, and mobile-friendly request experience with instant notifications.
- **For Librarians**: Reduced administrative workload, clear rack mapping for physical retrieval, and automated overdue tracking.
- **For Administrators**: Rich reports on book popularity, check-out velocities, and overall library usage metrics.

### Conclusion
The Smart Library Request Workflow bridges the gap between old-school physical tracking and modern enterprise service management. Leveraging ServiceNow for request workflows and Prism for database and inventory tracking creates a highly scalable, reliable, and user-friendly solution for educational institutes and public library systems alike.
