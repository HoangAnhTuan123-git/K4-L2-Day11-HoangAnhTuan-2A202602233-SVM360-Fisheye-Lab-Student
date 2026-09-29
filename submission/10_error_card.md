# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | MISSING | 1 |
| center | B4 | SPURIOUS | 9 |
| center | B4 | WRONG_CLASS | 1 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | SPURIOUS | 2 |
| edge | B4 | SPURIOUS | 1 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B4 | ATTRIBUTE | 2 |
| mid | B4 | BOX_GEOMETRY | 1 |
| mid | B4 | DUPLICATE | 1 |
| mid | B4 | MISSING | 5 |
| mid | B4 | SPURIOUS | 8 |
| mid | B4 | WRONG_CLASS | 2 |
| mid | C0 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 21 (ví dụ frame adasind_019560.jpg)
- MISSING: 6 (ví dụ frame adasind_265065.jpg)
- WRONG_CLASS: 4 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Phân tích tập trung vào cụm lỗi nổi bật nhất là **SPURIOUS** (21 ca) và **WRONG_CLASS** (4 ca) tại vùng `mid` và `center` trên frame `adasind_261480.jpg` và `adasind_265065.jpg`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy**:
  1. `E4_model_domain` (chiếm phần lớn ca SPURIOUS của mô hình M): Mô hình YOLO26m được huấn luyện chủ yếu trên ảnh phối cảnh pinhole thông thường, khi áp dụng trực tiếp lên camera fisheye góc rộng gặp hiện tượng lệch miền dữ liệu (domain shift) nghiêm trọng. Độ cong quang học phi tuyến tính ở vùng `mid` khiến mô hình nhầm các vệt bóng râm, dải phân cách và kết cấu nền tĩnh thành phương tiện giao thông (như các box `M2, M5, M8, M10, M12, M13` trong `adasind_261480.jpg`).
  2. `E1_annotator_error` (các ca WRONG_CLASS và thiếu thuộc tính): Phương tiện ba bánh (`ThreeWheeler` - auto rickshaw đặc trưng đường phố Ấn Độ) có kích thước cabin gần tương đương ô tô con (`Car`) khi nhìn từ góc xiên bị méo, dẫn đến việc người gán nhãn nhận diện sai class (`L1+R6` trên frame `adasind_261480.jpg`). Ngoài ra, người gán nhãn quên kích hoạt cờ `truncated=true` khi xe máy chạm viền ảnh bên trái (`L7`).
  3. `E0_reference_defect` và `E2_guideline_gap`: Ca `L1+M6` cả người và mô hình đều phát hiện một ô tô thực tế đang lưu thông nhưng reference lại bỏ sót; ca xe hai bánh bám sát nhau chưa có quy tắc rõ ràng trong guideline về khoảng cách tối thiểu để tách box.

- **Cách sửa và ai nhận việc (`owner`)**:
  - `annotator`: Rework toàn bộ các ca phân loại sai class trên frame slice cá nhân (sửa `Car` thành `ThreeWheeler`), bật cờ `truncated=true` cho vật thể sát viền ảnh (`R04`), loại bỏ các box vẽ thừa hoặc dưới ngưỡng 40 px.
  - `ai_team`: Thu thập thêm dữ liệu fisheye thực tế để tái huấn luyện mô hình; áp dụng kỹ thuật fisheye augmentation và điều chỉnh ngưỡng tin cậy (confidence threshold) theo từng zone (tăng ngưỡng ở zone `mid` và `edge` để dập tắt box rác).
  - `guideline`: Ban hành bản vá quy tắc (guideline patch) bổ sung hình ảnh trực quan nhận diện `ThreeWheeler` từ các góc chụp méo fisheye và làm rõ quy tắc tách xe hai bánh đi sát nhau.

- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule)**:
  - Dòng findings đối chứng: 
    - `r1_craft,B4-mid,adasind_261480.jpg,L1+R6,na,WRONG_CLASS`
    - `r2_qa,B4-mid,adasind_261480.jpg,L7,L_only,ATTRIBUTE` (Rule `R04`)
    - `r3_diag,B4-mid,adasind_261480.jpg,L1+M6,LM_noR,SPURIOUS` (Rule `R01`, evidence: cả người và model đều thấy)
    - `r3_diag,B4-mid,adasind_261480.jpg,M2,M_only,SPURIOUS` (Rule `R01`, model hallucination)
  - Quy tắc: Tuân thủ Rule `R01` (đúng 6 class động, ngưỡng $H \ge 40\text{ px}$), Rule `R04` (thuộc tính `truncated` bắt buộc khi chạm viền ảnh hoặc vòng kính), Rule `R08` (loại bỏ box trùng lặp).
  - Ảnh minh chứng: File ảnh chụp màn hình trong `submission/screenshots/screenshot_b4_mid_diagnosis.png` và `submission/screenshots/screenshot_b4_mid_rework.png`.

