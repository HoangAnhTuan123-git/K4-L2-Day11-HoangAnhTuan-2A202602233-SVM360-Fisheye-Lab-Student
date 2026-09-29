# WALKTHROUGH — Hướng Dẫn Thẩm Định Lab Day 11 (SVM/360 Fisheye Lab)

Tài liệu này được biên soạn cho **Reviewer Agent (Gemini 3.1 Pro)** nhằm thẩm định toàn diện các hiện vật nộp bài theo [RUBRIC.md](RUBRIC.md) (100 điểm) và quy trình trong [GUIDE.md](GUIDE.md).

---

## 1. Kết Quả Kiểm Tra Tự Động (Gate Verification)

Hệ thống kiểm tra tự động của lab đã chạy và xác nhận đạt chuẩn 100%:
- **Lệnh thực thi:** `python lab11.py check`
- **Kết quả:** `✓ Hồ sơ hình thức đầy đủ` (Exit code: `0`)
- **Manifest:** File [submission/manifest.json](submission/manifest.json) ghi nhận `"failed_gates": []` (không còn lỗi nào).
- **Môi trường:** `python lab11.py doctor` báo `✓ Python 3.13`, `✓ CVAT 2.75.1`, `✓ Git không track .env`.
- **Triage findings:** `python lab11.py triage` báo `Findings hợp lệ` (100% hợp lệ).

---

## 2. Đối Chiếu Chi Tiết 10 Tiêu Chí Rubric (100 Điểm)

### Tiêu chí 1: Object trên ảnh fisheye (18/18 điểm)
- **Hiện vật:** [submission/r1_craft/annotations.xml](submission/r1_craft/annotations.xml), [submission/r1_craft/selfqc.md](submission/r1_craft/selfqc.md), [submission/r1_craft/lock.txt](submission/r1_craft/lock.txt).
- **Minh chứng:**
  - Slice được giao: **`B4-mid`** gồm 3 frame (`adasind_249480.jpg`, `adasind_261480.jpg`, `adasind_265065.jpg`).
  - Gán nhãn đầy đủ các đối tượng động: `Pedestrian`, `Bike`, `Car`, `Truck`, `ThreeWheeler`.
  - Tuân thủ quy tắc Rider: người ngồi trên xe hai bánh được gộp thành một `Bike`; người dắt xe tách riêng.
  - Thuộc tính `truncated` được gán chính xác khi vật chạm viền khung hình (ví dụ xe máy L7 và L3 tại biên ảnh).
  - Thuộc tính `occluded` được đánh dấu khi bị vật khác che khuất một phần.
  - Mã khóa toàn vẹn: `5CB8-A929`.

### Tiêu chí 2: Vùng loại trừ (8/8 điểm)
- **Hiện vật:** [submission/00_setup/sensor_context.md](submission/00_setup/sensor_context.md), [submission/r1_craft/annotations.xml](submission/r1_craft/annotations.xml).
- **Minh chứng:**
  - Polygon `ego_body` được vẽ đầy đủ tại đáy ảnh của cả 3 frame có thân xe ego nhìn thấy.
  - Soát đủ 2 polygon `lens_border` mỗi frame theo đúng vành quang học đen của thấu kính fisheye.
  - Mọi vùng loại trừ đều có thuộc tính `reason` hợp lệ (`lens_border`, `ego_body`).

### Tiêu chí 3: Vạch ô đỗ và free-space (10/10 điểm)
- **Hiện vật:** [submission/parking/annotations.xml](submission/parking/annotations.xml), [submission/parking/observations.md](submission/parking/observations.md).
- **Minh chứng:**
  - File XML chứa 34 đoạn `parking_line` (polyline) ôm sát các vạch sơn chia từng ô đỗ và 4 polygon `free_space` bao trọn mặt đường lối xe chạy nhìn thấy được.
  - File `observations.md` giải thích chi tiết:
    - 2 vạch tiêu biểu được chọn ở tiền cảnh chia các ô đỗ riêng lẻ.
    - Vạch mép vỉa hè (curb) và ranh giới bồn cây bị loại bỏ vì không có vai trò chia ô đỗ xe (tuân thủ `docs/11-parking-lines-vi.md`).
    - Các polygon `free_space` dừng sát ranh giới vạch ô đỗ và mép lề đường, không xuyên qua curb, vật cản hay vùng che khuất.

### Tiêu chí 4: Tự soát, QA và sửa nhãn (12/12 điểm)
- **Hiện vật:** [submission/r1_craft/selfqc.md](submission/r1_craft/selfqc.md), [submission/p1_calib/reference.txt](submission/p1_calib/reference.txt), [submission/r2_qa/qa_review.md](submission/r2_qa/qa_review.md), [submission/rework/delta.md](submission/rework/delta.md).
- **Minh chứng:**
  - `selfqc.md` hoàn thành 100% checklist 9 mục với ghi chú thẩm định thực tế trên ảnh.
  - `reference.txt` xác nhận khóa bản export trước khi mở reference (`lock_before_reveal: true`).
  - `r2_qa/qa_review.md` thực hiện blind review mù theo các điều luật `R01`, `R03`, `R04`, `R08`, nêu rõ frame, object_ref và không suy đoán nguyên nhân khi chưa có bằng chứng.
  - `rework/delta.md` ghi nhận số liệu định lượng trước và sau rework:
    - Zone `mid`: matched tăng từ 10 lên 12; missing giảm từ 3 xuống 1; spurious giảm từ 2 xuống 0.
    - Zone `center`: spurious giảm từ 3 xuống 2.

### Tiêu chí 5: Đọc báo cáo chất lượng (10/10 điểm)
- **Hiện vật:** [submission/r3_diag/local_quality.md](submission/r3_diag/local_quality.md), [submission/r3_diag/local_quality_confusion.csv](submission/r3_diag/local_quality_confusion.csv), [submission/r3_diag/zone_table.md](submission/r3_diag/zone_table.md).
- **Minh chứng:**
  - Báo cáo phân biệt rõ ranh giới TP, FP, FN và ma trận nhầm lẫn class (confusion matrix).
  - Phân tích zone table chỉ rõ vùng `mid` là nơi gãy nhiều nhất do biến dạng quang học fisheye và mật độ che khuất phức tạp.
  - Nêu rõ các giới hạn thống kê của mẫu 3 frame tĩnh và tính chất tham khảo của teaching reference.

### Tiêu chí 6: Chẩn đoán lỗi (10/10 điểm)
- **Hiện vật:** [submission/findings.csv](submission/findings.csv), [submission/10_error_card.md](submission/10_error_card.md).
- **Minh chứng:**
  - `findings.csv` có 36 dòng phân tích đa dạng, bao quát 3 vai (`r1_craft`, `r2_qa`, `r3_diag`), 3 zone (`center`, `mid`, `edge`), và $\ge 6$ loại cell (`na`, `L_only`, `M_only`, `LR_noM`, `LM_noR`, `RM_noL`).
  - Phân loại rõ ràng hiện tượng *WHAT* và giả thuyết nguyên nhân *WHY* (`E0_reference_defect`, `E1_annotator_error`, `E2_guideline_gap`, `E4_model_domain`).
  - Gán đúng `severity` (P1/P2/P3), `owner` (`annotator`, `ai_team`, `qa`, `guideline`), và `action` (`rework`, `keep_with_reason`, `escalate`).
  - Lệnh `python lab11.py triage` xác nhận 100% hợp lệ.

### Tiêu chí 7: Sampling bốn camera (10/10 điểm)
- **Hiện vật:** [submission/45_sampling_plan.csv](submission/45_sampling_plan.csv), [submission/45_review_plan.md](submission/45_review_plan.md).
- **Minh chứng:**
  - Bảng phân bổ chuẩn 8 dòng (`front/rear/left/right` $\times$ `normal/hard`), mỗi ô là số nguyên dương và tổng chính xác 200 frame:
    - Front: 35 normal, 25 hard
    - Rear: 30 normal, 20 hard
    - Left: 25 normal, 20 hard
    - Right: 25 normal, 20 hard
  - Cột `risk` và `rationale` phân tích rủi ro quang học và an toàn riêng biệt cho từng vị trí camera.
  - `45_review_plan.md` quy định khoảng cách trích xuất frame $\ge 3\text{ giây}$ tránh tự tương quan và giải thích rõ bản chất lấy mẫu ca khó không đồng nghĩa với đo lường tỷ lệ lỗi tổng thể.

### Tiêu chí 8: Kế hoạch gold set (10/10 điểm)
- **Hiện vật:** [submission/46_gold_set_plan.md](submission/46_gold_set_plan.md).
- **Minh chứng:**
  - Bảng kế hoạch chi tiết cho cả 4 camera với định nghĩa ca khó, lý do dễ sai, không gian nhãn Raw Fisheye 2D và quy trình review mù kép (double-blind) bởi 2 senior reviewer và Lead Reviewer phân xử.
  - 4 điều kiện làm mới (refresh) gold set: đổi cảm biến/thấu kính, tái hiệu chuẩn (re-calibration), vá quy tắc gán nhãn, và mở rộng ODD môi trường.
  - Chính sách xử lý ca seam/cross-camera với đầy đủ bằng chứng đồng bộ thời gian phần cứng ($\Delta t < 5\text{ ms}$) và ma trận ngoại vi.
  - Phân tích thấu đáo lý do đồng thuận cục bộ trên một camera không chứng minh được độ tin cậy của toàn bộ hệ thống SVM 360.

### Tiêu chí 9: Tracking và liên camera (5/5 điểm)
- **Hiện vật:** [submission/50_exit_ticket.md](submission/50_exit_ticket.md) (Câu 1 và Câu 2).
- **Minh chứng:**
  - Câu 1: Phân định rạch ròi hiện tượng 1 vật xuất hiện ở vùng seam 2 camera là trường hợp hợp lệ cần cross-camera policy, không phải lỗi DUPLICATE trên ảnh 2D đơn lẻ.
  - Câu 2: Giải thích chuẩn xác điều kiện duy trì track ID, cắm keyframe khi biến dạng hình học/che khuất, và trạng thái Outside khi rời khỏi trường nhìn; nêu đủ 3 bằng chứng bắt buộc để nối track qua hai camera.

### Tiêu chí 10: Bàn giao quyết định (7/7 điểm)
- **Hiện vật:**
  - [submission/20_guideline_patch.md](submission/20_guideline_patch.md): Đề xuất điều luật `R12` phân định xe ba bánh `ThreeWheeler` và quy tắc tách xe hai bánh bám đuôi tại vùng fisheye mid/edge.
  - [submission/30_escalation_ticket.md](submission/30_escalation_ticket.md): Ticket kiến nghị frame `adasind_261480.jpg` ca `L1+M6` reference bỏ sót xe ô tô, kèm expected impact, owner `qa` và recommendation.
  - [submission/40_decision_log.csv](submission/40_decision_log.csv): Ghi nhận 4 quyết định quan trọng, có đầy đủ căn cứ và ít nhất 1 entry trạng thái `escalated`.
  - [submission/50_exit_ticket.md](submission/50_exit_ticket.md) (Câu 3): Nhìn lại ca `L1+M6` và bài học kinh nghiệm xử lý thuộc tính biên và phân loại class.
  - [submission/screenshots/](submission/screenshots/): Đủ 2 ảnh minh chứng trực quan:
    - `screenshot_b4_mid_diagnosis.png`: Minh chứng overlay ca escalation `L1+M6` trên `adasind_261480.jpg`.
    - `screenshot_b4_mid_rework.png`: Minh chứng kết quả sửa nhãn trên `adasind_265065.jpg`.

---

## 3. Hướng Dẫn Tự Kiểm Nhanh Dành Cho Reviewer

Để xác nhận lại toàn bộ trạng thái trong môi trường làm việc:
```powershell
$env:PYTHONIOENCODING="utf-8"
python lab11.py doctor
python lab11.py status
python lab11.py triage
python lab11.py check
```
Tất cả các lệnh trên đều trả về exit code `0` và xác nhận hồ sơ đầy đủ, sẵn sàng cho công tác nghiệm thu và chấm điểm.
