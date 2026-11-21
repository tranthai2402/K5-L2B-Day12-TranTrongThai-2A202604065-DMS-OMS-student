# TEAM — Ngày 12

Mã bộ ảnh: G07 · Hình thức: cá nhân (coach ghép cặp review chéo)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
| Trần Trọng Thái | 2A202604065 | có (22/22, export `export_final_v2.zip`) | Phạm Duy Thanh | 79/79 (đã điền đủ cột `verdict`, `guideline_rule`, `action`) |
| Phạm Duy Thanh | 2A202603343 | có (22/22, export `peer_export_final.zip`) | Trần Trọng Thái | 79/79 (đã điền đủ cột `verdict`, `guideline_rule`, `action`) |

## Cá nhân

Mỗi người một mục, 1–2 dòng mỗi ý.

### Trần Trọng Thái -2A202604065
- Phần mình làm (ngoài 22 ảnh của mình, ví dụ ai review ai, ai tổng hợp ca hỏi coach): Mình tự gán đủ 22 ảnh trong task `Day12-G07` trên CVAT local; export `export_final.zip` (E1), tự chạy `cabin_check` ở Checkpoint A, Checkpoint B và lần cuối; chạy `cabin_check diff` với bạn cùng mã G07 và điền đủ 79 dòng `disagreement_log.csv`; sau đó sửa lại các lỗi đã rõ (modality, light, xóa `object` ở DMS, xóa `hand` ở OMS, xóa `face13` ở S08) và export `export_final_v2.zip`.
- Điều mình học được hôm nay: (1) Quy tắc ghế theo xe — camera OMS nhìn về phía sau nên trái ảnh là phải xe — là lỗi mình mắc nhiều nhất; phải nói to vị trí theo xe trước khi chọn giá trị `seat`. (2) Trạng thái mắt và cờ `occluded` trên `face13` phải thống nhất: mắt không thấy mí thì `unknown`, không ghi `open`. (3) `cabin_check` bắt được lỗi hình thức (thiếu thuộc tính, giá trị sai, thiếu `frame_meta`) nhưng không bắt được lỗi ngữ nghĩa như lật trái/phải ghế hay sai loại nhãn — phần đó phải tự soát và dùng review chéo. (4) Có những chỗ guideline v1.0 chưa khớp dữ liệu thực tế (thuộc tính `occupied` của `child_seat` và `infant_seat`); mình đã ghi lại trong `Report.md` để coach phân xử thay vì tự đặt luật mới.

### Phạm Duy Thanh - 2A202603343
- Phần mình làm: Mình tự gán đủ 22 ảnh của bộ G07 trong task `Day12-G07` trên CVAT local và export ra file riêng để review chéo với Trần Trọng Thái (2A202604065). Mình là bên "peer" trong `disagreement_log.csv` — trong 79 dòng bất đồng, mình thắng 16 dòng (đúng ở `modality = ir` cho D01–D06, `light = unknown` cho ảnh SVIRO, thêm box `face` cho người lớn ở S02_ir, và không vẽ `face13` ở S08_ir vì guideline §4.6 chỉ cho vẽ trên ảnh D##).
- Những chỗ mình làm khác bạn review: Mình gán `seat` cho ghế theo hướng ảnh (trái ảnh là `rear_left`) trong khi bạn review gán theo hướng xe (trái ảnh là `rear_right`) — theo GUIDE §4.4-3 thì bạn review đúng, mình sai ở nhóm này (27 dòng). Mình cũng không vẽ `object` ở D01/D03 và không vẽ `hand` ở S02_ir, đúng với §4.1–§4.2.
- Điều mình học được hôm nay: Hai người gán giống nhau không chứng minh nhãn đúng — hơn 1/3 số dòng bất đồng là do lật trái/phải ghế, và đây đúng là loại lỗi mà `cabin_check` không bắt được. Ngoài ra, ảnh SVIRO không ai thắt dây nên `seatbelt` chỉ có `not_worn`/`unknown`, và IR của SVIRO là ảnh mô phỏng nên không có hiện tượng chói kính, mắt sáng như IR thật.
