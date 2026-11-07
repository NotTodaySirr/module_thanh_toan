# Bộ thiết kế hiện hành - payment-management

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt: người dùng trong phiên, 07/11/2026 (Asia/Bangkok), prompt “ok approve”; căn cứ PRM-008 trong [nhật ký prompt](../../process/prompt-log.md).
- Ngày: 07/11/2026, Asia/Bangkok. Người soạn: Codex / System Design Agent; người dùng trong phiên đã phê duyệt tại PRM-008, chưa cung cấp tên cá nhân.
- Nguồn: [PDF gốc, 3 trang](../../requirements/btcohong.pdf), [phân tích BA](../../analysis/payment-management/requirements-analysis.md), [14 quyết định đã xác nhận PRM-006](../../analysis/payment-management/decisions.md).
- Loại mốc: **Thiết kế lần chạy đầu**. Vòng review: chưa mở. Người dùng yêu cầu lưu baseline tại PRM-009; mốc được định danh bằng tag `payment-management-design-baseline-v1`.

## 1. Tài liệu

| Tài liệu | Nội dung |
| --- | --- |
| [01 - Tóm tắt yêu cầu](01-requirements-summary.md) | Phạm vi, actor, 5 US, 8 PAY rule, 2 NFR và xác nhận nghiệp vụ |
| [02 - Kiến trúc](02-architecture-design.md) | AS-IS có bằng chứng, TO-BE phân lớp, cổng nguồn chỉ đọc, bảo mật, giao dịch, audit và thông báo |
| [03 - Thiết kế chi tiết](03-detailed-design.md) | Class/interface, tính tiền, guard và vòng đời, luồng chính/lỗi |
| [04 - Hợp đồng API](04-api-contract.md) | A01-A19, payload, quyền, ETag, idempotency, status/error, audit và mapping |
| [05 - Dữ liệu](05-data-design.md) | Entity/kiểu/quan hệ, claim tháng, revision, submission, constraint/index/TX và lịch sử |
| [06 - Truy vết](06-traceability-matrix.md) | Từng US/rule/NFR, 30 AC và 14 PAY-DEC → lớp/UML/API/data/test |
| [07 - ADR](07-design-decisions.md) | 8 lựa chọn kỹ thuật, lý do, phương án, rủi ro và căn cứ nghiệp vụ |
| [08 - Bàn giao](08-handoff-to-developer.md) | Thứ tự triển khai, 5 nhóm phụ thuộc DEP, 20 nhóm kịch bản test và điều kiện hoàn thành |

Chưa có mã nguồn ứng dụng được cung cấp trong workspace module. Endpoint/schema/lớp/giải pháp là đề xuất kỹ thuật; PAY-DEC-001..014 là quyết định nghiệp vụ đã xác nhận, không hỏi lại. Quyền xem, dữ liệu nguồn, nhân sự, kho biên bản và môi trường thực đo vẫn cần được cung cấp khi tích hợp; đây là phụ thuộc thực hiện, không phải mâu thuẫn nghiệp vụ đang mở.

## 2. UML và bản xem

Dùng ngữ nghĩa [UML 2.5.1](https://www.omg.org/spec/UML/2.5.1), cú pháp [PlantUML chính thức](https://plantuml.com/). Sáu sơ đồ đều có tên/phạm vi/nguồn. Ba sequence bổ sung tách luồng để dễ đọc; không tạo component/deployment diagram dư thừa.

| Sơ đồ | Mã nguồn | Bản PNG cục bộ |
| --- | --- | --- |
| Class bắt buộc | [class.puml](diagrams/class.puml) | [Xem class](diagrams/rendered/class.png) |
| Sequence trình bắt buộc | [sequence.puml](diagrams/sequence.puml) | [Xem trình/thông báo](diagrams/rendered/sequence.png) |
| State bắt buộc | [state-machine.puml](diagrams/state-machine.puml) | [Xem vòng đời](diagrams/rendered/state-machine.png) |
| Sequence lập/cập nhật | [sequence-create.puml](diagrams/sequence-create.puml) | [Xem lập/cập nhật](diagrams/rendered/sequence-create.png) |
| Sequence duyệt/từ chối | [sequence-review.puml](diagrams/sequence-review.puml) | [Xem duyệt/mở lại](diagrams/rendered/sequence-review.png) |
| Sequence điều chỉnh | [sequence-adjustment.puml](diagrams/sequence-adjustment.puml) | [Xem điều chỉnh](diagrams/rendered/sequence-adjustment.png) |

PlantUML là nguồn chỉnh sửa, PNG là bản xem phát sinh; sửa sơ đồ phải render lại. Không gửi tài liệu tới dịch vụ render công khai. Lượt này dùng Java 17, Graphviz có sẵn và PlantUML 1.2025.10 cục bộ, kiểm cú pháp bằng `-charset UTF-8 -checkonly`, render `-tpng -o rendered`; đã mở ảnh kiểm tra và chỉnh bố cục/nhãn bị đè ở bản state ban đầu. Lệnh đầy đủ có thể tái tạo với đường dẫn JAR cục bộ tại máy thực hiện, không coi đường dẫn runtime là dependency của dự án.

## 3. Kiểm tra và giới hạn

Đối chiếu nội dung với đủ ba trang PDF và BA/PAY-DEC; kiểm tra phủ 5 US, 8 rule, 2 NFR, 30 AC, 14 quyết định; kiểm tra liên kết, bảng Markdown, JSON ví dụ, mã API/test và syntax/render PlantUML. Kết quả định lượng và lệnh thực tế ghi ở PRM-007 trong [nhật ký prompt](../../process/prompt-log.md).

Đây là tự kiểm tra chất lượng tài liệu của Design Agent, không phải Reviewer độc lập hoặc test ứng dụng. Chưa đo phản hồi 3 giây, kiểm tra DB thật, gửi thông báo hoặc bàn giao kế toán thực tế. Phê duyệt thiết kế tại PRM-008 không thay thế review độc lập hoặc nghiệm thu ứng dụng.

## 4. Baseline trước review

Theo yêu cầu PRM-009, lưu baseline chứa thiết kế đã phê duyệt, hồ sơ BA, PDF nguồn ở vị trí hiện tại và nhật ký liên quan. Tag `payment-management-design-baseline-v1` định danh mốc thiết kế lần đầu trước review; SHA đầy đủ được lưu bằng commit hồ sơ kế tiếp để tránh tự tham chiếu. Commit hướng dẫn `72ec6f0` không phải baseline thiết kế. Chưa mở vòng review hoặc điền đánh giá nhóm. Reviewer phải đọc đúng SHA của mốc; quy trình tại [hướng dẫn review](../../reviews/README.md).
