# Hồ sơ vòng review

Đây là biểu mẫu. Chỉ sao chép vào vòng thật sau khi xác định module và baseline commit. Thay các chỗ trong ngoặc vuông bằng dữ kiện; không bịa người, ngày, SHA hoặc kết quả kiểm tra.

| Trường | Giá trị |
| --- | --- |
| Module | [module-id] |
| Vòng | [Rxxx] |
| Giai đoạn | Phân tích - sau BA, trước Design |
| Loại mốc | [Phân tích BA lần đầu / Vòng cải tiến phân tích tiếp theo] |
| Trạng thái vòng | [Chuẩn bị / Đang review / Chờ đánh giá nhóm / Đang chỉnh sửa / Đã đối chiếu] |
| Trạng thái phân tích | [BA hoàn thành lượt phân tích - còn điểm mở / Đã cập nhật sau review / Đã bàn giao Design; không tự ghi phê duyệt] |
| Repository root | [Đường dẫn gốc repository] |
| Phạm vi phân tích | [Ba file requirements-analysis.md, issues.md, decisions.md trong docs/analysis/<module-id>/; đường dẫn từ workspace và tiền tố từ repository root nếu khác] |
| Ngoài phạm vi | Thiết kế HLD/LLD, UML thiết kế, API/schema, mã ứng dụng và đánh giá toàn bộ quy trình |
| baseline_commit | [SHA đầy đủ đã kiểm tra tồn tại, không dùng HEAD làm giá trị cố định] |
| revised_commit | [Chưa có - chỉ điền SHA thật sau commit phân tích BA đã sửa] |
| Nguồn đặc tả/xác nhận tại baseline | [Đường dẫn PDF và căn cứ xác nhận; ghi riêng nguồn bổ sung ngoài baseline nếu có] |
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

Ghi kiểm tra bao phủ US/rule/NFR, AC, căn cứ vấn đề/quyết định, liên kết, tính nhất quán giữa ba hồ sơ BA và Git diff. Không yêu cầu UML/API/schema hoặc phép đo ứng dụng trong review phân tích. Chưa chạy thì ghi chưa chạy; không điền “đạt” mặc định.

## 3. Kết quả và điểm còn mở

[Liên kết findings.md, team-evaluation.md, changes.md; finding/vấn đề chưa xử lý; ảnh hưởng; đề nghị vòng tiếp nếu có.]

## 4. Xác nhận đóng vòng

[Ai xác nhận và căn cứ thực tế nếu có; bản phân tích bàn giao cho Design và điểm còn mở. Đã đối chiếu không đồng nghĩa phân tích, nghiệp vụ hoặc thiết kế đã được phê duyệt.]
