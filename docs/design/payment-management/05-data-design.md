# 05. Thiết kế dữ liệu và giao dịch

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt bản cập nhật: người dùng trong phiên, 12/11/2026 (Asia/Bangkok), prompt “that is fine, change the status to approve”; căn cứ PRM-013 trong [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật: 12/11/2026, Asia/Bangkok; tiếp nhận BA sau review phân tích R002, PRM-012. Giữ nguyên baseline/tag lịch sử.
- Phê duyệt lịch sử: bản baseline ngày 07/11/2026 được người dùng duyệt tại PRM-008; bản cập nhật ngày 12/11/2026 có xác nhận riêng tại PRM-013.
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf) trang 1 mục 1-3, trang 2 mục 4-5, trang 3 mục 6; [BA](../../analysis/payment-management/requirements-analysis.md); [PAY-DEC-001 đến PAY-DEC-015](../../analysis/payment-management/decisions.md).
- Đầu vào review: [R002 - phân tích BA đã đối chiếu](../../reviews/payment-management/R002/review-record.md); nguồn BA tại revised_commit được ghi trong tài liệu bàn giao. Không thực hiện review mới trong lượt Design.

## 1. Kiểu và quan hệ logic

Chọn kho quan hệ có transaction ACID, unique/check/FK, khóa hàng, exact numeric và dữ liệu JSON/text UTF-8. `Id`: mã opaque bất biến; `SubjectId/ExternalId`: mã từ nguồn, chỉ FK logic ngoài module; `Instant`: UTC; `Date`: ngày nghiệp vụ; `Month`: ngày đầu tháng; `Version`: số nguyên dương tăng theo lệnh. `ExactDecimal`: số thập phân chính xác có độ dài biến thiên; `WholeVnd`: số nguyên chính xác. Lưu NUMERIC chính xác không hạn chế precision ở mô hình logic, tuyệt đối không float/money nhị phân. Triển khai phải kiểm tra DB/thư viện hỗ trợ precision cần thiết, báo lỗi dung lượng rõ ràng thay vì tràn/cắt số; không biến giới hạn thư viện thành quy tắc nghiệp vụ mới. DEP-01/04 bao gồm kiểm chứng độ lớn số thật từ nguồn trước chọn kiểu vật lý.

Quantity/unitPrice có scale tối đa 6 chữ số có nghĩa; exactAmount/exactTotal có thể có scale 12; amount/officialTotal/delta scale 0. SUM dùng độ chính xác mở rộng, không mất phần lẻ/overflow. Canonical decimal qua JSON là string, bỏ 0 cuối nhưng không dùng scientific notation. Ngày serviceFrom/serviceTo ở API là hai đầu bao gồm; bộ tính chuyển sang khoảng nửa mở `[from, to+1 ngày)` để không tính trùng ngày đổi giá.

| Quan hệ | Cardinality và ý nghĩa |
| --- | --- |
| PaymentChain → PaymentDocument | 1 → 1..*; một gốc Original và các Adjustment, gốc có thể chưa Approved |
| PaymentDocument → PaymentRevision | 1 → 1..*; currentRevisionId chỉ đúng revision thuộc document |
| PaymentRevision → PaymentLine | 1 → 0..*; lineKey duy nhất trong revision; thiếu dịch vụ bị chặn trình, không tự cấm tổng 0 |
| PaymentRevision → SourceSnapshot | N → 1; dùng lại snapshot khi chỉ sửa ghi chú; snapshot bất biến |
| PaymentDocument → Submission | 1 → 0..*; mỗi submission cố định một revision cùng document |
| Original → MonthClaim | 1 → 1 hoặc 3 khi chưa Cancelled; điều chỉnh không có claim riêng |
| Adjustment → base Approved document | N → 1 cùng chain; base phải là bản hiện hành lúc tạo/duyệt |
| PaymentRevision → Evidence | N → 0..1; điều chỉnh bắt buộc trước trình; ngoại lệ hợp đồng kết thúc cần ngay khi tạo |
| Submission → Notification | 1 → 0..* theo assignmentVersion; chỉ một bản ghi mục đích yêu cầu duyệt cho mỗi assignment |
| Notification → NotificationAttempt | 1 → 0..*; giữ lịch sử gửi/lỗi, tự động và thủ công |

Đây là mô hình dữ liệu quan hệ; [class.puml](diagrams/class.puml) là UML hành vi/cấu trúc lớp, không gọi là ERD.

## 2. Từ điển entity và constraint

| Entity; PK | Thuộc tính chính và kiểu | FK/unique/check; invariant |
| --- | --- | --- |
| PaymentChain; id:Id | rootDocumentId:Id, contractId:ExternalId, unitId:ExternalId, currentApprovedDocumentId:Id?, activeAdjustmentId:Id?, version:Version | root duy nhất; con trỏ cùng chain/đúng kind; currentApproved phải Approved; activeAdjustment chỉ Draft/PendingApproval/Rejected; invariant kiểm tra trong TX khóa chain |
| PaymentDocument; id:Id | chainId:Id, kind:enum Original/Adjustment, contractId/customerId/unitId:ExternalId, periodType:Month/Quarter, year:int, number:int, serviceFrom/To:Date, originalCreatorId/createdBy/responsibleId:SubjectId, status:enum năm trạng thái, currentRevisionId:Id, currentSubmissionId:Id?, baseApprovedDocumentId:Id?, replacementOfDocumentId:Id?, replacementReason:text?, cancellationReason:text?, createdAt:Instant, version:Version | FK chain, base/replacement nội bộ; kind Original có base null, Adjustment có base cùng chain; period number đúng type; serviceFrom≤serviceTo; identity/contract/period bất biến sau tạo. replacement phải Cancelled cùng hợp đồng, lý do bắt buộc; rule kiểm tra TX |
| PaymentRevision; id:Id | documentId:Id, revisionNo:int, snapshotId:Id, note:text, exactTotal:ExactDecimal, officialTotal:WholeVnd, adjustmentReason:text?, evidenceId:Id?, delta:WholeVnd?, createdBy:SubjectId, createdAt:Instant | FK document/snapshot/evidence; unique(documentId,revisionNo), unique(documentId,id) cho FK ghép; nội dung append-only. delta chỉ Adjustment, bằng tổng mới trừ tổng base. Không check tổng≥0 toàn bảng vì Draft được giữ tổng âm để sửa nguồn |
| PaymentLine; (revisionId,lineKey) | serviceId:ExternalId, unit:text, sourceQuantityId/priceId:ExternalId, serviceFrom/To:Date, quantity/unitPrice:ExactDecimal, exactAmount:ExactDecimal, amount:WholeVnd, note:text | FK revision; scale đầu vào≤6; khoảng trong kỳ dịch vụ; exactAmount=quantity×unitPrice, amount=HALF_UP(exactAmount,0) kiểm bằng calculator và validation trước persist; khóa dòng ổn định theo chứng cứ nguồn+đoạn giá |
| SourceSnapshot; id:Id | contractId:ExternalId, period:JSON, payload:JSON/text, semanticHash:text SHA-256, sourceVersions:JSON, observedAt:Instant, consistencyProof:JSON | Bất biến; hash không unique vì có thể trùng payload; giữ cả mã nguồn/chứng cứ/lịch sử cấu hình, không chỉ hash; không FK vật lý sang DB ngoài |
| MonthClaim; (contractId,calendarMonth) | rootDocumentId:Id, acquiredAt:Instant | FK bảng gốc; PK chống mọi kỳ chồng; chỉ tháng đầu kỳ của lịch dương; root chưa Cancelled. Claim của quý ghi đủ ba hàng trong cùng TX; không xóa khi Approved/Rejected |
| Submission; id:Id | documentId/revisionId:Id, roundNo:int, requiredRole:KTT/GDKD, assignedTo:SubjectId, assignmentVersion:Version, assignmentAuthorityRef:text, assignmentDirectoryVersion:text, state:Open/Approved/Rejected, submittedBy:SubjectId, submittedAt:Instant, decidedBy:SubjectId?, decidedAt:Instant?, reason:text? | FK ghép(documentId,revisionId)→revision; unique(documentId,roundNo), unique(documentId,revisionId). Open không có quyết định; Rejected có trim(reason) khác rỗng; Approved/Rejected có decidedBy/At. Một Open/document bằng unique có điều kiện hoặc khóa document+currentSubmission invariant |
| AssignmentHistory; id:Id | documentId:Id, submissionId:Id?, kind:Responsibility/Approver, previousSubject/newSubject:SubjectId, role:text, authorityRef/reason:text, actorId:SubjectId, at:Instant, assignmentVersion:Version? | FK document/submission; before/after/căn cứ không rỗng; append-only; thay phụ trách không sửa createdBy/originalCreatorId; thay approver cùng requiredRole |
| Evidence; id:Id | unitId:ExternalId, documentCode:text, documentDate:Date, confirmedParties:JSON array, blobRef:text, mediaType:text, sizeBytes:int, checksum:text SHA-256, registeredBy:SubjectId, registeredAt:Instant | Metadata đầy đủ; blob bất biến/không rỗng, checksum đối chiếu khi đọc; giới hạn upload kỹ thuật tại API; documentCode không tự coi unique toàn tổ chức |
| AuditEvent; id:Id | resourceType:text, resourceId:Id, chainId/documentId:Id?, revisionId/submissionId:Id?, actorId:SubjectId, action:text, at:Instant, beforeStatus/afterStatus:enum?, diff:JSON, correlationId:text | FK nội bộ tùy sự kiện; thay đổi hồ sơ bắt buộc documentId/chainId; EvidenceRegistered trước khi gắn hồ sơ dùng resourceType=Evidence/resourceId, không giả FK document. Append-only, không xóa dây chuyền; gồm trước-sau, không chỉ message |
| Notification; id:Id | submissionId:Id, assignmentVersion:Version, recipientId:SubjectId, purpose:ApprovalRequested, state:Queued/Delivered/Retrying/NeedsAttention/Obsolete, pendingSendKind:Initial/Automatic/Manual, manualRequestedBy:SubjectId?, manualReason:text?, automaticRetries:int, firstFailureAt/nextAttemptAt:Instant?, leaseOwner:text?, leaseUntil:Instant?, version:Version | FK submission; unique(submissionId,assignmentVersion,purpose); automaticRetries 0..3; Manual là một lượt được vận hành yêu cầu, không reset ngân sách tự động; outbox và trạng thái gửi cùng entity |
| NotificationAttempt; id:Id | notificationId:Id, attemptNo:int, kind:Initial/Automatic/Manual, requestedBy:SubjectId?, startedAt/finishedAt:Instant?, result:Delivered/Failed/Unknown, errorCode/providerRef:text? | FK notification; unique(notificationId,attemptNo); append lịch sử; lỗi process sau gửi ghi Unknown khi khôi phục, không cam kết exactly-once ngoài hệ thống |
| IdempotencyReceipt; (actorId,method,route,key) | fingerprint:text, responseStatus:int, responseBody:JSON, responseETag:text?, resourceId:Id?, completedAt:Instant | Unique scope key; insert/hoàn tất cùng transaction mutation; retry chờ transaction thắng, mismatch trả conflict; kiểm quyền trước replay |
| RefreshCandidate; token:Id | actorId:SubjectId, documentId:Id, baseVersion:Version, snapshotId:Id, calculatedPayload/diff/unmappedLineNotes:JSON, expiresAt:Instant, consumedAt:Instant? | Gắn actor/document/version; hạn 15 phút; accept kiểm nguồn còn khớp; chỉ candidate phụ trợ, không là revision chính thức cho tới commit |

Các trường reference đến người/hợp đồng là logical reference kèm snapshot mô tả cần thiết, không giả định cùng DB. Quyền phụ trách đọc từ nguồn hợp đồng khi tạo; chuyển giao trong module lưu authorityRef và lịch sử, kiểm tra tính hiệu lực với AssignmentDirectory. Revision giữ ghi chú/biên bản; không cập nhật inplace để vừa có lịch sử vừa khóa nội dung đã trình.

Vòng FK lúc tạo chain/document/currentRevision xử lý bằng FK trì hoãn đến commit nếu DB hỗ trợ, hoặc tạo parent với con trỏ tạm null chỉ trong transaction rồi gán trước commit, repository bắt buộc xác minh invariant. Không để trạng thái nửa tạo lộ ra ngoài transaction. currentApprovedDocumentId là metadata của chain, không là sửa tiền/trạng thái bản Approved cũ.

## 3. Index, audit và bất biến liên bảng

| Index/constraint đề xuất | Mục tiêu |
| --- | --- |
| MonthClaim PK(contractId,calendarMonth) | Chống duplicate gốc/tháng/quý ở DB, không dùng unique(contractId,periodType,periodNumber) vì sẽ lọt chồng tháng/quý |
| Document(chainId,createdAt), Document(unitId,status,createdAt,id), Document(responsibleId,status,id) | Lịch sử chuỗi, quyền đơn vị và danh sách xử lý |
| Submission(assignedTo,state,submittedAt,id) | Inbox độc lập thông báo |
| Revision(documentId,revisionNo) unique; Line(revisionId,lineKey) PK | Đọc đúng nội dung mỗi lần trình và lịch sử |
| Notification(state,nextAttemptAt,leaseUntil,id) | Worker lấy công việc đến hạn; lease có version để tránh hai worker ghi cùng kết quả |
| AuditEvent(documentId,at,id), AssignmentHistory(documentId,at,id) | Đối chiếu lịch sử/cursor ổn định |

Những invariant không biểu diễn bằng CHECK một hàng (state con trỏ/độc lập danh tính/tổng bằng SUM/duy nhất điều chỉnh mở) được enforce bởi domain+repository trong transaction có khóa chain/document; test trực tiếp tầng repository phải kiểm tra. Nếu DB hỗ trợ constraint trigger có thể bổ sung nhưng không thay guard ứng dụng. Role truy cập DB thường chỉ cấp thông qua repository; không cấp UPDATE/DELETE cho revision/snapshot/audit, không cascade delete hồ sơ nghiệp vụ.

Lưu `originalCreatorId` của gốc trên toàn chain để kiểm PAY-07; với Adjustment còn loại `createdBy` của hồ sơ đó. Không thay các subject này khi bàn giao. Người nhận phải còn đúng vai trò/ủy quyền/assignment tại quyết định. `requiredRole` cố định theo tổng revision đã trình; reassign không được đổi role.

## 4. Định nghĩa transaction

Thứ tự khóa thống nhất: receipt key → chain (nếu đã có) → document → submission → notification. Khi claim nhiều tháng, chèn theo thứ tự tăng tháng; unique conflict rollback toàn TX. Đọc nguồn bên ngoài TX, kèm expected document/assignment versions, rồi kiểm lại dưới khóa. Version directory phải được adapter xác minh còn hiệu lực theo DEP-02; không tự cho phép quyền đã thu hồi.

| TX | Các ghi phải cùng commit | Rollback và cạnh tranh |
| --- | --- | --- |
| TX-CREATE | chain + gốc + revision/lines/snapshot + 1/3 claim + Created + receipt | Bất kỳ tháng bị chiếm → không có Draft dở dang; source lỗi trước TX không ghi claim |
| TX-EDIT | document.currentRevision/version + revision/lines/snapshot nếu mới + audit + receipt; accept tiêu thụ candidate | Version/semantic hash đổi → không accept một phần; note edit giữ tiền/căn cứ |
| TX-SUBMIT | submission + document.PendingApproval/currentSubmission/version + audit + Notification Queued + receipt | Lỗi bất kỳ ghi/audit → giữ Draft; nếu đã PendingApproval trả submission cũ không ghi outbox |
| TX-DECIDE | submission kết quả + document status/version + audit + outbox Obsolete + receipt; Approved điều chỉnh cập nhật chain.currentApproved và clear activeAdjustmentId chỉ nếu trỏ đúng hồ sơ đó; Rejected giữ vị trí | Tranh chấp approve/reject/reassign chỉ một kết quả; nguồn không đọc được không vào TX quyết định |
| TX-STATE | Rejected→Draft + revision sao chép mới giữ tiền/căn cứ + version + audit + receipt | Tạo revision mới ngay lúc reopen dù chưa sửa; submission cũ vẫn tham chiếu revision cũ, unique(documentId,revisionId) không chặn round mới |
| TX-CANCEL | Cancelled + version + lý do + audit + receipt; xóa claim gốc hoặc clear activeAdjustmentId chỉ nếu đang trỏ chính điều chỉnh bị hủy | Chỉ Draft; mọi thay đổi giải phóng kỳ/slot cùng commit, không xóa nội dung cũ |
| TX-ADJUST | khóa chain + kiểm base hiện hành + Adjustment/revision/lines/snapshot + chain.activeAdjustmentId + audit + receipt | Hai create chỉ một slot; Reject không giải phóng slot, Cancel/Approve mới giải phóng |
| TX-ASSIGN | phụ trách hoặc submission.assignedTo/assignmentVersion + document.version + AssignmentHistory + audit + receipt; đổi duyệt thêm outbox mới/dừng cũ | Người cũ hết quyền sau commit; root identity không đổi |
| TX-EVIDENCE | metadata Evidence + EvidenceRegistered + receipt sau ghi blob bền vững | Blob có trước nhưng metadata thất bại → blob mồ côi chờ dọn, không được attach; blob mất → chặn trình/duyệt điều chỉnh |
| TX-NOTIFY | lease/attempt/result/nextAttemptAt/version; retry thủ công đặt pendingSendKind=Manual, thêm receipt và audit vận hành | Manual được gửi một lượt dù cửa sổ tự động đã hết, không reset bộ đếm; lỗi lại về NeedsAttention. Gửi ngoài transaction, check trạng thái/assignment trước gửi; lỗi sau delivery có thể lặp cùng reference |

TX-STATE tạo ngay revision mới sao chép payload/căn cứ ở bước reopen (số revision tăng, số tiền giữ nguyên); currentRevision trỏ revision mới, Submission cũ vẫn trỏ revision cũ. Đây là lựa chọn kỹ thuật thống nhất cho việc trình lại không cần sửa ghi chú. Audit Reopened ghi cả revision trước/sau.

## 5. Lịch sử, hiệu lực và migration

Giá trị hiệu lực của chain = officialTotal ở `currentApprovedDocumentId`; không SUM tổng của gốc và các điều chỉnh. delta của điều chỉnh là thông tin đối chiếu; ví dụ 480→510 triệu là tổng hiệu lực 510, delta +30. Hủy Draft điều chỉnh chỉ clear slot, không xóa claim hoặc currentApproved. Hủy gốc chỉ có thể trước duyệt, claim được giải phóng; bảng thay thế có chain/mã mới và replacementOfDocumentId về bản Cancelled.

Migration là **tạo mới** toàn bộ schema module vì chưa có DB thực tế; không có migration dữ liệu cũ đã xác định. Khi triển khai: xác minh khả năng NUMERIC/khóa/FK, tạo parent-child/index/role DB, kiểm thử rollback và quyền append-only, nạp dữ liệu phân quyền qua adapter; không import hoặc sửa dự án mẫu bên cạnh. Sau này có dữ liệu thật phải dùng migration tiến, backup/khôi phục và kiểm tra invariant trước phát hành; không xóa lịch sử để đổi kỳ hoặc hợp thức hóa Approved.

DEP-05 phải có chính sách backup/khôi phục, retention và quyền quản trị audit/biên bản/idempotency trước production. Thiết kế không tự đặt số năm lưu hay tuyên bố đáp ứng pháp lý; không có yêu cầu nghiệp vụ nào được sửa để phù hợp schema.

## 6. Bất biến toàn chuỗi theo PAY-DEC-015 / R002-F001

Đặt `processingAdjustments` là tập mọi PaymentDocument kind=Adjustment thuộc chain có status Draft/PendingApproval/Rejected; số phần tử phải là 0 hoặc 1. `activeAdjustmentId` null khi tập rỗng, ngược lại bằng id phần tử duy nhất. `PaymentChain.hasProcessingAdjustment()` và cờ API handoffBlockedByAdjustment phải khớp tập này; không suy từ status của một hồ sơ Approved/Cancelled cũ.

- TX-ADJUST khóa chain và kiểm tập toàn chuỗi trước tạo; chỉ tạo dựa currentApproved hiện hành, giữ nguyên claim tháng gốc.
- TX-DECIDE reject và TX-STATE reopen giữ nguyên vị trí; reopen tạo revision mới nhưng không clear activeAdjustmentId.
- TX-DECIDE approve Adjustment và TX-CANCEL Adjustment chỉ clear vị trí khi con trỏ khớp chính hồ sơ được xử lý. Trường hợp không khớp là lỗi invariant/transaction, rollback và theo dõi vận hành; không xóa vị trí của hồ sơ khác hoặc tự sửa lịch sử.
- API A02 đọc chain và hồ sơ theo cùng snapshot nhất quán. Nếu có điều chỉnh mới B đang xử lý sau khi A Approved/Cancelled thì xem A vẫn phải trả activeAdjustmentId=B và cờ true. Cờ là suy diễn đọc, không thêm cột lưu trùng và không là khóa bàn giao ngoài module.

Không thêm entity, trạng thái, claim hay migration dữ liệu so thiết kế cũ; diễn giải lại invariant/concurrency với căn cứ nghiệp vụ đã xác nhận. Bốn nhánh F002 thất bại giữ currentRevisionId, snapshot, totals và status; không tạo revision/submission/claim mới hoặc audit thay đổi thành công. Log lỗi kỹ thuật/cảnh báo được phép như tài liệu 02.
