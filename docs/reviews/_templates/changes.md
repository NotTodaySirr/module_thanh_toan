# Đối chiếu phân tích BA sau review

Module: [module-id]. Vòng: [Rxxx]. Hai SHA được lưu trong `review-record.md` sau khi đã có commit thật.
Giai đoạn: Phân tích. BA cập nhật báo cáo này sau xử lý các nhận xét đã được chấp nhận, trước bàn giao Design.

## 1. Mapping nhận xét và thay đổi

| Finding ID | Quyết định từ team-evaluation.md | Quyết định nghiệp vụ liên quan | File/mục thay đổi | Trước → Sau | Kiểm tra thực tế và kết quả | Trạng thái/điểm còn mở |
| --- | --- | --- | --- | --- | --- | --- |

Ghi cả nhận xét không sửa và lý do. Nếu chỉ ghi nhận mâu thuẫn chứ chưa giải quyết, thể hiện đúng; không báo đã sửa hết. Mọi thay đổi ngoài findings phải ghi căn cứ nhiệm vụ riêng và tác động.

## 2. Bằng chứng Git

[Lệnh diff dùng SHA thật và đường dẫn đúng module; tóm tắt kết quả. Chưa chạy thì ghi chưa chạy.]

## 3. Tính nhất quán và giới hạn

[Nguồn ↔ US/rule/NFR ↔ AC ↔ vấn đề/quyết định trong ba hồ sơ BA; câu hỏi nghiệp vụ còn mở, phần bàn giao được cho Design và phần phụ thuộc xác nhận. Không yêu cầu UML/API/schema ở bước này.]

Đây là báo cáo thay đổi, không phải chứng nhận hiệu năng hay triển khai thành công của ứng dụng.
