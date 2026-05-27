# Test Execution — Kết quả thực thi kiểm thử

> **Hướng dẫn**: Chạy từng TC trên hệ thống https://stqa.rbc.vn, ghi lại kết quả thực tế.
> Kết luận: **Pass** (kết quả đúng), **Fail** (kết quả sai → tạo bug report), **Blocked** (không thực hiện được vì lỗi khác chặn), **Not Run** (chưa chạy).

| Thông tin         |                        |
| ----------------- | ---------------------- |
| **Nhóm**          | stqa_group_15          |
| **Ngày thực thi** | 24/05/2026             |
| **Trình duyệt**   | Chrome (chua xac dinh) |
| **Hệ điều hành**  | Linux                  |

---

## Kết quả chi tiết

| Mã TC | Nhóm chức năng     | Kết quả mong đợi (tóm tắt)                        | Kết quả thực tế                                          | Kết luận | Minh chứng | Bug    |
| ----- | ------------------ | ------------------------------------------------- | -------------------------------------------------------- | -------- | ---------- | ------ |
| TC-01 | Đăng nhập          | Đăng nhập thành công, hiển thị tên + vai trò      | -                                                        | Not Run  | -          | -      |
| TC-02 | Đăng nhập          | Báo lỗi "Không tìm thấy thành viên"               | -                                                        | Not Run  | -          | -      |
| TC-03 | Đăng nhập          | Báo lỗi "Mật khẩu không đúng"                     | -                                                        | Not Run  | -          | -      |
| TC-04 | Đăng nhập          | Báo lỗi "Vui lòng nhập email và mật khẩu"         | -                                                        | Not Run  | -          | -      |
| TC-05 | Đăng nhập          | Báo lỗi khi chỉ bỏ trống email                    | -                                                        | Not Run  | -          | -      |
| TC-06 | Đăng nhập          | Báo lỗi khi chỉ bỏ trống mật khẩu                 | -                                                        | Not Run  | -          | -      |
| TC-07 | Danh sách sách     | Hiển thị đủ thông tin + trạng thái seed           | -                                                        | Not Run  | -          | -      |
| TC-08 | Danh sách sách     | Trạng thái sách cập nhật sau mượn                 | -                                                        | Not Run  | -          | -      |
| TC-09 | Danh sách sách     | Trạng thái sách real-time sau khi trả             | -                                                        | Not Run  | -          | -      |
| TC-10 | Tìm kiếm/lọc       | Tìm theo tên sách                                 | -                                                        | Not Run  | -          | -      |
| TC-11 | Tìm kiếm/lọc       | Tìm theo tác giả                                  | -                                                        | Not Run  | -          | -      |
| TC-12 | Tìm kiếm/lọc       | Case-insensitive                                  | -                                                        | Not Run  | -          | -      |
| TC-13 | Tìm kiếm/lọc       | Thông báo "Không tìm thấy sách"                   | -                                                        | Not Run  | -          | -      |
| TC-14 | Tìm kiếm/lọc       | Lọc theo thể loại                                 | -                                                        | Not Run  | -          | -      |
| TC-15 | Tìm kiếm/lọc       | Tìm kiếm kết hợp lọc thể loại                     | -                                                        | Not Run  | -          | -      |
| TC-16 | Tìm kiếm/lọc       | Kết hợp Flutter + Kinh tế không có kết quả        | Hiển thị sách thuộc thể loại Kinh tế hoặc có tên Flutter | Fail     | -          | BUG-02 |
| TC-17 | Mượn sách          | Mượn thành công, tạo phiếu + đổi trạng thái       | -                                                        | Not Run  | -          | -      |
| TC-18 | Mượn sách          | Từ chối sách đã mượn                              | -                                                        | Not Run  | -          | -      |
| TC-19 | Mượn sách          | Từ chối thành viên tạm ngưng                      | -                                                        | Not Run  | -          | -      |
| TC-20 | Mượn sách          | Từ chối thành viên hết hạn                        | -                                                        | Not Run  | -          | -      |
| TC-21 | Mượn sách          | Từ chối vượt giới hạn 3 sách                      | -                                                        | Not Run  | -          | -      |
| TC-22 | Mượn sách          | Từ chối mượn sách thất lạc                        | -                                                        | Not Run  | -          | -      |
| TC-23 | Mượn sách          | Mượn tại biên = 2 sách vẫn OK (BVA)               | -                                                        | Not Run  | -          | -      |
| TC-24 | Trả sách           | Trả thành công, đổi trạng thái                    | -                                                        | Not Run  | -          | -      |
| TC-25 | Trả sách           | Từ chối trả sách không mượn                       | -                                                        | Not Run  | -          | -      |
| TC-26 | Trả sách           | Cảnh báo trả quá hạn                              | -                                                        | Not Run  | -          | -      |
| TC-27 | Quá hạn            | Đánh dấu phiếu quá hạn                            | -                                                        | Not Run  | -          | -      |
| TC-28 | Quá hạn            | Thành viên chỉ thấy phiếu quá hạn của mình        | -                                                        | Not Run  | -          | -      |
| TC-29 | Thành viên         | Thêm thành viên hợp lệ                            | -                                                        | Not Run  | -          | -      |
| TC-30 | Thành viên         | Từ chối email không hợp lệ                        | -                                                        | Not Run  | -          | -      |
| TC-31 | Thành viên         | Từ chối email trùng                               | -                                                        | Not Run  | -          | -      |
| TC-32 | Thành viên         | Từ chối email không có @                          | -                                                        | Not Run  | -          | -      |
| TC-33 | Tra cứu phiếu mượn | Thủ thư thấy tất cả, thành viên chỉ thấy của mình | -                                                        | Not Run  | -          | -      |
| TC-34 | Tra cứu phiếu mượn | Thành viên không xem được phiếu người khác        | -                                                        | Not Run  | -          | -      |
| TC-35 | Tổng quát          | Khôi phục dữ liệu về seed data                    | -                                                        | Not Run  | -          | -      |

---

## Tổng hợp kết quả

| Chỉ số            | Giá trị |
| ----------------- | ------- |
| Tổng số test case | 35      |
| Pass              | 0       |
| Fail              | 1       |
| Blocked           | 0       |
| Not Run           | 34      |
| **Tỷ lệ Pass**    | 0%      |

### Kết quả theo nhóm chức năng

| Nhóm               | Tổng TC | Pass | Fail | Tỷ lệ Pass |
| ------------------ | ------- | ---- | ---- | ---------- |
| Đăng nhập          | 6       | 0    | 0    | 0%         |
| Danh sách sách     | 3       | 0    | 0    | 0%         |
| Tìm kiếm/lọc       | 7       | 0    | 1    | 0%         |
| Mượn sách          | 7       | 0    | 0    | 0%         |
| Trả sách           | 3       | 0    | 0    | 0%         |
| Quá hạn            | 2       | 0    | 0    | 0%         |
| Thành viên         | 4       | 0    | 0    | 0%         |
| Tra cứu phiếu mượn | 2       | 0    | 0    | 0%         |
| Tổng quát          | 1       | 0    | 0    | 0%         |
