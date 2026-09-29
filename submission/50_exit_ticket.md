# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. **Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?**
   - Đây **không phải là lỗi DUPLICATE** mà là **trường hợp hợp lệ cần một quy tắc xử lý riêng (cross-camera seam policy)**.
   - *Vì sao:* Vùng seam là khu vực chồng lấn trường nhìn (FOV overlap) của hai cảm biến vật lý độc lập (ví dụ camera Trước và camera Sườn Trái). Tại vùng giáp ranh này, cùng một vật thể thực nhưng từ hai góc nhìn khác nhau sẽ có cự ly, góc chiếu, mức độ che khuất và độ biến dạng thấu kính fisheye hoàn toàn khác nhau (ở camera Trước vật thể có thể nằm ở rìa ngoài `edge` bị cắt biên `truncated`, còn ở camera Trái lại nằm ở vùng `mid`). Trên không gian nhãn 2D raw fisheye, mỗi camera phải phản ánh trung thực dữ liệu thu nhận được tại thấu kính đó để phục vụ huấn luyện mô hình phát hiện đơn camera; do đó việc vẽ box trên từng camera là chuẩn xác. Lỗi `DUPLICATE` chỉ áp dụng khi vẽ hai box trùng lặp cho cùng một vật trên *cùng một ảnh camera*. Việc kết hợp danh tính, loại bỏ box trùng hoặc hợp nhất thành một vật thể 3D duy nhất là nhiệm vụ của tầng thuật toán đa camera (sensor fusion / BEV tracking layer) dựa trên ma trận calibration và chính sách hệ thống, không được tự ý xóa box ở tầng dữ liệu 2D gốc.

2. **Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.**
   - *Trên cùng một camera:*
     - *Giữ cùng track ID:* Khi vật thể tiếp tục tồn tại và di chuyển trong trường nhìn của camera, duy trì được tính nhất quán về danh tính (identity consistency) qua các frame kế tiếp dù có thay đổi nhẹ về góc quay hoặc vị trí.
     - *Thêm keyframe:* Khi vật thể có sự biến đổi đột ngột về hình học (do phóng to/thu nhỏ nhanh khi tiếp cận gần xe, chuyển hướng đột ngột làm đổi góc nhìn), hoặc thay đổi trạng thái che khuất (từ nhìn rõ sang bị xe khác che một phần `occluded`), cần cắm keyframe để thuật toán nội suy (interpolation) bounding box chính xác giữa các mốc thời gian.
     - *Gán trạng thái Outside:* Khi vật thể đi hoàn toàn ra ngoài vòng kính quang học hoặc lọt vào điểm mù vĩnh viễn, gán Outside để kết thúc chuỗi quan sát của track trên camera đó mà không làm gãy ID lịch sử.
   - *Bằng chứng cần thiết trước khi nối track qua hai camera:*
     - 1) Đồng bộ thời gian phần cứng tuyệt đối (hardware clock synchronization / timestamp) với độ lệch cực nhỏ ($\Delta t < 5\text{ ms}$).
     - 2) Ma trận hiệu chuẩn ngoại vi (extrinsic calibration) và hệ số méo nội suy (intrinsic calibration) đã được kiểm định độ chính xác hình học giữa hai camera so với hệ tọa độ phương tiện.
     - 3) Chính sách và thuật toán kết hợp không gian (cross-camera association policy) trên mặt phẳng Bird's-Eye View (BEV) hoặc không gian 3D, chứng minh hai tia chiếu từ 2 camera hội tụ tại cùng một vị trí vật lý trên mặt đường tại cùng thời điểm.

3. **Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?**
   - *Ca cụ thể:* Tại frame `adasind_261480.jpg`, ca `L1+M6` (phương tiện ô tô con đang di chuyển ở làn trung tâm). Tôi và mô hình YOLO26m đều phát hiện rõ ràng xe ô tô này với độ ôm sát cao, nhưng teaching reference lại bỏ sót hoàn toàn (`cell=LM_noR`).
   - *Cách xử lý:* Tôi kiên quyết không xóa box để chạy theo reference, mà phân loại nguyên nhân là `E0_reference_defect` (lỗi thiếu sót của tập tham chiếu), lập hồ sơ kiến nghị `submission/30_escalation_ticket.md` chuyển QA Lead phê duyệt bổ sung box, đồng thời ghi nhận vào `submission/40_decision_log.csv` với trạng thái `escalated`.
   - *Nếu làm lại slice này:* Tôi sẽ rà soát kỹ các thuộc tính biên ngay từ đầu (bật `truncated=true` cho xe L7 sát lề trái), phân biệt chuẩn xác hơn giữa xe ba bánh (`ThreeWheeler`) và ô tô nhỏ (`Car`) ở vùng bán kính `mid` bị méo cong, đồng thời dùng công cụ đo kích thước để loại bỏ sớm các box nhỏ vi phạm ngưỡng $H < 40\text{ px}$.

