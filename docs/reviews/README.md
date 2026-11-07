# Tổ chức review theo module và vòng

Mỗi module có bộ thiết kế hiện hành ở `docs/design/<module-id>/` và nhiều vòng review ở đây. Các bản thiết kế cũ được lấy bằng Git; thư mục review chỉ giữ nhận xét, đánh giá và bằng chứng thay đổi.

```text
docs/reviews/
├── _templates/                  # Biểu mẫu dùng chung
├── payment-management/
│   ├── R001/
│   └── R002/
└── <module-id-khac>/
    └── R001/
```

Các thư mục vòng trên chỉ là ví dụ, chưa được tạo để tránh nhầm với review đã diễn ra. Khi bắt đầu, sao chép đủ bốn file trong `_templates/` vào vòng mới. Dùng `.cursor/rules/versioning-review.mdc` làm quy trình chính.

## Phiên bản và lịch sử

`baseline_commit` chứa bộ thiết kế đã được Reviewer đọc; `revised_commit` chứa bộ thiết kế sau khi xử lý. Chúng có thể chứa nhiều module, nhưng mỗi review phải khai báo đường dẫn module cụ thể. Không gán một SHA mới cho toàn bộ các vòng trước.

Lệnh đối chiếu mẫu dưới đây dùng hai SHA thật; thay các chuỗi trong dấu ngoặc nhọn trước khi chạy. Nếu Git root ở thư mục cha, dùng đường dẫn tương đối từ Git root.

```powershell
git show --stat '{BASELINE_SHA}'
git show --stat '{REVISED_SHA}'
git diff '{BASELINE_SHA}' '{REVISED_SHA}' -- docs/design/payment-management
```

`git show` giúp kiểm tra commit và phạm vi file, `git diff` cho thấy nội dung đã thay đổi. Hash của commit bằng chứng lưu báo cáo không cần ghi trong chính báo cáo đó.

Một vòng có thể đã đối chiếu nhưng thiết kế vẫn còn câu hỏi nghiệp vụ. Ghi rõ cả hai trạng thái, tránh dùng “đã review” để suy ra “đã được phê duyệt”.
