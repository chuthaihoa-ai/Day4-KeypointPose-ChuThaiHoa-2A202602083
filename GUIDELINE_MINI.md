# Mini guideline - nhóm: ______  |  người gán: Chu Thái Hòa  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Đặt điểm hông theo vị trí ước lượng của khớp hông, tại vùng nối giữa thân và đùi, không đặt theo mép quần áo. Nếu khớp còn trong ảnh thì gán `v = 1`.<br><br>Ảnh mẫu: ![train_01](dataset/images/train/train_01.jpg) | Hông là khớp giải phẫu, không phải vị trí của mép quần áo. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đặt điểm tai tại vị trí ước lượng của tai. Nếu tai còn trong khung ảnh dù bị che một phần thì gán `v = 1`.<br><br>Ảnh mẫu: ![train_04](dataset/images/train/train_04.jpg) | Bị che khuất không đồng nghĩa với nằm ngoài ảnh. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài khung ảnh gán `v = 0` và không đặt chấm. Các khớp còn trong ảnh vẫn phải gán đầy đủ.<br><br>Ảnh mẫu: ![train_10](dataset/images/train/train_10.jpg) | Cần phân biệt khớp bị cắt khỏi ảnh với khớp bị che khuất. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí cổ tay dựa trên hướng của cẳng tay và bàn tay; nếu cổ tay còn trong ảnh thì đặt chấm và gán `v = 1`.<br><br>Ảnh mẫu: ![train_12](dataset/images/train/train_12.jpg) | Vị trí khớp vẫn có thể suy luận khi bị vật khác che. |
| Hai người chồng lên nhau | Gán đủ 17 điểm riêng cho từng người. Điểm bị người kia che nhưng còn trong ảnh được đặt theo vị trí ước lượng và gán `v = 1`; không gán nhầm điểm giữa hai người.<br><br>Ảnh mẫu: ![train_03](dataset/images/train/train_03.jpg) | Mỗi người cần một bộ keypoint riêng và phải giữ trái/phải theo cơ thể người đó. |
| Người nhỏ đến mức nào thì không gán nữa | Vẫn gán khi còn xác định được người và có thể ước lượng các khớp. Chỉ không gán khi người quá nhỏ hoặc không đủ thông tin để xác định đúng người và vị trí khớp.<br><br>Ảnh mẫu: ![train_07](dataset/images/train/train_07.jpg) | Tránh bỏ sót người nhưng không tạo nhãn đoán mò. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ `1`, khớp `right_eye` và `left_eye`

- Mơ hồ ở chỗ nào: Người thứ nhất đội mũ bảo hiểm kín mặt, nên không nhìn thấy rõ hai mắt và khó xác định chính xác vị trí mắt.
- Bạn quyết thế nào: Đặt hai điểm mắt tại vị trí ước lượng dựa trên hướng của đầu và các chi tiết còn nhìn thấy trên mũ; vì người vẫn nằm trong khung ảnh nên gán `v = 1`.
- Vì sao: Mũ che khuất mắt nhưng mắt vẫn là khớp nằm trong ảnh, không phải khớp đã bị cắt khỏi ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gán `v = 0` hoặc xoá điểm, model sẽ học rằng người đội mũ không có mắt và giảm khả năng dự đoán keypoint khi khuôn mặt bị che.

### Ca 2 - ảnh `train_12.jpg`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay trái nằm gần tay lái và bị thân xe cùng tay che một phần, nên ranh giới giữa cổ tay và bàn tay không rõ.
- Bạn quyết thế nào: Đặt `left_wrist` tại vị trí nối ước lượng giữa cẳng tay và bàn tay trên tay lái, rồi gán `v = 1` vì vị trí vẫn nằm trong khung ảnh.
- Vì sao: Điểm bị vật thể che một phần vẫn cần được ước lượng và giữ lại theo luật chung của lớp.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt điểm vào bàn tay hoặc bỏ điểm, model sẽ học sai vị trí cổ tay và không ổn định khi nhận diện người đang điều khiển xe.

### Ca 3 - ảnh `train_03.jpg`, người thứ `2`, khớp `left_shoulder`

- Mơ hồ ở chỗ nào: Hai người đứng sát và chồng lên nhau; vai trái của người thứ hai bị người thứ nhất che một phần, nên dễ gán nhầm vai của hai người.
- Bạn quyết thế nào: Xác định người thứ hai trước, sau đó đặt `left_shoulder` theo đường nối từ cổ đến khuỷu tay của chính người đó; điểm còn trong ảnh nên gán `v = 1`.
- Vì sao: Mỗi người phải có bộ 17 điểm riêng và trái/phải được tính theo cơ thể của người đó, không theo vị trí trên ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu lấy vai của người thứ nhất làm vai người thứ hai, model sẽ học sai liên kết thân người và làm nhầm keypoint khi có nhiều người đứng gần nhau.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: Chưa xác định vì chưa có visibility report của bạn cùng nhóm để đối chiếu. Trong report của tôi, `left_ear` có `%v=1` cao nhất: `62%`.
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Chưa đủ dữ liệu để kết luận chênh lệch giữa hai người. Qua report của tôi, nguyên nhân cần kiểm tra trước là guideline về tai bị tóc hoặc mũ che; không nên tự kết luận một bên gán sai khi chưa so cùng ảnh và cùng người.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Tai bị tóc hoặc mũ bảo hiểm che nhưng vẫn còn trong khung ảnh thì đặt điểm theo vị trí giải phẫu ước lượng và gán `v = 1`; chỉ gán `v = 0` khi tai nằm ngoài mép ảnh hoặc không còn căn cứ nào để xác định vị trí.
