# Bảng phân công và đóng góp thực tế

Chưa có tên thành viên hoặc phân công được xác nhận. Không gán tên, tỷ lệ hoặc khối lượng công việc giả. Vai trò dưới đây là gợi ý để nhóm phân công, không phải bằng chứng công việc đã làm.

| Công việc gợi ý | Thành viên phụ trách | Sản phẩm dự kiến | Đóng góp thực tế/phần AI và phần tự sửa | Minh chứng prompt/commit/review | Trạng thái |
| --- | --- | --- | --- | --- | --- |
| Phân tích đặc tả và câu hỏi xác nhận | Chưa có tên thành viên; người dùng giao Codex vai trò BA | Hồ sơ phân tích và xác nhận | AI cập nhật ba hồ sơ BA sau R002, ghi PAY-DEC-015 và 7 AC mới; người dùng đã xác nhận phương án tại findings mục 4, chưa ghi nhận phần nhóm tự sửa file | PRM-011; docs/analysis/payment-management/; revised_commit fb1160f459da7c65e475651af3bba1dc6a0de477 | BA đã sửa và đối chiếu R002; bàn giao Design |
| Kiến trúc, API và thiết kế dữ liệu | Chưa có tên thành viên; người dùng giao Codex vai trò Design | Bộ thiết kế hiện hành | AI giữ bộ thiết kế ban đầu đã duyệt, cập nhật 8 tài liệu theo BA R002/PAY-DEC-015 và 7 AC mới; chưa ghi nhận phần nhóm tự sửa | PRM-007/008/009/012; docs/design/payment-management/; tag baseline ban đầu giữ nguyên | Bản cập nhật sau BA R002: Design Approved theo PRM-013; chưa commit |
| UML và kiểm tra nhất quán | Chưa có tên thành viên; Codex thực hiện trong nhiệm vụ Design | Class/sequence/state diagram | AI cập nhật 6 PlantUML/PNG, render cục bộ và tự kiểm tra; không là review độc lập | PRM-007/012; docs/design/payment-management/diagrams/; ma trận 37 AC | Đã đồng bộ sau BA R002; người dùng phê duyệt cùng bộ thiết kế tại PRM-013 |
| Đánh giá Reviewer và đối chiếu phiên bản | Người dùng trong hội thoại đánh giá; Codex vai trò BA ghi bằng chứng đối chiếu | Hồ sơ R002 và Git diff | Người dùng xác nhận 16 Đúng/0 Sai/1 Thừa tại findings mục 4; AI áp dụng phần được chấp thuận, giữ nguyên findings/team-evaluation và đối chiếu baseline → revised_commit | docs/reviews/payment-management/R002/changes.md và review-record.md mục 5; PRM-011 | R002 đã đối chiếu ở phạm vi BA; không là phê duyệt thiết kế hoặc kiểm thử ứng dụng |
| Nhật ký, trình bày và chuẩn bị vấn đáp | Chưa phân công | Minh chứng và slide | Chưa ghi nhận | Chưa có | Chưa thực hiện |

Một thành viên có thể đảm nhiệm nhiều việc. Bổ sung kết quả thực tế sau từng mốc; mọi người cần hiểu quyết định, mâu thuẫn và tác động khi thay đổi yêu cầu, không chỉ phần được giao.
