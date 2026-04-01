# TMU Co-op Support Application (CSA)
## Comprehensive Test Plan
**Course:** CPS 406 - Software Engineering 

This document outlines an end-to-end testing script to verify the core functional requirements of the CSA portal.

---

### Prerequisites
1. Ensure the backend server is running (`npm run server`).
2. Ensure the frontend development server is running (`npm run dev`).
3. Open the application in your browser (`http://localhost:5173/login`).

⚠️ **Note:** To start from a clean state at any time, stop your backend server, run `rm server/csa.db`, and restart `npm run server`. The database will auto-seed with fresh mock data.

---

### Test Scenario 1: Coordinator Account Management

**Objective:** Verify that the Coordinator can log in and successfully create placeholder accounts for students to apply with.

| Step | Action | Expected Output | Pass/Fail |
| :--- | :----- | :-------------- | :-------- |
| 1.1 | On the Login screen, click the **Coordinator** demo access button. | Logs into the Coordinator dashboard. Profile badge shows "C" or coordinator's initial. | [ ] |
| 1.2 | In the sidebar, navigate to the **Student Accounts** tab. | A list of 6 existing students is shown (some "Not yet applied", some with statuses). | [ ] |
| 1.3 | Scroll down to the **Create Student Account** form. | The form is visible. | [ ] |
| 1.4 | Enter Full Name: "Test Student", Email: "test.student@torontomu.ca", Username: "tstudent", Password: "password123". | Inputs accept the values. | [ ] |
| 1.5 | Click the **Create Account** button. | A green success banner appears saying "Success: Account created for Test Student". | [ ] |
| 1.6 | Scroll to the "All Student Accounts" table. | "Test Student" appears at the bottom with username `tstudent` and status `Not yet applied`. | [ ] |
| 1.7 | Click **Logout** in the bottom left sidebar. | Redirected smoothly back to the Login screen. | [ ] |

---

### Test Scenario 2: Student Application Flow

**Objective:** Verify that a student with a new account (or an unapplied demo account) is forced to submit an application and upload a required PDF resume, then is smoothly transitioned to their dashboard.

| Step | Action | Expected Output | Pass/Fail |
| :--- | :----- | :-------------- | :-------- |
| 2.1 | On the Login screen, under "Username", enter `tstudent` (or `dkim`). Under password, enter `password123`. | Inputs accept the values. | [ ] |
| 2.2 | Click **Sign In**. | Redirects to a special "Submit New Application" page. Topbar reads "Hello, Test!" (or David). | [ ] |
| 2.3 | Attempt to click the "Submit Application" button immediately without filling the form. | A red error banner appears saying "Error: All text fields are required." | [ ] |
| 2.4 | Fill in Student ID as `123`. Click Submit. | A red error banner complains about university format (min 7 digits). | [ ] |
| 2.5 | Readjust Student ID to `501999888` and click Submit. | A red error banner appears stating a PDF resume upload is required. | [ ] |
| 2.6 | Under "Resume Upload", click to browse or drag & drop *any non-PDF file* (e.g., an image). | Form displays an error: "Error: Resume must be a PDF file." | [ ] |
| 2.7 | Upload a valid PDF file. | "Selected: filename.pdf" appears in blue text below the dropzone. | [ ] |
| 2.8 | Click **Submit Application**. | Screen briefly flashes a "Loading..." spinner, then transitions smoothly into the normal Applicant Dashboard. | [ ] |
| 2.9 | Verify the main dashboard. | Admission Status shows **PENDING**. | [ ] |
| 2.10 | Click the **Documents** tab in the sidebar. | The document you uploaded during the application process is already visibly listed in the table. | [ ] |

---

### Test Scenario 3: Real-Time Coordinator Management

**Objective:** Verify the Coordinator receives the new application instantly and can manage statuses and missing requirements.

| Step | Action | Expected Output | Pass/Fail |
| :--- | :----- | :-------------- | :-------- |
| 3.1 | As the newly applied student, click **Logout**. Log back in via the **Coordinator** quick demo button. | Logs into the Coordinator dashboard. | [ ] |
| 3.2 | Ensure you are on the **Applications** (default) tab. Scroll to the bottom of the table. | "Test Student" (or "David Kim") is listed with a "PENDING" yellow status badge. | [ ] |
| 3.3 | Under "Actions" for that specific student row, click the **Reject** button (red X). | The badge instantly flips from yellow "PENDING" to red "REJECTED". | [ ] |
| 3.4 | Click the **Accept** button (green check) on the exact same row. | The badge instantly flips from red "REJECTED" to green "FINAL ACCEPTANCE". | [ ] |
| 3.5 | In the sidebar, click the **Requirements** tab. | "Test Student" is listed as having Uploaded files and a green "Complete" missing document badge. | [ ] |
| 3.6 | Look for "Priya Sharma" or "Sofia Rodriguez" on the same list. | They are highlighted with red warning rows, 0 files, and a "Missing" document badge. | [ ] |
| 3.7 | Under "Actions", click the secondary **Remind** button for Priya. | A green banner appears at the top confirming the reminder was sent to Priya. | [ ] |

---

### Test Scenario 4: Returning Student Flow

**Objective:** Verify that an applied student can upload and delete new documents.

| Step | Action | Expected Output | Pass/Fail |
| :--- | :----- | :-------------- | :-------- |
| 4.1 | Log out from Coordinator. Log in as `achen` (password: `password123`) using manual input. | Logs directly into Alex Chen's dashboard. His application form is bypassed. | [ ] |
| 4.2 | Go to the **Documents** tab. | Alex already has a "Resume.pdf" showing up there. | [ ] |
| 4.3 | Click the red **Remove** button next to his Resume. | The file disappears from the list, indicating deletion from the database. | [ ] |
| 4.4 | Use the top dropzone to upload a new PDF file (e.g. WorkTermEvaluation.pdf). | The blue loader circle appears, and the new file populates the document table. | [ ] |
| 4.5 | Check the "Uploaded At" column for that new file. | It accurately records today's timestamp and date instead of 'N/A'. | [ ] |

---
**End of Test Document**
