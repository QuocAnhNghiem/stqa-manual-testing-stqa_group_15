# Test Cases

> **Instructions**: Write a minimum of **20 TCs** covering all core features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) for how to write a good TC.
> Organize and group test cases logically.

| Information | |
| --- | --- |
| **Team** | stqa_group_15 |
| **Date Created** | 06/06/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

### IDM — Login (REQ-01)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| Email exists in DB? | Yes | `librarian@library.com` | Login successful |
| | No | `noone@email.com` | Error message |
| Password correct? | Yes | `admin123` | Login successful |
| | No | `wrongpass` | Error message |
| Input fields empty? | Not empty | (any value) | Normal processing |
| | Both empty | `""` | Message "Please enter..." |
| | Only email empty | Email: `""`, PW: `admin123` | Message "Please enter..." |
| | Only PW empty | Email: `librarian@library.com`, PW: `""` | Message "Please enter..." |

### IDM — Search Books (REQ-03)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| Keyword exists in DB? | Yes (book name) | `"Flutter"` | Displays books containing "Flutter" |
| | Yes (author name) | `"Nguyễn"` | Displays books by author Nguyễn |
| | No | `"XYZ123"` | Empty list |
| Case sensitivity? | Lowercase | `"flutter"` | Same result as "Flutter" |
| | Uppercase | `"FLUTTER"` | Same result as "Flutter" |
| Combine search + filter? | Yes | Filter "Công nghệ" + search "Flutter" | Shows books matching both |
| | No | Only search or only filter | Results based on 1 criteria |

### IDM — Borrow Books (REQ-04, REQ-05)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| Book status? | Available | BOOK001 | Allowed to borrow |
| | Borrowed | BOOK003 | Not allowed |
| | Lost | BOOK007 | Not allowed |
| Member status? | Active | MEM002 | Allowed to borrow |
| | Suspended | MEM004 | Denied, error message |
| | Expired | MEM005 | Denied, error message |
| Active borrows? | BVA: 0 (below limit) | MEM003 (0 books in seed) | Allowed to borrow |
| | BVA: 1 (below limit) | MEM006 (1 book: BOOK013) | Allowed to borrow |
| | BVA: 2 (below limit) | MEM002 (2 books: BOOK003 + BOOK008) | Allowed to borrow |
| | BVA: 3 (at limit) | MEM002 (3 books: BOOK003 + BOOK008 + BOOK009) | Denied, exceed limit message |

### IDM — Return Books (REQ-05)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| Book currently borrowed by member? | Yes | MEM002 borrowing BOOK003 | Allowed to return, status to "Available" |
| | No | MEM003 doesn't have BOOK003 | No return option for BOOK003 |
| Return overdue? | Yes | BR001 (dueDate 15/09/2024) | Overdue warning |
| | No | New record within limit | No warning |

### IDM — Overdue (REQ-06)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| Librarian triggers overdue check? | Yes | Clicks "Check Overdue" | Overdue records marked as "Overdue" |
| | No | Doesn't click | Status unchanged |
| Viewer of overdue records | Librarian | librarian@library.com | Views all overdue records |
| | Member | ba.nguyen@email.com | Views only their own overdue records |

### IDM — Member Management (REQ-07)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| Email valid? | Valid | user@domain.com | Creation allowed |
| | Invalid (missing dot) | user@domain | Invalid email error |
| | Invalid (missing @) | userdomain.com | Invalid email error |
| Email duplicated? | Not duplicated | new.member@email.com | New member created |
| | Duplicated | ba.nguyen@email.com | Duplicated email error |

### IDM — View Borrow Records (REQ-08)
| Characteristic | Block | Value | Expected Result |
| --- | --- | --- | --- |
| User role | Librarian | librarian@library.com | Views all borrow records |
| | Member | dam.tran@email.com | Views only their own records |
| Record status | Borrowing | BR001 (MEM002) | Displays in member's list |
| | Returned | BR002, BR005 (MEM003) | Remains visible in MEM003's history |

## Step 1b: Decision Table — Borrow Book (REQ-04)

| Condition | Rule 1 | Rule 2 | Rule 3 | Rule 4 | Rule 5 |
| --- | --- | --- | --- | --- | --- |
| Book status is "Available"? | Y | N | Y | Y | Y |
| Member is active? | Y | Y | N (Suspended) | N (Expired) | Y |
| Borrowed books < 3? | Y | Y | Y | Y | N |
| **Result** | Allow borrow | Deny (borrowed/lost) | Deny (suspended) | Deny (expired) | Deny (limit exceeded) |

---

## Step 2: Test Cases

### 2.1. REQ-01: Login

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-01 | Successful login with valid account | At login page | 1. Enter email 2. Enter password 3. Click "Login" | Email: librarian@library.com; PW: admin123 | Navigates to home page, AppBar shows "Nguyễn Thủ Thư" and role Librarian | REQ-01 | EP |
| TC-02 | Error when email doesn't exist | At login page | 1. Enter email 2. Enter password 3. Click "Login" | Email: nobody@test.com; PW: anything | Displays "Member not found" | REQ-01 | EP |
| TC-03 | Error when password is incorrect | At login page | 1. Enter email 2. Enter password 3. Click "Login" | Email: ba.nguyen@email.com; PW: wrongpassword | Displays "Incorrect password" | REQ-01 | EP |
| TC-04 | Error when email and password are empty | At login page | 1. Leave email and password empty 2. Click "Login" | Email: ""; PW: "" | Displays "Please enter email and password" | REQ-01 | EP |
| TC-05 | Error when only email is empty | At login page | 1. Leave email empty 2. Enter password 3. Click "Login" | Email: ""; PW: admin123 | Displays "Please enter email and password" | REQ-01 | EP |
| TC-06 | Error when only password is empty | At login page | 1. Enter email 2. Leave password empty 3. Click "Login" | Email: librarian@library.com; PW: "" | Displays "Please enter email and password" | REQ-01 | EP |

### 2.2. REQ-02: Book List

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-07 | Display full book info matching seed data | Logged in | 1. Open "Books" tab 2. Observe list | None | Displays title, author, category, year, status; BOOK003/BOOK013 show "Borrowed"; BOOK007/BOOK020 show "Lost" | REQ-02 | EP |
| TC-08 | Book status updates real-time after borrowing | Logged in as MEM006 (borrowing BOOK013); seed data state | 1. Go to "Books" tab 2. Select BOOK002 3. Click "Borrow" | MEM006; BOOK002 | BOOK002 changes to "Borrowed" immediately after borrowing | REQ-02, REQ-04 | EP |
| TC-09 | Book status updates real-time after returning | Logged in as MEM002 (borrowing BOOK003 - BR001; overdue) | 1. Go to "Borrow/Return" tab 2. Select BR001 3. Click "Return" 4. Go to "Books" tab | MEM002; BOOK003 | System displays overdue warning; BOOK003 changes to "Available" immediately | REQ-02, REQ-05 | EP |

### 2.3. REQ-03: Search and Filter Books

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-10 | Search by book name | Logged in | 1. Go to "Books" tab 2. Enter keyword | Keyword: Flutter | List displays only "Lập trình Flutter cơ bản" (BOOK001) | REQ-03 | EP |
| TC-11 | Search by author | Logged in | 1. Go to "Books" tab 2. Enter keyword | Keyword: Nguyễn Minh Đức | Displays books by Nguyễn Minh Đức: BOOK001 and BOOK009 | REQ-03 | EP |
| TC-12 | Case-insensitive search | Logged in | 1. Go to "Books" tab 2. Enter keyword | Keyword: flutter | Results same as "Flutter" (BOOK001) | REQ-03 | EP |
| TC-13 | Message when no results found | Logged in | 1. Go to "Books" tab 2. Enter keyword | Keyword: XYZ123 | Displays "No books found" | REQ-03 | EP |
| TC-14 | Filter by category | Logged in | 1. Go to "Books" tab 2. Select category | Category: Kinh tế | List displays 3 Economics books: BOOK007, BOOK014, BOOK015 | REQ-03 | EP |
| TC-15 | Combine search and category filter | Logged in | 1. Go to "Books" tab 2. Select "Công nghệ" 3. Enter "Flutter" | Cat: Công nghệ; Kw: Flutter | Displays only "Lập trình Flutter cơ bản" (BOOK001) | REQ-03 | EP |
| TC-16 | Combine Flutter search and Economics filter | Logged in | 1. Go to "Books" tab 2. Select "Kinh tế" 3. Enter "Flutter" | Cat: Kinh tế; Kw: Flutter | Displays no books (none are both Economics and contain Flutter) | REQ-03 | EP |

### 2.4. REQ-04: Borrow Book

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-34 | Successful borrow at boundary = 0 active books | Logged in as MEM003 (no active records in seed) | 1. Go to "Books" tab 2. Select BOOK004 3. Click "Borrow" | MEM003; BOOK004 | Record created, dueDate = today + 14, MEM003 has 1 active book, BOOK004 becomes "Borrowed" | REQ-04 | BVA |
| TC-17 | Successful borrow at boundary = 1 active book | Logged in as MEM006 (1 active book: BOOK013) | 1. Go to "Books" tab 2. Select BOOK008 3. Click "Borrow" | MEM006; BOOK008 | Record created, dueDate = today + 14, MEM006 has 2 active books, BOOK008 becomes "Borrowed" | REQ-04 | EP, BVA |
| TC-23 | Successful borrow at boundary = 2 active books | Logged in as MEM002; **setup**: borrow BOOK008 so MEM002 has 2 active books | 1. Go to "Books" tab 2. Select BOOK009 3. Click "Borrow" | MEM002; BOOK009 | Record created, MEM002 has 3 active books, BOOK009 becomes "Borrowed" | REQ-04 | BVA |
| TC-21 | Deny when exceeding 3 books limit (boundary) | **Run right after TC-23 (no reset)**; MEM002 has 3 active books | 1. Go to "Books" tab 2. Select BOOK012 3. Click "Borrow" | MEM002; BOOK012 | Borrow denied, exceeds 3 books limit message | REQ-04 | BVA |
| TC-18 | Deny borrowing already borrowed book | Logged in as MEM002 (BOOK003 is "Borrowed" in seed) | 1. Go to "Books" tab 2. Select BOOK003 3. Click "Borrow" | MEM002; BOOK003 | Borrow denied, book is already borrowed (button disabled or error message) | REQ-04 | EP |
| TC-19 | Deny borrowing when member is suspended | Logged in as MEM004 (Status: Suspended) | 1. Go to "Books" tab 2. Select BOOK010 3. Click "Borrow" | MEM004; BOOK010 | Borrow denied, shows reason: member is suspended (not expired) | REQ-04 | EP |
| TC-20 | Deny borrowing when member is expired | Logged in as MEM005 (Status: Expired) | 1. Go to "Books" tab 2. Select BOOK011 3. Click "Borrow" | MEM005; BOOK011 | Borrow denied, shows reason: member is expired | REQ-04 | EP |
| TC-22 | Deny borrowing lost book | Logged in as MEM006 | 1. Go to "Books" tab 2. Select BOOK007 3. Click "Borrow" | MEM006; BOOK007 | Borrow denied, book is lost/unavailable | REQ-04 | EP |

### 2.5. REQ-05: Return Book

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-24 | Return book within time limit | Logged in as MEM003; **setup**: borrow BOOK002 (future dueDate) | 1. Go to "Borrow/Return" 2. Select BOOK002 3. Click "Return" | MEM003; BOOK002 | Record becomes "Returned", BOOK002 becomes "Available", NO overdue warning | REQ-05 | EP |
| TC-25 | Cannot return books belonging to others | Logged in as MEM003 (BOOK003 borrowed by MEM002) | 1. Go to "Borrow/Return" 2. Observe list | MEM003; BOOK003 | BOOK003 not in MEM003's list; no Return button for BOOK003 | REQ-05 | EP |
| TC-26 | Warning when returning overdue book | Logged in as MEM002; BR001 (dueDate 15/09/2024) is overdue | 1. Go to "Borrow/Return" 2. Select BR001 3. Click "Return" | MEM002; BOOK003 | System shows overdue warning; record becomes "Returned", BOOK003 becomes "Available" | REQ-05 | EP |

### 2.6. REQ-06: Overdue Handling

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-27 | Mark record as overdue after checking | Logged in as Librarian; seed state | 1. Click "Check Overdue" 2. Open borrow records | librarian@library.com | BR001 marked as "Overdue" | REQ-06 | EP |
| TC-28 | Member only sees their own overdue records | Overdue check completed (TC-27 done) | 1. Log in as MEM002 2. Go to "Borrow/Return" | MEM002 | MEM002 sees BR001 as "Overdue"; cannot see others' overdue records | REQ-06, REQ-08 | EP |

### 2.7. REQ-07: Member Management

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-29 | Add valid new member | Logged in as Librarian | 1. Go to "Members" 2. Click "Add" 3. Enter info 4. Save | Le Test; new.member@email.com | Member created successfully, appears in list | REQ-07 | EP |
| TC-30 | Deny invalid email (missing dot) | Logged in as Librarian | 1. Go to "Members" 2. Click "Add" 3. Enter info 4. Save | Le Sai; user@domain | Shows invalid email error | REQ-07 | EP |
| TC-31 | Deny duplicate email | Logged in as Librarian | 1. Go to "Members" 2. Click "Add" 3. Enter info 4. Save | Trung Lap; ba.nguyen@email.com | Shows duplicate email error | REQ-07 | EP |
| TC-32 | Deny email without @ | Logged in as Librarian | 1. Go to "Members" 2. Click "Add" 3. Enter info 4. Save | Le Sai; userdomain.com | Shows invalid email error | REQ-07 | EP |

### 2.8. REQ-08: View Borrow Records

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-33 | Correct record visibility per role | Logged in as Librarian, then MEM003 | 1. Librarian: open "Borrow/Return" 2. Verify all records 3. Logout 4. Login MEM003 5. Open "Borrow/Return" | librarian@library.com; dam.tran@email.com | Librarian sees all; MEM003 sees only their own (BR002, BR005) | REQ-08 | EP |

### 2.9. General

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-35 | Restore seed data | Logged in as Librarian, data modified | 1. Click "Restore Data" 2. Confirm | librarian@library.com | All data restored to initial state (seed data) | General | EP |

---

## Summary

| Functional Group | TC Count | Covered REQs | IDM Technique |
| --- | --- | --- | --- |
| Login | 6 | REQ-01 | EP |
| Book List | 3 | REQ-02 | EP |
| Search/Filter | 7 | REQ-03 | EP |
| Borrow Book | 8 | REQ-04 | EP, BVA, Decision Table |
| Return Book | 3 | REQ-05 | EP |
| Overdue Checking | 2 | REQ-06 | EP |
| Member Management | 4 | REQ-07 | EP |
| View Borrow Records | 1 | REQ-08 | EP |
| General | 1 | - | EP |
| **Total** | **35** | REQ-01 → REQ-08 | EP, BVA, Decision Table |

> **Note on BVA Execution for Borrow Limit (REQ-04)**:
> BVA for the 3-book limit is distributed across 4 TCs sequentially:
> - **TC-34**: MEM003 — 0 books → successful borrow (BVA at 0)
> - **TC-17**: MEM006 — 1 book → successful borrow (BVA at 1)
> - **TC-23**: MEM002 — 2 books → successful borrow (BVA at boundary−1 = 2)
> - **TC-21**: MEM002 — 3 books → borrow fails (BVA at boundary = 3); **run immediately after TC-23, no data reset between them**
