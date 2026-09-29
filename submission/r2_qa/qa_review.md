# QA review · B2-center

Mã khóa: AAE9-927E

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_086220.jpg | L6 | R04 | Đối tượng mép trái (x: 35–102) có đặc điểm phương tiện 3 bánh (auto-rickshaw/ThreeWheeler) nhưng đang gán nhãn Car; nghi vấn nhầm class theo quy định R04. |
| adasind_117120.jpg | L7 | R01| Box L7 ThreeWheeler (x: 665–734) chồng lấn lớn với người đi bộ L8 (x: 694–730) và ThreeWheeler L6; nghi vấn vẽ thừa/trùng lặp (spurious). |
| adasind_062370.jpg | L6 | R02 | Box L6 Bike nằm ở sát rìa ngoài bên phải (x: 872–1080), cần kiểm tra lại độ ôm sát hình học fisheye và cờ truncated theo R02/R05. |


Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
