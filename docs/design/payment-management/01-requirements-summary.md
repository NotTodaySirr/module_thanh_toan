# 01. Tóm tắt yêu cầu thiết kế

- Module: `payment-management`.
- Trạng thái: **Design Approved - Người dùng đã phê duyệt** - thiết kế lần đầu; chưa review độc lập.
- Phê duyệt: người dùng trong phiên, 07/11/2026 (Asia/Bangkok), prompt “ok approve”; căn cứ PRM-008 trong [nhật ký prompt](../../process/prompt-log.md).
- Ngày: 07/11/2026, Asia/Bangkok. Người soạn: Codex, vai trò System Design Agent.
- Nguồn: [PDF gốc](../../requirements/btcohong.pdf), [phân tích BA](../../analysis/payment-management/requirements-analysis.md), [14 quyết định đã xác nhận](../../analysis/payment-management/decisions.md), PRM-006.
- Vòng review: chưa mở; phải lưu baseline Git chứa thiết kế thật trước review.

## 1. Phạm vi và cách hiểu căn cứ

Module lập bảng, sửa Draft trong phạm vi cho phép, trình, duyệt/từ chối, mở lại, hủy và lập hồ sơ điều chỉnh. Hợp đồng, bảng giá, sản lượng là nguồn chỉ đọc. Hóa đơn, công nợ, đối soát nguồn, ký điện tử và tự động bàn giao kế toán nằm ngoài phạm vi (PDF trang 1 mục 1; PAY-DEC-013).

Nguồn S01-S11 giữ nguyên định nghĩa trong phân tích BA: S01-S03 ở trang 1; S04-S09 ở trang 2; S10-S11 ở trang 3. US/PAY là mã PDF; AC/NFR/S là mã BA. Endpoint, kiểu dữ liệu, lớp, bảng, khóa và cơ chế giao dịch trong bộ này đều là **Đề xuất kỹ thuật**, không phải phát biểu nguyên gốc PDF. Các PAY-DEC là **Đã xác nhận** theo PRM-006; không yêu cầu xác nhận lại.

## 2. Tác nhân

| Tác nhân | Trách nhiệm và giới hạn |
| --- | --- |
| NVKD phụ trách/được bàn giao | Lập bảng gốc; sửa ghi chú, chấp nhận cập nhật nguồn, trình, mở lại Rejected, hủy Draft đúng phân công; không nhập tay số tài chính |
| KTT | Duyệt/từ chối khi tổng dưới 500 triệu và được giao; lập và xử lý Draft điều chỉnh mình phụ trách; bàn giao kế toán thủ công |
| GĐKD | Duyệt/từ chối khi tổng từ 500 triệu và được giao |
| Đầu mối phân công | Bàn giao người phụ trách, thay người duyệt cùng vai trò, có căn cứ và audit; không đổi người lập ban đầu |
| Đầu mối vận hành | Theo dõi thông báo lỗi, gửi lại thủ công cùng lần trình |
| Kế toán tiếp nhận | Đối chiếu Approved và bằng chứng gửi/nhận bên ngoài module; không mặc định là KTT |

PAY-07/PAY-DEC-011 yêu cầu đồng thời kiểm tra vai trò, đơn vị, phân công hiện hành và danh tính: loại người lập gốc cùng người lập điều chỉnh khỏi cả duyệt lẫn từ chối. Quyền xem do hệ thống phân quyền cung cấp riêng; quyền xem không cấp quyền sửa.

## 3. Yêu cầu chức năng và quy tắc

| US; nguồn | Hành vi đã chốt | AC BA |
| --- | --- | --- |
| US-PAY-01; S04, trang 2 mục 4.1 | Lập Draft từ nguồn hợp lệ, tách dòng theo giá, tính tiền; sửa ghi chú hoặc chủ động cập nhật toàn bộ căn cứ | AC-US-PAY-01-01 đến AC-US-PAY-01-08 |
| US-PAY-02; S05, trang 2 mục 4.2 | Kiểm tra lại nguồn, tổng/dịch vụ/người duyệt; cố định lần trình, chuyển PendingApproval; chống trùng và theo dõi thông báo riêng | AC-US-PAY-02-01 đến AC-US-PAY-02-05 |
| US-PAY-03; S06, trang 2 mục 4.3 | Một cấp duyệt theo ngưỡng; kiểm tra nguồn trước duyệt; từ chối có lý do vào Rejected; mở lại Draft là thao tác riêng | AC-US-PAY-03-01 đến AC-US-PAY-03-08 |
| US-PAY-04; S07, trang 2 mục 4.4 | KTT lập hồ sơ điều chỉnh đầy đủ dòng/tổng, biên bản, duyệt riêng; giữ nguyên Approved cũ | AC-US-PAY-04-01 đến AC-US-PAY-04-03 |
| US-PAY-05; S08, trang 2 mục 4.5 | Phụ trách hủy Draft có lý do; Cancelled không phục hồi; lập thay thế mã mới và kiểm tra nguồn/kỳ lại | AC-US-PAY-05-01 đến AC-US-PAY-05-03 |

| Rule; PDF trang 2 mục 5 (S09) | Áp dụng với xác nhận nghiệp vụ |
| --- | --- |
| PAY-01 | Active và giá hợp lệ; chiếm từng tháng của tháng/quý cho mọi bảng gốc chưa Cancelled. PAY-DEC-004/007/008/012; điều chỉnh kỳ cũ có ngoại lệ Active có điều kiện |
| PAY-02 | Sản lượng đã đối soát, đủ phân bổ theo thời gian; kiểm tra lại trước trình/duyệt. PAY-DEC-008/012 |
| PAY-03 | Sản lượng × giá theo ngày dịch vụ; tối đa 6 số lẻ đầu vào, HALF_UP từng dòng đến đồng rồi cộng. PAY-DEC-008/009 |
| PAY-04 | Chặn tổng chính thức âm hoặc tổng tích chưa làm tròn âm; không cấm 0; đủ dịch vụ bắt buộc; chỉ một lần trình. PAY-DEC-005/009/014 |
| PAY-05 | Không sửa nội dung Approved; hồ sơ điều chỉnh liên kết được duyệt mới thay giá trị hiện hành. PAY-DEC-001 |
| PAY-06 | Dưới 500.000.000 VND giao KTT; từ 500.000.000 giao GĐKD; một cấp theo tổng chính thức. PAY-DEC-002/009/011 |
| PAY-07 | Cấm cả người lập gốc/người lập điều chỉnh tự duyệt hoặc từ chối, kể cả đúng vai trò. PAY-DEC-001/011 |
| PAY-08 | Từ chối có lý do vào Rejected; phụ trách chủ động mở lại Draft trước khi sửa/trình. PAY-DEC-003/011 |

Mâu thuẫn PDF giữ trong BA, áp dụng quyết định đã chốt: US-PAY-04 với PAY-05 → PAY-DEC-001; đúng 500 triệu ở US-PAY-03 với PAY-06 → PAY-DEC-002; Rejected với PAY-08 → PAY-DEC-003; tháng/quý với một bảng/tháng → PAY-DEC-004. Không xóa trạng thái hoặc dùng cấu hình để mở lại lựa chọn đã chốt.

## 4. NFR và điều kiện nghiệm thu

| NFR; nguồn | Thiết kế và kiểm chứng dự kiến |
| --- | --- |
| NFR-PAY-01; S07/S10, trang 2 mục 4.4 và trang 3 mục 6 | Giao dịch thay đổi ghi audit ai/lúc nào/trước-sau; bao gồm sửa Draft, cập nhật nguồn, mở lại, chuyển trạng thái, phân công và điều chỉnh. AC-NFR-PAY-01-01/02 |
| NFR-PAY-02; S10, trang 3 mục 6; PAY-DEC-010 | Từ bấm Lập đến hiển thị Draft đã lưu, gồm nguồn/mạng. Bộ 1/199 dòng trước tách giá, cả 199 sinh trên 199 dòng; tải 1/10 người × 30 lượt/người/hợp đồng-kỳ chưa có bảng. Mọi lượt thành công ≤3 giây; lỗi/timeout không đạt. AC-NFR-PAY-02-01 |

200 dòng trở lên vẫn được lập nếu hợp lệ, nhưng chỉ khảo sát hiệu năng; không tự đặt SLA mới. Chưa có ứng dụng hoặc kết quả đo để kết luận đạt NFR.

## 5. Quyết định, phụ thuộc và trạng thái

PAY-DEC-001 đến PAY-DEC-014 đều đã xác nhận. Thiết kế áp dụng thêm các nội dung: dịch vụ bắt buộc từ hợp đồng (005); sửa ghi chú và audit Draft (006); hủy/lập lại (007); giá theo ngày (008); VND/làm tròn (009); phép đo (010); phân công độc lập (011); kiểm tra nguồn (012); bàn giao thủ công (013); thử gửi lại (014). Toàn văn có hiệu lực tại sổ xác nhận, không chỉ bảng tóm tắt này.

Không còn lựa chọn nghiệp vụ nào trong 14 vấn đề cần hỏi lại. Các phụ thuộc triển khai được theo dõi riêng ở [bàn giao](08-handoff-to-developer.md): hợp đồng dữ liệu nguồn nhất quán, danh sách người dùng/quyền, kho biên bản và môi trường đo. Thiếu đầu vào thì chặn/chờ đúng nghiệp vụ; không nới quy tắc. Các quyết định kỹ thuật trong [ADR](07-design-decisions.md) được người dùng phê duyệt cùng bộ thiết kế tại PRM-008.
