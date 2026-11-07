# 04. Hợp đồng API đề xuất

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt: người dùng trong phiên, 07/11/2026 (Asia/Bangkok), prompt “ok approve”; căn cứ PRM-008 trong [nhật ký prompt](../../process/prompt-log.md).
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf) trang 1 mục 1-3, trang 2 mục 4-5, trang 3 mục 6; [30 AC của BA](../../analysis/payment-management/requirements-analysis.md); [PAY-DEC-001 đến PAY-DEC-014](../../analysis/payment-management/decisions.md).
- Vòng review: chưa mở. **Toàn bộ method/path/field/header/status dưới đây là đề xuất kỹ thuật**, không phải API đã có hoặc nội dung PDF.

## 1. Quy ước chung

Base path `/api/v1`; JSON UTF-8. `D` trong bảng là `/api/v1/payment-documents/{id}`. ID opaque string; không suy quyền từ việc biết ID. Timestamp ISO-8601 UTC; ngày dịch vụ/kỳ dùng ngày dương lịch của nghiệp vụ Asia/Bangkok. Period là `{ "type":"Month", "year":2026, "number":10 }` hoặc `Quarter` với number 1..4; Month 1..12. `calendarMonths` do server tính. Số tiền/sản lượng/giá qua JSON bằng chuỗi decimal chính xác, không JSON float. Tất cả request cấm field không được khai báo, đặc biệt createdBy/role/status/tổng/dòng tài chính tự nhập.

**Xác thực và phân quyền chung:** phiên/token do host xác thực → ActorContext; trả 401 nếu thiếu/sai phiên. Kiểm tra đơn vị và quyền xem riêng trước đọc; kiểm tra phụ trách, vai trò và phân công hiện hành cho từng lệnh; 403 nếu không đủ quyền. Lập gốc chỉ NVKD phụ trách hợp đồng; điều chỉnh chỉ KTT phụ trách trong đơn vị. Không suy quyền sửa từ quyền xem; `originalCreatorId/createdBy` lấy từ subject tin cậy. Endpoint vận hành/phân công cần capability riêng do nguồn quyền cung cấp. Danh sách lọc phía server theo quyền, không tin unitId từ client.

**Đồng thời:** GET document trả `ETag: "17"` ứng với version; lệnh thay đổi đối tượng đã có yêu cầu `If-Match: "17"` (thiếu → 428; lệch → 412). Version tăng cả khi thay phân công/cập nhật nguồn để màn hình cũ không ghi đè. POST tạo gốc không có ETag; tạo điều chỉnh yêu cầu If-Match của bản Approved được tham chiếu và khóa chain hiện hành. Preview không đổi document nhưng cần If-Match để gắn candidate đúng bản.

**Idempotency:** lệnh thay đổi, đăng ký biên bản và lệnh retry thủ công bắt buộc `Idempotency-Key`; preview là phép đọc nên không bắt buộc. Key tối đa 128 ký tự, lưu theo subject+method+route+key cùng fingerprint body/If-Match và kết quả. Cùng key/fingerprint → trả nguyên kết quả sau kiểm tra quyền hiện hành, header `Idempotency-Replayed: true`; khác fingerprint → 409 `IDEMPOTENCY_KEY_REUSED`. Không tự xóa key trong giai đoạn đầu; retention là DEP-05. Yêu cầu cùng key đang chạy được serialize qua unique key/transaction, không xử lý hai lần. Quyền đã bị thu hồi thì không trả lại dữ liệu ngoài quyền.

**Thứ tự riêng của submit:** xác thực/quyền → replay key nếu có → đọc/khóa trạng thái. Nếu PendingApproval và người gọi còn quyền trình, trả submission hiện hữu (200) ngay cả ETag cũ, không đọc nguồn hoặc sinh outbox mới. Vẫn yêu cầu header If-Match có mặt. Nếu Draft thì áp If-Match chặt, đọc/recheck nguồn rồi transaction kiểm lại version. Hai submit của cùng revision đồng thời, lệnh thua thấy PendingApproval trả cùng submission thay vì tạo round mới. Approved/Rejected/Cancelled với key mới → 409 `INVALID_STATE`; replay đúng key cũ vẫn chỉ trả receipt cũ, không đổi trạng thái hiện tại.

Transaction ký hiệu `TX-*` được mô tả ở tài liệu 05. Tất cả lệnh tài chính/trạng thái/phân công ghi audit cùng transaction; lỗi audit rollback lệnh. GET/preview không tạo audit thay đổi; lịch sử gửi là notification attempt riêng. Không gọi nguồn dưới khóa DB.

## 2. Kiểu phản hồi và lỗi

Ví dụ rút gọn document; GET chi tiết trả đủ dòng/căn cứ/biên bản/người duyệt và lịch sử liên quan theo quyền:

```json
{
  "id":"PAY-001", "kind":"Original", "chainId":"CH-001",
  "status":"Draft", "contractId":"HD-01", "customerId":"KH-01", "unitId":"DV-01",
  "period":{"type":"Month","year":2026,"number":10},
  "calendarMonths":["2026-10"], "version":1, "revisionId":"REV-001",
  "originalCreatorId":"U-01", "createdBy":"U-01", "responsibleId":"U-01",
  "exactTotal":"3400000", "officialTotal":"3400000", "currency":"VND",
  "lines":[
    {"lineKey":"S1-A","serviceId":"S1","unit":"luot","serviceFrom":"2026-10-01","serviceTo":"2026-10-14","quantity":"10","unitPrice":"100000","exactAmount":"1000000","amount":"1000000","note":""},
    {"lineKey":"S1-B","serviceId":"S1","unit":"luot","serviceFrom":"2026-10-15","serviceTo":"2026-10-31","quantity":"20","unitPrice":"120000","exactAmount":"2400000","amount":"2400000","note":""}
  ],
  "sourceSnapshotId":"SS-001", "sourceObservedAt":"2026-11-07T02:00:00Z",
  "currentSubmission":null, "currentApprovedDocumentId":null,
  "baseApprovedDocumentId":null, "delta":null,
  "replacementOfDocumentId":null, "handoffBlockedByAdjustment":false
}
```

Mutation trả document hoặc receipt chỉ rõ id/status/version/revisionId. Receipt submission:

```json
{"documentId":"PAY-001","status":"PendingApproval","version":2,"submissionId":"SUB-001","roundNo":1,"revisionId":"REV-001","officialTotal":"3400000","requiredRole":"KTT","assignedTo":"U-02","assignmentVersion":1,"notificationState":"Queued","message":"Đã trình duyệt; thông báo đang chờ gửi"}
```

Nếu worker đã ghi lỗi, GET trả `Retrying`/`NeedsAttention` và “Đã trình duyệt; thông báo đang chờ gửi lại”. POST submit không đợi gửi để commit. `Delivered` chỉ dùng khi adapter đã có kết quả giao nhận tương ứng, không mặc định là người nhận đã đọc.

```json
{"error":{"code":"SOURCE_CHANGED","message":"Căn cứ sản lượng đã thay đổi; hãy xem và chấp nhận cập nhật trước khi trình","details":[{"source":"Quantity","referenceId":"SL-01","field":"quantity","old":"10","new":"12"}],"correlationId":"REQ-001"}}
```

| HTTP | Mã ổn định | Ý nghĩa/hành động |
| --- | --- | --- |
| 400 | INVALID_REQUEST, UNKNOWN_FIELD | JSON/period/header sai dạng; field tài chính không thuộc request có thể trả FINANCIAL_FIELD_FORBIDDEN |
| 401 / 403 / 404 | UNAUTHENTICATED / FORBIDDEN, SELF_APPROVAL, NOT_ASSIGNED / NOT_FOUND | Danh tính/quyền hoặc đối tượng không tồn tại; SELF_APPROVAL áp cả reject |
| 409 | PERIOD_CONFLICT, SOURCE_CHANGED, SOURCE_INCONSISTENT, INVALID_STATE, ADJUSTMENT_IN_PROGRESS, STALE_BASE, IDEMPOTENCY_KEY_REUSED, PREVIEW_EXPIRED | Trạng thái/dữ liệu tranh chấp; không tự cập nhật tiền hoặc tạo hồ sơ khác |
| 412 / 428 | VERSION_CONFLICT / PRECONDITION_REQUIRED | Client tải lại hoặc bổ sung If-Match |
| 422 | SOURCE_INVALID, PRICE_MISSING, PRICE_AMBIGUOUS, QUANTITY_UNRECONCILED, ALLOCATION_MISSING, INPUT_PRECISION, NEGATIVE_TOTAL, MISSING_REQUIRED_SERVICE, NO_ELIGIBLE_APPROVER, EVIDENCE_REQUIRED, REASON_REQUIRED, ASSIGNMENT_INVALID | Nêu nguồn/field/điều kiện chưa đạt; giữ trạng thái cũ |
| 503 | SOURCE_UNAVAILABLE, DEPENDENCY_UNAVAILABLE | Nguồn/kho chứng cứ không truy cập được; không tự reject hoặc rollback lần trình đã commit |
| 500 | INTERNAL_ERROR | Lỗi ngoài dự kiến; transaction chưa commit phải rollback; client retry cùng key khi không biết kết quả |

## 3. API hồ sơ và cập nhật

Mỗi dòng áp dụng toàn bộ hợp đồng chung ở mục 1-2. Body ghi trong bảng là ví dụ JSON hợp lệ; response “Document” là kiểu mục 2, không phải chỉ tên đối tượng.

| ID; method/path; mục tiêu | Actor; precondition/validation | Request ví dụ | Response; status/lỗi đặc thù | Idempotency; concurrency; transaction/audit | Truy vết |
| --- | --- | --- | --- | --- | --- |
| A01 `POST /api/v1/payment-documents`; lập gốc | NVKD phụ trách; Active/giá/đối soát/period hợp lệ, không chồng tháng. replacement phải là Cancelled cùng hợp đồng và lý do không rỗng | `{"contractId":"HD-01","period":{"type":"Month","year":2026,"number":10},"replacementOfDocumentId":null,"replacementReason":null}` | 201 Document Draft, Location và ETag; 409 PERIOD_CONFLICT; 422 SOURCE_INVALID/PRICE_MISSING/PRICE_AMBIGUOUS/QUANTITY_UNRECONCILED/ALLOCATION_MISSING/INPUT_PRECISION | Key; không If-Match; TX-CREATE; Created và liên kết lập thay thế | US-PAY-01/05; PAY-01/02/03; AC-US-PAY-01-01/02/03/04/05/07/08, AC-US-PAY-05-03; DEC-004/007/008/009/011/012 |
| A02 `GET D`; xem hồ sơ/căn cứ | Người có quyền xem đơn vị/hồ sơ; không cấp quyền sửa | Không body | 200 Document+ETag; 404; hiện currentApproved/activeAdjustment, nội dung từng lần trình theo revision | Không key; snapshot đọc nhất quán; không transaction ghi/audit; không tự refresh nguồn | US-PAY-01/03/04; PAY-05; AC-US-PAY-03-08, AC-US-PAY-04-01/03; DEC-006/012/013 |
| A03 `PATCH D/notes`; sửa ghi chú | Phụ trách, Draft; lineKey phải thuộc revision hiện hành; chỉ note, lineNotes | `{"note":"Đã kiểm tra","lineNotes":[{"lineKey":"S1-A","note":"Đợt 1"}]}` | 200 Document+ETag; 409 INVALID_STATE; 400 FINANCIAL_FIELD_FORBIDDEN/UNKNOWN_FIELD; 422 lineKey sai | Key+If-Match; TX-EDIT; NotesEdited trước-sau, tiền/snapshot giữ nguyên | US-PAY-01; PAY-05; AC-US-PAY-01-06, AC-NFR-PAY-01-02; DEC-006/011 |
| A04 `POST D/source-previews`; xem thay đổi nguồn | Phụ trách, Draft, If-Match; nguồn coherent và tính được; không nhận số tài chính | `{}` | 200 `{"previewToken":"PV-001","baseVersion":3,"expiresAt":"2026-11-07T02:15:00Z","newOfficialTotal":"3600000","diff":[{"field":"quantity","old":"10","new":"12"}],"unmappedLineNotes":[]}`; 422/503 lỗi nguồn | Không key bắt buộc; If-Match; chỉ lưu candidate phụ trợ, không đổi document/audit tiền; token gắn actor/baseVersion, hạn 15 phút | US-PAY-01/04; AC-US-PAY-01-05/06, AC-US-PAY-04-01; DEC-006/008/009/012 |
| A05 `POST D/source-refreshes`; chấp nhận toàn bộ candidate | Phụ trách, Draft; token đúng actor/bảng/version/chưa hết hạn; `accept:true`, đã xem diff và note không ánh xạ; nguồn vẫn khớp candidate | `{"previewToken":"PV-001","accept":true,"acknowledgeUnmappedNotes":true}` | 200 Document+ETag; 409 SOURCE_CHANGED/PREVIEW_EXPIRED; 412 version; 422 nguồn sai; không cập nhật một phần | Key+If-Match; TX-EDIT; SourcesAccepted trước-sau, snapshot mới; token consumed cùng commit | US-PAY-01/04; PAY-01/02/03; AC-US-PAY-01-05/06, AC-US-PAY-04-03, AC-NFR-PAY-01-02; DEC-006/008/009/012 |

## 4. API vòng đời và điều chỉnh

| ID; method/path; mục tiêu | Actor; precondition/validation | Request ví dụ | Response; status/lỗi đặc thù | Idempotency; concurrency; transaction/audit | Truy vết |
| --- | --- | --- | --- | --- | --- |
| A06 `POST D/submissions`; trình | Phụ trách NVKD hoặc KTT điều chỉnh; Draft đạt toàn bộ guard tài liệu 03; PendingApproval xử lý replay như mục 1 | `{}` | 201 receipt lần mới+ETag; 200 cùng receipt khi đang chờ; 409 SOURCE_CHANGED/INVALID_STATE; 422 NEGATIVE_TOTAL/MISSING_REQUIRED_SERVICE/NO_ELIGIBLE_APPROVER/EVIDENCE_REQUIRED; 503 nguồn | Key+If-Match, ngoại lệ ETag cũ chỉ khi đang chờ; TX-SUBMIT; Submitted+outbox duy nhất; không gửi dưới transaction | US-PAY-02/04; PAY-01/02/04/06/07; AC-US-PAY-02-01 đến AC-US-PAY-02-05, AC-US-PAY-04-02; DEC-001/002/005/009/011/012/014 |
| A07 `POST D/approval-decisions`; duyệt/từ chối | Đúng người đang được giao, vai trò ngưỡng và độc lập hai người lập; PendingApproval; submissionId đúng hiện hành. Approve recheck nguồn và chứng cứ điều chỉnh; Reject lý do trim không rỗng | `{"submissionId":"SUB-001","decision":"Approve","reason":null}` hoặc `{"submissionId":"SUB-001","decision":"Reject","reason":"Căn cứ sản lượng đã thay đổi"}` | 200 `{"documentId":"PAY-001","submissionId":"SUB-001","status":"Approved","version":3,"decidedBy":"U-02"}`; Reject trả Rejected; 403 SELF_APPROVAL/NOT_ASSIGNED; 409 SOURCE_CHANGED/INVALID_STATE; 422 REASON_REQUIRED/EVIDENCE_REQUIRED; 503 nguồn/kho chứng cứ khi approve | Key+If-Match; TX-DECIDE; Approved/Rejected; chỉ một kết quả, dừng outbox; cập nhật con trỏ chain khi duyệt | US-PAY-03/04; PAY-05/06/07/08; AC-US-PAY-03-01 đến AC-US-PAY-03-07, AC-US-PAY-04-01/03; DEC-001/002/003/011/012 |
| A08 `POST D/reopenings`; Chỉnh sửa lại | Phụ trách, Rejected; không đổi tiền/người lập | `{}` | 200 Document Draft+ETag, revisionId mới sao chép tiền/căn cứ; 409 INVALID_STATE; không xóa lý do/lần trình cũ | Key+If-Match; TX-STATE; Reopened; lần trình sau sinh round mới | US-PAY-01/03/04; PAY-08; AC-US-PAY-03-06, AC-NFR-PAY-01-01; DEC-003/011 |
| A09 `POST D/cancellations`; hủy | Phụ trách, Draft, lý do trim không rỗng; KTT không hủy mọi bảng gốc | `{"reason":"Lập nhầm kỳ"}` | 200 `{"documentId":"PAY-001","status":"Cancelled","version":4}`; 409 INVALID_STATE; 422 REASON_REQUIRED | Key+If-Match; TX-CANCEL; Cancelled+claim release với gốc, chỉ activeAdjustment release với điều chỉnh | US-PAY-05; PAY-01/04; AC-US-PAY-05-01/02/03; DEC-004/007/011 |
| A10 `POST D/adjustments`; lập điều chỉnh từ bản Approved hiện hành | KTT được phép phụ trách; id là currentApproved; chưa có điều chỉnh đang xử lý; lý do không rỗng; source sửa hợp lệ; đủ biên bản ngay nếu hợp đồng đã kết thúc | `{"reason":"Sản lượng đã được sửa theo xác nhận","evidenceId":"EV-001"}` | 201 Document Adjustment, baseApprovedDocumentId, delta và tổng mới đầy đủ; 409 STALE_BASE/ADJUSTMENT_IN_PROGRESS; 422 EVIDENCE_REQUIRED/SOURCE_INVALID | Key+If-Match của base; TX-ADJUST; AdjustmentCreated; không sửa base/không chiếm tháng thêm | US-PAY-04; PAY-03/05/06/07; AC-US-PAY-04-01/02/03; DEC-001/008/009/011/012 |
| A11 `PATCH D/adjustment-details`; lý do/biên bản | KTT phụ trách, Draft Adjustment; lý do không rỗng; evidenceId phải có quyền/đã đăng ký/bất biến. Active contract có thể chưa gắn biên bản lúc lập nhưng phải đủ trước trình | `{"reason":"Đã xác nhận lại số liệu","evidenceId":"EV-002"}` | 200 Document+ETag; 409 INVALID_STATE; 422 EVIDENCE_REQUIRED; không nhận dòng/tổng | Key+If-Match; TX-EDIT; AdjustmentDetailsEdited, revision mới giữ tiền | US-PAY-04; AC-US-PAY-04-02/03, AC-NFR-PAY-01-02; DEC-001/006/012 |

## 5. API phân công, chứng cứ và vận hành

| ID; method/path; mục tiêu | Actor; precondition/validation | Request ví dụ | Response; status/lỗi đặc thù | Idempotency; concurrency; transaction/audit | Truy vết |
| --- | --- | --- | --- | --- | --- |
| A12 `POST D/approver-assignments`; đổi người duyệt | Đầu mối phân công có capability đơn vị; PendingApproval; người mới cùng requiredRole, độc lập, ủy quyền hiệu lực, lý do/căn cứ không rỗng | `{"assigneeId":"U-03","reason":"Người chính vắng mặt","authorityRef":"PC-01"}` | 200 `{"submissionId":"SUB-001","assignedTo":"U-03","assignmentVersion":2,"version":4}`; 422 ASSIGNMENT_INVALID; 409 INVALID_STATE | Key+If-Match; TX-ASSIGN; ApproverReassigned; outbox cũ dừng, mới cùng submission; không round mới | US-PAY-03; PAY-07; AC-US-PAY-03-04/07; DEC-011/014 |
| A13 `POST D/responsibility-transfers`; bàn giao phụ trách | Đầu mối có capability đơn vị; người nhận NVKD hoặc KTT theo kind, cùng đơn vị/phân công hợp lệ; authorityRef/reason bắt buộc | `{"responsibleId":"U-04","reason":"Bàn giao hợp đồng","authorityRef":"BG-01"}` | 200 `{"documentId":"PAY-001","responsibleId":"U-04","originalCreatorId":"U-01","version":5}`; 422 ASSIGNMENT_INVALID | Key+If-Match; TX-ASSIGN; ResponsibilityTransferred; giữ nội dung Approved và danh tính người lập; với Draft chỉ người nhận được thao tác sau commit | US-PAY-01/05; PAY-07; AC-US-PAY-01-06, AC-US-PAY-05-01; DEC-007/011 |
| A14 `GET /api/v1/approval-inbox?cursor=...&limit=50`; danh sách chờ | Người có vai trò; server chỉ trả submission đang được giao/được xem, không nhận assignedTo tùy ý; limit 1..100 | Không body | 200 `{"items":[{"documentId":"PAY-001","submissionId":"SUB-001","officialTotal":"3400000","version":2}],"nextCursor":null}` | Không key/If-Match; đọc DB nhất quán, không phụ thuộc thông báo; không audit thay đổi | US-PAY-02/03; AC-US-PAY-02-05, AC-US-PAY-03-07; DEC-011/014 |
| A15 `GET D/audit-events?cursor=...&limit=50`; lịch sử | Quyền xem audit trong đơn vị/hồ sơ; limit 1..100, cursor opaque | Không body | 200 `{"items":[{"action":"Submitted","actorId":"U-01","at":"2026-11-07T02:01:00Z","beforeStatus":"Draft","afterStatus":"PendingApproval","submissionId":"SUB-001"}],"nextCursor":null}` | Không key/If-Match; đọc append-only, không audit thay đổi | NFR-PAY-01; AC-NFR-PAY-01-01/02; DEC-001/003/006/007/011/012 |
| A16 `POST /api/v1/evidence`; đăng ký biên bản | KTT có quyền đơn vị; multipart `file`, metadata mã/ngày/các bên; file không rỗng, kiểu nội bộ cho phép PDF/PNG/JPEG, tối đa 20 MiB (giới hạn kỹ thuật); không URL tùy ý | metadata `{"documentCode":"BB-01","documentDate":"2026-11-06","confirmedParties":["Bên A","Bên B"],"unitId":"DV-01"}` | 201 `{"evidenceId":"EV-001","checksum":"sha256:...","documentCode":"BB-01"}`; 413 quá dung lượng; 415 sai kiểu; 422 metadata thiếu | Key; không If-Match; blob lưu bất biến trước metadata TX-EVIDENCE; EvidenceRegistered; lỗi metadata không cho liên kết blob; dọn blob mồ côi theo vận hành | US-PAY-04; AC-US-PAY-04-01/02; DEC-001/012 |
| A17 `GET /api/v1/evidence/{evidenceId}`; đọc biên bản | Có quyền với hồ sơ đang liên kết; khi chưa liên kết chỉ người đăng ký/KTT có quyền trong đơn vị; kiểm quyền trước trả blob | Không body | 200 binary có Content-Type/checksum; 403/404/503 kho lỗi | Không key/If-Match; chỉ đọc; không audit thay đổi; log truy cập theo chính sách host | US-PAY-04; AC-US-PAY-04-01/02; DEC-001 |
| A18 `GET /api/v1/notifications?state=NeedsAttention&cursor=...&limit=50`; lỗi cần vận hành | Capability vận hành đơn vị; limit 1..100; lọc quyền phía server | Không body | 200 `{"items":[{"id":"NT-01","submissionId":"SUB-001","state":"NeedsAttention","automaticRetries":3,"version":5}],"nextCursor":null}` | Không key/If-Match; chỉ đọc; không audit trạng thái bảng | US-PAY-02; AC-US-PAY-02-05; DEC-014 |
| A19 `POST /api/v1/notifications/{notificationId}/retries`; gửi lại thủ công | Vận hành, notification còn gắn submission/người nhận hiện hành, PendingApproval; lý do bắt buộc | `{"reason":"Đã khôi phục kênh gửi"}` | 202 `{"notificationId":"NT-01","submissionId":"SUB-001","state":"Queued","version":6}`; 409 NOTIFICATION_OBSOLETE; 422 REASON_REQUIRED | Key+If-Match version notification; TX-NOTIFY; ManualRetryRequested và attempt sau gửi; không tạo submission/round hoặc reset bộ đếm tự động | US-PAY-02; PAY-04; AC-US-PAY-02-05; DEC-014 |

`DEC-xxx` là PAY-DEC-xxx. A16 là kho chứng cứ tối thiểu phục vụ US-PAY-04, không là module quản lý tài liệu tổng quát; giới hạn kỹ thuật được ghi tại ADR-007 và có thể điều chỉnh khi tích hợp. Không có endpoint bàn giao kế toán, xuất hóa đơn, sửa hợp đồng/giá/sản lượng hoặc sửa tay tiền.

## 6. Kiểm tra hợp đồng API dự kiến

Kiểm tra request/response bằng contract test khi có implementation; đối chiếu ActorContext với đơn vị/phân công, key replay/mismatch, If-Match thiếu/cũ, đồng thời trình/duyệt/đổi người, không nhận field tài chính. Lỗi nguồn phải giữ đúng trạng thái; lỗi thông báo không làm mất lần trình. Quy tắc và 30 AC được ánh xạ cụ thể trong [ma trận](06-traceability-matrix.md). Tài liệu hiện tại chưa là OpenAPI sinh tự động hoặc endpoint đã chạy.
