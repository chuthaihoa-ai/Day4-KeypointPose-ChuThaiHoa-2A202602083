# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.34 khớp có v > 0 mỗi người
- Tổng: v=2 314 | v=1 131 | v=0 48

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 8 | 0 | 28% |
| 1 | left_eye | 18 | 11 | 0 | 38% |
| 2 | right_eye | 19 | 10 | 0 | 34% |
| 3 | left_ear | 11 | 18 | 0 | 62% |
| 4 | right_ear | 15 | 14 | 0 | 48% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 26 | 3 | 0 | 10% |
| 7 | left_elbow | 19 | 8 | 2 | 28% |
| 8 | right_elbow | 26 | 2 | 1 | 7% |
| 9 | left_wrist | 17 | 8 | 4 | 28% |
| 10 | right_wrist | 19 | 8 | 2 | 28% |
| 11 | left_hip | 19 | 7 | 3 | 24% |
| 12 | right_hip | 22 | 4 | 3 | 14% |
| 13 | left_knee | 14 | 8 | 7 | 28% |
| 14 | right_knee | 15 | 7 | 7 | 24% |
| 15 | left_ankle | 14 | 6 | 9 | 21% |
| 16 | right_ankle | 14 | 5 | 10 | 17% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
