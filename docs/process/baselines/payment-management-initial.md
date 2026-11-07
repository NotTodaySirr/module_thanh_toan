# Mốc baseline thiết kế lần đầu - payment-management

| Trường | Giá trị |
| --- | --- |
| Module | `payment-management` |
| Loại mốc | **Thiết kế lần chạy đầu** |
| Trạng thái mốc | Đã lưu commit baseline và tag Git cục bộ |
| Trạng thái thiết kế tại mốc | Design Approved - người dùng phê duyệt tại PRM-008 |
| baseline_commit | `44c586c47105a9107af9673ff56604e83bd07ece` |
| Tag cố định | `payment-management-design-baseline-v1` |
| Tên commit | docs(payment-management): lưu baseline thiết kế lần đầu đã phê duyệt |
| Repository root | `C:/Users/Admin/OneDrive/Documents/HCMUS/Year4-S1/MISD/Template/QTKD-Agent-Seminar/module_thanh_toan` |
| Workspace module / tiền tố trong repository | Repository root cũng là workspace module; không có tiền tố bổ sung |
| Phạm vi thiết kế | `docs/design/payment-management/` tính từ repository root |
| Thời điểm commit theo Git | 07/11/2026 16:21:46 +07:00, tương ứng Asia/Bangkok |
| Người yêu cầu / phê duyệt | Người dùng trong hội thoại; chưa cung cấp tên cá nhân; PRM-008/009 |
| Công cụ thực hiện | Codex / System Design Agent, dùng danh tính Git đã cấu hình sẵn, không thay cấu hình |
| Vòng review | Chưa mở; mốc chuẩn bị cho review lần đầu, không phải review đã thực hiện |
| revised_commit | Chưa có - chưa có thiết kế sửa sau review |
| Push | Chưa thực hiện; commit và tag được lưu cục bộ |

## 1. Nội dung đã lưu tại baseline

- Tám tài liệu thiết kế và README điều hướng, trạng thái đã phê duyệt.
- Sáu nguồn PlantUML và sáu PNG render cục bộ trong `docs/design/payment-management/diagrams/`.
- Ba hồ sơ BA tại `docs/analysis/payment-management/`: requirements-analysis.md, issues.md, decisions.md; gồm 14 PAY-DEC đã được xác nhận.
- PDF gốc tại `docs/requirements/btcohong.pdf`. Git nhận diện di chuyển từ `btcohong.pdf` với nội dung giống 100%; SHA-256 `8514A086A403AC4B1B57EEE4993894230677525FC66D19F7CDA9C827D057E436`.
- Nhật ký prompt và đóng góp tại `docs/process/prompt-log.md`, `docs/process/task-allocation.md`, gồm phê duyệt PRM-008 và yêu cầu lưu mốc PRM-009.

Baseline có 27 đường dẫn thay đổi so với commit hướng dẫn trước đó; không có mã ứng dụng. Các file hướng dẫn có sẵn cũng được giữ trong snapshot Git. Hồ sơ mốc này được lưu bằng commit sau baseline để ghi được SHA thật; commit lưu hồ sơ không là thiết kế sửa sau review và không thay `baseline_commit`.

## 2. Kiểm tra thực tế khi lưu

- Trước stage, index trống; đã đối chiếu phạm vi thay đổi và chỉ stage thiết kế/BA/nguồn/process liên quan.
- `git diff --cached --check` không có lỗi khoảng trắng; kiểm tra **120 liên kết nội bộ** của thiết kế và BA đều có đích trong index, đủ 8 tài liệu/6 PlantUML/6 PNG.
- Đọc PDF từ index và so bản gốc tại HEAD trước commit: giống từng byte, hash không đổi.
- Commit baseline tạo thành công; tag được giải tham chiếu ra đúng SHA đầy đủ nêu trên. Không amend, rebase, reset hoặc push.
- Các kết quả kiểm tra nội dung/UML trước mốc được ghi tại PRM-007/008. Đây không là test ứng dụng, phép đo NFR hoặc review độc lập.

## 3. Dùng mốc cho review

Khi được giao mở R001, ghi nguyên SHA ở trường `baseline_commit` vào review-record.md, kèm phạm vi `docs/design/payment-management/` và nguồn BA/PDF cùng commit. Không lấy HEAD hiện hành thay SHA cố định, không đổi tag hoặc baseline sau khi đã phát hành findings. Chưa tạo bốn hồ sơ vòng review trong nhiệm vụ lưu mốc này.

Các lệnh chỉ đọc để xác minh, chạy tại repository root:

```powershell
git show --stat 44c586c47105a9107af9673ff56604e83bd07ece
git rev-parse 'payment-management-design-baseline-v1^{commit}'
git show 44c586c47105a9107af9673ff56604e83bd07ece:docs/design/payment-management/README.md
```

[Thiết kế hiện hành](../../design/payment-management/README.md) có thể được cập nhật ở các nhiệm vụ sau; bản baseline luôn đọc từ commit cố định. [Nhật ký prompt](../prompt-log.md) lưu căn cứ giao việc và phê duyệt. Hồ sơ review chỉ được mở khi có nhiệm vụ riêng.
