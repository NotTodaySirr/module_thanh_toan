# Đối chiếu phân tích BA sau review

Module: payment-management. Vòng: R002. Hai SHA được lưu trong `review-record.md` sau khi đã có commit thật.
Giai đoạn: Phân tích. BA cập nhật báo cáo này sau xử lý các nhận xét đã được chấp nhận, trước bàn giao Design.

Trạng thái: **Đã đối chiếu - BA xử lý sau review ngày 12/11/2026, Asia/Bangkok**. Đánh giá/phương án của người dùng tại [findings.md](findings.md) mục 4; [team-evaluation.md](team-evaluation.md) dẫn về cùng căn cứ. BA không thay đổi hai file này để khớp kết quả sửa.

- `baseline_commit`: `44c586c47105a9107af9673ff56604e83bd07ece`.
- `revised_commit`: `fb1160f459da7c65e475651af3bba1dc6a0de477` - commit thật chỉ chứa ba hồ sơ BA đã sửa.
- Người thực hiện: Codex, vai trò BA; người dùng giao việc và chấp thuận hướng xử lý, chưa cung cấp tên cá nhân. Nhật ký: PRM-011 tại [prompt-log.md](../../../process/prompt-log.md).
- Các đường dẫn BA dưới đây tính từ `docs/analysis/payment-management/`: [requirements-analysis.md](../../../analysis/payment-management/requirements-analysis.md), [issues.md](../../../analysis/payment-management/issues.md), [decisions.md](../../../analysis/payment-management/decisions.md). Repository root tại review-record.md.

## 1. Mapping nhận xét và thay đổi

| Finding ID | Quyết định từ team-evaluation.md | Quyết định nghiệp vụ liên quan | File/mục thay đổi | Trước → Sau | Kiểm tra thực tế và kết quả | Trạng thái/điểm còn mở |
| --- | --- | --- | --- | --- | --- | --- |
| payment-management-R002-A001 | Đúng; giữ phương án, bổ sung F001 | PAY-DEC-001/015 | requirements-analysis mục 4.4/6.1/8.3; issues mục 7.2; decisions mục 4.15 | Chưa rõ “đang xử lý” → tập Draft/PendingApproval/Rejected và điều kiện toàn chuỗi | Bảng năm trạng thái khớp xác nhận; AC-US-PAY-04-04 đến 06 có truy vết | Đã xử lý qua F001 |
| payment-management-R002-A002 | Đúng; giữ nguyên | PAY-DEC-002 | Không sửa riêng | Giữ từ 500 triệu giao GĐKD, một cấp | AC-US-PAY-03-01/02/03 giữ nguyên so baseline | Đã đối chiếu, không chọn lại |
| payment-management-R002-A003 | Đúng; giữ luồng mở lại | PAY-DEC-003/015 | decisions mục 4.3/4.15; requirements-analysis mục 4.4/6.1 | Giữ Rejected → Draft chủ động; bổ sung Rejected của điều chỉnh vẫn giữ vị trí | AC-US-PAY-03-06 giữ nguyên; AC-US-PAY-04-05 kiểm mở lại rồi hủy | Đã đồng bộ F001 |
| payment-management-R002-A004 | Đúng; làm rõ nhãn | PAY-DEC-004 | issues mục 1/2 | “Không nhất quán về kỳ” → “Chưa rõ cách kết hợp quy tắc”; không coi là cấm quý | Quyết định và AC chống chồng tháng giữ nguyên | Đã biên tập nhãn, không đổi nghiệp vụ |
| payment-management-R002-A005 | Đúng; giữ nguyên | PAY-DEC-005 | Không sửa riêng | Giữ nguồn hợp đồng/phụ lục, phân biệt rỗng/thiếu/0 đã đối soát | Dòng quyết định và AC-US-PAY-02-03/04 giữ nguyên | Đã đối chiếu; dữ liệu thực tế còn phải cung cấp |
| payment-management-R002-A006 | Đúng; giữ nguyên | PAY-DEC-006 | Không sửa riêng | Giữ sửa ghi chú, tài chính cập nhật chủ động từ nguồn, audit | AC-US-PAY-01-06 và AC-NFR-PAY-01-02 giữ nguyên | Đã đối chiếu |
| payment-management-R002-A007 | Đúng; bổ sung F001 | PAY-DEC-007/015 | requirements-analysis mục 4.5/6.1; decisions mục 4.7/4.15; issues mục 7.2 | Hủy điều chỉnh không giải phóng kỳ gốc; bổ sung kết thúc giữ vị trí và quyền tạo tiếp | AC-US-PAY-04-05/06; không hủy trực tiếp Rejected | Đã xử lý qua F001 |
| payment-management-R002-A008 | Đúng; giữ nguyên | PAY-DEC-008 | Không sửa riêng | Giữ giá theo ngày dịch vụ, tách dòng, không tự chia đều | Dòng quyết định và AC chọn giá giữ nguyên | Đã đối chiếu; phân bổ nguồn còn phải cung cấp |
| payment-management-R002-A009 | Đúng; giữ nguyên | PAY-DEC-009 | Không sửa riêng | Giữ 6 số lẻ/HALF_UP/tổng chính thức và kiểm tổng âm trước làm tròn | Dòng quyết định và AC tính tiền/tổng 0/biên giữ nguyên | Đã đối chiếu, không thêm thuế/phí |
| payment-management-R002-A010 | Đúng; giữ nguyên | PAY-DEC-010 | Không sửa riêng | Giữ phép đo, hồ sơ tải và biên dưới 200 dòng | Hai NFR và ba AC-NFR giữ nguyên so baseline | Đã đối chiếu tài liệu; chưa thực đo |
| payment-management-R002-A011 | Đúng; giữ nguyên | PAY-DEC-011 | Không sửa riêng | Giữ phân công/độc lập/thay thế cùng thẩm quyền | Dòng quyết định và AC quyền giữ nguyên | Đã đối chiếu; danh sách người thực tế còn phải cung cấp |
| payment-management-R002-A012 | Đúng; bổ sung F002 | PAY-DEC-012 | requirements-analysis mục 4.2/4.3/5/8.3; issues mục 7.3; decisions mục 4.12 | Đã có quy tắc nhưng AC thiếu ngoại lệ → bốn AC mới | AC-US-PAY-02-06/07, AC-US-PAY-03-09/10 có Cho trước/Khi/Thì và mapping | Đã xử lý qua F002 |
| payment-management-R002-A013 | Đúng; bổ sung F001 | PAY-DEC-013/015 | requirements-analysis mục 4.3/6.1; decisions mục 4.13/4.15; issues mục 7.2 | Giữ bàn giao thủ công; làm rõ dừng/tiếp tục sau từ chối/hủy/duyệt điều chỉnh | Cùng bảng năm trạng thái và AC-US-PAY-04-04 đến 06 | Đã xử lý qua F001 |
| payment-management-R002-A014 | Đúng; giữ nguyên | PAY-DEC-014 | Không sửa riêng | Giữ PendingApproval khi gửi lỗi, gửi lại cùng tham chiếu, giới hạn 3 lần/15 phút | Dòng quyết định và AC-US-PAY-02-05 giữ nguyên | Đã đối chiếu |
| payment-management-R002-F001 | Đúng có giới hạn; áp bảng trạng thái đã xác nhận | PAY-DEC-015 bổ sung 001/003/007/013 | Cả ba hồ sơ BA; requirements-analysis mục 6.1/8.3; decisions mục 4.15; issues mục 7.2 | Chưa định nghĩa tập trạng thái → bảng đủ năm trạng thái, điều kiện giữ vị trí/tạo tiếp và bàn giao mới | Hai bảng BA khớp từng dòng với bảng người dùng chấp thuận; ba AC mới phủ giữ vị trí/mở-hủy/duyệt và toàn chuỗi | Đã xử lý trong tài liệu; không kết luận hệ thống từng chắc chắn bị kẹt/tạo trùng |
| payment-management-R002-F002 | Đúng; bổ sung AC theo quy tắc đã có | PAY-DEC-012 giữ nguyên | requirements-analysis mục 4.2/4.3/8.3; issues mục 7.3; decisions mục 4.12 | Chỉ có AC nhánh hợp lệ → bốn AC nguồn đổi/không đọc được trước trình/duyệt | Kiểm kết quả Draft/100 triệu, PendingApproval/nội dung lần trình, thông báo và không tự duyệt/từ chối; đủ mapping | Đã xử lý trong tài liệu; chưa kiểm thử ứng dụng |
| payment-management-R002-F003 | Thừa; không mở việc riêng, dùng tùy chọn biên tập đã cho phép | Không có quyết định nghiệp vụ mới | requirements-analysis mục 5; issues mục 7.4 ghi lý do | Nhãn “giữ nguyên” → “Tóm lược phát biểu nguồn” | Tám ô nội dung nguồn giữ nguyên so baseline, chỉ đổi nhãn/lời dẫn | Đã biên tập khi sửa tài liệu; giữ nguyên đánh giá Thừa |

Ghi cả nhận xét không sửa và lý do. Nếu chỉ ghi nhận mâu thuẫn chứ chưa giải quyết, thể hiện đúng; không báo đã sửa hết. Mọi thay đổi ngoài findings phải ghi căn cứ nhiệm vụ riêng và tác động.

## 2. Bằng chứng Git

Chạy tại repository root `C:/Users/Admin/OneDrive/Documents/HCMUS/Year4-S1/MISD/Template/QTKD-Agent-Seminar/module_thanh_toan`:

```powershell
git diff --stat 44c586c47105a9107af9673ff56604e83bd07ece fb1160f459da7c65e475651af3bba1dc6a0de477 -- docs/analysis/payment-management
git diff --numstat 44c586c47105a9107af9673ff56604e83bd07ece fb1160f459da7c65e475651af3bba1dc6a0de477 -- docs/analysis/payment-management
git diff --check 44c586c47105a9107af9673ff56604e83bd07ece fb1160f459da7c65e475651af3bba1dc6a0de477 -- docs/analysis/payment-management
git show --format=fuller --stat fb1160f459da7c65e475651af3bba1dc6a0de477
```

Kết quả thực tế: ba file BA, **146 dòng thêm/40 dòng xóa**; decisions 30/4, issues 45/11, requirements-analysis 71/25. `diff --check` exit 0. Commit sửa chỉ chứa ba file này. Baseline giữ nguyên; không amend/rebase/reset hoặc push. Git author theo cấu hình sẵn có, không suy author là người xác nhận nghiệp vụ.

Đầu lượt HEAD là `7a21fe7a75f6d3b9293dbfa52453305757394ffc`, index trống; R001/R002 có sẵn nhưng chưa track. Commit hồ sơ sau commit BA chỉ lưu R002 cùng hai file process thuộc nhiệm vụ; không đưa R001 vào commit. Findings và team-evaluation được lưu đúng nội dung có sẵn, không phải AI tạo lại hoặc đổi đánh giá trong lượt BA.

## 3. Tính nhất quán và giới hạn

Kiểm tra bằng Python runtime cục bộ và đối chiếu nội dung: **25/25 mục đạt**. Các kiểm tra cấu trúc/truy vết không thay thế đọc nghiệp vụ hoặc kiểm thử ứng dụng.

- Giữ 5 US, 8 PAY rule, 2 NFR; 30 AC cũ nguyên nội dung và mã, thêm đúng 7 AC thành 37. Mọi AC có Cho trước/Khi/Thì, nguồn và quyết định.
- Giữ 14 mã vấn đề; 14 dòng xác nhận ban đầu và toàn bộ lịch sử đề xuất PRM-005 trong issues mục 6 giữ nguyên. PAY-DEC-015 là xác nhận bổ sung thật từ findings mục 4.2.
- Bảng đủ Draft/PendingApproval/Rejected/Approved/Cancelled trong requirements-analysis và decisions khớp bảng người dùng chấp thuận. Bảy AC mới có finding → quyết định/vấn đề → US → AC → kết quả mong đợi tại requirements-analysis mục 8.3.
- Bốn nhánh F002 giữ đúng khác biệt: trước trình giữ Draft/số tiền đã lưu; trước duyệt giữ nội dung lần trình, nguồn đổi thì chặn và thông báo để người được giao từ chối có lý do; không đọc được nguồn thì chờ, không tự Approved/Rejected.
- Kiểm liên kết nội bộ có đích file, số cột bảng Markdown, Unicode và khoảng trắng; không phát hiện lỗi. Kiểm hash trước/sau xác nhận không sửa PDF, thiết kế, instructions, R001, findings hoặc đánh giá nhóm. Phạm vi thay đổi đúng ba hồ sơ BA, changes/review-record R002 và hai file process.

Đã đọc đủ ba trang PDF bằng pypdf và render/mở ba PNG cục bộ. PDF giữ SHA-256 `8514a086a403ac4b1b57eee4993894230677525fc66d19f7cda9c827d057e436`. Script/snapshot/kết quả QA tạm ở `../../tmp/ba-R002/` tính từ repository root; lệnh và chi tiết tại review-record mục 5.

## 4. Bàn giao Design và trạng thái vòng

**R002 đã đối chiếu ở phạm vi BA**: có đánh giá người dùng, phương án được chấp thuận, bản phân tích sửa, SHA thật, mapping và kết quả kiểm tra. Đây không phải lượt Reviewer độc lập mới hoặc phê duyệt/triển khai thiết kế.

Design nhận ba hồ sơ BA tại revised_commit trên và hồ sơ R002, áp dụng requirements-analysis mục 9.4. Cần đồng bộ vòng đời điều chỉnh và guard toàn chuỗi, dừng/tiếp tục bàn giao, truy vết bốn AC nguồn ngoại lệ. Bộ thiết kế đã có giữ nguyên trong lượt này và chưa được tái phê duyệt theo R002.

Không còn lựa chọn nghiệp vụ đang chờ xác nhận cho F001/F002. Dữ liệu hợp đồng/giá/sản lượng, phân công người thật, quy trình bàn giao và môi trường đo vẫn là phụ thuộc thực hiện; thiếu thì áp đúng chặn/chờ, không tự nới quy tắc.

Đây là báo cáo thay đổi, không phải chứng nhận hiệu năng hay triển khai thành công của ứng dụng.
