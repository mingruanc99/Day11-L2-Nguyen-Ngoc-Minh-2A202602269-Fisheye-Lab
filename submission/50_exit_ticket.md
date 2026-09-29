# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   - **Trả lời:** Đây **không phải là lỗi `DUPLICATE`**, mà bắt buộc **cần một quy tắc riêng (Cross-camera Seam Protocol)**.
   - **Vì sao:** Ở mức cảm biến 2D riêng lẻ, hai camera ghi nhận vật thể từ hai góc đặt, cự ly và độ méo quang học hoàn toàn khác nhau (ví dụ: vật nằm ở vùng rìa kính méo mạnh `edge` của camera trước, nhưng lại hiển thị ở góc nhìn nghiêng `mid`/`center` của camera bên hông). Nếu máy móc coi đây là lỗi `DUPLICATE` và ép annotator xóa đi một box, camera bị xóa box sẽ mất dữ liệu phát hiện chướng ngại vật cự ly gần trong vùng quan sát của nó. Quy tắc chuẩn trong hệ thống SVM là cho phép gán nhãn độc lập trên từng camera (hoặc liên kết thông qua một Global Object ID / 3D bounding box thống nhất), và giao nhiệm vụ khử trùng lặp (deduplication / fusion) cho tầng xử lý dung hợp đa camera (BEV Fusion / tracking downstream).

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   - **Giữ cùng track ID:** Khi vật thể di chuyển liên tục trong trường nhìn, các đặc trưng nhận dạng cốt lõi (loại xe, màu sắc, hướng di chuyển) được bảo toàn và có thể nội suy quỹ đạo mượt mà giữa các frame, kể cả khi bị che khuất tạm thời trong khoảng thời gian rất ngắn (< 1–2 giây) mà vị trí tái xuất hiện khớp với dự đoán động học.
   - **Thêm keyframe:** Khi hình dạng hình học hoặc hướng phối cảnh của vật thể thay đổi đột ngột (ví dụ phương tiện rẽ góc 90 độ, thay đổi tỷ lệ khung hình mạnh do hiệu ứng mắt cá fisheye), hoặc khi xuất hiện trạng thái che khuất mới cần định hình lại ranh giới box.
   - **Gán trạng thái Outside:** Khi vật thể di chuyển hoàn toàn vượt ra ngoài vòng kính/trường nhìn (FOV) của camera, hoặc bị vật cản che khuất hoàn toàn kéo dài khiến việc suy đoán vị trí không còn căn cứ tin cậy.
   - **Bằng chứng cần thiết trước khi nối track qua hai camera:**
     1. Đồng bộ thời gian phần cứng chính xác tuyệt đối giữa hai camera (hardware timestamp synchronization).
     2. Ma trận hiệu chuẩn không gian ngoại vi và nội vi (Camera Intrinsics & Extrinsics) để xác định chính xác vùng giao thoa (overlapping footprint) trên mặt đất.
     3. Tính liên tục về động học không-thời gian (spatio-temporal continuity): vị trí và vận tốc khi thoát khỏi camera A phải tương thích logic với vị trí và hướng xuất hiện trên camera B.
     4. Độ tương đồng cao về đặc trưng thị giác (Visual Re-ID embeddings: màu sắc, loại xe, biển số hoặc vệt nhận dạng đặc thù).

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   - **Tình huống cụ thể:** Trên frame `adasind_086220.jpg` đối tượng `L6` (tọa độ mép trái x: 35–102, y: 961–1015), annotator gán nhãn là `Car` dựa trên hình khối tổng thể vuông vắn ở cự ly xa, trong khi reference lại coi đây là `ThreeWheeler`. 
   - **Cách xử lý:** Trong pha QA và chẩn đoán, mình không vội vàng sửa nhắm mắt theo reference mà kiểm tra kỹ đặc điểm cấu trúc dưới góc nhìn fisheye. Nhận thấy đây là dạng phương tiện 3 bánh chở hàng có thùng che kín (auto-rickshaw thùng kín) rất phổ biến ở Ấn Độ, mình đã ghi nhận trường hợp này vào `40_decision_log.csv` và tạo `30_escalation_ticket.md` yêu cầu guideline làm rõ ranh giới nhận diện.
   - **Nếu làm lại slice này:** Mình sẽ phóng to tối đa để soi phần tiếp đất của bánh xe (vệt 1 bánh trước hay 2 bánh trước) và độ dốc của kính chắn gió trước khi chốt nhãn `Car`; đồng thời chủ động soát kỹ các vùng ranh giới che khuất giữa phương tiện và người đi bộ để tránh các box trùng lấn như trên frame `adasind_117120.jpg`.
