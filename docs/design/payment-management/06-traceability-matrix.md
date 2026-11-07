# 06. Ma trận truy vết

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt: người dùng trong phiên, 07/11/2026 (Asia/Bangkok), prompt “ok approve”; căn cứ PRM-008 trong [nhật ký prompt](../../process/prompt-log.md).
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf), [định nghĩa nguồn S và 30 AC của BA](../../analysis/payment-management/requirements-analysis.md), [PAY-DEC-001 đến PAY-DEC-014](../../analysis/payment-management/decisions.md).
- Vòng review: chưa mở. Trạng thái mọi dòng: **đã mô tả thiết kế, chưa kiểm thử ứng dụng**; phụ thuộc triển khai không là vấn đề nghiệp vụ chưa xác nhận.

## 1. Quy ước và đường dẫn

Component/class xem tài liệu [02](02-architecture-design.md)/[03](03-detailed-design.md); API A01-A19 xem [04](04-api-contract.md); data/TX xem [05](05-data-design.md); test T01-T20 và DEP-01..05 xem [08](08-handoff-to-developer.md). Quyết định kỹ thuật ADR-001..008 xem [07](07-design-decisions.md).

UML: C = [class.puml](diagrams/class.puml); S = [state-machine.puml](diagrams/state-machine.puml); Q1 = [sequence-create.puml](diagrams/sequence-create.puml); Q2 = [sequence.puml](diagrams/sequence.puml); Q3 = [sequence-review.puml](diagrams/sequence-review.puml); Q4 = [sequence-adjustment.puml](diagrams/sequence-adjustment.puml). Các sơ đồ bổ sung tách luồng theo use case, không thêm yêu cầu nghiệp vụ.

## 2. US, rule và NFR đến thiết kế/kiểm chứng

| Mã; nguồn PDF | Component/class | UML | API | Data | Kiểm chứng; trạng thái/phụ thuộc |
| --- | --- | --- | --- | --- | --- |
| US-PAY-01; S04 trang 2 mục 4.1 | PaymentService, MoneyCalculator, SourceGateway | C/S/Q1 | A01-A05, A08, A13 | Document/Revision/Line/Snapshot/MonthClaim | T01-T03/T11-T14; DEP-01/02/04 |
| US-PAY-02; S05 trang 2 mục 4.2 | PaymentService, ApprovalPolicy, NotificationDispatcher | C/S/Q2 | A06, A14, A18/A19 | Submission/Notification/Receipt | T04-T08/T12/T17; DEP-01/02/03 |
| US-PAY-03; S06 trang 2 mục 4.3 | PaymentService, ApprovalPolicy, AssignmentDirectory | C/S/Q3 | A02, A07/A08/A12/A14/A15 | Submission/AssignmentHistory/Audit | T06/T07/T09/T10/T13/T19; DEP-01/02/03 |
| US-PAY-04; S07 trang 2 mục 4.4 | PaymentChain, PaymentService, EvidenceStore | C/S/Q4 | A04-A08, A10/A11/A16/A17 | Chain/Document/Revision/Evidence | T03/T06/T07/T12/T13/T16/T18/T19; DEP-01/02/03 |
| US-PAY-05; S08 trang 2 mục 4.5 | PaymentDocument, PaymentRepository | C/S/Q1 | A01/A08/A09/A13 | Document/MonthClaim/Audit | T14/T15; DEP-02/04 |
| PAY-01; S09 trang 2 mục 5 | SourceGateway, PaymentRepository | C/Q1/Q2/Q4 | A01/A04/A05/A06/A09/A10 | Snapshot/MonthClaim/Chain | T01/T02/T12/T14/T15/T16; DEC-004/007/008/012 |
| PAY-02; S09 trang 2 mục 5 | SourceGateway | C/Q1/Q2/Q3/Q4 | A01/A04/A05/A06/A07/A10 | Snapshot/Line | T01/T02/T12/T13; DEC-008/012 |
| PAY-03; S09 trang 2 mục 5 | MoneyCalculator | C/Q1/Q4 | A01/A04/A05/A10 | Line/Revision | T02/T03/T06/T16; DEC-008/009 |
| PAY-04; S09 trang 2 mục 5 | PaymentDocument, PaymentService | C/S/Q2 | A06/A19 | Submission/Revision/Receipt/Notification | T04/T05/T08/T17; DEC-005/009/014 |
| PAY-05; S09 trang 2 mục 5 | PaymentChain, PaymentRevision | C/S/Q4 | A02/A07/A10/A11 | Chain/Revision/Snapshot | T11/T16/T18; DEC-001 |
| PAY-06; S09 trang 2 mục 5; mâu thuẫn S06 | ApprovalPolicy | C/Q2/Q3/Q4 | A06/A07/A12 | Submission.requiredRole/Revision.officialTotal | T06/T07; DEC-002/009/011 |
| PAY-07; S09 trang 2 mục 5 | ApprovalPolicy, AssignmentDirectory | C/Q2/Q3/Q4 | A06/A07/A12/A13 | Document.creator/Submission/AssignmentHistory | T07/T09; DEC-001/011 |
| PAY-08; S09 trang 2 mục 5; mâu thuẫn S03 | PaymentDocument, PaymentService | C/S/Q3 | A07/A08 | Submission.reason/Revision/Audit | T10; DEC-003/011 |
| NFR-PAY-01; S07/S10 trang 2 mục 4.4/trang 3 mục 6 | UnitOfWork, PaymentRepository | C/Q1-Q4/S | Mọi mutation A01/A03/A05-A13/A16/A19; đọc A15 | Audit/AssignmentHistory/Revision/Attempt | T11/T15/T16/T18; DEP-04/05; chưa kiểm transaction thật |
| NFR-PAY-02; S10 trang 3 mục 6 | PaymentWeb, SourceGateway, MoneyCalculator, Repository | Q1/C | A01 và UI hiển thị | Claim/Revision/Line, index theo tài liệu 05 | T20; DEP-01/04; chưa đo |

`DEC-xxx` trong các bảng là viết tắt PAY-DEC-xxx; `Q1-Q4` là bốn sequence đã liên kết. Cột API/data ghi thành phần thực hiện, không phải khẳng định đã tồn tại.

## 3. Truy vết từng AC

Từng AC giữ nguyên định danh và expected result đã chốt trong BA, không tái định nghĩa ở đây. Mọi hàng đều áp dụng trạng thái đầu tài liệu; mã test chỉ là kế hoạch.

| AC BA | Class/component | UML | API | Data/TX | Test; phụ thuộc |
| --- | --- | --- | --- | --- | --- |
| AC-US-PAY-01-01 | PaymentService/SourceGateway/MoneyCalculator | C/Q1 | A01 | Snapshot/Revision/Line/Claim; TX-CREATE | T01/T02/T03/T14; DEP-01/02/04 |
| AC-US-PAY-01-02 | SourceGateway | Q1 | A01 | Snapshot; không commit khi invalid | T01; DEP-01 |
| AC-US-PAY-01-03 | SourceGateway | Q1 | A01/A04/A05 | Snapshot/Line | T01/T02; DEP-01 |
| AC-US-PAY-01-04 | SourceGateway | Q1 | A01 | Snapshot | T01; DEP-01 |
| AC-US-PAY-01-05 | MoneyCalculator | C/Q1 | A01/A04/A05 | Line/Revision; TX-CREATE/EDIT | T02/T03; DEP-01/04 |
| AC-US-PAY-01-06 | PaymentService/PaymentRevision | C/S/Q1 | A03/A04/A05/A13 | Revision/Audit; TX-EDIT | T11/T12/T18; DEP-01/02 |
| AC-US-PAY-01-07 | PaymentRepository | Q1 | A01 | MonthClaim PK; TX-CREATE | T14; DEP-04 |
| AC-US-PAY-01-08 | PaymentRepository/SourceGateway | S/Q1 | A01/A09 | Document.replacementOf/Claim; TX-CANCEL/CREATE | T14/T15; DEP-01/04 |
| AC-US-PAY-02-01 | PaymentService/ApprovalPolicy/Dispatcher | C/S/Q2 | A06 | Submission/Outbox/Audit; TX-SUBMIT | T04/T05/T06/T07/T12/T17; DEP-01/02/03 |
| AC-US-PAY-02-02 | PaymentDocument/MoneyCalculator | C/Q2 | A06 | Revision.exactTotal/officialTotal | T03/T05; DEP-04 |
| AC-US-PAY-02-03 | SourceGateway/PaymentDocument | Q2 | A06 | Snapshot/Line | T04; DEP-01 |
| AC-US-PAY-02-04 | PaymentDocument | Q2 | A06 | Revision/Line | T04/T05; DEP-01 |
| AC-US-PAY-02-05 | PaymentService/Dispatcher/Repository | Q2 | A06/A14/A18/A19 | Receipt/Submission/Notification/Attempt | T08/T17; DEP-03/04 |
| AC-US-PAY-03-01 | ApprovalPolicy/PaymentDocument | Q3 | A07 | Submission; TX-DECIDE | T06/T13; DEP-01/02 |
| AC-US-PAY-03-02 | ApprovalPolicy/PaymentDocument | Q3 | A07 | Submission; TX-DECIDE | T06/T13; DEP-01/02 |
| AC-US-PAY-03-03 | ApprovalPolicy | C/Q2/Q3 | A06/A07/A12 | Submission.requiredRole | T03/T06; DEP-02 |
| AC-US-PAY-03-04 | ApprovalPolicy/AssignmentDirectory | Q2/Q3/Q4 | A06/A07/A12 | Document.creators/Submission | T07; DEP-02 |
| AC-US-PAY-03-05 | PaymentDocument | S/Q3 | A07 | Submission.reason | T10; DEP-04 |
| AC-US-PAY-03-06 | PaymentDocument/PaymentRevision | S/Q3 | A07/A08 | Submission/Revision/Audit; TX-DECIDE/STATE | T10/T18; DEP-04 |
| AC-US-PAY-03-07 | AssignmentDirectory/Repository | Q3 | A07/A12/A14 | Submission/AssignmentHistory; TX-ASSIGN/DECIDE | T07/T09; DEP-02/04 |
| AC-US-PAY-03-08 | PaymentWeb/PaymentChain, quy trình KTT ngoài module | Q4 | A02 cung cấp căn cứ; không API bàn giao | Chain.currentApproved/Revision/Evidence | T19; DEP-03 |
| AC-US-PAY-04-01 | PaymentChain/PaymentService/EvidenceStore | C/S/Q4 | A10/A11/A16/A17/A06/A07 | Chain/Document/Revision/Evidence; TX-ADJUST/DECIDE | T02/T07/T13/T16; DEP-01/02/03 |
| AC-US-PAY-04-02 | PaymentService/EvidenceStore | Q2/Q4 | A10/A11/A16/A06 | Evidence/Revision | T16/T18; DEP-03 |
| AC-US-PAY-04-03 | UnitOfWork/PaymentChain | Q4 | A05/A07/A10/A11/A15 | Revision.delta/Audit; TX-EDIT/ADJUST/DECIDE | T16/T18; DEP-04 |
| AC-US-PAY-05-01 | PaymentDocument/AssignmentDirectory | S | A09/A13 | Document/Audit; TX-CANCEL | T15; DEP-02/04 |
| AC-US-PAY-05-02 | PaymentDocument | S | A03/A06/A08/A09 | Document.status, guard | T15; DEP-04 |
| AC-US-PAY-05-03 | PaymentChain/PaymentRepository | S/Q1/Q4 | A01/A09 | MonthClaim/Chain/Document.replacementOf | T14/T15/T16; DEP-01/04 |
| AC-NFR-PAY-01-01 | UnitOfWork/PaymentRepository | S/Q2/Q3/Q4 | A06/A07/A08/A09/A15 | Audit; TX-SUBMIT/DECIDE/STATE/CANCEL | T10/T15/T18; DEP-04/05 |
| AC-NFR-PAY-01-02 | UnitOfWork/PaymentRevision | C/Q1/Q4 | A03/A05/A10/A11/A15 | Revision/Snapshot/Audit | T11/T12/T16/T18; DEP-04/05 |
| AC-NFR-PAY-02-01 | PaymentWeb/Service/SourceGateway | Q1 | A01 và UI | Revision/Line/Claim/index | T20; DEP-01/04 |

## 4. Quyết định nghiệp vụ đến phần thiết kế chịu ảnh hưởng

| Vấn đề đã chốt → quyết định | Phần thực hiện chính | Test |
| --- | --- | --- |
| PAY-ISS-001 → PAY-DEC-001 | Chain/Adjustment/Evidence; A10/A11/A06/A07; C/S/Q4; ADR-006/007 | T07/T16/T18/T19 |
| PAY-ISS-002 → PAY-DEC-002 | ApprovalPolicy.requiredRole; A06/A07; Q2/Q3/Q4 | T06 |
| PAY-ISS-003 → PAY-DEC-003 | Rejected→reopen→Draft revision mới; A07/A08; S/Q3 | T10 |
| PAY-ISS-004 → PAY-DEC-004 | Period/config snapshot/MonthClaim; A01; Q1; ADR-003 | T14 |
| PAY-ISS-005 → PAY-DEC-005 | SourceBundle mandatory services và coverage; A01/A06; Q1/Q2 | T04 |
| PAY-ISS-006 → PAY-DEC-006 | Notes/revision/audit/preview-accept; A03-A05; C/Q1; ADR-002 | T11/T12/T18 |
| PAY-ISS-007 → PAY-DEC-007 | Cancel/claim release/replacement; A09/A01; S/Q1 | T15 |
| PAY-ISS-008 → PAY-DEC-008 | SourceGateway/MoneyCalculator/tách dòng; A01/A05/A10; Q1/Q4 | T02 |
| PAY-ISS-009 → PAY-DEC-009 | ExactDecimal/WholeVnd/dual total; A01/A05/A06; C/Q1/Q2; ADR-008 | T03/T05/T06 |
| PAY-ISS-010 → PAY-DEC-010 | Luồng A01/UI, batching/index và hồ sơ đo; Q1 | T20 |
| PAY-ISS-011 → PAY-DEC-011 | AssignmentDirectory/ApprovalPolicy/history; A06/A07/A12/A13/A14; Q2/Q3 | T07/T09 |
| PAY-ISS-012 → PAY-DEC-012 | Snapshot semantic hash/source recheck; A04/A05/A06/A07/A10; Q1-Q4 | T01/T12/T13/T16 |
| PAY-ISS-013 → PAY-DEC-013 | A02 hiển thị chuỗi Approved/điều chỉnh; quy trình thủ công ngoài module | T19 |
| PAY-ISS-014 → PAY-DEC-014 | Receipt/outbox/attempt/inbox; A06/A14/A18/A19; Q2; ADR-004/005 | T08/T17 |

Không có PAY-ISS nào trong 14 vấn đề trên còn chờ chọn phương án nghiệp vụ. Mốc baseline được lưu theo PRM-009; chưa có review độc lập, dữ liệu nguồn thật hoặc kết quả test ứng dụng; vì vậy không gán trạng thái “đã nghiệm thu” cho các dòng truy vết.
