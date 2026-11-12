# 07. Quyết định thiết kế (ADR)

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt bản cập nhật: người dùng trong phiên, 12/11/2026 (Asia/Bangkok), prompt “that is fine, change the status to approve”; căn cứ PRM-013 trong [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật: 12/11/2026, Asia/Bangkok; tiếp nhận BA sau review phân tích R002, PRM-012. Giữ nguyên baseline/tag lịch sử.
- Phê duyệt lịch sử: bản baseline ngày 07/11/2026 được người dùng duyệt tại PRM-008; bản cập nhật ngày 12/11/2026 có xác nhận riêng tại PRM-013.
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf), [BA](../../analysis/payment-management/requirements-analysis.md), [sổ xác nhận PAY-DEC](../../analysis/payment-management/decisions.md).
- Đầu vào review: [R002 - phân tích BA đã đối chiếu](../../reviews/payment-management/R002/review-record.md); nguồn BA tại revised_commit được ghi trong tài liệu bàn giao. Không thực hiện review mới trong lượt Design.

## 1. Quyền quyết định và căn cứ

PAY-DEC-001..014 là quyết định **nghiệp vụ đã xác nhận** tại PRM-006; PAY-DEC-015 là xác nhận bổ sung qua R002-F001 ngày 12/11/2026. Bản ADR ban đầu đã được phê duyệt tại PRM-008; cập nhật căn cứ/diễn giải ADR-006 dưới đây đã được người dùng phê duyệt cùng bộ thiết kế tại PRM-013. ADR không thay đổi ngưỡng, actor, reject, khóa kỳ, làm tròn hoặc giới hạn gửi lại; chỉ chọn cơ chế thực hiện. Không coi việc hoàn thiện ADR là phê duyệt của KTT/GĐKD, kết quả Reviewer hoặc bằng chứng chạy hệ thống.

## 2. Danh mục ADR

| Mã | Bối cảnh/vấn đề | Phương án đã cân nhắc | Lựa chọn kỹ thuật và lý do | Hậu quả/rủi ro/kiểm chứng | Căn cứ nghiệp vụ |
| --- | --- | --- | --- | --- | --- |
| ADR-001 | Module nhỏ, chưa có code/hạ tầng; cần transaction đơn giản | Một module phân lớp; microservices; dùng nguyên template bên cạnh | Một ứng dụng phân lớp + DB quan hệ giao dịch + worker nền dùng cùng DB; interface cho nguồn. Giảm vận hành và giao dịch phân tán; không mặc định ngôn ngữ/framework/vendor | Cần DB có khóa/unique/exact numeric; worker cần theo dõi; kiểm repository transaction và DEP-04. Không tái sử dụng code mẫu chưa khảo sát | S01/S04-S10; toàn bộ US |
| ADR-002 | Nguồn đổi; cần giữ lịch sử tiền và không đổi ngầm khi mở xem | Luôn đọc trực tiếp; chỉ lưu tổng; snapshot/revision bất biến | Giữ payload căn cứ + output + semantic hash/version/time; mỗi lưu Draft tạo revision. Reopen cũng tạo bản sao revision để submission sau độc lập; preview/accept tách biệt | Tăng dung lượng; phải định nghĩa canonical/hash và source coherence. Hash không chứng minh tính đúng nguồn; snapshot ngoài hệ thống cần DEP-01, không có atomicity xuyên nguồn. Test nguồn đổi nhưng tổng không đổi | PAY-DEC-001/006/008/009/012; NFR-PAY-01 |
| ADR-003 | Tháng/quý, Cancelled cho lập mới, hai lập đồng thời | Unique theo type kỳ; kiểm tra query trước insert; claim từng tháng | MonthClaim PK(contractId,month), quý chèn 3 hàng trong một TX; xóa claim khi hủy gốc, giữ lịch sử qua document/audit | Dễ chứng minh chống chồng; cần toàn bộ tạo/hủy dùng cùng giao dịch; không được xóa claim Approved. Test quý-vs-tháng và rollback | PAY-01; PAY-DEC-004/007 |
| ADR-004 | Retry mạng, bấm lặp, tranh chấp approve/reject/đổi người | UI disable; chỉ optimistic version; version + khóa + receipt | ETag/If-Match, khóa chain→document→submission, unique submission/revision và receipt key. PendingApproval submit trả lần hiện hành dù ETag cũ, vẫn kiểm quyền. Không tạo thông báo mới | Receipt tăng dữ liệu; quyền kiểm lại trước replay; deadlock cần lỗi kiểm soát. Test hai key/hai kết nối, mất phản hồi sau commit, đổi người cùng lúc quyết định | PAY-04/07/08; PAY-DEC-011/014 |
| ADR-005 | DB commit thành công nhưng gửi lỗi hoặc worker chết | Gửi dưới transaction; gửi sau commit không lưu; transactional outbox | Lưu Notification trong TX-SUBMIT, worker đọc và gửi ngoài TX; retry ở +1/+5/+14 phút từ lỗi đầu, tối đa 3/15 phút; sau hạn chuyển vận hành. Check submission/assignment trước gửi | Có thể giao lặp nếu crash sau gửi; cùng reference giúp nhận biết. Không hứa exactly-once kênh ngoài; link luôn kiểm quyền hiện hành. Test crash/restart/lease/đổi người/quá hạn | US-PAY-02; PAY-DEC-014 |
| ADR-006 | Approved bất biến nhưng cần chỉnh sai, không cộng lặp hoặc tự duyệt | Sửa trực tiếp; clone độc lập không liên kết; chuỗi có con trỏ | Original/Adjustment chung mô hình vòng đời, chain giữ bản Approved hiện hành + một slot điều chỉnh đang xử lý; lưu đầy đủ tổng mới và delta; người lập gốc/điều chỉnh đều bị loại duyệt | Draft/PendingApproval/Rejected giữ vị trí; Approved/Cancelled chỉ kết thúc vị trí chính hồ sơ, vẫn xét toàn chuỗi khi tạo tiếp/bàn giao. Guard/con trỏ và flag đọc cùng snapshot; kiểm T21-T23 ngoài ví dụ 480→510 | US-PAY-04; PAY-05/07; PAY-DEC-001/002/003/007/011/013/015 |
| ADR-007 | Biên bản là điều kiện nghiệp vụ; chưa có kho tài liệu thật | URL ngoài không kiểm soát; blob trong DB; EvidenceStore bất biến | Cổng kho nội bộ + metadata mã/ngày/các bên/checksum; PDF/PNG/JPEG ≤20 MiB là mặc định kỹ thuật upload, có thể đổi khi triển khai. Không tải URL tùy ý; blob ghi trước metadata, orphan xử lý vận hành | Cần DEP-03; lỗi kho chặn attach/trình/duyệt khi không xác minh được bằng chứng. Giới hạn dung lượng không là quy tắc trong PDF; không tự xây module tài liệu tổng quát | US-PAY-04; PAY-DEC-001/012 |
| ADR-008 | Làm tròn và so ngưỡng phải nhất quán, dữ liệu đầu vào chưa biết độ lớn | Binary float; fixed decimal bị cắt; decimal chính xác | Arbitrary-precision decimal ở tính toán và NUMERIC chính xác ở kho; JSON số dạng string; giữ tích tối đa 12 số lẻ, HALF_UP dòng rồi cộng; hai tổng tách biệt | Cần kiểm thư viện/DB với dữ liệu nguồn thật; tránh overflow và quá 6 số lẻ. Không tự đặt trần giá trị nghiệp vụ. Test -0,4, ±0,5, hai dòng 100,5 và biên ngưỡng | PAY-03/04/06; PAY-DEC-008/009 |

## 3. Các phụ thuộc chưa có bằng chứng thực hiện

DEP-01..DEP-05 ở [bàn giao](08-handoff-to-developer.md) là đầu vào tích hợp/vận hành, không phải 14 lựa chọn nghiệp vụ cần phê duyệt lại. Khi có dữ liệu thực tế khác năng lực dự kiến, cập nhật adapter/ADR và kiểm tra tác động; chỉ mở quyết định nghiệp vụ mới khi thực sự đổi hành vi đã xác nhận. Không sửa ngược PRM-006 hoặc PDF.

## 4. Mốc thiết kế và cập nhật sau review phân tích

Baseline thiết kế đã duyệt tại PRM-008/009 giữ nguyên SHA `44c586c47105a9107af9673ff56604e83bd07ece` và tag `payment-management-design-baseline-v1`. R002 sau đó review **phân tích BA**; revised_commit BA là `fb1160f459da7c65e475651af3bba1dc6a0de477`, không phải thiết kế sửa và không dùng làm revised_commit của một vòng review thiết kế.

Lượt PRM-012 tiếp nhận BA sửa và hồ sơ R002, mở rộng truy vết từ 30 lên 37 AC, thêm căn cứ PAY-DEC-015 cho ADR-006/guard chuỗi, làm rõ response A02 và bổ sung bảy kịch bản kiểm chứng. Không thêm ADR, module, endpoint hoặc trạng thái. Đây là cập nhật thiết kế, không thay findings/team-evaluation/changes của BA; mapping tác động nằm tại tài liệu 06 và 08.

Workflow hiện hành không giao Reviewer review thiết kế; không yêu cầu mở vòng review thiết kế mới. Phê duyệt bản baseline giữ nguyên như bằng chứng lịch sử; bản cập nhật được người dùng phê duyệt riêng tại PRM-013. Không tạo commit hoặc đổi baseline/tag trong lượt rà soát này; chỉ lưu mốc thiết kế tiếp theo khi được giao. Nếu sau này có quyết định nghiệp vụ mới, nối thêm căn cứ và giữ nguyên lịch sử xác nhận.
