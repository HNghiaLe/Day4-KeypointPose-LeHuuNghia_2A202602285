# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Hữu Nghĩa   Nhóm: Solo (MSSV: 2A202602285)   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 341 / 111 / 24 |
| Thời gian trung bình mỗi ảnh | 4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_hip` / `right_hip` (~45% v=1)
2. `left_ankle` / `right_ankle` (~32% v=1)
3. `left_knee` / `right_knee` (~25% v=1)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng như vậy. Khớp hông là vị trí khó xác định nhất về mặt giải phẫu vì hầu hết đối tượng trong bộ ảnh đều mặc quần áo dài phủ ngoài. Việc chấm điểm hông bắt buộc phải ước lượng dựa vào khoảng cách từ thắt lưng tới gốc đùi chứ không có điểm thị giác trực tiếp. Trong khi đó, khớp cổ chân và đầu gối thường xuyên bị các vật thể nội thất, xe cộ hoặc người khác che khuất một phần.

## 2. Chấm với gold

## 2. Chấm với gold
<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->
| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.952 | 0.951 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |
**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):
<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->
- `train_13.jpg` + người thứ 1 + toàn bộ cơ thể: Đã gán bổ sung thêm 1 skeleton bị bỏ sót. Điểm OKS@0.50 và OKS@0.75 đều đạt mức hoàn hảo tuyệt đối 1.000.
**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?
Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: Solo (Tự rà soát đơn lẻ)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_hip` | 46% | 46% | 0% | Đã tự rà soát và thống nhất quy tắc ước lượng hông |
| `right_knee` | 25% | 25% | 0% | Giữ đúng nguyên tắc v=1 cho khớp bị che trong khung hình |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Mọi khớp cơ thể bị che khuất đằng sau trang phục hoặc vật thể (bàn ghế, xe cộ) nhưng cơ thể người đó vẫn nằm trọn trong khung hình thì bắt buộc phải ước lượng vị trí giải phẫu, chọn `v=1` và chấm điểm, tuyệt đối không được chọn `v=0`.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 5. Một rule evidence bạn đã dùng

Tại ảnh `train_04.jpg`, người thứ 2: Khớp gối trái (`left_knee`) bị chiếc bàn phía trước che khuất hoàn toàn. Căn cứ thị giác là phần đùi trên và cẳng chân phía dưới vẫn lộ ra rõ ràng và đối tượng nằm trọn ở giữa khung hình. Do đó, tôi xác định vị trí giao nhau ước lượng của đùi và cẳng chân phía sau mặt bàn, chấm điểm keypoint và chọn trạng thái `v=1` (Occluded) thay vì bỏ qua hay dùng `v=0`.
