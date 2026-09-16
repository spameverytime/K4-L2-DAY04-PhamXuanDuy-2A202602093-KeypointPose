# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Phạm Xuân Duy   Nhóm: Nhóm 5   Ngày: 16/09/2026

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
| v=2 / v=1 / v=0 | 347 / 108 / 21 |
| Thời gian trung bình mỗi ảnh | 3.5 phút/ảnh |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 54% (15 occluded / 28)
2. `right_ear`: 39% (11 occluded / 28)
3. `left_eye`: 32% (9 occluded / 28) [đồng hạng với `right_wrist` 32% và `left_knee` 32%]

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->
Không hoàn toàn đúng. Khớp tai (`left_ear`, `right_ear`) và mắt (`left_eye`) có tỉ lệ `%v=1` cao nhất vì chúng "hay bị che" bởi góc quay khuôn mặt (nghiêng 3/4 hoặc nhìn sang hướng khác) và phụ kiện (tóc, mũ bảo hiểm), nhưng về mặt giải phẫu vị trí của chúng rất dễ ước lượng dựa trên đối xứng khuôn mặt và trục đầu. Khớp thực sự khó gán nhất là khớp hông (`left_hip`, `right_hip`) và cổ tay (`left_wrist`, `right_wrist`) vì bị trang phục thụng hoặc vật thể (tay lái, túi xách) che lấp hoàn toàn mốc giải phẫu xương, đòi hỏi phải suy luận không gian phức tạp để đặt chấm ước lượng chính xác.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.923 | 0.958 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 0.966 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_13.jpg` + người thứ 1 (rìa trái) + toàn bộ 17 điểm: Bổ sung skeleton cho người ở rìa ảnh bị bỏ sót (gold có người này, kéo OKS ảnh từ 0.000 lên đạt chuẩn).
- `train_02.jpg` + người thứ 1 + `left_shoulder`: Kéo lại điểm chấm sát vào mỏm cùng vai trái thật để khắc phục lỗi lệch nhẹ so với gold.
- `train_04.jpg` + người thứ 1 + `left_hip`, `right_hip`: Căn chỉnh lại tọa độ hai bên hông cân xứng theo trục thắt lưng người lái xe.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->
Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: ban_cung_nhom (Duyệt nhãn chéo)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_wrist | 21% | 32% | 11% | Guideline chưa rõ: Quy ước cờ v khi cổ tay bị che khuất bởi tay lái/tay áo (bạn gán v=2 theo bàn tay, bạn cùng nhóm gán v=1 do khớp bị che) |
| left_hip | 7% | 18% | 11% | Guideline chưa rõ: Người mặc đồ dài rộng (bạn gán v=2 ước lượng theo form người, bạn cùng nhóm gán v=1 vì không thấy mốc giải phẫu) |
| right_ear | 39% | 46% | 7% | Guideline chưa rõ: Mức độ tóc hoặc mũ che một phần tai để phân định giữa v=2 và v=1 |
| left_eye | 32% | 25% | 7% | Thao tác gán: Đánh giá góc nghiêng khuôn mặt khi mắt khuất sau sống mũi |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Cổ tay: Nếu nhìn thấy bàn tay/cẳng tay nhưng khớp cổ tay bị che khuất bởi tay lái xe hoặc gấu áo -> Bắt buộc chọn `v=1` và chấm điểm ước lượng tại vị trí giải phẫu; chỉ chọn `v=2` khi nhìn thấy rõ bề mặt khớp cổ tay.
- Hông: Với người mặc áo dài hoặc quần thụng che mất mốc xương hông -> Thống nhất chọn `v=1` và ước lượng tâm xoay mấu chuyển lớn xương đùi theo phương thẳng đứng từ vai xuống; không dùng `v=2` khi không có điểm mốc thị giác.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.820 | 0.835 | +0.015 |
| pose_mAP50-95 | 0.584 | 0.591 | +0.007 |
| pose_precision | 0.795 | 0.812 | +0.017 |
| pose_recall | 0.742 | 0.750 | +0.008 |
| box_mAP50-95 | 0.710 | 0.715 | +0.005 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   `pose_mAP50-95` tăng nhẹ khoảng +0.007 (+0.7%) hoặc thay đổi không đáng kể. Tập 20 ảnh train tập trung vào bối cảnh người lái xe máy và người đi đường đặc thù giao thông Việt Nam giúp model thích nghi tốt hơn với tư thế ngồi xe, nhưng do lượng ảnh quá nhỏ (20 ảnh) nên model có thể bị overfit nhẹ vào kiểu trang phục kín/áo chống nắng khiến dự đoán các tư thế tự do khác kém linh hoạt hơn.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   `box_mAP50-95` cao hơn `pose_mAP50-95` khoảng ~0.12 (71.5% so với 59.1%). Model tìm *người* (bounding box) dễ hơn nhiều so với tìm *khớp* (keypoints). Lý do: Bounding box chỉ cần bao quát hình bóng toàn thể (silhouette/texture) của con người, trong khi keypoint pose đòi hỏi xác định chính xác vị trí không gian của từng khớp nhỏ (như cổ tay, mắt cá chân) ngay cả khi bị che khuất hoặc thay đổi góc nhìn mạnh.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   Ở ảnh `test_03.jpg`, người điều khiển xe máy bị lỗi **lệch nhẹ** ở khớp cổ tay trái (`left_wrist`) do bị tay lái xe che khuất, và lỗi **trượt hẳn** ở khớp bàn chân do góc chụp che khuất gầm xe.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   Ảnh `test_06.jpg` có OKS thấp nhất giữa nhãn và model. Nhãn gán của người gán đúng hơn vì người gán quan sát được ngữ cảnh giải phẫu toàn thân và nếp gấp quần áo khi ngồi để suy luận tâm hông và đầu gối, trong khi model bị nhiễu bởi các hoa văn trên trang phục dẫn đến đặt trôi điểm keypoint ra ngoài thân người.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   Có, ảnh có nhiều người chồng lấn hoặc bị cắt mép mạnh (như người ngồi sau xe máy) vừa khiến người gán phân vân nhiều nhất, vừa là nơi model cho độ tin cậy thấp nhất. Điều này phản ánh bức ảnh có độ mơ hồ thị giác cao (high visual ambiguity), ranh giới cơ thể bị che lấp nặng (severe occlusion) và thiếu các đặc trưng trực quan rõ ràng.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
Trong ảnh `train_04.jpg`, người thứ 1 (người đang điều khiển xe máy), tôi phải quyết định trạng thái cờ cho khớp cổ tay phải `right_wrist`. Về bằng chứng thị giác, cẳng tay phải và một phần ngón tay nắm tay lái vẫn quan sát được rõ ràng, nhưng phần khớp nối cổ tay bị che khuất hoàn toàn bởi cụm gương và tay ga xe máy. Do khớp cổ tay chắc chắn vẫn nằm trọn trong giới hạn khung hình (không hề bị cắt ra ngoài mép ảnh), tôi chọn trạng thái `v=1` (Occluded) thay vì `v=0`. Tôi đặt điểm chấm ước lượng tại giao điểm giải phẫu tự nhiên giữa cẳng tay và bàn tay theo hướng cán ghi đông xe.

