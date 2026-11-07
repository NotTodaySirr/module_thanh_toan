# Nhật ký prompt và kết quả

Ghi mỗi lượt thực tế, kể cả lượt lỗi hoặc không tạo file. Dùng mã `PRM-001`... Liên kết module, vai trò và vòng review nếu có. Không tạo lại lịch sử giả sau khi hoàn thành.

## PRM-001 - Tạo bộ hướng dẫn

- Module: `payment-management`.
- Ngày theo phiên làm việc: 07/11/2026, múi giờ Asia/Bangkok; không ghi giờ vì chưa lưu chính xác.
- Người yêu cầu: người dùng trong phiên hiện tại; không gán tên thành viên.
- Vai trò thực hiện: AI hỗ trợ soạn hướng dẫn, không phải lượt Design/Reviewer của bài tập.
- Prompt nguyên văn: “ok begin implementing the instructions, using the language in the template”.
- Ngữ cảnh đã thống nhất: dùng tiếng Việt theo template; tổ chức theo module và vòng review; Git giữ phiên bản thiết kế thay vì thư mục first-run/after-review.
- Đầu vào: PDF `btcohong.pdf`, ba file instructions của template trong dự án bên cạnh và trao đổi của phiên.
- Kết quả: tạo README, AGENTS.md, rules, ba agent, sổ vấn đề/xác nhận, biểu mẫu review và hướng dẫn nhật ký/phân công/trình bày.
- Phần AI tạo: nội dung khởi tạo của các file hướng dẫn và biểu mẫu trong workspace này.
- Phần nhóm sửa: chưa ghi nhận.
- Phiên bản: lượt soạn hướng dẫn này không tạo commit thiết kế. Workspace đã có repository và commit khởi tạo khi kiểm tra cuối lượt; commit thiết lập không phải baseline thiết kế lần đầu.

## Mẫu cho lượt tiếp theo

### [PRM-xxx] - [Tên nhiệm vụ]

- Thời điểm thực tế/múi giờ nếu biết:
- Module, vai trò, vòng review:
- Người thực hiện và công cụ/model nếu biết:
- Prompt nguyên văn hoặc đường dẫn bản lưu đầy đủ:
- Nguồn đầu vào và baseline SHA nếu có:
- File/kết quả tạo ra, kể cả lỗi và giới hạn:
- Nhận xét của người dùng/nhóm và căn cứ:
- Thay đổi tiếp theo, SHA thật nếu đã commit:
- Liên kết findings/quyết định/đóng góp:

Nếu prompt dài, lưu nguyên văn trong file thuộc `docs/process/prompts/` và liên kết; không chỉ ghi tóm tắt làm bằng chứng duy nhất.
