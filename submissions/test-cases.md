# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin      |                     |
| -------------- | ------------------- |
| **Nhóm**       | stqa_group_15       |
| **Ngày tạo**   | 24/05/2026          |
| **Hệ thống**   | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0            |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — _Input Domain Modeling_, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic)  | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi             |
| -------------------------- | ----------------- | ------------------------ | ---------------------------- |
| Email có tồn tại trong DB? | Có                | `librarian@library.com`  | Đăng nhập thành công         |
|                            | Không             | `noone@email.com`        | Thông báo lỗi                |
| Mật khẩu có đúng?          | Đúng              | `admin123`               | Đăng nhập thành công         |
|                            | Sai               | `wrongpass`              | Thông báo lỗi                |
| Ô nhập có rỗng?            | Không rỗng        | (giá trị bất kỳ)         | Xử lý bình thường            |
|                            | Rỗng              | `""`                     | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic)    | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi                 |
| ---------------------------- | ----------------- | ------------------------ | -------------------------------- |
| Từ khóa có tồn tại trong DB? | Có (tên sách)     | `"Flutter"`              | Hiển thị sách chứa "Flutter"     |
|                              | Có (tên tác giả)  | `"Nguyễn"`               | Hiển thị sách của tác giả Nguyễn |
|                              | Không             | `"XYZ123"`               | Danh sách rỗng                   |
| Phân biệt HOA/thường?        | Chữ thường        | `"flutter"`              | Kết quả giống "Flutter"          |
|                              | Chữ HOA           | `"FLUTTER"`              | Kết quả giống "Flutter"          |

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block)   | Giá trị đại diện (Value) | Kết quả mong đợi                 |
| ------------------------- | ------------------- | ------------------------ | -------------------------------- |
| Trạng thái sách?          | Có sẵn              | BOOK001                  | Cho phép mượn                    |
|                           | Đang mượn           | BOOK003                  | Không cho phép                   |
|                           | Thất lạc            | BOOK007                  | Không cho phép                   |
| Trạng thái thành viên?    | Hoạt động           | MEM002                   | Cho phép mượn                    |
|                           | Tạm ngưng           | MEM004                   | Từ chối, thông báo lỗi           |
|                           | Hết hạn             | MEM005                   | Từ chối, thông báo lỗi           |
| Số sách đang mượn?        | < 3 (BVA: 0, 1, 2)  | MEM006 (0 sách)          | Cho phép mượn                    |
|                           | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách       | Từ chối, thông báo vượt giới hạn |

### IDM — Trả sách (REQ-05)

| Đặc tính (Characteristic)          | Phân vùng (Block) | Giá trị đại diện (Value)   | Kết quả mong đợi                          |
| ---------------------------------- | ----------------- | -------------------------- | ----------------------------------------- |
| Sách có đang được thành viên mượn? | Có                | MEM002 đang mượn BOOK003   | Cho phép trả, trạng thái sách về "Có sẵn" |
|                                    | Không             | MEM003 cố trả BOOK003      | Từ chối trả, dữ liệu không đổi            |
| Trả quá hạn?                       | Có                | BR001 (dueDate 15/09/2024) | Cảnh báo quá hạn                          |
|                                    | Không             | Sách mượn trong hạn        | Không cảnh báo                            |

### IDM — Quá hạn (REQ-06)

| Đặc tính (Characteristic)           | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi                      |
| ----------------------------------- | ----------------- | ------------------------ | ------------------------------------- |
| Thủ thư kích hoạt kiểm tra quá hạn? | Có                | Nhấn "Kiểm tra quá hạn"  | Phiếu quá hạn được đánh dấu "Quá hạn" |
|                                     | Không             | Không nhấn               | Trạng thái không đổi                  |
| Người xem phiếu quá hạn             | Thủ thư           | librarian@library.com    | Xem tất cả phiếu quá hạn              |
|                                     | Thành viên        | ba.nguyen@email.com      | Chỉ thấy phiếu của mình nếu quá hạn   |

### IDM — Quản lý thành viên (REQ-07)

| Đặc tính (Characteristic) | Phân vùng (Block)          | Giá trị đại diện (Value) | Kết quả mong đợi          |
| ------------------------- | -------------------------- | ------------------------ | ------------------------- |
| Email hợp lệ?             | Hợp lệ                     | user@domain.com          | Cho phép tạo              |
|                           | Không hợp lệ (thiếu dấu .) | user@domain              | Thông báo lỗi email       |
| Email bị trùng?           | Không trùng                | new.member@email.com     | Tạo thành viên mới        |
|                           | Trùng                      | ba.nguyen@email.com      | Thông báo lỗi trùng email |

### IDM — Tra cứu phiếu mượn (REQ-08)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi             |
| ------------------------- | ----------------- | ------------------------ | ---------------------------- |
| Vai trò người dùng        | Thủ thư           | librarian@library.com    | Xem tất cả phiếu mượn        |
|                           | Thành viên        | dam.tran@email.com       | Chỉ xem phiếu của chính mình |

## Bước 1b: Decision Table — Mượn sách (REQ-04)

| Điều kiện                   | Rule 1        | Rule 2                          | Rule 3              | Rule 4            | Rule 5                  |
| --------------------------- | ------------- | ------------------------------- | ------------------- | ----------------- | ----------------------- |
| Sách ở trạng thái "Có sẵn"? | Y             | N                               | Y                   | Y                 | Y                       |
| Thành viên hoạt động?       | Y             | Y                               | N (Tạm ngưng)       | N (Hết hạn)       | Y                       |
| Số sách đang mượn < 3?      | Y             | Y                               | Y                   | Y                 | N                       |
| **Kết quả**                 | Cho phép mượn | Từ chối (sách đã mượn/thất lạc) | Từ chối (tạm ngưng) | Từ chối (hết hạn) | Từ chối (vượt giới hạn) |

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử                               | Tiền điều kiện                             | Bước thực hiện                                                                                                                            | Dữ liệu đầu vào                                                | Kết quả mong đợi                                                                                                  | REQ            | Kỹ thuật |
| ----- | ----------------------------------------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | -------------- | -------- |
| TC-01 | Đăng nhập thành công bằng tài khoản hợp lệ      | Ở trang đăng nhập                          | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                        | Email: librarian@library.com; MK: admin123                     | Chuyển sang trang chủ, AppBar hiển thị "Nguyễn Thủ Thư" và vai trò Thủ thư                                        | REQ-01         | EP       |
| TC-02 | Báo lỗi khi email không tồn tại                 | Ở trang đăng nhập                          | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                        | Email: nobody@test.com; MK: anything                           | Hiển thị thông báo "Không tìm thấy thành viên"                                                                    | REQ-01         | EP       |
| TC-03 | Báo lỗi khi mật khẩu sai                        | Ở trang đăng nhập                          | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                        | Email: ba.nguyen@email.com; MK: wrongpassword                  | Hiển thị thông báo "Mật khẩu không đúng"                                                                          | REQ-01         | EP       |
| TC-04 | Báo lỗi khi bỏ trống email và mật khẩu          | Ở trang đăng nhập                          | 1. Để trống email và mật khẩu 2. Nhấn "Đăng nhập"                                                                                         | Email: ""; MK: ""                                              | Hiển thị thông báo "Vui lòng nhập email và mật khẩu"                                                              | REQ-01         | EP       |
| TC-05 | Hiển thị đầy đủ thông tin sách theo seed data   | Đăng nhập thành công                       | 1. Mở tab "Sách" 2. Quan sát danh sách                                                                                                    | Không                                                          | Hiển thị tên sách, tác giả, thể loại, năm XB, trạng thái; BOOK003 hiển thị "Đã mượn", BOOK007 hiển thị "Thất lạc" | REQ-02         | EP       |
| TC-06 | Cập nhật trạng thái sách real-time sau khi mượn | Đăng nhập thành viên MEM006                | 1. Vào tab "Sách" 2. Chọn BOOK002 3. Nhấn "Mượn"                                                                                          | MEM006; BOOK002                                                | BOOK002 đổi trạng thái sang "Đã mượn" ngay sau khi mượn                                                           | REQ-02, REQ-04 | EP       |
| TC-07 | Tìm kiếm theo tên sách                          | Đăng nhập thành công                       | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: Flutter                                               | Danh sách chỉ hiển thị "Lập trình Flutter cơ bản"                                                                 | REQ-03         | EP       |
| TC-08 | Tìm kiếm theo tác giả                           | Đăng nhập thành công                       | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: Nguyễn Minh Đức                                       | Hiển thị các sách của Nguyễn Minh Đức (BOOK001, BOOK009)                                                          | REQ-03         | EP       |
| TC-09 | Tìm kiếm không phân biệt hoa/thường             | Đăng nhập thành công                       | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: flutter                                               | Kết quả giống khi tìm "Flutter"                                                                                   | REQ-03         | EP       |
| TC-10 | Thông báo khi không có kết quả tìm kiếm         | Đăng nhập thành công                       | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: XYZ123                                                | Hiển thị thông báo "Không tìm thấy sách"                                                                          | REQ-03         | EP       |
| TC-11 | Lọc theo thể loại                               | Đăng nhập thành công                       | 1. Vào tab "Sách" 2. Chọn thể loại                                                                                                        | Thể loại: Kinh tế                                              | Danh sách chỉ hiển thị sách thể loại Kinh tế                                                                      | REQ-03         | EP       |
| TC-12 | Mượn sách khi đủ điều kiện                      | Đăng nhập MEM006, chưa mượn sách           | 1. Vào tab "Sách" 2. Chọn BOOK008 3. Nhấn "Mượn"                                                                                          | MEM006; BOOK008                                                | Tạo phiếu mượn mới, dueDate = ngày mượn + 14 ngày, trạng thái phiếu "Đang mượn", BOOK008 chuyển "Đã mượn"         | REQ-04         | EP, BVA  |
| TC-13 | Từ chối mượn sách đã được mượn                  | Đăng nhập MEM002                           | 1. Vào tab "Sách" 2. Chọn BOOK003 3. Nhấn "Mượn"                                                                                          | MEM002; BOOK003                                                | Từ chối mượn, thông báo lý do sách đã được mượn                                                                   | REQ-04         | EP       |
| TC-14 | Từ chối mượn khi thành viên tạm ngưng           | Đăng nhập MEM004                           | 1. Vào tab "Sách" 2. Chọn BOOK010 3. Nhấn "Mượn"                                                                                          | MEM004; BOOK010                                                | Từ chối mượn, thông báo thành viên bị tạm ngưng                                                                   | REQ-04         | EP       |
| TC-15 | Từ chối mượn khi thành viên hết hạn             | Đăng nhập MEM005                           | 1. Vào tab "Sách" 2. Chọn BOOK011 3. Nhấn "Mượn"                                                                                          | MEM005; BOOK011                                                | Từ chối mượn, thông báo thành viên hết hạn                                                                        | REQ-04         | EP       |
| TC-16 | Từ chối khi vượt giới hạn 3 sách (biên)         | MEM002 đang mượn 3 sách                    | 1. Vào tab "Sách" 2. Chọn BOOK012 3. Nhấn "Mượn"                                                                                          | MEM002; BOOK012                                                | Từ chối mượn, thông báo vượt giới hạn 3 sách                                                                      | REQ-04         | BVA      |
| TC-17 | Trả sách đang mượn                              | Đăng nhập MEM002, đang mượn BOOK003        | 1. Vào tab "Mượn/Trả" 2. Chọn BOOK003 3. Nhấn "Trả"                                                                                       | MEM002; BOOK003                                                | Phiếu mượn chuyển "Đã trả", BOOK003 chuyển "Có sẵn"                                                               | REQ-05         | EP       |
| TC-18 | Không cho trả sách không mượn                   | Đăng nhập MEM003                           | 1. Vào tab "Mượn/Trả" 2. Chọn BOOK003 3. Nhấn "Trả"                                                                                       | MEM003; BOOK003                                                | Hệ thống từ chối trả, trạng thái sách và phiếu không đổi                                                          | REQ-05         | EP       |
| TC-19 | Cảnh báo khi trả sách quá hạn                   | Đăng nhập MEM002, BR001 quá hạn            | 1. Vào tab "Mượn/Trả" 2. Chọn BOOK003 3. Nhấn "Trả"                                                                                       | MEM002; BOOK003                                                | Hiển thị cảnh báo quá hạn khi trả sách                                                                            | REQ-05         | EP       |
| TC-20 | Đánh dấu phiếu quá hạn sau khi kiểm tra         | Đăng nhập Thủ thư                          | 1. Nhấn "Kiểm tra quá hạn" 2. Mở danh sách phiếu mượn                                                                                     | librarian@library.com                                          | BR001 được đánh dấu trạng thái "Quá hạn"                                                                          | REQ-06         | EP       |
| TC-21 | Thành viên chỉ thấy phiếu quá hạn của mình      | Đã chạy "Kiểm tra quá hạn"                 | 1. Đăng nhập MEM002 2. Vào tab "Mượn/Trả"                                                                                                 | MEM002                                                         | MEM002 thấy phiếu của mình nếu quá hạn, không thấy phiếu quá hạn của người khác                                   | REQ-06, REQ-08 | EP       |
| TC-22 | Thêm thành viên mới hợp lệ                      | Đăng nhập Thủ thư                          | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Le Test; Email: new.member@email.com; SĐT: 0900000000  | Thêm thành viên mới thành công, xuất hiện trong danh sách                                                         | REQ-07         | EP       |
| TC-23 | Từ chối email không hợp lệ (thiếu dấu .)        | Đăng nhập Thủ thư                          | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Le Sai; Email: user@domain; SĐT: 0900000001            | Thông báo lỗi email không hợp lệ                                                                                  | REQ-07         | EP       |
| TC-24 | Từ chối email trùng                             | Đăng nhập Thủ thư                          | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Trung Lap; Email: ba.nguyen@email.com; SĐT: 0900000002 | Thông báo lỗi email đã tồn tại                                                                                    | REQ-07         | EP       |
| TC-25 | Tra cứu phiếu mượn đúng quyền                   | Đăng nhập Thủ thư, sau đó đăng nhập MEM003 | 1. Đăng nhập Thủ thư, mở tab "Mượn/Trả" 2. Xác nhận thấy phiếu của nhiều thành viên 3. Đăng xuất 4. Đăng nhập MEM003 5. Mở tab "Mượn/Trả" | librarian@library.com; dam.tran@email.com                      | Thủ thư thấy tất cả phiếu, MEM003 chỉ thấy phiếu của mình                                                         | REQ-08         | EP       |

---

## Tổng hợp

| Nhóm chức năng     | Số TC  | REQ phủ         | Kỹ thuật IDM áp dụng    |
| ------------------ | ------ | --------------- | ----------------------- |
| Đăng nhập          | 4      | REQ-01          | EP                      |
| Danh sách sách     | 2      | REQ-02          | EP                      |
| Tìm kiếm/lọc sách  | 5      | REQ-03          | EP                      |
| Mượn sách          | 5      | REQ-04          | EP, BVA, Decision Table |
| Trả sách           | 3      | REQ-05          | EP                      |
| Quá hạn            | 2      | REQ-06          | EP                      |
| Thành viên         | 3      | REQ-07          | EP                      |
| Tra cứu phiếu mượn | 1      | REQ-08          | EP                      |
| **Tổng**           | **25** | REQ-01 → REQ-08 | EP, BVA, Decision Table |
