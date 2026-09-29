# QA review · B4-mid

Mã khóa: 5CB8-A929

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_261480.jpg | L7 | R04 | Box Bike bên trái chạm viền ảnh x=0.0 nhưng thuộc tính truncated đang là false; cần bật truncated=true |
| adasind_261480.jpg | L8 | R08 | Box Bike L8 chồng lấn diện tích đáng kể với L7; cần soi kỹ trên ảnh xem là hai người đi xe riêng hay bị vẽ thừa box |
| adasind_265065.jpg | L4 | R03 | Box Bike chiều cao h=43.8 px sát ngưỡng tối thiểu H=40 px; kiểm tra điểm đáy ôm sát lốp xe |
| adasind_249480.jpg | L2 | R01 | Xe thùng kín phía xa gán nhãn Truck; kiểm tra kích thước và hình dáng phân biệt với SpecialVehicle |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.

