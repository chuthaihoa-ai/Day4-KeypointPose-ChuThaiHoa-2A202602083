# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Chu Thái Hòa   Nhóm: ______   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 314 / 131 / 48 |
| Thời gian trung bình mỗi ảnh | 5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 62%
2. `right_ear` - 48%
3. `left_eye` - 38%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Các khớp này có tỷ lệ bị che cao nhất, nhưng không hoàn toàn trùng với các khớp khó xác định vị trí giải phẫu nhất. Tai thường bị tóc hoặc mũ bảo hiểm che, nên khó quyết định vị trí và cờ visibility; điều này thể hiện qua `left_ear` có 18 lần `v=1` trên tổng 29 lần xuất hiện. Mắt trái cũng thường bị che nhưng vị trí có thể ước lượng từ hướng đầu và các điểm lân cận. Trong thực tế, cổ tay và hông cũng khó gán ở các ảnh bị che bởi tay lái, thân người hoặc quần áo, dù tỷ lệ `%v=1` của chúng không nằm trong ba vị trí cao nhất.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | Chưa có dữ liệu | 0.9169 |
| OKS@0.50 | Chưa có dữ liệu | 1.0 |
| OKS@0.75 | Chưa có dữ liệu | 0.9655 |
| Lỗi `dao_trai_phai` | Chưa có dữ liệu | 0 |
| Lỗi `nham_nguoi` | Chưa có dữ liệu | 0 |
| Lỗi `xoa_khop_bi_che` | Chưa có dữ liệu | 5 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- Trong kết quả hiện tại, `train_10.jpg`, người 1: `left_hip` và `right_hip` đang là `v=0` nhưng gold yêu cầu `v=1`; cần đặt lại hai điểm ở vị trí hông ước lượng và gán `v=1`.
- Trong kết quả hiện tại, `train_11.jpg`, người 1: `left_hip` và `right_hip` đang là `v=0` nhưng gold yêu cầu `v=1`; cần đặt lại hai điểm ở vị trí hông ước lượng và gán `v=1`.
- Trong kết quả hiện tại, `train_14.jpg`, người 2: `right_ankle` đang là `v=0` nhưng gold yêu cầu `v=1`; cần đặt lại điểm mắt cá phải ở vị trí ước lượng và gán `v=1`.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Không có lỗi `dao_trai_phai` trong kết quả hiện tại `outputs/eval_vs_gold.json`, nên không có ảnh để phân tích lỗi này. Cột trước rework chưa có dữ liệu riêng.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

-

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng `+0.0055`, từ `0.6853` lên `0.6908`. Điều này cho thấy 20 ảnh giúp model định vị keypoint tốt hơn một chút trên tập test, nhưng mức tăng nhỏ; đồng thời `box_mAP50-95` giảm `0.0078`, nên khả năng định vị bounding box bị giảm nhẹ.

2. Sau fine-tune, `box_mAP50-95` là `0.8041` và `pose_mAP50-95` là `0.6908`, chênh `0.1133`. Model tìm người dễ hơn tìm đúng các khớp, vì bounding box chỉ cần bao đúng cơ thể còn pose phải định vị đủ 17 điểm, kể cả các điểm bị che hoặc chồng lấp.

3. Ở `train_10`, model phát hiện `2` người trong khi nhãn có `1` người; đây là lỗi **nhầm người/đếm sai người** (dự đoán thêm một người). Cần mở ảnh dự đoán để xác định box thừa thuộc nền hay bị tách nhầm từ cùng một người.

4. Ảnh có OKS thấp nhất giữa model và nhãn là `train_15`, `OKS = 0.556`. Khi đối chiếu gold, ảnh này có skeleton tương ứng `OKS = 0.7751`, nên nhãn của tôi có căn cứ tốt hơn model ở ảnh này; tuy nhiên vẫn cần xem ảnh visualize để xác nhận từng keypoint.

5. Ảnh tôi gán tệ nhất là `train_11`, người 1, với `OKS = 0.7406` so với gold; ảnh model bất đồng nhiều nhất với nhãn là `train_15`, `OKS = 0.556`, nên hai ảnh không trùng nhau. `train_11` có lỗi xóa `left_hip` và `right_hip` bị che (`v=0` thay vì `v=1`), còn `train_15` là ảnh model khó khớp với nhãn dù nhãn vẫn đạt `OKS = 0.7751` với gold.

## 5. Một rule evidence bạn đã dùng

Trong ảnh `train_11.jpg`, người thứ 1, khớp `left_hip` bị quần áo che nên không nhìn thấy rõ bề mặt khớp. Tuy nhiên thân người vẫn nằm trong khung ảnh và có thể suy ra vị trí hông từ đường nối giữa vai, thân và chân. Vì khớp bị che nhưng vẫn còn trong ảnh, tôi chọn đặt điểm tại vị trí hông ước lượng và gán `v=1`, không gán `v=0`. Kết quả gold cũng xác nhận điểm này cần `v=1`; nhãn hiện tại đã bị đánh dấu `v=0` và cần rework.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
