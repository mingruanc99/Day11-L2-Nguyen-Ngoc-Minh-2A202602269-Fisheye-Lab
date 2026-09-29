# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Ngã tư đông đúc, tắc đường, che khuất liên hoàn và chóa đèn pha ngược sáng ban đêm | Mật độ xe dày đặc khiến ranh giới vật thể đan xen; ánh sáng lóa làm mất biên dạng xe | Camera coordinate fisheye gốc kèm thông số méo góc rộng (intrinsic polynomial coefficients) | Soát độc lập 3 bên (2 annotator cấp cao + 1 lead QA), giải quyết 100% ca tranh chấp theo rule v1.1.0 |
| rear | Xe bám sát đuôi cự ly cực gần (< 1.5m), nắp thùng xe rung lắc, góc nhìn từ trên cao xuống | Thân xe sau lấp kín toàn bộ khung nhìn khiến tỷ lệ chiều cao/rộng bị cắt cụt (truncated cực mạnh) | Calibration chiều cao đặt camera sau và góc chúi (pitch angle) | Chiếu đối sánh với cảm biến siêu âm / radar lùi để xác thực khoảng cách đáy phương tiện |
| left | Xe máy và người đi bộ lách sát sườn xe ở vùng rìa thấu kính khi xe chuẩn bị rẽ trái | Hiệu ứng kéo dãn phối cảnh fisheye (barrel distortion) làm méo hình người và phương tiện | Mặt phẳng ảnh fisheye 2D kèm ma trận chuyển đổi sang hệ tọa độ xe (Vehicle Rig Extrinsics) | Kiểm tra đồng thời cả 2 view (view camera sườn trái và view BEV được unwarp) để kiểm tra độ ôm sát |
| right | Chướng ngại vật tĩnh (bồn cây, cột mốc, curb) và người đi bộ bước xuống lòng đường ở góc khuất | Điểm mù gương phụ, góc tối dưới sườn xe và ranh giới không rõ ràng với nền vỉa hè | Vòng kính lens border thực tế và góc quay cảm biến bên phải | Double-blind review (soát mù độc lập), chỉ nghiệm thu gold khi IoU giữa 2 chuyên gia đạt ≥ 0.85 |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):**
  - Khi có sự thay đổi về phần cứng cảm biến (thay loại lens mắt cá mới, đổi độ phân giải sensor).
  - Khi xe được cân chỉnh lại góc đặt camera (re-calibration làm thay đổi ma trận Extrinsics/Intrinsics).
  - Khi cập nhật phiên bản quy tắc gán nhãn mới (như bản vá rule v1.1.0 quy định lại ranh giới phụ xe/người bốc vác).
  - Khi dữ liệu vận hành gặp phải miền OOD mới (Operational Domain Shift: mở rộng sang thời tiết tuyết rơi, mưa bão ngập lụt).

- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:**
  - *Policy:* Cần quy định rõ ràng phương thức đồng thuận: liệu hệ thống cho phép tồn tại 2 bounding box 2D độc lập trên 2 camera kèm chung một `global_track_id`, hay bắt buộc phải hợp nhất thành một 3D bounding box duy nhất tại tầng BEV.
  - *Evidence:* Cần đầy đủ bằng chứng đồng bộ timestamp (chênh lệch thời gian giữa 2 camera < 5ms), tính liên tục về hình học không gian (vị trí vật thể nằm chính xác trong vùng nón giao thoa tầm nhìn giữa 2 camera) và độ tương đồng đặc trưng thị giác (Re-ID similarity score > 0.8).

- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:**
  - Sự đồng thuận cao trên một camera đơn lẻ chỉ phản ánh chất lượng nhận diện 2D cục bộ (local visual fidelity).
  - Nó hoàn toàn không chứng minh được tính nhất quán toàn cục 360 độ (global geometric consistency): một cặp box rất chuẩn trên 2 camera đơn lẻ có thể bị xung đột vị trí nghiêm trọng (chiếu ra 2 tọa độ cách xa nhau cả mét trên mặt đất) nếu ma trận cân chỉnh ngoại vi (extrinsics calibration) bị lệch hoặc mặt đường không phẳng. Do đó, gold set thực sự của hệ thống SVM bắt buộc phải được thẩm định qua kiểm thử đa góc nhìn đồng thời (cross-sensor validation).
