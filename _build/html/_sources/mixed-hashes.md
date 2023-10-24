# 2. Mixed hashes

## Đề bài

Alice và Bob trao đổi các thông điệp mã hóa. Họ dùng thuật toán mã hóa khối **PRESENT** với key 80-bit và ECB mode. Ở đây, thông tin được lưu dạng ảnh **.ppm**.

Header của file .ppm gồm 3 dòng theo dạng

```
P6

X Y

255
```

trong đó X và Y là kích thước của ảnh theo chiều ngang và dọc.

Để đảm bảo an toàn, header sẽ được loại bỏ trước khi encrypt. Để có thể khôi phục header, hash của header sẽ được gửi đi thay vì header. Khi đó 3 phần của header sẽ được ngăn cách bởi dấu cách (space) thay vì newline như trên. Nghĩa là "P6 X Y 255".

Bob chuẩn bị 8 ảnh (trong file đính kèm) mà không có header. Bob encrypt 8 ảnh đó với cùng một key theo thuật toán **PRESENT** và ECB mode. Bob cũng gửi hash của 8 headers đi kèm. Tuy nhiên các hash đã bị trộn lẫn với nhau. Liệu chúng ta có thể khôi phục thông điệp mà Bob muốn gửi Alice?

Dưới đây là các hash.

```
602a4a8fff652291fdc0e049e3900dae608af64e5e4d2c5d4332603c9938171d
f40e838809ddaa770428a4b2adc1fff0c38a84abe496940d534af1232c2467d5
aa105295e25e11c8c42e4393c008428d965d42c6cb1b906e30be99f94f473bb5
70f87d0b880efcdbe159011126db397a1231966991ae9252b278623aeb9c0450
77a39d581d3d469084686c90ba08a5fb6ce621a552155730019f6c02cb4c0cb6
456ae6a020aa2d54c0c00a71d63033f6c7ca6cbc1424507668cf54b80325dc01
bd0fd461d87fba0d5e61bed6a399acdfc92b12769f9b3178f9752e30f1aeb81d
372df01b994c2b14969592fd2e78d27e7ee472a07c7ac3dfdf41d345b2f8e305
```

## Giải

Bài này là bài 3 ở round 1 và round 2. Trong thời gian 2 round mình đều giải ra (round 2 chi tiết hơn và trình bày đẹp hơn :v).

Đề cho một file mẫu là mikky.ppm. Khi phân tích file này mình thấy rằng, nếu gọi $w$ và $h$ là độ rộng và độ cao của ảnh (lấy từ header) thì độ dài file không có header là $3 \cdot w \cdot h$.

Sau khi encrypt bằng thuật toán mã hóa khối với ECB mode, độ dài sẽ là $3 \cdot w \cdot h + pd$, trong đó $pd$ là padding. Theo thuật toán **PRESENT** thì $0 \leqslant pd \leqslant 8$.

Với dự đoán rằng $w \approx h$, mình lấy căn bậc hai của độ dài các filel đề cho, và đưa ra dự đoán $w, h \in [400, 600]$. Nếu sai thì mình tăng độ rộng khoảng này thôi.

Tiếp theo, bruteforce $w$ và $h$ trong khoảng này, cho tới khi hash "P6 $x$ $y$ 255" xuất hiện trong số các hash trên, và $0 \leqslant len(ciphertext) - 3 * w * h \leqslant 8$ thì mình lấy $w$ và $h$ này. Thế là mình có header.

Do cả 8 file được encrypt bởi cùng một key **PRESENT**, và key có 80 bit tương ứng 10 bytes, hay 10 "ký tự", nhìn đề mình nhận thấy có chuỗi `P6 X Y 255` là hợp lý. Như vậy `key = "P6 X Y 255"`.

Cuối cùng, mình giải mã lần lượt từng file với key trên, ghép header tương ứng vào, như vậy là mình giải mã được tất cả file rồi.

```{figure} ./images/File1.png
:height: 150px

File1.ppm
```

```{figure} ./images/File2.png
:height: 150px

File2.ppm
```

```{figure} ./images/File3.png
:height: 150px

File3.ppm
```

```{figure} ./images/File4.png
:height: 150px

File4.ppm
```

```{figure} ./images/File5.png
:height: 150px

File5.ppm
```

```{figure} ./images/File6.png
:height: 150px

File6.ppm
```

```{figure} ./images/File7.png
:height: 150px

File7.ppm
```

```{figure} ./images/File8.png
:height: 150px

File8.ppm
```

Vậy thông điệp gốc là `💔Loveyou`.