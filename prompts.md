# Khai báo dùng AI — Bùi Sỹ Tùng

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (Claude Opus 5.5) | Đọc tài liệu repo (README, GUIDE, RULES, SUBMISSION, RUBRIC, CHECKPOINTS) và tóm tắt lab cần làm gì | Dùng bản tóm tắt để nắm task, lịch và các file nộp | Có, đối chiếu lại với GUIDE.md và SUBMISSION.md |
| 2 | Claude Code (Claude Opus 5.5) | Nhờ AI gán nhãn ảnh và xuất file export | AI **từ chối** vì RULES §3 cấm. Không có nhãn nào do AI tạo, không ảnh nào được đưa cho AI | — |
| 3 | Claude Code (Claude Opus 5.5) | Chuyển thư mục `G07/` (giải nén sai chỗ) vào repo, kiểm tra `.gitignore` chặn ảnh, chạy `cabin_check.py --help` | AI chỉ chuyển file, không mở ảnh. Repo sẵn sàng để chạy script | Có, chạy `git check-ignore` thấy ảnh bị chặn |
| 4 | Claude Code (Claude Opus 5.5) | Soạn thẻ tra nhanh tóm tắt Guideline Cabin v1.0 và lệnh `cabin_check` cho G07; chép các file mẫu ra gốc repo | Mở thẻ cạnh CVAT khi tự gán nhãn | Có, đối chiếu thẻ với GUIDE §4 |
| 5 | Claude Code (Claude Opus 5.5, rồi Sonnet 5.5) | Giải thích cách gán `person`, `face`, `face13`, `hand`, `phone`, ghế và đồ vật theo Guideline Cabin v1.0 (hỏi bằng chữ, không gửi ảnh) | Làm theo hướng dẫn khi tự gán nhãn trên CVAT | Có, đối chiếu với GUIDE §4 |
| 6 | Claude Code (Claude Opus 5.5, rồi Sonnet 5.5) | Chạy `cabin_check check` trên file export (chỉ có toạ độ, không có ảnh) và giải thích lỗi/cảnh báo | Tự sửa nhãn trên CVAT theo từng lỗi (D05 `side`, `light`, `seat`, cờ điểm mắt), rồi export lại. AI không sửa nhãn thay | Có, chạy lại `cabin_check` đến khi 0 lỗi, 0 cảnh báo |
| 7 | Claude Code (Claude Sonnet 5.5) | Chạy `cabin_check diff` giữa export của mình và của bạn review chéo, tạo `disagreement_log.csv` | Hai bạn tự điền `verdict`, `guideline_rule`, `action` | Có, đối chiếu từng dòng với guideline trên CVAT |
| 8 | Claude Code (Claude Sonnet 5.5) | Hỏi nghĩa của `facing` (ghế em bé). Có lỡ gửi 1 ảnh S## (ảnh dựng, không có người thật) | AI chỉ giải thích khái niệm, không kết luận nhãn từ ảnh. Mình tự quyết | Có, đối chiếu GUIDE §4.8 |
