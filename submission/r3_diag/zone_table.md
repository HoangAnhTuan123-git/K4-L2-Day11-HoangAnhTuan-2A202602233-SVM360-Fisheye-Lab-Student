# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 3 | 1 | 4 | SPURIOUS (2) |
| mid | 13 | 3 | 2 | 3 | 7 | WRONG_CLASS (2) |
| edge | 0 | 0 | 0 | 0 | 1 | — |

## Nhận xét

- **Zone người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên**:
  - Cả người gán nhãn (L) và mô hình (M) đều gãy nhiều nhất ở vùng **`mid`**:
    - Về phía L: vùng `mid` ghi nhận 3 ca `L missing` và 2 ca `L spurious`, lỗi chính tập trung ở `WRONG_CLASS` (2 ca phân loại nhầm giữa Car/ThreeWheeler/Truck và Bike/Pedestrian) và `SPURIOUS`.
    - Về phía M: vùng `mid` là nơi mô hình gãy nặng nề nhất với 3 ca `M missing` (`LR_noM` + `R_only`) và tới 7 ca `M thừa` (`LM_noR` + `M_only`), chiếm đa số tổng số lỗi của mô hình.
    - Ở vùng `center`: L phát hiện đủ đối tượng (`L missing = 0`) nhưng có 3 ca `L spurious`; M có 1 ca missing và thừa 4 ca spurious.
    - Ở vùng `edge`: Không có đối tượng chuẩn (`n_ref = 0`), L kiểm soát tốt không vẽ box thừa nào, trong khi M bị 1 box ảo do nhiễu vành đen quang học.

- **Giả thuyết nguyên nhân và giới hạn của slice ba frame**:
  - *Biến dạng quang học (barrel distortion)*: Vùng `mid` là dải chuyển tiếp có độ cong quang học fisheye tăng nhanh. Mô hình YOLO26m vốn được huấn luyện chủ yếu trên ảnh phẳng thông thường nên gặp hiện tượng domain shift mạnh, nhận diện sai biên dạng và gán thừa box rác.
  - *Che khuất phức tạp và mật độ phương tiện*: Trong ảnh đường phố Ấn Độ ở slice B4-mid, xe hai bánh, người đi bộ và xe ba bánh bám sát nhau. Ranh giới giữa rider ngồi trên xe (`Bike`) và người đi bộ tách rời (`Pedestrian`) dễ gây nhầm lẫn phân loại và sai lệch kích thước box.
  - *Giới hạn của slice 3 frame*: Tập dữ liệu chỉ có 3 frame với tổng cộng 17 đối tượng reference ($n_{\text{ref}} = 17$), số lượng mẫu thống kê còn nhỏ và hoàn toàn thiếu đối tượng ở vùng `edge` ($n_{\text{ref}} = 0$). Dù vậy, dữ liệu này chỉ ra rõ ràng rằng khâu hậu kiểm và đào tạo mô hình cần tập trung giải quyết vùng `mid` nơi xảy ra phần lớn các ca bất đồng.

