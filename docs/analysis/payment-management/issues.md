# Sổ vấn đề đặc tả - payment-management

Nguồn: `btcohong.pdf`, trang đánh số từ 1. Đây là danh sách phát hiện ban đầu do AI soạn khi tạo bộ hướng dẫn; chưa có xác nhận nghiệp vụ của nhóm. BA phải kiểm tra lại nguồn và bổ sung, không coi đây là kết quả phân tích đầy đủ.

Trạng thái: `Cần xác nhận` → `Đã xác nhận` khi có căn cứ thật trong `decisions.md`. Việc xác nhận một vấn đề tồn tại không đồng nghĩa đã chọn phương án xử lý. Không xóa vấn đề đã giải quyết; giữ lịch sử liên kết quyết định.

## 1. Mâu thuẫn và điểm không nhất quán

| Mã | Vị trí nguồn | Loại | Phát biểu cần đối chiếu | Câu hỏi và lý do | Ảnh hưởng | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- |
| PAY-ISS-001 | Trang 2, mục 4.4 US-PAY-04; PAY-05 | Mâu thuẫn | KTT được điều chỉnh Approved có biên bản; PAY-05 cấm sửa Approved dưới mọi hình thức | Cho sửa bản gốc, tạo hồ sơ điều chỉnh riêng hay không cho điều chỉnh? Cần xác nhận vì ba cách dẫn đến quyền, dữ liệu và vòng đời khác nhau | US-PAY-04, API điều chỉnh, audit, schema, state diagram | Cần xác nhận |
| PAY-ISS-002 | Trang 2, mục 4.3 US-PAY-03; PAY-06 | Mâu thuẫn tại biên | US dùng từ 500 triệu trở lên; rule dùng trên 500 triệu | Đúng 500.000.000 đồng ai duyệt? Không được tự chọn >= hoặc > vì có thể gán sai người duyệt | Routing, guards, authorization, boundary tests | Cần xác nhận |
| PAY-ISS-003 | Trang 1, mục 3; trang 2 PAY-08 | Không nhất quán về trạng thái | Mục 3 mô tả khi từ chối chuyển Rejected; PAY-08 nói quay về Draft để sửa và trình lại | Rejected là trạng thái lưu, trạng thái trung gian hay chỉ hành động audit? Ai/khi nào chuyển về Draft? Cần biết để tránh API và sơ đồ khác nhau | Reject/resubmit, state machine, audit, quyền chỉnh sửa | Cần xác nhận |
| PAY-ISS-004 | Trang 1, mục 2; trang 2 PAY-01 | Không nhất quán về kỳ và tính duy nhất | Kỳ là tháng hoặc quý theo hợp đồng; PAY-01 chỉ một bảng mỗi hợp đồng mỗi tháng | Với hợp đồng theo quý, quy tắc duy nhất áp theo quý hay từng tháng? Có cấm chồng kỳ giữa tháng/quý không? Không được tự thay quy tắc một bảng mỗi tháng bằng một bảng mỗi kỳ | Period model, unique constraint, tạo đồng thời, kiểm tra trùng | Cần xác nhận |

## 2. Thông tin còn thiếu

| Mã | Vị trí liên quan | Loại | Câu hỏi và lý do | Ảnh hưởng | Trạng thái |
| --- | --- | --- | --- | --- | --- |
| PAY-ISS-005 | Trang 2, PAY-04 | Thiếu thông tin | Danh sách dịch vụ bắt buộc lấy từ đâu và có phụ thuộc hợp đồng/kỳ không? Không thể thiết kế validation cụ thể nếu không có căn cứ | Read-only integration, submit validation, AC | Cần xác nhận |
| PAY-ISS-006 | Trang 2, US-PAY-01 | Mơ hồ | NVKD được chỉnh trường nào ở Draft: sản lượng, đơn giá, dòng dịch vụ hay chỉ ghi chú? Các giá trị tự sinh có được ghi đè không? | Update API, công thức tính lại, audit | Cần xác nhận |
| PAY-ISS-007 | Trang 2, US-PAY-05; PAY-01 | Thiếu thông tin | Ai có quyền hủy? Bảng Cancelled có còn tính vào giới hạn một bảng mỗi tháng không, có được lập bảng thay thế không? “Không dùng lại” chưa xác định quyền tái lập | Authorization, lịch sử, unique constraint | Cần xác nhận |
| PAY-ISS-008 | Trang 2, PAY-01, PAY-03 | Mơ hồ | “Đơn giá tại thời điểm tính” lấy theo ngày tính, ngày thực hiện dịch vụ hay kỳ? Khi bảng giá đổi trong kỳ dùng một hay nhiều giá? Thiết kế không thể tự chọn ngày định giá | Pricing lookup, line generation, lịch sử tính tiền | Cần xác nhận |
| PAY-ISS-009 | Trang 2, PAY-03, PAY-04 | Thiếu thông tin | Độ chính xác sản lượng/đơn giá, quy tắc làm tròn dòng/tổng và định nghĩa tổng tiền là gì? Không tự thêm thuế, chiết khấu hoặc phí ngoài nguồn | Numeric model, tổng tiền, biên 500 triệu | Cần xác nhận |
| PAY-ISS-010 | Trang 3, mục 6 | Mơ hồ về phép đo | 3 giây đo từ đâu đến đâu, với tải và độ trễ nguồn đọc nào? Dùng tiêu chí thống kê nào để đánh giá? Đề không quy định trường hợp đúng 200 dòng | Kế hoạch kiểm tra hiệu năng, kiến trúc tích hợp | Cần xác nhận |
| PAY-ISS-011 | Trang 2, US-PAY-02, US-PAY-03; PAY-07 | Thiếu thông tin | Nếu có nhiều KTT/GĐKD thì ai nhận thông báo và được duyệt? Nếu người lập cũng giữ vai trò duyệt, phân công thay thế ra sao? PAY-07 vẫn phải được giữ | Recipient selection, authorization, notification | Cần xác nhận |

## 3. Cách cập nhật

- Mã mới tăng dần, không đổi mã đã được thiết kế/review tham chiếu.
- Thêm phương án và phân tích ảnh hưởng khi cần nhưng ghi rõ là đề xuất; không tự điền lựa chọn của nhóm.
- Mỗi xác nhận liên kết đến `decisions.md` và chỉ rõ US/AC, API, dữ liệu, sơ đồ, kiểm chứng cần cập nhật.
- Nhóm có thể gộp câu hỏi liên quan để trả lời; giữ truy vết từng mã. Không cần dừng phần thiết kế độc lập với các câu trả lời.
