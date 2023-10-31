# 6. Primes

Đây là bài 5 của round 2 và được giải bởi người đồng đội vip pro còn lại của mình.

## Đề bài

Marcus chọn hai số nguyên tố lớn $p$ và $q$ rồi tính $n = p \cdot q$ và $m = p + q$. Sau đó số $n \cdot m$ được sử dụng trong cryptosystem.

Khi test Marcus thấy các số $p$ và $q$ cho tích $n \cdot m$ kết thúc bởi 2023. Điều đó khả thi không?

## Giải

Do $p$ và $q$ là các số nguyên tố lớn nên chúng lẻ. Suy ra $m = p + q$ là số chẵn, nên tích $n \cdot m$ cũng là số chẵn.

Số chẵn kết thúc bởi 0, 2, 4, 6, 8 nên không thể kết thúc bởi 3. Do đó điều này không thể xảy ra.