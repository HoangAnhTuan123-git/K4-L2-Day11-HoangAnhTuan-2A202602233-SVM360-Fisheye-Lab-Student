# Sensor context

- **Rig**: Camera fisheye góc siêu rộng gắn phía trước phương tiện di chuyển (xe máy hoặc xe thử nghiệm đường phố đô thị Ấn Độ), hướng nhìn thẳng về phía trước theo hướng di chuyển của xe. Camera đơn, không có calibration chi tiết hay thông số rig đa camera SVM 360 trong bộ dữ liệu ADASIND.
- **`ego_body`**: Nhìn thấy ở phần dưới cùng (đáy giữa/hai góc dưới) của khung hình, cụ thể là phần đầu xe/tay lái/gương chiếu hậu của phương tiện ego (xuất hiện ở 46/48 frame, ngoại trừ 2 frame ngoại lệ `adasind_006840.jpg` và `adasind_271039.jpg` không nhìn thấy thân xe).
- **Vòng kính (lens circle)**: Vòng tròn kính fisheye chiếm phần lớn diện tích trung tâm khung hình (khoảng 70-80% diện tích ảnh 1920x1080), có tâm gần tâm ảnh. Hai dải viền bên ngoài vòng tròn kính là vùng tối/đen không chứa dữ liệu quang học hữu ích (`lens_border`), có méo quang học dạng cong mạnh về phía rìa vòng kính (barrel distortion).
