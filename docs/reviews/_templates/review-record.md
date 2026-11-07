# Hồ sơ vòng review

Đây là biểu mẫu. Chỉ sao chép vào vòng thật sau khi xác định module và baseline commit. Thay các chỗ trong ngoặc vuông bằng dữ kiện; không bịa người, ngày, SHA hoặc kết quả kiểm tra.

| Trường | Giá trị |
| --- | --- |
| Module | [module-id] |
| Vòng | [Rxxx] |
| Loại mốc | [Thiết kế lần chạy đầu / Vòng cải tiến tiếp theo] |
| Trạng thái vòng | [Chuẩn bị / Đang review / Chờ đánh giá nhóm / Đang chỉnh sửa / Đã đối chiếu] |
| Trạng thái thiết kế | [Bản nháp - còn điểm cần xác nhận / Design Pending Approval / Đã phê duyệt có căn cứ] |
| Repository root | [Đường dẫn gốc repository] |
| Phạm vi thiết kế | [Đường dẫn từ workspace và tiền tố từ repository root nếu khác] |
| baseline_commit | [SHA đầy đủ đã kiểm tra tồn tại, không dùng HEAD làm giá trị cố định] |
| revised_commit | [Chưa có - chỉ điền SHA thật sau commit thiết kế sửa] |
| Nguồn phân tích/đặc tả tại baseline | [Đường dẫn và phiên bản; ghi riêng nguồn bổ sung ngoài baseline nếu có] |
| Người/công cụ tạo baseline | [Thông tin thực tế] |
| Reviewer và công cụ/model nếu biết | [Thông tin thực tế] |
| Người đánh giá nhận xét | [Chưa có / Thông tin thực tế] |
| Thời điểm các mốc | [Giá trị thực tế, ghi múi giờ nếu biết] |
| Prompt ID | [Liên kết nhật ký prompt] |

## 1. Phạm vi và giới hạn

[Các file đã đọc tại baseline, phần không review được, lý do, thay đổi working tree có liên quan.]

## 2. Kiểm tra thực tế

| Kiểm tra/lệnh/công cụ | Phiên bản được kiểm tra | Kết quả thực tế | Giới hạn |
| --- | --- | --- | --- |

Ghi riêng kiểm tra cú pháp/render UML, liên kết, tính nhất quán và Git diff. Chưa chạy thì ghi chưa chạy; không điền “đạt” mặc định.

## 3. Kết quả và điểm còn mở

[Liên kết findings.md, team-evaluation.md, changes.md; finding/vấn đề chưa xử lý; ảnh hưởng; đề nghị vòng tiếp nếu có.]

## 4. Xác nhận đóng vòng

[Ai xác nhận và căn cứ thực tế nếu có. Đã đối chiếu không đồng nghĩa thiết kế đã được phê duyệt.]
