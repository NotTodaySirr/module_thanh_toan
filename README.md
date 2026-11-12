# Bộ hướng dẫn thiết kế module bảng thanh toán bằng AI Agent

Workspace: thư mục chứa file này. Mọi đường dẫn bên dưới được tính từ workspace này.
Mở `module_thanh_toan/` làm thư mục gốc trong công cụ để tránh nạp nhầm rules của dự án mẫu bên cạnh.

## 1. Mục tiêu và nguồn

Chuyển đặc tả `btcohong.pdf` thành thiết kế có căn cứ, đánh giá kết quả AI và chứng minh thay đổi sau review. Bộ hướng dẫn viết bằng tiếng Việt, kế thừa phương pháp BA, HLD/LLD, UML và truy vết của template QTKD.

Workflow hiện hành: **BA → Reviewer phân tích → nhóm đánh giá → BA chỉnh sửa → Design**. Reviewer chỉ kiểm tra đầu ra phân tích của BA; không review thiết kế hoặc toàn bộ quy trình. Commit khởi tạo hướng dẫn không thay thế baseline phân tích. Các mốc thiết kế được lưu riêng khi làm sản phẩm bài tập.

Hồ sơ review thiết kế và baseline đã có là bằng chứng lịch sử theo workflow cũ, không đổi tên, ghi đè hoặc diễn giải lại thành review phân tích. Áp dụng workflow mới cho lượt tiếp theo, dùng số vòng chưa tồn tại trong module.

## 2. Cấu trúc và phân loại

```text
module_thanh_toan/
├── AGENTS.md                              # Điểm vào cho công cụ đọc AGENTS.md
├── README.md                              # Cách sử dụng và trình tự làm bài
├── .cursor/
│   ├── rules/
│   │   ├── general-design.mdc             # Nguyên tắc dùng chung
│   │   ├── payment-context.mdc            # Ngữ cảnh riêng của bài thanh toán
│   │   └── versioning-review.mdc          # Git và quy trình review dùng chung
│   └── agents/
│       ├── ba.md                          # Phân tích yêu cầu
│       ├── design.md                      # Thiết kế kiến trúc và chi tiết
│       └── reviewer.md                    # Review đầu ra BA trước Design
└── docs/
    ├── requirements/btcohong.pdf          # Đặc tả gốc, không sửa
    ├── analysis/payment-management/       # Đầu ra BA được Reviewer kiểm tra
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

> Đọc AGENTS.md và .cursor/agents/ba.md. Phân tích module payment-management từ docs/requirements/btcohong.pdf. Cập nhật ba hồ sơ phân tích, nêu mâu thuẫn và câu hỏi kèm lý do; chưa chọn phương án nghiệp vụ khi chưa có xác nhận. Tiếp tục các phần không phụ thuộc câu trả lời. Bàn giao đầu ra BA cho Reviewer trước Design.

Ví dụ prompt Reviewer, sau khi BA hoàn thành lượt phân tích và đã có baseline commit thật:

> Đọc .cursor/agents/reviewer.md. Review đầu ra phân tích BA của payment-management tại docs/analysis/payment-management/, vòng [Rxxx mới chưa tồn tại], baseline commit [SHA đầy đủ]. Đối chiếu requirements-analysis.md, issues.md, decisions.md với PDF và xác nhận liên quan. Không review thiết kế, không sửa hồ sơ BA và không tự điền đánh giá của nhóm.

Ví dụ prompt BA chỉnh sửa sau review:

> Cập nhật phân tích payment-management theo các nhận xét đã được nhóm đánh giá trong [Rxxx]. Tuân thủ quyết định nghiệp vụ đã xác nhận; giữ các vấn đề chưa có quyết định ở trạng thái Cần xác nhận. Cập nhật changes.md và truy vết để bàn giao Design.

Ví dụ prompt Design, sau khi đã xử lý review phân tích:

> Đọc .cursor/agents/design.md, bản phân tích payment-management sau chỉnh sửa và hồ sơ review [Rxxx]. Tạo bộ thiết kế, truy vết đầy đủ; phần chưa xác nhận trình bày phương án và ảnh hưởng, không biến giả định thành quy tắc chính thức.

## 4. Trình tự và quản lý phiên bản

1. BA đọc đủ ba trang PDF, hoàn thành requirements-analysis.md, issues.md, decisions.md. Ghi đúng người và căn cứ các xác nhận đã có; phần chưa rõ giữ trạng thái và câu hỏi, không cần chờ tất cả câu trả lời để review.
2. Lưu baseline phân tích bằng Git trước khi Reviewer làm việc. `baseline_commit` là SHA đầy đủ của commit chứa ba đầu ra BA cùng nguồn/xác nhận liên quan, không cần có thiết kế.
3. Tạo vòng mới `docs/reviews/payment-management/Rxxx/` từ bốn biểu mẫu, ghi giai đoạn `Phân tích`. Reviewer chỉ kiểm tra đầu ra BA và ghi findings; nhóm đánh giá đúng/sai/thừa và hướng xử lý.
4. BA sửa theo nhận xét đã được chấp nhận và quyết định đã có. Commit phân tích đã sửa, ghi SHA vào `revised_commit`, cập nhật changes.md và lưu hồ sơ review bằng commit tiếp theo. Cách tách này tránh yêu cầu một commit phải chứa chính SHA của nó.
5. Đối chiếu các thay đổi, bàn giao bản phân tích và điểm còn mở cho Design. Design tạo/cập nhật bộ thiết kế dựa trên phân tích sau review; lưu mốc thiết kế riêng khi được giao.
6. Các vòng phân tích tiếp theo dùng cùng cơ chế. Không tạo lại hay ghi đè vòng cũ, kể cả vòng từng review thiết kế. Mỗi module có chuỗi số vòng riêng.

Sử dụng repository hiện có. Nếu tái sử dụng bộ hướng dẫn ở workspace khác chưa thuộc repository, khởi tạo Git tại workspace đó hoặc dùng repository cha đã được nhóm chọn. Không khởi tạo repository lồng nhau khi đã có repository cha. Không tự cấu hình danh tính Git thay thành viên.

Ví dụ lệnh PowerShell, chạy từ workspace sau khi đã tạo tài liệu tương ứng:

```powershell
git status
git add -- docs/analysis/payment-management docs/requirements docs/process
git commit -m "docs(payment-management): lưu baseline phân tích BA trước review"
git rev-parse HEAD
```

Lưu SHA vừa trả về vào `review-record.md` của vòng mới. Sau khi BA sửa phân tích theo review:

```powershell
git add -- docs/analysis/payment-management
git commit -m "docs(payment-management): cập nhật phân tích BA theo review"
git rev-parse HEAD
# Ghi SHA vào revised_commit, hoàn thiện hồ sơ review và nhật ký trước khi lưu tiếp.
# Thay Rxxx bằng số vòng mới đã được giao.
git add -- docs/reviews/payment-management/Rxxx docs/process
git commit -m "docs(payment-management): lưu bằng chứng review phân tích"
```

Các lệnh trên lưu hai thời điểm phân tích và hồ sơ giải thích; chúng không tự tạo tài liệu hay xác nhận nghiệp vụ. Chỉ lưu baseline khi đã có ba hồ sơ BA thật, không dùng riêng README hoặc biểu mẫu trống. Trước mỗi commit, xem diff và chỉ stage các file đúng nhiệm vụ. Không dùng reset/rebase/amend để thay lịch sử đã dùng làm bằng chứng. Nếu Git không dùng được, ghi rõ bước lưu phiên bản chưa hoàn tất. Mốc thiết kế đầu/sau chỉnh sửa theo yêu cầu bài tập lưu riêng; review phân tích không tự chứng minh đã có hai phiên bản thiết kế.

## 5. Đối chiếu sản phẩm bài tập, trang 3

| Yêu cầu | Nơi lưu và cách chứng minh |
| --- | --- |
| 1. Workspace agent, rules, thiết kế | `.cursor/` và `docs/design/payment-management/` |
| 2. Báo cáo vấn đề và đánh giá Reviewer | `docs/analysis/payment-management/issues.md`, `docs/reviews/payment-management/Rxxx/` |
| 3. Thiết kế lần đầu và sau review | Lưu mốc thiết kế và diff riêng trong `docs/process/baselines/`; `baseline_commit`/`revised_commit` trong vòng mới chỉ chứng minh thay đổi phân tích BA. Không tự tuyên bố hai mốc phân tích đáp ứng yêu cầu phiên bản thiết kế |
| 4. Nhật ký prompt và phân công | `docs/process/prompt-log.md`, `task-allocation.md` |
| 5. Tối đa 5 slide | Nội dung theo `docs/process/presentation-outline.md`; phải tạo slide thực tế khi làm sản phẩm cuối |

Ưu tiên phát hiện đúng vấn đề và không suy diễn (30%), chất lượng thiết kế (25%), review và cải thiện (15%). Chuẩn bị vấn đáp cá nhân (15%), giải thích agent/rules và phần nhóm sửa (10%), lưu nhật ký và minh chứng (5%). Không tuyên bố hoàn thành bài tập chỉ vì đã tạo bộ hướng dẫn.

## 6. Những thay đổi so với template

Giữ phương pháp HLD/LLD, PlantUML, API/data design, ma trận truy vết và bàn giao. Thay nguồn QTKD bằng PDF bài này; bỏ yêu cầu mặc định xuất Excel, đường dẫn `src/Qtkd.Api/` và phụ thuộc vào `docs/business-rules.md` không tồn tại. Không kế thừa .NET/PostgreSQL, e-sign hay workflow của dự án cũ như yêu cầu bắt buộc. Bổ sung Reviewer, sổ vấn đề, xác nhận nghiệp vụ, review theo module/vòng, mốc Git và nhật ký đóng góp. Chi tiết nguồn gốc được ghi trong `docs/process/instruction-changes.md`.
