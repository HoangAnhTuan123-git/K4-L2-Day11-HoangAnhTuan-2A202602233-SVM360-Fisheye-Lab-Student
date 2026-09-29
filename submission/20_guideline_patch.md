# Guideline patch

- **Rule mới đề xuất:** `R12 — Phân định ThreeWheeler và quy tắc tách xe hai bánh bám đuôi (tailgating bikes) tại vùng fisheye mid/edge`:
  1. *Phân định xe ba bánh (ThreeWheeler):* Mọi phương tiện có kết cấu 3 bánh (như auto-rickshaw, xe lam, xe lôi cơ giới), kể cả khi có mui bạt che kín cabin hoặc kích thước tương đương ô tô nhỏ, bắt buộc phải gán nhãn `ThreeWheeler`, tuyệt đối không gán nhãn `Car` hoặc `SpecialVehicle`.
  2. *Quy tắc tách xe bám đuôi (tailgating bikes):* Khi hai xe hai bánh di chuyển nối đuôi nhau với khoảng cách hẹp, nếu quan sát thấy khoảng sáng mặt đường giữa hai xe hoặc nhận diện được hai người lái (rider) riêng biệt, bắt buộc phải vẽ 2 box `Bike` độc lập ôm sát từng xe. Không gộp thành một box lớn và không vẽ 2 box đè lên nhau vượt quá IoU 0.4 nếu không chứng minh được có 2 phương tiện song hành.
- **Áp dụng cho:** Class `ThreeWheeler`, `Bike`, `Car`; đặc biệt tại vùng bán kính `mid` và `edge` trên camera fisheye ADASIND và hệ thống SVM.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Phiên bản hiện tại v1.0.0 chỉ có quy tắc `R02` về gộp người lái vào xe hai bánh, thiếu chỉ dẫn phân biệt hình thái học cho dòng xe 3 bánh đặc thù đô thị đang phát triển khi bị biến dạng quang học dạng cong (như ca nhầm lẫn `L1+R6` trên frame `adasind_261480.jpg`), đồng thời chưa có tiêu chí định lượng giải quyết tranh chấp gộp/tách xe bám đuôi (`L8+M11`).
- **`rules_version` mới:** `v1.1.0`
- **Hiệu lực từ:** Vòng `rework` (P5) và bắt buộc cho các đợt gắn nhãn toàn diện tiếp theo.

