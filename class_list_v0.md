# Danh sách lớp v0 — Đặng Gia Tuấn · G09

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_state` (`eye_left`, `eye_right`) | `open` / `closed` / `unknown` | Phát hiện ngủ gật, tính chỉ số PERCLOS (tỷ lệ thời gian mắt nhắm kéo dài) | Có, trừ khi tròng kính bị đốm phản xạ LED 940nm che khuất |
| 2 | `gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Phát hiện mất tập trung (distraction), cảnh báo khi tài xế không nhìn đường quá thời gian quy định | Có, ước lượng qua vector hướng nhìn của mắt và hướng xoay đầu |
| 3 | `glasses` | `none` / `clear` / `sun` / `unknown` | Điều chỉnh thuật toán nhận diện mắt; cảnh báo khi kính râm cản trở theo dõi mắt | Có, nhưng kính râm tối màu có thể bị nhìn xuyên thấu dưới phổ NIR |
| 4 | `head_pose` (từ `face` & `face13`) | Góc yaw, pitch, roll | Phát hiện tài xế cúi gục đầu hoặc quay mặt quá 30 độ khỏi hướng lái xe | Rất tốt, không phụ thuộc vào điều kiện ánh sáng ngày/đêm |
| 5 | `phone` | `held` / `mounted` / `lap` | Phát hiện hành vi sử dụng điện thoại khi đang lái xe để kích hoạt cảnh báo nguy hiểm tức thì | Có (dựa vào hình khối chữ nhật, viền máy/màn hình), khó khi tay cầm vật tối |
| 6 | `hand.on_wheel` | `yes` / `no` / `unknown` | Kiểm tra quy định đặt tay lên vô lăng (Hands-on-wheel detection) trong hệ thống hỗ trợ lái L2/L2+ | Có, quan sát rõ vị trí bàn tay tiếp xúc với vành vô lăng |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person.seat` (ghế ngồi) | `driver` / `front_passenger` / `rear_left` / `rear_middle` / `rear_right` | Xác định vị trí có hành khách để điều khiển túi khí tương ứng và điều hòa vùng ghế | Có, nhận diện rõ dựa vào hình dáng cơ thể người |
| 2 | `seatbelt` | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Kích hoạt chuông nhắc cài dây an toàn; điều chỉnh lực nổ túi khí hoặc kích hoạt bộ siết đai khẩn cấp | Có nếu tương phản độ sáng tốt; khó thấy khi áo tối màu trùng màu dây |
| 3 | `child_seat` | `occupied` (`no` / `yes` / `unknown`), `seat` | Nhận diện ghế trẻ em để ngắt túi khí ghế phụ hoặc tối ưu túi khí bên hông | Có, nhận diện qua khung nhựa và đệm ghế đặc trưng |
| 4 | `infant_seat` | `facing` (`rear` / `front`), `seat` | Bắt buộc ngắt túi khí trước nếu nôi em bé quay về sau (`rear`), tránh nguy hiểm khi túi khí bung | Có, nhận diện hướng quay của nôi và quai xách |
| 5 | `object` | `type` (`bag` / `box` / `cup` / `other`), `seat` | Phân biệt hành lý với con người, tránh cảnh báo thắt dây giả hoặc kích hoạt nhầm túi khí | Có, dựa vào hình học khối hộp hoặc túi xách |
| 6 | `forgotten_child_detection` (kết hợp `person` & `child_seat`) | Có / không | Cảnh báo chống bỏ quên trẻ em/vật nuôi trong xe khi tắt máy (chuẩn an toàn Euro NCAP CPD) | Rất tốt vì camera hồng ngoại quét được khoang sau trong bóng tối |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Khi tài xế đeo kính râm sẫm màu nhưng dưới camera NIR nhìn xuyên qua thấy rõ con ngươi đang mở, thì thuộc tính `glasses` nên ghi là `sun` (theo loại kính thực tế) hay `clear` (theo hình ảnh camera thấy), và `eye_*` có được phép gán `open` không?
   Khung ví dụ: `D04.png`
   Bạn tạm gán thế nào và vì sao: Tạm gán `glasses = sun` (vì thực tế là kính râm) và `eye_* = open` vì camera NIR nhìn xuyên qua cho thấy rõ khe mắt đang mở, hỗ trợ model nhận diện trạng thái thức tỉnh của tài xế.

2. Câu hỏi: Trong ảnh hồng ngoại ban đêm, nếu áo khoác của hành khách trùng màu tối với dây an toàn và ghế ngồi khiến không nhìn thấy đường chéo của dây, có được phép suy luận trạng thái cài dây từ vị trí chốt cài ở hông ghế không?
   Khung ví dụ: `S01_ir.png`
   Bạn tạm gán thế nào và vì sao: Tạm gán `seatbelt = unknown` theo đúng nguyên tắc 1 của guideline ("Không thấy thì unknown"), tuyệt đối không suy đoán để tránh đưa nhiễu (noise) vào tập huấn luyện của model.
