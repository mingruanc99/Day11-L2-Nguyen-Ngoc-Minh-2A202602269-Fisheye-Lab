# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `8983e965c8df14093bf999623a18e3033d3a4dc9d9a14d03528c9811712678bf`; slice `B2-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_062370.jpg, adasind_086220.jpg, adasind_117120.jpg. Frame thiếu trong export: không.
TP=17; FP=4; FN=3; số lần đối chiếu=24; mean IoU của TP=0.855.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.708 | 0.942 | 0.875 |
| precision | 0.810 | 0.686 | 0.000 |
| recall | 0.850 | 0.743 | 0.000 |
| jaccard | 0.708 | 0.636 | 0.000 |
| dice | 0.829 | 0.701 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 6 | 0 | 1 | 0.958 | 1.000 | 0.857 | 0.857 | 0.923 |
| Car | 4 | 3 | 0 | 0.875 | 0.571 | 1.000 | 0.571 | 0.727 |
| Pedestrian | 1 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 6 | 1 | 1 | 0.917 | 0.857 | 0.857 | 0.750 | 0.857 |
| Truck | 0 | 0 | 1 | 0.958 | 0.000 | 0.000 | 0.000 | 0.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_062370.jpg | 7 | 0 | 2 | 0.778 | 1.000 | 0.778 |
| adasind_086220.jpg | 4 | 2 | 1 | 0.571 | 0.667 | 0.800 |
| adasind_117120.jpg | 6 | 2 | 0 | 0.750 | 0.750 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 6 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 4 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 1 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 6 | 0 | 1 |
| Truck | 0 | 0 | 0 | 0 | 0 | 1 |
| <extra> | 0 | 3 | 0 | 1 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
