# Danh sách lớp v0 — Trần Trọng Thái · G07

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**
Nguồn định nghĩa lớp và giá trị thuộc tính: `labels_cabin_v1.json` (Guideline Cabin v1.0).

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face.eye_left` / `face.eye_right` | open / closed / unknown | Phát hiện buồn ngủ (mắt nhắm lâu) và mất tập trung; `unknown` để không dạy model đoán bừa khi mắt bị che | Có, trừ khi kính phản xạ hoặc đốm LED; ở D02 và D04 vùng mắt phải bị che nên phải ghi `unknown` |
| 2 | `face.glasses` | unknown / none / clear / sun | Biết tài xế có đeo kính để chọn camera phù hợp (kính râm cần lọc IR) và điều chỉnh độ nhạy detector mắt | Có nhưng kính râm trên NIR có thể trông trong suốt; GUIDE §4.5 yêu cầu chọn `unknown` thay vì `none` khi không rõ |
| 3 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Cảnh báo tài xế nhìn đi đâu: nhìn gương thay vì đường, nhìn xuống điện thoại, nhìn màn hình giữa | Có, nhưng hướng mắt trên ảnh NIR xoay 90° phải suy từ hướng đầu; `other` khi không chắc |
| 4 | `face13` (13 điểm mặt + cờ `occluded` / `outside`) | đủ 13 điểm, mỗi điểm có cờ | Lấy vector đặc trưng khuôn mặt chuẩn hoá để so khớp người lái; cờ cần thiết vì điểm bị che không đặt "cho đủ" | Có, nhưng nhiều điểm bị che (miệng ở D03, mắt ở D02/D04) nên cờ `occluded` / `outside` là bắt buộc |
| 5 | `person.seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo không thắt dây an toàn — lớp cần học được cả dây đeo sai (qua nách, sau lưng) | Không chắc: băng thắt tối hoặc chìm trong bóng sẽ không thấy trên NIR, phải ghi `unknown` chứ không đoán |
| 6 | `hand.on_wheel` | yes / no / unknown | Biết tài xế có đặt tay lên vô lăng; `no` khi xe đang tự lái là bình thường, không phải lỗi | Có, nhưng vô lăng nằm ngoài khung thì `unknown` |
| 7 | `hand.side` / `hand.whose` | left / right; driver / passenger | Tách tay trái-phải của chính người để đo hành vi (tay với vô lăng, tay cầm điện thoại) và nhận biết tay hành khách | Có, nhưng phải theo người chứ không theo bên trái ảnh (ảnh D## xoay 90°) |
| 8 | `phone.state` | held / mounted / lap | Phân biệt đang cầm điện thoại (bị cấm khi lái) với điện thoại gắn cố định (hợp lệ) | Có, nhưng chỉ vẽ khi thấy cạnh, góc hoặc màn hình; tay áp tai che kín vật tối thì không vẽ |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `infant_seat` (+ `facing`) | rear / front | Nhắc quên trẻ trong xe; `facing` quyết định có được tắt túi khí ghế trước hay không | Có (hình dạng nôi), nhưng em bé bên trong nôi thường không thấy trên ảnh SVIRO |
| 2 | `child_seat` (+ `occupied`) | unknown / no (guideline mới có `yes`) | Biết ghế trẻ em có trẻ ngồi vào hay không, để cảnh báo trẻ không thắt dây | Có (hình dạng ghế); trạng thái có trẻ hay không phụ thuộc tư thế, khó phân biệt trên ảnh tối |
| 3 | `object.type` | bag / box / cup / other | Kiểm kê vật mang theo, phát hiện vật cứng có thể thành vật nguy hiểm trong va chạm | Có (hình dạng), nhưng vật nhỏ ở hàng ghế sau chỉ còn khối sáng trên ảnh IR |
| 4 | `person.seat` (theo xe) | rear_left / rear_middle / rear_right | Định vị người ngồi để gán dây an toàn đúng bên và cảnh báo ghế trống có người | Có, nhưng phải nhớ camera OMS nhìn về phía sau nên trái ảnh là phải xe |
| 5 | `person.seatbelt` | not_worn / unknown (SVIRO) | Trên bộ SVIRO không ai thắt dây nên chỉ ghi `not_worn` hoặc `unknown`; ở bộ khác dùng để cảnh báo thiếu dây | Trên ảnh dựng SVIRO: dây tối hoàn toàn không thấy, chỉ ghi `unknown` |
| 6 | `face` (chỉ box, không thuộc tính ở OMS) | box trần đến cằm, tai đến tai | Với người lớn trên ghế sau, cần biết có mặt người để đếm số người và gắn vùng mặt cho các model khác | Có (hình dạng), nhưng ở ảnh tối thường chỉ thấy vùng sáng, phải chỉnh brightness trong CVAT rồi vẽ |
| 7 | `frame_meta.modality` / `light` / `camera_blocked` | rgb / ir; day / night / unknown; yes / no | Gắn nhãn chất lượng khung hình để model biết ảnh nào là IR, ảnh nào bị camera che; cũng để không gán nhầm thuộc tính màu lên ảnh hồng ngoại | Là chính tag này giúp tách ảnh RGB và ảnh IR trong tập dữ liệu |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Với ghế `child_seat` có trẻ ngồi vào thì ghi `occupied = yes`; nhưng với nôi `infant_seat` có em bé thì lại không có thuộc tính `occupied`. Có nên thêm `occupied` cho `infant_seat` không, hay để trẻ sơ sinh trong nôi chỉ được mô tả bằng class riêng?
   Khung ví dụ: `S03_rgb`, `S03_ir`, `S05_rgb`, `S05_ir` (bộ G07) — em bé nằm trong nôi ở giữa hàng ghế sau; `S01_rgb`, `S01_ir`, `S08_rgb`, `S08_ir` — ghế trẻ em có trẻ ngồi vào.
   Bạn tạm gán thế nào và vì sao: Em giữ nguyên schema hiện tại — `infant_seat` chỉ có `seat` và `facing`, không gán `occupied`; `child_seat` thì điền `occupied = yes` theo guideline mới. Việc này làm script `cabin_check` báo 4 lỗi, em đã ghi lại đầy đủ trong `Report.md` và nhờ coach phân xử (hướng A: thêm `occupied` cho `infant_seat`).

2. Câu hỏi: Ở ảnh DMS, khi cả 4 điểm `right_eye_*` đều bị đánh dấu `occluded` (do đốm phản xạ LED che mắt), thì `eye_right` nên là `unknown` hay vẫn cố đoán `open`/`closed`? Tương tự, khi mặt bị che gần hết thì có nên vẽ box `face` không?
   Khung ví dụ: `D02.png` (4 điểm `right_eye_*` đều `occluded`), `D04.png` (mắt phải bị che, mắt trái cũng không phân biệt được), `D03.png` (miệng bị che nên 4 điểm miệng phải đặt `outside`).
   Bạn tạm gán thế nào và vì sao: Ở `D02` em để `eye_right = unknown` và bật cờ `occluded` cho 4 điểm mắt phải; ở `D03` em đặt `outside` cho 4 điểm miệng. Guideline §4.4.1 nói "không thấy thì `unknown`", nên em không đoán trạng thái mí mắt khi điểm đã bị che hết.
