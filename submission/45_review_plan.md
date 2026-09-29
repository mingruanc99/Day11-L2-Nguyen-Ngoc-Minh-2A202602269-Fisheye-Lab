# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Vùng tâm mật độ cao (`center` zone) trên frame `adasind_062370.jpg` và `adasind_086220.jpg` | 3 ca `MISSING` (ThreeWheeler R7, Truck R8, Bike R4), 3 ca `SPURIOUS` | Zone `center` tập trung lượng phương tiện lớn nhất (13/20 vật thể), độ che khuất chéo cao; cả annotator lẫn mô hình đều bỏ sót nhiều nhất ở đây (mô hình mất 7 vật). Cần rà soát để tránh sót phương tiện giữa làn đường. | Ảnh crop vùng tâm kèm overlay nhãn so sánh L vs R (`compare.html`), ma trận confusion và bảng `iou_sweep.md`. |
| Vùng chuyển tiếp mép quang học (`mid` zone) trên frame `adasind_086220.jpg` và `adasind_117120.jpg` | 1 ca `SPURIOUS` (chồng lấn L7/L8), 1 ca `WRONG_CLASS` (L6 Car vs ThreeWheeler) | Vùng `mid` chịu ảnh hưởng bắt đầu của méo quang học fisheye, các phương tiện đi ngang dễ bị biến dạng tỷ lệ khung hình dẫn tới nhầm lẫn class hoặc vẽ box quá lỏng làm trùng lấn với người đi bộ. | Tọa độ bounding box, chỉ số IoU đối sánh với reference, ảnh crop phóng to chi tiết bánh xe và vùng đu bám. |

Giới hạn của kết luận từ ba frame ADASIND: Tập dữ liệu 3 frame thuộc cùng một chuỗi thời gian ngắn trong một hành trình, cùng điều kiện ban ngày thời tiết tốt và sử dụng duy nhất một camera trước. Do đó, kết quả này không thể đại diện cho các điều kiện môi trường bất lợi (ban đêm, ngược sáng chói, trời mưa), không phản ánh tính ổn định bám đuổi (tracking drift) dài hạn, và hoàn toàn không bao quát được các góc nhìn từ camera bên hông hay camera lùi.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- **Cách soát độ phủ 200 frame:** Phân bổ đều và có chủ đích qua 4 góc nhìn camera vật lý (`front`, `rear`, `left`, `right`) với 2 phân nhóm điều kiện (`normal` và `hard`). Để tránh hiện tượng tự tương quan (temporal autocorrelation) — nơi các frame liên tiếp trong cùng một video clip ngắn có bối cảnh gần như y hệt nhau và khiến các lỗi bị đếm lặp lại giả tạo — việc lấy mẫu phải áp dụng bước nhảy thời gian tối thiểu (stride ≥ 30–60 frame hoặc chỉ lấy frame đại diện khi có sự kiện chuyển cảnh/rẽ hướng/vào ngã tư).
- **Vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi tổng thể:** Kế hoạch phân bổ này dựa trên chiến lược lấy mẫu phân tầng theo rủi ro (risk-based / stratified sampling), cố tình tăng tỷ trọng các trường hợp khó (`hard` slice, vùng biên seam, cự ly gần). Do mật độ ca khó trong mẫu cao hơn rất nhiều so với phân phối vận hành thực tế (operational distribution), tỷ lệ lỗi đo được từ 200 frame này chỉ có tác dụng định vị và chẩn đoán các dạng lỗi nguy hiểm (stress test / failure discovery), chứ không thể suy rộng ra để kết luận tỷ lệ lỗi trung bình (unbiased error rate) của toàn hệ thống khi chạy trên đường.
