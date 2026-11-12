# Nguồn gốc và chỉnh sửa bộ hướng dẫn

Các đường dẫn template dưới đây tính từ workspace: `../QTKD-Agent-Seminar/`. Không chỉnh sửa template gốc.

| Thành phần mới | Nguồn tham khảo | Giữ lại | Thay đổi cho bài tập | Nguồn gốc nội dung khởi tạo |
| --- | --- | --- | --- | --- |
| `general-design.mdc` | `.cursor/rules/qtkd-context.mdc` | Đọc nguồn, dẫn căn cứ, không tự thêm nghiệp vụ, không sửa code khi chỉ phân tích | Tách nhãn bằng chứng, trạng thái xác nhận và cách tiếp tục phần độc lập | AI soạn; chưa ghi nhận nhóm sửa |
| `payment-context.mdc` | `btcohong.pdf` và cấu trúc rules mẫu | Ngữ cảnh nghiệp vụ theo nguồn | Thay toàn bộ miền QTKD rộng bằng phạm vi thanh toán và các mâu thuẫn của đề | AI soạn; chưa ghi nhận nhóm sửa |
| `ba.md` | `.cursor/agents/ba.md` | Actor, precondition, flows, rules, AC và câu hỏi | Thay nguồn, giữ mã US/PAY, bỏ bắt buộc Excel, bổ sung hồ sơ vấn đề và xác nhận | AI soạn; chưa ghi nhận nhóm sửa |
| `design.md` | `.cursor/agents/design.md` | HLD/LLD, AS-IS/TO-BE, UML, API/data, 8 tài liệu, truy vết và handoff | Bỏ đường dẫn code cố định, công nghệ mặc định và suy diễn nghiệp vụ; bắt buộc class/sequence/state theo đề | AI soạn; chưa ghi nhận nhóm sửa |
| `reviewer.md` | Yêu cầu review trong `btcohong.pdf`, trang 3 | Kiểm tra chất lượng và căn cứ | Vai trò mới: review đúng baseline, findings có bằng chứng, không quyết thay nhóm | AI soạn; chưa ghi nhận nhóm sửa |
| `versioning-review.mdc` và biểu mẫu | Yêu cầu 3 của PDF, lựa chọn Git đã thống nhất trong phiên | Lưu thiết kế đầu và sau review | Theo module/Rxxx, baseline/revised SHA, đánh giá nhóm và mapping thay đổi | AI soạn theo lựa chọn người dùng; chưa ghi nhận nhóm sửa |
| Hồ sơ process | Yêu cầu sản phẩm/đánh giá trang 3 | Nhật ký và minh chứng | Theo dõi prompt, phân công, AI/nhóm sửa và trình bày tối đa 5 slide | AI soạn; chưa ghi nhận nhóm sửa |

## Nhật ký nhóm chỉnh sửa instructions

| Mốc/người sửa thực tế | File/mục | Trước → Sau | Lý do | Prompt/commit hoặc bằng chứng |
| --- | --- | --- | --- | --- |

Nhóm phải cập nhật phần này khi thật sự sửa hướng dẫn; việc người dùng yêu cầu tạo file chưa có nghĩa đã kiểm tra và phê duyệt từng nội dung AI viết. Không gán toàn bộ nội dung do AI tạo thành đóng góp biên soạn của nhóm.

## Cập nhật workflow review phân tích - 12/11/2026

- Căn cứ: người dùng yêu cầu Reviewer review đầu ra giai đoạn phân tích sau khi BA hoàn thành, thay cho phạm vi phân tích và thiết kế; prompt nguyên văn tại PRM-010 trong `prompt-log.md`.
- Người thực hiện: Codex chỉnh sửa theo yêu cầu; chưa ghi nhận nhóm tự biên soạn hoặc kiểm tra lại nội dung.
- `reviewer.md`: đổi vai trò thành Analysis Reviewer, chỉ review requirements-analysis.md, issues.md, decisions.md cùng nguồn/xác nhận liên quan; kiểm tra coverage, AC, mâu thuẫn và căn cứ quyết định. Bỏ kiểm tra HLD/LLD, UML thiết kế, API/schema và đánh giá toàn bộ quy trình.
- Đồng bộ BA/Design handoff, rules truy vết và phiên bản, README cùng biểu mẫu review: BA → review phân tích → nhóm đánh giá → BA chỉnh sửa → Design. Hai SHA của vòng mới định danh phân tích trước/sau sửa; mốc thiết kế lưu riêng.
- Giữ nguyên hồ sơ BA, bộ thiết kế, baseline và vòng review cũ; không diễn giải bằng chứng review thiết kế cũ thành review phân tích, không tạo findings hoặc vòng review mới trong lượt sửa hướng dẫn này.
