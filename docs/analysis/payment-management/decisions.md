# Sổ xác nhận nghiệp vụ - payment-management

- Module: `payment-management`.
- Cập nhật BA: 07/11/2026, múi giờ Asia/Bangkok; xem PRM-006 trong [nhật ký prompt](../../process/prompt-log.md).
- Trạng thái hiện tại: **14 quyết định nghiệp vụ Đã xác nhận**, tương ứng PAY-ISS-001 đến PAY-ISS-014 trong [issues.md](issues.md).
- Căn cứ xác nhận: người dùng trong phiên chấp thuận các phương án PRM-005: “I read your proposed solutions, they are fine, write it down decision.md”. Chưa có tên cá nhân được cung cấp; không gán việc phê duyệt này cho KTT, GĐKD hoặc tổ chức ngoài hội thoại.
- Phạm vi chấp thuận: toàn bộ 14 phương án chi tiết tại mục 6.2-6.15 của issues.md, gồm điều kiện, ngoại lệ, ví dụ và các thông số được đề xuất. Ngày xác nhận theo ngữ cảnh phiên: 07/11/2026, Asia/Bangkok; không ghi giờ chưa biết.
- Nguồn nghiệp vụ: [btcohong.pdf](../../requirements/btcohong.pdf), 3 trang; đầu vào phân tích tại [requirements-analysis.md](requirements-analysis.md).

Các dòng dưới đây là xác nhận lựa chọn nghiệp vụ thực tế của người dùng, khác với yêu cầu lập hồ sơ PRM-004 và yêu cầu đề xuất PRM-005. Phần diễn giải chi tiết tại mục 4 áp dụng cùng phạm vi toàn văn phương án đã chấp thuận; không biến đề xuất kỹ thuật hoặc việc cung cấp dữ liệu/nhân sự thành kết quả đã triển khai.

## 1. Xác nhận thực tế

| Mã xác nhận | Mã vấn đề | Nội dung được xác nhận và phạm vi áp dụng | Người xác nhận | Thời điểm nếu biết | Căn cứ phiên/prompt/tài liệu | Tài liệu cần cập nhật | Thay thế xác nhận nào |
| --- | --- | --- | --- | --- | --- | --- | --- |
| PAY-DEC-001 | PAY-ISS-001 | Giữ nguyên Approved; KTT lập hồ sơ điều chỉnh liên kết, có biên bản và duyệt riêng theo tổng đầy đủ sau điều chỉnh; không tự duyệt hoặc cộng lặp tổng. Toàn bộ mục 6.2 PRM-005; chi tiết mục 4.1 dưới đây | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-03/04; PAY-05; NFR-PAY-01; AC-US-PAY-04-01 đến 03, AC-NFR-PAY-01-02; quyền/trạng thái, Design và truy vết | Không |
| PAY-DEC-002 | PAY-ISS-002 | KTT duyệt tổng chính thức dưới 500 triệu; GĐKD duyệt từ 500 triệu, một cấp theo ngưỡng. Toàn bộ mục 6.3 PRM-005; chi tiết mục 4.2 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-02/03; PAY-06; AC-US-PAY-02-01, AC-US-PAY-03-01 đến 03; phân cấp/thông báo và truy vết | Không |
| PAY-DEC-003 | PAY-ISS-003 | Từ chối về Rejected có lý do; người phụ trách chủ động chuyển Draft trước sửa/trình lại. Toàn bộ mục 6.4 PRM-005; chi tiết mục 4.3 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/02/03; PAY-08; NFR-PAY-01; AC-US-PAY-03-06, AC-US-PAY-01-06, AC-US-PAY-02-01; trạng thái/quyền và truy vết | Không |
| PAY-DEC-004 | PAY-ISS-004 | Kỳ tháng/quý dương lịch theo hợp đồng; mỗi tháng chỉ thuộc một bảng chưa Cancelled; kiểm soát đổi cấu hình/kỳ lẻ. Toàn bộ mục 6.5 PRM-005; chi tiết mục 4.4 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/05; PAY-01; AC-US-PAY-01-01/07/08, AC-US-PAY-05-03; mô hình kỳ và truy vết | Không |
| PAY-DEC-005 | PAY-ISS-005 | Danh sách dịch vụ bắt buộc từ hợp đồng/phụ lục hiệu lực; phân biệt rỗng, thiếu dữ liệu và sản lượng 0 đã đối soát. Toàn bộ mục 6.6 PRM-005; chi tiết mục 4.5 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/02; PAY-04; AC-US-PAY-02-01/03/04; dữ liệu nguồn và truy vết | Không |
| PAY-DEC-006 | PAY-ISS-006 | NVKD chỉ sửa ghi chú Draft; số liệu tài chính cập nhật chủ động từ nguồn hợp lệ; audit mọi lần lưu thay đổi. Toàn bộ mục 6.7 PRM-005; chi tiết mục 4.6 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01; PAY-03; NFR-PAY-01; AC-US-PAY-01-06, AC-NFR-PAY-01-02; quyền/dữ liệu và truy vết | Không |
| PAY-DEC-007 | PAY-ISS-007 | Người phụ trách hủy Draft có lý do; Cancelled giải phóng kỳ, lập bảng mã mới có liên kết lịch sử; không khôi phục bản cũ. Toàn bộ mục 6.8 PRM-005; chi tiết mục 4.7 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/05; PAY-01; AC-US-PAY-05-01 đến 03, AC-US-PAY-01-07/08; quyền/kỳ và truy vết | Không |
| PAY-DEC-008 | PAY-ISS-008 | Giá theo ngày dịch vụ và hợp đồng; tách sản lượng theo khoảng hiệu lực, không tự chia đều/chọn giá mới nhất. Toàn bộ mục 6.9 PRM-005; chi tiết mục 4.8 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/04; PAY-01/PAY-03; AC-US-PAY-01-01/03/05, AC-US-PAY-04-01; nguồn giá và truy vết | Không |
| PAY-DEC-009 | PAY-ISS-009 | VND, đầu vào tối đa 6 số lẻ, HALF_UP từng dòng đến đồng rồi cộng; kiểm tra cả tổng âm trước làm tròn. Toàn bộ mục 6.10 PRM-005; chi tiết mục 4.9 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01 đến 04; PAY-03/PAY-04/PAY-06; AC tính tiền, tổng 0 và biên 500 triệu tại PAY-ISS-009; truy vết | Không |
| PAY-DEC-010 | PAY-ISS-010 | Đo từ chọn Lập đến hiển thị Draft đã lưu, bao gồm nguồn/mạng; thông qua hồ sơ tải 1/10 người, 30 lượt/người. Toàn bộ mục 6.11 PRM-005; chi tiết mục 4.10 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01; NFR-PAY-02; AC-NFR-PAY-02-01; kế hoạch đo và truy vết | Không |
| PAY-DEC-011 | PAY-ISS-011 | Một người duyệt chính và người thay thế đúng thẩm quyền theo đơn vị; cấm người lập tự duyệt/từ chối; không đủ người thì chặn trình. Toàn bộ mục 6.12 PRM-005; chi tiết mục 4.11 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/02/03; PAY-06/PAY-07/PAY-08; AC quyền/phân công tại PAY-ISS-011; thông báo và truy vết | Không |
| PAY-DEC-012 | PAY-ISS-012 | Giữ căn cứ đã xác nhận, kiểm tra lại trước trình/duyệt; nguồn đổi thì chặn và cập nhật có chủ động; ngoại lệ Active cho điều chỉnh kỳ cũ có điều kiện. Toàn bộ mục 6.13 PRM-005; chi tiết mục 4.12 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-01/02/03/04; PAY-01/PAY-02/PAY-03; AC-US-PAY-01-01, AC-US-PAY-02-01, AC-US-PAY-03-01 đến 03, AC-US-PAY-04-01; truy vết | Không |
| PAY-DEC-013 | PAY-ISS-013 | KTT bàn giao thủ công có bằng chứng gửi/nhận; đối chiếu đúng lần duyệt và chuỗi điều chỉnh, hóa đơn ngoài module. Toàn bộ mục 6.14 PRM-005; chi tiết mục 4.13 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | Ranh giới module; US-PAY-03/04; AC-US-PAY-03-08; bàn giao/đối chiếu và truy vết | Không |
| PAY-DEC-014 | PAY-ISS-014 | Lỗi gửi giữ PendingApproval; gửi lại tối đa 3 lần/15 phút rồi vận hành theo dõi; không tạo lần trình trùng, dừng thông báo lỗi thời. Toàn bộ mục 6.15 PRM-005; chi tiết mục 4.14 | Người dùng trong phiên; chưa cung cấp tên | 07/11/2026, Asia/Bangkok; chưa biết giờ | PRM-006, chấp thuận PRM-005 | US-PAY-02; PAY-04; NFR-PAY-01; AC-US-PAY-02-01/05; ngoại lệ thông báo và truy vết | Không |

Chỉ thêm dòng khi có câu trả lời hoặc tài liệu xác nhận thực tế. Dùng mã `PAY-DEC-001`...; ghi cả ngoại lệ và điều kiện đi kèm. Nếu chưa biết người/thời điểm thì nói rõ, không tự đặt tên hoặc ngày.

Với một vấn đề được trả lời một phần, chỉ ghi phạm vi thật sự được xác nhận và giữ phần còn thiếu mở. Xác nhận rằng PDF có mâu thuẫn không đồng nghĩa xác nhận phương án giải quyết.

## 2. Khi thay đổi yêu cầu

Giữ dòng quyết định cũ, thêm dòng mới chỉ rõ thay thế. Phân tích tác động tới US/AC, UML, API, schema, truy vết và kế hoạch kiểm chứng. Dùng vòng review tiếp theo nếu cần, không sửa lịch sử Git đã dùng làm bằng chứng. Quyết định kỹ thuật nằm trong ADR của bộ thiết kế; chỉ liên kết đến đây nếu có lựa chọn nghiệp vụ đã xác nhận.

## 3. Các yêu cầu cách làm đã có căn cứ

Các mục sau không dùng mã PAY-DEC vì không chốt nghiệp vụ thanh toán:

| Yêu cầu cách làm | Căn cứ thực tế | Cách áp dụng |
| --- | --- | --- |
| Viết đầu ra bằng tiếng Việt | Người dùng: “The language for the outputs should be Vietnamese”; PRM-003 | Tiếng Việt cho nội dung, giữ tên file/mã/trạng thái cần thiết |
| Lập hồ sơ đầy đủ, giữ vấn đề chưa rõ ở Cần xác nhận | Người dùng chọn “Document open issues (Recommended)” khi lập kế hoạch; PRM-002 | Bàn giao phần rõ cho Design và giữ các nhánh chưa chốt có mã vấn đề |
| Chỉ thực hiện phạm vi BA đã nêu, không tạo commit/baseline | Người dùng yêu cầu “PLEASE IMPLEMENT THIS PLAN:” kèm kế hoạch; PRM-004 | Ba tài liệu BA và prompt-log; không tự mở rộng sang Design/Reviewer/triển khai |

Không chuyển các yêu cầu cách làm trên thành kết luận về người duyệt tại 500 triệu, điều chỉnh Approved, trạng thái từ chối hoặc tính duy nhất của kỳ.

## 4. Nội dung quyết định đã xác nhận

Mỗi mục sau diễn giải phương án tương ứng đã được chấp thuận toàn bộ trong PRM-006. Lý do, đánh đổi và ví dụ nguyên gốc được giữ tại mục 6 của [issues.md](issues.md) để truy vết. Các lời “cần xác nhận”, “đề nghị”, “nếu chấp thuận” tại bản đề xuất PRM-005 mô tả thời điểm trước phê duyệt; trạng thái hiện hành theo sổ này. Không yêu cầu người dùng xác nhận lại cùng lựa chọn. Việc cung cấp danh sách dịch vụ, dữ liệu nguồn, nhân sự được phân công và môi trường đo là việc chuẩn bị thực hiện, không phải 14 quyết định còn mở.

### 4.1 PAY-DEC-001 - Điều chỉnh bằng hồ sơ liên kết

- Bản Approved không bị sửa. KTT lập hồ sơ điều chỉnh gắn với bản Approved hiện hành, đủ lý do, nội dung trước/sau, chênh lệch và biên bản có mã/ngày/các bên xác nhận; biên bản bắt buộc trước trình. Một chuỗi chỉ có một hồ sơ điều chỉnh đang xử lý.
- Hồ sơ chứa toàn bộ dòng/tổng sau điều chỉnh, đi qua Draft → PendingApproval → Approved hoặc Rejected; người lập KTT trình và sửa lý do/biên bản, số liệu phải từ nguồn đã sửa/kiểm tra. Dùng tổng đầy đủ sau điều chỉnh để phân cấp, loại cả người lập gốc và người lập điều chỉnh khỏi quyền duyệt/từ chối theo PAY-DEC-011.
- Chỉ điều chỉnh Approved mới có hiệu lực; kế thừa bản Approved gần nhất, không cộng lặp toàn bộ các bản. Hồ sơ bị từ chối/hủy không đổi giá trị đã duyệt, không chiếm thêm tháng. Chênh lệch giảm có thể âm nhưng tổng mới phải hợp lệ theo PAY-DEC-009. Hóa đơn xử lý ngoài module.
- Ví dụ đã chấp thuận: 480 → 510 triệu phải do GĐKD duyệt; sau duyệt tổng hiện hành 510 triệu, chênh lệch +30 triệu; không thành 990 triệu. Không có biên bản thì chặn trình; KTT lập không được tự duyệt kể cả dưới ngưỡng.

### 4.2 PAY-DEC-002 - Ngưỡng duyệt bao gồm 500 triệu

- Tổng chính thức dưới 500.000.000 đồng do KTT duyệt; từ 500.000.000 đồng trở lên do GĐKD duyệt. Một cấp theo ngưỡng, không thêm KTT duyệt trước GĐKD.
- Dùng tổng sau làm tròn theo PAY-DEC-009, cố định theo lần trình. Các tổng 499.999.999 / 500.000.000 / 500.000.001 đồng lần lượt giao KTT / GĐKD / GĐKD; vẫn kiểm tra độc lập người lập.

### 4.3 PAY-DEC-003 - Rejected trước khi mở lại Draft

- Người được giao từ chối phải nhập lý do không rỗng; PendingApproval → Rejected, giữ nội dung đã trình và lý do. Không tự về Draft; không sửa/trình trực tiếp tại Rejected.
- NVKD phụ trách chọn “Chỉnh sửa lại” để chuyển Rejected → Draft rồi mới sửa/cập nhật/hủy/trình lại. KTT làm tương tự với hồ sơ điều chỉnh mình phụ trách. Audit cả hai chuyển trạng thái, giữ lịch sử cũ; lần trình mới kiểm tra lại và phân công theo tổng hiện hành. Giữ đủ năm trạng thái nguồn.

### 4.4 PAY-DEC-004 - Kỳ tháng/quý không chồng tháng

- Kỳ là tháng hoặc quý dương lịch theo cấu hình hợp đồng hiệu lực; quý bao phủ ba tháng. Mỗi tháng/hợp đồng chỉ thuộc tối đa một bảng chưa Cancelled, bao gồm Draft/PendingApproval/Rejected/Approved. Hồ sơ điều chỉnh cùng chuỗi không chiếm tháng lần hai; yêu cầu lập đồng thời cũng phải bảo đảm duy nhất.
- Không đổi kỳ của bảng đã lập. Cấu hình mới chỉ áp kỳ tương lai chưa bị chiếm; tháng sang quý từ đầu quý tiếp theo chưa bị chiếm, quý sang tháng sau kỳ quý đã lập. Có bảng tháng trong quý thì chặn lập quý chồng lên.
- Kỳ lẻ đầu/cuối chỉ lập nếu nguồn hợp đồng đã cung cấp khoảng thời gian/cách phân bổ; thiếu thì chặn để làm rõ, không tính đủ kỳ. Hủy Draft quý giải phóng ba tháng nhưng bảng thay thế vẫn phải phù hợp cấu hình và điều kiện hiện tại.

### 4.5 PAY-DEC-005 - Danh sách dịch vụ bắt buộc theo hợp đồng

- Nguồn là hợp đồng/phụ lục hiệu lực trong kỳ, do nghiệp vụ hợp đồng cung cấp; từng dịch vụ bắt buộc phải có dòng bao phủ đúng thời gian áp dụng. Bảng giá không tự xác định nghĩa vụ bắt buộc.
- Sản lượng 0 chỉ dùng khi nguồn đã đối soát xác nhận 0. Không biến thiếu sản lượng thành 0. Danh sách được nguồn xác nhận rỗng khác với không đọc được danh sách; trường hợp chưa xác định phải chặn trình và chỉ rõ dữ liệu thiếu.
- Ngoại lệ phải có ở hợp đồng/phụ lục, NVKD không tự bỏ dịch vụ bắt buộc. Không thêm chức năng quản lý danh mục vào module thanh toán. Thiếu B trong hợp đồng bắt buộc A/B thì chặn; có dòng B bằng 0 hợp lệ thì không chặn vì thiếu B.

### 4.6 PAY-DEC-006 - Phạm vi sửa Draft và audit

- NVKD chỉ sửa ghi chú chung/từng dòng. Không sửa trực tiếp hợp đồng, kỳ, mã dịch vụ, đơn vị tính, sản lượng, đơn giá, thành tiền, tổng; không thêm/xóa dòng bằng tay. Sai hợp đồng/kỳ thì hủy và lập mới.
- Sai số liệu thì chủ quản nguồn sửa, NVKD chủ động xem/chấp nhận cập nhật toàn bộ căn cứ liên quan, tính lại theo PAY-DEC-012. Sửa ghi chú không tính lại tiền.
- Mọi lần lưu thay đổi Draft đều audit ai/lúc nào/nội dung trước-sau, kể cả ghi chú; mở xem không là thay đổi. KTT sửa thêm lý do/biên bản trên hồ sơ điều chỉnh nhưng không ghi đè số liệu thiếu căn cứ. Ngoại lệ ghi đè tài chính mới phải có quyết định bổ sung.

### 4.7 PAY-DEC-007 - Hủy Draft và lập bảng thay thế

- NVKD phụ trách được hủy Draft của mình; người được bàn giao chính thức kế thừa quyền xử lý và lưu lịch sử bàn giao. KTT chỉ có quyền tương ứng trên Draft điều chỉnh mình phụ trách, không mặc nhiên hủy mọi bảng gốc. Bắt buộc lý do hủy/audit.
- Không hủy PendingApproval/Approved/Rejected; Rejected phải được đưa về Draft đúng quyền trước. Cancelled không phục hồi hoặc sử dụng lại.
- Cancelled giải phóng kỳ; được lập bảng mã mới liên kết bảng hủy/lý do lập lại, kiểm tra lại Active/giá/đối soát/cấu hình/chồng kỳ và lập đồng thời. Hủy điều chỉnh không giải phóng kỳ của bảng gốc Approved.

### 4.8 PAY-DEC-008 - Giá theo ngày dịch vụ

- Chọn bảng giá được hợp đồng chỉ định, đúng dịch vụ/đơn vị tính, hiệu lực tại ngày cung cấp dịch vụ. Đổi giá giữa kỳ thì tách sản lượng đã đối soát theo khoảng hiệu lực thành nhiều dòng; không dùng ngày bấm lập/trình để chọn giá.
- Thiếu giá hoặc nhiều giá cùng ưu tiên thì chặn lập/cập nhật, yêu cầu chủ quản xử lý. Không tự chọn giá mới nhất. Chỉ có tổng kỳ mà không phân bổ được theo khoảng đổi giá thì yêu cầu phân bổ đã đối soát, không tự chia đều.
- Sửa ghi chú không chọn lại giá; cập nhật Draft chủ động tính lại; Approved chỉ thay bằng điều chỉnh. Ví dụ 10 × 100.000 và 20 × 120.000 → hai dòng, tổng 3.400.000 đồng.

### 4.9 PAY-DEC-009 - Độ chính xác, tổng và làm tròn

- Dùng VND. Sản lượng/đơn giá tối đa 6 số lẻ; giữ độ chính xác đầu vào, dữ liệu vượt giới hạn không tự làm tròn mà báo chủ quản để thống nhất. Tính tích chính xác, HALF_UP từng dòng đến 1 đồng: độ lớn phần lẻ từ 0,5 làm tròn ra xa 0, dưới 0,5 về số nguyên gần nhất.
- Tổng chính thức bằng tổng các dòng đã làm tròn, không làm tròn lại tổng; dùng cùng tổng cho hiển thị/trình/phân cấp/bàn giao. Không thêm thuế/phí/chiết khấu ngoài công thức.
- Chặn trình nếu tổng chính thức âm hoặc tổng các tích trước làm tròn âm; tổng chính thức 0 vẫn được trình nếu điều kiện khác đạt. Điều kiện kiểm tra trước làm tròn là bổ sung đã được người dùng chấp thuận, không nhận là lời nguyên gốc PAY-04.
- Ví dụ: 100,5 + 100,5 → 101 + 101 = 202 đồng; tích -0,4 → 0 nhưng chặn trình; hai tích 249.999.999,5 → tổng 500 triệu, GĐKD duyệt.

### 4.10 PAY-DEC-010 - Phép đo phản hồi lập bảng

- Đo từ chọn “Lập bảng” sau nhập hợp đồng/kỳ hợp lệ đến giao diện hiển thị Draft đã lưu. Gồm mạng trong môi trường đo, đọc nguồn, kiểm tra, tính tiền, lưu và hiển thị; không gồm thời gian người dùng nhập/chọn hoặc thao tác trình/gửi thông báo.
- Đếm dòng dịch vụ hợp đồng áp dụng trong kỳ trước tách giá; ghi riêng dòng thanh toán đầu ra. Dùng bộ 1/199 dòng, có trường hợp 199 dòng sinh trên 199 dòng thanh toán; tải 1 và 10 người lập đồng thời, mỗi người 30 lượt trên hợp đồng/kỳ chưa có bảng.
- Mọi lượt thành công phải không quá 3 giây; lỗi/timeout không đạt. Ghi cấu hình, dữ liệu, độ trễ/khả dụng nguồn, dòng đầu vào/ra và từng lần đo; không loại thời gian nguồn. Đúng/trên 200 dòng khảo sát riêng, chưa áp ngưỡng mới. Hồ sơ tải đã được chấp thuận nhưng chưa có kết quả thực đo.

### 4.11 PAY-DEC-011 - Phân công và độc lập người xử lý

- Mỗi hợp đồng thuộc đơn vị, có NVKD phụ trách hoặc người được bàn giao chính thức để sửa/trình/hủy Draft; giữ danh tính người lập ban đầu. Quyền xem theo đơn vị được quy định riêng, không tự suy ra quyền sửa. Điều chỉnh có KTT phụ trách.
- Mỗi đơn vị có người duyệt chính/thay thế cho vai trò KTT/GĐKD. Chọn đúng vai trò theo tổng, loại người lập bảng và người lập điều chỉnh, giao người chính hợp lệ hoặc người thay thế đủ cùng thẩm quyền. Thiếu người hợp lệ thì giữ Draft/chặn trình/báo đầu mối phân công; không tự nâng/hạ cấp.
- Chỉ người đang được giao được duyệt hoặc từ chối; cả hai phải độc lập với người lập. Vắng mặt sau trình thì đầu mối có thẩm quyền giao lại người cùng vai trò, audit/thông báo người mới, hết quyền người cũ; không thêm vòng duyệt/trình lại. Thao tác đồng thời chỉ ghi một kết quả. Danh sách cá nhân/ủy quyền phải được đơn vị cung cấp.

### 4.12 PAY-DEC-012 - Kiểm tra lại dữ liệu nguồn

- Giữ căn cứ/kết quả lần lập hoặc cập nhật được người phụ trách xác nhận; mở xem/sửa ghi chú không đổi tiền. Trước trình kiểm tra lại Active, đối soát, cấu hình kỳ, dịch vụ bắt buộc, sản lượng, bảng giá/hiệu lực cho đúng hợp đồng/kỳ.
- Nguồn đổi ảnh hưởng tính hợp lệ/số tiền thì giữ Draft, chặn trình, thông báo khác biệt; người phụ trách phối hợp sửa nguồn, xem/chấp nhận cập nhật toàn bộ tập căn cứ liên quan, tính lại/audit rồi trình lần sau. Nguồn không đọc được/không nhất quán thì chặn; mô tả nguồn thay đổi không ảnh hưởng không bắt tính lại.
- Cố định nội dung/căn cứ lần trình. Trước duyệt thấy thay đổi ảnh hưởng hoặc nguồn không hợp lệ thì chặn duyệt/thông báo người được giao; người đó từ chối có lý do để về quy trình sửa Draft. Nguồn không đọc được thì giữ PendingApproval chờ kiểm tra, không tự từ chối. Approved không bị đổi hồi tố.
- Ngoại lệ cho hợp đồng đã kết thúc: chỉ lập điều chỉnh kỳ cũ nếu kỳ dịch vụ thuộc thời gian hợp đồng hiệu lực, sản lượng/giá sửa đã xác nhận, đủ biên bản; không áp ngoại lệ Active này cho lập bảng thông thường. Cách lưu/so sánh căn cứ thuộc Design.

### 4.13 PAY-DEC-013 - Bàn giao kế toán thủ công có bằng chứng

- Giai đoạn đầu bàn giao ngoài module. KTT đơn vị kiểm tra/gửi cho đầu mối kế toán được chỉ định sau Approved, lưu bằng chứng gửi/nhận bằng quy trình vận hành. Không thêm trạng thái bảng/API; chưa giả định module có xuất file. Kế toán chỉ xuất hóa đơn sau đối chiếu căn cứ đã duyệt.
- Gửi mã bảng/lần duyệt hoặc điều chỉnh, hợp đồng/khách hàng/kỳ, các dòng sản lượng/giá/thành tiền/tổng chính thức, người/thời điểm duyệt, tài liệu; điều chỉnh thêm bản tham chiếu, tổng cũ/mới, chênh lệch/biên bản. Mã bảng + lần duyệt/điều chỉnh giúp đối chiếu gửi lại, không tính tiền hai lần.
- Chưa nhận xác nhận tiếp nhận thì KTT theo dõi/gửi lại cùng tham chiếu; Approved không là bằng chứng đã bàn giao. Có điều chỉnh chưa duyệt thì tạm dừng bàn giao mới của chuỗi; gốc đã gửi thì báo kế toán phối hợp, chỉ gửi kết quả mới sau duyệt. Không cộng toàn bộ tổng mới vào tổng cũ. Hóa đơn đã xuất do kế toán xử lý ngoài module; tự động hóa cần yêu cầu riêng.

### 4.14 PAY-DEC-014 - Lỗi thông báo và trình trùng

- Lần trình đã ghi nhận thành công, đạt điều kiện và có người duyệt hợp lệ thì giữ PendingApproval khi gửi lỗi. Báo NVKD “Đã trình duyệt; thông báo đang chờ gửi lại”, không báo gửi thành công hoặc đưa Draft chỉ vì lỗi gửi. Người duyệt xem được yêu cầu trong danh sách chờ.
- Gửi lại tự động tối đa 3 lần trong 15 phút sau lỗi đầu; hết giới hạn giao đầu mối vận hành theo dõi và cho gửi lại thủ công cùng lần trình. Lưu lịch sử gửi/lỗi; không coi lỗi kênh là lỗi dữ liệu bảng.
- Bấm Trình ở PendingApproval trả lần trình hiện hữu, không tạo vòng duyệt/yêu cầu gửi mới. Gửi lại theo cùng tham chiếu lần trình/người đang được giao; dừng thông báo cũ khi Approved/Rejected/Cancelled hoặc đã đổi người nhận. Kênh có thể giao lặp nhưng cùng tham chiếu phải nhận biết là một yêu cầu; không cam kết đúng một thông báo mọi kênh. Cơ chế kỹ thuật thuộc Design; thiếu người duyệt vẫn chặn từ Draft theo PAY-DEC-011.

## 5. Bàn giao và giới hạn của việc xác nhận

- Xác nhận giải quyết 14 lựa chọn nghiệp vụ; không xác nhận hệ thống đã triển khai, module nguồn đã cung cấp đủ dữ liệu, người cụ thể đã được phân công hoặc phép đo đã đạt.
- Hồ sơ phân tích/trạng thái/truy vết được đồng bộ theo PAY-DEC-001 đến PAY-DEC-014; PDF giữ nguyên để còn bằng chứng mâu thuẫn nguồn. Design tiếp tục theo các quyết định và xác định hợp đồng dữ liệu/kỹ thuật; không tự coi đề xuất outbox, loại số triển khai hoặc công nghệ là đã được chốt.
- Thay đổi sau này ghi quyết định mới chỉ rõ thay thế quyết định nào, không ghi đè lịch sử PRM-005/006. Không có quyết định cũ bị thay thế trong lượt xác nhận đầu tiên này.
