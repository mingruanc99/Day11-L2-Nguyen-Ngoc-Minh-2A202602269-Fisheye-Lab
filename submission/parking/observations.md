# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): 4 parking line trắng ở góc dưới của ảnh
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: ở xa, bên cạnh chiếc xe. Lý do: quá mờ và  không thể vẽ
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Không vẽ đè lên phần parking line.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): Cần phải vẽ bên trong khu vực trống để parking không hay chỉ vẽ phần đường xe đi?
