# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.17 khớp có v > 0 mỗi người
- Tổng: v=2 356 | v=1 113 | v=0 24

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 26 | 3 | 0 | 10% |
| 1 | left_eye | 23 | 6 | 0 | 21% |
| 2 | right_eye | 23 | 6 | 0 | 21% |
| 3 | left_ear | 14 | 15 | 0 | 52% |
| 4 | right_ear | 16 | 13 | 0 | 45% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 26 | 3 | 0 | 10% |
| 8 | right_elbow | 26 | 3 | 0 | 10% |
| 9 | left_wrist | 20 | 9 | 0 | 31% |
| 10 | right_wrist | 19 | 9 | 1 | 31% |
| 11 | left_hip | 21 | 8 | 0 | 28% |
| 12 | right_hip | 22 | 6 | 1 | 21% |
| 13 | left_knee | 18 | 8 | 3 | 28% |
| 14 | right_knee | 19 | 7 | 3 | 24% |
| 15 | left_ankle | 16 | 5 | 8 | 17% |
| 16 | right_ankle | 13 | 8 | 8 | 28% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
