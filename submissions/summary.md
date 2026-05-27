# Test Summary — Báo cáo tổng hợp kiểm thử

> **Hướng dẫn**: Đây là hoạt động **Quality Assurance** — bạn đánh giá chất lượng tổng thể của phần mềm, không chỉ liệt kê lỗi.

---

## 1. Thông tin nhóm

| Mục                   | Thông tin                  |
| --------------------- | -------------------------- |
| **Nhóm**              | stqa_group_15              |
| **Lớp**               | `<!-- Cap nhat sau -->`    |
| **Ngày báo cáo**      | 24/05/2026                 |
| **Hệ thống kiểm thử** | https://stqa.rbc.vn — v1.0 |

---

## 2. Tổng quan kết quả

| Chỉ số               | Giá trị |
| -------------------- | ------- |
| Tổng số test case    | 25      |
| Pass                 | 0       |
| Fail                 | 0       |
| Blocked              | 0       |
| Not Run              | 25      |
| **Tỷ lệ Pass**       | 0%      |
| **Số bug phát hiện** | 0       |

### Phân bổ theo nhóm chức năng

| Nhóm chức năng     | TC  | Pass | Fail | Bug | Đánh giá      |
| ------------------ | --- | ---- | ---- | --- | ------------- |
| Đăng nhập          | 4   | 0    | 0    | 0   | Chua thuc thi |
| Danh sách sách     | 2   | 0    | 0    | 0   | Chua thuc thi |
| Tìm kiếm/lọc       | 5   | 0    | 0    | 0   | Chua thuc thi |
| Mượn sách          | 5   | 0    | 0    | 0   | Chua thuc thi |
| Trả sách           | 3   | 0    | 0    | 0   | Chua thuc thi |
| Quá hạn            | 2   | 0    | 0    | 0   | Chua thuc thi |
| Thành viên         | 3   | 0    | 0    | 0   | Chua thuc thi |
| Tra cứu phiếu mượn | 1   | 0    | 0    | 0   | Chua thuc thi |

### Phân bổ bug theo mức độ

| Mức độ | Số lượng | Bug IDs |
| ------ | -------- | ------- |
| High   | 0        | -       |
| Medium | 0        | -       |
| Low    | 0        | -       |

---

## 3. Kỹ thuật thiết kế đã sử dụng

| Kỹ thuật       | Áp dụng cho REQ nào?                                   | Số TC sử dụng | Giải thích cách áp dụng                                                       |
| -------------- | ------------------------------------------------------ | ------------- | ----------------------------------------------------------------------------- |
| EP             | REQ-01, REQ-02, REQ-03, REQ-05, REQ-06, REQ-07, REQ-08 | 20            | Chia miền đầu vào theo phân vùng rời rạc (valid/invalid, vai trò, trạng thái) |
| BVA            | REQ-04                                                 | 1             | Kiểm tra biên giới hạn 3 sách đang mượn                                       |
| Decision Table | REQ-04                                                 | 5             | Kết hợp điều kiện trạng thái sách, trạng thái thành viên, giới hạn mượn       |

---

## 4. Phân tích chất lượng phần mềm

### 4.1. Điểm mạnh

Chua thuc thi nen chua danh gia duoc diem manh.

### 4.2. Điểm yếu

Chua thuc thi nen chua phat hien van de.

---

## 5. Đề xuất ưu tiên sửa lỗi

> 💡 Đây là phần **Quality Assurance**: bạn không chỉ tìm lỗi mà còn **đề xuất thứ tự ưu tiên** sửa chữa và đánh giá tác động.
> Nêu rõ tiêu chí ưu tiên: dựa vào **severity** (mức độ nghiêm trọng kỹ thuật) và/hoặc **priority** (mức độ ưu tiên kinh doanh).

| Thứ tự | Bug | Mức độ | Lý do ưu tiên                |
| ------ | --- | ------ | ---------------------------- |
| -      | -   | -      | Chua co bug do chua thuc thi |

---

## 6. Kết luận

Chua thuc thi nen chua du co so danh gia san sang phat hanh.

---

## 7. Bài học rút ra (Tùy chọn)

`<!-- Nhóm bạn học được gì từ quá trình kiểm thử này? -->`

---

## 8. Khai báo sử dụng AI (Tùy chọn)

> Nếu nhóm có sử dụng công cụ AI (ChatGPT, Copilot, Gemini...), hãy ghi rõ bên dưới. Khai báo trung thực **không ảnh hưởng điểm** — đây là kỹ năng minh bạch trong nghề.

| Công cụ AI     | Dùng cho phần nào                   | Bạn đã kiểm tra/chỉnh sửa thế nào           |
| -------------- | ----------------------------------- | ------------------------------------------- |
| GitHub Copilot | Soan test case va tong hop theo SRS | Doi chieu va chinh sua theo SRS + seed data |
