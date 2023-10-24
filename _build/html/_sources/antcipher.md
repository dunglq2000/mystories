# 1. AntCipher

Bài này là bài số 2 ở round 1 và là bài số 11 ở round 2. Lúc thi round 1 mình không biết giải, còn ở round 2 thì mình đã giải theo cách như sau.

## Đề bài

Đặt

$$\begin{align*}
    f = (x_1 \lor x_2 \lor x_9) \land (\lnot x_1 \lor \lnot x_2 \lor \lnot x_9) \land (\lnot x_1 \lor x_2 \lnot x_9) \land (x_1 \lor \lnot x_2 \lor x_9) \land \\
    (x_1 \lor x_2 \lor x_3) \land (\lnot x_9 \lor \lnot x_{10} \lor \lnot x_3) \land (x_1 \lor \lnot x_2 \lor x_4) \land (\lnot x_9 \lor x_{10} \lor \lnot x_4) \land \\
    (\lnot x_1 \lor x_2 \lor x_5) \land (x_9 \lor \lnot x_{10} \lor \lnot x_5) \land (\lnot x_1 \lor \lnot x_2 \lor x_6) \land (x_9 \lor x_{10} \lor \lnot x_6) \land \\
    (x_1 \lor x_2 \lor x_3 \lor x_4 \lor \lnot x_7) \land (x_2 \lor x_3 \lor x_4 \lor \lnot x_7 \lor \lnot x_8)
\end{align*}$$

Hàm $f$ gồm 10 biến được viết dưới dạng CNF (conjunctive normal form). Thuật toán mã hóa dựa trên hàm $f$ biến đối hai bit plaintext $(x_1, x_2)$ thành hai bit ciphertext $(x_9, x_{10})$ khi giá trị hàm $f = True$. Hàm $f$ này có 10 biến $x_1, x_2, \ldots, x_{10}$ và 46 literals, là các hạng tử trong biểu diễn CNF của hàm. Ví dụ với dấu ngoặc thứ hai có 3 literals là $\lnot x_1$, $\lnot x_2$ và $\lnot x_9$.

**Câu hỏi**. Vì các giới hạn tính toán nên chúng ta chỉ có thể sử dụng tối đa 16 biến với 20 literals. Nhắc lại rằng hàm $f$ ở trên có 10 biến và 46 literals. Hãy tìm cách biểu diễn tương đương của thuật toán mã hóa trên với giới hạn đã cho.

## Giải

Khi mình code hàm để tính giá trị hàm $f$ và xem xét những vector $(x_1, \ldots, x_{10})$ mà $f = True$, mình nhận thấy rằng:

- nếu $(x_1, x_2) = (0, 0)$ thì $(x_9, x_{10}) = (1, 0)$
- nếu $(x_1, x_2) = (0, 1)$ thì $(x_9, x_{10}) = (1, 1)$
- nếu $(x_1, x_2) = (1, 0)$ thì $(x_9, x_{10}) = (0, 0)$
- nếu $(x_1, x_2) = (1, 1)$ thì $(x_9, x_{10}) = (0, 1)$

Mình nhận ra rằng các biến $x_3, x_4, \ldots, x_7, x_8$ hoàn toàn không tác động lên việc mã hóa từ $(x_1, x_2)$ thành $(x_9, x_{10})$ (ít nhất là ở những chỗ $f = True$ :v).

Như vậy bài toán được rút gọn thành hàm boolean 4 biến $x_1$, $x_2$, $x_9$ và $x_{10}$. Ở đó $f(0010) = f(0111) = f(1000) = f(1101) = 1$. Các vector còn lại thì $f=0$. Sau đây là bảng chân trị
```{list-table}
:header-rows: 1

*   - $x_1$
    - $x_2$
    - $x_9$
    - $x_{10}$
    - $f$
*   - 0
    - 0
    - 0
    - 0
    - 0
*   - 0
    - 0
    - 0
    - 1
    - 0
*   - 0
    - 0
    - 1
    - 0
    - 1
*   - 0
    - 0
    - 1
    - 1
    - 0
*   - 0
    - 1
    - 0
    - 0
    - 0
*   - 0
    - 1
    - 0
    - 1
    - 0
*   - 0
    - 1
    - 1
    - 0
    - 0
*   - 0
    - 1
    - 1
    - 1
    - 1
*   - 1
    - 0
    - 0
    - 0
    - 1
*   - 1
    - 0
    - 0
    - 1
    - 0
*   - 1
    - 0
    - 1
    - 0
    - 0
*   - 1
    - 0
    - 1
    - 1
    - 0
*   - 1
    - 1
    - 0
    - 0
    - 0
*   - 1
    - 1
    - 0
    - 1
    - 1
*   - 1
    - 1
    - 1
    - 0
    - 0
*   - 1
    - 1
    - 1
    - 1
    - 0
```

Từ bảng chân trị trên, sử dụng phương pháp bìa Karnaugh mình rút gọn được thành

$$\begin{align*}
    f(x_1, x_2, x_9, x_{10}) = & (\lnot x_1 \lor \lnot x_9) \land (x_1 \lor x_9) \land \\ 
    & (\lnot x_1 \lor \lnot x_2 \lor x_{10}) \land (x_1 \lor x_2 \lor \lnot x_{10}) \land \\
    & (\lnot x_1 \lor x_2 \lor \lnot x_{10}) \land (x_1 \lor \lnot x_2 \lor x_{10})
\end{align*}$$

CNF này có 4 biến và 16 literals, thỏa mãn yêu cầu đề bài (còn thỏa mãn đáp án không thì chưa biết :v).