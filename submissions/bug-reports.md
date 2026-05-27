# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin        |               |
| ---------------- | ------------- |
| **Nhóm**         | stqa_group_15 |
| **Ngày báo cáo** | 24/05/2026    |

---

## BUG-01

| Thuộc tính          | Chi tiết              |
| ------------------- | --------------------- |
| **Mã lỗi**          | BUG-01                |
| **TC liên quan**    | TC-07                 |
| **REQ liên quan**   | REQ-02                |
| **Mức độ**          | LOW                   |
| **Người phát hiện** | Nghiêm Trọng Quốc Anh |
| **Ngày phát hiện**  | 27/05/2026            |
| **Trạng thái**      | Open                  |

**Tiêu đề:**
`BOOK003 hiển thị trạng thái "Đang mượn" thay vì "Đã mượn" trong danh sách sách`

**Môi trường:**

- Trình duyệt: Chrome (chưa xác định phiên bản)
- Hệ điều hành: Linux
- Ngôn ngữ giao diện: Tiếng Việt

**Điều kiện tiên quyết:**
Đăng nhập thành công, dữ liệu đang ở trạng thái seed.

**Bước tái hiện:**

1. Đăng nhập bằng tài khoản thành viên binh.pham@email.com.
2. Vào tab "Sách".
3. Quan sát danh sách và trạng thái của BOOK003.

**Kết quả mong đợi:**
Hiển thị đầy đủ thông tin sách; BOOK003 hiển thị "Đã mượn".

**Kết quả thực tế:**
BOOK003 hiển thị trạng thái "Đang mượn".

**Tác động:**
Hiển thị sai trạng thái sách trên giao diện, gây nhầm lẫn cho thành viên khi theo dõi danh mục. Ảnh hưởng trực tiếp đến trải nghiệm người dùng và tính chính xác của luồng quản lý mượn/trả.
**Minh chứng:**
![BUG01](ScreenShot/BUG01.png)

**Đề xuất xử lý:**
-Xác minh lại tính chính xác của dữ liệu trong tệp seed data đối với bản ghi của mã sách này.
-Đồng bộ lại logic hiển thị trạng thái trên giao diện (UI component) để khớp với mã trạng thái (status code) trả về từ API Backend.

---

## BUG-02

| Thuộc tính          | Chi tiết              |
| ------------------- | --------------------- |
| **Mã lỗi**          | BUG-02                |
| **TC liên quan**    | TC-16                 |
| **REQ liên quan**   | REQ-03                |
| **Mức độ**          | Medium                |
| **Người phát hiện** | Nghiêm Trọng Quốc Anh |
| **Ngày phát hiện**  | 27/05/2026            |
| **Trạng thái**      | Open                  |

**Tiêu đề:**
`Kết hợp tìm kiếm "Flutter" với thể loại "Kinh tế" vẫn trả về sách`

**Bước tái hiện:**

1. Đăng nhập bằng tài khoản thành viên.
2. Vào tab "Sách".
3. Chọn thể loại "Kinh tế".
4. Nhập từ khóa "Flutter".

**Kết quả mong đợi:**
Không hiển thị sách nào.

**Kết quả thực tế:**
Hệ thống hiển thị sách thuộc thể loại Kinh tế hoặc có tên "Flutter".

**Tác động:**
Kết hợp lọc + tìm kiếm không đúng logic giao, làm sai kết quả tìm kiếm của người dùng.

**Minh chứng:**
![BUG02](ScreenShot/BUG02.png)

**Đề xuất xử lý:**
Kiểm tra logic kết hợp điều kiện lọc thể loại và tìm kiếm theo từ khóa ở API/UI.

---

<!-- Copy template BUG trên để thêm BUG-03, BUG-04, ... cho mỗi TC Fail -->
