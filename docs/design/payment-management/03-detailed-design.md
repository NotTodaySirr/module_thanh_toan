# 03. Thiết kế chi tiết

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt bản cập nhật: người dùng trong phiên, 12/11/2026 (Asia/Bangkok), prompt “that is fine, change the status to approve”; căn cứ PRM-013 trong [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật: 12/11/2026, Asia/Bangkok; tiếp nhận BA sau review phân tích R002, PRM-012. Giữ nguyên baseline/tag lịch sử.
- Phê duyệt lịch sử: bản baseline ngày 07/11/2026 được người dùng duyệt tại PRM-008; bản cập nhật ngày 12/11/2026 có xác nhận riêng tại PRM-013.
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf) trang 1 mục 3, trang 2 mục 4-5, trang 3 mục 6; [BA và AC](../../analysis/payment-management/requirements-analysis.md); [PAY-DEC-001 đến PAY-DEC-015](../../analysis/payment-management/decisions.md).
- Đầu vào review: [R002 - phân tích BA đã đối chiếu](../../reviews/payment-management/R002/review-record.md); nguồn BA tại revised_commit được ghi trong tài liệu bàn giao. Không thực hiện review mới trong lượt Design.

## 1. Mô hình trách nhiệm và interface

| Class/interface | Thuộc tính/chữ ký chính | Trách nhiệm |
| --- | --- | --- |
| PaymentChain | id, rootDocumentId, currentApprovedDocumentId?, activeAdjustmentId?, version | Một chuỗi bảng gốc và điều chỉnh; `hasProcessingAdjustment(): Boolean` xét Draft/PendingApproval/Rejected toàn chuỗi, giữ con trỏ hiệu lực và serialize thay đổi |
| PaymentDocument | id, kind Original/Adjustment, chainId, baseApprovedDocumentId?, status, originalCreatorId, createdBy, responsibleId, currentRevisionId, currentSubmissionId?, version | `submit`, `approve`, `reject`, `reopen`, `cancel`; guard vòng đời và bất biến Approved |
| PaymentRevision | id, documentId, revisionNo, sourceSnapshotId, note, exactTotal, officialTotal, adjustmentReason?, evidenceId? | Nội dung bất biến sau lưu; mọi sửa Draft tạo revision mới, submission tham chiếu đúng revision |
| PaymentLine | lineKey, serviceId, sourceQuantityId, priceId, serviceFrom/To, quantity, unitPrice, exactAmount, amount, note | Một đoạn sản lượng có cùng hiệu lực giá; không tồn tại độc lập revision |
| SourceSnapshot | id, payload, semanticHash, sourceVersions, observedAt, consistencyProof | Giữ căn cứ có thể giải thích; kiểm tra khác biệt có ngữ nghĩa |
| Submission | id, revisionId, roundNo, assignedTo, requiredRole, assignmentVersion, state, submittedAt, decidedBy?, decidedAt?, reason? | Một lần trình; chỉ một kết quả quyết định; thay người không sinh round mới |
| MoneyCalculator | `calculate(bundle): CalculationResult` | Decimal chính xác; tích dòng, HALF_UP, tổng chưa làm tròn và tổng chính thức |
| ApprovalPolicy | `select(total, unit, excludedSubjects): Assignment` | Một vai trò theo ngưỡng, người chính rồi thay thế đủ cùng thẩm quyền |
| SourceGateway | `load(contractId, period, mode): SourceBundle`; `compare(snapshot,bundle): SourceDiff` | Chỉ đọc, xác thực bundle nhất quán và so sánh toàn bộ căn cứ liên quan |
| AssignmentDirectory | `authorize(actor,action,document)`; `resolveApprover(unit,role,excluded)` | Quyền xem độc lập; phân công/ủy quyền có phiên bản và căn cứ |
| PaymentRepository / UnitOfWork | `loadForUpdate`; `save`; `transaction(work)` | Khóa chain/document; kiểm tra version, constraints; persist audit/outbox cùng transaction |
| PaymentService | `createOriginal`, `editNotes`, `previewRefresh`, `acceptRefresh`, `submit`, `approve`, `reject`, `reopen`, `cancel`, `createAdjustment`, `reassignApprover`, `transferResponsibility` | Điều phối API → domain → repository; không lách guard ở lệnh phụ |
| NotificationDispatcher | `dispatch(outboxId)`; `retry(notificationId)` | Kiểm tra tính hiện hành, gửi và lưu attempt; chuyển vận hành khi hết lượt/hạn |
| EvidenceStore | `register(file, metadata): EvidenceRef`; `read(id)` | Biên bản có mã/ngày/các bên, checksum và blob bất biến; đọc có quyền |

Quan hệ UML ở [class.puml](diagrams/class.puml): chain liên kết 1..* document; document composition 1..* revision, 0..* submission; revision composition 0..* line (dữ liệu thiếu bắt buộc vẫn phải bị chặn trình); mỗi revision có đúng 1 snapshot, snapshot có thể dùng lại khi chỉ sửa ghi chú; điều chỉnh tham chiếu đúng 1 bản Approved cùng chain. Không dùng kế thừa chỉ để biểu diễn hai loại hồ sơ giống vòng đời. Mô hình bảng dữ liệu riêng ở tài liệu 05.

## 2. Tính tiền, kỳ và nhận biết nguồn đổi

1. Xác định kỳ tháng/quý dương lịch và khoảng dịch vụ thực tế; quý có ba month claim. Cấu hình mới chỉ áp dụng kỳ tương lai chưa bị chiếm; tháng → quý từ đầu quý tiếp theo, quý → tháng sau quý đã lập. Kỳ lẻ phải có quy định nguồn, thiếu thì chặn.
2. Kiểm tra nguồn hợp đồng Active cho bảng gốc; sản lượng đã đối soát; bảng giá được chỉ định; dịch vụ/đơn vị tính phù hợp. Lập điều chỉnh hợp đồng đã kết thúc chỉ dùng ngoại lệ PAY-DEC-012 khi kỳ dịch vụ thuộc thời gian hiệu lực, số liệu sửa được xác nhận và đủ biên bản.
3. Cắt đoạn theo giao của thời gian dịch vụ, phân bổ đối soát và hiệu lực giá; chọn duy nhất giá có ưu tiên đúng. Thiếu giá, giá mơ hồ hoặc không phân bổ được thì chặn; không chia đều sản lượng.
4. Mỗi quantity và unitPrice là decimal chính xác với tối đa 6 chữ số thập phân có nghĩa; không dùng binary float, không tự làm tròn đầu vào vượt giới hạn. `exactAmount = quantity × unitPrice`; `amount = HALF_UP(exactAmount, 0)`. Cộng exactAmount thành exactTotal; cộng amount thành officialTotal. Không VAT/phí/chiết khấu, không làm tròn tổng lần nữa.
5. Khi trình, `officialTotal >= 0` và `exactTotal >= 0`. Tổng 0 vẫn được trình nếu điều kiện khác đạt. Chênh lệch điều chỉnh có thể âm, nhưng hai tổng mới phải hợp lệ. Ngưỡng phân cấp chỉ dùng officialTotal.
6. Kiểm tra từng dịch vụ bắt buộc bao phủ đủ thời gian nguồn quy định. Danh sách được xác nhận rỗng khác missing/unavailable; sản lượng 0 phải là 0 đã đối soát. Không dùng bảng giá để suy ra danh sách bắt buộc.

Ví dụ: hai tích 100,5 → hai dòng 101 → tổng 202; -0,4 → amount 0 nhưng exactTotal âm nên chặn trình; hai tích 249.999.999,5 → 500.000.000, giao GĐKD. Giá đổi: 10 × 100.000 + 20 × 120.000 → hai dòng, tổng 3.400.000. Các ví dụ là căn cứ kiểm chứng thiết kế, chưa là kết quả chạy ứng dụng.

Semantic hash bao gồm trạng thái/hiệu lực hợp đồng, kỳ và lịch sử cấu hình áp dụng, danh sách bắt buộc/ngoại lệ, dịch vụ/đơn vị/thời gian/phân bổ/đối soát, bảng giá/ưu tiên/hiệu lực/giá và chứng cứ xác nhận. Loại mô tả thuần túy; canonical decimal bỏ số 0 cuối, khóa dòng sắp ổn định. Ngay cả tổng không đổi nhưng dịch vụ bắt buộc hoặc trạng thái đối soát đổi vẫn là thay đổi ảnh hưởng. Revision giữ snapshot lịch sử và output đầy đủ.

## 3. Bảng chuyển trạng thái và guard

Ký hiệu phụ trách: NVKD của bảng gốc hoặc KTT của điều chỉnh, kể cả người đã nhận bàn giao chính thức. Mọi hàng thành công ghi actor/thời điểm/trước-sau/action trong cùng transaction; hàng không đổi không tạo audit thay đổi giả.

| Từ | Hành động / actor | Guard | Đến / tác động | Audit; lỗi chính | Nguồn |
| --- | --- | --- | --- | --- | --- |
| Chưa có | createOriginal / NVKD | Đúng hợp đồng được giao, nguồn hợp lệ, kỳ đúng cấu hình, chưa bị chiếm | Draft; chain và claim tháng mới | Created; PERIOD_CONFLICT, SOURCE_INVALID | US-PAY-01; PAY-01/02/03; DEC-004/008/009/011/012 |
| Draft | editNotes / phụ trách | Đúng quyền/version, chỉ ghi chú chung/dòng hiện có | Draft; revision mới, tiền giữ nguyên | NotesEdited; FINANCIAL_FIELD_FORBIDDEN | US-PAY-01; DEC-006 |
| Draft | acceptRefresh / phụ trách | Đã preview/chấp nhận; nguồn vẫn khớp candidate, hợp đồng/kỳ bất biến | Draft; revision/căn cứ toàn bộ mới | SourcesAccepted; SOURCE_CHANGED, VERSION_CONFLICT | US-PAY-01/04; DEC-006/012 |
| Draft | submit / phụ trách | Nguồn hợp lệ/không đổi ảnh hưởng; đủ bắt buộc; hai tổng không âm; có người duyệt độc lập; điều chỉnh đủ biên bản | PendingApproval; submission cố định, outbox | Submitted; SOURCE_CHANGED, MISSING_REQUIRED_SERVICE, NEGATIVE_TOTAL, NO_ELIGIBLE_APPROVER, EVIDENCE_REQUIRED | US-PAY-02/04; PAY-04/06/07; DEC-001/002/005/009/011/012/014 |
| PendingApproval | submit / phụ trách | Quyền hiện hành; lần trình đang mở | Giữ nguyên, trả submission cũ; không outbox/audit mới | Không chuyển; không coi If-Match cũ là lý do tạo round | PAY-04; DEC-014 |
| PendingApproval | approve / người đang được giao | Đúng role theo tổng; không là người lập gốc/điều chỉnh; source recheck hợp lệ/không đổi; version đúng | Approved; đóng submission, cập nhật currentApproved, kết thúc activeAdjustment nếu có | Approved; SELF_APPROVAL, NOT_ASSIGNED, SOURCE_CHANGED, SOURCE_UNAVAILABLE | US-PAY-03; PAY-05/06/07; DEC-001/002/011/012 |
| PendingApproval | reject / người đang được giao | Đúng quyền/độc lập; lý do trim không rỗng; không cần nguồn đọc được | Rejected; giữ revision đã trình và lý do | Rejected; REASON_REQUIRED, SELF_APPROVAL, NOT_ASSIGNED | US-PAY-03; PAY-07/08; DEC-003/011 |
| Rejected | reopen / phụ trách | Đúng phân công/version | Draft; sao chép revision mới giữ nguyên tiền/căn cứ và lịch sử submission cũ | Reopened; INVALID_STATE | US-PAY-01/03/04; DEC-003 |
| Draft | cancel / phụ trách | Lý do trim không rỗng; đúng quyền/version | Cancelled; bảng gốc giải phóng claim, điều chỉnh giải phóng activeAdjustment nhưng giữ claim gốc | Cancelled; REASON_REQUIRED, INVALID_STATE | US-PAY-05; DEC-004/007/011 |
| Approved | createAdjustment / KTT | Bản hiện hành, đúng đơn vị/phân công; chưa có điều chỉnh Draft/PendingApproval/Rejected; nguồn sửa hợp lệ | Approved gốc giữ nguyên; hồ sơ khác Draft, tổng đầy đủ mới | AdjustmentCreated; ADJUSTMENT_IN_PROGRESS, STALE_BASE | US-PAY-04; PAY-05; DEC-001/012 |
| PendingApproval | reassignApprover / đầu mối | Cùng vai trò, độc lập, căn cứ bàn giao; version đúng | Giữ chờ; tăng assignmentVersion, outbox người mới; người cũ mất quyền | ApproverReassigned; ASSIGNMENT_INVALID | US-PAY-03; DEC-011/014 |
| Bất kỳ có hồ sơ | transferResponsibility / đầu mối | Cùng đơn vị/loại phụ trách, có căn cứ bàn giao; không đổi người lập hoặc nội dung | Giữ trạng thái; thay responsibleId và lịch sử phân công | ResponsibilityTransferred; ASSIGNMENT_INVALID | DEC-007/011; NFR-PAY-01 |
| PendingApproval | retry thông báo / worker/vận hành | Còn cùng submission/assignment; đúng giới hạn với tự động | Giữ chờ, thêm attempt | Lịch sử gửi; không có audit chuyển trạng thái | US-PAY-02; DEC-014 |
| Cancelled | sửa/trình/phục hồi | Luôn cấm | Không đổi | INVALID_STATE | US-PAY-05; DEC-007 |

`DEC-xxx` trong bảng là viết tắt của `PAY-DEC-xxx` trong sổ xác nhận. Approved không có đường sửa/hủy/reopen; chỉ tạo đối tượng điều chỉnh mới. Rejected không hủy hoặc trình trực tiếp. Hồ sơ điều chỉnh Rejected vẫn chiếm vị trí đang xử lý của chain cho tới Approved hoặc Cancelled; không bỏ qua bằng cách lập hồ sơ thứ hai. Căn cứ bổ sung hiện hành là PAY-DEC-015 sau R002-F001, không gán ngược cho PRM-006.

## 4. Luồng xử lý và lỗi

### Lập, sửa và cập nhật nguồn

API xác thực/phân quyền trước đọc nguồn. `createOriginal` tính toàn bộ bundle, rồi transaction khóa/claim tháng và lưu Draft; conflict rollback toàn bộ. Hai tháng/quý giao nhau chỉ một yêu cầu chiếm được tháng tranh chấp. Lập thay thế cần id bảng Cancelled cùng hợp đồng, lý do lập lại; tạo mã mới, liên kết lịch sử, vẫn kiểm tra cấu hình hiện hành.

Sửa ghi chú chỉ tạo revision sao chép tiền/căn cứ, không đọc/tính lại. `previewRefresh` không đổi document, lưu candidate có token gắn actor/document/baseVersion/semanticHash/diff trong 15 phút (đề xuất kỹ thuật). Accept đọc lại và xác nhận hash candidate; không khớp thì bắt preview lại. Khi tách/gộp dòng, chỉ giữ note trên lineKey còn khớp; UI hiển thị note không còn ánh xạ và yêu cầu ghi chú lại trước accept, không âm thầm gán sang dịch vụ khác. Candidate hết hạn/đã dùng không được áp lần nữa; replay đúng idempotency vẫn trả kết quả cũ.

### Trình và thông báo

Kiểm tra quyền và PendingApproval lặp trước khi đọc lại nguồn. Với Draft, đọc nguồn và phân công, kiểm tra snapshot/guard. Transaction khóa chain/document, so version và phân công, ghi submission + trạng thái + audit + outbox. Sau commit UI hiển thị trạng thái gửi thực tế `Queued/Delivered/Retrying/NeedsAttention`, không báo gửi thành công chỉ vì đã trình. Worker xử lý như tài liệu 02. [sequence.puml](diagrams/sequence.puml) mô tả nhánh trùng, nguồn đổi, thiếu người và lỗi gửi.

### Duyệt, từ chối, mở lại và hủy

Duyệt kiểm tra nguồn trước transaction và kiểm lại quyền/version dưới khóa. Với điều chỉnh, xác minh thêm biên bản/metadata/blob vẫn truy cập được và checksum đúng; lỗi chứng cứ thì chặn duyệt, không thay Approved hiện hành. Nguồn không hợp lệ/đổi ảnh hưởng → giữ chờ, hiển thị cảnh báo cho người được giao, có hành động từ chối nhập lý do; không tự chuyển Rejected. Mạng nguồn lỗi → 503, giữ chờ và cho thử lại. Từ chối không bị buộc đọc lại nguồn nên vẫn có thể trả hồ sơ để sửa khi có căn cứ lý do. Hai lệnh approve/reject hay reassign/approve tranh chấp chỉ lệnh thắng khóa/version có hiệu lực; lệnh còn lại lỗi conflict hoặc mất quyền. Reopen và cancel là lệnh riêng có audit; hủy không xóa nội dung/lịch sử.

### Điều chỉnh và giá trị hiện hành

KTT tạo từ `currentApprovedDocumentId`, lưu `baseApprovedDocumentId`, đủ toàn bộ dòng/tổng mới, `delta = newOfficialTotal - baseOfficialTotal`. Một chain chỉ có một điều chỉnh Draft/PendingApproval/Rejected theo PAY-DEC-015. Kiểm `hasProcessingAdjustment()` trên toàn chuỗi, không dựa riêng trạng thái base/điều chỉnh cũ. Trình dùng tổng mới đầy đủ và loại người lập gốc/người lập điều chỉnh; biên bản bắt buộc trước trình, và bắt buộc ngay khi dùng ngoại lệ hợp đồng đã kết thúc. Khi Approved, cập nhật con trỏ chain sang hồ sơ mới; không cộng các bản đầy đủ. Bản cũ giữ nguyên; khác biệt và căn cứ trước-sau đọc được. Hủy/từ chối không thay con trỏ hiệu lực; hủy điều chỉnh không giải phóng tháng của gốc.

### Bổ sung PAY-DEC-015: điều kiện toàn chuỗi

| Trạng thái điều chỉnh đang xét | Giữ vị trí của chính hồ sơ? | Tạo điều chỉnh khác / bàn giao mới |
| --- | --- | --- |
| Draft | Có | Không tạo khác; tạm dừng bàn giao |
| PendingApproval | Có | Không tạo khác; tạm dừng bàn giao |
| Rejected | Có | Chỉ mở lại chính hồ sơ sang Draft để sửa/hủy; vẫn dừng bàn giao |
| Approved | Không | Chỉ tạo tiếp từ bản Approved hiện hành khi toàn chuỗi không có điều chỉnh đang xử lý; bàn giao căn cứ mới đã duyệt khi điều kiện toàn chuỗi đạt |
| Cancelled | Không | Không khôi phục; chỉ tạo mã mới nếu đủ điều kiện và toàn chuỗi không có điều chỉnh đang xử lý; bàn giao theo bản Approved hiện hành |

Rejected → Draft vẫn giữ vị trí; Draft → Cancelled mới kết thúc vị trí của hồ sơ đó, giữ nguyên claim tháng gốc và bản Approved hiện hành. Nếu hồ sơ A đã Approved/Cancelled nhưng hồ sơ B đang xử lý thì vẫn chặn tạo C và dừng bàn giao mới. Đây là bổ sung F001/AC-US-PAY-04-04/05/06, không thêm trạng thái.

### Bổ sung kiểm chứng F002: giữ nguyên nội dung khi nguồn lỗi/đổi

| AC | Thời điểm/nhánh | Kết quả thiết kế |
| --- | --- | --- |
| AC-US-PAY-02-06 | Trình Draft lưu 100 triệu, nguồn đổi thành 110 triệu | 409 SOURCE_CHANGED; giữ Draft, revision/căn cứ và 100 triệu, báo diff; không tạo submission/outbox. Chỉ A04/A05 có chấp nhận chủ động mới thay tiền |
| AC-US-PAY-02-07 | Trình khi nguồn không đọc được | 503 SOURCE_UNAVAILABLE; giữ Draft và tiền/căn cứ, nêu nguồn lỗi, không có PendingApproval |
| AC-US-PAY-03-09 | Duyệt khi nguồn đổi ảnh hưởng/không còn hợp lệ | 409 SOURCE_CHANGED hoặc 422 SOURCE_INVALID; giữ PendingApproval và revision/căn cứ lần trình, thông báo trong phản hồi cho người đang được giao. Chỉ reject có lý do của người đó mới chuyển Rejected |
| AC-US-PAY-03-10 | Duyệt khi nguồn không đọc được | 503 SOURCE_UNAVAILABLE; giữ PendingApproval và toàn bộ nội dung lần trình để chờ kiểm tra, không tự approve/reject |

Thông báo lỗi nguồn ở đây là phản hồi/cảnh báo hiển thị cho người thao tác, không mặc định tạo thêm Notification yêu cầu duyệt hoặc vòng trình mới. Đây là truy vết bổ sung cho PAY-DEC-012 đã có.

## 5. UML và cách đọc

- [Class](diagrams/class.puml): cấu trúc, operation và multiplicity; hình thoi đặc chỉ dành cho phần thuộc nội dung/vòng đời chủ thể.
- [Sequence trình](diagrams/sequence.puml), [lập/cập nhật](diagrams/sequence-create.puml), [duyệt/từ chối](diagrams/sequence-review.puml), [điều chỉnh](diagrams/sequence-adjustment.puml): tách theo use case để không tạo một hình quá dài.
- [State machine](diagrams/state-machine.puml): đủ năm trạng thái; hành động trên Approved tạo đối tượng mới, không quay Approved cũ về Draft.

Dùng ngữ nghĩa [UML 2.5.1 của OMG](https://www.omg.org/spec/UML/2.5.1); cú pháp tham chiếu chính thức [class](https://plantuml.com/class-diagram), [sequence](https://plantuml.com/sequence-diagram), [state](https://plantuml.com/state-diagram). Kết quả render/kiểm tra thực tế được ghi trong README thiết kế và bàn giao, không suy từ việc đọc mã PlantUML.
