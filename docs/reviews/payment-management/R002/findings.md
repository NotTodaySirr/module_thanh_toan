# Nhận xét của AI Reviewer - R002

- Module: `payment-management`.
- Giai đoạn: **Phân tích - review đầu ra BA**.
- Baseline: `44c586c47105a9107af9673ff56604e83bd07ece`.
- Ngày review: 12/11/2026, Asia/Bangkok; không ghi giờ chưa xác minh.
- Phạm vi: ba hồ sơ tại `docs/analysis/payment-management/`; đối chiếu PDF đủ ba trang và xác nhận PRM-006 tại cùng baseline.
- Trạng thái: **Đã ghi nhận đánh giá của người dùng - chờ BA xử lý**. Kết quả Đúng/Sai/Thừa và phương án xử lý được người dùng trong phiên chấp thuận ngày 12/11/2026; xem mục 4. Chưa có bản BA sửa hoặc revised_commit, chưa đóng vòng.

## 1. Bảng nhận xét và đánh giá của bạn/nhóm

Các ô `[x]` và lý do ở cột cuối ghi nhận đánh giá người dùng đã chấp thuận sau khi đọc đề xuất trong chat. AI hỗ trợ ghi lại, không tự nhận là thành viên nhóm đã đánh giá. Người xác nhận chưa cung cấp tên cá nhân; căn cứ và phạm vi xác nhận tại mục 4.

- **Đúng:** Nhận xét phù hợp với nguồn và bản phân tích được review.
- **Sai:** Nhận xét có kết luận hoặc căn cứ không phù hợp.
- **Không liên quan (Thừa):** Nhận xét ngoài phạm vi, trùng lặp hoặc không cần thiết cho bài tập. Dùng nhãn **Thừa** khi tổng hợp theo yêu cầu bài tập.

### 1.1 Reviewer đánh giá bảng 14 vấn đề của BA

Phần này thể hiện nhận xét của Reviewer về **từng phát hiện của BA**, gồm căn cứ phát hiện và cách xử lý đã được ghi nhận. Bạn/nhóm đánh giá **nhận xét của Reviewer** ở hai cột cuối. Nhận xét đồng tình với BA vẫn là nhận xét để bạn kiểm tra, không bắt buộc phải là lỗi mới.

Mã `payment-management-R002-A001` đến `A014` định danh các nhận xét đối chiếu, không phải 14 lỗi mới hoặc mã vấn đề nghiệp vụ mới. Mỗi dòng liên kết PAY-ISS/PAY-DEC cùng số; toàn bộ nguồn được đọc tại baseline đã khai báo. Cột cách xử lý đã ghi nhận tóm tắt quyết định PRM-006; hai cột cuối ghi đánh giá nhận xét Reviewer theo xác nhận mới tại mục 4. Việc đã đánh giá không có nghĩa BA đã sửa sau R002.

| Mã nhận xét Reviewer | Vấn đề BA / Loại vấn đề | Vị trí trong đặc tả | Nhận xét của AI Reviewer | Cách xử lý đã ghi nhận / Điểm cần theo dõi | Bạn/nhóm đánh giá nhận xét Reviewer | Lý do đánh giá / Cách xử lý sau review |
| --- | --- | --- | --- | --- | --- | --- |
| payment-management-R002-A001 | PAY-ISS-001: Điều chỉnh Approved / Mâu thuẫn | Trang 2 mục 4.4 US-PAY-04 và mục 5 PAY-05 | BA phát hiện có căn cứ: một phát biểu cho điều chỉnh Approved có biên bản, phát biểu kia cấm sửa dưới mọi hình thức. Hồ sơ đã dẫn đủ hai phía; phương án điều chỉnh riêng là lựa chọn đã xác nhận, không phải quy trình gốc PDF. | PAY-DEC-001: giữ bản Approved, điều chỉnh liên kết có biên bản và duyệt riêng. F001 yêu cầu làm rõ thêm phạm vi trạng thái đang xử lý. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng mâu thuẫn giữa US-PAY-04/PAY-05. Giữ điều chỉnh riêng đã xác nhận; BA làm rõ vòng đời theo F001/mục 4.2. |
| payment-management-R002-A002 | PAY-ISS-002: Đúng 500 triệu / Mâu thuẫn tại biên | Trang 2 mục 4.3 US-PAY-03 và mục 5 PAY-06 | BA xác định đúng mâu thuẫn chỉ tại đúng 500 triệu: US dùng từ mức này trở lên, rule dùng trên mức này. Có AC dưới/đúng/trên ngưỡng; không thấy BA tự ưu tiên nguồn sau khi đã có xác nhận. | PAY-DEC-002: dưới 500 triệu giao KTT, từ 500 triệu giao GĐKD; một cấp. Không cần hỏi lại lựa chọn này. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng tại biên 500 triệu. Giữ từ 500 triệu giao GĐKD và AC dưới/đúng/trên ngưỡng; không sửa thêm. |
| payment-management-R002-A003 | PAY-ISS-003: Rejected hay Draft / Không nhất quán trạng thái | Trang 1 mục 3 và trang 2 mục 5 PAY-08 | BA có căn cứ yêu cầu làm rõ: nguồn vừa mô tả Rejected sau từ chối vừa nói quay về Draft, chưa định nghĩa bước nối. Phân tích giữ đủ năm trạng thái và ghi hành động mở lại theo xác nhận. | PAY-DEC-003: từ chối có lý do về Rejected; người phụ trách chủ động mở lại Draft, giữ lịch sử/audit. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng điểm thiếu bước nối. Giữ từ chối về Rejected rồi chủ động mở Draft, có audit; không chọn lại phương án. |
| payment-management-R002-A004 | PAY-ISS-004: Tháng/quý và tính duy nhất / Chưa rõ cách kết hợp quy tắc | Trang 1 mục 2 và trang 2 mục 5 PAY-01 | BA nêu đúng điểm cần làm rõ giữa kỳ quý và giới hạn mỗi tháng. Đây là thiếu cách kết hợp/chống chồng kỳ, chưa đủ căn cứ kết luận phải cấm quý hoặc hai phát biểu không thể cùng được đáp ứng. Bản BA đã giữ giới hạn này. | PAY-DEC-004: kiểm tra mọi tháng của bảng chưa Cancelled; quý bao phủ ba tháng, điều chỉnh không chiếm thêm kỳ. Đã có AC chồng kỳ/lập đồng thời. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng điểm chưa rõ cách kết hợp kỳ, không coi là hai quy tắc không thể cùng đúng. Giữ chống chồng từng tháng; dùng nhãn chưa rõ cách kết hợp quy tắc khi trình bày. |
| payment-management-R002-A005 | PAY-ISS-005: Dịch vụ bắt buộc / Thiếu thông tin | Trang 2 mục 5 PAY-04 | BA phát hiện có căn cứ: nguồn yêu cầu đủ dịch vụ nhưng không cung cấp danh sách/nguồn danh sách. Phân tích phân biệt thiếu dữ liệu với danh sách rỗng và số lượng 0 đã đối soát; không tự thêm chức năng quản lý danh mục. | PAY-DEC-005: lấy từ hợp đồng/phụ lục hiệu lực; thiếu danh sách hoặc thiếu dịch vụ thì chặn trình. Dữ liệu thực tế vẫn là phụ thuộc cần cung cấp. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng khoảng trống nguồn danh sách. Giữ hợp đồng/phụ lục làm nguồn; phân biệt rỗng, thiếu dữ liệu và sản lượng 0 đã đối soát. |
| payment-management-R002-A006 | PAY-ISS-006: Sửa Draft và audit / Mơ hồ | Trang 2 mục 4.1 US-PAY-01; trang 3 mục 6 | BA có căn cứ hỏi phạm vi sửa vì PDF chưa liệt kê trường; việc mở rộng audit sang lưu ghi chú là lựa chọn bổ sung đã xác nhận. Không được diễn giải lệnh cho sửa Draft thành quyền sửa mọi dữ liệu tài chính. | PAY-DEC-006: chỉ sửa ghi chú; số liệu cập nhật chủ động từ nguồn hợp lệ; audit nội dung trước/sau. AC-US-PAY-01-06 và AC-NFR-PAY-01-02 có liên kết. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng phạm vi sửa chưa rõ. Giữ sửa ghi chú/cập nhật tài chính từ nguồn; ghi rõ đây là quyết định bổ sung đã xác nhận. |
| payment-management-R002-A007 | PAY-ISS-007: Quyền hủy/lập thay thế / Thiếu thông tin | Trang 2 mục 4.5 US-PAY-05 và mục 5 PAY-01 | BA phát hiện có căn cứ: PDF không nêu actor hủy hoặc Cancelled còn chiếm kỳ không. Bản phân tích đã tách quyền hủy, không dùng lại bản cũ và điều kiện lập mã mới; không mặc nhiên cấp KTT quyền hủy mọi bảng. | PAY-DEC-007: người phụ trách hủy Draft có lý do; bảng gốc hủy giải phóng kỳ, bản thay thế có mã mới; hủy điều chỉnh không giải phóng kỳ gốc. F001 còn liên quan quyền xử lý tiếp trong chuỗi điều chỉnh. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng khoảng trống actor hủy/lập thay thế. Giữ quy tắc hiện hành; bổ sung xử lý tiếp của điều chỉnh theo F001/mục 4.2. |
| payment-management-R002-A008 | PAY-ISS-008: Thời điểm chọn giá / Mơ hồ | Trang 2 mục 5 PAY-01 và PAY-03 | BA nêu đúng điểm chưa rõ về giá phù hợp/thời điểm tính và đổi giá giữa kỳ. Giá theo ngày dịch vụ là phương án đã xác nhận, không phải ý nghĩa duy nhất có thể suy ra từ PDF. Có xử lý giá thiếu/trùng và sản lượng không đủ phân bổ. | PAY-DEC-008: giá theo ngày dịch vụ, tách dòng theo hiệu lực; không tự chọn giá mới nhất hoặc chia đều sản lượng. Nguồn phải cung cấp phân bổ đã đối soát. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng điểm mơ hồ về giá. Giữ giá theo ngày dịch vụ/tách dòng; thiếu phân bổ thì yêu cầu nguồn bổ sung, không tự chia đều. |
| payment-management-R002-A009 | PAY-ISS-009: Làm tròn/tổng tiền / Thiếu thông tin | Trang 2 mục 4.1, 4.3; mục 5 PAY-03/04/06 | BA phát hiện có căn cứ vì PDF không quy định độ chính xác hoặc làm tròn. Hồ sơ giữ rõ 6 số lẻ, HALF_UP và kiểm tra tổng trước làm tròn là bổ sung đã xác nhận; các ví dụ tổng 0/âm nhỏ và ngưỡng duyệt nhất quán khi tính lại. | PAY-DEC-009: làm tròn từng dòng đến đồng rồi cộng; tổng chính thức dùng thống nhất; chặn cả tổng chính xác âm. Không thêm thuế/phí ngoài công thức. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng khoảng trống làm tròn. Giữ quy ước/tổng chính thức đã xác nhận; không nhận thông số bổ sung là yêu cầu nguyên gốc. |
| payment-management-R002-A010 | PAY-ISS-010: Phép đo 3 giây / Mơ hồ về phép đo và biên | Trang 3 mục 6 | BA phát hiện có căn cứ: PDF thiếu điểm đầu/cuối phép đo, điều kiện tải và cách đếm dòng; đúng 200 dòng chưa thuộc ngưỡng được nêu. Hồ sơ phân biệt kế hoạch đã chốt với kết quả thực đo, không tuyên bố ứng dụng đạt NFR. | PAY-DEC-010: đo đến Draft đã lưu/hiển thị, gồm mạng và nguồn; hồ sơ 1/199 dòng, tải 1/10 người, 30 lượt/người. Đúng/trên 200 dòng khảo sát riêng. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng thiếu điều kiện đo và biên 200 dòng. Giữ kế hoạch đã xác nhận; chỉ ghi đạt khi thực sự đo, không sửa thêm ngưỡng. |
| payment-management-R002-A011 | PAY-ISS-011: Phân công/phạm vi quyền / Thiếu thông tin | Trang 1 mục 2; trang 2 mục 4.1-4.3, PAY-06/07 | BA có căn cứ nêu thiếu người nhận cụ thể, người thay thế và phạm vi bảng được xử lý. Cấm tự duyệt là yêu cầu gốc; cấm tự từ chối, phân công theo đơn vị và quy trình thay người là phần bổ sung đã được xác nhận, không phải tất cả đều có sẵn trong PAY-07. | PAY-DEC-011: giao người đúng vai trò, độc lập người lập; thiếu người thì chặn trình; thay người có audit, người cũ hết quyền. Không suy quyền sửa từ quyền xem. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng thiếu phân công/thay thế. Giữ cơ chế hiện hành; phân biệt cấm tự duyệt từ PDF với cấm tự từ chối đã được bổ sung, xác nhận. |
| payment-management-R002-A012 | PAY-ISS-012: Nguồn đổi sau Draft / Thiếu thông tin | Trang 2 mục 4.1, 4.2, 4.4; mục 5 PAY-01/02/03 | BA phát hiện có căn cứ: nguồn không chốt chính sách kiểm tra lại/giữ căn cứ khi dữ liệu đổi. Phân tích đã ghi nhận quyết định và luồng ngoại lệ; tuy nhiên bộ AC chưa thể hiện đủ nhánh nguồn đổi/không đọc được, như F002. | PAY-DEC-012: không đổi tiền ngầm; kiểm tra lại trước trình/duyệt, chặn hoặc chờ đúng trường hợp; điều chỉnh kỳ cũ có ngoại lệ Active được xác nhận. Cần bổ sung bao phủ AC theo F002 nếu nhóm chấp nhận. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng: đã có quy tắc nhưng thiếu AC ngoại lệ. Giữ PAY-DEC-012; BA bổ sung AC theo F002/mục 4.3. |
| payment-management-R002-A013 | PAY-ISS-013: Bàn giao kế toán / Thiếu thông tin tại ranh giới | Trang 1 mục 1; trang 2 mục 4.3 | BA có căn cứ hỏi trách nhiệm, dữ liệu và xác nhận tiếp nhận: PDF chỉ chốt phải duyệt trước chuyển kế toán. Bàn giao thủ công được xác nhận giữ đúng ranh giới; không cần biến khoảng trống này thành yêu cầu API/xuất file hoặc quản lý hóa đơn. | PAY-DEC-013: KTT bàn giao ngoài module, có bằng chứng gửi/nhận, chống tính tiền hai lần. F001 còn nêu điểm chưa rõ về thời gian dừng bàn giao khi điều chỉnh bị từ chối/hủy. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng khoảng trống bàn giao. Giữ bàn giao thủ công ngoài module; bổ sung dừng/tiếp tục theo F001/mục 4.2. |
| payment-management-R002-A014 | PAY-ISS-014: Lỗi thông báo/trình lại / Thiếu luồng ngoại lệ | Trang 2 mục 4.2 US-PAY-02 và mục 5 PAY-04 | BA phát hiện có căn cứ: nguồn chưa mô tả gửi lỗi sau khi đã ghi nhận trình. Giữ PendingApproval và giới hạn 3 lần/15 phút là chính sách đã xác nhận; PDF không bắt buộc outbox hoặc cam kết thông báo đúng một lần. | PAY-DEC-014: gửi lại cùng lần trình, theo dõi lỗi riêng, dừng thông báo lỗi thời; không tạo thêm vòng duyệt khi bấm trình lặp. AC-US-PAY-02-05 đã đề cập chống trùng/lỗi thông báo. | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Đúng thiếu ngoại lệ gửi lỗi. Giữ PendingApproval/gửi lại cùng tham chiếu; 3 lần/15 phút là bổ sung đã xác nhận. |

Căn cứ BA: [issues.md](../../../analysis/payment-management/issues.md) mục 2-4 và 6; [decisions.md](../../../analysis/payment-management/decisions.md) mục 1 và 4; [requirements-analysis.md](../../../analysis/payment-management/requirements-analysis.md) mục 4-9; [prompt-log.md](../../../process/prompt-log.md) PRM-005/006 tại baseline. Nội dung nhận xét AI giữ nguyên; đánh giá của người dùng được ghi riêng ở hai cột cuối và mục 4.

### 1.2 Ba điểm cần cải thiện trong hồ sơ phân tích

Ba finding F001-F003 dưới đây giữ nguyên ID và nội dung đã phát hành. Chúng là các điểm cần xử lý được phát hiện qua review toàn bộ hồ sơ BA, không thay thế 14 nhận xét đối chiếu ở mục 1.1.

| Finding ID | Nhận xét của AI (tóm tắt) | Mức độ | Đánh giá của bạn/nhóm | Lý do đánh giá / Cách nhóm xử lý |
| --- | --- | --- | --- | --- |
| payment-management-R002-F001 | Chưa rõ điều chỉnh Rejected/Cancelled còn chặn điều chỉnh tiếp theo hoặc bàn giao kế toán hay không. | Trung bình | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Chấp nhận có giới hạn: thiếu định nghĩa trạng thái, chưa chứng minh hệ thống chắc chắn bị kẹt/tạo trùng. Chấp thuận bảng trạng thái mục 4.2; chờ BA cập nhật. |
| payment-management-R002-F002 | Thiếu AC cho ngoại lệ nguồn thay đổi hoặc không đọc được, dù quy tắc đã được mô tả. | Trung bình | [x] Đúng; [ ] Sai; [ ] Không liên quan (Thừa) | Chấp nhận thiếu bao phủ AC, không phải thiếu toàn bộ quy tắc nghiệp vụ. Bổ sung AC/biến thể mục 4.3, giữ PAY-DEC-012; chờ BA cập nhật. |
| payment-management-R002-F003 | Bảng ghi giữ nguyên phát biểu nguồn nhưng một số dòng thực tế đã diễn đạt rút gọn. | Thấp | [ ] Đúng; [ ] Sai; [x] Không liên quan (Thừa) | Thừa trong phạm vi bài tập vì chưa đổi nghĩa nghiệp vụ. Không bắt buộc sửa riêng; tùy chọn đổi nhãn Tóm lược phát biểu nguồn khi biên tập. |

Đánh giá **Đúng** chưa đồng nghĩa đã sửa xong. Trong lượt xác nhận này, người dùng chấp thuận cả phân loại và phương án xử lý đã đề xuất, gồm bảng trạng thái F001 tại mục 4.2. Căn cứ được lưu ngay trong file này; [team-evaluation.md](team-evaluation.md) dẫn về kết quả tại đây, BA đối chiếu thay đổi tại [changes.md](changes.md).

### Bằng chứng và khuyến nghị của Reviewer

Số dòng trong bảng và phần chi tiết được tính từ nội dung `git show <baseline>:<path>`, bắt đầu từ 1. Các liên kết dẫn tới file hiện hành để điều hướng; nội dung ba hồ sơ BA hiện hành trùng baseline sau chuẩn hóa LF/CRLF.

| Finding ID | Loại | Mức độ | Vị trí tại baseline | Căn cứ PDF/US/rule/AC | Vấn đề và bằng chứng | Tác động | Khuyến nghị | Mã vấn đề đặc tả nếu có |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| payment-management-R002-F001 | Thiếu thông tin nguồn | Trung bình | `decisions.md`:57-59, 69-70, 132; `requirements-analysis.md`:164-167, 230 | PDF trang 2 mục 4.4 US-PAY-04; PAY-DEC-001/003/007/013; PRM-006 | Quy tắc một điều chỉnh "đang xử lý" và dừng bàn giao khi có điều chỉnh "chưa duyệt" chưa định nghĩa tập trạng thái. Rejected có thể mở lại, còn Cancelled không có hiệu lực nhưng vẫn là hồ sơ chưa Approved. Chưa có kết quả rõ cho việc lập điều chỉnh kế tiếp và tiếp tục bàn giao sau các nhánh này. | Có thể cho tồn tại hai hồ sơ cùng có khả năng mở lại hoặc giữ dừng bàn giao theo một hồ sơ đã hủy. Đây là khoảng trống về điều kiện chuyển tiếp, không phải bằng chứng BA chọn sai phương án điều chỉnh riêng. | Bổ sung bảng theo từng trạng thái: còn giữ quyền xử lý độc quyền của chuỗi không, được mở điều chỉnh khác không, còn dừng bàn giao không. Dùng căn cứ đã xác nhận để diễn giải; phần chưa quyết định phải ghi câu hỏi bổ sung và ảnh hưởng, không tự chọn kết quả. | Liên quan PAY-ISS-001/003/007/013; chưa cấp mã PAY-ISS mới thay BA |
| payment-management-R002-F002 | Sai/thiếu phân tích | Trung bình | `requirements-analysis.md`:135-139, 152-159, 273; đối chiếu `decisions.md`:123-125 | PDF trang 2 mục 4.1-4.3 và PAY-01/02/03; PAY-DEC-012; US-PAY-02/03 | Luồng văn bản đã mô tả nguồn đổi/không đọc được, nhưng AC và truy vết hiện có chỉ phủ nhánh nguồn hợp lệ hoặc không đổi. Không có AC xác định kết quả khác nhau khi nguồn đổi ảnh hưởng trước trình, đổi trước duyệt và không đọc được trước duyệt. | Nếu kiểm chứng theo danh mục 30 AC, có thể bỏ sót việc không đổi tiền ngầm, giữ Draft khi chặn trình và giữ PendingApproval khi không đọc được nguồn; đủ số AC không chứng minh các nhánh này được bao phủ. | Bổ sung AC hoặc nhóm biến thể Cho trước - Khi - Thì có định danh/truy vết cho các ngoại lệ đã chốt, giữ nguyên mã AC hiện có. Không yêu cầu chạy ứng dụng hoặc xác nhận lại PAY-DEC-012. | PAY-ISS-012 đã có quyết định; finding về bao phủ AC, không mở lại lựa chọn nghiệp vụ |
| payment-management-R002-F003 | Không nhất quán | Thấp | `requirements-analysis.md`:192, 196-203 | PDF trang 2 mục 5, PAY-01 đến PAY-08; chỉ mục nguồn S09 | Lời dẫn nói cột phát biểu nguồn "giữ nguyên" PDF, nhưng một số dòng là diễn đạt rút gọn. Ví dụ PAY-08 thay phần "sau khi bị từ chối, bảng quay về trạng thái Draft" bằng "bảng về Draft"; PAY-02 cũng rút gọn phát biểu nguồn. | Không phát hiện thay đổi nghĩa nghiệp vụ từ các ví dụ này, nhưng nhãn giữ nguyên khiến người đọc hiểu đây là bản chép nguyên văn để làm bằng chứng. | Chép đúng phát biểu PDF vào cột nguồn, hoặc đổi nhãn thành "Tóm lược phát biểu nguồn". Giữ riêng cột diễn giải đã xác nhận và không sửa PDF. | Không cần quyết định nghiệp vụ mới |

## 2. Chi tiết và cách kiểm tra sau xử lý

### payment-management-R002-F001 - Định nghĩa trạng thái giữ điều chỉnh và dừng bàn giao

Căn cứ: [decisions.md](../../../analysis/payment-management/decisions.md) dòng 57 quy định một chuỗi chỉ có một hồ sơ điều chỉnh đang xử lý; dòng 59 nói hồ sơ bị từ chối/hủy không đổi giá trị đã duyệt. Dòng 69-70 vẫn cho Rejected mở lại Draft. Dòng 132 yêu cầu tạm dừng bàn giao mới khi có điều chỉnh chưa duyệt. [issues.md](../../../analysis/payment-management/issues.md) mục 6.2/6.4/6.8/6.14 cũng chưa định nghĩa rõ tập trạng thái tương ứng; không tìm thấy xác nhận bổ sung giải quyết ranh giới này trong PRM-006.

Tình huống cần làm rõ: bảng A đã Approved; điều chỉnh B bị Rejected. B có tiếp tục giữ vị trí điều chỉnh duy nhất để mở lại, hay được tạo C? Nếu tạo C thì B còn được mở lại không? Khi B đã mở Draft rồi hủy thành Cancelled, điều kiện dừng bàn giao của A kết thúc theo mốc nào? Cụm "không đổi giá trị đã duyệt" xác định số tiền, chưa tự xác định các quyền và điều kiện bàn giao này.

Reviewer không chọn tập trạng thái thay nghiệp vụ. BA cần xác định phần có thể diễn giải chắc chắn từ các quyết định hiện có, phần còn thiếu cần câu trả lời mới. Không thay đổi các lựa chọn đã chốt: giữ Approved bất biến, dùng hồ sơ liên kết, không cộng lặp tổng và không giải phóng kỳ gốc khi hủy điều chỉnh.

Kiểm tra sau xử lý: đối chiếu cùng một bảng quy tắc cho Draft/PendingApproval/Rejected/Approved/Cancelled của hồ sơ điều chỉnh; nêu được kết quả lập điều chỉnh tiếp, mở lại và bàn giao tại từng nhánh. Nếu chưa có đủ căn cứ, ghi rõ câu hỏi và tác động bị chặn thay vì bịa expected result. Không yêu cầu UML/API/schema ở bước này.

### payment-management-R002-F002 - Bổ sung tiêu chí cho ngoại lệ nguồn đã được chốt

Căn cứ: [requirements-analysis.md](../../../analysis/payment-management/requirements-analysis.md) dòng 130 và 144 đã mô tả ngoại lệ đúng hướng theo PAY-DEC-012. Tuy nhiên AC-US-PAY-02-01 giả định nguồn kiểm tra lại không đổi ảnh hưởng; AC-US-PAY-03-01/02 giả định nguồn hợp lệ; AC-US-PAY-03-03 xét phân công tại ngưỡng. Dòng 273 liên kết mốc kiểm tra trước duyệt với AC-US-PAY-03-01 đến 03, chưa có nhánh kiểm chứng các kết quả bị chặn/chờ. Các AC còn lại của hai US không bổ sung những kết quả này.

Các trường hợp cần diễn đạt bằng AC hoặc biến thể được định danh, dựa đúng quyết định sẵn có:

1. Draft 100 triệu; nguồn đổi có ảnh hưởng thành 110 triệu; trình phải bị chặn, giữ Draft và số tiền đã lưu 100 triệu đến khi người phụ trách xem/chấp nhận cập nhật; không tự cập nhật rồi trình ngay.
2. PendingApproval; nguồn đổi ảnh hưởng hoặc không còn hợp lệ; chặn duyệt và thông báo người được giao; không tự sửa nội dung lần trình hoặc tự từ chối. Người được giao từ chối có lý do theo luồng hiện hành để xử lý lại.
3. PendingApproval; nguồn không đọc được; giữ PendingApproval chờ kiểm tra, không tự Approved/Rejected. Trước trình không đọc được nguồn thì chặn và giữ Draft.

Đây là thiếu bao phủ AC/truy vết cho nghiệp vụ đã mô tả, không phải kết luận thiếu toàn bộ yêu cầu kiểm tra nguồn. Dòng 104 cho biết mọi US áp dụng toàn văn quyết định; nhận xét không phủ nhận quy tắc đó. Khuyến nghị nhằm đưa các kết quả có khác biệt đáng kể vào bộ tiêu chí kiểm chứng, thay vì chỉ viện dẫn quyết định chung. Không cần một AC riêng cho từng câu trong mọi PAY-DEC.

Kiểm tra sau xử lý: từng nhánh có thể lần theo PAY-DEC-012 → US → AC/biến thể → trạng thái và số tiền mong đợi; không còn dùng riêng AC của nguồn hợp lệ để chứng minh nhánh lỗi. Đây là kiểm tra tài liệu, chưa phải kiểm thử ứng dụng.

### payment-management-R002-F003 - Phân biệt trích nguyên văn với tóm lược

Căn cứ: lời dẫn mục 5 của [requirements-analysis.md](../../../analysis/payment-management/requirements-analysis.md) và PDF trang 2 mục 5. Kiểm tra trực tiếp trang PDF cho thấy các phát biểu giữ ý chính nhưng đã rút gọn. Tình trạng này cũng giải thích vì sao không nên dùng lời "nguyên văn 8 PAY rule" trong nhật ký PRM-006 làm kết quả kiểm chứng mới của Reviewer.

Kiểm tra sau xử lý: nếu giữ nhãn nguyên văn/giữ nguyên, so từng dòng với PDF, cho phép chuẩn hóa xuống dòng và khoảng trắng do trình bày; nếu dùng tóm lược, ghi rõ nhãn và nguồn trang/mục. Không sửa diễn giải nghiệp vụ đã xác nhận chỉ để đồng nhất với nguồn còn mâu thuẫn.

## 3. Kết quả tích cực và giới hạn

- Có đủ 5 US, 8 PAY rule, 2 NFR, 27 AC theo US và 3 AC theo NFR; 30 mã AC duy nhất, đều có Cho trước - Khi - Thì.
- Bốn mâu thuẫn gốc đều được nêu đủ các phát biểu liên quan và liên kết PAY-DEC-001 đến 004. PRM-006 tại baseline ghi prompt xác nhận của người dùng và phạm vi 14 phương án; Reviewer dùng căn cứ đã lưu, không hỏi lại hoặc gán xác nhận cho KTT/GĐKD.
- Kiểm tra ranh giới 499.999.999 / 500.000.000 / 500.000.001 đồng, tổng âm/0, trình trùng, không tự duyệt/từ chối, lý do từ chối, hủy, sản lượng chưa đối soát; không phát hiện việc tự chọn lại bốn phương án gốc hoặc áp công nghệ từ template thành ràng buộc nghiệp vụ.
- Giữ yêu cầu audit và 3 giây với dưới 200 dòng; trường hợp đúng/trên 200 dòng không bị áp thêm ngưỡng. Kế hoạch đo đã được phân biệt với kết quả chạy thật.
- Kiểm 33 liên kết nội bộ trong ba hồ sơ BA: đều có đích file tại baseline; bảng Markdown nhất quán số cột. Kiểm đích file chưa chứng minh đúng mọi nội dung liên kết.
- F001 là điểm cần làm rõ tiếp; F002 là thiếu bao phủ AC; F003 là độ chính xác nhãn nguồn. Không có finding mức Cao hoặc Chặn quyết định nghiệp vụ trong lượt này; kết quả không bảo đảm tuyệt đối không còn lỗi.
- Không review hoặc sửa thiết kế, mã ứng dụng, toàn bộ quy trình hay đóng góp nhóm. Không chạy ứng dụng, đo hiệu năng, gửi thông báo hoặc bàn giao kế toán thực tế.

Chi tiết phiên bản/công cụ và giới hạn tại [review-record.md](review-record.md). Đánh giá người dùng hiện tại nằm ở bảng mục 1 và mục 4; BA cập nhật [changes.md](changes.md) sau xử lý được chấp nhận. Reviewer chưa sửa hồ sơ BA hoặc tự phê duyệt nghiệp vụ.

## 4. Đánh giá và phương án xử lý được người dùng xác nhận

### 4.1 Căn cứ và phạm vi xác nhận

- Người xác nhận: người dùng trong hội thoại; chưa cung cấp tên cá nhân. Không gán xác nhận này cho KTT, GĐKD hoặc thành viên khác.
- Ngày: 12/11/2026, Asia/Bangkok theo ngữ cảnh phiên; chưa ghi nhận giờ chính xác.
- Đầu vào: đề xuất trong chat ngay trước lượt xác nhận, gồm đánh giá 14 nhận xét A001-A014, ba finding F001-F003, bảng trạng thái điều chỉnh cho F001 và các trường hợp AC cho F002.
- Prompt xác nhận và yêu cầu ghi lại nguyên văn:

```text
ok the proposal looks fine. Help me to write that down the findings file
```

- Phạm vi chấp thuận: A001-A014 là **Đúng**; F001 là **Đúng, chấp nhận có giới hạn**; F002 là **Đúng**; F003 là **Thừa**. Tổng 16 Đúng, 0 Sai, 1 Thừa. Chấp thuận cả hướng xử lý và bảng trạng thái đề xuất, không chỉ nhãn phân loại.
- AI thực hiện việc ghi nhận theo xác nhận có thật. Đây không còn là đề xuất chưa xác nhận; chưa phải bằng chứng BA đã cập nhật, nhóm đã tự sửa file hoặc ứng dụng đã được kiểm thử.

### 4.2 F001 - Chấp nhận có giới hạn và làm rõ vòng đời điều chỉnh

Chấp nhận nhận xét về việc chưa định nghĩa rõ các trạng thái đang xử lý/chưa duyệt; không coi đây là bằng chứng hệ thống chắc chắn bị kẹt hoặc tạo trùng. Người dùng chấp thuận phương án chi tiết sau để BA cập nhật:

| Trạng thái hồ sơ điều chỉnh | Có được tạo điều chỉnh khác cùng chuỗi? | Bàn giao mới của chuỗi |
| --- | --- | --- |
| Draft | Không | Tạm dừng |
| PendingApproval | Không | Tạm dừng |
| Rejected | Không; mở lại chính hồ sơ này để sửa hoặc hủy | Tạm dừng |
| Approved | Có, dựa trên bản Approved hiện hành và khi không có hồ sơ khác đang xử lý | Được tiếp tục theo căn cứ mới đã duyệt, nếu không có điều chỉnh khác đang xử lý |
| Cancelled | Có, khi không có hồ sơ khác đang xử lý và vẫn đủ điều kiện lập điều chỉnh | Được tiếp tục theo bản Approved hiện hành, nếu không có điều chỉnh khác đang xử lý |

Draft/PendingApproval/Rejected giữ vị trí điều chỉnh duy nhất của chuỗi. Khi không tiếp tục một hồ sơ Rejected, KTT phụ trách mở lại Draft rồi hủy đúng quyền trước khi tạo hồ sơ khác. Approved/Cancelled kết thúc việc giữ vị trí của chính hồ sơ đó. Cancelled không khôi phục, không đổi giá trị đã duyệt hoặc giải phóng kỳ gốc.

BA cần ghi nhận xác nhận bổ sung này trong sổ quyết định với liên kết F001, rồi đồng bộ điều kiện lập điều chỉnh, mở lại/hủy và tạm dừng/tiếp tục bàn giao trong ba hồ sơ phân tích. Giữ nguyên quyết định cũ và chỉ rõ nội dung được bổ sung; không tự báo đã cập nhật khi mới ghi trong hồ sơ review.

### 4.3 F002 - Chấp nhận bổ sung AC cho quy tắc đã có

Giữ PAY-DEC-012; bổ sung AC hoặc biến thể có định danh và truy vết, không thay mã AC đã có:

| Trường hợp | Cho trước | Khi | Thì |
| --- | --- | --- | --- |
| Nguồn đổi trước trình | Draft đang lưu 100 triệu; nguồn liên quan đổi có ảnh hưởng thành 110 triệu | Người phụ trách trình | Chặn trình, giữ Draft và 100 triệu đến khi người phụ trách xem/chấp nhận cập nhật; không tự đổi tiền rồi trình ngay |
| Nguồn đổi trước duyệt | PendingApproval; nguồn liên quan đổi ảnh hưởng hoặc không còn hợp lệ | Người được giao duyệt | Chặn duyệt, giữ nội dung lần trình và thông báo người được giao; người đó từ chối có lý do để về quy trình sửa, không tự sửa tiền hoặc tự từ chối |
| Không đọc được nguồn trước trình | Draft, không đọc được nguồn cần kiểm tra | Người phụ trách trình | Chặn trình và giữ Draft |
| Không đọc được nguồn trước duyệt | PendingApproval, không đọc được nguồn cần kiểm tra | Người được giao duyệt | Giữ PendingApproval chờ kiểm tra, không tự Approved hoặc Rejected |

BA bổ sung mapping PAY-DEC-012 → US-PAY-02/03 → AC/biến thể → kết quả mong đợi. Đây là hoàn thiện tiêu chí kiểm chứng, không thêm chính sách nghiệp vụ mới hoặc yêu cầu chạy test ứng dụng trong lượt ghi nhận này.

### 4.4 F003 - Thừa, không bắt buộc sửa riêng

Nhận xét về nhãn nguồn có cơ sở nhưng các ví dụ chưa làm thay đổi nghĩa nghiệp vụ; người dùng chấp thuận xếp **Thừa** trong phạm vi bài tập. Không sửa quy tắc nghiệp vụ và không bắt buộc mở công việc riêng. Khi biên tập tài liệu, có thể đổi nhẹ nhãn thành **Tóm lược phát biểu nguồn**; đây là tùy chọn, chưa được thực hiện trong lượt này.

### 4.5 Trạng thái xử lý sau xác nhận

A001-A014 giữ các quyết định hiện có; A001/A007/A013 theo dõi bổ sung F001, A012 theo dõi F002. Chờ BA cập nhật phần được chấp nhận và ghi mapping/kết quả kiểm tra trong changes.md; revised_commit chưa có. Không đánh dấu các sửa đổi là hoàn tất hoặc đóng vòng chỉ vì đã có đánh giá và phương án được chấp thuận.
