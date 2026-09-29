# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `Slice B4-mid` / frame `adasind_261480.jpg` | 8 ca: WRONG_CLASS (ThreeWheeler nhầm thành Car), ATTRIBUTE (thiếu truncated khi chạm viền), DUPLICATE (hai xe máy bám sát), và SPURIOUS của model | Mật độ phương tiện hỗn hợp dày đặc; nhầm lẫn class giữa ThreeWheeler và Car ảnh hưởng trực tiếp đến ước lượng kích thước vật thể và an toàn điều khiển ADAS | Ảnh chụp overlay `screenshot_b4_mid_diagnosis.png`, mã khóa XML `5CB8-A929`, và các dòng findings `L1+R6`, `L7`, `L8` |
| `Slice B4-mid` / frame `adasind_265065.jpg` | 6 ca: MISSING (bỏ sót Pedestrian ở rìa phải), SPURIOUS (vẽ thừa box người và xe nhỏ dưới ngưỡng H40), WRONG_CLASS (Truck/Car) | Rủi ro an toàn va chạm với người đi bộ (VRU); cần rà soát nghiêm ngặt ngưỡng kích thước tối thiểu H=40 px để loại bỏ box rác | Ảnh so sánh sau rework trong `rework/delta.md`, dòng findings `R7 MISSING`, `L5/L6 SPURIOUS` |

- **Giới hạn của kết luận từ ba frame ADASIND:**
  Bộ dữ liệu thực hành chỉ gồm 3 frame tĩnh trích đoạn từ một camera đơn duy nhất hướng trước của ADASIND, không thể đại diện cho toàn bộ các tình huống giao thông đô thị, và hoàn toàn không phản ánh được góc nhìn, điểm mù hay đặc tính quang học của camera sau và hai camera sườn trong hệ thống SVM 360.

## Chuyển sang kế hoạch bốn camera giả lập

- **Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`:**
  - Thực hiện lấy mẫu phân tầng (stratified sampling) trên cả 4 camera (`front`, `rear`, `left`, `right`) chia đều cho 2 lát cắt `normal` và `hard`.
  - Để tránh thiên lệch do chọn nhiều frame liền kề trong cùng một video clip (temporal autocorrelation), quy định khoảng cách tối thiểu giữa hai frame được trích xuất là $\ge 3\text{ giây}$ (tương đương cách nhau ít nhất 90 frame ở tốc độ ghi hình 30 fps), đảm bảo đa dạng về điều kiện ánh sáng (ngược nắng, bóng râm, ban đêm), thời tiết và mật độ giao thông.

- **Vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:**
  - Kế hoạch lấy mẫu 200 frame có chủ đích tập trung một tỷ lệ rất cao vào các ca khó (`hard slice`) để truy tìm các trường hợp biên nguy hiểm (edge cases) và kiểm tra độ bền vững của quy tắc/mô hình.
  - Do tỷ lệ phân bố ca khó trong mẫu 200 frame cao hơn rất nhiều so với tần suất xuất hiện tự nhiên trong tập 50.000 frame ngoài thực tế, tỷ lệ lỗi đo được trên tập này chỉ phản ánh các điểm mù cục bộ cần đào tạo lại hoặc sửa luật, tuyệt đối không được coi là tỷ lệ lỗi trung bình hay chỉ số năng lực của toàn bộ hệ thống SVM khi vận hành thực tế.

