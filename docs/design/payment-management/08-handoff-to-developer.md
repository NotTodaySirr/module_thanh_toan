# 08. Bàn giao cho Developer

- Module: `payment-management`. Trạng thái: **Design Approved - Người dùng đã phê duyệt**.
- Phê duyệt bản cập nhật: người dùng trong phiên, 12/11/2026 (Asia/Bangkok), prompt “that is fine, change the status to approve”; căn cứ PRM-013 trong [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật: 12/11/2026, Asia/Bangkok; tiếp nhận BA sau review phân tích R002, PRM-012. Giữ nguyên baseline/tag lịch sử.
- Phê duyệt lịch sử: bản baseline ngày 07/11/2026 được người dùng duyệt tại PRM-008; bản cập nhật ngày 12/11/2026 có xác nhận riêng tại PRM-013.
- Ngày: 07/11/2026, Asia/Bangkok; Codex / System Design Agent.
- Nguồn: [PDF](../../requirements/btcohong.pdf), [BA và 37 AC](../../analysis/payment-management/requirements-analysis.md), [PAY-DEC-001 đến PAY-DEC-015](../../analysis/payment-management/decisions.md).
- Đầu vào review: [R002 - phân tích BA đã đối chiếu](../../reviews/payment-management/R002/review-record.md); nguồn BA tại revised_commit được ghi trong tài liệu bàn giao. Không thực hiện review mới trong lượt Design.

## 1. Gói bàn giao và thứ tự triển khai đề xuất

Đọc [README thiết kế](README.md) để điều hướng tám tài liệu, sáu PlantUML và bản render. Quy tắc/AC lấy từ BA + PAY-DEC; API/schema/UML là đề xuất kỹ thuật có giải thích trong ADR, không dùng template bên cạnh làm nguồn nghiệp vụ.

1. Tạo project module độc lập theo stack do nhóm triển khai lựa chọn, đáp ứng năng lực giao dịch/exact numeric; dựng domain MoneyCalculator, ApprovalPolicy, PaymentDocument/Chain và state guard. Chưa có code ứng dụng nào để kế thừa.
2. Tạo schema/repository/audit/receipt/claim tháng và kiểm tra đồng thời ở DB thật; dựng adapter giả lập cho nguồn, phân quyền, EvidenceStore, kênh gửi để kiểm tra toàn bộ luồng xác định.
3. Tạo API A01-A19 và giao diện: ghi chú Draft, preview/accept nguồn, trình/inbox/duyệt/từ chối/mở lại/hủy, hồ sơ điều chỉnh/biên bản, phân công, vận hành thông báo. Quyền và tiền đều kiểm ở server.
4. Hoàn tất hợp đồng adapter nguồn/quyền/chứng cứ, chạy contract/integration test, sau đó nghiệm thu đầu-cuối và hiệu năng. Không bật tích hợp thật khi chưa có bundle nhất quán/quyền hợp lệ.

Không triển khai quản lý hợp đồng/giá/đối soát sản lượng, hóa đơn/công nợ/e-sign, tự động xuất file/bàn giao kế toán. KTT bàn giao thủ công theo PAY-DEC-013; màn hình đọc cung cấp căn cứ Approved/chuỗi điều chỉnh, không tạo trạng thái bàn giao mới.

## 2. Phụ thuộc phải hoàn tất trước tích hợp/nghiệm thu

| Mã | Đầu vào cần cung cấp và bên phối hợp | Điều kiện chưa đáp ứng / công việc vẫn làm được |
| --- | --- | --- |
| DEP-01 | Chủ quản hợp đồng/giá/sản lượng: giao thức thực tế, mapping ID, lịch sử kỳ/kỳ lẻ, danh sách bắt buộc có dấu rỗng hợp lệ, hiệu lực giá/ưu tiên, đối soát/phân bổ theo ngày, phiên bản/căn cứ nhất quán và độ lớn decimal | Chưa thể tích hợp/khẳng định source coherence; dựng port/domain và test dữ liệu giả lập. Không giả định HTTP hay DB chung; nếu không xác lập bundle nhất quán phải chặn nguồn |
| DEP-02 | Chủ quản phân quyền/đơn vị: định danh subject, quyền xem riêng, NVKD/KTT phụ trách, người duyệt chính/thay thế/ủy quyền, capability phân công/vận hành, cơ chế xác minh version quyền và thu hồi | Chưa thể kiểm quyền thật hoặc giao người duyệt; dùng ActorContext/AssignmentDirectory giả lập. Thiếu người hợp lệ chặn trình; không chuyển cấp duyệt |
| DEP-03 | Đơn vị vận hành: kho biên bản bất biến/kiểm quyền/khả dụng, kênh thông báo và kết quả delivery, đầu mối lỗi, kế toán tiếp nhận và nơi lưu bằng chứng bàn giao thủ công | Chưa thể gửi/đọc biên bản hay chứng minh bàn giao thật; kiểm bằng adapter giả lập. Không coi Approved là đã bàn giao; không tự chọn API hóa đơn |
| DEP-04 | Nhóm triển khai/kiểm thử: stack, DB đủ exact numeric/khóa/unique/FK, topology/mạng/cấu hình, tập dữ liệu 1/199 dòng và tách giá, tải 1/10 người | Chưa thể kết luận NFR; chọn triển khai vật lý và đo. 200+ dòng khảo sát riêng, không tự áp ngưỡng 3 giây |
| DEP-05 | Chủ vận hành: quyền quản trị, backup/restore, retention audit/biên bản/receipt, dọn candidate/blob mồ côi, cảnh báo outbox và log | Chưa đủ vận hành production; giữ dữ liệu lịch sử, không tự xóa. Không phải quyết định nghiệp vụ trong 14 PAY-ISS cần xác nhận lại |

Nếu tích hợp đòi hỏi thay đổi nghiệp vụ, thêm quyết định mới có căn cứ, không ghi đè PAY-DEC hiện hành. Không hỏi lại quyết định người dùng đã xác nhận chỉ vì chưa có dữ liệu/cá nhân cụ thể.

## 3. Kế hoạch kiểm chứng và kết quả mong đợi

T01-T20 là các nhóm đã có; T21-T27 bổ sung mapping bảy AC mới sau R002. Mã T dưới đây là kế hoạch test của thiết kế, **chưa chạy trên ứng dụng**. Truy vết từng AC ở tài liệu 06. Test đồng thời phải dùng ít nhất hai kết nối/transaction thật, đồng bộ thời điểm qua barrier; chỉ test UI disable là không đủ.

| Mã | Tình huống | Kết quả mong đợi theo quy tắc đã xác nhận |
| --- | --- | --- |
| T01 | Lập Active; không Active; giá thiếu/trùng; sản lượng chưa đối soát; sai đơn vị/không phân công | Hợp lệ tạo Draft; còn lại chặn, không tạo claim/revision dở dang. Ngoại lệ hợp đồng kết thúc chỉ cho điều chỉnh đủ điều kiện |
| T02 | Giá đổi giữa kỳ; tổng sản lượng không phân bổ; kỳ lẻ thiếu căn cứ | Tách đúng hai đoạn theo phân bổ/giá; không chia đều, không chọn giá mới nhất; thiếu phân bổ/kỳ lẻ thì chặn |
| T03 | Hai dòng tích 100,5; ±0,5; -0,4; đầu vào quá 6 số lẻ | 101+101=202; HALF_UP ra xa 0 ở nửa đơn vị; -0,4 có tổng làm tròn 0 nhưng exactTotal âm chặn trình; precision vượt không tự làm tròn |
| T04 | Trình thiếu dịch vụ B; B=0 đã đối soát; danh sách bắt buộc rỗng đã xác nhận; danh sách không đọc được | Thiếu B/chưa xác định chặn; B=0 hợp lệ không thiếu; rỗng thật không tự tạo nghĩa vụ; không chuyển missing thành 0 |
| T05 | Tổng âm chính thức hoặc tổng tích âm; hai tổng 0 hợp lệ | Chặn hai loại âm; tổng 0 vẫn trình nếu guard khác đạt |
| T06 | Tổng 499.999.999 / 500.000.000 / 500.000.001; hai tích 249.999.999,5 | Lần lượt KTT / GĐKD / GĐKD; hai tích làm tròn thành đúng 500 triệu giao GĐKD; không hai cấp duyệt nối tiếp |
| T07 | Người lập có vai trò KTT/GĐKD; thay phụ trách; KTT tự lập điều chỉnh; thiếu người thay đúng role | Không tự approve/reject dù đúng vai trò; bàn giao không đổi creator; loại hai người lập; thiếu người hợp lệ giữ Draft, không tự nâng/hạ cấp |
| T08 | Submit lặp cùng key; khác key tại PendingApproval; hai submit cùng revision đồng thời; mất phản hồi sau commit | Một submission/round/outbox, trả receipt hiện hành; retry không đổi tiền; key khác payload lỗi; không tạo thông báo do bấm Trình lại |
| T09 | Approve/reject đồng thời; approve/reassign đồng thời; người cũ quyết định sau thay | Chỉ một kết quả hợp lệ; version/người nhận được kiểm lại dưới khóa; người cũ mất quyền; audit chính xác |
| T10 | Reject thiếu/chuỗi trắng; reject hợp lệ; trình/hủy trực tiếp Rejected; reopen rồi trình không sửa | Thiếu lý do chặn; hợp lệ vào Rejected; phải reopen Draft có audit/revision mới; lần trình mới có round mới, giữ lịch sử cũ |
| T11 | Sửa note; thêm/xóa dòng; sửa quantity/price/contract/period; mở xem | Note giữ tiền/căn cứ và audit trước-sau; tài chính/kỳ/dòng thủ công bị chặn; mở xem không tính lại/audit thay đổi giả |
| T12 | Nguồn đổi quantity hoặc điều kiện nhưng cùng tổng; đổi mô tả thuần túy; lỗi mạng/không nhất quán; candidate hết hạn/đổi nguồn trước accept | Thay đổi ảnh hưởng chặn trình; mô tả không ảnh hưởng không buộc tính lại; lỗi nguồn chặn/chờ; accept chỉ toàn bộ candidate còn hợp lệ, không cập nhật ngầm |
| T13 | Nguồn đổi/invalid/unavailable trước approve | Đổi ảnh hưởng/invalid chặn duyệt và cảnh báo, người được giao tự reject có lý do; unavailable giữ PendingApproval, không tự từ chối |
| T14 | Quý vs tháng trùng; hai lập đồng thời; quý chèn lỗi tháng thứ ba; đổi cấu hình kỳ tương lai | Không chồng tháng, một yêu cầu thắng; lỗi bất kỳ tháng rollback toàn bộ; cấu hình chỉ áp tương lai chưa chiếm, chuyển tháng/quý đúng ranh giới đã chốt |
| T15 | Hủy Draft không lý do/sai phụ trách; hủy Pending/Approved/Rejected; lập mới sau Cancelled; hủy điều chỉnh | Chỉ Draft đúng quyền/có lý do; Cancelled không dùng lại; bảng mới mã mới/tham chiếu/lý do và kiểm nguồn/kỳ lại; hủy điều chỉnh không giải phóng tháng gốc |
| T16 | 480→510 triệu; điều chỉnh giảm; điều chỉnh thiếu biên bản; hai điều chỉnh đồng thời; base cũ | Tổng mới đầy đủ quyết định role; hiệu lực chỉ sau approve, 510 và delta +30, không 990; delta âm được nhưng tổng mới hợp lệ; chặn thiếu biên bản/slot trùng/stale base; Rejected vẫn giữ slot |
| T17 | Kênh gửi lỗi; worker crash sau commit/trước gửi/sau delivery; retry +1/+5/+14; worker trở lại sau 15 phút; đổi người/đã quyết định | Giữ PendingApproval; inbox còn yêu cầu; cùng reference, tối đa 3 retry trong cửa sổ; quá hạn chuyển vận hành; dừng thông báo lỗi thời; chấp nhận khả năng delivery lặp, link kiểm quyền hiện hành |
| T18 | Lỗi audit/DB lúc lưu; đọc audit sau mọi lệnh; sửa quyền DB; tải biên bản trái quyền/metadata thiếu/blob mất | Không commit nghiệp vụ thiếu audit; append-only, đủ actor/time/diff; không đọc trái quyền; thiếu chứng cứ chặn trình/duyệt điều chỉnh; không lộ URL tùy ý |
| T19 | Bàn giao trước duyệt; gửi lại Approved; điều chỉnh đang xử lý; kế toán đã nhận bản gốc | Không bàn giao trước Approved; cùng mã bảng/lần duyệt không tính hai lần; tạm dừng bàn giao mới khi có điều chỉnh chưa duyệt; KTT phối hợp thủ công, không sửa hóa đơn trong module |
| T20 | Phép đo NFR-PAY-02 theo PAY-DEC-010 | Bộ 1 và 199 dòng trước tách, có 199→trên199; mỗi bộ ở tải 1 và 10 người, mỗi người 30 lượt trên kỳ chưa có bảng. Ghi mọi thời gian đầu-cuối gồm mạng/nguồn/lưu/render; mọi lượt thành công ≤3 giây, lỗi/timeout không đạt |
| T21 | AC-US-PAY-04-04; xét riêng điều chỉnh Draft/PendingApproval/Rejected, kể cả xem bản Approved/Cancelled cũ | Mỗi trạng thái đều giữ vị trí toàn chuỗi, A10 chặn tạo khác; A02 trả activeAdjustmentId đúng hồ sơ và handoffBlockedByAdjustment=true; tạm dừng bàn giao mới, bản Approved hiện hành không đổi |
| T22 | AC-US-PAY-04-05; Rejected → mở lại Draft → hủy có lý do; lập mới nếu đủ guard | Mở lại vẫn giữ vị trí/cờ dừng; cancel mới kết thúc vị trí hồ sơ đó, audit và giữ claim kỳ gốc/currentApproved. Không khôi phục Cancelled; nếu không còn hồ sơ khác đang xử lý, được tạo mã điều chỉnh mới từ Approved hiện hành và tiếp tục bàn giao thủ công |
| T23 | AC-US-PAY-04-06; duyệt điều chỉnh A rồi tạo B; mở lại bản A hoặc Cancelled cũ để thử tạo C/đối chiếu bàn giao | Duyệt A dùng tổng/căn cứ mới, không cộng lặp; chỉ khi toàn chuỗi trống mới được tạo B. Khi B Draft/PendingApproval/Rejected, cờ vẫn true khi xem A, chặn C và dừng bàn giao mới; Approved/Cancelled cũ không bỏ qua guard toàn chuỗi |
| T24 | AC-US-PAY-02-06; Draft 100 triệu, nguồn đổi ảnh hưởng thành 110 triệu rồi trình | 409 SOURCE_CHANGED và báo diff; giữ Draft/revision/căn cứ/100 triệu, không submission/outbox. Chỉ preview+accept chủ động mới đổi thành 110 triệu; trình ở lệnh sau |
| T25 | AC-US-PAY-02-07; không đọc được nguồn trước trình | 503 SOURCE_UNAVAILABLE, nêu nguồn chưa kiểm tra được; giữ Draft, tiền/căn cứ/version tài chính, không PendingApproval hoặc submission/outbox |
| T26 | AC-US-PAY-03-09; nguồn đổi ảnh hưởng hoặc invalid trước duyệt, xét riêng hai biến thể | 409 SOURCE_CHANGED hoặc 422 SOURCE_INVALID, cảnh báo người đang được giao; giữ PendingApproval và nội dung/tiền/snapshot lần trình. Không tự đổi tiền/reject; chỉ lệnh reject có lý do rồi reopen mới đi quy trình sửa |
| T27 | AC-US-PAY-03-10; không đọc được nguồn trước duyệt | 503 SOURCE_UNAVAILABLE; giữ PendingApproval, submission Open và revision/tiền/căn cứ lần trình để chờ; không tự Approved/Rejected |

Test T20 ghi cấu hình CPU/RAM/DB, phiên bản triển khai, topology/độ trễ/khả dụng nguồn, số dòng đầu vào/ra, concurrency và từng lần đo từ thao tác UI đến Draft đã lưu hiển thị. Không chỉ đo handler API, không dùng màn hình chờ làm điểm kết thúc. 200/trên200 dòng khảo sát riêng và không kết luận đạt SLA chưa được yêu cầu.

## 4. Điều kiện hoàn thành triển khai và review

- Quy tắc/tên trạng thái/lớp/API/schema nhất quán, đủ 37 AC, 5 US, 8 PAY rule, 2 NFR và 15 PAY-DEC; tài liệu 06 ánh xạ đến T01-T20.
- Contract/integration/concurrency test chạy trên implementation/DB đã chọn; có báo cáo NFR với dữ liệu/môi trường thật; không dùng kiểm tra Markdown thay kết quả ứng dụng.
- Tích hợp nguồn/quyền/biên bản/thông báo vượt qua DEP tương ứng; audit cùng transaction; dữ liệu Approved/revision bất biến; khôi phục được dữ liệu và không mất outbox sau restart.
- Giữ baseline thiết kế đã duyệt và hồ sơ review lịch sử; dùng bản BA đã đối chiếu R002 làm đầu vào hiện hành. Reviewer ở workflow mới chỉ review BA, không yêu cầu mở vòng review thiết kế. Nếu được giao lưu mốc thiết kế mới thì lưu riêng, không đổi baseline_commit/revised_commit của R002.

## 5. Kiểm tra thực tế của lượt Design

Kết quả kiểm tra tài liệu và render được cập nhật tại cuối lượt trong [README](README.md) và PRM-007 ở [nhật ký prompt](../../process/prompt-log.md). PRM-012 cập nhật theo BA sau R002; kết quả kiểm tra mới được ghi riêng trong nhật ký, không nhận lại kết quả PRM-007 cho bản sửa. Đây là tự kiểm tra của Design Agent, không là review độc lập/phê duyệt hoặc kết quả kiểm thử ứng dụng. Lượt soạn PRM-007 chưa tạo commit/baseline; việc lưu mốc được người dùng giao tại PRM-009, định danh bằng tag `payment-management-design-baseline-v1`. Commit `72ec6f0` có sẵn chỉ chứa hướng dẫn, không được dùng như baseline của bộ thiết kế này.

## 6. Báo cáo tác động và bàn giao cập nhật sau BA R002

### 6.1 Cố định đầu vào và quyền quyết định

- BA revised_commit: `fb1160f459da7c65e475651af3bba1dc6a0de477`; ba hồ sơ hiện hành đã đối chiếu văn bản bằng phiên bản tại commit này. Hồ sơ bằng chứng R002 được lưu tại `431f2ed` (Git history), không lẫn với mốc thiết kế.
- Đã đọc [review-record](../../reviews/payment-management/R002/review-record.md), [findings](../../reviews/payment-management/R002/findings.md) mục 4, [đánh giá](../../reviews/payment-management/R002/team-evaluation.md) và [changes](../../reviews/payment-management/R002/changes.md). R002 Đã đối chiếu ở phạm vi BA; những đoạn chờ BA trong findings/team-evaluation được giữ như lịch sử theo review-record hiện hành, không tự sửa nhận xét của người khác.
- PAY-DEC-015 xác nhận bổ sung bởi người dùng ngày 12/11/2026, PRM-011; F002 hoàn thiện AC theo PAY-DEC-012, không xác nhận lại nghiệp vụ. F003 Thừa, không tạo yêu cầu mới. Không còn câu hỏi nghiệp vụ chặn hai phần F001/F002.

### 6.2 Kết luận đối chiếu thiết kế trước và sau

| Đầu vào BA mới | Thiết kế trước đã có | Phần sửa/bổ sung cần bàn giao |
| --- | --- | --- |
| PAY-DEC-015; F001 | ADR-006, slot Draft/PendingApproval/Rejected; approve/cancel giải phóng, Approved bất biến | Dẫn căn cứ xác nhận mới, bảng đủ 5 trạng thái, giữ vị trí khi reopen và chỉ clear đúng hồ sơ; A02 đọc flag toàn chuỗi kể cả bản cũ; T21-T23 |
| Bốn AC F002 | SourceGateway/snapshot; SOURCE_CHANGED/INVALID/UNAVAILABLE giữ đúng trạng thái | Mapping từng AC tới A06/A07, Q2/Q3, dữ liệu không đổi và T24-T27; ví dụ Draft 100 → nguồn 110, không tự tính lại |
| 7 AC mới; tổng 37 | Ma trận chỉ liệt kê 30 AC cũ | Giữ mã cũ, thêm 7 dòng riêng và finding → quyết định → AC → API/UML/data → test; mở rộng bảng test lên 27 nhóm |
| Workflow BA → review phân tích → BA sửa → Design | Metadata cũ vẫn chỉ đến PRM-006 và chờ review thiết kế | Dẫn BA revised_commit/R002, phân biệt phê duyệt baseline cũ với trạng thái Design Approved của bản cập nhật theo PRM-013 |

Giữ 5 US/8 PAY rule/2 NFR, 19 endpoint, 5 trạng thái, 8 ADR, bộ entity và 5 DEP. Không cần thêm chức năng bàn giao kế toán, bảng/claim mới, module hoặc công nghệ. Bổ sung response A02 `activeAdjustmentId` để giải thích cờ đã có; không có request ghi mới. Không sửa đầu vào BA/PDF, R001/R002, hồ sơ mốc hoặc tag lịch sử; chưa tạo commit trong lượt này.

Bản sửa đã được người dùng phê duyệt tại PRM-013 ngày 12/11/2026 sau khi đọc báo cáo tóm tắt; phê duyệt này được ghi nhận riêng với baseline cũ. Kiểm chứng tài liệu/render trong PRM-012 không thay test ứng dụng hoặc nghiệm thu NFR.
