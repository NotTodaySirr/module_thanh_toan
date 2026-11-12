# Phân tích yêu cầu nghiệp vụ - payment-management

- Module: `payment-management`.
- Trạng thái: **BA cập nhật sau review R002 - PRM-011**; giữ 14 lựa chọn PRM-006, bổ sung PAY-DEC-015 theo xác nhận F001 và AC cho F002.
- Ngày lập: 07/11/2026, múi giờ Asia/Bangkok; không ghi giờ khi chưa có căn cứ.
- Người soạn: AI hỗ trợ vai trò BA; người dùng trong phiên đã chấp thuận phương án PRM-005, chưa cung cấp tên cá nhân.
- Phạm vi tài liệu: phân tích và bàn giao đầu vào cho Design; không phải thiết kế hay bằng chứng ứng dụng đã đáp ứng yêu cầu.
- Nguồn: [btcohong.pdf](../../requirements/btcohong.pdf), đủ 3 trang, đánh số từ 1. File thực tế nằm trong `docs/requirements/`, khác đường dẫn ở một số hướng dẫn cũ; không di chuyển hoặc sửa nguồn.
- Hồ sơ liên quan: [sổ vấn đề](issues.md), [sổ xác nhận](decisions.md), [nhật ký prompt](../../process/prompt-log.md).
- Cập nhật PRM-006 ngày 07/11/2026: người dùng chấp thuận toàn bộ 14 phương án PRM-005, ghi thành PAY-DEC-001 đến PAY-DEC-014 trong [sổ xác nhận](decisions.md). Nội dung dưới đây đã đồng bộ; PDF và mã nguồn gốc giữ nguyên. Quyết định chưa phải bằng chứng triển khai hoặc nghiệm thu.
- Cập nhật sau R002 ngày 12/11/2026, Asia/Bangkok: áp dụng [findings](../../reviews/payment-management/R002/findings.md) mục 4.2/4.3 đã được người dùng xác nhận. PAY-DEC-015 bổ sung vòng đời điều chỉnh; thêm 7 AC, giữ nguyên 30 mã AC cũ. Bằng chứng phiên bản/kiểm tra và trạng thái vòng tại [changes R002](../../reviews/payment-management/R002/changes.md).

## 1. Chỉ mục nguồn và cách đọc căn cứ

Mỗi mã S bên dưới luôn chỉ về `docs/requirements/btcohong.pdf`. Cách ghi `S09/PAY-01` nghĩa là PDF trang 2, mục 5, dòng PAY-01; không phải nguồn nghiệp vụ bổ sung. Các mã S, AC và NFR là mã BA đặt để truy vết; mã US và PAY được giữ nguyên từ PDF.

| Mã nguồn | Trang | Mục | Nội dung dùng làm căn cứ |
| --- | --- | --- | --- |
| S01 | 1 | 1 | Mục đích, phạm vi, ranh giới module và điều kiện duyệt trước khi chuyển kế toán |
| S02 | 1 | 2 | Vai trò và thuật ngữ, kỳ tháng/quý theo hợp đồng |
| S03 | 1 | 3 | Năm trạng thái và luồng trạng thái được mô tả |
| S04 | 2 | 4.1, US-PAY-01 | Lập, tự sinh dòng, tổng tiền, lưu và sửa Draft |
| S05 | 2 | 4.2, US-PAY-02 | Kiểm tra PAY-04, trình duyệt và gửi thông báo |
| S06 | 2 | 4.3, US-PAY-03 | Phân cấp duyệt, duyệt hoặc từ chối |
| S07 | 2 | 4.4, US-PAY-04 | KTT điều chỉnh Approved có biên bản và ghi log |
| S08 | 2 | 4.5, US-PAY-05 | Hủy Draft, chuyển Cancelled, không dùng lại |
| S09 | 2 | 5 | PAY-01 đến PAY-08 |
| S10 | 3 | 6 | Audit log và phản hồi lập bảng trong 3 giây với hợp đồng dưới 200 dòng |
| S11 | 3 | Yêu cầu bài tập; Đánh giá | Sản phẩm và tiêu chí bài tập; không phải chức năng thanh toán bổ sung |

| Nhãn | Ý nghĩa và cách sử dụng |
| --- | --- |
| Có trong đặc tả | Phát biểu được nguồn nêu rõ; không đồng nghĩa đã được nhóm phê duyệt hoặc không có mâu thuẫn |
| Suy ra trực tiếp | Hệ quả trực tiếp của phát biểu nguồn; phải nêu lập luận, không dùng để thêm nghiệp vụ |
| Đề xuất kỹ thuật | Giải pháp tham khảo thuộc bước thiết kế, không phải yêu cầu gốc; hồ sơ BA này không chốt giải pháp |
| Cần xác nhận | Chưa đủ căn cứ hoặc có mâu thuẫn; phải gắn mã PAY-ISS tương ứng |
| Đã xác nhận | Nội dung có xác nhận thực tế trong decisions.md; hiện có PAY-DEC-001 đến PAY-DEC-015, phân biệt xác nhận đầu PRM-006 và bổ sung R002 |

Trong bảng AC, cột **Căn cứ** phân loại bằng chứng, còn cột **Mức độ rõ/điểm mở** cho biết có thể chốt nội dung đến đâu. `Rõ` chỉ áp dụng cho mệnh đề đang xét. Các quy tắc đã xác nhận còn phụ thuộc dữ liệu nguồn, phân công nhân sự và thiết kế; không khẳng định ứng dụng đã triển khai.

## 2. Mục tiêu, phạm vi và ranh giới

### 2.1 Mục tiêu và phần thuộc module

**Có trong đặc tả - S01, S04-S08:** Định kỳ lập bảng từ sản lượng và đơn giá; lập, trình, duyệt/từ chối, điều chỉnh và hủy. Phải được duyệt trước bàn giao kế toán xuất hóa đơn. **Đã xác nhận - PAY-DEC-001/013:** điều chỉnh bằng hồ sơ liên kết duyệt riêng, bàn giao thủ công ngoài module ở giai đoạn đầu.

### 2.2 Phần ngoài module

| Đối tượng ngoài | Dữ liệu/trách nhiệm liên quan | Căn cứ và giới hạn |
| --- | --- | --- |
| Module hợp đồng | Active, đơn vị/NVKD phụ trách, kỳ và lịch sử cấu hình, dịch vụ bắt buộc/phụ lục hiệu lực | S01/S02/S09; PAY-DEC-004/005/011/012; chỉ đọc, không quản lý hợp đồng ở đây |
| Module bảng giá | Giá được hợp đồng chỉ định, dịch vụ/đơn vị tính và khoảng hiệu lực | S01/S04/PAY-01/PAY-03; PAY-DEC-008/009/012; chỉ đọc |
| Module sản lượng | Sản lượng đã đối soát và phân bổ theo ngày/khoảng đổi giá | S01/S02/PAY-02; PAY-DEC-005/008/012; không thực hiện đối soát tại đây |
| Kế toán xuất hóa đơn | KTT bàn giao sau Approved, kế toán đối chiếu và xác nhận tiếp nhận | S01; PAY-DEC-001/013; hóa đơn ngoài module, không thêm trạng thái bảng hoặc mặc định API tích hợp |

Nguồn không chốt giao thức/API/database chung hoặc công nghệ. Không thêm quản lý hóa đơn, công nợ, ký điện tử, thuế/phí/chiết khấu vào công thức. PAY-DEC-009 chốt cách tính trong phạm vi bài, không là quyết định miễn thuế. Các chủ quản cần cung cấp dữ liệu/phân công thực tế; việc người dùng chấp thuận quy tắc chưa chứng minh các nguồn đã đáp ứng.

## 3. Thuật ngữ, tác nhân và dữ liệu nghiệp vụ

### 3.1 Thuật ngữ

| Thuật ngữ | Ý nghĩa hiện hành | Căn cứ |
| --- | --- | --- |
| NVKD | Người lập bảng; phụ trách hợp đồng hoặc được bàn giao chính thức để sửa/trình/hủy | S02/S04/S05; PAY-DEC-006/007/011 |
| KTT | Duyệt theo ngưỡng; lập/trình hồ sơ điều chỉnh và phụ trách bàn giao kế toán | S02/S06/S07; PAY-DEC-001/002/013 |
| GĐKD | Người duyệt tổng chính thức từ 500 triệu trở lên, độc lập với người lập | S02/S06; PAY-DEC-002/011 |
| Kỳ thanh toán | Tháng/quý dương lịch theo cấu hình hợp đồng hiệu lực, quý bao phủ ba tháng | S02; PAY-DEC-004 |
| Sản lượng đã đối soát | Sản lượng hai bên xác nhận; đủ phân bổ theo khoảng giá, kể cả số 0 được xác nhận | S02/PAY-02; PAY-DEC-005/008 |
| Dòng thanh toán | Dịch vụ, sản lượng, đơn giá, thành tiền; có thể tách nhiều dòng theo hiệu lực giá | S02/PAY-03; PAY-DEC-008/009 |
| Tổng chính thức | Tổng các thành tiền dòng đã HALF_UP đến 1 đồng; dùng cho hiển thị/trình/duyệt/bàn giao | PAY-DEC-009 |
| Hồ sơ điều chỉnh | Hồ sơ liên kết bản Approved hiện hành, đầy đủ dòng/tổng sau điều chỉnh và chênh lệch/biên bản | S07; PAY-DEC-001 |
| Điều chỉnh đang xử lý | Hồ sơ điều chỉnh Draft/PendingApproval/Rejected; tối đa một hồ sơ thuộc tập này cho mỗi chuỗi, tạm dừng bàn giao mới | PAY-DEC-015; xác nhận R002-F001, không phải thuật ngữ gốc PDF |
| Draft | Được sửa trong phạm vi quyền, cập nhật nguồn chủ động, trình hoặc hủy | S03/S04/S08; PAY-DEC-006/007/012 |
| PendingApproval | Lần trình hợp lệ đã ghi nhận, có người duyệt được giao; lỗi thông báo không đổi trạng thái | S03/S05; PAY-DEC-011/014 |
| Approved | Nội dung đã duyệt được giữ nguyên; điều chỉnh hiệu lực qua hồ sơ khác đã duyệt | S03/S06/PAY-05; PAY-DEC-001 |
| Rejected | Kết quả từ chối có lý do, giữ nội dung đã trình; phụ trách chủ động mở lại Draft | S03/PAY-08; PAY-DEC-003 |
| Cancelled | Draft đã hủy, không dùng lại; giải phóng kỳ của bảng gốc để lập bảng mới đủ điều kiện | S08; PAY-DEC-004/007 |

### 3.2 Tác nhân và giới hạn quyền

| Tác nhân | Quyền/trách nhiệm đã xác nhận | Giới hạn |
| --- | --- | --- |
| NVKD phụ trách/được bàn giao | Lập, sửa ghi chú Draft, cập nhật nguồn chủ động, trình, mở lại Rejected, hủy Draft | PAY-DEC-003/006/007/011; không ghi đè tài chính hoặc xử lý bảng ngoài phân công |
| KTT | Duyệt/từ chối dưới 500 triệu nếu được giao; lập/trình/sửa lý do/biên bản/mở lại/hủy Draft điều chỉnh mình phụ trách; bàn giao kế toán | PAY-DEC-001/002/003/007/011/013; không tự duyệt/từ chối bảng hoặc điều chỉnh mình lập |
| GĐKD | Duyệt/từ chối từ 500 triệu nếu được giao | PAY-DEC-002/011; phải độc lập người lập |
| Đầu mối phân công có thẩm quyền | Chỉ định/thay thế người xử lý cùng thẩm quyền, lưu bàn giao/audit và thông báo người mới | PAY-DEC-011; không tự nâng/hạ cấp hoặc cho người lập tự xử lý |
| Kế toán tiếp nhận | Đối chiếu căn cứ Approved, xác nhận tiếp nhận, xử lý hóa đơn ngoài module | PAY-DEC-013; không mặc định là KTT |
| Đầu mối vận hành | Theo dõi lỗi gửi quá giới hạn, yêu cầu gửi lại thủ công cùng lần trình | PAY-DEC-014; không tạo vòng duyệt mới |

**S09/PAY-07 và PAY-DEC-001/011:** loại cả người lập gốc và người lập hồ sơ điều chỉnh khỏi quyền duyệt/từ chối. Bàn giao không thay danh tính người lập. Thiếu người cùng thẩm quyền hợp lệ thì giữ Draft/chặn trình. Quyền xem theo đơn vị quy định riêng, không suy ra quyền sửa.

### 3.3 Dữ liệu cần phân tích, chưa phải schema

| Nhóm | Nội dung cần có | Căn cứ |
| --- | --- | --- |
| Căn cứ lập/tính | Hợp đồng/kỳ/lịch sử cấu hình, dịch vụ bắt buộc, sản lượng đối soát theo thời gian, bảng giá/hiệu lực | S02-S05/PAY-01 đến 04; PAY-DEC-004/005/008/012 |
| Kết quả | Sản lượng/đơn giá tối đa 6 số lẻ, tích trước làm tròn, thành tiền từng dòng và tổng chính thức | S04/PAY-03; PAY-DEC-009 |
| Xử lý | Người lập/phụ trách/được giao, lần trình, trạng thái, lý do từ chối/hủy, bàn giao quyền | S03-S09; PAY-DEC-003/007/011/014 |
| Điều chỉnh/bàn giao | Chuỗi bảng và bản Approved hiện hành, nội dung trước/sau/chênh lệch, biên bản, mã lần duyệt, bằng chứng gửi/nhận | S01/S07; PAY-DEC-001/013 |
| Lịch sử | Ai/lúc nào/thay đổi gì, nội dung trước-sau khi sửa Draft, lần gửi/lỗi và đối chiếu nguồn | S10; PAY-DEC-006/011/012/014 |

Không chốt kiểu số triển khai, PK/FK, lưu bản chụp/so sánh phiên bản hoặc công nghệ audit tại bước ghi nhận nghiệp vụ.

## 4. Phân tích user story và tiêu chí chấp nhận

AC dùng **Cho trước - Khi - Thì**. Giữ mã đã có; 30 AC ban đầu theo PRM-006, 7 AC bổ sung sau R002 được ghi căn cứ riêng, không thay phát biểu PDF. Mỗi US áp dụng toàn bộ điều kiện/ngoại lệ trong các PAY-DEC liên quan, kể cả phần chưa được viết thành AC riêng. Ví dụ chi tiết và kế hoạch bổ sung kiểm chứng ở decisions.md/issues.md là yêu cầu đã chấp thuận, chưa phải kết quả chạy ứng dụng.

### 4.1 US-PAY-01 - Lập bảng thanh toán

- **Tác nhân/mục tiêu:** NVKD được phân công lập bảng hợp đồng/kỳ; S04, PAY-DEC-011.
- **Điều kiện trước:** Active, giá phù hợp theo ngày dịch vụ, sản lượng đã đối soát/phân bổ được, kỳ không chồng tháng của bảng chưa Cancelled; S09/PAY-01/02 và PAY-DEC-004/007/008/012.
- **Luồng chính:** Chọn hợp đồng/kỳ → đọc căn cứ và dịch vụ bắt buộc → sinh/tách dòng theo hiệu lực giá → nhân/làm tròn dòng/tính tổng → lưu Draft. Chỉ sửa ghi chú; dữ liệu tài chính cập nhật từ nguồn sau xem/chấp nhận thay đổi, audit mọi lần lưu; PAY-DEC-005/006/008/009/012.
- **Ngoại lệ:** Thiếu/trùng giá, thiếu phân bổ sản lượng hoặc nguồn không nhất quán thì chặn lập/cập nhật. Sai hợp đồng/kỳ thì hủy và lập mới; Cancelled không khôi phục. Kỳ lẻ cần căn cứ hợp đồng cụ thể, thiếu thì chặn; PAY-DEC-004/007/008.
- **Đầu vào/ra:** Hợp đồng/kỳ/căn cứ nguồn → Draft có dòng/tổng và lịch sử. **Quy tắc/NFR:** PAY-01/02/03, PAY-05, NFR-PAY-01/02; PAY-DEC-004 đến 010, 011/012.

| Mã AC | Cho trước - Khi - Thì | Nguồn | Căn cứ | Mức độ rõ/điểm mở |
| --- | --- | --- | --- | --- |
| AC-US-PAY-01-01 | Cho trước hợp đồng Active, căn cứ giá/sản lượng hợp lệ, kỳ không chồng tháng; Khi NVKD được phân công lập bảng; Thì sinh dòng theo khoảng hiệu lực giá, tính tổng chính thức và lưu Draft | S04; S09/PAY-01, PAY-02, PAY-03; PAY-DEC-004, PAY-DEC-005, PAY-DEC-008, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-02 | Cho trước hợp đồng không Active; Khi lập bảng thông thường; Thì chặn lập, không áp ngoại lệ điều chỉnh kỳ cũ cho bảng thông thường | S09/PAY-01; PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-03 | Cho trước thiếu giá phù hợp hoặc nhiều giá cùng ưu tiên cho một dịch vụ/thời điểm; Khi lập/cập nhật; Thì chặn và yêu cầu chủ quản xử lý, không chọn giá mới nhất | S09/PAY-01; PAY-DEC-008 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-04 | Cho trước sản lượng kỳ chưa Đã đối soát; Khi lập bảng; Thì chặn lập | S09/PAY-02; PAY-DEC-008, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-05 | Cho trước sản lượng và đơn giá tối đa 6 số lẻ, đúng thời gian dịch vụ; Khi tính tiền; Thì tính tích chính xác, HALF_UP từng dòng đến đồng rồi cộng thành tổng; ví dụ 100,5 + 100,5 thành 202 đồng | S09/PAY-03; PAY-DEC-008, PAY-DEC-009 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-06 | Cho trước Draft và người phụ trách có quyền; Khi sửa ghi chú; Thì giữ nguyên tiền và audit; khi sửa trực tiếp số liệu tài chính hoặc thêm/xóa dòng bằng tay thì chặn | S04; PAY-DEC-006, PAY-DEC-011 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-07 | Cho trước tháng của hợp đồng đã thuộc một bảng chưa Cancelled, kể cả bảng quý; Khi lập bảng khác chồng tháng đó; Thì chặn và bảo đảm hai yêu cầu đồng thời không tạo hai bảng | S09/PAY-01; PAY-DEC-004, PAY-DEC-007 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-01-08 | Cho trước chỉ còn bảng cũ Cancelled chiếm kỳ và nguồn/cấu hình hiện hành hợp lệ; Khi lập lại; Thì tạo mã mới liên kết bảng hủy và kiểm tra mọi tháng của kỳ, không khôi phục bản cũ | S02; S09/PAY-01; S08; PAY-DEC-004, PAY-DEC-007, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |

### 4.2 US-PAY-02 - Trình duyệt

- **Tác nhân/mục tiêu:** NVKD phụ trách trình bảng; KTT trình hồ sơ điều chỉnh theo US-PAY-04; S05, PAY-DEC-001/011.
- **Điều kiện trước:** Draft, đúng quyền, đủ dịch vụ bắt buộc, tổng chính thức và tổng trước làm tròn không âm; nguồn kiểm tra lại hợp lệ/không đổi nội dung tính tiền; có người duyệt hợp lệ; PAY-DEC-005/009/011/012.
- **Luồng chính:** Kiểm tra điều kiện → xác định người duyệt theo tổng chính thức → ghi nhận một lần trình/PendingApproval, cố định nội dung/căn cứ → gửi thông báo, audit; PAY-DEC-002/011/012/014.
- **Ngoại lệ:** Nguồn đổi/không đọc được hoặc thiếu người duyệt thì giữ Draft, chặn trình và nêu lý do. Lỗi gửi sau khi ghi nhận lần trình thì giữ PendingApproval, hiển thị chờ gửi lại; người duyệt xem được danh sách chờ. Tối đa 3 lần gửi lại trong 15 phút rồi vận hành theo dõi; dừng thông báo lỗi thời. Trình lặp trả lần trình hiện hữu, không sinh vòng/thông báo mới; PAY-DEC-012/014.
- **Nguồn đổi có ảnh hưởng:** giữ cả Draft và số tiền đã lưu; không tự đổi tiền rồi trình ngay. Người phụ trách phải xem/chấp nhận cập nhật trước lần trình sau; PAY-DEC-012, AC-US-PAY-02-06.
- **Đầu vào/ra:** Draft/tổng/căn cứ/phân công → một PendingApproval hoặc bị chặn; tình trạng gửi theo dõi riêng. **Quy tắc/NFR:** PAY-01/02/04/06/07, NFR-PAY-01; PAY-DEC-002/005/009/011/012/014.

| Mã AC | Cho trước - Khi - Thì | Nguồn | Căn cứ | Mức độ rõ/điểm mở |
| --- | --- | --- | --- | --- |
| AC-US-PAY-02-01 | Cho trước Draft đúng quyền, đủ dịch vụ, tổng hợp lệ, nguồn kiểm tra lại không đổi ảnh hưởng và người duyệt hợp lệ; Khi trình; Thì ghi một lần PendingApproval, cố định nội dung/căn cứ, gửi thông báo và audit | S03, S05; S09/PAY-04; S10; PAY-DEC-002, PAY-DEC-005, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012, PAY-DEC-014 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-02-02 | Cho trước tổng chính thức âm hoặc tổng các tích trước làm tròn âm; Khi trình; Thì chặn, giữ Draft kể cả tổng chính thức đã làm tròn về 0 | S05; S09/PAY-04; PAY-DEC-009 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-02-03 | Cho trước bảng thiếu dịch vụ bắt buộc trong thời gian hợp đồng áp dụng hoặc chưa xác định được danh sách; Khi trình; Thì chặn và nêu dữ liệu thiếu, không coi danh sách không đọc được là rỗng | S09/PAY-04; PAY-DEC-005 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-02-04 | Cho trước tổng chính thức 0, tổng trước làm tròn không âm, đủ dịch vụ và điều kiện khác; Khi trình; Thì không chặn chỉ vì tổng 0; dòng sản lượng 0 phải có căn cứ đã đối soát | S09/PAY-04; PAY-DEC-005, PAY-DEC-009 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-02-05 | Cho trước cùng lần trình bị lặp/đồng thời hoặc PendingApproval bị bấm Trình lại; Khi xử lý; Thì chỉ một lần trình được ghi nhận, trả lần hiện hữu; lỗi thông báo không đổi về Draft và gửi lại cùng tham chiếu theo giới hạn 3 lần/15 phút | S09/PAY-04; S03; PAY-DEC-014 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-02-06 | Cho trước Draft đang lưu 100 triệu và nguồn liên quan đổi có ảnh hưởng thành 110 triệu; Khi người phụ trách trình; Thì chặn trình, thông báo khác biệt, giữ Draft và 100 triệu đến khi người phụ trách xem/chấp nhận cập nhật; không tự đổi tiền rồi trình ngay | S04/S05; S09/PAY-01, PAY-02, PAY-03; PAY-DEC-012; R002-F002 | Quy tắc đã xác nhận PRM-006; bổ sung AC theo findings R002 mục 4.3, PRM-011 | Chưa kiểm thử ứng dụng |
| AC-US-PAY-02-07 | Cho trước Draft và không đọc được nguồn cần kiểm tra; Khi người phụ trách trình; Thì chặn trình, nêu nguồn chưa kiểm tra được, giữ Draft và số tiền đã lưu, không tạo PendingApproval | S05; S09/PAY-01, PAY-02; PAY-DEC-012; R002-F002 | Quy tắc đã xác nhận PRM-006; bổ sung AC theo findings R002 mục 4.3, PRM-011 | Chưa kiểm thử ứng dụng |

### 4.3 US-PAY-03 - Duyệt hoặc từ chối

- **Tác nhân/mục tiêu:** KTT dưới 500 triệu, GĐKD từ 500 triệu, đúng người được giao và độc lập người lập; S06, PAY-DEC-002/011.
- **Điều kiện trước:** PendingApproval, nội dung/căn cứ cố định; kiểm tra nguồn trước duyệt. Không còn hợp lệ/đổi tính tiền thì chặn duyệt, người được giao từ chối có lý do để sửa; không đọc được nguồn thì giữ chờ, không tự từ chối; PAY-DEC-012.
- **Luồng chính:** Người được giao đủ điều kiện duyệt → Approved, audit. Một cấp theo ngưỡng, thao tác đồng thời chỉ một kết quả; PAY-DEC-002/011.
- **Luồng thay thế:** Từ chối có lý do → Rejected; người phụ trách chủ động “Chỉnh sửa lại” → Draft, giữ lịch sử cũ. Không có lý do hoặc người lập tự duyệt/từ chối thì chặn. Thay người đúng thẩm quyền có audit/thông báo, người cũ mất quyền; PAY-DEC-003/011.
- **Đầu vào/ra:** Bảng/tổng/người lập/người được giao/căn cứ nguồn/lý do → Approved hoặc Rejected, audit. KTT bàn giao thủ công ngoài module chỉ sau Approved, đối chiếu đúng lần duyệt và điều chỉnh; PAY-DEC-013.
- **Điều kiện bàn giao của chuỗi:** tạm dừng bàn giao mới khi có điều chỉnh Draft/PendingApproval/Rejected. Chỉ tiếp tục theo bản Approved hiện hành khi không còn hồ sơ đang xử lý; điều chỉnh Approved dùng căn cứ mới đã duyệt, Cancelled không đổi căn cứ đã duyệt; PAY-DEC-015, mục 6.1.
- **Quy tắc/NFR:** PAY-05/06/07/08, NFR-PAY-01; PAY-DEC-001/002/003/011/012/013/015.

| Mã AC | Cho trước - Khi - Thì | Nguồn | Căn cứ | Mức độ rõ/điểm mở |
| --- | --- | --- | --- | --- |
| AC-US-PAY-03-01 | Cho trước PendingApproval tổng chính thức 499.999.999 đồng, nguồn kiểm tra hợp lệ và KTT được giao độc lập người lập; Khi KTT duyệt; Thì Approved và audit | S03, S06; S09/PAY-06, PAY-07; PAY-DEC-002, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-02 | Cho trước PendingApproval tổng chính thức 500.000.001 đồng, nguồn kiểm tra hợp lệ và GĐKD được giao độc lập người lập; Khi GĐKD duyệt; Thì Approved và audit | S03, S06; S09/PAY-06, PAY-07; PAY-DEC-002, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-03 | Cho trước tổng chính thức đúng 500.000.000 đồng; Khi phân công/xét quyền duyệt; Thì chỉ GĐKD đủ điều kiện được giao, không giao KTT | S06; S09/PAY-06; PAY-DEC-002, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-04 | Cho trước người xử lý là người lập bảng gốc hoặc người lập điều chỉnh; Khi duyệt hoặc từ chối kể cả đúng vai trò; Thì chặn; thiếu người thay thế cùng thẩm quyền thì không được trình từ Draft | S09/PAY-07; PAY-DEC-001, PAY-DEC-011 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-05 | Cho trước PendingApproval; Khi từ chối mà không nhập lý do hoặc lý do rỗng; Thì chặn từ chối | S06; S09/PAY-08; PAY-DEC-003, PAY-DEC-011 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-06 | Cho trước người được giao đủ quyền và lý do không rỗng; Khi từ chối; Thì Rejected, giữ nội dung/lịch sử; khi người phụ trách chọn Chỉnh sửa lại thì Draft và audit trước khi sửa/trình lại | S03; S09/PAY-08; PAY-DEC-003, PAY-DEC-011 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-07 | Cho trước người không đúng vai trò/người được giao hoặc người cũ đã bị thay thế; Khi duyệt hoặc từ chối; Thì chặn; hai người thao tác đồng thời chỉ ghi một kết quả hợp lệ | S06; S09/PAY-06; PAY-DEC-002, PAY-DEC-011 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-08 | Cho trước bảng chưa Approved; Khi bàn giao kế toán; Thì chưa đủ điều kiện; với Approved, KTT bàn giao thủ công đúng mã bảng/lần duyệt, lưu bằng chứng gửi/nhận, gửi lại không tạo nghĩa vụ tính tiền lần hai | S01, S06; PAY-DEC-001, PAY-DEC-013 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-03-09 | Cho trước PendingApproval và nguồn liên quan đổi ảnh hưởng hoặc không còn hợp lệ; Khi người được giao duyệt; Thì chặn duyệt, giữ PendingApproval và nội dung/số tiền/căn cứ lần trình, thông báo người được giao; không tự sửa tiền hoặc tự từ chối. Khi người đó từ chối có lý do thì về Rejected theo AC-US-PAY-03-06 để người phụ trách mở lại Draft xử lý | S04/S05/S06; S09/PAY-01, PAY-02, PAY-03; PAY-DEC-003, PAY-DEC-012; R002-F002 | Quy tắc đã xác nhận PRM-006; bổ sung AC theo findings R002 mục 4.3, PRM-011 | Chưa kiểm thử ứng dụng |
| AC-US-PAY-03-10 | Cho trước PendingApproval và không đọc được nguồn cần kiểm tra; Khi người được giao duyệt; Thì giữ PendingApproval và nội dung/số tiền/căn cứ lần trình để chờ kiểm tra, không tự Approved hoặc Rejected | S06; S09/PAY-01, PAY-02; PAY-DEC-012; R002-F002 | Quy tắc đã xác nhận PRM-006; bổ sung AC theo findings R002 mục 4.3, PRM-011 | Chưa kiểm thử ứng dụng |

### 4.4 US-PAY-04 - Điều chỉnh bảng đã duyệt

- **Tác nhân/mục tiêu:** KTT lập hồ sơ điều chỉnh liên kết để xử lý sai sót, không sửa bản Approved; S07/PAY-05, PAY-DEC-001.
- **Điều kiện trước:** Có bản Approved hiện hành, sai sót và căn cứ nguồn sửa hợp lệ; không còn điều chỉnh Draft/PendingApproval/Rejected trong chuỗi khi lập hồ sơ khác (PAY-DEC-015). Trước trình phải đủ lý do/trước-sau/chênh lệch/biên bản có mã/ngày/các bên xác nhận. Ngoại lệ Active cho kỳ cũ của hợp đồng đã kết thúc theo PAY-DEC-012, không áp lập bảng thường.
- **Luồng chính:** KTT lập Draft điều chỉnh đủ dòng/tổng mới → cập nhật số liệu từ nguồn có kiểm tra → đính biên bản/trình → người khác đủ cùng thẩm quyền duyệt theo tổng đầy đủ mới → hồ sơ Approved có hiệu lực; audit mọi thay đổi. Không cộng toàn bộ tổng mới vào cũ; chênh lệch giảm có thể âm nhưng tổng mới phải hợp lệ; PAY-DEC-001/002/006/008/009/011/012.
- **Ngoại lệ:** Thiếu biên bản/nguồn/người duyệt thì chặn trình. Rejected vẫn giữ vị trí và dừng bàn giao; KTT phụ trách mở lại chính hồ sơ sang Draft để sửa hoặc hủy. Muốn bỏ hồ sơ Rejected phải mở Draft rồi hủy đúng quyền trước khi tạo khác. Cancelled kết thúc giữ vị trí của chính hồ sơ, không khôi phục/đổi giá trị đã duyệt/giải phóng kỳ gốc. Chỉ điều chỉnh Approved có hiệu lực; quyền tạo tiếp và tiếp tục bàn giao kiểm tra toàn chuỗi theo mục 6.1; PAY-DEC-001/003/007/015.
- **Đầu vào/ra:** Bản Approved/căn cứ sửa/biên bản → hồ sơ liên kết, tổng sau điều chỉnh/chênh lệch và lịch sử; bản gốc giữ nguyên, không chiếm thêm tháng. Phối hợp kế toán theo PAY-DEC-013, hóa đơn ngoài module.
- **Quy tắc/NFR:** PAY-03/04/05/06/07/08, NFR-PAY-01; PAY-DEC-001/002/003/006/007/008/009/011/012/013/015.

| Mã AC | Cho trước - Khi - Thì | Nguồn | Căn cứ | Mức độ rõ/điểm mở |
| --- | --- | --- | --- | --- |
| AC-US-PAY-04-01 | Cho trước Approved có sai sót, nguồn sửa hợp lệ và KTT lập hồ sơ có biên bản trước trình; Khi điều chỉnh; Thì bản gốc giữ nguyên, tạo hồ sơ liên kết duyệt riêng theo tổng đầy đủ mới; chỉ sau Approved mới có hiệu lực | S07; S09/PAY-05; PAY-DEC-001, PAY-DEC-002, PAY-DEC-008, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-04-02 | Cho trước Draft điều chỉnh chưa có biên bản đủ mã/ngày/các bên xác nhận; Khi KTT trình điều chỉnh; Thì chặn trình, bản Approved hiện hành không đổi | S07; PAY-DEC-001 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-04-03 | Cho trước một hồ sơ điều chỉnh được xử lý đúng quyền; Khi lưu thay đổi/chuyển trạng thái; Thì audit ai/lúc nào/nội dung trước-sau, liên kết bản Approved và chênh lệch, không cộng lặp tổng | S07, S10; PAY-DEC-001, PAY-DEC-006 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-04-04 | Cho trước chuỗi có một điều chỉnh ở Draft, PendingApproval hoặc Rejected (xét từng trạng thái); Khi KTT yêu cầu tạo điều chỉnh khác hoặc bàn giao mới của chuỗi; Thì chặn tạo khác và tạm dừng bàn giao mới; bản Approved hiện hành giữ nguyên, Rejected vẫn giữ vị trí điều chỉnh duy nhất | S07; PAY-DEC-001, PAY-DEC-003, PAY-DEC-013, PAY-DEC-015; R002-F001 | Đã xác nhận bổ sung tại findings R002 mục 4.2, PRM-011 | Ba biến thể trạng thái; chưa kiểm thử ứng dụng |
| AC-US-PAY-04-05 | Cho trước điều chỉnh Rejected và KTT phụ trách đúng quyền; Khi mở lại chính hồ sơ sang Draft; Thì vẫn giữ vị trí và dừng bàn giao mới. Khi hủy Draft có lý do thì Cancelled, audit, không khôi phục/đổi giá trị đã duyệt/giải phóng kỳ gốc; khi không còn hồ sơ khác đang xử lý, được tiếp tục bàn giao theo bản Approved hiện hành và tạo hồ sơ điều chỉnh mã mới nếu đủ điều kiện | S07/S08/S10; PAY-DEC-003, PAY-DEC-007, PAY-DEC-013, PAY-DEC-015; R002-F001 | Đã xác nhận bổ sung tại findings R002 mục 4.2, PRM-011 | Bao phủ Rejected → Draft → Cancelled; chưa kiểm thử ứng dụng |
| AC-US-PAY-04-06 | Cho trước điều chỉnh PendingApproval đủ điều kiện và người duyệt độc lập hợp lệ; Khi duyệt thành Approved; Thì kết thúc giữ vị trí của hồ sơ đó, dùng căn cứ mới đã duyệt, không cộng lặp tổng. Khi không còn hồ sơ khác đang xử lý thì được tiếp tục bàn giao và tạo điều chỉnh tiếp dựa trên bản Approved hiện hành nếu đủ điều kiện; nếu đã có điều chỉnh khác Draft/PendingApproval/Rejected thì vẫn chặn tạo thêm và dừng bàn giao mới dù hồ sơ trước đã Approved hoặc Cancelled | S06/S07; PAY-DEC-001, PAY-DEC-011, PAY-DEC-013, PAY-DEC-015; R002-F001 | Đã xác nhận bổ sung tại findings R002 mục 4.2, PRM-011 | Kiểm điều kiện toàn chuỗi; chưa kiểm thử ứng dụng |

### 4.5 US-PAY-05 - Hủy bảng nháp

- **Tác nhân/mục tiêu:** NVKD phụ trách hoặc được bàn giao hợp lệ hủy Draft; KTT có quyền tương ứng với Draft điều chỉnh mình phụ trách; S08, PAY-DEC-007/011.
- **Điều kiện trước:** Draft, đúng quyền, có lý do. Rejected cần mở lại Draft trước; không hủy PendingApproval/Approved; PAY-DEC-003/007.
- **Luồng chính:** Hủy → Cancelled, lưu lý do/audit; không phục hồi/sửa/trình chính bảng đó; S03/S08/S10, PAY-DEC-007.
- **Ngoại lệ/lập lại:** Bảng gốc Cancelled giải phóng kỳ; bảng mới có mã mới/tham chiếu bảng hủy/lý do, kiểm tra lại nguồn/cấu hình/chồng kỳ và lập đồng thời. Hủy điều chỉnh không giải phóng kỳ của bảng gốc Approved; PAY-DEC-004/007/012.
- **Hủy điều chỉnh sau từ chối:** KTT phụ trách mở Rejected → Draft rồi mới hủy có lý do/audit. Cancelled kết thúc giữ vị trí của hồ sơ đó; chỉ tạo điều chỉnh khác hoặc tiếp tục bàn giao khi không còn hồ sơ đang xử lý và đủ điều kiện, theo PAY-DEC-015 và AC-US-PAY-04-05/06.
- **Đầu vào/ra:** Draft/người xử lý/lý do → Cancelled và audit. **Quy tắc/NFR:** PAY-01/04, NFR-PAY-01; PAY-DEC-003/004/007/011/012/015.

| Mã AC | Cho trước - Khi - Thì | Nguồn | Căn cứ | Mức độ rõ/điểm mở |
| --- | --- | --- | --- | --- |
| AC-US-PAY-05-01 | Cho trước Draft, người phụ trách hoặc được bàn giao hợp lệ và lý do hủy; Khi hủy; Thì Cancelled và audit; người không được phân công hoặc thiếu lý do bị chặn | S03, S08, S10; PAY-DEC-007, PAY-DEC-011 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-05-02 | Cho trước Cancelled; Khi khôi phục/sửa/trình chính bảng đó; Thì chặn, không dùng lại | S08; PAY-DEC-007 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |
| AC-US-PAY-05-03 | Cho trước bảng gốc Cancelled và kỳ không bị bảng khác chiếm, nguồn/cấu hình hợp lệ; Khi lập mới thay thế; Thì được mã mới liên kết bảng hủy; hủy Draft điều chỉnh không giải phóng kỳ gốc Approved | S08; S09/PAY-01; S02; PAY-DEC-004, PAY-DEC-007, PAY-DEC-012 | Đã xác nhận - PRM-006; đối chiếu nguồn PDF | Quy tắc đã chốt; chưa kiểm thử ứng dụng |

## 5. Danh mục quy tắc nghiệp vụ

Cột **Tóm lược phát biểu nguồn** diễn đạt rút gọn **PDF trang 2 mục 5, S09**, không phải trích nguyên văn. Cột áp dụng ghi quyết định đã xác nhận để xử lý mâu thuẫn/bổ sung; không sửa PDF hoặc tự ưu tiên rule hơn US. AC tương ứng nằm tại mục 4, NFR tại mục 7; áp dụng toàn văn quyết định trong decisions.md. Đổi nhãn khi biên tập theo tùy chọn F003, không thay đổi nghiệp vụ.

| Mã | Tóm lược phát biểu nguồn | Diễn giải áp dụng đã xác nhận | AC liên quan |
| --- | --- | --- | --- |
| PAY-01 | Chỉ lập bảng cho hợp đồng Active và có bảng giá phù hợp. Mỗi hợp đồng chỉ có một bảng thanh toán cho mỗi tháng | Tháng/quý chống chồng mọi tháng của bảng chưa Cancelled, cho lập mới sau hủy; giá theo ngày dịch vụ; kiểm tra lại nguồn. Ngoại lệ Active chỉ cho điều chỉnh kỳ cũ có điều kiện. PAY-DEC-004/007/008/012 | AC-US-PAY-01-01/02/03/07/08, AC-US-PAY-05-03; nguồn lỗi/đổi: AC-US-PAY-02-06/07, AC-US-PAY-03-09/10 |
| PAY-02 | Sản lượng kỳ phải Đã đối soát trước khi lập | Giữ điều kiện đã đối soát lúc lập, kiểm tra lại trước trình/duyệt; cần phân bổ theo khoảng giá. PAY-DEC-008/012 | AC-US-PAY-01-01/04, AC-US-PAY-02-01, AC-US-PAY-03-01 đến 03; nguồn lỗi/đổi: AC-US-PAY-02-06/07, AC-US-PAY-03-09/10 |
| PAY-03 | Thành tiền mỗi dòng = sản lượng × đơn giá tại thời điểm tính | Giá theo ngày dịch vụ, đầu vào tối đa 6 số lẻ, HALF_UP dòng đến đồng rồi cộng tổng; thay đổi căn cứ xử lý theo PAY-DEC-012. PAY-DEC-008/009 | AC-US-PAY-01-01/05/06; nguồn đổi: AC-US-PAY-02-06, AC-US-PAY-03-09 |
| PAY-04 | Khi trình chặn tổng âm hoặc thiếu dòng dịch vụ bắt buộc; chống thao tác trình duyệt trùng | Danh sách bắt buộc từ hợp đồng hiệu lực; chặn tổng chính thức hoặc tổng trước làm tròn âm, không cấm 0; chống trình trùng, lỗi thông báo theo dõi riêng. PAY-DEC-005/009/014 | AC-US-PAY-02-01 đến 05 |
| PAY-05 | Approved không được sửa dưới bất kỳ hình thức nào | Không sửa nội dung Approved, KTT lập hồ sơ liên kết/biên bản/duyệt riêng, chỉ bản điều chỉnh Approved có hiệu lực. Vòng đời/điều kiện chuỗi theo PAY-DEC-015 bổ sung PAY-DEC-001 | AC-US-PAY-04-01 đến 06 |
| PAY-06 | Tổng tiền trên 500 triệu do GĐKD duyệt; còn lại KTT | Tổng chính thức từ 500 triệu là GĐKD, dưới là KTT, một cấp. PAY-DEC-002/009/011 | AC-US-PAY-03-01/02/03/07 |
| PAY-07 | Người lập không được duyệt bảng do chính mình lập | Cấm người lập gốc và người lập điều chỉnh tự duyệt hoặc từ chối, phân công chính/thay thế cùng thẩm quyền. PAY-DEC-001/011 | AC-US-PAY-03-04/07 |
| PAY-08 | Từ chối phải có lý do; bảng về Draft để NVKD sửa và trình lại | Từ chối có lý do → Rejected; phụ trách chủ động Chỉnh sửa lại → Draft, giữ lịch sử/audit. PAY-DEC-003/011 | AC-US-PAY-03-05/06, AC-US-PAY-01-06, AC-US-PAY-02-01 |

### 5.1 Đối chiếu ngưỡng duyệt với nguồn

| Tổng chính thức | Theo S06, trang 2 mục 4.3 | Theo S09/PAY-06, trang 2 mục 5 | Áp dụng PAY-DEC-002 |
| --- | --- | --- | --- |
| Dưới 500.000.000 đồng | KTT | KTT | KTT đủ quyền/được giao |
| Đúng 500.000.000 đồng | GĐKD | KTT | GĐKD đủ quyền/được giao; mâu thuẫn đã giải quyết |
| Trên 500.000.000 đồng | GĐKD | GĐKD | GĐKD đủ quyền/được giao |

Tổng theo PAY-DEC-009; kiểm tra danh tính/ủy quyền theo PAY-DEC-001/011. Không dùng cấu hình để mở lại nhánh chưa được chọn.

## 6. Phân tích trạng thái và quyền thao tác

Đây là bảng nghiệp vụ hiện hành theo nguồn và PAY-DEC, chưa phải UML/API/schema. Giữ đủ năm trạng thái; hồ sơ điều chỉnh dùng cùng vòng đời và không làm đổi nội dung bản Approved gốc.

| Trạng thái đầu | Hành động/tác nhân | Điều kiện | Kết quả | Audit/căn cứ |
| --- | --- | --- | --- | --- |
| Chưa có bảng | Lập / NVKD phụ trách | Active, giá/sản lượng/cấu hình hợp lệ, không chồng tháng | Draft | S04/PAY-01/02/03; PAY-DEC-004/008/009/011/012 |
| Draft | Sửa ghi chú / phụ trách | Đúng quyền, không sửa tay số liệu tính tiền | Draft, tổng giữ nguyên | S04/S10; PAY-DEC-006/011 |
| Draft | Cập nhật từ nguồn / phụ trách | Xem/chấp nhận thay đổi, kiểm tra toàn bộ căn cứ liên quan | Draft, tính lại/audit | PAY-DEC-006/008/009/012 |
| Draft | Trình / NVKD hoặc KTT điều chỉnh | Đạt điều kiện trình/nguồn/người duyệt | Một PendingApproval; cố định nội dung/căn cứ, gửi thông báo | S03/S05/S10; PAY-DEC-001/002/005/009/011/012/014 |
| PendingApproval | Duyệt / người được giao | Đúng vai trò, độc lập người lập, nguồn vẫn hợp lệ | Approved | S06/S10; PAY-DEC-002/011/012 |
| PendingApproval | Từ chối / người được giao | Đúng quyền, độc lập, lý do không rỗng | Rejected, giữ nội dung đã trình | S03/PAY-08/S10; PAY-DEC-003/011 |
| Rejected | Chỉnh sửa lại / người phụ trách | Đúng phân công bảng/hồ sơ điều chỉnh | Draft trước sửa/hủy/trình mới | PAY-DEC-001/003/011; audit, giữ lý do cũ |
| PendingApproval | Gửi lại thông báo / vận hành hoặc hệ thống | Cùng lần trình/người nhận hiện hành, chưa có kết quả xử lý | Giữ PendingApproval, không tạo vòng duyệt mới | PAY-DEC-014; lịch sử gửi/lỗi |
| PendingApproval | Phân công lại / đầu mối có thẩm quyền | Người thay đủ cùng vai trò và độc lập | Giữ trạng thái, người cũ hết quyền | PAY-DEC-011; audit/thông báo người mới |
| Approved | Lập hồ sơ điều chỉnh / KTT | Có sai sót/căn cứ hợp lệ, không còn điều chỉnh Draft/PendingApproval/Rejected trong chuỗi | Bản gốc giữ nguyên, hồ sơ liên kết Draft giữ vị trí điều chỉnh duy nhất | S07/S10; PAY-DEC-001/012/015; cần biên bản trước trình |
| Draft | Hủy / người phụ trách | Đúng quyền, lý do hủy | Cancelled | S08/S10; PAY-DEC-007/011; audit |
| Cancelled | Dùng lại chính bảng | Không được phép | Chặn phục hồi/sửa/trình | S08; PAY-DEC-007; bảng mới kiểm tra lại toàn bộ |

Không hủy PendingApproval/Approved/Rejected trực tiếp. Không thêm hai cấp duyệt nối tiếp hoặc quyền sửa theo quyền xem. Lỗi gửi không đổi bảng về Draft; không đọc được nguồn trước duyệt thì giữ PendingApproval chờ kiểm tra, không tự từ chối.

### 6.1 Điều chỉnh giữ vị trí duy nhất và điều kiện bàn giao mới

**Đã xác nhận bổ sung - PAY-DEC-015, findings R002 mục 4.2.** Bảng dưới áp dụng cho hồ sơ điều chỉnh; không dùng Cancelled của điều chỉnh để giải phóng kỳ gốc.

| Trạng thái hồ sơ điều chỉnh | Có được tạo điều chỉnh khác cùng chuỗi? | Bàn giao mới của chuỗi |
| --- | --- | --- |
| Draft | Không | Tạm dừng |
| PendingApproval | Không | Tạm dừng |
| Rejected | Không; mở lại chính hồ sơ này để sửa hoặc hủy | Tạm dừng |
| Approved | Có, dựa trên bản Approved hiện hành và khi không có hồ sơ khác đang xử lý | Được tiếp tục theo căn cứ mới đã duyệt, nếu không có điều chỉnh khác đang xử lý |
| Cancelled | Có, khi không có hồ sơ khác đang xử lý và vẫn đủ điều kiện lập điều chỉnh | Được tiếp tục theo bản Approved hiện hành, nếu không có điều chỉnh khác đang xử lý |

Draft/PendingApproval/Rejected giữ vị trí duy nhất; mở lại Rejected → Draft không giải phóng vị trí. KTT phụ trách muốn bỏ hồ sơ phải mở Draft rồi hủy có lý do/audit. Approved/Cancelled kết thúc giữ vị trí của chính hồ sơ đó; vẫn kiểm tra toàn chuỗi trước tạo tiếp/bàn giao. Cancelled không phục hồi hoặc đổi giá trị đã duyệt. Bàn giao vẫn là công việc thủ công ngoài module theo PAY-DEC-013, có bằng chứng và chống tính tiền hai lần. AC-US-PAY-04-04/05/06 kiểm chứng các nhánh này.

## 7. Yêu cầu phi chức năng và cách kiểm chứng

Mã NFR/AC-NFR do BA đặt. Phần gốc từ S10 được giữ; phạm vi audit và hồ sơ đo cụ thể đã xác nhận theo PAY-DEC, chưa có kết quả chạy ứng dụng.

| Mã | Yêu cầu hiện hành | Căn cứ | Kế hoạch kiểm chứng | Trạng thái |
| --- | --- | --- | --- | --- |
| NFR-PAY-01 | Audit ai/lúc nào/thay đổi gì cho chuyển trạng thái/điều chỉnh; mọi lần lưu sửa Draft gồm trước-sau; phân công lại có audit | S07/S10; PAY-DEC-001/003/006/007/011/012 | Đối chiếu từng sự kiện được phép, bao gồm mở lại Draft, sửa ghi chú/cập nhật nguồn, hủy và điều chỉnh | Đã xác nhận; chưa kiểm thử, không tự thêm retention/công nghệ |
| NFR-PAY-02 | Lập trong không quá 3 giây với hợp đồng dưới 200 dòng; từ chọn Lập đến hiển thị Draft đã lưu, bao gồm nguồn/mạng | S10; PAY-DEC-010 | Bộ 1/199 dòng, cả trường hợp tách giá làm tăng đầu ra; tải 1/10 người đồng thời, 30 lượt/người, từng lượt thành công ≤3 giây, lỗi/timeout không đạt; ghi môi trường/độ trễ nguồn và dòng đầu vào/ra | Đã xác nhận hồ sơ đo; đúng/trên 200 dòng khảo sát riêng, chưa áp ngưỡng mới |

| Mã AC | Cho trước - Khi - Thì | Nguồn | Căn cứ và trạng thái |
| --- | --- | --- | --- |
| AC-NFR-PAY-01-01 | Cho trước một chuyển trạng thái được phép; Khi chuyển thành công; Thì có audit ai/lúc nào/thay đổi gì, kể cả Rejected → Draft và hủy | S10; PAY-DEC-003/007 | Đã xác nhận; chưa kiểm thử |
| AC-NFR-PAY-01-02 | Cho trước lưu thay đổi Draft hoặc xử lý hồ sơ điều chỉnh đúng quyền; Khi lưu thành công; Thì có audit ai/lúc nào/nội dung trước-sau, bản Approved gốc không bị ghi đè | S07/S10; PAY-DEC-001/006 | Đã xác nhận; chưa kiểm thử |
| AC-NFR-PAY-02-01 | Cho trước hợp đồng đủ điều kiện và dưới 200 dòng dịch vụ áp dụng trước tách giá, trong hồ sơ đo PAY-DEC-010; Khi đo từ chọn Lập đến hiển thị Draft đã lưu; Thì mọi lượt thành công không quá 3 giây, lỗi/timeout không tính đạt | S10; PAY-DEC-010 | Đã xác nhận phép đo; chưa thực đo |

Không tính một phản hồi chờ là Draft đã lưu. Không bỏ thời gian nguồn khỏi phép đo. 200 dòng trở lên không bị mặc nhiên cấm lập; chưa được gán yêu cầu 3 giây hoặc SLA mới. Cần ghi cấu hình/mạng/khả dụng nguồn thực tế trước nghiệm thu.

## 8. Ma trận truy vết đầu vào cho Design

### 8.1 Theo US và NFR

| Yêu cầu | Nguồn | Quy tắc liên quan | AC | Vấn đề đã giải quyết | Quyết định áp dụng và mức độ bàn giao |
| --- | --- | --- | --- | --- | --- |
| US-PAY-01 | S02/S04/S09 | PAY-01/02/03/05 | AC-US-PAY-01-01 đến AC-US-PAY-01-08 | PAY-ISS-004, PAY-ISS-005, PAY-ISS-006, PAY-ISS-007, PAY-ISS-008, PAY-ISS-009, PAY-ISS-010, PAY-ISS-011, PAY-ISS-012 | Đã xác nhận: PAY-DEC-004, PAY-DEC-005, PAY-DEC-006, PAY-DEC-007, PAY-DEC-008, PAY-DEC-009, PAY-DEC-010, PAY-DEC-011, PAY-DEC-012; bàn giao Design, chưa triển khai/kiểm thử |
| US-PAY-02 | S03/S05/S09 | PAY-01/02/04/06/07 | AC-US-PAY-02-01 đến AC-US-PAY-02-07 | PAY-ISS-002, PAY-ISS-005, PAY-ISS-009, PAY-ISS-011, PAY-ISS-012, PAY-ISS-014 | Đã xác nhận: PAY-DEC-002, PAY-DEC-005, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012, PAY-DEC-014; F002 bổ sung AC, mục 8.3; chưa triển khai/kiểm thử |
| US-PAY-03 | S01/S03/S06/S09 | PAY-05/06/07/08 | AC-US-PAY-03-01 đến AC-US-PAY-03-10; bàn giao chuỗi: AC-US-PAY-04-04 đến 06 | PAY-ISS-001, PAY-ISS-002, PAY-ISS-003, PAY-ISS-011, PAY-ISS-012, PAY-ISS-013 | Đã xác nhận: PAY-DEC-001, PAY-DEC-002, PAY-DEC-003, PAY-DEC-011, PAY-DEC-012, PAY-DEC-013, PAY-DEC-015; F001/F002, mục 8.3; chưa triển khai/kiểm thử |
| US-PAY-04 | S07/S09/S10 | PAY-03/04/05/06/07/08 | AC-US-PAY-04-01 đến AC-US-PAY-04-06 | PAY-ISS-001, PAY-ISS-002, PAY-ISS-003, PAY-ISS-006, PAY-ISS-007, PAY-ISS-008, PAY-ISS-009, PAY-ISS-011, PAY-ISS-012, PAY-ISS-013 | Đã xác nhận: PAY-DEC-001, PAY-DEC-002, PAY-DEC-003, PAY-DEC-006, PAY-DEC-007, PAY-DEC-008, PAY-DEC-009, PAY-DEC-011, PAY-DEC-012, PAY-DEC-013, PAY-DEC-015; F001, mục 8.3; chưa triển khai/kiểm thử |
| US-PAY-05 | S03/S08/S09 | PAY-01/04 | AC-US-PAY-05-01 đến AC-US-PAY-05-03; hủy điều chỉnh: AC-US-PAY-04-05/06 | PAY-ISS-001, PAY-ISS-003, PAY-ISS-004, PAY-ISS-007, PAY-ISS-011, PAY-ISS-012, PAY-ISS-013 | Đã xác nhận: PAY-DEC-003, PAY-DEC-004, PAY-DEC-007, PAY-DEC-011, PAY-DEC-012, PAY-DEC-013, PAY-DEC-015; F001, mục 8.3; chưa triển khai/kiểm thử |
| NFR-PAY-01 | S07/S10 | Mọi thay đổi trạng thái/điều chỉnh | AC-NFR-PAY-01-01, AC-NFR-PAY-01-02 | PAY-ISS-001, PAY-ISS-003, PAY-ISS-006, PAY-ISS-007, PAY-ISS-011, PAY-ISS-012 | Đã xác nhận: PAY-DEC-001, PAY-DEC-003, PAY-DEC-006, PAY-DEC-007, PAY-DEC-011, PAY-DEC-012; bàn giao Design, chưa triển khai/kiểm thử |
| NFR-PAY-02 | S10 | Điều kiện lập PAY-01/02/03 | AC-NFR-PAY-02-01 | PAY-ISS-010 | Đã xác nhận: PAY-DEC-010; bàn giao Design, chưa triển khai/kiểm thử |

Truy vết từng PAY nằm ở mục 5; đủ 8 quy tắc, 14 quyết định đầu và PAY-DEC-015 bổ sung sau R002. Design cần rà soát component/API/UML/schema trong bộ thiết kế đã có theo phân tích sửa; BA chưa thực hiện bước đó.

### 8.2 Phát hiện bổ sung và kết quả chốt

| Mã vấn đề | Khoảng trống nguồn ban đầu | Quyết định hiện hành |
| --- | --- | --- |
| PAY-ISS-012 | PAY-01/02 ràng buộc lúc lập, chưa quy định nguồn đổi sau Draft hoặc trước duyệt | PAY-DEC-012: giữ căn cứ, kiểm tra lại/chặn/cập nhật chủ động; F002 bổ sung AC-US-PAY-02-06/07 và AC-US-PAY-03-09/10 tại mục 8.3 |
| PAY-ISS-013 | S01 chỉ yêu cầu duyệt trước chuyển kế toán | PAY-DEC-013: KTT bàn giao thủ công có bằng chứng/đối chiếu; PAY-DEC-015 bổ sung điều kiện tạm dừng/tiếp tục của chuỗi, AC-US-PAY-04-04 đến 06; không thêm US hoặc trạng thái hóa đơn |
| PAY-ISS-014 | S05/PAY-04 chưa có luồng lỗi gửi/thử lại | PAY-DEC-014: giữ PendingApproval, giới hạn gửi lại và vận hành theo dõi, không sinh trình trùng |

### 8.3 Truy vết phần bổ sung sau R002

Nguồn PDF xác định phạm vi US; kết quả bổ sung dưới đây căn cứ các quyết định và xác nhận thật tại findings R002 mục 4, không nhận là PDF đã quy định chi tiết.

| Finding | Quyết định/vấn đề | US áp dụng | AC bổ sung | Kết quả mong đợi cần kiểm chứng |
| --- | --- | --- | --- | --- |
| payment-management-R002-F001 | PAY-DEC-015 bổ sung 001/003/007/013; PAY-ISS-001/003/007/013 | US-PAY-03/04 | AC-US-PAY-04-04 | Draft/PendingApproval/Rejected đều giữ vị trí; chặn tạo khác và dừng bàn giao mới |
| payment-management-R002-F001 | PAY-DEC-015; PAY-ISS-001/003/007/013 | US-PAY-03/04/05 | AC-US-PAY-04-05 | Rejected → Draft vẫn giữ vị trí; hủy thành Cancelled kết thúc giữ vị trí của chính hồ sơ, giữ kỳ gốc/căn cứ Approved; tạo tiếp/bàn giao xét toàn chuỗi |
| payment-management-R002-F001 | PAY-DEC-015; PAY-ISS-001/013 | US-PAY-03/04 | AC-US-PAY-04-06 | Approved dùng căn cứ mới, không cộng lặp; chỉ tạo tiếp/bàn giao khi không còn điều chỉnh đang xử lý |
| payment-management-R002-F002 | PAY-DEC-012; PAY-ISS-012 | US-PAY-02 | AC-US-PAY-02-06 | Nguồn đổi 100 → 110 triệu trước trình: chặn, giữ Draft/100 triệu đến khi xem/chấp nhận cập nhật |
| payment-management-R002-F002 | PAY-DEC-012; PAY-ISS-012 | US-PAY-02 | AC-US-PAY-02-07 | Không đọc được nguồn trước trình: chặn, giữ Draft và số tiền đã lưu |
| payment-management-R002-F002 | PAY-DEC-012; PAY-ISS-012 | US-PAY-03 | AC-US-PAY-03-09 | Nguồn đổi/không hợp lệ trước duyệt: chặn, giữ PendingApproval/nội dung lần trình, thông báo; chỉ người được giao từ chối có lý do |
| payment-management-R002-F002 | PAY-DEC-012; PAY-ISS-012 | US-PAY-03 | AC-US-PAY-03-10 | Không đọc được nguồn trước duyệt: giữ PendingApproval/nội dung lần trình chờ kiểm tra, không tự Approved/Rejected |

## 9. Bàn giao cho Design và việc còn cần thực hiện

### 9.1 Căn cứ đã chốt

- PAY-DEC-001 đến PAY-DEC-014 giữ xác nhận PRM-006; PAY-DEC-015 ghi xác nhận bổ sung F001 ngày 12/11/2026 tại findings R002. Nguồn PDF giữ nguyên; không hỏi lại các lựa chọn đã chấp thuận.
- Bàn giao đủ 5 US, 8 PAY rule, 2 NFR, **37 AC** gồm 30 mã cũ và 7 mã mới sau R002, cùng điều kiện/ngoại lệ và ví dụ trong decisions.md. Các ví dụ/kiểm chứng đều chưa chạy trên ứng dụng.
- Phần mở rộng đã chốt gồm hồ sơ điều chỉnh riêng, phạm vi sửa/hủy/phân công, kiểm tra nguồn trước trình/duyệt, ngưỡng bao gồm 500 triệu, làm tròn/tổng, phép đo/tải, bàn giao và thử gửi lại.

### 9.2 Phụ thuộc thực hiện, không phải yêu cầu xác nhận lại nghiệp vụ

| Nhóm | Việc cần chuẩn bị hoặc thiết kế | Căn cứ |
| --- | --- | --- |
| Dữ liệu nguồn | Cung cấp cấu hình/lịch sử kỳ, danh sách bắt buộc, giá hiệu lực, sản lượng phân bổ đã đối soát; xử lý giá trùng/thiếu và số lẻ vượt giới hạn | PAY-DEC-004/005/008/009/012 |
| Nhân sự/quyền | Cung cấp NVKD phụ trách, người duyệt chính/thay thế cùng thẩm quyền, đầu mối phân công/vận hành và kế toán nhận bàn giao; quyền xem riêng theo đơn vị | PAY-DEC-007/011/013/014 |
| Thiết kế | Luồng điều chỉnh/vòng đời, audit, chống chồng kỳ/lập đồng thời, cố định căn cứ lần trình, so sánh nguồn, theo dõi thông báo; lựa chọn kỹ thuật chưa chốt | PAY-DEC-001 đến PAY-DEC-014 |
| Bàn giao kế toán | Chuẩn bị quy trình/kênh chia sẻ và bằng chứng tiếp nhận, đối chiếu bảng/lần duyệt/điều chỉnh, phối hợp hóa đơn ngoài module | PAY-DEC-013 |
| Nghiệm thu | Chuẩn bị dữ liệu/môi trường/tải theo hồ sơ đã duyệt, ghi thời gian/nguồn; khảo sát 200 dòng trở lên riêng | PAY-DEC-010 |

Nếu dữ liệu/nhân sự chưa đáp ứng thì áp đúng điều kiện chặn/chờ trong quyết định, không tự nới quy tắc. Yêu cầu hoặc ngoại lệ mới ngoài phạm vi chốt phải có quyết định mới; không ghi đè quyết định đã xác nhận.

### 9.3 Giới hạn xác minh

Lượt PRM-006 là lịch sử ghi nhận phê duyệt đầu tiên. PRM-011 chỉnh sửa BA sau R002, đối chiếu tài liệu và lưu phiên bản theo workflow hiện hành; bằng chứng thực tế tại changes R002. Không có kiểm thử ứng dụng, đo hiệu năng hoặc bàn giao kế toán thực tế trong lượt BA này.

### 9.4 Nội dung Design cần tiếp nhận sau R002

- Dùng PAY-DEC-015 và mục 6.1 làm căn cứ đồng bộ điều kiện tạo điều chỉnh, mở lại/hủy, duyệt và bàn giao; không suy Cancelled giải phóng kỳ gốc hoặc Rejected mất vị trí.
- Bổ sung truy vết/kiểm chứng bốn AC nguồn đổi/không đọc được tại mục 8.3; giữ nguyên chính sách PAY-DEC-012. F003 chỉ đổi nhãn tóm lược nguồn, không tạo quyết định nghiệp vụ.
- Các lựa chọn F001/F002 trong phạm vi R002 đã có căn cứ xác nhận; không còn câu hỏi nghiệp vụ chặn hai phần này. Phụ thuộc dữ liệu/nhân sự/môi trường tại mục 9.2 vẫn phải được đáp ứng. Bộ thiết kế đã có chưa được cập nhật hoặc tái phê duyệt trong lượt BA này; Design xử lý khi được giao.
