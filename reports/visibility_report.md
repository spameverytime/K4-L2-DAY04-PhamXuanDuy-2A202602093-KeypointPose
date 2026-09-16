# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 28 skeleton, trung bình 16.25 khớp có v > 0 mỗi người
- Tổng: v=2 347 | v=1 108 | v=0 21

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 0 | 21% |
| 1 | left_eye | 19 | 9 | 0 | 32% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 13 | 15 | 0 | 54% |
| 4 | right_ear | 17 | 11 | 0 | 39% |
| 5 | left_shoulder | 26 | 2 | 0 | 7% |
| 6 | right_shoulder | 28 | 0 | 0 | 0% |
| 7 | left_elbow | 24 | 4 | 0 | 14% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 22 | 6 | 0 | 21% |
| 10 | right_wrist | 18 | 9 | 1 | 32% |
| 11 | left_hip | 26 | 2 | 0 | 7% |
| 12 | right_hip | 25 | 3 | 0 | 11% |
| 13 | left_knee | 18 | 9 | 1 | 32% |
| 14 | right_knee | 19 | 8 | 1 | 29% |
| 15 | left_ankle | 13 | 6 | 9 | 21% |
| 16 | right_ankle | 12 | 7 | 9 | 25% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
