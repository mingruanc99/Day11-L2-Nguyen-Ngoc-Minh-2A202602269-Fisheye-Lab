# Guideline patch

- **Rule mới đề xuất:** R11 — Phân định người đứng trên/bám ngoài phương tiện 3 bánh và thùng xe tải (Passenger/Loader on ThreeWheeler & Open Truck): Người đứng hoặc ngồi trong khoang chở hàng hở của xe ba bánh (`ThreeWheeler`) hoặc trên thùng sau xe tải (`Truck`) được gán chung vào bounding box của chính phương tiện đó (không vẽ box `Pedestrian` riêng), trừ trường hợp người đó đã bước hẳn một chân xuống tiếp xúc với mặt đường thì mới tách thành một box `Pedestrian` độc lập.
- **Áp dụng cho:** Các class `ThreeWheeler`, `Truck`, `Pedestrian` ở mọi zone (`center`, `mid`).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật R03 hiện tại chỉ quy định trường hợp xe hai bánh (`Bike` + Rider) và người ngồi trong cabin kín, nhưng bỏ trống tình huống giao thông thực tế trong tập ADASIND: người bốc vác, phụ xe đứng đu bám bên hông ThreeWheeler hoặc ngồi trên thùng xe hở. Điều này dẫn tới tranh chấp giữa việc vẽ trùng lặp box người đi bộ đè lên xe (như trường hợp L7/L8 trên frame `adasind_117120.jpg`), làm tăng False Positive cho class Pedestrian.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Round `rework` (bắt đầu áp dụng ngay từ pha P5).

