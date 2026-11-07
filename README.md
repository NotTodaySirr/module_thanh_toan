# Bộ hướng dẫn thiết kế module bảng thanh toán bằng AI Agent

Workspace: thư mục chứa file này. Mọi đường dẫn bên dưới được tính từ workspace này.
Mở `module_thanh_toan/` làm thư mục gốc trong công cụ để tránh nạp nhầm rules của dự án mẫu bên cạnh.

## 1. Mục tiêu và nguồn

Chuyển đặc tả `btcohong.pdf` thành thiết kế có căn cứ, đánh giá kết quả AI và chứng minh thay đổi sau review. Bộ hướng dẫn viết bằng tiếng Việt, kế thừa phương pháp BA, HLD/LLD, UML và truy vết của template QTKD.

Đây là bộ hướng dẫn và biểu mẫu, chưa phải bộ thiết kế hoàn chỉnh hay kết quả review thực tế. Workspace đã có repository Git khi kiểm tra cuối lượt thiết lập. Commit khởi tạo hướng dẫn không thay thế baseline thiết kế lần đầu; các mốc thiết kế trước/sau review phải được lưu trong quá trình làm bài.

## 2. Cấu trúc và phân loại

```text
module_thanh_toan/
├── AGENTS.md                              # Điểm vào cho công cụ đọc AGENTS.md
├── README.md                              # Cách sử dụng và trình tự làm bài
├── btcohong.pdf                           # Đặc tả gốc, không sửa
├── .cursor/
│   ├── rules/
│   │   ├── general-design.mdc             # Nguyên tắc dùng chung
│   │   ├── payment-context.mdc            # Ngữ cảnh riêng của bài thanh toán
│   │   └── versioning-review.mdc          # Git và quy trình review dùng chung
│   └── agents/
│       ├── ba.md                          # Phân tích yêu cầu
│       ├── design.md                      # Thiết kế kiến trúc và chi tiết
│       └── reviewer.md                    # Đánh giá độc lập thiết kế
└── docs/
    ├── analysis/payment-management/       # Vấn đề và xác nhận nghiệp vụ
    ├── design/payment-management/         # Bộ thiết kế hiện hành
    ├── reviews/
    │   ├── README.md                      # Cách tổ chức các vòng review
    │   ├── _templates/                    # Biểu mẫu, chưa phải review thật
    │   └── <module-id>/R001/              # Chỉ tạo khi bắt đầu vòng review
    └── process/                           # Prompt, phân công, nguồn gốc chỉnh sửa
```

Các phần dùng chung có thể tái sử dụng cho module mới. Khi mở rộng, bổ sung ngữ cảnh nguồn, `docs/analysis/<module-id>/`, `docs/design/<module-id>/` và `docs/reviews/<module-id>/`; không đổi ý nghĩa mã `PAY-*` để dùng cho module khác. Agent phải được chỉ rõ module và nguồn đang xử lý. Nếu có nhiều module mà yêu cầu không chỉ rõ, cần xác định phạm vi trước khi sửa tài liệu.

## 3. Cách dùng agent

Trong công cụ hỗ trợ định dạng `.cursor`, sử dụng các rules và agent tương ứng. Nếu công cụ không tự nạp những file này, đính kèm hoặc yêu cầu đọc chúng rõ ràng; không mặc định agent của công cụ khác tự được cấu hình. Xem `AGENTS.md` để biết thứ tự đọc. Các vai trò được chạy tuần tự theo nhiệm vụ; không tự sinh một lượt Reviewer rồi coi đó là quyết định của nhóm.

Ví dụ prompt BA:

> Đọc AGENTS.md và .cursor/agents/ba.md. Phân tích module payment-management từ btcohong.pdf. Cập nhật hồ sơ phân tích, nêu mâu thuẫn và câu hỏi kèm lý do; chưa chọn phương án nghiệp vụ khi chưa có xác nhận. Tiếp tục các phần không phụ thuộc câu trả lời.

Ví dụ prompt Design:

> Đọc .cursor/agents/design.md và hồ sơ phân tích payment-management. Tạo bộ thiết kế lần đầu, truy vết đầy đủ; phần chưa xác nhận trình bày phương án và ảnh hưởng, không biến giả định thành quy tắc chính thức.

Ví dụ prompt Reviewer, sau khi đã có baseline commit thật:

> Đọc .cursor/agents/reviewer.md. Review module payment-management, vòng R001, baseline commit [SHA đầy đủ]. Ghi nhận phát hiện có căn cứ; không sửa bộ thiết kế và không tự điền đánh giá của nhóm.

Ví dụ prompt chỉnh sửa:

> Cập nhật thiết kế payment-management theo các nhận xét đã được nhóm đánh giá trong R001. Tuân thủ quyết định nghiệp vụ đã xác nhận; giữ các vấn đề chưa có quyết định ở trạng thái Cần xác nhận. Cập nhật changes.md và truy vết.

## 4. Trình tự và quản lý phiên bản

1. BA đọc đủ ba trang PDF, cập nhật phân tích, vấn đề và câu hỏi. Nhóm xác nhận các lựa chọn nghiệp vụ cần quyết định; ghi đúng người và căn cứ xác nhận.
2. Design tạo bộ thiết kế hiện hành. Lần đầu có thể còn điểm mở nhưng phải thể hiện rõ; không sửa lại để che lỗi trước khi lưu baseline review.
3. Lưu phiên bản lần đầu bằng Git trước khi Reviewer làm việc. Mỗi review có `baseline_commit` là SHA đầy đủ của commit đã chứa thiết kế được review.
4. Tạo hồ sơ `docs/reviews/payment-management/R001/` từ bốn biểu mẫu. Reviewer ghi findings; nhóm đánh giá đúng/sai/thừa và hướng xử lý.
5. Design sửa theo nhận xét đã được chấp nhận và quyết định đã có. Lưu bằng một commit chứa thiết kế đã sửa. Ghi SHA đó vào `revised_commit`, cập nhật báo cáo và lưu hồ sơ review bằng commit tiếp theo. Cách tách này tránh yêu cầu một commit phải chứa chính SHA của nó.
6. R002, R003... tiếp tục cùng cơ chế. Không tạo lại hay ghi đè R001. Mỗi module có chuỗi số vòng riêng.

Sử dụng repository hiện có. Nếu tái sử dụng bộ hướng dẫn ở workspace khác chưa thuộc repository, khởi tạo Git tại workspace đó hoặc dùng repository cha đã được nhóm chọn. Không khởi tạo repository lồng nhau khi đã có repository cha. Không tự cấu hình danh tính Git thay thành viên.

Ví dụ lệnh PowerShell, chạy từ workspace sau khi đã tạo tài liệu tương ứng:

```powershell
git status
git add -- AGENTS.md README.md .cursor btcohong.pdf docs/analysis docs/design docs/process
git commit -m "docs(payment-management): lưu thiết kế ban đầu trước review R001"
git rev-parse HEAD
```

Lưu SHA vừa trả về vào `review-record.md`. Sau khi sửa thiết kế theo review:

```powershell
git add -- docs/design/payment-management docs/analysis/payment-management
git commit -m "docs(payment-management): cập nhật thiết kế theo review R001"
git rev-parse HEAD
# Ghi SHA vào revised_commit, hoàn thiện hồ sơ review và nhật ký trước khi lưu tiếp.
git add -- docs/reviews/payment-management/R001 docs/process
git commit -m "docs(payment-management): lưu bằng chứng review R001"
```

Các lệnh trên lưu hai thời điểm thiết kế và hồ sơ giải thích; chúng không tự tạo tài liệu hay xác nhận nghiệp vụ. Chỉ lưu baseline thiết kế sau khi đã có các tài liệu thiết kế thật, không dùng riêng README hoặc biểu mẫu trống. Trước mỗi commit, xem diff và chỉ stage các file đúng nhiệm vụ. Không dùng lệnh reset/rebase/amend để thay lịch sử đã dùng làm bằng chứng. Nếu Git không dùng được, ghi rõ bước lưu phiên bản chưa hoàn tất; không tuyên bố đã đáp ứng yêu cầu 3. Bộ hướng dẫn này chọn phương án lịch sử Git được đề cho phép.

## 5. Đối chiếu sản phẩm bài tập, trang 3

| Yêu cầu | Nơi lưu và cách chứng minh |
| --- | --- |
| 1. Workspace agent, rules, thiết kế | `.cursor/` và `docs/design/payment-management/` |
| 2. Báo cáo vấn đề và đánh giá Reviewer | `docs/analysis/payment-management/issues.md`, `docs/reviews/payment-management/Rxxx/` |
| 3. Thiết kế lần đầu và sau review | Hai commit thật, SHA trong `review-record.md`, diff trong `changes.md` |
| 4. Nhật ký prompt và phân công | `docs/process/prompt-log.md`, `task-allocation.md` |
| 5. Tối đa 5 slide | Nội dung theo `docs/process/presentation-outline.md`; phải tạo slide thực tế khi làm sản phẩm cuối |

Ưu tiên phát hiện đúng vấn đề và không suy diễn (30%), chất lượng thiết kế (25%), review và cải thiện (15%). Chuẩn bị vấn đáp cá nhân (15%), giải thích agent/rules và phần nhóm sửa (10%), lưu nhật ký và minh chứng (5%). Không tuyên bố hoàn thành bài tập chỉ vì đã tạo bộ hướng dẫn.

## 6. Những thay đổi so với template

Giữ phương pháp HLD/LLD, PlantUML, API/data design, ma trận truy vết và bàn giao. Thay nguồn QTKD bằng PDF bài này; bỏ yêu cầu mặc định xuất Excel, đường dẫn `src/Qtkd.Api/` và phụ thuộc vào `docs/business-rules.md` không tồn tại. Không kế thừa .NET/PostgreSQL, e-sign hay workflow của dự án cũ như yêu cầu bắt buộc. Bổ sung Reviewer, sổ vấn đề, xác nhận nghiệp vụ, review theo module/vòng, mốc Git và nhật ký đóng góp. Chi tiết nguồn gốc được ghi trong `docs/process/instruction-changes.md`.
