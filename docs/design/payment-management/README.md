# Bộ thiết kế hiện hành - payment-management

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt bản cập nhật: người dùng trong phiên, 12/11/2026 (Asia/Bangkok), prompt “that is fine, change the status to approve”; căn cứ PRM-013 trong [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật: 12/11/2026, Asia/Bangkok; tiếp nhận BA sau review phân tích R002, PRM-012. Giữ nguyên baseline/tag lịch sử.
- Phê duyệt lịch sử: bản baseline ngày 07/11/2026 được người dùng duyệt tại PRM-008; bản cập nhật ngày 12/11/2026 có xác nhận riêng tại PRM-013.
- Ngày: 07/11/2026, Asia/Bangkok. Người soạn: Codex / System Design Agent; phê duyệt PRM-008 thuộc bản baseline ngày 07/11/2026, bản cập nhật được người dùng phê duyệt tại PRM-013.
- Nguồn: [PDF gốc, 3 trang](../../requirements/btcohong.pdf), [phân tích BA](../../analysis/payment-management/requirements-analysis.md), [15 quyết định đã xác nhận, PRM-006 và bổ sung R002](../../analysis/payment-management/decisions.md).
- Phiên bản hiện hành: **Cập nhật Design theo BA sau R002** (PRM-012). R002 đã đối chiếu phạm vi phân tích BA; không yêu cầu Reviewer review thiết kế theo workflow mới. Baseline thiết kế lần đầu được giữ tại [hồ sơ mốc và SHA đầy đủ](../../process/baselines/payment-management-initial.md), tag `payment-management-design-baseline-v1`.

## 1. Tài liệu

| Tài liệu | Nội dung |
| --- | --- |
| [01 - Tóm tắt yêu cầu](01-requirements-summary.md) | Phạm vi, actor, 5 US, 8 PAY rule, 2 NFR và xác nhận nghiệp vụ |
| [02 - Kiến trúc](02-architecture-design.md) | AS-IS có bằng chứng, TO-BE phân lớp, cổng nguồn chỉ đọc, bảo mật, giao dịch, audit và thông báo |
| [03 - Thiết kế chi tiết](03-detailed-design.md) | Class/interface, tính tiền, guard và vòng đời, luồng chính/lỗi |
| [04 - Hợp đồng API](04-api-contract.md) | A01-A19, payload, quyền, ETag, idempotency, status/error, audit và mapping |
| [05 - Dữ liệu](05-data-design.md) | Entity/kiểu/quan hệ, claim tháng, revision, submission, constraint/index/TX và lịch sử |
| [06 - Truy vết](06-traceability-matrix.md) | Từng US/rule/NFR, 37 AC và 15 PAY-DEC → lớp/UML/API/data/test |
| [07 - ADR](07-design-decisions.md) | 8 lựa chọn kỹ thuật, lý do, phương án, rủi ro và căn cứ nghiệp vụ |
| [08 - Bàn giao](08-handoff-to-developer.md) | Thứ tự triển khai, 5 nhóm phụ thuộc DEP, 27 nhóm kịch bản test và điều kiện hoàn thành |

Chưa có mã nguồn ứng dụng được cung cấp trong workspace module. Endpoint/schema/lớp/giải pháp là đề xuất kỹ thuật; PAY-DEC-001..015 là quyết định nghiệp vụ đã xác nhận, không hỏi lại; PAY-DEC-015 là phần bổ sung sau R002. Quyền xem, dữ liệu nguồn, nhân sự, kho biên bản và môi trường thực đo vẫn cần được cung cấp khi tích hợp; đây là phụ thuộc thực hiện, không phải mâu thuẫn nghiệp vụ đang mở.

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

PRM-007 ghi kiểm tra bản thiết kế ban đầu với 30 AC/14 quyết định. Bản hiện hành mở rộng lên 37 AC/15 quyết định sau R002; kiểm tra mới về truy vết, liên kết, JSON, bảng, UML/render và bảo toàn đầu vào được ghi riêng tại PRM-012 trong [nhật ký prompt](../../process/prompt-log.md).

Đây là tự kiểm tra chất lượng tài liệu của Design Agent, không phải Reviewer độc lập hoặc test ứng dụng. Chưa đo phản hồi 3 giây, kiểm tra DB thật, gửi thông báo hoặc bàn giao kế toán thực tế. Phê duyệt thiết kế tại PRM-008 không thay thế review độc lập hoặc nghiệm thu ứng dụng.

## 4. Baseline lịch sử và đầu vào BA hiện hành

Đã lưu baseline thiết kế `44c586c47105a9107af9673ff56604e83bd07ece`, tag `payment-management-design-baseline-v1`; [hồ sơ mốc](../../process/baselines/payment-management-initial.md) giữ nguyên. Mốc này cũng được R002 sử dụng làm snapshot BA với phạm vi phân tích riêng, không đổi ý nghĩa mốc thiết kế. Bản cập nhật hiện hành tiếp nhận BA revised_commit `fb1160f459da7c65e475651af3bba1dc6a0de477` và hồ sơ R002, không ghi đè baseline/tag hoặc đánh giá Reviewer. Chưa lưu commit thiết kế sửa trong lượt này.

## 5. Kết quả rà soát sau BA R002

Cần đồng bộ thiết kế; phần lớn cơ chế đã phù hợp nên giữ kiến trúc, 19 endpoint, schema và 5 trạng thái. Đã bổ sung căn cứ PAY-DEC-015, guard/cờ bàn giao toàn chuỗi kể cả hồ sơ lịch sử, 7 dòng AC mới trong ma trận và T21-T27. Response đọc A02 thêm activeAdjustmentId; không thêm API bàn giao hoặc trạng thái. Xem [báo cáo tác động và nguồn đầu vào](08-handoff-to-developer.md#6-báo-cáo-tác-động-và-bàn-giao-cập-nhật-sau-ba-r002).

Bản baseline đã được duyệt vẫn là bằng chứng lịch sử; bản cập nhật hiện hành ở trạng thái Design Approved theo xác nhận PRM-013 ngày 12/11/2026. Không còn lựa chọn nghiệp vụ mới cần hỏi lại cho F001/F002. Các phụ thuộc dữ liệu/nhân sự/môi trường và giới hạn chưa kiểm thử ứng dụng vẫn giữ nguyên.
