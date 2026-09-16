# Báo cáo Kiểm chéo bài bạn cùng nhóm (Review Partner)

- **Người gán (được kiểm)**: bạn cùng nhóm (`ban_cung_nhom`)
- **Người kiểm**: Phạm Xuân Duy
- **Ngày kiểm**: 16/09/2026
- **Thư mục nhãn kiểm tra**: `ban_cung_nhom/dataset/labels/train`
- **Kết quả chạy công cụ**:
  - `tools/check_pose_labels.py`: Đọc 20/20 file nhãn, 28 skeleton. Đạt định dạng (v=2: 328, v=1: 119, v=0: 29), 3 cảnh báo lỗi `v=0` giữa ảnh.
  - `tools/evaluate_pose_annotations.py`: OKS trung bình: **0.920** | OKS@0.50: **0.931** | OKS@0.75: **0.931**.
  - Tổng số lỗi tìm được: 1 lỗi đảo trái/phải, 1 người bị thiếu, 2 lỗi xoá khớp bị che, 3 lỗi trượt hẳn, 13 lỗi lệch nhẹ.

---

## 1. Bảng danh sách lỗi chi tiết

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_15.jpg` | 1 | Toàn bộ skeleton | Đảo trái/phải (`dao_trai_phai`) | Đổi lại toàn bộ các cặp điểm đối xứng trái và phải (mắt, tai, vai, khuỷu, cổ tay, hông, gối, cổ chân) cho skeleton người này |
| `train_13.jpg` | 1 | Toàn bộ skeleton | Thiếu hẳn một người (`thieu_nguoi`) | Bổ sung skeleton cho người ở sát rìa bên trái khung hình (gold có người này, hiện OKS người này bị tính 0.000) |
| `train_01.jpg` | 1 | `right_wrist` | Xoá khớp bị che (`xoa_khop_bi_che`) | Đổi cờ từ `v=0` sang `v=1`, đặt chấm ước lượng tại vị trí giải phẫu cổ tay phải bị che khuất |
| `train_01.jpg` | 2 | `left_wrist` | Xoá khớp bị che (`xoa_khop_bi_che`) | Đổi cờ từ `v=0` sang `v=1`, đặt chấm ước lượng tại vị trí giải phẫu cổ tay trái bị che khuất |
| `train_15.jpg` | 1 | `right_ear` | Trượt hẳn (`truot_han`) | Chấm lệch quá 3 lần bán kính dung sai tolerance; kéo chấm về đúng vành tai phải |
| `train_15.jpg` | 1 | `left_elbow` | Trượt hẳn (`truot_han`) | Kéo chấm về đúng mỏm khuỷu tay trái của người này |
| `train_15.jpg` | 1 | `right_elbow` | Trượt hẳn (`truot_han`) | Kéo chấm về đúng mỏm khuỷu tay phải của người này |
| `train_10.jpg` | 1 | 4 khớp chân (`knee`, `ankle`) | Dùng nhầm `v=0` thay vì `v=1` | Người nằm gọn giữa ảnh không ra ngoài mép; nếu bị che thì phải chọn `v=1` và chấm ước lượng, không được dùng `v=0` |
| `train_11.jpg` | 1 | 4 khớp chân (`knee`, `ankle`) | Dùng nhầm `v=0` thay vì `v=1` | Người nằm gọn giữa ảnh không ra ngoài mép; nếu bị che thì phải chọn `v=1` và chấm ước lượng, không được dùng `v=0` |
| `train_12.jpg` | 1 | `left_knee`, `right_knee`, `right_ankle` | Lệch nhẹ (`lech_nhe`) | Căn chỉnh lại chấm sát hơn vào tâm xoay khớp giải phẫu |
| `train_13.jpg` | 2 | `right_wrist`, `left_knee`, `right_knee` | Lệch nhẹ (`lech_nhe`) | Căn chỉnh lại điểm chấm khớp cổ tay và khớp gối |
| `train_15.jpg` | 2 | `right_elbow` | Lệch nhẹ (`lech_nhe`) | Kéo chấm dịch vào trong tâm khớp khuỷu tay phải |

---

## 2. Phân tích độ lệch Visibility Report giữa hai bài

Theo số liệu so sánh tại `reports/visibility_compare.md`:
- Khớp lệch `%v=1` nhiều nhất:
  - `left_wrist`: Bạn gán 21% / Họ gán 32% (lệch **11%**)
  - `left_hip`: Bạn gán 7% / Họ gán 18% (lệch **11%**)
  - `right_ear`: Bạn gán 39% / Họ gán 46% (lệch **7%**)
  - `left_eye`: Bạn gán 32% / Họ gán 25% (lệch **7%**)
- **Nguyên nhân**:
  - Đối với `left_wrist`: Bài của bạn cùng nhóm có xu hướng chọn `v=1` nhiều hơn khi cổ tay bị tay áo hoặc tay lái che khuất. Tuy nhiên ở `train_01.jpg`, bạn cùng nhóm lại bị lỗi chọn nhầm sang `v=0` (xoá khớp bị che).
  - Đối với `left_hip`: Bạn cùng nhóm tuân thủ guideline khắt khe hơn khi đánh dấu `v=1` cho các trường hợp mặc quần áo rộng không thấy mốc xương.

---

## 3. Kết luận và Hướng sửa (Action Items)

1. **Ưu tiên 1 (Nguy hiểm nhất - sửa ngay)**: Sửa lỗi **đảo trái/phải** ở `train_15.jpg` người #1. Đổi lại toàn bộ các cặp điểm trái/phải, sau đó kéo lại 3 điểm trượt hẳn (`right_ear`, `left_elbow`, `right_elbow`). Lỗi này kéo OKS người này xuống chỉ còn **0.394**.
2. **Ưu tiên 2**: Bổ sung skeleton cho người ở rìa mép trái ảnh `train_13.jpg` để không bị mất điểm thiếu người (OKS đang bị 0.000 cho người này).
3. **Ưu tiên 3**: Sửa lỗi **xoá khớp bị che** ở `train_01.jpg` (người #1 và người #2), đổi các khớp cổ tay từ `v=0` sang `v=1` và đặt chấm ước lượng.
4. **Ưu tiên 4**: Rà soát lại `train_10.jpg` và `train_11.jpg`, thay thế các cờ `v=0` nằm giữa ảnh thành `v=1`.

