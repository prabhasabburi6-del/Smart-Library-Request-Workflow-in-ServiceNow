# Smart Library Request Workflow in ServiceNow
## Section 21: Project Demo Video Planning Documentation

## 1. Objective
The objective of this task is to prepare a structured demonstration plan for showcasing the Smart Library Request Workflow application. The demo highlights the complete borrowing lifecycle, including role-based access, workflow automation, approvals, notifications, and reporting. This plan ensures the presentation is organized, easy to follow, and demonstrates all major functionalities of the project.

## 2. Introduction
A project demonstration is an important part of validating the application's functionality. The demo should provide a complete walkthrough from the perspective of both Student and Librarian, illustrating how the ServiceNow platform automates library operations.

The demonstration focuses on:
* Library Service Portal
* Book Borrow Request
* Approval Workflow
* Book Status Updates
* Email Notifications
* Reports
* Role-Based Security

---

## 3. Demo Overview

| Demo Section | Demonstration Target | Traversal Path |
| :--- | :--- | :--- |
| **Login** | Role separation demonstration | Student user session / Librarian user session |
| **Portal Walkthrough** | UI/UX portal overview | Home ──> Service Catalog ──> Library Services |
| **Borrow Request** | Input form fields & validation | Student fills book reference and submits |
| **Approval** | Workflow task interception | Librarian receives email/inbox approval request |
| **Automation** | System updates | Flow Designer triggers state changes |
| **Notification** | Verification of email alerts | View SMTP queue outbound confirmation email |
| **Return Process** | Check-in procedure | Book status reset to `Available` |
| **Reporting** | Dashboard aggregations | Bar chart ranks checkout velocity |

---

## 4. Demo Environment
* **Platform**: ServiceNow Personal Developer Instance (PDI)
* **Application Scope**: Smart Library Request Workflow (`x_library`)
* **Test Users**: 
  * `student_user` (equipped with `x_library.student` role)
  * `librarian_user` (equipped with `x_library.librarian` role)
* **Browser**: Google Chrome / Microsoft Edge (running incognito to hold simultaneous active sessions)

---

## 5. Demo Flow

### Step 1 – Login & Home Page
1. Open Google Chrome.
2. Log in as a Student user using PDI user credentials.
3. Open the Service Portal landing page: `/sp?id=library_portal`.
4. Verify that the user dashboard displays personal active loans.

#### UI Mockup 1: Service Portal Landing Page
```
================================================================================
| [Book Logo]  Smart Library Service Portal                [My Requests] [LogOut] |
================================================================================
| Welcome Back, Student User!                                                  |
| Quick Actions:                                                               |
|  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐            |
|  │  Request a Book  │  │   Active Loans   │  │   Return Guide   │            |
|  │   [ Open Form ]  │  │   [ 2 Records ]  │  │   [ View Info ]  │            |
|  └──────────────────┘  └──────────────────┘  └──────────────────┘            |
================================================================================
```
*Figure 1: Student logged into the ServiceNow portal.*

---

### Step 2 – Portal Walkthrough
1. Click on the **Request a Book** quick action (or navigate to Service Catalog -> Library Services).
2. The user is redirected to the Service Catalog categories list displaying available libraries.

#### UI Mockup 2: Library Services Category Page
```
================================================================================
| Home > Service Catalog > Library Services                                    |
================================================================================
| Category: Library Services                                                   |
| Displays standard requests for academic literature and study rooms.          |
|                                                                              |
|  * Book Checkout Request                                                     |
|    "Submit a new borrow request for catalogued books."                       |
|                                                                              |
|  * Study Room Reservation                                                    |
|    "Reserve a quiet group room in the central library."                      |
================================================================================
```
*Figure 2: Library Services page.*

---

### Step 3 – Submit Borrow Request
1. Open the **Book Checkout Request** catalog item form.
2. Configure form:
   * **Requested By**: Auto-populated to current Student.
   * **Book**: Click lookup, select *Java Programming* (only Available items are listed).
   * **Request Date**: Auto-filled with timestamp.
3. Click **Submit**.

#### Figure 3: Borrow Request form layout prior to submission
![ServiceNow Completed Borrow Request Form View](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_completed_borrow_request_form_1782821559877.jpg)

---

### Step 4 – Librarian Approval
1. Switch browser to the Librarian incognito session.
2. Navigate to **ServiceNow Standard UI** ──> **Self-Service** ──> **My Approvals**.
3. Locate the pending approval for *Java Programming*.
4. Click **Approve**.

#### UI Mockup 4: Librarian Approvals List
```
================================================================================
|  My Approvals                                                                |
================================================================================
|  State     | Approver     | Source Table     | Document Details              |
--------------------------------------------------------------------------------
|  Requested | Librarian    | u_borrow_request | Request BK001004 - Student   |
|            |              |                  | [ Approve ] [ Reject ]        |
================================================================================
```
*Figure 4: Librarian approving the borrow request.*

---

### Step 5 – Email Notification
1. Switch to administrator view.
2. Open System Logs -> Emails.
3. Search for outgoing mails where subject is "Your Borrow Request has been Approved".
4. Open the log to display the SMTP mail body.

#### UI Mockup 5: ServiceNow Outgoing Email Log
```
================================================================================
|  Email Log: Your Borrow Request has been Approved                            |
================================================================================
|  To:      student@edu.com                                                    |
|  Subject: Your Borrow Request has been Approved                              |
|  Body:                                                                       |
|  Hello Student User,                                                         |
|  The book "Java Programming" has been issued to you. Please collect it.       |
================================================================================
```
*Figure 5: Approval email notification.*

---

### Step 6 – Status Tracking
1. Open the `u_borrow_request` record for `BK001004`.
2. Check that the Status column has updated to `Approved` (or `Issued`).
3. Open the corresponding Book record `u_book` for *Java Programming*.
4. Check that status has updated to `Issued`.

#### UI Mockup 6: Status columns synchronization
```
================================================================================
|  Book [u_book] : Java Programming                                            |
================================================================================
|  Title:   Java Programming                                                   |
|  Status:  [ Issued                                                      |▼]  |
================================================================================
```
*Figure 6: Borrow Request and Book status tracking.*

---

### Step 7 – Return Process
1. Impersonating the Librarian user, open the `BK001004` Borrow Request record.
2. Change the **Status** choice field to `Returned`.
3. Click **Update**.
4. Open the *Java Programming* Book record and verify that status has reset to `Available`.

#### UI Mockup 7: Book Returned validation state
```
================================================================================
|  Book [u_book] : Java Programming                                            |
================================================================================
|  Title:   Java Programming                                                   |
|  Status:  [ Available                                                   |▼]  |
================================================================================
```
*Figure 7: Book return process completed.*

---

### Step 8 – Report Demonstration
1. Open **Reports** ──> **View / Run**.
2. Search and open **Most Borrowed Books**.
3. Click **Run** to generate the bar chart showing aggregate metrics.

#### Figure 8: Most Borrowed Books bar chart visualization report
![ServiceNow Most Borrowed Books Bar Chart Preview](C:/Users/harik/.gemini/antigravity/brain/7bb71848-cec9-43d6-b8fd-89e02e62b9c9/servicenow_most_borrowed_books_report_1782834870049.jpg)

---

## 6. Complete Demo Workflow
```
[Student Login] ──> [Portal View] ──> [Submit Request] ──> [Flow Designer Trigger] ──> [Librarian Approval] ──> [Book Issued] ──> [Email Sent] ──> [Status Sync] ──> [Book Returned] ──> [Generate Report]
```

---

## 7. Suggested Demo Video Script Timeline

| Time Segment | Demo Activity | Key Screen Element to Highlight |
| :--- | :--- | :--- |
| **00:00 – 00:30** | Project Introduction | Slides mapping title: "Smart Library Request Workflow in ServiceNow" |
| **00:30 – 01:30** | Student Login & Portal Walkthrough | Service Portal Homepage; Active Reservation widgets |
| **01:30 – 03:00** | Borrow Request Submission | Selecting Available book from lookup; form submission toast |
| **03:00 – 04:30** | Librarian Approval | Impersonation transition; Approvals List layout |
| **04:30 – 05:30** | Book Status & Email Notification | u_book status set to `Issued`; email logs view |
| **05:30 – 06:30** | Return Process | Resetting request state to `Returned`; stock reset checks |
| **06:30 – 07:30** | Reports | Open Report view; dynamic bar height calculations |
| **07:30 – 08:00** | Project Summary | Conclusion slides outlining internship deliverables |

---

## 8. Expected Outcome
After completing the demonstration:
* Users understand the application workflow.
* Borrow request lifecycle is successfully demonstrated.
* Automation features are validated.
* Role-based access is verified.
* Reports and notifications function correctly.
* The application is ready for deployment and evaluation.

## 9. Benefits
* **Structured Walkthrough**: Guarantees all features are presented cleanly.
* **Shows Automation Values**: Flow logs trace the real-time execution.
* **Demonstrates Security Profile**: Validates student restrictions under impersonation.
* **Speeds Presentation Time**: Follows a strict timeline, keeping the demonstration concise.

## 10. Conclusion
The Project Demo Video Plan provides a structured approach for presenting the Smart Library Request Workflow application. By demonstrating the complete process—from student login and borrow request submission to librarian approval, automated status updates, email notifications, book return, and reporting—the video effectively showcases the application's functionality and the practical use of ServiceNow features. Following this plan ensures a professional, concise, and comprehensive demonstration suitable for the SmartBridge internship evaluation.
