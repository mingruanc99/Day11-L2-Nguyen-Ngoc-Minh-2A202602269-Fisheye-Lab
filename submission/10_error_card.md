# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | MISSING | 10 |
| center | B2 | SPURIOUS | 15 |
| center | C0 | SPURIOUS | 1 |
| edge | B2 | BOX_GEOMETRY | 1 |
| edge | B2 | SPURIOUS | 1 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B2 | MISSING | 3 |
| mid | B2 | SPURIOUS | 6 |
| mid | B2 | WRONG_CLASS | 1 |
| mid | C0 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 24 (ví dụ frame adasind_019560.jpg)
- MISSING: 13 (ví dụ frame adasind_062370.jpg)
- WRONG_CLASS: 2 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Guideline v1.0.0 chưa có quy định cho người phụ xe / người bốc xếp đứng đu bám hoặc ngồi trên thùng xe hở, khiến annotator lúng túng giữa việc vẽ tách người đi bộ hay gộp vào xe.
- Cách sửa và ai nhận việc (`owner`): Guideline (escalate): Ban hành rule R11 phân định rõ người trên thùng xe.
Annotator (rework): Đã xóa box thừa L7 trên frame 117120 và hiệu chỉnh nhãn L6 trên 086220.
AI team (keep_with_reason): Lập kế hoạch bổ sung dữ liệu biến dạng thấu kính fisheye để fine-tune mô hình.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): Liên kết trực tiếp ảnh minh chứng: 
conflict_117120_l7_l8.png
 và 
escalation_086220_l6.png
.
Đối chiếu quy tắc R01, R03, R04 trong 
docs/02-rules-vi.md
.
