# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide, **không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference ADASIND hoặc nhãn bạn vừa vẽ.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Ngược sáng mặt trời chiếu thẳng ống kính; người đi bộ cắt ngang đầu xe tốc độ cao; xe hai bánh luồn lách từ điểm mù | Tương phản cực đại gây cháy sáng pixel; biến dạng quang học rìa lớn khi vật thể di chuyển nhanh; dễ nhầm class giữa rider và pedestrian | Không gian ảnh gốc Raw Fisheye 2D; bảo toàn ma trận nội suy K, hệ số méo D và ma trận ngoại vi so với tâm xe | 2 chuyên gia gán nhãn độc lập mù (double-blind); tính IoU đối sánh (>0.7); Lead Reviewer ADAS phân xử trực tiếp trên chuỗi frame liền kề |
| rear | Lùi xe ban đêm bị đèn pha xe sau rọi chói; chướng ngại vật thấp (gờ đá, cọc tiêu, trẻ nhỏ) sát cản sau | Thân xe ego cản trở tầm nhìn dưới; quầng sáng đèn pha gây lóa mất biên dạng; bóng đổ dài gây phán đoán sai ranh giới tiếp đất | Raw Fisheye 2D; giữ cố định mask đa giác `ego_body` của cản sau; đồng bộ timestamp với cảm biến siêu âm hỗ trợ | Kiểm tra nghiêm ngặt điểm tiếp xúc mặt đất (ground-contact point); đối chiếu cự ly thực tế; yêu cầu đồng thuận 100% cho mọi vật thể cự ly dưới 2m |
| left | Xe máy vượt sát sườn xe tại vùng seam góc trước-trái; vật thể ở biên ngoài rìa vòng kính fisheye | Biến dạng méo cong hình học cực đại làm kéo giãn hoặc dẹp box; vật thể bị cắt biên (`truncated`); một phần cơ thể nằm ở camera trước | Raw Fisheye 2D góc rộng; thông số góc xoay yaw/pitch/roll của camera gương trái so với hệ tọa độ xe | Soát đồng thời trên ảnh gốc và đối chiếu trên hình chiếu BEV; thẩm định chặt chẽ cờ `truncated` và `occluded` theo checklist 9 mục |
| right | Đỗ xe song song sát mép vỉa hè cao; người đi bộ đứng nửa trên vỉa hè nửa dưới lòng đường; bóng cây râm che khuất | Khó phân định ranh giới giữa người đi bộ và vật thể tĩnh trên vỉa hè; độ tương phản bóng râm thấp; méo góc seam trước-phải | Raw Fisheye 2D kết hợp vector mặt phẳng mặt đất (ground plane homography); ma trận cân chỉnh camera sườn phải | Đánh giá chéo đa góc nhìn; đo độ ôm sát của box với phần cơ thể nhìn thấy thực tế; QA Lead phê duyệt độc lập |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):**
  1. Khi thay đổi phần cứng cảm biến hoặc ống kính quang học (đổi camera, FOV, độ phân giải hoặc vị trí gắn trên phương tiện).
  2. Khi hiệu chuẩn lại hệ thống (re-calibration) dẫn đến sai lệch ma trận nội suy/ngoại vi vượt ngưỡng dung sai cho phép.
  3. Khi có phiên bản cập nhật của quy tắc gán nhãn (guideline patch, ví dụ thay đổi định nghĩa class rider, ngưỡng chiều cao tối thiểu H, hoặc phạm vi vùng `ignore_region`).
  4. Khi phạm vi hoạt động thiết kế (ODD) mở rộng sang điều kiện thời tiết/môi trường mới (mưa lớn, tuyết, sương mù, ban đêm đô thị thiếu sáng).

- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:**
  - *Ca ví dụ:* Một người đi bộ đang băng qua góc trước-trái của xe, xuất hiện đồng thời trong vùng chồng hình ảnh (seam) của cả camera Front và camera Left.
  - *Evidence bắt buộc:* Phải có đồng bộ thời gian cấp phần cứng chính xác ($\Delta t < 5\text{ ms}$) và ma trận hiệu chuẩn ngoại vi giữa hai camera được xác thực còn hiệu lực.
  - *Policy:* Trên ảnh 2D đơn camera, mỗi camera bắt buộc phải gán nhãn một box riêng biệt ôm phần nhìn thấy trên camera đó và gán đúng thuộc tính cắt biên `truncated` (tuyệt đối không coi là lỗi `DUPLICATE` trên tầng camera đơn). Việc hợp nhất danh tính (track fusion / deduplication) chỉ được thực hiện ở tầng fusion đa camera cấp cao hơn dựa trên tọa độ 3D hoặc phép chiếu BEV, có quy định rõ ràng về quy tắc chọn box hay gộp box.

- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:**
  1. Tập dữ liệu ADASIND trong bài thực hành chỉ là một camera đơn hướng trước, không phản ánh các đặc tính quang học, góc nghiêng, điểm mù và vùng che khuất của camera sau và hai camera sườn xe.
  2. Tỷ lệ đồng thuận giữa hai người gán nhãn (peer agreement) trên một camera có thể chứa "thiên kiến chung" (systematic bias) nếu cả hai người cùng hiểu sai hoặc có lỗ hổng trong guideline.
  3. Báo cáo chất lượng trên một camera không có khả năng kiểm tra tính nhất quán không gian tại vùng giáp ranh (seam consistency) hay tính liền mạch khi chiếu lên mặt phẳng Bird's-Eye View (BEV). Để xây dựng gold set chuẩn cho hệ thống SVM 360, bắt buộc phải có tập kiểm chuẩn độc lập cho đủ cả 4 camera với quy trình thẩm định đa góc nhìn nghiêm ngặt.
