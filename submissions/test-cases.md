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

| Đặc tính (Characteristic)  | Phân vùng (Block) | Giá trị đại diện (Value)                 | Kết quả mong đợi             |
| -------------------------- | ----------------- | ---------------------------------------- | ---------------------------- |
| Email có tồn tại trong DB? | Có                | `librarian@library.com`                  | Đăng nhập thành công         |
|                            | Không             | `noone@email.com`                        | Thông báo lỗi                |
| Mật khẩu có đúng?          | Đúng              | `admin123`                               | Đăng nhập thành công         |
|                            | Sai               | `wrongpass`                              | Thông báo lỗi                |
| Ô nhập có rỗng?            | Không rỗng        | (giá trị bất kỳ)                         | Xử lý bình thường            |
|                            | Cả hai rỗng       | `""`                                     | Thông báo "Vui lòng nhập..." |
|                            | Chỉ email rỗng    | Email: `""`, MK: `admin123`              | Thông báo "Vui lòng nhập..." |
|                            | Chỉ MK rỗng       | Email: `librarian@library.com`, MK: `""` | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic)    | Phân vùng (Block) | Giá trị đại diện (Value)        | Kết quả mong đợi                 |
| ---------------------------- | ----------------- | ------------------------------- | -------------------------------- |
| Từ khóa có tồn tại trong DB? | Có (tên sách)     | `"Flutter"`                     | Hiển thị sách chứa "Flutter"     |
|                              | Có (tên tác giả)  | `"Nguyễn"`                      | Hiển thị sách của tác giả Nguyễn |
|                              | Không             | `"XYZ123"`                      | Danh sách rỗng                   |
| Phân biệt HOA/thường?        | Chữ thường        | `"flutter"`                     | Kết quả giống "Flutter"          |
|                              | Chữ HOA           | `"FLUTTER"`                     | Kết quả giống "Flutter"          |
| Kết hợp tìm kiếm + lọc?      | Có                | Lọc "Công nghệ" + tìm "Flutter" | Chỉ hiển thị sách khớp cả hai    |
|                              | Không             | Chỉ tìm hoặc chỉ lọc            | Kết quả theo 1 tiêu chí          |

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block)        | Giá trị đại diện (Value)                              | Kết quả mong đợi                 |
| ------------------------- | ------------------------ | ----------------------------------------------------- | -------------------------------- |
| Trạng thái sách?          | Có sẵn                   | BOOK001                                               | Cho phép mượn                    |
|                           | Đang mượn                | BOOK003                                               | Không cho phép                   |
|                           | Thất lạc                 | BOOK007                                               | Không cho phép                   |
| Trạng thái thành viên?    | Hoạt động                | MEM002                                                | Cho phép mượn                    |
|                           | Tạm ngưng                | MEM004                                                | Từ chối, thông báo lỗi           |
|                           | Hết hạn                  | MEM005                                                | Từ chối, thông báo lỗi           |
| Số sách đang mượn?        | BVA: 0 (dưới giới hạn)   | MEM003 (0 sách đang mượn trong seed data)             | Cho phép mượn                    |
|                           | BVA: 1 (dưới giới hạn)   | MEM006 (1 sách: BOOK013)                              | Cho phép mượn                    |
|                           | BVA: 2 (dưới giới hạn)   | MEM002 (2 sách: BOOK003 + BOOK008 sau khi setup)      | Cho phép mượn                    |
|                           | BVA: 3 (đúng giới hạn)   | MEM002 (3 sách: BOOK003 + BOOK008 + BOOK009 sau TC-23) | Từ chối, thông báo vượt giới hạn |

### IDM — Trả sách (REQ-05)

| Đặc tính (Characteristic)          | Phân vùng (Block) | Giá trị đại diện (Value)               | Kết quả mong đợi                          |
| ---------------------------------- | ----------------- | -------------------------------------- | ----------------------------------------- |
| Sách có đang được thành viên mượn? | Có                | MEM002 đang mượn BOOK003               | Cho phép trả, trạng thái sách về "Có sẵn" |
|                                    | Không             | MEM003 không có phiếu mượn BOOK003     | Không có tùy chọn trả BOOK003             |
| Trả quá hạn?                       | Có                | BR001 (dueDate 15/09/2024)             | Cảnh báo quá hạn                          |
|                                    | Không             | Phiếu mượn mới trong hạn (MEM003 mượn BOOK002 trong phiên hiện tại) | Không cảnh báo |

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
|                           | Không hợp lệ (thiếu @)     | userdomain.com           | Thông báo lỗi email       |
| Email bị trùng?           | Không trùng                | new.member@email.com     | Tạo thành viên mới        |
|                           | Trùng                      | ba.nguyen@email.com      | Thông báo lỗi trùng email |

### IDM — Tra cứu phiếu mượn (REQ-08)

| Đặc tính (Characteristic) | Phân vùng (Block)   | Giá trị đại diện (Value)    | Kết quả mong đợi                        |
| ------------------------- | ------------------- | --------------------------- | --------------------------------------- |
| Vai trò người dùng        | Thủ thư             | librarian@library.com       | Xem tất cả phiếu mượn                   |
|                           | Thành viên          | dam.tran@email.com          | Chỉ xem phiếu của chính mình            |
| Trạng thái phiếu          | Đang mượn           | BR001 (MEM002)              | Hiển thị trong danh sách của thành viên |
|                           | Đã trả              | BR002, BR005 (MEM003)       | Vẫn hiển thị trong lịch sử của MEM003   |

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

| Mã TC | Mục tiêu kiểm thử                               | Tiền điều kiện                                                                                                                                  | Bước thực hiện                                                                                                                            | Dữ liệu đầu vào                                                | Kết quả mong đợi                                                                                                                          | REQ            | Kỹ thuật |
| ----- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------- | -------- |
| TC-01 | Đăng nhập thành công bằng tài khoản hợp lệ      | Ở trang đăng nhập                                                                                                                               | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                        | Email: librarian@library.com; MK: admin123                     | Chuyển sang trang chủ, AppBar hiển thị "Nguyễn Thủ Thư" và vai trò Thủ thư                                                                | REQ-01         | EP       |
| TC-02 | Báo lỗi khi email không tồn tại                 | Ở trang đăng nhập                                                                                                                               | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                        | Email: nobody@test.com; MK: anything                           | Hiển thị thông báo "Không tìm thấy thành viên"                                                                                            | REQ-01         | EP       |
| TC-03 | Báo lỗi khi mật khẩu sai                        | Ở trang đăng nhập                                                                                                                               | 1. Nhập email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                        | Email: ba.nguyen@email.com; MK: wrongpassword                  | Hiển thị thông báo "Mật khẩu không đúng"                                                                                                  | REQ-01         | EP       |
| TC-04 | Báo lỗi khi bỏ trống email và mật khẩu          | Ở trang đăng nhập                                                                                                                               | 1. Để trống email và mật khẩu 2. Nhấn "Đăng nhập"                                                                                         | Email: ""; MK: ""                                              | Hiển thị thông báo "Vui lòng nhập email và mật khẩu"                                                                                      | REQ-01         | EP       |
| TC-05 | Báo lỗi khi chỉ bỏ trống email                  | Ở trang đăng nhập                                                                                                                               | 1. Để trống email 2. Nhập mật khẩu 3. Nhấn "Đăng nhập"                                                                                    | Email: ""; MK: admin123                                        | Hiển thị thông báo "Vui lòng nhập email và mật khẩu"                                                                                      | REQ-01         | EP       |
| TC-06 | Báo lỗi khi chỉ bỏ trống mật khẩu               | Ở trang đăng nhập                                                                                                                               | 1. Nhập email 2. Để trống mật khẩu 3. Nhấn "Đăng nhập"                                                                                    | Email: librarian@library.com; MK: ""                           | Hiển thị thông báo "Vui lòng nhập email và mật khẩu"                                                                                      | REQ-01         | EP       |
| TC-07 | Hiển thị đầy đủ thông tin sách theo seed data   | Đăng nhập thành công                                                                                                                            | 1. Mở tab "Sách" 2. Quan sát danh sách                                                                                                    | Không                                                          | Hiển thị tên sách, tác giả, thể loại, năm XB, trạng thái; BOOK003 và BOOK013 hiển thị "Đã mượn"; BOOK007 và BOOK020 hiển thị "Thất lạc"   | REQ-02         | EP       |
| TC-08 | Cập nhật trạng thái sách real-time sau khi mượn | Đăng nhập thành viên MEM006 (đang mượn BOOK013); dữ liệu ở trạng thái seed                                                                      | 1. Vào tab "Sách" 2. Chọn BOOK002 3. Nhấn "Mượn"                                                                                          | MEM006; BOOK002                                                | BOOK002 đổi trạng thái sang "Đã mượn" ngay sau khi mượn                                                                                   | REQ-02, REQ-04 | EP       |
| TC-09 | Trạng thái sách real-time sau khi trả           | Đăng nhập MEM002 (đang mượn BOOK003 — BR001 theo seed data; BR001 đã quá hạn)                                                                   | 1. Vào tab "Mượn/Trả" 2. Chọn phiếu BR001 (BOOK003) 3. Nhấn "Trả" 4. Vào tab "Sách"                                                      | MEM002; BOOK003                                                | Hệ thống hiển thị cảnh báo quá hạn (BR001 quá hạn); BOOK003 chuyển về trạng thái "Có sẵn" ngay sau khi trả                                | REQ-02, REQ-05 | EP       |
| TC-10 | Tìm kiếm theo tên sách                          | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: Flutter                                               | Danh sách chỉ hiển thị "Lập trình Flutter cơ bản" (BOOK001)                                                                               | REQ-03         | EP       |
| TC-11 | Tìm kiếm theo tác giả                           | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: Nguyễn Minh Đức                                       | Hiển thị các sách của Nguyễn Minh Đức: BOOK001 và BOOK009                                                                                 | REQ-03         | EP       |
| TC-12 | Tìm kiếm không phân biệt hoa/thường             | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: flutter                                               | Kết quả giống khi tìm "Flutter" (BOOK001)                                                                                                 | REQ-03         | EP       |
| TC-13 | Thông báo khi không có kết quả tìm kiếm         | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Nhập từ khóa                                                                                                         | Từ khóa: XYZ123                                                | Hiển thị thông báo "Không tìm thấy sách"                                                                                                  | REQ-03         | EP       |
| TC-14 | Lọc theo thể loại                               | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Chọn thể loại                                                                                                        | Thể loại: Kinh tế                                              | Danh sách chỉ hiển thị 3 sách thể loại Kinh tế: BOOK007 (Thất lạc), BOOK014, BOOK015                                                     | REQ-03         | EP       |
| TC-15 | Tìm kiếm kết hợp lọc thể loại                   | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Chọn thể loại "Công nghệ" 3. Nhập từ khóa "Flutter"                                                                  | Thể loại: Công nghệ; Từ khóa: Flutter                          | Chỉ hiển thị "Lập trình Flutter cơ bản" (BOOK001)                                                                                         | REQ-03         | EP       |
| TC-16 | Kết hợp tìm kiếm Flutter và lọc Kinh tế         | Đăng nhập thành công                                                                                                                            | 1. Vào tab "Sách" 2. Chọn thể loại "Kinh tế" 3. Nhập từ khóa "Flutter"                                                                    | Thể loại: Kinh tế; Từ khóa: Flutter                            | Không hiển thị sách nào (không có sách vừa là Kinh tế vừa chứa "Flutter")                                                                 | REQ-03         | EP       |
| TC-17 | Mượn sách thành công tại biên = 1 sách đang mượn (BVA) | Đăng nhập MEM006 (đang mượn 1 sách: BOOK013 — BR003 theo seed data)                                                                      | 1. Vào tab "Sách" 2. Chọn BOOK008 3. Nhấn "Mượn"                                                                                          | MEM006; BOOK008                                                | Tạo phiếu mượn mới, dueDate = ngày mượn + 14 ngày, MEM006 có tổng 2 sách đang mượn, BOOK008 chuyển "Đã mượn"                              | REQ-04         | EP, BVA  |
| TC-18 | Từ chối mượn sách đã được mượn                  | Đăng nhập MEM002 (BOOK003 đang ở trạng thái "Đã mượn" trong seed data)                                                                         | 1. Vào tab "Sách" 2. Chọn BOOK003 3. Nhấn "Mượn"                                                                                          | MEM002; BOOK003                                                | Từ chối mượn, thông báo lý do sách đã được mượn (nút "Mượn" vô hiệu hóa hoặc báo lỗi)                                                    | REQ-04         | EP       |
| TC-19 | Từ chối mượn khi thành viên tạm ngưng           | Đăng nhập MEM004 (trạng thái: Tạm ngưng)                                                                                                        | 1. Vào tab "Sách" 2. Chọn BOOK010 3. Nhấn "Mượn"                                                                                          | MEM004; BOOK010                                                | Từ chối mượn, thông báo **đúng lý do**: thành viên bị **tạm ngưng** (không phải "hết hạn")                                                | REQ-04         | EP       |
| TC-20 | Từ chối mượn khi thành viên hết hạn             | Đăng nhập MEM005 (trạng thái: Hết hạn)                                                                                                          | 1. Vào tab "Sách" 2. Chọn BOOK011 3. Nhấn "Mượn"                                                                                          | MEM005; BOOK011                                                | Từ chối mượn, thông báo **đúng lý do**: thành viên **hết hạn** (không phải "tạm ngưng")                                                   | REQ-04         | EP       |
| TC-22 | Từ chối mượn sách thất lạc                      | Đăng nhập MEM006                                                                                                                                | 1. Vào tab "Sách" 2. Chọn BOOK007 3. Nhấn "Mượn"                                                                                          | MEM006; BOOK007                                                | Từ chối mượn, thông báo sách không có sẵn (thất lạc)                                                                                      | REQ-04         | EP       |
| TC-23 | Mượn sách thành công tại biên = 2 sách đang mượn (BVA) | Đăng nhập MEM002 (đang mượn BOOK003 từ seed data); **setup trước**: mượn thêm BOOK008 để MEM002 có tổng 2 sách đang mượn (BOOK003 + BOOK008) | 1. Vào tab "Sách" 2. Chọn BOOK009 3. Nhấn "Mượn"                                                                                          | MEM002; BOOK009                                                | Mượn thành công, tạo phiếu mượn, MEM002 có tổng 3 sách đang mượn, BOOK009 chuyển "Đã mượn"                                               | REQ-04         | BVA      |
| TC-21 | Từ chối khi vượt giới hạn 3 sách (biên)         | **Chạy ngay sau TC-23 trong cùng phiên (không reset)**; MEM002 đang mượn 3 sách: BOOK003, BOOK008, BOOK009                                      | 1. Vào tab "Sách" 2. Chọn BOOK012 3. Nhấn "Mượn"                                                                                          | MEM002; BOOK012                                                | Từ chối mượn, thông báo vượt giới hạn 3 sách                                                                                              | REQ-04         | BVA      |
| TC-24 | Trả sách trong hạn thành công (không quá hạn)   | Đăng nhập MEM003 (không có phiếu mượn đang hoạt động trong seed data); **setup trước**: mượn BOOK002 để tạo phiếu mới với dueDate trong tương lai | 1. Vào tab "Mượn/Trả" 2. Chọn phiếu mượn BOOK002 3. Nhấn "Trả"                                                                            | MEM003; BOOK002 (phiếu mượn mới, chưa quá hạn)                 | Phiếu mượn BOOK002 chuyển "Đã trả", BOOK002 chuyển "Có sẵn", **KHÔNG** hiển thị cảnh báo quá hạn                                         | REQ-05         | EP       |
| TC-25 | Xác nhận thành viên không thể trả sách không phải của mình | Đăng nhập MEM003 (không có phiếu mượn đang hoạt động; BOOK003 đang được MEM002 mượn theo seed data)                            | 1. Vào tab "Mượn/Trả" 2. Quan sát danh sách phiếu đang mượn của MEM003                                                                    | MEM003; BOOK003 (đang mượn bởi MEM002, không phải MEM003)      | Danh sách phiếu đang mượn của MEM003 không có BOOK003; không có nút "Trả" cho BOOK003                                                     | REQ-05         | EP       |
| TC-26 | Cảnh báo khi trả sách quá hạn                   | Đăng nhập MEM002; BR001 có dueDate 15/09/2024 đã qua ngày hiện tại — hệ thống tự phát hiện quá hạn khi trả (REQ-05 độc lập với nút "Kiểm tra quá hạn" của REQ-06, không cần nhấn nút đó trước) | 1. Vào tab "Mượn/Trả" 2. Chọn phiếu BR001 (BOOK003) 3. Nhấn "Trả"                                                                         | MEM002; BOOK003 (BR001, dueDate 15/09/2024)                    | Hệ thống hiển thị cảnh báo quá hạn (tự động theo dueDate, không cần nhấn "Kiểm tra quá hạn" trước); phiếu chuyển "Đã trả", BOOK003 chuyển "Có sẵn" | REQ-05         | EP       |
| TC-27 | Đánh dấu phiếu quá hạn sau khi kiểm tra         | Đăng nhập Thủ thư; dữ liệu ở trạng thái seed (BR001 dueDate 15/09/2024 chưa được đánh dấu "Quá hạn")                                           | 1. Nhấn "Kiểm tra quá hạn" 2. Mở danh sách phiếu mượn                                                                                     | librarian@library.com                                          | BR001 được đánh dấu trạng thái "Quá hạn"                                                                                                  | REQ-06         | EP       |
| TC-28 | Thành viên chỉ thấy phiếu quá hạn của mình      | Đã chạy "Kiểm tra quá hạn" (TC-27 đã thực hiện)                                                                                                | 1. Đăng nhập MEM002 2. Vào tab "Mượn/Trả"                                                                                                 | MEM002                                                         | MEM002 thấy phiếu BR001 của mình với trạng thái "Quá hạn"; không thấy phiếu quá hạn của thành viên khác                                   | REQ-06, REQ-08 | EP       |
| TC-29 | Thêm thành viên mới hợp lệ                      | Đăng nhập Thủ thư                                                                                                                               | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Le Test; Email: new.member@email.com; SĐT: 0900000000  | Thêm thành viên mới thành công, xuất hiện trong danh sách                                                                                 | REQ-07         | EP       |
| TC-30 | Từ chối email không hợp lệ (thiếu dấu .)        | Đăng nhập Thủ thư                                                                                                                               | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Le Sai; Email: user@domain; SĐT: 0900000001            | Thông báo lỗi email không hợp lệ                                                                                                          | REQ-07         | EP       |
| TC-31 | Từ chối email trùng                             | Đăng nhập Thủ thư                                                                                                                               | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Trung Lap; Email: ba.nguyen@email.com; SĐT: 0900000002 | Thông báo lỗi email đã tồn tại                                                                                                            | REQ-07         | EP       |
| TC-32 | Từ chối email không có @                        | Đăng nhập Thủ thư                                                                                                                               | 1. Vào tab "Thành viên" 2. Nhấn "Thêm" 3. Nhập thông tin 4. Lưu                                                                           | Họ tên: Le Sai; Email: userdomain.com; SĐT: 0900000003         | Thông báo lỗi email không hợp lệ                                                                                                          | REQ-07         | EP       |
| TC-33 | Tra cứu phiếu mượn đúng quyền (cả hai vai trò)  | Đăng nhập Thủ thư, sau đó đăng nhập MEM003                                                                                                     | 1. Đăng nhập Thủ thư, mở tab "Mượn/Trả" 2. Xác nhận thấy phiếu của nhiều thành viên 3. Đăng xuất 4. Đăng nhập MEM003 5. Mở tab "Mượn/Trả" | librarian@library.com; dam.tran@email.com                      | Thủ thư thấy tất cả phiếu; MEM003 chỉ thấy phiếu của mình (BR002, BR005), không thấy phiếu của MEM002 hay thành viên khác                 | REQ-08         | EP       |
| TC-34 | Mượn sách thành công tại biên = 0 sách đang mượn (BVA) | Đăng nhập MEM003 (không có phiếu mượn đang hoạt động trong seed data — BR002 và BR005 đều đã trả)                                         | 1. Vào tab "Sách" 2. Chọn BOOK004 3. Nhấn "Mượn"                                                                                          | MEM003; BOOK004                                                | Tạo phiếu mượn mới, dueDate = ngày mượn + 14 ngày, MEM003 có 1 sách đang mượn, BOOK004 chuyển "Đã mượn"                                   | REQ-04         | BVA      |
| TC-35 | Khôi phục dữ liệu về seed data                  | Đăng nhập Thủ thư, đã thay đổi dữ liệu                                                                                                         | 1. Nhấn nút "Khôi phục dữ liệu" 2. Xác nhận                                                                                               | librarian@library.com                                          | Tất cả dữ liệu trở về trạng thái ban đầu (seed data)                                                                                      | Tổng quát      | EP       |

---

## Tổng hợp

| Nhóm chức năng     | Số TC  | REQ phủ         | Kỹ thuật IDM áp dụng    |
| ------------------ | ------ | --------------- | ----------------------- |
| Đăng nhập          | 6      | REQ-01          | EP                      |
| Danh sách sách     | 3      | REQ-02          | EP                      |
| Tìm kiếm/lọc sách  | 7      | REQ-03          | EP                      |
| Mượn sách          | 8      | REQ-04          | EP, BVA, Decision Table |
| Trả sách           | 3      | REQ-05          | EP                      |
| Quá hạn            | 2      | REQ-06          | EP                      |
| Thành viên         | 4      | REQ-07          | EP                      |
| Tra cứu phiếu mượn | 1      | REQ-08          | EP                      |
| Tổng quát          | 1      | -               | EP                      |
| **Tổng**           | **35** | REQ-01 → REQ-08 | EP, BVA, Decision Table |

> **Ghi chú thực thi BVA giới hạn mượn sách (REQ-04)**:
> BVA cho giới hạn 3 sách được phân bổ qua 4 TC theo thứ tự sau:
> - **TC-34**: MEM003 — 0 sách → mượn thành công (BVA tại 0)
> - **TC-17**: MEM006 — 1 sách → mượn thành công (BVA tại 1)
> - **TC-23**: MEM002 — 2 sách → mượn thành công (BVA tại boundary−1 = 2)
> - **TC-21**: MEM002 — 3 sách → mượn thất bại (BVA tại boundary = 3); **chạy ngay sau TC-23, không reset dữ liệu giữa hai TC này**
