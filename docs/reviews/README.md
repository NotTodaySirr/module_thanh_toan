# Tổ chức review theo module và vòng

Mỗi module có ba hồ sơ BA hiện hành ở `docs/analysis/<module-id>/` và nhiều vòng review ở đây. Reviewer kiểm tra đầu ra phân tích sau khi BA hoàn thành lượt phân tích, trước Design. Các bản phân tích cũ được lấy bằng Git; thư mục review chỉ giữ nhận xét, đánh giá và bằng chứng thay đổi. Thiết kế không nằm trong phạm vi review hiện hành.

```text
docs/reviews/
├── _templates/                  # Biểu mẫu dùng chung
├── payment-management/
│   ├── R001/
│   └── R002/
└── <module-id-khac>/
    └── R001/
```

Các số vòng trên minh họa cấu trúc; kiểm tra thư mục thực tế trước khi chọn vòng mới. Khi bắt đầu, sao chép đủ bốn file trong `_templates/` vào số vòng chưa tồn tại và ghi giai đoạn `Phân tích`. Dùng `.cursor/rules/versioning-review.mdc` làm quy trình chính. Giữ nguyên vòng review thiết kế đã có và baseline lịch sử; không đổi phạm vi của findings cũ.

## Phiên bản và lịch sử

Trong vòng phân tích mới, `baseline_commit` chứa ba hồ sơ BA đã được Reviewer đọc; `revised_commit` chứa phân tích sau khi BA xử lý nhận xét được chấp nhận. Chúng có thể chứa nhiều module hoặc cả thiết kế, nhưng mỗi review phải khai báo ba đường dẫn phân tích cụ thể và nguồn/xác nhận liên quan; không review thiết kế trong cùng commit. Không gán một SHA mới cho toàn bộ các vòng trước. Mốc thiết kế được quản lý riêng, không phải điều kiện mở review phân tích.

Lệnh đối chiếu mẫu dưới đây dùng hai SHA thật; thay các chuỗi trong dấu ngoặc nhọn trước khi chạy. Nếu Git root ở thư mục cha, dùng đường dẫn tương đối từ Git root.

```powershell
git show --stat '{BASELINE_SHA}'
git show --stat '{REVISED_SHA}'
git diff '{BASELINE_SHA}' '{REVISED_SHA}' -- docs/analysis/payment-management
```

`git show` giúp kiểm tra commit và phạm vi file, `git diff` cho thấy nội dung đã thay đổi. Hash của commit bằng chứng lưu báo cáo không cần ghi trong chính báo cáo đó.

Một vòng có thể đã đối chiếu nhưng phân tích vẫn còn câu hỏi nghiệp vụ. Ghi rõ trạng thái và ảnh hưởng khi bàn giao Design, tránh dùng “đã review” để suy ra “đã được phê duyệt”. Nhóm đánh giá findings; BA chỉnh sửa và cập nhật changes.md trước khi bàn giao cho Design.
