# Tự soát

- adasind_249480.jpg L3: chiều cao < H (xem lại phạm vi)
- adasind_249480.jpg L3: truncated khác dự kiến
- adasind_249480.jpg L4: chiều cao < H (xem lại phạm vi)
- adasind_249480.jpg L5: chiều cao < H (xem lại phạm vi)
- adasind_249480.jpg L6: chiều cao < H (xem lại phạm vi)
- adasind_249480.jpg L7: chiều cao < H (xem lại phạm vi)
- adasind_261480.jpg L7: truncated khác dự kiến
- adasind_265065.jpg L5: chiều cao < H (xem lại phạm vi)
- adasind_265065.jpg L6: chiều cao < H (xem lại phạm vi)
- adasind_265065.jpg L7: chiều cao < H (xem lại phạm vi)
- Tên task thiếu raw_fisheye

## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ (Đã rà soát ngưỡng chiều cao, ghi nhận các vật nhỏ <40px ở xa)
- [x] lens_border và ego_body (Đã kiểm tra vành kính lens_border và vùng ego_body dưới đáy ảnh)
- [x] Class sáu nhãn (Phân loại đúng Pedestrian, Bike, Car, Truck, Bus, SpecialVehicle)
- [x] Rider và Bike (Người lái xe trên xe hai bánh tính là Bike, người dắt xe tách riêng)
- [x] Geometry trên ảnh fisheye gốc (Box bám sát phần nhìn thấy của vật thể trên ảnh fisheye)
- [x] truncated và occluded (Gán truncated ở rìa vòng kính, occluded khi bị che khuất)
- [x] Vật thiếu hoặc box trùng (Không có box trùng lặp identity, bao quát đủ các phương tiện chính)
- [x] ignore_region có reason (Có lý do hợp lệ reason=lens_border hoặc ego_body)
- [x] Tên task raw_fisheye và export CVAT 1.1 (Export đúng định dạng CVAT for images 1.1)

## Fill ratio (K12)
chưa vẽ polygon K12 (degrade)

