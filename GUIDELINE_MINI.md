# Mini guideline - nhóm: Solo  |  người gán: Lê Hữu Nghĩa (MSSV: 2A202602285)  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng vị trí xương hông dựa trên vị trí khớp thắt lưng và đùi. Đặt `v = 1`. | Hông không lộ ra bề mặt nhưng có giải phẫu cố định giúp model học đúng khung xương body. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đặt tại vị trí cuống tai ước lượng, đặt cờ `v = 1` (Occluded). | Tóc/mũ che nhưng đầu và khuôn mặt xác định rõ gốc tai. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài mép ảnh đặt `v = 0` (Outside) và không đặt chấm. Các khớp trong mép ảnh gán bình thường. | Điểm nằm ngoài mép ảnh không có tọa độ pixel thực tế. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí gốc cổ tay đằng sau vật che, chọn `v = 1` (Occluded). | Tay lái/thân mình che khuất nhưng hướng cánh tay cho phép suy luận tọa độ cổ tay. |
| Hai người chồng lên nhau | Gán từng người hoàn chỉnh. Khớp bị người trước che chọn `v = 1` (Occluded) và đặt chấm ước lượng. | Đảm bảo mỗi skeleton đủ 17 điểm và không kéo nhầm điểm sang người bên cạnh. |
| Người nhỏ đến mức nào thì không gán nữa | Mọi người xuất hiện đầy đủ nét trong 20 ảnh này đều được gán nhãn. | Bộ ảnh train 20 ảnh đã chọn lọc người đủ rõ để annotation. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02.jpg`, người thứ `1`, khớp `left_shoulder / right_shoulder`

- Mơ hồ ở chỗ nào: Người quay lưng hoặc hướng mặt hơi nghiêng dẫn đến nhầm lẫn hướng Trái/Phải theo góc nhìn người chụp.
- Bạn quyết thế nào: Tính Trái/Phải dựa theo cơ thể thực tế của đối tượng (tự đặt mình vào vị trí đối tượng giơ tay trái).
- Vì sao: Tránh lỗi đảo trái/phải nguy hiểm nhất khi train model Pose Estimation.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học sai vĩnh viễn nhánh màu xương trái/phải và bị phạt điểm OKS rất nặng khi lật ảnh (augmentation).

### Ca 2 - ảnh `train_04.jpg`, người thứ `2`, khớp `left_knee / right_knee`

- Mơ hồ ở chỗ nào: Khớp gối bị vật thể/nội thất che hoàn toàn nhưng người vẫn nằm gọn trong khung hình.
- Bạn quyết thế nào: Ước lượng vị trí khớp gối đằng sau vật che và đánh cờ `v = 1` (Occluded), không dùng `v = 0`.
- Vì sao: `v = 0` chỉ dành cho điểm bị cắt ra ngoài mép ảnh (Outside). Khớp bị che còn trong khung hình phải là `v = 1`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model tưởng đối tượng bị cắt cụt chân thay vì hiểu rằng chân đang bị che khuất.

### Ca 3 - ảnh `train_13.jpg`, người thứ `2`, khớp `left_ankle / right_ankle`

- Mơ hồ ở chỗ nào: Cổ chân nằm phía sau ghế/vật cản.
- Bạn quyết thế nào: Chấm điểm ước lượng cổ chân theo trục cẳng chân và tick `v = 1`.
- Vì sao: Tuân thủ đúng guideline 3 trạng thái visibility của lớp.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Đánh mất thông tin tỷ lệ cơ thể và làm rỗng dữ liệu train cho khớp cổ chân.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `Làm Solo` (Tự gán nhãn và rà soát đơn lẻ)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Đã tự đối chiếu với script `check_pose_labels.py` và chuẩn hóa toàn bộ cờ `v=1`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Giữ nguyên quy chuẩn ước lượng giải phẫu cho toàn bộ các khớp bị che (`v=1`).
