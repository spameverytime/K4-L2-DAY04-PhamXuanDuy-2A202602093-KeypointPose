# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 28 skeleton, trung bình 16.25 khớp có v > 0 mỗi người
- Tổng: v=2 347 | v=1 108 | v=0 21

So sánh với `../ban_cung_nhom/dataset/labels/train` (0 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 54% | 0% | 54 |
| 4 | right_ear | 39% | 0% | 39 |
| 1 | left_eye | 32% | 0% | 32 |
| 10 | right_wrist | 32% | 0% | 32 |
| 13 | left_knee | 32% | 0% | 32 |
| 2 | right_eye | 29% | 0% | 29 |
| 14 | right_knee | 29% | 0% | 29 |
| 16 | right_ankle | 25% | 0% | 25 |
| 0 | nose | 21% | 0% | 21 |
| 9 | left_wrist | 21% | 0% | 21 |
| 15 | left_ankle | 21% | 0% | 21 |
| 7 | left_elbow | 14% | 0% | 14 |
| 8 | right_elbow | 11% | 0% | 11 |
| 12 | right_hip | 11% | 0% | 11 |
| 5 | left_shoulder | 7% | 0% | 7 |
| 11 | left_hip | 7% | 0% | 7 |
| 6 | right_shoulder | 0% | 0% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
