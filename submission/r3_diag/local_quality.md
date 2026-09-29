# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `5cb8a9292a374c9718720c8ee68db34a398c0570ece63d18beb589ae771b05c4`; slice `B4-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_249480.jpg, adasind_261480.jpg, adasind_265065.jpg. Frame thiếu trong export: không.
TP=14; FP=5; FN=3; số lần đối chiếu=20; mean IoU của TP=0.840.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.700 | 0.920 | 0.900 |
| precision | 0.737 | 0.803 | 0.500 |
| recall | 0.824 | 0.793 | 0.500 |
| jaccard | 0.636 | 0.610 | 0.500 |
| dice | 0.778 | 0.753 | 0.667 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 5 | 2 | 0 | 0.900 | 0.714 | 1.000 | 0.714 | 0.833 |
| Car | 2 | 2 | 0 | 0.900 | 0.500 | 1.000 | 0.500 | 0.667 |
| Pedestrian | 4 | 1 | 1 | 0.900 | 0.800 | 0.800 | 0.667 | 0.800 |
| ThreeWheeler | 2 | 0 | 1 | 0.950 | 1.000 | 0.667 | 0.667 | 0.800 |
| Truck | 1 | 0 | 1 | 0.950 | 1.000 | 0.500 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_249480.jpg | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_261480.jpg | 6 | 2 | 1 | 0.750 | 0.750 | 0.857 |
| adasind_265065.jpg | 6 | 3 | 2 | 0.600 | 0.667 | 0.750 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 5 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 2 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 4 | 0 | 0 | 1 |
| ThreeWheeler | 0 | 1 | 0 | 2 | 0 | 0 |
| Truck | 0 | 1 | 0 | 0 | 1 | 0 |
| <extra> | 2 | 0 | 1 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
