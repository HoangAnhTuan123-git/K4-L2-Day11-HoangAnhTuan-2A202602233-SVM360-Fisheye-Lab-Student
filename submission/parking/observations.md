# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): 
  1. Vạch thứ nhất: Vạch phân chia ô đỗ tiền cảnh ở trung tâm dưới (từ khoảng x=407.90, y=651.30 kéo xuống x=530.95, y=719.22), phân chia rõ rệt ô đỗ góc dưới bên trái và ô trung tâm.
  2. Vạch thứ hai: Vạch phân chia ô đỗ tiền cảnh phía bên phải (từ khoảng x=697.11, y=621.62 kéo xiên xuống x=959.45, y=682.47), xác định ranh giới ô đỗ ngoài cùng bên phải.
  (Tổng cộng toàn bộ ảnh đã vẽ đầy đủ 34 đoạn vạch phân chia các ô đỗ trên toàn bộ bãi).
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  - Mép gờ vỉa hè (curb), ranh giới bồn cây cỏ phía xa và dải phân cách sát chân tường công trình. Không vẽ thành `parking_line` vì đó là ranh giới vật lý phân cách bãi đỗ với công trình kiến trúc/vỉa hè, không phải vạch sơn phân định ranh giới ô đỗ xe (theo quy tắc docs/11-parking-lines-vi.md).
- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  - Đã vẽ 4 polygon `free_space` bao phủ các khoảng mặt đường trống nhìn thấy được của các làn xe chạy (driveway) giữa các dãy ô đỗ. Các polygon dừng chính xác tại ranh giới đầu các vạch ô đỗ, dừng sát mép lề đường bên trái và mép ảnh, hoàn toàn không đè lên curb, bồn cây, cột đèn hay các vị trí bị khuất tầm nhìn.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):
  - không có.


