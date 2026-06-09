# Test Execution

> **Instructions**: Execute each TC on the system https://stqa.rbc.vn, and record the actual results.
> Status: **Pass** (correct result), **Fail** (incorrect result → create bug report), **Blocked** (unable to execute due to other blocking bugs), **Not Run**.

| Information         |                        |
| ------------------- | ---------------------- |
| **Team**            | stqa_group_15          |
| **Execution Date**  | 06/06/2026             |
| **Browser**         | Chrome                 |
| **OS**              | Linux                  |

---

## Detailed Results

| TC ID | Functional Group   | Expected Result (Summary)                         | Actual Result                                            | Status | Evidence   | Bug    |
| ----- | ------------------ | ------------------------------------------------- | -------------------------------------------------------- | ------ | ---------- | ------ |
| TC-01 | Login              | Login successful, shows name + role               | As expected                                              | Pass   | -          | -      |
| TC-02 | Login              | Shows error "Member not found"                    | As expected                                              | Pass   | -          | -      |
| TC-03 | Login              | Shows error "Incorrect password"                  | As expected                                              | Pass   | -          | -      |
| TC-04 | Login              | Shows error "Please enter email and password"     | As expected                                              | Pass   | -          | -      |
| TC-05 | Login              | Shows error when only email is empty              | As expected                                              | Pass   | -          | -      |
| TC-06 | Login              | Shows error when only password is empty           | As expected                                              | Pass   | -          | -      |
| TC-07 | Book List          | Full info displayed + matches seed status         | BOOK003 shows status "Borrowing" instead of "Borrowed"   | Fail   | BUG01.png  | BUG-01 |
| TC-08 | Book List          | Book status updates after borrowing               | As expected                                              | Pass   | -          | -      |
| TC-09 | Book List          | Book status real-time update after returning      | As expected                                              | Pass   | -          | -      |
| TC-10 | Search/Filter      | Search by book name                               | As expected                                              | Pass   | -          | -      |
| TC-11 | Search/Filter      | Search by author                                  | As expected                                              | Pass   | -          | -      |
| TC-12 | Search/Filter      | Case-insensitive search                           | As expected                                              | Pass   | -          | -      |
| TC-13 | Search/Filter      | Message "No books found"                          | As expected                                              | Pass   | -          | -      |
| TC-14 | Search/Filter      | Filter by category                                | As expected                                              | Pass   | -          | -      |
| TC-15 | Search/Filter      | Combine search with category filter               | As expected                                              | Pass   | -          | -      |
| TC-16 | Search/Filter      | Combine Flutter + Economics returns no result     | Displays books in Economics category or named Flutter    | Fail   | BUG02.png  | BUG-02 |
| TC-17 | Borrow Book        | Borrow successful, record created + status updated| As expected                                              | Pass   | -          | -      |
| TC-18 | Borrow Book        | Deny borrowing already borrowed books             | As expected                                              | Pass   | -          | -      |
| TC-19 | Borrow Book        | Deny suspended member                             | Shows "Member has expired" instead of "Suspended"        | Fail   | BUG04.png  | BUG-04 |
| TC-20 | Borrow Book        | Deny expired member                               | As expected                                              | Pass   | -          | -      |
| TC-21 | Borrow Book        | Deny borrowing beyond limit of 3 books            | System allows borrowing the 4th book                     | Fail   | BUG03.png  | BUG-03 |
| TC-22 | Borrow Book        | Deny borrowing lost books                         | As expected                                              | Pass   | -          | -      |
| TC-23 | Borrow Book        | Borrow at boundary = 2 active books works (BVA)   | As expected                                              | Pass   | -          | -      |
| TC-24 | Return Book        | Return successful, status updated                 | As expected                                              | Pass   | -          | -      |
| TC-25 | Return Book        | Deny returning a book not borrowed                | As expected                                              | Pass   | -          | -      |
| TC-26 | Return Book        | Warning when returning overdue book               | Return successful but no warning is displayed            | Fail   | BUG06.png  | BUG-06 |
| TC-27 | Overdue Checking   | Mark record as overdue                            | Record due today not marked; 2nd scan gives wrong count  | Fail   | BUG07.png  | BUG-07, BUG-08 |
| TC-28 | Overdue Checking   | Member only sees their own overdue records        | As expected                                              | Pass   | -          | -      |
| TC-29 | Member Management  | Add valid member                                  | Shows "Invalid email" error due to reversed regex        | Fail   | BUG10.png  | BUG-10 |
| TC-30 | Member Management  | Deny invalid email                                | Accepts email without a dot, creates successfully        | Fail   | BUG09.png  | BUG-09 |
| TC-31 | Member Management  | Deny duplicate email                              | Shows "Invalid email" error (side effect of BUG-10)      | Fail   | BUG10.png  | BUG-10 |
| TC-32 | Member Management  | Deny email without @                              | As expected                                              | Pass   | -          | -      |
| TC-33 | View Borrow Record | Librarian sees all, Member sees only their own    | Member can see and return records of other members       | Fail   | BUG11.png  | BUG-11 |
| TC-34 | Borrow Book        | Borrow successful at boundary = 0 active books    | As expected                                              | Pass   | -          | -      |
| TC-35 | General            | Restore to seed data                              | As expected                                              | Pass   | -          | -      |

*Note: The missing "Borrow" functionality for the Librarian on the Books tab does not map 1:1 to a specific TC, so it is recorded as BUG-05 (Additional Discovery) in the Bug Reports.*

---

## Result Summary

| Metric                 | Value   |
| ---------------------- | ------- |
| Total Test Cases       | 35      |
| Pass                   | 25      |
| Fail                   | 10      |
| Blocked                | 0       |
| Not Run                | 0       |
| **Pass Rate**          | 71.4%   |

### Results by Functional Group

| Group                  | Total TC | Pass | Fail | Pass Rate  |
| ---------------------- | -------- | ---- | ---- | ---------- |
| Login                  | 6        | 6    | 0    | 100%       |
| Book List              | 3        | 2    | 1    | 66.7%      |
| Search/Filter          | 7        | 6    | 1    | 85.7%      |
| Borrow Book            | 8        | 6    | 2    | 75.0%      |
| Return Book            | 3        | 2    | 1    | 66.7%      |
| Overdue Checking       | 2        | 1    | 1    | 50.0%      |
| Member Management      | 4        | 1    | 3    | 25.0%      |
| View Borrow Record     | 1        | 0    | 1    | 0%         |
| General                | 1        | 1    | 0    | 100%       |
