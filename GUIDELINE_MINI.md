# Mini guideline - nhóm: Nhóm 5  |  người gán: Phạm Xuân Duy  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Ước lượng tâm mấu chuyển lớn xương đùi; gán `v=1` nếu áo dài/quần thụng che khuất mốc xương, chỉ gán `v=2` khi thấy rõ đường nét giải phẫu. | Hông không lộ bề mặt qua trang phục, cần suy luận giải phẫu từ trục cột sống và điểm gập đùi. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nhìn thấy một phần vành tai hoặc dái tai -> gán `v=2`; nếu bị tóc dày hoặc mũ bảo hiểm/quai mũ che kín -> gán `v=1` tại vị trí đối xứng qua đầu. | Phân biệt rõ giữa khớp nhìn thấy một phần và khớp bị che khuất hoàn toàn theo chuẩn COCO. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp ngoài mép ảnh (gối, cổ chân) bắt buộc gán `v=0` và không đặt chấm (tọa độ 0 0 0); các khớp còn trong ảnh gán `v=2` hoặc `v=1`. | Tuân thủ nghiêm ngặt quy tắc Outside (v=0), không đặt điểm ngoài khung hình tránh làm nhiễu loss khi huấn luyện. |
| Cổ tay nằm sau tay lái / sau thân mình | Gán `v=1` và chấm ước lượng tại vị trí giao khớp giữa cẳng tay và bàn tay dựa trên hướng tay lái/cánh tay. | Khớp vẫn còn trong khung hình nhưng bị che bởi vật cản (Occluded), cần biểu diễn tư thế hoàn chỉnh. |
| Hai người chồng lên nhau | Gán dứt điểm từng skeleton của từng người; khớp của người nào gán cho người đó; khớp bị người phía trước che gán `v=1`. | Tránh tuyệt đối lỗi nhầm người (`nham_nguoi`) và lỗi đảo trái/phải (`dao_trai_phai`). |
| Người nhỏ đến mức nào thì không gán nữa | Trong 20 ảnh core gán đủ mọi người; chỉ bỏ qua nếu chiều cao bounding box < 10% chiều cao ảnh hoặc không nhận diện được đầu - thân. | Đảm bảo độ bao phủ theo gold, tránh sinh nhãn rác khi không đủ độ phân giải. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_13.jpg`, người thứ `1`, khớp `left_shoulder`

- Mơ hồ ở chỗ nào: Người đứng ở rìa mép trái ảnh, nửa thân bị mép ảnh cắt ngang qua vai và đầu, khó phân định khớp nào còn trong khung (`v=1`) và khớp nào đã ra ngoài mép (`v=0`).
- Bạn quyết thế nào: Ban đầu bỏ sót skeleton này vì ngỡ là người rìa quá nhỏ; sau khi đối chiếu gold đã bổ sung skeleton với vai phải và mắt phải `v=2`, vai trái và chân ngoài mép gán `v=0`.
- Vì sao: Phần cơ thể còn trong ảnh vẫn nhận diện được pose người; khớp nào ngoài biên ảnh bắt buộc phải gán `v=0`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu bỏ qua không gán, model sẽ học bỏ sót người ở biên (false negative); nếu chấm bừa ra ngoài rìa ảnh với `v=1` thì model học dự đoán tọa độ ảo ngoài biên.

### Ca 2 - ảnh `train_04.jpg`, người thứ `1`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: Bàn tay phải cầm tay lái xe máy, phần khớp cổ tay bị che khuất một phần bởi cụm tay ga và gương xe, phân vân giữa nhìn thấy (`v=2`) và bị che (`v=1`).
- Bạn quyết thế nào: Quyết định gán `v=1`, đặt chấm ước lượng tại vị trí giao khớp giữa cẳng tay và bàn tay theo trục cẳng tay.
- Vì sao: Nếp gấp cổ tay thực tế bị che khuất bởi vật cản phía trước, việc ước lượng giải phẫu chính xác hơn là chấm lệch lên bề mặt tay lái.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gán `v=2` vào tay lái, model sẽ nhầm cụm tay lái là cổ tay người; nếu gán `v=0` sẽ làm mất điểm keypoint hợp lệ khi khớp vẫn ở trong ảnh.

### Ca 3 - ảnh `train_03.jpg`, người thứ `2`, khớp `left_hip`

- Mơ hồ ở chỗ nào: Người mặc áo phông thụng tối màu trùm qua hông và quần dài tối màu, không có điểm phân định khớp hông rõ ràng bằng mắt.
- Bạn quyết thế nào: Dựa vào trục cột sống và điểm uốn đùi khi đứng để ước lượng tâm mấu chuyển lớn xương đùi, gán `v=1`.
- Vì sao: Khớp hông là điểm neo trung tâm liên kết thân trên và chi dưới, cần giữ khoảng cách cân đối hai bên hông.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt sai lệch tọa độ hoặc gán cờ không nhất quán, mô hình sẽ dự đoán khung xương bị vẹo trục cơ thể hoặc co cụm hông khi gặp người mặc quần áo tối màu.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_wrist` (bạn `21%` / họ `32%`) và `left_hip` (bạn `7%` / họ `18%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline chưa rõ về tiêu chí phân định giữa `v=2` và `v=1` đối với cổ tay khi cầm vật thể và hông người mặc đồ dài. Bạn coi việc ước lượng theo form dáng là `v=2`, trong khi bạn cùng nhóm coi việc không nhìn thấy trực tiếp mốc xương giải phẫu là `v=1`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Với cổ tay bị che khuất bởi tay áo hoặc tay lái xe, thống nhất chọn `v=1` và ước lượng vị trí giải phẫu; với hông người mặc đồ thụng/dài che mất mốc xương, thống nhất gán `v=1` thay vì `v=2`.
