# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Phạm Xuân Duy-2A202602093   Nhóm: G01-T006  Ngày: 16/09/2026

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

Bạn cùng nhóm: Phạm Hữu Hải-02098

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
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

*(Ghi chú bổ sung từ file JSON: `box_mAP50` giảm từ 0.9785 xuống 0.9600, chênh -0.0185).*

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   - **Mức thay đổi**: `pose_mAP50-95` tăng từ **0.6853** lên **0.6908**, tức tăng **+0.0055** (+0.55 điểm phần trăm). `pose_precision` tăng từ **0.9734** lên **0.9792** (+0.0058), trong khi `pose_mAP50` và `pose_recall` giữ nguyên ở mức **0.8450** và **0.8462**.
   - **Giải thích**:
     - *Dạy được điều gì mà COCO chưa dạy*: Tập 20 ảnh train phản ánh bối cảnh giao thông đường phố thực tế với các tư thế ngồi điều khiển xe máy, người ngồi sau, và trang phục che chắn đặc thù (áo khoác rộng, mũ bảo hiểm che tai, bàn tay nắm ghi đông xe). Với nhãn gán chuẩn theo cờ `v=1` khi bị che (không có lỗi đảo trái/phải, vị trí khớp ước lượng giải phẫu chính xác), mô hình học được phân bố không gian và định vị khớp tốt hơn khi gặp các trường hợp bị che khuất (occlusion), giúp tăng độ chính xác vị trí (`precision` tăng +0.0058 và `pose_mAP50-95` tăng nhẹ).
     - *Làm hỏng điều gì*: Mặc dù pose mAP tăng nhẹ, chỉ số phát hiện người tổng quát `box_mAP50-95` lại giảm nhẹ **-0.0078** (từ 0.8119 xuống 0.8041) và `box_mAP50` giảm **-0.0185** (từ 0.9785 xuống 0.9600). Do kích thước tập dữ liệu fine-tune quá nhỏ (chỉ 20 ảnh), mô hình xuất hiện hiện tượng quên cục bộ (catastrophic forgetting nhẹ) ở nhánh bounding box detection, khiến việc bao quát các hộp bao người ngoài góc nhìn xe máy bị suy giảm nhẹ so với pre-trained gốc được huấn luyện trên hàng trăm nghìn ảnh COCO.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   - **Độ chênh lệch**:
     - Ở model gốc (baseline): `box_mAP50-95` là **0.8119**, `pose_mAP50-95` là **0.6853** $\rightarrow$ chênh nhau **0.1266** (box cao hơn pose 12.66 điểm phần trăm). `box_mAP50` (0.9785) cao hơn `pose_mAP50` (0.8450) tới **0.1335** (13.35%).
     - Sau fine-tune: `box_mAP50-95` là **0.8041**, `pose_mAP50-95` là **0.6908** $\rightarrow$ chênh nhau **0.1133** (11.33 điểm phần trăm).
   - **Kết luận**: Model tìm *người* (bounding box) dễ hơn rất nhiều so với tìm *khớp* (keypoints).
   - **Vì sao**: Bounding box chỉ cần bắt được vùng bao chứa các đặc trưng nhận dạng tổng thể của cơ thể (silhouette, diện mạo người, tương phản nền). Ngược lại, tìm khớp đòi hỏi xác định tọa độ cục bộ chính xác tuyệt đối của 17 điểm giải phẫu nhỏ. Trong thực tế, các khớp thường xuyên bị che khuất (cổ tay sau tay lái xe, hông sau trang phục thụng, tai sau mũ bảo hiểm) hoặc có góc gập phức tạp. Công thức OKS chấm điểm với bán kính dung sai khắt khe cho từng khớp ($2\sigma\sqrt{A}$); chỉ cần vài khớp bị che khuất hoặc lệch vị trí vài pixel là OKS giảm mạnh, khiến `pose_mAP` luôn khó đạt điểm cao hơn `box_mAP`.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - Quan sát ảnh `test_03.jpg` (ảnh hai người cùng ngồi trên một xe máy):
     - Người lái xe (Person 1): Khớp cổ tay phải (`right_wrist`) bị lỗi **lệch nhẹ** do model đặt chấm lên tay nắm ghi đông xe máy thay vì tâm khớp giải phẫu bị khuất sau tay ga; khớp cổ chân phải (`right_ankle`) bị lỗi **trượt hẳn** do gầm xe và bô xe che khuất hoàn toàn, model dự đoán trôi hẳn ra ngoài mặt đường.
     - Người ngồi sau (Person 2): Bị lỗi **nhầm người** ở khớp đầu gối trái (`left_knee`) và cổ chân trái (`left_ankle`), do hai người ngồi sát nhau nên xương chân người ngồi sau bị model bắt nhầm kéo sang phần đùi và thân xe của người lái phía trước.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   - **Ảnh có OKS thấp nhất**: Ảnh `train_13.jpg` (và `train_11.jpg` xét trên từng skeleton riêng lẻ). Tại `train_13.jpg`, có sự bất đồng lớn nhất khi người thứ 1 đứng ở mép ngoài cùng bên trái ảnh bị cắt ngang thân, dẫn đến số người phát hiện bị lệch (`model 2 / bạn 2` trong khi gold có 3 người, hoặc model bỏ sót và đoán sai hoàn toàn pose của người ở mép). Ở `train_11.jpg`, OKS giữa nhãn và model chỉ đạt ~0.84.
   - **Ai đúng**: **Nhãn của bạn (người gán) đúng hơn model**.
   - **Căn cứ**: Dựa vào đối chiếu thị giác trực tiếp với ảnh gốc và kết quả chấm với Gold (`outputs/eval_vs_gold.json` đạt OKS 0.923). Người gán nắm vững tri thức giải phẫu và tuân thủ đúng quy tắc cờ (`v=1` cho khớp bị che khuất trong khung và `v=0` cho khớp ra ngoài mép ảnh). Trong khi đó, model chỉ dựa vào phân bố pixel thống kê cục bộ nên khi gặp các vùng bị che khuất nặng hoặc người bị cắt biên, model dễ bị đánh lừa bởi họa tiết quần áo, bóng đổ hoặc đặt điểm trôi ra ngoài biên cơ thể.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   - **Trả lời**: **Có**. Trong `outputs/eval_vs_gold.json`, ảnh `train_13.jpg` là ảnh có điểm OKS trung bình thấp nhất (0.6188 do bị sót 1 người ở mép trái), và đối với người đơn lẻ là `train_11.jpg` (OKS = 0.8416). Đây cũng chính là những ảnh mà model đối chiếu cho điểm OKS thấp nhất và có sự bất đồng lớn nhất về số lượng người cũng như vị trí khớp.
   - **Điều đó nói gì về bức ảnh đó**:
     - Bức ảnh đó thuộc nhóm **ca biên phức tạp (edge cases) có độ mơ hồ thị giác cực cao (high visual ambiguity)**.
     - *Bị cắt mép nghiêm trọng (truncation)*: Đối tượng đứng sát mép khung hình bị cắt một phần cơ thể, khiến ranh giới xác định một cá thể người trở nên không rõ ràng cho cả người gán lẫn mạng nơ-ron phát hiện đối tượng.
     - *Che khuất nặng nề (heavy occlusion)*: Bối cảnh có nhiều vật cản, góc nhìn nghiêng gắt và trang phục tối màu làm biến mất các điểm mốc giải phẫu thị giác chuẩn, đòi hỏi việc suy luận không gian giải phẫu mức cao mà model hiện tại với 20 ảnh fine-tune chưa thể khái quát hóa hoàn hảo.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
Trong ảnh `train_04.jpg`, người thứ 1 (người đang điều khiển xe máy), tôi phải quyết định trạng thái cờ cho khớp cổ tay phải `right_wrist`. Về bằng chứng thị giác, cẳng tay phải và một phần ngón tay nắm tay lái vẫn quan sát được rõ ràng, nhưng phần khớp nối cổ tay bị che khuất hoàn toàn bởi cụm gương và tay ga xe máy. Do khớp cổ tay chắc chắn vẫn nằm trọn trong giới hạn khung hình (không hề bị cắt ra ngoài mép ảnh), tôi chọn trạng thái `v=1` (Occluded) thay vì `v=0`. Tôi đặt điểm chấm ước lượng tại giao điểm giải phẫu tự nhiên giữa cẳng tay và bàn tay theo hướng cán ghi đông xe.

