# 5. Affine cipher

Đây là bài 1 của round 2 và được giải bởi bạn @vnc.

## Đề bài

Ta xét bảng chữ cái {A, ..., Z, $\alpha$, $\beta$, $\gamma$} có 29 chữ cái. Ta đánh số A, ..., Z từ 0 tới 25, và $\alpha$, $\beta$, $\gamma$ là 26, 27, 28.

Ta sử dụng cryptosystem mã hóa từng khối 2 ký tự, gọi là bigram. Với $x$ và $y$ là hai ký tự của bigram, thì plaintext sẽ là $P = 29 x + y$.

Mã hóa sử dụng biến đổi affine (giống hệ mã affine) là $C = a P + b \pmod{841}$.

Khi phân tích một đoạn văn bản dài, người ta phát hiện ra rằng các bigram sau xuất hiện nhiều nhất "$\beta\gamma$", "UM" và "LC". Đồng thời, trong tiếng Anh thì các bigram "TH", "HE" và "IN" cũng xuất hiện nhiều nhất.

Câu hỏi: có thể giải mã "KEUDCR" mà không cần khóa hay không? Còn key thì sao?

## Giải

Theo thống kê các bigram xuất hiện nhiều nhất trong ciphertext và trong plaintext sẽ khớp nhau. Do đó có thể thấy "TH" mã hóa thành "$\beta\gamma$" và "HE" mã hóa thành "LC". Như vậy ta có hệ phương trình

$$\begin{align*} 812 = a \cdot 558 + b \\ 321 = a \cdot 207 + b\end{align*} \pmod{841}$$

Giải hệ ta có $a = 15, b = 10$. Đây là key.

Từ đây chúng ta có thể giải mã thành **CRYPTO** là plaintext ban đầu.