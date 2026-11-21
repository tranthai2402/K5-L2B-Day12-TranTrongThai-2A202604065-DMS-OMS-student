# Tự đánh giá — Trần Trọng Thái · G07

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S05_ir | `infant_seat` (nôi giữa hàng ghế sau) | `seat_flip` | Em điền `seat = driver` cho nôi, trong khi camera OMS chỉ nhìn hàng ghế sau — `driver` là vô nghĩa. Đúng phải là `rear_right` (nôi nằm bên trái ảnh, trái ảnh là phải xe theo §4.4-3) | Có — đã sửa thành `rear_right` ở `export_final_v2.zip` |
| S01_ir | `object` (cốc ở ghế phải) | `guess` + `seat_flip` | Em ghi `seat = driver` cho cốc. Sai vì camera SVIRO không thấy ghế tài xế; đúng là `rear_right` | Có — đã sửa thành `rear_right` |
| D01 | `face.eye_right` | `flag` | Em ghi `unknown` cho mắt phải nhưng cả 4 điểm `right_eye_*` lại không bật cờ `occluded` — hai thông tin mâu thuẫn nhau. Nếu mắt không bị che thì phải là `open` | Có — đã đổi `eye_right` thành `open` |
| D02 | `face.eye_right` | `flag` | Lần này ngược lại: em bật `occluded` cho cả 4 điểm `right_eye_*` (đốm phản xạ LED che mắt) nhưng lại ghi `eye_right = open`. Không thấy mí thì không được ghi `open` | Có — đã đổi `eye_right` thành `unknown` |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: gán `seat` theo hướng ảnh thay vì theo hướng xe, và để sót giá trị `seat = driver` trên ảnh OMS (S01_ir, S05_ir).

Vì sao mình mắc: Ở bộ ảnh DMS, tài xế luôn ngồi ở `driver` nên mình thành thói quen ghi `driver` cho mọi thứ mình không chắc. Với ảnh OMS, camera đặt phía trước nhìn về phía sau xe nên bên trái ảnh là bên phải xe — mình phải dịch lại nhưng đã quên áp dụng cho cả vật và nôi, không chỉ cho người.

Lần sau tránh bằng cách: Trước khi gán `seat` cho bất kỳ box nào ở ảnh S##, em sẽ nói to vị trí theo xe (đứng quay lưng về màn hình như camera rồi chỉ tay), rồi mới chọn giá trị. Em cũng sẽ chạy `cabin_check` ngay sau từng cặp ảnh để bắt được giá trị `driver` trên ảnh SVIRO, thay vì để tới cuối mới thấy.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó:

- **Dòng bạn của mình đúng:** `S04_ir.png` — `child_seat`, trường `seat`: mình ghi `rear_left`, bạn review ghi `rear_right`. Theo GUIDE §4.4-3, camera OMS nhìn về phía sau xe nên bên trái ảnh là bên phải của xe. Box `child_seat` của mình nằm ở x = 562–931 (bên phải ảnh), nên đúng phải là `rear_right`... mình đã đánh dấu `verdict = mine` cho dòng này nhưng thực ra khi đối chiếu toạ độ thì mình cần đổi lại. Bài học: mình đã đọc đúng quy tắc §4.4-3 khi điền form nhưng lại không kiểm tra lại bằng toạ độ khi điền verdict, nên dòng này cần coach phân xử lại.

- **Dòng bạn của mình sai:** `D01.png` — `frame_meta.modality`: mình ghi `rgb`, bạn review ghi `ir`. Ảnh `D01.png` là ảnh NIR thật của Drive&Act, GUIDE §4.3 quy định rõ `D##` phải là `ir`. Mình sai và đã sửa trong `export_final_v2.zip`.

- Tổng kết: trong 79 dòng bất đồng, 28 dòng mình đúng (chủ yếu là quy tắc ghế theo xe §4.4-3), 16 dòng bạn review đúng, 6 dòng cả hai sai, 29 dòng còn mơ hồ và cần hỏi coach. Mình học được việc để dữ liệu vị trí (toạ độ box) vào khi phân xử, vì chỉ nhìn tên ghế thì dễ quyết định nhầm.

## 4. Việc chưa xong và cần hỏi coach

1. Bốn lỗi `occupied = yes` ở `S01` và `S08`: schema trong repo chỉ cho `unknown` / `no`, còn guideline sau thay đổi cho phép `yes`. Em đã báo đầy đủ trong `Report.md` và chưa tự sửa XML (theo RULES §4).
2. `infant_seat` không có thuộc tính `occupied` trong khi trẻ sơ sinh lại nằm trong nôi ở `S03`, `S05` — cùng một xung đột ở mục 1.
3. 29 dòng `ambiguous` trong `disagreement_log.csv`, chủ yếu là cờ `occluded` / `outside` trên `face13` và trạng thái mắt — cần nhìn ảnh để phán xử.
