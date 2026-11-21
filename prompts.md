# Khai báo dùng AI — Trần Trọng Thái

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Cursor (AI assistant trong IDE) | Hỏi ý nghĩa thuộc tính `gaze_zone` trong guideline Cabin v1.0: có những giá trị nào, gán theo hướng mắt hay hướng đầu, ảnh D## xoay 90° thì xử lý thế nào | Đọc lại mục §4.5 của `GUIDE.md` và đối chiếu với `labels_cabin_v1.json`; dùng làm căn cứ khi điền thuộc tính `face` cho ảnh D01–D06 | Có — đối chiếu trực tiếp với `GUIDE.md` §4.1 và §4.5 và với danh sách giá trị trong `labels_cabin_v1.json` |
| 2 | Cursor (AI assistant trong IDE) | Nhờ đọc và giải thích `scripts/cabin_check.py`: script kiểm những quy tắc nào, lệnh `check` và `diff` khác nhau thế nào, cảnh báo nào là false-positive trên ảnh xoay 90° | Dùng để hiểu ý nghĩa từng dòng `[ERROR]` / `[WARN]` trong `check_cpA.txt`, `check_cpB.txt`, `check_final.txt`; biết cảnh báo nào phải tự xét thay vì sửa theo script | Có — đọc mã nguồn `cabin_check.py` và đối chiếu với `CHECKPOINTS.md`, `COMMON_ERRORS.md` |
| 3 | Cursor (AI assistant trong IDE) | Nhờ đọc kết quả `cabin_check diff` và cho biết bất đồng nào thuộc về quy tắc ghế theo xe (§4.4-3), bất đồng nào là lỗi thuộc tính rõ ràng (modality, light), bất đồng nào cần hỏi coach | Dùng để điền 3 cột `verdict` / `guideline_rule` / `action` cho `disagreement_log.csv`; các dòng `ambiguous` em giữ lại để hỏi coach, không tự quyết | Có — tự đối chiếu từng dòng với `GUIDE.md` §4.1–§4.9 và với toạ độ box trong hai file export (`export_final.zip` và `peer_export_final.zip`) |
| 4 | Cursor (AI assistant trong IDE) | Nhờ viết bản nháp `Report.md` trình bày xung đột guideline: `child_seat` có `occupied` (có giá trị `yes` theo bản guideline mới) nhưng `infant_seat` không có, trong khi trẻ sơ sinh lại nằm trong nôi ở `S03`, `S05` | Lấy làm khung báo cáo, sau đó em tự viết lại nội dung và tự kiểm lại mọi trích dẫn §4.1, §4.2, §4.8, §4.9 với file `GUIDE.md` | Có — mở `GUIDE.md` và đối chiếu từng mục §4.1, §4.2, §4.8, §4.9 và với `labels_cabin_v1.json` |

## Ghi chú

- Không có lần nào em đưa ảnh cabin, ảnh tài xế hay ảnh chụp màn hình cho công cụ AI, đúng theo `RULES.md` §2 và §3.
- AI chỉ được dùng để hỏi về khái niệm, đọc tài liệu và đọc mã nguồn script trong repo; mọi quyết định gán nhãn và mọi trích dẫn guideline đều do em tự kiểm lại trên file trong repo.
