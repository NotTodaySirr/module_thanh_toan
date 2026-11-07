# Hướng dẫn cho agent trong workspace

Đọc theo thứ tự trước khi làm việc:

1. `README.md` để xác định workflow và cấu trúc.
2. `.cursor/rules/general-design.mdc`.
3. `.cursor/rules/payment-context.mdc` cho module `payment-management`.
4. `.cursor/rules/versioning-review.mdc`.
5. File vai trò trong `.cursor/agents/` tương ứng với nhiệm vụ.

Các file `.md` và `.mdc` trên cũng là tài liệu hướng dẫn khi công cụ không tự nạp cấu hình Cursor. Tôn trọng yêu cầu và xác nhận của người dùng trong phiên làm việc; ghi lại xác nhận liên quan trước khi áp dụng, không yêu cầu xác nhận lại một quyết định đã có căn cứ.

Mặc định hiện tại là module `payment-management`, nguồn `btcohong.pdf`. Chỉ tạo/cập nhật tài liệu phục vụ nhiệm vụ; không triển khai ứng dụng khi đang phân tích, thiết kế hoặc review. Không sửa PDF nguồn hoặc dự án mẫu bên cạnh.

Khi tạo code minh họa theo yêu cầu, giải thích ngắn cách hoạt động và logic để người học hiểu. Mã minh họa không phải mã đã triển khai hay kiểm thử.
