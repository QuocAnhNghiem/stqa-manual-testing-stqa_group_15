# Test Summary — Software Quality Report

> **Objective**: Summarize the results from `test-execution.md` and `bug-reports.md`, evaluate the software quality, and propose the next steps.

## 1. Team Information

| Item                   | Information                  |
| ---------------------- | ---------------------------- |
| **Team**               | stqa_group_15                |
| **Class**              | SE125.O1                     |
| **Report Date**        | 06/06/2026                   |
| **Testing System**     | https://stqa.rbc.vn — v1.0   |

---

## 2. Execution Overview

| Metric                 | Value   |
| ---------------------- | ------- |
| Total Test Cases       | 35      |
| Pass                   | 25      |
| Fail                   | 10      |
| Blocked                | 0       |
| Not Run                | 0       |
| **Pass Rate**          | 71.4%   |
| **Total Bugs Found**   | 11      |

### Results by Functional Group

| Functional Group       | TC  | Pass | Fail | Bugs | Evaluation      |
| ---------------------- | --- | ---- | ---- | ---- | --------------- |
| Login                  | 6   | 6    | 0    | 0    | Stable          |
| Book List              | 3   | 2    | 1    | 1    | UI/Seed Bug     |
| Search/Filter          | 7   | 6    | 1    | 1    | Logic Bug (AND/OR) |
| Borrow Book            | 8   | 6    | 2    | 3    | Critical (Missing Librarian role feature) |
| Return Book            | 3   | 2    | 1    | 1    | Missing Warning Bug |
| Overdue Checking       | 2   | 1    | 1    | 2    | Boundary & State Logic Bugs |
| Member Management      | 4   | 1    | 3    | 2    | Critical (Regex blocking users) |
| View Borrow Records    | 1   | 0    | 1    | 1    | Highly Critical (Authorization) |
| General (Seed Data)    | 1   | 1    | 0    | 0    | Stable          |

---

## 3. Software Quality Assessment

Based on the manual testing results comprising 35 Test Cases covering all 8 requirements (REQ) of the system, the team evaluates the quality of the current version as follows:

**Strengths:**
- Basic features such as Login, Book List Display, and Search/Filter operate smoothly with a good response time.
- The data restoration feature (Seed data) works exactly as designed, greatly assisting repetitive testing processes.
- The happy-path flows for borrowing/returning books do not have major issues.

**Weaknesses:**
- **Security & Authorization Flaw (Critical):** The system fails to enforce data ownership permissions. BUG-11 allows a member to view and manipulate (return books) borrow records belonging to other members.
- **Core Business Logic Errors (High):** The Librarian completely lacks a "Borrow" button (BUG-05), breaking the primary over-the-counter service workflow. The email validation regular expression (Regex) is reversed (BUG-10), paralyzing the ability to create new members.
- **Business Rule Violations:** The system fails to enforce the borrowing limit of 3 books per member (BUG-03).
- **Boundary and UI Bugs:** Boundary value handling for dates is incorrect (BUG-07).

**Conclusion:**
Although the Pass Rate seems acceptable (71.4%), the failing test cases affect the most crucial, life-line features of the library system (Member Registration, Borrow Limits, Access Control). Therefore, the current product is evaluated as **NOT READY FOR RELEASE**. It must not be deployed to a production environment until all High and Critical priority bugs are resolved.

---

## 4. Bug Triage & Proposed Fixes

Based on the Severity of the identified bugs, the team proposes the following resolution sequence for the Dev Team:

**🔥 Priority 1 (Critical & High) - Must fix immediately:**
- **BUG-11 (Authorization):** Add an ownership check (`memberId == currentUserId`) when querying and performing the Return action on borrow records for Member accounts.
- **BUG-10 (Create Member):** Fix the reversed email Regex (currently negating valid formats `!`) to allow the creation of valid accounts.
- **BUG-05 (Librarian UI):** Add a "Borrow" button to the Book list for the Librarian account, integrating a popup to select the borrowing member.
- **BUG-03 (Borrow Limit):** Implement a strict check to block borrowing transactions when the number of currently borrowed books `(active_borrows) >= 3`.

**🚧 Priority 2 (High & Medium) - Fix within this Sprint:**
- **BUG-09:** Update the email Regex to make the dot `.` in the domain mandatory.
- **BUG-04:** Correct the error message text from "Expired" to "Suspended" to accurately reflect the account's actual status.
- **BUG-06:** Implement a UI warning message when a user returns an overdue book.
- **BUG-07:** Fix the overdue date validation operator from `< today` to `<= today`.

**🛠 Priority 3 (Low) - Add to backlog to fix later:**
- **BUG-08:** Reset the state variable containing the overdue list before the system performs a new scan.
- **BUG-02:** Change the search combination filter condition from `OR` to `AND`.
- **BUG-01:** Update the seed data file (change `Borrowing` to `Returned` for BOOK003) or fix the UI status parser.

---

## 5. QA Feedback & Process Improvement

From a Quality Assurance (QA) perspective, the team provides the following observations on the process and architecture:

1. **Recommend Unit Tests for Regex & Date Logic:**
   The reversed email logic (BUG-10) and date operator error (BUG-07) are classic bugs that could have been caught early through Automated Unit Tests. We strongly recommend that the Dev team integrates Unit Testing into their CI/CD pipeline.

2. **Standardize Mock Data for Edge Cases:**
   The current sample data (Seed Data) does not support dynamic date testing (e.g., a due date falling exactly on the current day). The project team should implement system time mocking tools or a backend API to generate borrow records with custom dates, preventing testing from being dependent on the OS clock.

3. **Zero Trust Principle for Backend API:**
   The fact that the UI allows a user to click the "Return" button on another member's book (BUG-11) is a critical flaw. The team must implement the *Zero Trust* design principle: The backend API must always re-verify the user's identity (Session/Token) and permissions for all Create/Read/Update/Delete operations, regardless of the command sent by the Client UI.

---

## AI Usage Declaration
During the testing and documentation process, the team used AI tools (Antigravity Assistant) to help refine the tone of the bug reports, ensure accurate mapping between Test Cases and Bug IDs, and format Markdown tables to enhance professionalism. The core testing logic, test execution, and bug discoveries are entirely the actual effort of the team.
