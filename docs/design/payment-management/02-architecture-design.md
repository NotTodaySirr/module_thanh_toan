# 02. Thiết kế kiến trúc

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt bản cập nhật: người dùng trong phiên, 12/11/2026 (Asia/Bangkok), prompt “that is fine, change the status to approve”; căn cứ PRM-013 trong [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật: 12/11/2026, Asia/Bangkok; tiếp nhận BA sau review phân tích R002, PRM-012. Giữ nguyên baseline/tag lịch sử.
- Phê duyệt lịch sử: bản baseline ngày 07/11/2026 được người dùng duyệt tại PRM-008; bản cập nhật ngày 12/11/2026 có xác nhận riêng tại PRM-013.
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf) trang 1 mục 1-3, trang 2 mục 4-5, trang 3 mục 6; [BA](../../analysis/payment-management/requirements-analysis.md); [PAY-DEC-001 đến PAY-DEC-015](../../analysis/payment-management/decisions.md).
- Đầu vào review: [R002 - phân tích BA đã đối chiếu](../../reviews/payment-management/R002/review-record.md); nguồn BA tại revised_commit được ghi trong tài liệu bàn giao. Không thực hiện review mới trong lượt Design.

## 1. AS-IS có bằng chứng

Khảo sát AS-IS ban đầu ngày 07/11/2026 bằng `rg --files --hidden` và đọc các đường dẫn bên dưới trong repository module:

| Đã kiểm tra | Kết quả |
| --- | --- |
| `AGENTS.md`, `README.md`, `.cursor/rules/`, `.cursor/agents/design.md` | Hướng dẫn, workflow, giới hạn vai trò; không phải mã ứng dụng |
| `docs/requirements/btcohong.pdf` | PDF nguồn 3 trang; không còn ở đường dẫn gốc cũ, giữ nguyên vị trí thực tế |
| `docs/analysis/payment-management/` | requirements-analysis.md, issues.md, decisions.md; đủ 14 quyết định đã xác nhận |
| `docs/design/payment-management/README.md` trước lượt này | Chỉ có hướng dẫn nơi lưu; chưa có HLD/LLD/API/schema/UML |
| `docs/reviews/` | README và biểu mẫu; chưa có vòng review thực tế |
| Danh sách file toàn module, kể cả file ẩn ngoài `.git` | Không có project ứng dụng, component/API/domain/persistence/auth/transaction/audit/tests đã triển khai |

**Chưa có mã nguồn ứng dụng được cung cấp trong workspace** của module `module_thanh_toan`. Thư mục dự án mẫu bên cạnh có `Qtkd.Api.csproj` thuộc `QTKD-Agent-Seminar/QTKD-Agent-Seminar/`; không thuộc module, không khảo sát như triển khai thanh toán hoặc coi là thành phần tái sử dụng. Database, nhà cung cấp định danh, kênh thông báo và API nguồn thực tế đều chưa biết. Toàn bộ thành phần ở mục 2 là **cần thêm**, các adapter ngoài module là **cần tích hợp/chưa có hợp đồng thực tế**.

## 2. TO-BE: một module phân lớp

**Đề xuất kỹ thuật ADR-001:** chạy một ứng dụng module phân lớp cùng một kho dữ liệu giao dịch quan hệ. Worker thông báo dùng cùng mã triển khai/kho dữ liệu, có thể chạy nền trong tiến trình; không cần microservices, message broker hoặc CQRS. Lựa chọn framework/ngôn ngữ/vendor DB không bị ràng buộc bởi template; triển khai phải đáp ứng các năng lực ở tài liệu dữ liệu.

| Thành phần | Trách nhiệm | Phụ thuộc |
| --- | --- | --- |
| PaymentWeb | Hiển thị Draft/nguồn khác biệt/danh sách chờ; không tính tổng độc lập; giữ Idempotency-Key cho retry cùng ý định | PaymentApi |
| PaymentApi | Xác thực, ràng buộc dữ liệu, ETag, ánh xạ lỗi; không nhận userId/vai trò từ body | PaymentService, ActorContext |
| PaymentService | Điều phối lập/sửa/cập nhật/trình/duyệt/hủy/điều chỉnh/phân công | Domain, SourceGateway, AssignmentDirectory, UnitOfWork |
| PaymentDocument, PaymentChain | Guard trạng thái, bất biến Approved, lần trình, chuỗi điều chỉnh và `hasProcessingAdjustment()` xét toàn chuỗi | MoneyCalculator, ApprovalPolicy |
| SourceGateway | Tập hợp nguồn hợp đồng/giá/sản lượng chỉ đọc, kiểm tra nhất quán và so sánh căn cứ | Các adapter theo hợp đồng giao tiếp thực tế |
| AssignmentDirectory | Trả người phụ trách, quyền xem, người duyệt chính/thay thế/căn cứ ủy quyền có phiên bản | Nguồn phân quyền do đơn vị cung cấp |
| PaymentRepository, UnitOfWork | Dữ liệu bảng/dòng/căn cứ/chuỗi, khóa, kiểm tra version, audit và outbox trong một transaction | DB quan hệ giao dịch |
| NotificationDispatcher | Gửi sau commit, kiểm tra người nhận hiện hành, retry và chuyển vận hành | Outbox, NotificationChannel |
| EvidenceStore | Lưu/đọc biên bản bất biến theo mã và checksum, kiểm soát truy cập | Kho tệp nội bộ được cấu hình |

Luồng phụ thuộc đi từ Web → API → Application → Domain; lớp tích hợp thực thi các interface do Application định nghĩa. Domain không gọi HTTP/DB hoặc tự gửi thông báo. Lý do: dễ kiểm chứng quy tắc tài chính bằng dữ liệu giả lập, thay cách đọc nguồn mà không đổi nghiệp vụ.

## 3. Ranh giới tích hợp và căn cứ lịch sử

`SourceGateway.load(contractId, period, mode)` trả `SourceBundle`: mã hợp đồng/khách hàng/đơn vị, thời gian hiệu lực, trạng thái, lịch sử cấu hình kỳ, dịch vụ bắt buộc theo khoảng thời gian, sản lượng đã đối soát với phân bổ dịch vụ/ngày, giá/ưu tiên/hiệu lực/đơn vị tính, mã chứng cứ, vector phiên bản và dấu nhất quán. Không ghi dữ liệu về ba module nguồn.

| Nguồn | Dữ liệu bắt buộc | Cách xử lý thiếu |
| --- | --- | --- |
| Hợp đồng | Active; lịch sử kỳ; tháng/quý; kỳ lẻ được phép và phân bổ; danh sách bắt buộc có dấu xác nhận kể cả rỗng | Chặn tạo/cập nhật nếu không đủ căn cứ; trước trình chặn dịch vụ chưa xác định |
| Sản lượng | Đối soát hai bên, số lượng chính xác, phân bổ theo thời gian đổi giá; số 0 có xác nhận | Không thay missing bằng 0; thiếu phân bổ thì không chia đều |
| Bảng giá | Bảng được hợp đồng chỉ định, dịch vụ/đơn vị, giá theo ngày dịch vụ, ưu tiên | 0 hoặc nhiều ứng viên cùng ưu tiên → lỗi dữ liệu nguồn |
| Phân quyền | Subject bất biến, đơn vị, quyền xem riêng, phụ trách, người duyệt chính/thay thế và ủy quyền | Không có phân công hợp lệ → không cho trình/duyệt; không suy quyền từ vai trò đơn lẻ |

**ADR-002:** lưu bản chụp căn cứ bất biến cùng từng revision nội dung. Tạo canonical payload từ các trường ảnh hưởng nghĩa vụ, thời gian, số lượng, giá, tính hợp lệ; sắp theo khóa ổn định, chuẩn hóa decimal và ngày, hash SHA-256. Giữ payload và mã nguồn gốc để giải thích chênh lệch; hash không thay thế chứng cứ. Bỏ trường mô tả thuần túy khỏi hash tính tiền. Đối soát lại so sánh cả dữ liệu hợp lệ lẫn hash, không chỉ tổng tiền.

Đọc nguồn ngoài transaction DB cục bộ để không giữ khóa trong lúc chờ mạng. Adapter phải cung cấp bundle nhất quán theo snapshot/version được nguồn hỗ trợ; kiểm tra phiên bản trước/sau và phát hiện tham chiếu lệch. Đọc hai lần không tự chứng minh snapshot xuyên hệ thống. Nếu nguồn độc lập không cung cấp đủ căn cứ nhất quán thì trả `SOURCE_INCONSISTENT` và chặn, cần hoàn tất DEP-01 trước tích hợp thật. Kiểm tra nguồn xác nhận tính hợp lệ **tại thời điểm đọc đã ghi nhận**, không bảo đảm nguồn không thay đổi ngay sau đó. Không cam kết giao dịch phân tán hoặc khóa dữ liệu nguồn. Nếu cần bảo đảm đồng thời tuyệt đối với nguồn, phải bổ sung giao thức phối hợp được chủ quản nguồn chấp thuận.

Tại trình/duyệt ghi thời điểm kiểm tra và vector nguồn; trong transaction kiểm tra revision/version bảng chưa đổi từ lúc đọc. Mở xem hoặc sửa ghi chú không đọc lại để đổi tiền. Preview cập nhật giữ candidate bất biến, accept phải kiểm tra lại nguồn vẫn đúng candidate; nếu đổi thì trả conflict, yêu cầu preview mới. Approved giữ nguyên mọi căn cứ tài chính.

## 4. Giao dịch, đồng thời và chống trùng

| Thao tác | Một transaction DB nội bộ |
| --- | --- |
| Lập bảng gốc | Ghi document/revision/lines/snapshot + chain + claim từng tháng + audit + kết quả idempotency |
| Lập điều chỉnh | Khóa chain, kiểm tra bản Approved hiện hành và một hồ sơ đang xử lý; ghi hồ sơ/revision/snapshot/audit, không thêm month claim |
| Sửa/accept nguồn | Khóa chain rồi document, so version, tạo revision bất biến mới + audit; không cập nhật Approved |
| Trình | Cùng thứ tự khóa, kiểm tra version/phân công, tạo submission cố định + người nhận + PendingApproval + audit + outbox |
| Duyệt/từ chối | Khóa chain/document/submission, kiểm tra người nhận/version/guard; ghi một kết quả + audit, dừng outbox; duyệt cập nhật con trỏ currentApprovedDocumentId của chain |
| Hủy/mở lại/phân công | Thay trạng thái/quyền + version + audit; hủy bảng gốc xóa claim hoạt động, giữ lịch sử audit; thay người duyệt vô hiệu outbox cũ và ghi outbox mới cùng lần trình |

**ADR-003:** claim có PK `(contractId, calendarMonth)` là trọng tài chống chồng kỳ, kể cả lập quý và tháng đồng thời. **ADR-004:** version tăng theo lệnh thay đổi, `If-Match` tránh ghi đè, khóa chain trước document theo thứ tự cố định. Guard được kiểm lại sau khóa; không chỉ khóa ở UI. Submission duy nhất theo `(documentId, revisionId)`; PendingApproval trình lặp trả submission hiện hành, không chèn outbox mới. Xem chi tiết thứ tự kiểm tra retry ở API.

Mỗi lệnh tạo/thay đổi có Idempotency-Key theo subject + method + route + key; lưu fingerprint body/precondition và kết quả cùng transaction. Cùng key/khác ý định → 409. Mất phản hồi sau commit → retry cùng key nhận cùng kết quả. Không dùng key thay PK kỳ hay kiểm tra trạng thái; hai key khác vẫn chịu guard. DB deadlock/version conflict trả lỗi retry có kiểm soát; không tự chạy lại việc tính tiền bằng nguồn mới.

## 5. Bảo mật, audit và lỗi

**Đề xuất kỹ thuật:** host xác thực danh tính và cung cấp ActorContext đáng tin cậy; truyền qua TLS trong môi trường triển khai. Mọi truy cập kiểm tra đơn vị/quyền xem tài liệu; không nhận danh tính người lập từ client. Endpoint lệnh kiểm tra thêm quyền thao tác/phân công. Biên bản chỉ được truy cập theo cùng quyền hồ sơ; mã chứng cứ không cho phép tải tùy ý đường dẫn/URL bên ngoài. Không đưa toàn bộ payload nhạy cảm vào log kỹ thuật.

Audit append-only, timestamp UTC do server cấp (UI hiển thị Asia/Bangkok), actor subject, action, document/chain/revision/submission, trạng thái trước-sau, diff, correlationId. Role ứng dụng không được UPDATE/DELETE audit. Audit nghiệp vụ thành công phải commit cùng dữ liệu; ghi audit thất bại thì rollback lệnh. Lỗi xác thực/validation/conflict đi vào log kỹ thuật riêng, không giả là thay đổi thành công. Chưa đặt retention nghiệp vụ mới; DEP-05 phải chốt vận hành backup/quyền quản trị/retention theo tổ chức trước production.

Lỗi nguồn/thiếu phân công không tạo PendingApproval. Nguồn đổi trước duyệt → 409, giữ chờ, người được giao chủ động từ chối có lý do; nguồn không đọc được → 503, giữ chờ, không tự từ chối. UI trình bày lỗi với mã, nội dung thiếu, hành động khắc phục và correlationId, không lộ stack trace.

## 6. Thông báo và ranh giới kế toán

**ADR-005:** outbox cục bộ giải quyết khoảng hở giữa commit trình và gửi. Worker gửi lần đầu sau commit; nếu lỗi đầu ở t0, thử lại tại t0+1 phút, +5 phút, +14 phút, tối đa 3 lần trong 15 phút. Đây là lịch kỹ thuật đề xuất thực hiện PAY-DEC-014. Worker chạy lại sau hạn thì chuyển `NeedsAttention`, không gửi bù tự động ngoài cửa sổ. Claim/lease outbox tránh hai worker xử lý cùng lúc; lỗi sau gửi trước ghi nhận vẫn có thể giao lặp.

Trước mỗi lần gửi kiểm tra document còn PendingApproval, submission hiện hành và assignmentVersion/recipient còn khớp. Thay người tạo thông báo cho người mới cùng submission, dừng việc gửi cũ. Việc gửi đang trên đường truyền không thể thu hồi; link luôn kiểm quyền/assignment hiện hành, có cùng mã submission để người nhận nhận biết trùng. Khi đã xử lý, không tiếp tục lên lịch thông báo cũ. Inbox lấy từ submission DB, không phụ thuộc kênh ngoài. Vận hành xem mục NeedsAttention và yêu cầu một lượt Manual cùng tham chiếu; lượt này không chịu cửa sổ tự động đã hết và không reset số lần tự động, lỗi tiếp thì về NeedsAttention. Mỗi lần gửi có lịch sử.

PAY-DEC-013/015: màn hình đọc hồ sơ có thông tin để KTT đối chiếu; không thêm API bàn giao/xuất file hoặc trạng thái hóa đơn. Hiển thị bản Approved hiện hành, tổng cũ/mới/chênh lệch/biên bản, cảnh báo hồ sơ điều chỉnh đang xử lý để tạm dừng bàn giao mới. Bằng chứng gửi/nhận lưu ngoài module theo quy trình đơn vị; Approved không chứng minh đã bàn giao.

## 7. Hiệu năng và giới hạn

Đọc nguồn theo lô theo hợp đồng/kỳ, lấy giá sau khi biết bảng hợp đồng chỉ định; không gọi mỗi dòng một lần. Index kỳ, người nhận/trạng thái, audit theo bảng. Tính chính xác trong bộ nhớ rồi ghi theo lô một transaction; không gọi mạng dưới khóa DB. Cache chỉ được dùng nếu xác minh version/hiệu lực; không dùng cache cũ vượt qua kiểm tra nguồn trước trình/duyệt.

Đo đầu-cuối theo PAY-DEC-010, ghi thời gian đọc nguồn/tính/lưu/hiển thị để tìm nút thắt; tiêu chí vẫn là tổng từng lượt ≤3 giây trong tập dưới 200 dòng. Không khẳng định kiến trúc tự đáp ứng NFR khi chưa có môi trường và phép đo thật. Chi tiết kịch bản ở [bàn giao](08-handoff-to-developer.md).

## 8. Đối chiếu kiến trúc sau BA R002

Khảo sát lại ngày 12/11/2026: module đã có bộ thiết kế baseline và BA sửa, vẫn chưa có mã ứng dụng/API/DB/tests. Các dòng AS-IS về README thiết kế trống và chưa có vòng review ở mục 1 là ảnh chụp khảo sát ban đầu, không mô tả hiện tại. R002 hiện đã đối chiếu **phân tích BA**, không là review kiến trúc này.

PAY-DEC-015 xác nhận cơ chế đã được đề xuất ở ADR-006: `PaymentChain.hasProcessingAdjustment()` xét mọi Adjustment thuộc chain có status trong {Draft, PendingApproval, Rejected}. `activeAdjustmentId` phải khớp đúng kết quả này; không kiểm riêng trạng thái bản đang được xem. Tạo điều chỉnh kiểm guard dưới khóa chain, chặn nếu có hồ sơ đang xử lý; approve/cancel chỉ kết thúc vị trí của chính hồ sơ được quyết định, không xóa dấu giữ vị trí của hồ sơ khác.

API đọc A02 trả `activeAdjustmentId`, `currentApprovedDocumentId`, `handoffBlockedByAdjustment` từ cùng snapshot đọc của chain, kể cả khi mở Original hoặc Adjustment Approved/Cancelled cũ. Nếu có hồ sơ mới đang xử lý thì cờ vẫn true. Cờ là thông tin phục vụ quy trình thủ công, không chứng nhận đã bàn giao hoặc giữ chỗ cho thao tác ngoài module; KTT phải đối chiếu trạng thái hiện hành trước gửi. Không bổ sung endpoint bàn giao hoặc trạng thái mới.

Bốn AC ngoại lệ nguồn sau R002 dùng nguyên SourceGateway/snapshot/preview/accept và các lỗi hiện có; yêu cầu bổ sung là truy vết và kiểm chứng rõ việc giữ tiền/căn cứ khi lệnh bị chặn. Không cần thay kiến trúc phân lớp, kênh tích hợp hoặc schema cho bảy AC mới.
