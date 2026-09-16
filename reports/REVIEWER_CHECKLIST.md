# Reviewer checklist - điền khi kiểm bài người khác

Người gán: bạn cùng nhóm (ban_cung_nhom)   Người kiểm: Phạm Xuân Duy   Ngày: 16/09/2026

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels ban_cung_nhom/dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train --labels ban_cung_nhom/dataset/labels/train --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare ban_cung_nhom/dataset/labels/train
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đạt: 28 skeleton đều đủ 17 điểm. Lưu ý: Thiếu 1 người ở `train_13.jpg` so với gold. |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☒ | Không đạt: `train_15.jpg` người #1 bị lỗi đảo trái/phải (`dao_trai_phai`), đường nối xương chéo thân. |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Đạt: 0 lỗi nhầm người (`nham_nguoi`). |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☒ | Không đạt: `train_01.jpg` người #1 (`right_wrist`) và người #2 (`left_wrist`) bị xoá khớp bị che (`xoa_khop_bi_che`). |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☒ | Không đạt: `train_10.txt`, `train_11.txt`, `train_13.txt` có các khớp v=0 dù người nằm gọn giữa khung hình. |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Đạt: Không phát hiện điểm v=2 ở vị trí bất thường. |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Đạt: Định dạng xuất chuẩn. |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đạt: Mỗi dòng đủ 56 số (1 class + 4 box + 17*3 keypoints). |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Đạt: Đã so sánh chi tiết trong `reports/visibility_compare.md`. |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đạt. |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Đạt định dạng (0 lỗi định dạng, có 3 cảnh báo về cờ v=0 giữa ảnh). |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_15.jpg` | 1 | Toàn bộ skeleton | Đảo trái/phải (`dao_trai_phai`) | Đổi lại toàn bộ các cặp điểm đối xứng trái và phải (mắt, tai, vai, khuỷu, cổ tay, hông, gối, cổ chân) |
| `train_13.jpg` | 1 | Toàn bộ skeleton | Thiếu hẳn một người | Thêm skeleton cho người ở sát rìa mép trái ảnh (gold có người này) |
| `train_01.jpg` | 1 | `right_wrist` | Xoá khớp bị che (`v=0`) | Đổi cờ sang `v=1` và chấm ước lượng vị trí cổ tay phải bị che |
| `train_01.jpg` | 2 | `left_wrist` | Xoá khớp bị che (`v=0`) | Đổi cờ sang `v=1` và chấm ước lượng vị trí cổ tay trái bị che |
| `train_15.jpg` | 1 | `right_ear`, `left_elbow`, `right_elbow` | Trượt hẳn (`truot_han`) | Kéo các điểm chấm này về đúng vị trí mốc giải phẫu tai và khuỷu tay |
| `train_10.jpg` | 1 | Khớp chân (`knee`, `ankle`) | Dùng nhầm `v=0` thay vì `v=1` | Người nằm trọn giữa khung hình, sửa các khớp chân bị che thành `v=1` kèm vị trí ước lượng |
| `train_12.jpg` | 1 | `left_knee`, `right_knee`, `right_ankle` | Lệch nhẹ (`lech_nhe`) | Căn chỉnh chấm sát vào tâm xoay khớp giải phẫu |
| `train_13.jpg` | 2 | `right_wrist`, `left_knee`, `right_knee` | Lệch nhẹ (`lech_nhe`) | Căn chỉnh lại vị trí cổ tay và đầu gối theo trục cơ thể |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Nhầm lẫn giữa khớp bị che (`v=1`) và khớp ngoài khung hình (`v=0`) khi đối tượng nằm trọn giữa ảnh (như tại `train_01.jpg`, `train_10.jpg`, `train_11.jpg`), cùng 1 lỗi đảo trái/phải nghiêm trọng tại `train_15.jpg`.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Đây là sự kết hợp giữa lỗi **thao tác** (làm vội gây đảo trái/phải ở `train_15.jpg`) và lỗi **guideline chưa rõ** (chưa nắm chắc quy tắc cờ: khớp bị che còn trong khung bắt buộc dùng `v=1` và vẫn đặt chấm ước lượng, không được dùng `v=0`).
