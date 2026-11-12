# Sổ vấn đề đặc tả - payment-management

- Trạng thái hồ sơ: **14 vấn đề Đã xác nhận phương án xử lý**, BA cập nhật sau R002 theo PRM-011. Giữ PAY-DEC-001 đến PAY-DEC-014 của PRM-006; bổ sung PAY-DEC-015 làm rõ F001, thêm AC cho F002.
- Nguồn nghiệp vụ: [btcohong.pdf](../../requirements/btcohong.pdf), đủ 3 trang, trang tính từ 1. Mọi vị trí PDF bên dưới chỉ về file này.
- Lượt cập nhật sau review: BA ngày 12/11/2026, múi giờ Asia/Bangkok; xem PRM-011 trong [nhật ký](../../process/prompt-log.md), [findings R002](../../reviews/payment-management/R002/findings.md) mục 4 và phần bổ sung tại mục 7 dưới đây. Lượt xác nhận đầu là PRM-006 ngày 07/11/2026.
- Lịch sử: 11 vấn đề PAY-ISS-001 đến PAY-ISS-011 là phát hiện khởi tạo do AI soạn trong bộ hướng dẫn. Lượt BA này đã kiểm chứng lại và bổ sung phân tích; giữ nguyên mã và nội dung trọng tâm. PAY-ISS-012 đến PAY-ISS-014 là phát hiện bổ sung trong lượt này.
- Người dùng đã chấp thuận toàn bộ 14 phương án PRM-005 trong PRM-006; xem nội dung, căn cứ và phạm vi tại [decisions.md](decisions.md). Không cần xác nhận lại các lựa chọn đã chấp thuận.
- Lượt đề xuất PRM-005 ngày 07/11/2026: **đã có khuyến nghị BA cho đủ 14 vấn đề tại mục 6**, gồm quy tắc đề nghị, lý do/đánh đổi, ví dụ kiểm chứng và bên nên xác nhận. Đây là lịch sử đề xuất trước phê duyệt; PRM-006 đã chấp thuận toàn bộ. Giữ nguyên nội dung nguồn để truy vết.

## 1. Danh sách và trạng thái

| Mã | Loại | Chủ đề | Nguồn PDF | Trạng thái |
| --- | --- | --- | --- | --- |
| PAY-ISS-001 | Mâu thuẫn | Điều chỉnh Approved hay cấm sửa | Trang 2 mục 4.4 US-PAY-04; mục 5 PAY-05 | Đã xác nhận - PAY-DEC-001; bổ sung PAY-DEC-015 sau F001 |
| PAY-ISS-002 | Mâu thuẫn tại biên | Người duyệt đúng 500 triệu | Trang 2 mục 4.3 US-PAY-03; mục 5 PAY-06 | Đã xác nhận - PAY-DEC-002 |
| PAY-ISS-003 | Không nhất quán trạng thái | Rejected hay Draft sau từ chối | Trang 1 mục 3; trang 2 mục 5 PAY-08 | Đã xác nhận - PAY-DEC-003; bổ sung PAY-DEC-015 sau F001 |
| PAY-ISS-004 | Chưa rõ cách kết hợp quy tắc | Kỳ quý và một bảng mỗi tháng | Trang 1 mục 2; trang 2 mục 5 PAY-01 | Đã xác nhận - PAY-DEC-004 |
| PAY-ISS-005 | Thiếu thông tin | Danh sách dịch vụ bắt buộc | Trang 2 mục 5 PAY-04 | Đã xác nhận - PAY-DEC-005 |
| PAY-ISS-006 | Mơ hồ | Phạm vi sửa Draft | Trang 2 mục 4.1 US-PAY-01; trang 3 mục 6 | Đã xác nhận - PAY-DEC-006 |
| PAY-ISS-007 | Thiếu thông tin | Quyền hủy và lập bảng thay thế | Trang 2 mục 4.5 US-PAY-05; mục 5 PAY-01 | Đã xác nhận - PAY-DEC-007; bổ sung PAY-DEC-015 sau F001 |
| PAY-ISS-008 | Mơ hồ | Chọn giá và thời điểm tính | Trang 2 mục 5 PAY-01, PAY-03 | Đã xác nhận - PAY-DEC-008 |
| PAY-ISS-009 | Thiếu thông tin | Độ chính xác, làm tròn và tổng tiền | Trang 2 mục 4.1, 4.3; mục 5 PAY-03, PAY-04, PAY-06 | Đã xác nhận - PAY-DEC-009 |
| PAY-ISS-010 | Mơ hồ về phép đo | Phản hồi 3 giây/dưới 200 dòng | Trang 3 mục 6 | Đã xác nhận - PAY-DEC-010 |
| PAY-ISS-011 | Thiếu thông tin | Phân công và phạm vi quyền | Trang 1 mục 2; trang 2 mục 4.1-4.3; mục 5 PAY-06, PAY-07 | Đã xác nhận - PAY-DEC-011 |
| PAY-ISS-012 | Thiếu thông tin | Dữ liệu nguồn đổi sau khi lập Draft | Trang 2 mục 4.1, 4.2, 4.4; mục 5 PAY-01 đến PAY-03 | Đã xác nhận - PAY-DEC-012; bổ sung AC theo F002 |
| PAY-ISS-013 | Thiếu thông tin | Chuyển bảng sang kế toán | Trang 1 mục 1; trang 2 mục 4.3 | Đã xác nhận - PAY-DEC-013; bổ sung PAY-DEC-015 sau F001 |
| PAY-ISS-014 | Thiếu luồng ngoại lệ | Gửi thông báo thất bại và trình lại | Trang 2 mục 4.2; mục 5 PAY-04 | Đã xác nhận - PAY-DEC-014 |

Các câu hỏi và phương án tại mục 2-4 ghi lại phát hiện/lựa chọn trước khi chốt; nhãn Trạng thái dưới mỗi vấn đề là kết quả hiện hành. Mục 6 giữ lịch sử đề xuất PRM-005 đã được người dùng chấp thuận toàn bộ trong PRM-006. Áp dụng PAY-DEC tương ứng tại [decisions.md](decisions.md), không coi các nhánh tham khảo khác là phương án hiện hành. Các mã AC tham chiếu [requirements-analysis.md](requirements-analysis.md).

## 2. Bốn điểm trọng tâm đã kiểm chứng

### PAY-ISS-001 - Điều chỉnh Approved

- **Vị trí và hai phát biểu:** PDF trang 2 mục 4.4 US-PAY-04 cho KTT điều chỉnh Approved nếu có biên bản xác nhận đính kèm, phải ghi log; mục 5 PAY-05 cấm sửa Approved dưới bất kỳ hình thức nào.
- **Loại:** Mâu thuẫn trực tiếp; không tự phân biệt “điều chỉnh” với “sửa” để bỏ qua mâu thuẫn.
- **Câu hỏi:** Cho sửa bảng gốc, lập hồ sơ điều chỉnh riêng hay không cho điều chỉnh? Nếu cho phép, trạng thái sau xử lý, việc duyệt lại và yêu cầu đối với biên bản là gì?
- **Lý do hỏi:** Các nhánh dẫn tới quyền, giá trị đã duyệt và vòng đời khác nhau; nguồn không cho ưu tiên US hơn rule hoặc ngược lại.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Sửa bản gốc có biên bản/audit, cần làm rõ ngoại lệ PAY-05. (B) Giữ nguyên bản gốc, lập hồ sơ điều chỉnh liên kết, cần bổ sung quy trình. (C) Cấm điều chỉnh, cần cập nhật US-PAY-04. Chưa chọn nhánh nào.
- **Ảnh hưởng:** US-PAY-04 và đầu ra US-PAY-03; PAY-05, NFR-PAY-01; AC-US-PAY-04-01 đến AC-US-PAY-04-03, AC-NFR-PAY-01-02; quyền, trạng thái, API/dữ liệu và UML ở bước Design.
- **Trạng thái:** Đã xác nhận - PAY-DEC-001; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-002 - Chính xác 500.000.000 đồng

- **Vị trí và hai phát biểu:** PDF trang 2 mục 4.3 US-PAY-03 dùng “từ 500 triệu đồng trở lên” cho GĐKD; mục 5 PAY-06 dùng “trên 500 triệu” cho GĐKD, còn lại KTT.
- **Loại:** Mâu thuẫn chỉ tại đúng 500 triệu; dưới và trên biên thống nhất.
- **Câu hỏi:** Tổng đúng 500.000.000 đồng do GĐKD hay KTT duyệt?
- **Lý do hỏi:** Tự chọn `>=` hoặc `>` có thể giao sai thẩm quyền và người nhận thông báo.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) GĐKD duyệt từ 500 triệu, sửa PAY-06. (B) GĐKD chỉ duyệt trên 500 triệu, sửa US-PAY-03. Không dùng cấu hình để né xác nhận.
- **Ảnh hưởng:** US-PAY-02, US-PAY-03; PAY-06; AC-US-PAY-02-01, AC-US-PAY-03-01 đến AC-US-PAY-03-03; phân cấp, thông báo và kiểm chứng biên. PAY-ISS-009 ảnh hưởng cách xác định tổng.
- **Trạng thái:** Đã xác nhận - PAY-DEC-002; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-003 - Trạng thái sau từ chối

- **Vị trí và hai phát biểu:** PDF trang 1 mục 3 nói chuyển Rejected khi từ chối và liệt kê trạng thái này; trang 2 mục 5 PAY-08 nói về Draft để NVKD sửa và trình lại.
- **Loại:** Không nhất quán về trạng thái, thiếu hành động nối tiếp.
- **Câu hỏi:** Sau từ chối lưu Rejected hay Draft? Nếu đi qua cả hai, ai thực hiện hành động nào, vào thời điểm nào để chuyển về Draft? NVKD sửa/trình lại từ đâu?
- **Lý do hỏi:** Kết quả từ chối, quyền sửa và audit phải nhất quán; không tự xóa Rejected hoặc thêm bước chuyển.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Về Draft ngay, lưu hành động từ chối trong lịch sử và làm rõ lại ý nghĩa Rejected. (B) Lưu Rejected rồi có hành động được xác nhận để về Draft. (C) Sửa/trình trực tiếp từ Rejected, cần cập nhật quyền và PAY-08. Chưa chọn nhánh.
- **Ảnh hưởng:** US-PAY-01, US-PAY-02, US-PAY-03; PAY-08, NFR-PAY-01; AC-US-PAY-03-06, AC-US-PAY-01-06, AC-US-PAY-02-01, AC-NFR-PAY-01-01; state diagram và quyền sửa/trình lại.
- **Trạng thái:** Đã xác nhận - PAY-DEC-003; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-004 - Kỳ tháng/quý và tính duy nhất

- **Vị trí và hai phát biểu:** PDF trang 1 mục 2 nêu kỳ tháng hoặc quý theo cấu hình hợp đồng; trang 2 mục 5 PAY-01 quy định mỗi hợp đồng chỉ có một bảng cho mỗi tháng.
- **Loại:** Chưa rõ cách kết hợp quy tắc kỳ và tính duy nhất; chưa đủ căn cứ kết luận hai phát biểu không thể cùng đúng hoặc phải cấm kỳ quý. Nhãn được làm rõ theo A004, không đổi PAY-DEC-004.
- **Câu hỏi:** Với kỳ quý, tính duy nhất theo quý hay từng tháng thuộc quý? Có cấm chồng kỳ tháng/quý không? Kiểm tra kỳ cũ thế nào nếu cấu hình hợp đồng đổi?
- **Lý do hỏi:** Tự đổi “mỗi tháng” thành “mỗi kỳ” là thay nghiệp vụ; ảnh hưởng chặn trùng và bao phủ sản lượng.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Một bảng theo kỳ cấu hình, bổ sung quy định chồng kỳ. (B) Bảng quý chiếm ba tháng tương ứng, kiểm tra từng tháng. Mỗi cách cần xác nhận chuyển cấu hình; chưa chốt khóa dữ liệu.
- **Ảnh hưởng:** US-PAY-01, US-PAY-05; PAY-01; AC-US-PAY-01-01, AC-US-PAY-01-07, AC-US-PAY-01-08, AC-US-PAY-05-03; mô hình kỳ, lập đồng thời, ràng buộc duy nhất; liên quan PAY-ISS-007.
- **Trạng thái:** Đã xác nhận - PAY-DEC-004; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

## 3. Thông tin mơ hồ hoặc thiếu đã kiểm chứng

### PAY-ISS-005 - Danh sách dịch vụ bắt buộc

- **Vị trí/phát biểu:** PDF trang 2 mục 5 PAY-04 yêu cầu chặn khi thiếu dòng dịch vụ bắt buộc nhưng không nêu danh sách hoặc nguồn tra cứu; không có phát biểu thứ hai mâu thuẫn.
- **Loại:** Thiếu thông tin.
- **Câu hỏi:** Dịch vụ nào bắt buộc, danh sách lấy từ đâu, có phụ thuộc hợp đồng/kỳ và thời điểm hiệu lực không?
- **Lý do hỏi:** Chưa thể xác định bảng đủ dòng hoặc chuẩn bị dữ liệu kiểm chứng cụ thể.
- **Phương án tham khảo trước xác nhận (lịch sử):** Dùng danh sách từ hợp đồng, từ bảng giá hoặc danh mục nghiệp vụ có chủ quản được xác nhận. Không tự tạo chức năng quản lý danh mục mới.
- **Ảnh hưởng:** US-PAY-01, US-PAY-02; PAY-04; AC-US-PAY-02-01, AC-US-PAY-02-03, AC-US-PAY-02-04; dữ liệu đọc và validation.
- **Trạng thái:** Đã xác nhận - PAY-DEC-005; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-006 - Phạm vi sửa Draft

- **Vị trí/phát biểu:** PDF trang 2 mục 4.1 US-PAY-01 cho NVKD sửa Draft, không nêu trường; trang 3 mục 6 yêu cầu audit mọi điều chỉnh.
- **Loại:** Mơ hồ. Lượt BA bổ sung câu hỏi “điều chỉnh” có bao gồm sửa Draft không.
- **Câu hỏi:** Được sửa sản lượng, đơn giá, thêm/bớt dòng, hợp đồng/kỳ hay chỉ mô tả? Có được ghi đè giá trị tự sinh? Tính lại phần nào? Sửa Draft có thuộc mọi điều chỉnh phải audit không?
- **Lý do hỏi:** Phạm vi sửa ảnh hưởng dữ liệu tính tiền và kiểm tra điều kiện lập; không thể suy ra mọi trường đều được sửa.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Chỉ sửa các trường mô tả được liệt kê. (B) Cho sửa một số dữ liệu tính tiền được liệt kê, kèm tính lại/audit được xác nhận. Cả hai cần danh mục trường rõ.
- **Ảnh hưởng:** US-PAY-01, sửa lại sau US-PAY-03; PAY-03, NFR-PAY-01; AC-US-PAY-01-06, AC-US-PAY-03-06, AC-NFR-PAY-01-02; dữ liệu, quyền và audit.
- **Trạng thái:** Đã xác nhận - PAY-DEC-006; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-007 - Quyền hủy và bảng thay thế

- **Vị trí/phát biểu:** PDF trang 2 mục 4.5 US-PAY-05 cho hủy Draft, chuyển Cancelled và không dùng lại; mục 5 PAY-01 nêu một bảng/hợp đồng/tháng. Không nêu actor hủy hoặc ngoại lệ tính duy nhất.
- **Loại:** Thiếu thông tin; “không dùng lại” không tự quyết định được quyền lập bảng mới.
- **Câu hỏi:** Ai được hủy, có giới hạn theo người lập/đơn vị không? Cancelled còn chiếm kỳ không? Có được lập bảng thay thế, cần liên kết lịch sử thế nào?
- **Lý do hỏi:** Có thể gán sai quyền hoặc chặn/cho tái lập sai ý định nghiệp vụ.
- **Phương án tham khảo trước xác nhận (lịch sử):** Quyền theo người lập hoặc vai trò cụ thể được nhóm xác nhận. Về tái lập: Cancelled vẫn chiếm kỳ hoặc được lập bảng mới theo cách đối chiếu lịch sử được xác nhận. Đây là hai nhóm lựa chọn riêng.
- **Ảnh hưởng:** US-PAY-05, US-PAY-01; PAY-01; AC-US-PAY-05-01 đến AC-US-PAY-05-03, AC-US-PAY-01-07, AC-US-PAY-01-08; quyền, kỳ và tính duy nhất.
- **Trạng thái:** Đã xác nhận - PAY-DEC-007; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-008 - Thời điểm chọn giá

- **Vị trí/phát biểu:** PDF trang 2 mục 5 PAY-01 yêu cầu bảng giá phù hợp; PAY-03 dùng đơn giá tại thời điểm tính. Không định nghĩa phù hợp theo hợp đồng/dịch vụ/thời gian hoặc xử lý đổi giá trong kỳ.
- **Loại:** Mơ hồ.
- **Câu hỏi:** Thời điểm tính là ngày lập, ngày dịch vụ hay mốc kỳ? Giá đổi giữa kỳ thì dùng một hay nhiều giá? Khi sửa/trình/điều chỉnh có chọn lại giá không?
- **Lý do hỏi:** Cùng sản lượng có thể ra số tiền khác nhau, kéo theo khác phân cấp duyệt.
- **Phương án tham khảo trước xác nhận (lịch sử):** Giá hiệu lực tại ngày lập/tính; giá theo ngày dịch vụ hoặc đoạn hiệu lực; giá của kỳ theo hợp đồng. Mốc áp dụng cần xác nhận, không tự chọn.
- **Ảnh hưởng:** US-PAY-01, US-PAY-04; PAY-01, PAY-03; AC-US-PAY-01-01, AC-US-PAY-01-03, AC-US-PAY-01-05, AC-US-PAY-04-01; dữ liệu đọc và lịch sử; liên quan PAY-ISS-009, PAY-ISS-012.
- **Trạng thái:** Đã xác nhận - PAY-DEC-008; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-009 - Độ chính xác, làm tròn và tổng tiền

- **Vị trí/phát biểu:** PDF trang 2 mục 4.1 yêu cầu tính tổng; mục 5 PAY-03 nêu phép nhân, PAY-04 kiểm tra tổng âm, PAY-06 dùng tổng phân cấp; không nêu độ chính xác/làm tròn.
- **Loại:** Thiếu thông tin.
- **Câu hỏi:** Số chữ số thập phân của các giá trị, phương pháp làm tròn dòng/tổng là gì? Tổng có phải tổng thành tiền các dòng không? Tổng dùng trình và phân cấp là trước hay sau làm tròn?
- **Lý do hỏi:** Làm tròn ảnh hưởng tổng âm/0 và biên 500 triệu. Không tự thêm thuế, chiết khấu hoặc phí ngoài nguồn.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Làm tròn từng dòng rồi cộng. (B) Giữ độ chính xác được xác nhận khi nhân/cộng rồi làm tròn tổng. Cần chốt định nghĩa tổng và phương pháp; chưa chọn kiểu số triển khai.
- **Ảnh hưởng:** US-PAY-01 đến US-PAY-04; PAY-03, PAY-04, PAY-06; AC-US-PAY-01-01, AC-US-PAY-01-05, AC-US-PAY-01-06, AC-US-PAY-02-02, AC-US-PAY-02-04, AC-US-PAY-03-01 đến AC-US-PAY-03-03; tính tiền và kiểm chứng biên.
- **Trạng thái:** Đã xác nhận - PAY-DEC-009; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-010 - Phép đo 3 giây

- **Vị trí/phát biểu:** PDF trang 3 mục 6 yêu cầu lập bảng phản hồi trong vòng 3 giây với hợp đồng có dưới 200 dòng; không nêu phép đo hoặc yêu cầu cho đúng/trên 200 dòng.
- **Loại:** Mơ hồ về phép đo và biên.
- **Câu hỏi:** Đo từ đâu đến đâu, có gồm đọc nguồn ngoài/lưu Draft không? Đếm dòng hợp đồng hay dòng thanh toán sinh ra? Điều kiện tải, độ trễ nguồn và cách đánh giá nhiều lần đo là gì? Có yêu cầu riêng cho đúng 200 và trên 200 không?
- **Lý do hỏi:** Không thể tuyên bố đạt phép đo chưa thống nhất; không đổi dưới 200 thành không quá 200.
- **Phương án tham khảo trước xác nhận (lịch sử):** Đo từ thao tác lập đến phản hồi Draft, hoặc đo trong ranh giới xử lý được nhóm xác định. Mỗi cách cần nêu thành phần tính trong 3 giây và điều kiện đo; không tự áp p95 hoặc tải bắt buộc mới.
- **Ảnh hưởng:** US-PAY-01; NFR-PAY-02; AC-NFR-PAY-02-01; dữ liệu đo và phụ thuộc nguồn đọc.
- **Trạng thái:** Đã xác nhận - PAY-DEC-010; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-011 - Phân công và phạm vi quyền

- **Vị trí/phát biểu:** PDF trang 1 mục 2 nêu vai trò; trang 2 mục 4.1-4.3 nêu lập/trình/thông báo người duyệt; mục 5 PAY-06 nêu vai trò duyệt, PAY-07 cấm tự duyệt. Không chỉ định cá nhân hoặc phạm vi bảng được xử lý.
- **Loại:** Thiếu thông tin. Lượt BA bổ sung phạm vi theo người lập/đơn vị và quyền tự từ chối bên cạnh câu hỏi khởi tạo về nhiều người duyệt/người thay thế.
- **Câu hỏi:** Nhiều KTT/GĐKD thì ai nhận thông báo và được xử lý? Người lập cũng có vai trò duyệt thì ai thay thế? NVKD sửa/trình bảng của mình hay cả đơn vị? Phân cấp và cấm tự xử lý có áp cho từ chối như duyệt không?
- **Lý do hỏi:** PAY-07 không đủ để xác định người nhận cụ thể hoặc mở rộng lệnh cấm sang từ chối; không tự cho mọi người cùng vai trò xử lý mọi bảng.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Phân công cá nhân theo hợp đồng/đơn vị. (B) Nhóm người đủ quyền nhận và một người xử lý. Cần xác nhận cơ chế thay thế nếu không có người hợp lệ; không cho tự duyệt để giải quyết bế tắc.
- **Ảnh hưởng:** US-PAY-01 đến US-PAY-03; PAY-06, PAY-07, PAY-08; AC-US-PAY-01-06, AC-US-PAY-02-01, AC-US-PAY-03-01 đến AC-US-PAY-03-07; quyền và thông báo.
- **Trạng thái:** Đã xác nhận - PAY-DEC-011; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

## 4. Phát hiện bổ sung trong lượt BA

### PAY-ISS-012 - Dữ liệu nguồn thay đổi sau khi lập Draft

- **Vị trí/phát biểu:** PDF trang 2 mục 5 PAY-01/PAY-02 ràng buộc lúc lập, mục 4.1 cho sửa Draft, mục 4.2 chỉ nêu PAY-04 khi trình, PAY-03 dùng giá tại thời điểm tính. Chưa nêu xử lý thay đổi nguồn sau lập.
- **Loại:** Thiếu thông tin; không phải mâu thuẫn đã chứng minh.
- **Câu hỏi:** Khi sửa/trình/điều chỉnh có kiểm tra lại Active, đối soát và giá không? Nếu nguồn đổi thì giữ căn cứ đã lập, tính lại hay chặn để xử lý?
- **Lý do hỏi:** Giữ giá trị cũ và đọc lại có thể tạo tổng/điều kiện hợp lệ khác nhau; không tự biến điều kiện lúc lập thành điều kiện bắt buộc mọi bước.
- **Phương án tham khảo trước xác nhận (lịch sử):** Giữ căn cứ lần lập, chỉ kiểm tra lại các mục được xác nhận; hoặc kiểm tra lại ở các mốc rõ ràng và có chính sách xử lý thay đổi. Giữ bản chụp/đọc lại là giải pháp Design sẽ đề xuất sau, chưa phải yêu cầu bắt buộc.
- **Ảnh hưởng:** US-PAY-01, US-PAY-02, US-PAY-04; PAY-01, PAY-02, PAY-03; AC-US-PAY-01-01, AC-US-PAY-02-01, AC-US-PAY-04-01; tính lại/lịch sử; liên quan PAY-ISS-006, PAY-ISS-008.
- **Trạng thái:** Đã xác nhận - PAY-DEC-012; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-013 - Chuyển giao kế toán

- **Vị trí/phát biểu:** PDF trang 1 mục 1 yêu cầu duyệt trước chuyển kế toán xuất hóa đơn; trang 2 mục 4.3 dừng ở kết quả Approved, không nêu thao tác/dữ liệu bàn giao.
- **Loại:** Thiếu thông tin ở ranh giới module.
- **Câu hỏi:** Ai chuyển, chuyển ngay sau duyệt hay theo thao tác khác? Gửi dữ liệu gì, có xác nhận tiếp nhận không? Nếu điều chỉnh bảng đã chuyển thì thông báo/gửi lại ra sao?
- **Lý do hỏi:** Điều kiện trước chuyển rõ nhưng chưa đủ thiết kế hợp đồng tích hợp/trách nhiệm vận hành; xuất hóa đơn vẫn ngoài module.
- **Phương án tham khảo trước xác nhận (lịch sử):** Người có trách nhiệm bàn giao ngoài module; hoặc module hỗ trợ bàn giao tự động theo hợp đồng tích hợp được xác nhận. Cả hai giữ điều kiện duyệt trước chuyển và xem ảnh hưởng PAY-ISS-001.
- **Ảnh hưởng:** Ranh giới và đầu ra US-PAY-03, liên hệ US-PAY-04; AC-US-PAY-03-08; thiết kế tích hợp/handoff. Không tự thêm US thứ sáu hoặc trạng thái đã xuất hóa đơn.
- **Trạng thái:** Đã xác nhận - PAY-DEC-013; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

### PAY-ISS-014 - Ngoại lệ gửi thông báo

- **Vị trí/phát biểu:** PDF trang 2 mục 4.2 yêu cầu đổi PendingApproval và gửi thông báo khi hợp lệ; mục 5 PAY-04 chống trình trùng. Chưa nêu kết quả khi gửi lỗi hoặc thử lại.
- **Loại:** Thiếu luồng ngoại lệ; không mặc định một cơ chế giao dịch kỹ thuật.
- **Câu hỏi:** Nếu trạng thái đã đổi mà thông báo chưa gửi, giữ chờ duyệt rồi gửi lại hay coi lần trình chưa hoàn tất? Người dùng thấy gì, ai theo dõi gửi lỗi, thử lại có tạo thông báo thêm không?
- **Lý do hỏi:** Các cách xử lý dẫn tới trạng thái và kết quả trình lại khác nhau; không tự báo hoàn tất toàn bộ khi thông báo chưa đáp ứng mà chưa có chính sách.
- **Phương án tham khảo trước xác nhận (lịch sử):** (A) Giữ PendingApproval và xác định trách nhiệm gửi lại. (B) Chỉ coi trình hoàn tất khi đạt điều kiện thông báo được xác nhận, làm rõ trạng thái nếu thất bại. Không tự chọn retry/outbox hoặc cam kết gửi đúng một lần.
- **Ảnh hưởng:** US-PAY-02; PAY-04, NFR-PAY-01 khi đổi trạng thái; AC-US-PAY-02-01, AC-US-PAY-02-05; ngoại lệ và kiểm chứng trình trùng.
- **Trạng thái:** Đã xác nhận - PAY-DEC-014; người dùng chấp thuận toàn bộ phương án tương ứng tại mục 6 trong PRM-006. Nội dung áp dụng và ngoại lệ xem [decisions.md](decisions.md).

## 5. Cách cập nhật và bàn giao

- Mã vấn đề ổn định, mục mới tăng từ mã cuối; không xóa vấn đề đã giải quyết hoặc tái sử dụng mã.
- Chỉ chuyển `Cần xác nhận` sang `Đã xác nhận` khi có căn cứ thật trong [decisions.md](decisions.md). Xác nhận có vấn đề không đồng nghĩa đã chọn phương án.
- Nếu chỉ trả lời một phần, ghi phạm vi đã xác nhận và tiếp tục giữ phần còn thiếu mở; không đóng cả vấn đề chỉ vì có một câu trả lời.
- Quyết định phải liên kết US/AC/quy tắc/NFR và tài liệu cần rà soát. API/dữ liệu/UML nêu ở trên chỉ là phạm vi ảnh hưởng dự kiến, chưa được tạo trong lượt BA này.
- Thứ tự làm rõ do BA đề xuất: bốn mâu thuẫn trọng tâm, quyền/dữ liệu/tính tiền, sau đó tích hợp và phép đo. Chưa gán cá nhân hoặc nhóm xác nhận thay người dùng.
- Hiện có 14 vấn đề Đã xác nhận, 14 quyết định ban đầu và PAY-DEC-015 bổ sung sau R002. Mục 7 và [phân tích yêu cầu](requirements-analysis.md) ghi nội dung hiện hành; việc cung cấp dữ liệu/nhân sự và kiểm chứng thực tế vẫn cần thực hiện. Giữ mục 6 nguyên vẹn như lịch sử đề xuất, không dùng số lượng/kết quả lịch sử ở cuối mục 6 làm trạng thái sau R002.

## 6. Lịch sử đề xuất quyết định của BA - PRM-005

**Trạng thái khi soạn PRM-005: Đề xuất nghiệp vụ - Cần xác nhận. Trạng thái hiện hành PRM-006: Đã xác nhận toàn bộ theo PAY-DEC-001 đến PAY-DEC-014.**

Phần tiếp theo giữ nguyên lời đề xuất trước phê duyệt, gồm các cụm “chưa phê duyệt”, “cần xác nhận”, “nếu chấp thuận”; các cụm này là lịch sử, không mở lại quyết định đã chấp thuận. Người xác nhận thực tế là người dùng trong phiên; các vai trò được BA đề nghị dưới đây không phải bằng chứng KTT/GĐKD hoặc bên khác đã phê duyệt.

**Bối cảnh nguyên gốc PRM-005:** Người dùng yêu cầu BA đề xuất cách giải quyết; chưa yêu cầu áp dụng hoặc phê duyệt các lựa chọn. Các quy tắc, con số và trách nhiệm bổ sung dưới đây là khuyến nghị của AI, không phải nội dung đã có trong PDF. Phạm vi ảnh hưởng US/PAY/AC/NFR của từng vấn đề vẫn theo mục 2-4; ví dụ dưới đây là AC ứng viên để bổ sung sau xác nhận, chưa phải kết quả kiểm thử.

### 6.1 Tóm tắt phương án khuyến nghị

| Vấn đề | Quyết định BA đề nghị | Đánh đổi chính |
| --- | --- | --- |
| PAY-ISS-001 | Giữ nguyên Approved, KTT lập hồ sơ điều chỉnh liên kết và trình duyệt riêng | Thêm quy trình điều chỉnh và đối chiếu kế toán |
| PAY-ISS-002 | GĐKD duyệt tổng từ 500.000.000 đồng trở lên | Tăng trách nhiệm GĐKD tại đúng điểm biên |
| PAY-ISS-003 | Từ chối về Rejected; NVKD chủ động chuyển về Draft để sửa | Thêm một hành động nhưng giữ rõ kết quả từ chối |
| PAY-ISS-004 | Kỳ tháng/quý theo hợp đồng; cấm chồng bất kỳ tháng nào trong kỳ | Đổi cấu hình kỳ cần kiểm tra các tháng đã có bảng |
| PAY-ISS-005 | Lấy dịch vụ bắt buộc từ hợp đồng/phụ lục có hiệu lực | Cần nguồn cung cấp danh sách có chủ quản |
| PAY-ISS-006 | NVKD sửa ghi chú; dữ liệu tính tiền chỉ cập nhật từ nguồn đã kiểm tra | Sai dữ liệu phải xử lý tại module nguồn |
| PAY-ISS-007 | NVKD phụ trách được hủy Draft; Cancelled giải phóng kỳ, cho lập bảng mới | Cần liên kết lịch sử và kiểm tra lập đồng thời |
| PAY-ISS-008 | Giá theo ngày cung cấp dịch vụ; tách dòng theo khoảng hiệu lực | Cần sản lượng phân bổ được theo thời gian |
| PAY-ISS-009 | VND, tối đa 6 số lẻ cho đầu vào; làm tròn từng dòng đến đồng rồi cộng | Có thể khác cách làm tròn ở hệ thống kế toán |
| PAY-ISS-010 | Đo từ thao tác lập đến hiển thị Draft đã lưu, gồm thời gian đọc nguồn | Cần thống nhất môi trường và hồ sơ tải kiểm chứng |
| PAY-ISS-011 | Phân công một người duyệt theo đơn vị/vai trò, có người thay thế hợp lệ | Phải quản lý trách nhiệm thay thế và trường hợp thiếu người |
| PAY-ISS-012 | Giữ căn cứ đã lập; kiểm tra lại lúc trình, nguồn đổi thì chặn để cập nhật chủ động | Thêm bước xử lý thay đổi nhưng tránh âm thầm đổi tổng |
| PAY-ISS-013 | Giai đoạn đầu KTT chịu trách nhiệm bàn giao thủ công có bằng chứng tiếp nhận | Cần vận hành đối chiếu, chưa tự động hóa tích hợp |
| PAY-ISS-014 | Giữ PendingApproval khi lỗi thông báo; gửi lại cùng yêu cầu duyệt | Cần theo dõi lỗi gửi độc lập với trạng thái bảng |

### 6.2 PAY-ISS-001 - Điều chỉnh bằng hồ sơ liên kết

- **Quy tắc đề nghị:** Chọn phương án B. Bảng Approved và nội dung đã duyệt được giữ nguyên. KTT lập hồ sơ điều chỉnh gắn với bản Approved hiện hành, ghi lý do, nội dung trước/sau, chênh lệch và biên bản xác nhận có mã/ngày/các bên xác nhận. Biên bản phải có trước khi trình điều chỉnh. Chỉ có một hồ sơ điều chỉnh đang xử lý cho cùng chuỗi bảng; hồ sơ bị từ chối/hủy không thay đổi giá trị đã duyệt.
- **Duyệt và hiệu lực:** Hồ sơ có bản đầy đủ các dòng và tổng sau điều chỉnh, dùng vòng Draft → PendingApproval → Approved hoặc Rejected, với hành động quay lại Draft như mục 6.4. KTT là người lập hồ sơ và trình điều chỉnh, được sửa lý do/biên bản; số liệu tính tiền vẫn phải có căn cứ nguồn đã sửa và được kiểm tra lại. Tổng đầy đủ sau điều chỉnh xác định cấp duyệt theo PAY-ISS-002, không dùng riêng số chênh lệch. PAY-07 áp dụng cả người lập bảng gốc và người lập hồ sơ điều chỉnh; cần người duyệt khác hợp lệ theo PAY-ISS-011. Chỉ hồ sơ Approved mới có hiệu lực; bản mới kế thừa bản Approved gần nhất, không cộng lặp toàn bộ tổng các bản.
- **Lý do/đánh đổi:** Giữ được bằng chứng người duyệt đã nhìn thấy, đồng thời xử lý được sai sót sau duyệt. Đổi lại, US-PAY-04 phải được diễn giải lại thành lập điều chỉnh; PAY-05 nói rõ không sửa nội dung bản Approved. Hồ sơ điều chỉnh thuộc chuỗi của cùng bảng, không chiếm thêm tháng như một bảng thanh toán độc lập theo PAY-01. Chênh lệch giảm có thể âm, nhưng tổng sau điều chỉnh phải hợp lệ theo PAY-04/PAY-ISS-009. Việc xử lý hóa đơn đã xuất thuộc kế toán ngoài module.
- **Ví dụ kiểm chứng đề nghị:** Bảng gốc 480 triệu, điều chỉnh thành 510 triệu: gốc vẫn 480 triệu; gửi GĐKD duyệt tổng 510 triệu; trước duyệt chưa có thay đổi hiệu lực. Sau duyệt, căn cứ hiện hành là 510 triệu và chênh lệch +30 triệu, không phải 990 triệu. KTT lập điều chỉnh không được tự duyệt kể cả tổng dưới 500 triệu; thiếu biên bản thì chặn trình.
- **Bên nên xác nhận và tài liệu:** KTT, GĐKD, đại diện nghiệp vụ và kế toán nhận bàn giao; đây là vai trò đề nghị, chưa có cá nhân nhận nhiệm vụ. Nếu chấp thuận, cập nhật US-PAY-04/PAY-05, quyền/trạng thái và AC đã liệt kê tại PAY-ISS-001; đồng bộ quy tắc bàn giao ở PAY-ISS-013.

### 6.3 PAY-ISS-002 - GĐKD duyệt từ 500 triệu

- **Quy tắc đề nghị:** Chọn phương án A: tổng chính thức dưới 500.000.000 đồng do KTT duyệt; từ 500.000.000 đồng trở lên do GĐKD duyệt. Dùng tổng sau làm tròn theo PAY-ISS-009, được cố định trong lần trình. Đây là một cấp duyệt theo ngưỡng, không thêm bước KTT duyệt trước GĐKD.
- **Lý do/đánh đổi:** Quy ước bao gồm điểm biên dễ giải thích và không giao mức chạm ngưỡng cao cho cấp thấp hơn. Đây là lựa chọn kiểm soát BA khuyến nghị, không phải kết luận US có thẩm quyền cao hơn bảng rule; cần sửa cách diễn đạt PAY-06 nếu được chấp thuận.
- **Ví dụ kiểm chứng đề nghị:** Tổng 499.999.999 đồng → KTT; 500.000.000 đồng → GĐKD; 500.000.001 đồng → GĐKD. Nếu tính tiền tạo tổng chính thức 500.000.000 đồng sau làm tròn thì cũng gửi GĐKD. Người nhận vẫn phải đáp ứng điều kiện độc lập PAY-07/PAY-ISS-011.
- **Bên nên xác nhận và tài liệu:** KTT và GĐKD xác nhận phạm vi thẩm quyền; cập nhật PAY-06, US-PAY-02/03 và AC biên đã liệt kê tại PAY-ISS-002.

### 6.4 PAY-ISS-003 - Giữ Rejected và chủ động mở lại Draft

- **Quy tắc đề nghị:** Chọn phương án B. Người duyệt từ chối với lý do không rỗng thì chuyển PendingApproval → Rejected; giữ nguyên nội dung đã trình và lý do từ chối. NVKD phụ trách chọn “Chỉnh sửa lại” để chuyển Rejected → Draft; chỉ sau đó mới sửa, cập nhật nguồn, hủy hoặc trình lại. Không tự chuyển ngay về Draft và không sửa/trình trực tiếp tại Rejected. KTT lập điều chỉnh thực hiện hành động tương ứng trên hồ sơ điều chỉnh của mình.
- **Lý do/đánh đổi:** Nhìn được bảng đang bị từ chối và phân biệt thời điểm người lập tiếp nhận xử lý. Thêm một hành động rõ ràng và hai bản ghi chuyển trạng thái; lịch sử lần trình/từ chối cũ vẫn được giữ khi có lần trình mới. PAY-08 cần viết lại hành động/quyền chuyển về Draft thay vì nói tự quay về.
- **Ví dụ kiểm chứng đề nghị:** Từ chối không có lý do bị chặn; từ chối hợp lệ tạo Rejected. Yêu cầu sửa ngay khi Rejected bị chặn. Sau “Chỉnh sửa lại”, bảng ở Draft, audit ghi ai chuyển/lúc nào, lý do từ chối cũ vẫn xem được; lần trình tiếp theo phải kiểm tra lại và phân công lại theo tổng hiện hành.
- **Bên nên xác nhận và tài liệu:** Đại diện NVKD, KTT và GĐKD; cập nhật PAY-08, US-PAY-01/02/03, bảng trạng thái/quyền và AC liên quan PAY-ISS-003. Không bổ sung trạng thái thứ sáu.

### 6.5 PAY-ISS-004 - Kỳ thanh toán và chống chồng tháng

- **Quy tắc đề nghị:** Chọn phương án B: tháng dương lịch hoặc quý dương lịch theo cấu hình hợp đồng có hiệu lực cho kỳ. Bảng quý bao phủ cả ba tháng của quý. Với cùng hợp đồng, một tháng chỉ được thuộc tối đa một bảng chưa Cancelled, kể cả Draft, PendingApproval, Rejected hoặc Approved; hồ sơ điều chỉnh theo PAY-ISS-001 thuộc cùng chuỗi bảng và không chiếm tháng lần thứ hai.
- **Thay đổi cấu hình:** Chỉ áp cấu hình mới cho kỳ tương lai chưa bị bảng hiện hữu chiếm tháng; không đổi kỳ của bảng đã lập. Nếu có bảng tháng trong quý thì không lập thêm bảng quý chồng lên tháng đó. Chuyển tháng sang quý từ đầu quý tiếp theo chưa bị chiếm; chuyển quý sang tháng sau khi kết thúc kỳ quý đã lập. Kỳ lẻ đầu/cuối hợp đồng chỉ được lập khi nguồn hợp đồng đã cung cấp rõ khoảng thời gian và cách phân bổ; thiếu quy định thì chặn để làm rõ, không tự tính đủ kỳ.
- **Lý do/đánh đổi:** Bao phủ tháng/quý mà tránh tính tiền hai lần cho cùng khoảng thời gian. PAY-01 cần đổi thành quy tắc chống chồng các tháng thuộc kỳ, với ngoại lệ Cancelled theo PAY-ISS-007. Phải kiểm tra cả yêu cầu lập đồng thời và lịch sử cấu hình thay vì chỉ so nhãn tháng/quý.
- **Ví dụ kiểm chứng đề nghị:** Đã có bảng quý I thì lập bảng tháng 2 bị chặn. Đã có Draft tháng 1 thì lập bảng quý I bị chặn. Hai yêu cầu đồng thời cùng hợp đồng/kỳ chỉ được tạo một bảng chưa Cancelled. Hủy Draft quý I giải phóng cả tháng 1, 2, 3; bảng thay thế vẫn phải phù hợp cấu hình kỳ và điều kiện lập hiện tại.
- **Bên nên xác nhận và tài liệu:** Đại diện hợp đồng, NVKD và KTT; cập nhật thuật ngữ kỳ, PAY-01, US-PAY-01/05 và AC trùng/chồng kỳ tại PAY-ISS-004/007.

### 6.6 PAY-ISS-005 - Dịch vụ bắt buộc từ hợp đồng

- **Quy tắc đề nghị:** Danh sách dịch vụ bắt buộc lấy từ hợp đồng/phụ lục có hiệu lực trong kỳ, do nghiệp vụ quản lý hợp đồng chịu trách nhiệm cung cấp. Mỗi dịch vụ bắt buộc phải có dòng thanh toán bao phủ khoảng áp dụng tương ứng. Dịch vụ chỉ có hiệu lực một phần kỳ không được yêu cầu cho phần còn lại. Bảng giá cung cấp giá, không mặc nhiên quyết định mọi dịch vụ đều bắt buộc.
- **Ngoại lệ và dữ liệu thiếu:** Sản lượng 0 vẫn có dòng nếu nguồn đã đối soát xác nhận rõ giá trị 0; không tạo số 0 để thay cho thiếu dữ liệu. Nguồn xác nhận danh sách rỗng nghĩa là không có dịch vụ bắt buộc; không lấy được danh sách nghĩa là chưa xác định, phải chặn trình và chỉ rõ dữ liệu còn thiếu. Ngoại lệ dịch vụ được xác nhận tại hợp đồng/phụ lục, không cho NVKD tự bỏ yêu cầu trong bảng thanh toán.
- **Lý do/đánh đổi:** Gắn nghĩa vụ dịch vụ với thỏa thuận khách hàng và thời gian hiệu lực. Cần xác nhận module hợp đồng thực sự cung cấp được danh sách và phiên bản; đây là điều kiện phụ thuộc cần đáp ứng trước khi chốt thiết kế, chưa tạo chức năng quản lý danh mục trong module thanh toán.
- **Ví dụ kiểm chứng đề nghị:** Hợp đồng bắt buộc A và B; bảng chỉ có A thì chặn trình, nêu thiếu B. B có sản lượng 0 đã đối soát và có dòng hợp lệ thì không chặn vì thiếu B. Không nhận được danh sách thì không tự coi là không có dịch vụ bắt buộc.
- **Bên nên xác nhận và tài liệu:** Chủ quản hợp đồng và KTT xác nhận nguồn/danh sách, NVKD xác nhận cách xử lý thiếu; cập nhật PAY-04, US-PAY-01/02 và AC thiếu dịch vụ tại PAY-ISS-005.

### 6.7 PAY-ISS-006 - Sửa Draft trong phạm vi rõ ràng

- **Quy tắc đề nghị:** Chọn phương án A với danh sách trường được sửa là ghi chú chung và ghi chú từng dòng. Không sửa trực tiếp hợp đồng, kỳ, mã dịch vụ, đơn vị tính, sản lượng, đơn giá, thành tiền hoặc tổng; không thêm/xóa dòng bằng tay. Hợp đồng/kỳ sai thì hủy Draft và lập bảng khác. Dữ liệu tính tiền sai thì yêu cầu chủ quản nguồn sửa, sau đó NVKD chủ động cập nhật lại toàn bộ dữ liệu tính tiền có kiểm tra theo PAY-ISS-012.
- **Tính lại và audit:** Sửa ghi chú không làm thay đổi số tiền. Cập nhật nguồn phải xem trước thay đổi và tính lại dòng/tổng; mọi lần lưu thay đổi Draft, kể cả ghi chú, đều ghi ai/lúc nào/nội dung trước-sau. Không ghi nhận thao tác mở/xem là thay đổi dữ liệu. Hồ sơ điều chỉnh của KTT có thêm lý do/biên bản theo PAY-ISS-001, nhưng không có quyền ghi đè số liệu không có căn cứ.
- **Lý do/đánh đổi:** Tránh bảng thanh toán lệch khỏi sản lượng đã đối soát hoặc giá hợp đồng. Phải phối hợp xử lý lỗi nguồn nên có thể lâu hơn sửa tay; nếu nghiệp vụ cần ngoại lệ ghi đè tài chính thì phải xác nhận thành một quyết định bổ sung với quyền và căn cứ riêng.
- **Ví dụ kiểm chứng đề nghị:** Sửa ghi chú Draft giữ nguyên tổng và tạo audit. Thay đơn giá trực tiếp bị chặn. Đơn giá nguồn đã sửa hợp lệ, người phụ trách xác nhận cập nhật thì tính lại tổng; bảng không còn Draft không được dùng thao tác này.
- **Bên nên xác nhận và tài liệu:** NVKD, KTT và chủ quản sản lượng/bảng giá; cập nhật US-PAY-01, NFR-PAY-01 và AC sửa Draft tại PAY-ISS-006.

### 6.8 PAY-ISS-007 - Quyền hủy và lập thay thế

- **Quy tắc đề nghị:** NVKD đang phụ trách hợp đồng trong đơn vị được hủy bảng Draft của mình; người được phân công tiếp nhận chính thức được xử lý như người phụ trách, phải lưu lịch sử bàn giao. Hủy phải có lý do và audit. KTT được hủy Draft của hồ sơ điều chỉnh do mình phụ trách theo PAY-ISS-001; không mặc nhiên có quyền hủy mọi bảng gốc. Không hủy PendingApproval, Approved hoặc Rejected; muốn hủy Rejected phải đưa về Draft đúng quyền trước.
- **Lập thay thế:** Cancelled không chiếm kỳ; được tạo một bảng mới với mã mới, tham chiếu bảng bị hủy và lý do lập lại. Không phục hồi hoặc trình lại bản Cancelled. Kiểm tra lại đầy đủ Active, giá, đối soát, cấu hình kỳ và chống chồng kỳ theo PAY-ISS-004. Hủy hồ sơ điều chỉnh không giải phóng kỳ của bảng gốc Approved.
- **Lý do/đánh đổi:** Cho sửa sai lựa chọn hợp đồng/kỳ và đáp ứng trường hợp nghiệp vụ cần lập lại. Cần diễn giải PAY-01 thành một bảng chưa Cancelled cho phần kỳ, bảo toàn toàn bộ lịch sử và bảo đảm chỉ một yêu cầu lập thay thế đồng thời được thành công.
- **Ví dụ kiểm chứng đề nghị:** Hủy Draft tháng 4 hợp lệ → Cancelled; lập lại được bảng mã mới nếu đủ điều kiện. Bản cũ vẫn không sửa/trình được. Nếu người khác đã lập bảng thay thế cho tháng 4 thì lần tạo tiếp theo bị chặn. Người không được phân công không được hủy dù có cùng vai trò NVKD.
- **Bên nên xác nhận và tài liệu:** Đại diện NVKD và KTT; cập nhật actor US-PAY-05, PAY-01, bảng quyền và AC hủy/tái lập tại PAY-ISS-007.

### 6.9 PAY-ISS-008 - Giá theo thời gian cung cấp dịch vụ

- **Quy tắc đề nghị:** Dùng bảng giá được hợp đồng chỉ định, đúng dịch vụ/đơn vị tính và có hiệu lực tại ngày cung cấp dịch vụ. Nếu giá thay đổi trong kỳ, tách sản lượng đã đối soát thành các khoảng hiệu lực; một dịch vụ có thể có nhiều dòng, mỗi dòng một đơn giá. Giá không phụ thuộc ngày NVKD bấm lập/trình. “Thời điểm tính” trong PAY-03 cần được diễn giải theo mốc dịch vụ này nếu đề xuất được chấp thuận.
- **Dữ liệu không đủ:** Thiếu giá hoặc có nhiều giá cùng ưu tiên cho cùng dịch vụ/thời điểm thì chặn lập/cập nhật và yêu cầu chủ quản bảng giá xử lý; không tự chọn giá mới nhất. Nếu chỉ có sản lượng tổng kỳ, không tách được theo ngày/khoảng đổi giá, yêu cầu nguồn cung cấp phân bổ đã đối soát; không tự chia đều. Sửa ghi chú không chọn lại giá; cập nhật Draft chủ động mới tính lại theo cùng nguyên tắc. Approved chỉ thay qua hồ sơ điều chỉnh.
- **Lý do/đánh đổi:** Phản ánh thời gian thực hiện nghĩa vụ và tránh cùng dịch vụ có giá khác chỉ vì lập muộn. Đổi lại, nguồn sản lượng cần có ngày hoặc phân bổ theo khoảng; đây là phụ thuộc nghiệp vụ phải xác nhận, không giả định dữ liệu hiện đã có.
- **Ví dụ kiểm chứng đề nghị:** Ngày 1-15 có 10 đơn vị giá 100.000; ngày 16-cuối kỳ có 20 đơn vị giá 120.000 → hai dòng, tổng 3.400.000 đồng. Chỉ biết tổng 30 đơn vị thì không áp toàn bộ giá 120.000 hoặc chia đều sản lượng.
- **Bên nên xác nhận và tài liệu:** Chủ quản hợp đồng/bảng giá/sản lượng và KTT; cập nhật PAY-01/PAY-03, US-PAY-01/04 và AC chọn giá tại PAY-ISS-008.

### 6.10 PAY-ISS-009 - Quy ước tính tiền và làm tròn

- **Quy tắc đề nghị:** Phạm vi hiện tại dùng VND. Sản lượng và đơn giá nhận tối đa 6 chữ số sau dấu thập phân; giữ nguyên độ chính xác đầu vào, không tự làm tròn dữ liệu nguồn vượt giới hạn mà báo cần thống nhất với chủ quản nguồn. Tính tích chính xác từ các giá trị đó, làm tròn từng thành tiền dòng đến 1 đồng theo quy tắc HALF_UP: phần lẻ có độ lớn từ 0,5 trở lên làm tròn ra xa 0, nhỏ hơn thì về số nguyên gần nhất. Tổng chính thức bằng tổng các thành tiền dòng đã làm tròn, không làm tròn tổng lần nữa.
- **Kiểm tra và phân cấp:** Tổng chính thức là tổng hiển thị, tổng trình duyệt, tổng bàn giao và tổng xác định người duyệt. Không bổ sung thuế/phí/chiết khấu vào công thức. Tổng chính thức bằng 0 vẫn được trình nếu đủ điều kiện khác. Để không che số âm nhỏ bằng làm tròn, BA đề nghị kiểm tra thêm tổng các tích trước làm tròn: nếu âm thì chặn trình dù tổng chính thức là 0. Đây là điều kiện bổ sung cần được chấp thuận, không nhận là PAY-04 đã nêu rõ.
- **Lý do/đánh đổi:** Các dòng hiển thị cộng đúng tổng và có một số tiền duy nhất cho xét duyệt. Mốc 6 số lẻ là giới hạn đề xuất cần đối chiếu dữ liệu thực tế; VND và cách làm tròn cần kế toán xác nhận trước áp dụng. Tổng theo cách này có thể khác làm tròn một lần sau cộng, nên phải dùng nhất quán cả phía nhận.
- **Ví dụ kiểm chứng đề nghị:** Hai tích dòng 100,5 và 100,5 đồng → 101 + 101 = 202 đồng. Một tích -0,4 đồng → dòng 0 đồng nhưng bị chặn trình vì tổng trước làm tròn âm. Hai tích 249.999.999,5 đồng → tổng chính thức 500.000.000 đồng → GĐKD theo PAY-ISS-002. Tổng 0 thật, đủ dịch vụ bắt buộc và nguồn hợp lệ thì không bị chặn chỉ vì bằng 0.
- **Bên nên xác nhận và tài liệu:** KTT, kế toán tiếp nhận và chủ quản nguồn số liệu; cập nhật định nghĩa tổng, PAY-03/PAY-04/PAY-06 và AC tính tiền/biên tại PAY-ISS-009.

### 6.11 PAY-ISS-010 - Đo đủ hành trình lập Draft

- **Quy tắc đề nghị:** Đo từ lúc NVKD chọn “Lập bảng” sau khi đã nhập hợp đồng/kỳ hợp lệ đến khi giao diện hiển thị bảng Draft đã được lưu thành công. Tính cả thời gian mạng trong môi trường đo, đọc hợp đồng/giá/sản lượng, kiểm tra, tính tiền, lưu Draft và hiển thị kết quả. Không tính thời gian người dùng chọn/nhập đầu vào và không cộng việc trình/gửi thông báo vào thao tác lập.
- **Cách đếm và tải đề nghị:** “Dòng hợp đồng” là số dòng dịch vụ hợp đồng áp dụng trong kỳ trước khi tách theo giá; ghi riêng cả số dòng thanh toán sinh ra, không thay điều kiện dưới 200 bằng số dòng đầu ra. Hồ sơ đo ban đầu dùng bộ dữ liệu 1 và 199 dòng, có trường hợp 199 dòng sinh trên 199 dòng thanh toán do đổi giá; chạy ở tải 1 và 10 người lập đồng thời, mỗi người 30 lượt trên các hợp đồng/kỳ chưa có bảng. Mọi lượt thành công phải không quá 3 giây; lỗi/timeout không được tính là đạt. Đây là hồ sơ kiểm chứng BA đề xuất, chưa phải tải/NFR đã được phê duyệt.
- **Lý do/đánh đổi:** Phản ánh thời gian NVKD phải chờ để có kết quả sử dụng được. Môi trường cần ghi cấu hình, dữ liệu, độ trễ/khả dụng nguồn, số dòng đầu vào/đầu ra và từng thời gian đo; không loại thời gian gọi nguồn để làm kết quả đẹp hơn. Đúng 200 và trên 200 dòng vẫn khảo sát riêng, chưa gán tiêu chí 3 giây hoặc ngưỡng mới cho các trường hợp đó.
- **Ví dụ kiểm chứng đề nghị:** Hợp đồng 199 dòng, hiển thị Draft đã lưu sau 2,8 giây → đạt trong điều kiện đo đã thống nhất; 3,1 giây → không đạt. Trả màn hình chờ sau 1 giây rồi lưu ở giây thứ 4 không được tính đạt. Chưa có ứng dụng nên lượt BA này không thực hiện phép đo.
- **Bên nên xác nhận và tài liệu:** Đại diện NVKD/chủ sản phẩm cùng bên phát triển, kiểm thử và chủ quản nguồn; cập nhật cách diễn giải NFR-PAY-02 và AC-NFR-PAY-02-01, giữ nguyên ngưỡng gốc dưới 200 dòng.

### 6.12 PAY-ISS-011 - Phân công theo đơn vị, giữ độc lập người duyệt

- **Quy tắc đề nghị:** Chọn phương án A. Mỗi hợp đồng thuộc một đơn vị và có một NVKD phụ trách; chỉ người phụ trách hoặc người được bàn giao chính thức được sửa/trình/hủy Draft. Quyền xem cần được quy định riêng theo đơn vị, không mặc nhiên có quyền sửa. Giữ nguyên danh tính người lập để kiểm tra PAY-07 dù đã bàn giao xử lý. Hồ sơ điều chỉnh có KTT phụ trách theo PAY-ISS-001.
- **Người duyệt:** Mỗi đơn vị chỉ định một người duyệt chính và người thay thế cho từng vai trò KTT/GĐKD. Lúc trình, chọn đúng vai trò theo tổng, loại người lập bảng và người lập hồ sơ điều chỉnh nếu có; giao cho người chính đủ điều kiện, nếu không thì người thay thế đủ cùng thẩm quyền. Không có người hợp lệ thì giữ Draft, chặn trình và báo cho đầu mối phân công nghiệp vụ của đơn vị; không tự nâng/hạ cấp hoặc cho người lập tự duyệt. Nhân sự cụ thể và căn cứ ủy quyền phải được nghiệp vụ cung cấp.
- **Từ chối và thay người:** Chỉ người đang được giao mới được duyệt hoặc từ chối; đề nghị áp dụng tính độc lập cho cả hai hành động. Nếu người được giao vắng mặt sau trình, đầu mối có thẩm quyền phân công lại cho người đủ cùng vai trò, ghi audit, thông báo người mới và kết thúc quyền xử lý của người cũ. Chỉ một kết quả xử lý được ghi nhận khi có thao tác đồng thời; không tạo thêm vòng duyệt hay yêu cầu trình lại chỉ vì đổi người hợp lệ.
- **Lý do/đánh đổi và ví dụ:** Trách nhiệm rõ hơn gửi cho tất cả người cùng vai trò, nhưng cần vận hành danh sách phân công. NVKD đồng thời là KTT, lập bảng 100 triệu → chỉ KTT khác hợp lệ được giao; không có thì chặn trình, không mặc nhiên chuyển GĐKD. Người lập cũng không được tự từ chối; người cũ không xử lý được sau phân công lại.
- **Bên nên xác nhận và tài liệu:** Đại diện quản lý đơn vị, KTT và GĐKD; cập nhật US-PAY-01/02/03, PAY-07/PAY-08, phân công/thông báo và AC quyền tại PAY-ISS-011. Lệnh cấm tự từ chối và quy trình bàn giao là phần đề xuất bổ sung.

### 6.13 PAY-ISS-012 - Kiểm tra lại có kiểm soát, không đổi tiền ngầm

- **Quy tắc đề nghị:** Giữ căn cứ dữ liệu và kết quả tính của lần lập/cập nhật được NVKD xác nhận; không thay số liệu chỉ vì mở xem hoặc sửa ghi chú. Lúc trình, kiểm tra lại Active, trạng thái đối soát và các căn cứ áp dụng cho đúng hợp đồng/kỳ: cấu hình kỳ, dịch vụ bắt buộc, sản lượng, bảng giá và hiệu lực liên quan. Đây là mở rộng mốc kiểm tra so với điều kiện lúc lập trong nguồn, cần được xác nhận.
- **Khi khác biệt/không đọc được nguồn:** Nếu thay đổi ảnh hưởng tính hợp lệ hoặc số tiền, chặn trình, giữ Draft và thông báo nội dung khác biệt; không tự cập nhật tổng rồi trình ngay. NVKD xử lý với chủ quản nguồn, chọn cập nhật, xem/chấp nhận thay đổi rồi mới trình lại. Cập nhật toàn bộ tập căn cứ liên quan trong một lần, tính lại dòng/tổng theo PAY-ISS-008/009, ghi audit. Không đọc được hoặc không xác định được căn cứ nhất quán thì chặn trình; thay đổi thông tin mô tả nguồn không ảnh hưởng tính tiền/điều kiện thì không bắt tính lại.
- **Sau trình:** Nội dung và căn cứ lần trình được cố định. Trước duyệt, nếu kiểm tra thấy nguồn liên quan không còn hợp lệ hoặc đã đổi nội dung tính tiền thì chặn duyệt và thông báo người được giao; người này từ chối kèm lý do để người phụ trách đưa về Draft xử lý. Không đọc được nguồn thì giữ PendingApproval chờ kiểm tra lại, không tự từ chối. Approved không bị hồi tố thay đổi; sai sót sau đó dùng hồ sơ điều chỉnh. Với hợp đồng đã kết thúc cần điều chỉnh kỳ cũ, chỉ cho lập hồ sơ điều chỉnh khi kỳ dịch vụ thuộc thời gian hợp đồng có hiệu lực, sản lượng/giá sửa có xác nhận và biên bản đầy đủ; đây là ngoại lệ Active được BA đề nghị riêng, chưa áp dụng cho lập bảng thông thường.
- **Lý do/đánh đổi và ví dụ:** Tránh duyệt số liệu cũ và tránh đổi số tiền không có người chịu trách nhiệm; phụ thuộc khả năng đọc lại nguồn. Draft 100 triệu, sản lượng liên quan đã đổi → lần trình bị chặn, tổng vẫn 100 triệu đến khi NVKD chấp nhận cập nhật thành 110 triệu; chỉ lần trình sau mới gửi tổng mới. Giá thay đổi cho kỳ tương lai không ảnh hưởng kỳ đang xét thì không bắt tính lại. Cách lưu căn cứ/so sánh phiên bản là đề xuất Design sau này.
- **Bên nên xác nhận và tài liệu:** NVKD, KTT và ba chủ quản nguồn; cập nhật các mốc kiểm tra US-PAY-01/02/03/04, PAY-01/PAY-02/PAY-03 và AC liên quan. Bổ sung truy vết US-PAY-03/AC-US-PAY-03-01 đến AC-US-PAY-03-03 cho mốc kiểm tra trước duyệt nếu được chấp thuận.

### 6.14 PAY-ISS-013 - Bàn giao thủ công có đối chiếu ở giai đoạn đầu

- **Quy tắc đề nghị:** Chọn bàn giao ngoài module trong phạm vi hiện tại. Sau Approved, KTT phụ trách đơn vị chịu trách nhiệm kiểm tra và bàn giao cho đầu mối kế toán đã được chỉ định, lưu bằng chứng gửi/tiếp nhận trong quy trình vận hành. Không cần thêm trạng thái bảng hoặc API tích hợp khi chưa có hợp đồng giao tiếp; kế toán chỉ xuất hóa đơn khi đối chiếu được đúng căn cứ đã duyệt. Phương tiện chia sẻ hồ sơ sử dụng quy trình sẵn có của đơn vị, chưa giả định module đã có chức năng xuất file.
- **Bộ thông tin tối thiểu:** Mã bảng và mã lần duyệt/điều chỉnh, hợp đồng/khách hàng, kỳ, các dòng sản lượng/giá/thành tiền, tổng chính thức, người/thời điểm duyệt, tài liệu liên quan; với điều chỉnh thêm bản tham chiếu, tổng cũ/mới, chênh lệch và biên bản. Dùng mã bảng + mã lần duyệt/điều chỉnh để đối chiếu tiếp nhận, gửi lại không tạo nghĩa vụ tính tiền lần thứ hai. Hồ sơ điều chỉnh là bản thay thế căn cứ tính tiền của cùng chuỗi; không yêu cầu kế toán cộng lại toàn bộ tổng mới vào tổng cũ.
- **Ngoại lệ và trách nhiệm:** Chưa được xác nhận tiếp nhận thì KTT theo dõi và gửi lại cùng tham chiếu; không coi Approved là bằng chứng đã bàn giao thành công. Nếu đang có điều chỉnh chưa duyệt thì tạm dừng bàn giao mới của chuỗi đó. Nếu bản gốc đã bàn giao, báo kế toán về hồ sơ điều chỉnh đang xử lý để phối hợp; chỉ bàn giao kết quả mới sau duyệt. Hóa đơn đã xuất thì kế toán quyết định quy trình xử lý ngoài module, không sửa bảng gốc hoặc tự thực hiện nghiệp vụ hóa đơn tại đây.
- **Lý do/đánh đổi và ví dụ:** Giữ được trách nhiệm vận hành và giới hạn bài tập, nhưng cần người theo dõi thủ công. Gửi lại bản Approved đã được kế toán tiếp nhận phải được đối chiếu là cùng một hồ sơ. Điều chỉnh 480 → 510 triệu phải kèm chênh lệch +30 triệu; không coi 510 triệu là bảng độc lập để xuất thêm toàn bộ. Chưa mặc định hồ sơ hoặc phương tiện thủ công hiện đã tồn tại; chủ quy trình phải xác nhận.
- **Bên nên xác nhận và tài liệu:** KTT và đại diện kế toán tiếp nhận; cập nhật ranh giới module, đầu ra US-PAY-03/04 và AC-US-PAY-03-08. Tự động hóa bàn giao là phạm vi phát triển tiếp theo nếu có yêu cầu riêng.

### 6.15 PAY-ISS-014 - Tách kết quả trình khỏi kết quả gửi thông báo

- **Quy tắc đề nghị:** Chọn phương án A. Khi các kiểm tra nghiệp vụ đạt, người duyệt hợp lệ đã được giao và lần trình đã được ghi nhận thành công, giữ PendingApproval dù gửi thông báo gặp lỗi. Phản hồi cho NVKD phải nêu rõ “Đã trình duyệt; thông báo đang chờ gửi lại” nếu biết có lỗi, không báo toàn bộ bước gửi đã thành công và không đưa về Draft chỉ vì lỗi thông báo.
- **Gửi lại và theo dõi:** Người duyệt xem được yêu cầu trong danh sách chờ dù thông báo ngoài ứng dụng chưa đến. Đề nghị gửi lại tự động tối đa 3 lần trong 15 phút sau lỗi đầu tiên; hết giới hạn, đầu mối vận hành được chỉ định nhận việc xử lý và có thể yêu cầu gửi lại thủ công cho cùng lần trình. Mốc 3 lần/15 phút và khả năng xem danh sách chờ là đề xuất mới cần xác nhận. Lưu lịch sử gửi/lỗi và bảo đảm luôn có đầu mối theo dõi; không xem lỗi gửi là lỗi dữ liệu bảng.
- **Chống trùng và kết thúc:** Bấm Trình lại tại PendingApproval trả kết quả của lần trình hiện hữu, không tạo thêm vòng duyệt hoặc yêu cầu thông báo mới. Một lần thử gửi lại phải cùng tham chiếu lần trình/người nhận đang được giao; không gửi tiếp thông báo cũ khi bảng đã Approved/Rejected/Cancelled hoặc đã đổi người nhận. Nếu kênh gửi có thể giao lặp, cùng tham chiếu giúp nhận biết đó vẫn là một yêu cầu; không cam kết người nhận chỉ thấy đúng một thông báo bằng mọi kênh.
- **Lý do/đánh đổi và ví dụ:** Lỗi kênh thông báo không làm mất yêu cầu nghiệp vụ đã hợp lệ, nhưng cần theo dõi giao nhận rõ ràng. Gửi lỗi sau khi chuyển PendingApproval → bảng vẫn chờ duyệt, NVKD thấy trạng thái gửi chờ thử lại; bấm Trình thêm lần nữa không sinh yêu cầu thứ hai. Nếu người duyệt đã xử lý trước lần thử gửi kế tiếp thì không gửi lời nhắc duyệt lỗi thời. Cơ chế kỹ thuật cụ thể do Design đề xuất, chưa chốt outbox/hàng đợi/giao dịch tại bước BA.
- **Bên nên xác nhận và tài liệu:** NVKD, KTT/GĐKD và đầu mối vận hành do đơn vị chỉ định; cập nhật ngoại lệ US-PAY-02, PAY-04, AC-US-PAY-02-01/05 và nghĩa vụ theo dõi gửi. Không có người duyệt hợp lệ vẫn chặn từ Draft theo PAY-ISS-011, không dùng chính sách gửi lại để bỏ qua điều kiện này.

### 6.16 Cách chốt và cập nhật sau khi có xác nhận

- **Ưu tiên 1 - vòng đời và thẩm quyền:** PAY-ISS-001/002/003/004, đọc cùng PAY-ISS-007/011 để tránh chốt quy trình không có người xử lý hoặc chiếm kỳ sai.
- **Ưu tiên 2 - căn cứ tính tiền:** PAY-ISS-005/006/008/009/012; đặc biệt xác nhận khả năng cấp danh sách bắt buộc, sản lượng theo thời gian, độ chính xác đầu vào và kiểm tra lại nguồn.
- **Ưu tiên 3 - vận hành và nghiệm thu:** PAY-ISS-013/014/010; chốt người chịu trách nhiệm, điều kiện gửi lại và hồ sơ đo thực tế. Thứ tự này không làm các vấn đề ưu tiên sau trở thành tùy chọn.
- Người dùng/nhóm có thể xác nhận từng vấn đề hoặc một phần quy tắc; ghi đúng ngoại lệ và phần chưa chấp thuận. Chỉ phần có bằng chứng được chuyển thành PAY-DEC tại [decisions.md](decisions.md); phần còn lại vẫn Cần xác nhận.
- Khi có xác nhận, cập nhật các US/AC, diễn giải PAY/NFR, trạng thái/quyền và ma trận truy vết trong [requirements-analysis.md](requirements-analysis.md); Design cập nhật tài liệu liên quan theo phạm vi ảnh hưởng tại từng vấn đề. Mục 6 này giữ lại làm lịch sử đề xuất, không thay bằng một bản ghi phê duyệt không có căn cứ.

Kết quả PRM-005: **14 khuyến nghị BA, 14 vấn đề Cần xác nhận, 0 quyết định nghiệp vụ đã phê duyệt**.

## 7. Bổ sung sau review phân tích R002 - PRM-011

### 7.1 Căn cứ và phạm vi

- Người dùng đã chấp thuận đánh giá và phương án tại findings R002 mục 4 ngày 12/11/2026: A001-A014 Đúng, F001 Đúng có giới hạn, F002 Đúng, F003 Thừa. Prompt xác nhận được lưu tại đó; người dùng giao BA áp dụng bằng “ok proceed on your tasks” trong PRM-011. Không tự gán xác nhận cho KTT/GĐKD hoặc thành viên khác.
- Giữ 14 vấn đề và phương án PRM-006. F001 bổ sung chi tiết cho PAY-ISS-001/003/007/013; F002 bổ sung tiêu chí kiểm chứng PAY-ISS-012. Không cấp lại mã PAY-ISS, không coi 14 nhận xét A là 14 lỗi mới. Mục 6 giữ nguyên lịch sử PRM-005.

### 7.2 F001 - Vòng đời điều chỉnh và bàn giao

- **Khoảng trống được chấp nhận:** cụm “đang xử lý/chưa duyệt” chưa nêu tập trạng thái; không kết luận hệ thống chắc chắn kẹt hoặc tạo trùng. Bảng trạng thái đã được người dùng xác nhận, không còn câu hỏi chặn phần này.
- **Quyết định bổ sung:** PAY-DEC-015 trong [decisions.md](decisions.md) mục 4.15. Draft/PendingApproval/Rejected giữ vị trí điều chỉnh duy nhất và tạm dừng bàn giao mới. Rejected chỉ mở lại chính hồ sơ bởi KTT phụ trách; khi bỏ thì mở Draft rồi hủy có lý do/audit trước tạo khác.
- **Kết thúc giữ vị trí:** Approved/Cancelled kết thúc giữ vị trí của chính hồ sơ đó. Chỉ tạo điều chỉnh khác khi toàn chuỗi không còn hồ sơ đang xử lý và đủ điều kiện. Approved dùng căn cứ mới đã duyệt để bàn giao; Cancelled dùng bản Approved hiện hành. Cả hai chỉ tiếp tục bàn giao khi không có điều chỉnh khác đang xử lý. Cancelled không phục hồi, không đổi giá trị đã duyệt hoặc giải phóng kỳ gốc.
- **Ảnh hưởng hiện hành:** PAY-ISS-001 → điều kiện tạo tiếp và hiệu lực; PAY-ISS-003 → Rejected giữ vị trí khi mở lại; PAY-ISS-007 → hủy kết thúc giữ vị trí, giữ kỳ gốc; PAY-ISS-013 → dừng/tiếp tục bàn giao. US-PAY-03/04/05, bảng trạng thái mục 6.1 và truy vết mục 8.3 của requirements-analysis.md đã đồng bộ.
- **AC bổ sung:** AC-US-PAY-04-04 (Draft/PendingApproval/Rejected), AC-US-PAY-04-05 (Rejected → Draft → Cancelled), AC-US-PAY-04-06 (Approved và kiểm tra toàn chuỗi). Giữ quyền, căn cứ nguồn, biên bản, duyệt độc lập và audit theo các quyết định trước.

### 7.3 F002 - Bao phủ ngoại lệ nguồn

Giữ PAY-DEC-012, không chốt lại nghiệp vụ hoặc nhận là PDF quy định chi tiết. Bốn AC mới tại requirements-analysis.md và mapping mục 8.3:

| Quyết định | US | AC | Nhánh và kết quả mong đợi |
| --- | --- | --- | --- |
| PAY-DEC-012 | US-PAY-02 | AC-US-PAY-02-06 | Nguồn đổi trước trình 100 → 110 triệu: chặn, giữ Draft/100 triệu đến khi người phụ trách xem/chấp nhận cập nhật; không tự đổi rồi trình |
| PAY-DEC-012 | US-PAY-02 | AC-US-PAY-02-07 | Không đọc được nguồn trước trình: chặn, giữ Draft/số tiền đã lưu |
| PAY-DEC-012 | US-PAY-03 | AC-US-PAY-03-09 | Nguồn đổi ảnh hưởng/không hợp lệ trước duyệt: chặn, giữ PendingApproval/nội dung lần trình, thông báo; người được giao từ chối có lý do để sửa, không tự từ chối |
| PAY-DEC-012 | US-PAY-03 | AC-US-PAY-03-10 | Không đọc được nguồn trước duyệt: giữ PendingApproval/nội dung lần trình chờ kiểm tra, không tự Approved/Rejected |

Truy vết PAY-ISS-012 hiện hành bao gồm cả US-PAY-03 và bốn AC này, bổ sung các tham chiếu nhánh hợp lệ ban đầu ở mục 4/6.13. Thiếu bao phủ AC đã được xử lý trong tài liệu; chưa kiểm thử ứng dụng.

### 7.4 Những nhận xét còn lại và bàn giao

- A001/A007/A013 theo F001; A012 theo F002. A003 cũng liên quan bước mở lại của F001. A004 chỉ làm rõ nhãn “Chưa rõ cách kết hợp quy tắc” ở mục 1/2, giữ PAY-DEC-004. Các nhận xét A còn lại giữ quyết định PRM-006, không thêm việc nghiệp vụ.
- F003 được đánh giá Thừa; khi biên tập chỉ đổi nhãn cột nguồn trong requirements-analysis.md thành “Tóm lược phát biểu nguồn” và lời dẫn tương ứng. Giữ nguyên nội dung tám dòng tóm lược, diễn giải nghiệp vụ và PDF; không mở quyết định mới cho việc đổi nhãn.
- Bàn giao Design theo phân tích 37 AC và PAY-DEC-001 đến PAY-DEC-015, xem requirements-analysis.md mục 9.4. Phụ thuộc dữ liệu/nhân sự/môi trường vẫn cần chuẩn bị; bộ thiết kế đã có chưa được cập nhật trong lượt BA này. Bằng chứng kiểm tra và phiên bản thực tế tại [changes R002](../../reviews/payment-management/R002/changes.md).
