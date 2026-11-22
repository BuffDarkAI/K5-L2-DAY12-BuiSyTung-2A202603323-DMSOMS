# Tự đánh giá — Bùi Sỹ Tùng · G07

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01 (rgb và ir) | child_seat | seat_flip | Ghế nằm ở nửa trái ảnh nhưng ghi `rear_left`; trái ảnh phải là `rear_right` | Đã sửa |
| S03_rgb | object (bag) | seat_flip | Quên đổi `seat` mặc định `driver`; ảnh `_ir` cùng cảnh ghi `rear_right` | Đã sửa |
| D05 | hand | lr_flip | Hai tay cùng `side = left`; tay cầm điện thoại phải là tay `right` | Đã sửa |
| D04 | face (eye_right) | guess | `eye_right = closed` nhưng 4 điểm mắt phải đánh `outside` (không thấy mắt) nên phải là `unknown` | Đã sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Quên sửa giá trị mặc định mà CVAT tự điền: `light = day` trên cả 16 ảnh S##, `seat = driver` ở S03_rgb, `side = left` ở hai tay D05.

Vì sao mình mắc: CVAT điền sẵn giá trị đầu danh sách cho mọi thuộc tính, còn mình chỉ chú ý những thuộc tính mình đang nghĩ tới và không mở lại DETAILS sau khi vẽ box.

Lần sau tránh bằng cách: Vẽ xong box nào thì mở DETAILS và đọc hết các thuộc tính của box đó ngay; trước khi export thì chạy `cabin_check` để bắt các giá trị mặc định còn sót.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: Không có dòng Tân đúng hẳn, nhưng ở D04 `gaze_zone` cả hai cùng sai (mình `center`, Tân `mirror_c`). Mắt trái nhắm, mắt phải không thấy thì không đủ căn cứ chọn vùng nhìn; mình sửa thành `other` trong `export_final_v2.zip`. Học được: khi không thấy mắt thì không đoán hướng nhìn.
