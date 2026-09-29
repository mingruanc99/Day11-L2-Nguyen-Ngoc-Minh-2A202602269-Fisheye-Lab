# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 13 | 3 | 3 | 7 | 10 | MISSING (3) |
| mid | 6 | 0 | 1 | 3 | 5 | SPURIOUS (1) |
| edge | 1 | 0 | 0 | 0 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: zone center
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: slice 3 frame sẽ không hiểu được sequence trong video, méo fisheyr vì xoay thấu kính, box lỏng vì khó nhìn, thiếu ego_body vì không hiểu ego body là gì?
