# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_249480.jpg
## adasind_261480.jpg
- L7+R2 mid ATTRIBUTE
- L1+R6 mid WRONG_CLASS
- L8 mid SPURIOUS
## adasind_265065.jpg
- L2+R3 mid WRONG_CLASS
- L5 center SPURIOUS
- L6 center SPURIOUS
- R7 mid MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 4 | 4 | 0 | 3 |
| mid | 13 | 10 | 3 | 2 |
| edge | 0 | 0 | 0 | 0 |
