# Tự đánh giá — Đặng Gia Tuấn · G09

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb.png | person | seat_flip | Nhầm bên trái ảnh là rear_left, đã điều chỉnh thành rear_right theo xe | Đã sửa |
| S02_ir.png | child_seat | seat_flip | CVAT tự điền giá trị đầu danh sách seat=driver, đã sửa thành rear_right | Đã sửa |
| D01.png | face | guess | Mắt dưới đốm phản xạ LED 940nm không được đoán mà phải chọn unknown | Đã sửa |
| D04.png | face13 | flag | Các điểm mặt bị che khuất được bật cờ occluded=1 chính xác | Đã sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Nhầm lẫn quy ước tọa độ vị trí ghế trên ảnh OMS (camera nhìn ngược về sau nên bên trái ảnh là bên phải của xe).

Vì sao mình mắc: Do thói quen nhìn ảnh theo góc nhìn trực quan thông thường (trái ảnh là bên trái) thay vì góc nhìn của người ngồi trong xe nhìn về phía trước.

Lần sau tránh bằng cách: Luôn tự định hướng trong đầu góc nhìn từ vô lăng hướng về sau xe trước khi vẽ box ghế OMS: Trái ảnh = rear_right, Phải ảnh = rear_left.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó:
Ở ca D02.png (điểm right_eye_outer), bạn đã bật cờ occluded vì có một phần mí mắt bị bóng đổ che khuất nhẹ. Qua đó mình học được sự cẩn trọng và khắt khe hơn trong việc đánh giá mức độ che khuất của các điểm skeleton trên ảnh NIR.
