# Escalation ticket

## Ticket 1

- **Frame:** `adasind_261480.jpg` (Đối tượng `L1+M6`, cell `LM_noR`)
- **Ảnh chụp:** `submission/screenshots/screenshot_b4_mid_diagnosis.png`
- **Expected impact:** Reference thiếu sót một phương tiện giao thông thực tế đang lưu thông trên đường mà cả người gán nhãn (L1) và mô hình YOLO26m (M6) đều nhận diện rõ ràng. Sai sót này làm phạt oan nhãn đúng thành lỗi `SPURIOUS`, gây sai lệch nghiêm trọng chỉ số đánh giá chất lượng (local quality metric) và làm nhiễu dữ liệu huấn luyện cho hệ thống SVM 360.
- **Owner:** `qa`
- **Recommendation:** QA Lead và Data Curator rà soát lại frame `adasind_261480.jpg`, chính thức phê duyệt bổ sung box cho phương tiện này vào bộ nhãn ground truth reference (với nhãn `ThreeWheeler`), đồng thời cập nhật lại bộ dữ liệu đánh giá chuẩn cho toàn bộ dự án.

