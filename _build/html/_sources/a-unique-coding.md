# 8. A unique decoding

Bài này khi nhìn đề thì "có vẻ" câu hỏi Q2 là trường hợp nhỏ hơn của Q1. Mình giải Q2 (không chắc đúng hoàn toàn) nên lời giải sau đây áp dụng cho cả Q1 và Q2 :v :v :v

## Đề bài

Xét binary error-correcting code $\mathcal{C}$ với độ dài $n$. Code này chỉ đơn giản là tập con của $\mathbb{F}_2^n$ thôi và ta truyền một phần tử của code này qua các kênh truyền.

Khi đi qua các kênh truyền các bit có thể bị sai, dẫn tới bị đảo bit. Khi nhận được vector $\boldsymbol{y} \in \mathbb{F}_2^n$, ta sẽ decode thành một phần tử thuộc $\mathcal{C}$ mà có khoảng cách gần $\boldsymbol{y}$ nhất, nói cách khác là Hamming weight ngắn nhất.

Xét cơ chế decode maximal-likelihood. Giả sử ta nhận được $\boldsymbol{y} \in \mathbb{F}_2^n$, ta muốn xét các trường hợp lỗi xảy ra ít nhất, gọi là $d_{\boldsymbol{y}}$, nghĩa là

$$d_{\boldsymbol{y}} = \min_{\boldsymbol{x} \in \mathcal{C}} wt(\boldsymbol{x}, \boldsymbol{y})$$

Tiếp theo, đặt $\mathcal{D} (\boldsymbol{y}) = \{ \boldsymbol{x} \in \mathcal{C} : wt(\boldsymbol{x}, \boldsymbol{y}) = d_{\boldsymbol{y}} \}$. Cuối cùng ta decode $\boldsymbol{y}$ thành phần tử $\boldsymbol{x}$ nào đó trong $\mathcal{D}(\boldsymbol{y})$.

Chúng ta quan tâm tới các trường hợp code $\mathcal{C}$ khiến $\lvert \mathcal{D}(\boldsymbol{y}) \rvert = 1$ với mọi $\boldsymbol{y} \in \mathbb{F}_2^n$. Nói cách khác khi nhận được $\boldsymbol{y}$ bất kì của $\mathbb{F}_2^n$ ta có thể decode thành một dạng duy nhất.

**Q1**. Code $\mathcal{C}$ như nào thì thỏa mãn tính chất này?

**Q2**. Code $\mathcal{C}$ như nào thỏa mãn tính chất này và là không gian tuyến tính con của $\mathbb{F}_2^n$?

## Giải

Đầu tiên mình có nhận xét (khá rõ ràng) sau đây:

**Nhận xét 1**. Với mọi $n$, code $\mathcal{C} \equiv \mathbb{F}_2^n$ thỏa mãn tính chất trên.

Chúng ta có thể thấy rằng với mọi $\boldsymbol{y} \in \mathbb{F}_2^n$ nhận được thì sẽ decode thành chính nó trong $\mathcal{C}$.

Tiếp theo, ta xét nửa trên của $\mathbb{F}_2^n$. Trong hệ thập phân thì đó là các số từ $0$ tới $2^{n-1} - 1$ và có dạng

$$\boldsymbol{x} = (0, x_1, x_2, \ldots, x_{n-1})$$

Nói cách khác, code $\mathcal{C}$ là tập hợp

$$\mathcal{C} = \{ \boldsymbol{x} = (0, x_1, x_2, \ldots, x_{n-1}): x_i \in \mathbb{F}_2 \}$$

Code $\mathcal{C}$ này thỏa mãn tính chất trên và mình sẽ chứng minh ngay sau đây.

**Chứng minh**. Giả sử ta nhận được $\boldsymbol{y} \in \mathbb{F}_2^n$. Ta có hai trường hợp:

- nếu $\boldsymbol{y} = (0, y_1, y_2, \ldots, y_{n-1})$, hay nói cách khác biểu diễn thập phân của $\boldsymbol{y}$ là từ $0$ tới $2^{n-1} - 1$, thì $\boldsymbol{y}$ được decode thành chính nó trong $\mathcal{C}$. Khi này $d_{\boldsymbol{y}} = 0$ nhỏ nhất và không có vector nào khác cho Hamming weight bằng $0$ trừ chính nó.
- nếu $\boldsymbol{y} = (1, y_1, y_2, \ldots, y_{n-1})$, hay nói cách khác biểu diễn thập phân của $\boldsymbol{y}$ là từ $2^{n-1}$ tới $2^n - 1$, thì $\boldsymbol{y}$ được decode thành $\boldsymbol{x} = (0, y_1, y_2, \ldots, y_{n-1})$ trong $\mathcal{C}$. Khi này $d_{\boldsymbol{y}} = 1$ nhỏ nhất vì khác mỗi bit đầu tiên và cũng không có vector nào khác cho Hamming weight bằng $1$.

Tiếp theo, mình viết các vector trong $\mathcal{C}$ thành các hàng của 1 ma trận $2^{n-1} \times n$. Gọi $A$ là ma trận hoán vị các cột của ma trận $2^{n-1} \times n$ đó. Khi đó $A$ là ma trận có tính chất: trên mỗi hàng và trên mỗi cột có đúng một phần tử (bằng 1) và ma trận $A$ khả nghịch. Ví dụ, với $n=4$, ma trận để hoán vị cột 2 với cột 4 là $\begin{pmatrix}1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \end{pmatrix}$.

Khi đó, nếu mình nhân ma trận $2^{n-1} \times n$ của code $\mathcal{C}$ với bất kì ma trận $A$ nào như vậy thì code $\mathcal{C}'$ nhận được cũng thỏa mãn tính chất trên.

**Ví dụ**, với $n=4$ thì code $\mathcal{C}$ gồm các vector 

$$\mathcal{C} = \{ 0000, 0001, 0010, 0011, 0100, 0101, 0110, 0111 \}$$

Với ma trận $A$ hoán vị cột 2 và 4 như trên ta có

$$\begin{pmatrix} 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 1 & 1 \\ 0 & 1 & 0 & 0 \\ 0 & 1 & 0 & 1 \\ 0 & 1 & 1 & 0 \\ 0 & 1 & 1 & 1 \end{pmatrix} \cdot \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 1 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 1 & 0 & 1 \\ 0 & 0 & 1 & 1 \\ 0 & 1 & 1 & 1\end{pmatrix} = \mathcal{C}'$$

Bây giờ mình sẽ chứng minh rằng với mọi ma trận $A$ hoán vị các cột như vậy thì code $\mathcal{C}'$ cũng thỏa mãn tính chất.

**Chứng minh**. Đặt

$$\mathcal{C} = \{ (0, x_1, x_2, \ldots, x_{n-1}), x_i \in \mathbb{F}_2 \}$$

Gọi $A$ là ma trận hoán vị cột kích thước $n \times n$. Khi đó ánh xạ

$$A: \mathbb{F}_2^n \to \mathbb{F}_2^n, \quad \boldsymbol{y} \to \boldsymbol{y} \cdot A$$
là song ánh do $A$ là ma trận khả nghịch. Khi đó xét code

$$\mathcal{C}' = \{ \boldsymbol{x} \cdot A: \boldsymbol{x} \in \mathcal{C} \}$$

Mình vẫn có hai trường hợp.

*Trường hợp 1.* Với $\boldsymbol{y} = (0, y_1, y_2, \ldots, y_{n-1}) \in \mathbb{F}_2^n$ từ $0$ tới $2^{n-1}-1$ như trên. Xét $\boldsymbol{y}' = \boldsymbol{y} \cdot A$.

Khi đó, với $\boldsymbol{x} = (0, y_1, y_2, \ldots, y_{n-1}) \in \mathcal{C}$, ta có $\boldsymbol{x}' = \boldsymbol{x} \cdot A \in \mathcal{C}'$. Từ đây suy ra

$$wt(\boldsymbol{x}' \oplus \boldsymbol{y}') = wt((\boldsymbol{x} \cdot A) \oplus (\boldsymbol{y} \cdot A)) = wt((\boldsymbol{x} \oplus \boldsymbol{y}) \cdot A) = wt(\boldsymbol{0} \cdot A) = 0$$

Ở đây $\boldsymbol{0} = (0, 0, \ldots, 0) \in \mathbb{F}_2^n$.

Nói cách khác $d_{\boldsymbol{y}'} = 0$ và có duy nhất một vector $\boldsymbol{x}'$ được định nghĩa như trên thỏa mãn. Do đó $\lvert \mathcal{D}(\boldsymbol{y}') \rvert = 1$.

*Trường hợp 2.* Với $\boldsymbol{y} = (1, y_1, y_2, \ldots, y_{n-1}) \in \mathbb{F}_2^n$ từ $2^{n-1}$ tới $2^n-1$ như trên. Ta cũng xét $\boldsymbol{y}' = \boldsymbol{y} \cdot A$.

Khi đó, với $\boldsymbol{x} = (0, y_1, y_2, \ldots, y_{n-1}) \in \mathcal{C}$, ta cũng có $\boldsymbol{x}' = \boldsymbol{x} \cdot \in \mathcal{C}'$. Từ đây ta có

$$wt(\boldsymbol{x}' \oplus \boldsymbol{y}') = wt((\boldsymbol{x} \cdot A) \oplus (\boldsymbol{y} \cdot A)) = wt((\boldsymbol{x} \oplus \boldsymbol{y}) \cdot A) = wt((1, 0, 0, \ldots, 0) \cdot A) = 1$$

Ở phép nhân vector $(1, 0, \ldots, 0)$ với ma trận $A$, vì ma trận $A$ chỉ có duy nhất một cột có dạng $(1, 0, \ldots, 0)^T$ nên kết quả phép nhân là một vector có đúng một phần tử 1, còn lại là 0.

Nói cách khác $d_{\boldsymbol{y}'} = 1$ và có duy nhất một vector $\boldsymbol{x}'$ được định nghĩa như trên thỏa mãn. Do đó $\lvert \mathcal{D}(\boldsymbol{y}') \rvert = 1$.

Như vậy ta đã chứng minh xong.

Hoàn toàn tương tự, khi code $\mathcal{C}$ là các vector bắt đầu với hai số 0 thì ta lần lượt xét $\boldsymbol{y}$ trong các khoảng $[0, 2^{n-2}-1]$, $[2^{n-2}, 2^{n-1}-1]$, $[2^{n-1}, 2^{n-1} + 2^{n-2}-1]$, $[2^{n-1} + 2^{n-2} - 1, 2^n-1]$. Nghĩa là

$$\mathcal{C} = \{ \boldsymbol{x} = (0, 0, x_1, x_2, \ldots, x_{n-2}: x_i \in \mathbb{F}_2^n) \}$$

Khi đó ta xét các vector $\boldsymbol{y}$ có dạng:

- $\boldsymbol{y} = (0, 0, y_1, y_2, \ldots, y_{n-2})$
- $\boldsymbol{y} = (0, 1, y_1, y_2, \ldots, y_{n-2})$
- $\boldsymbol{y} = (1, 0, y_1, y_2, \ldots, y_{n-2})$
- $\boldsymbol{y} = (1, 1, y_1, y_2, \ldots, y_{n-2})$

Theo quy nạp thì code $C$ bắt đầu với $i$ số 0 đều đúng, $0 \leqslant i \leqslant n$. Nghĩa là

$$\mathcal{C} = \{ \boldsymbol{x} = (0, \ldots, 0, x_1, x_2, \ldots, x_{n-i}): x_i \in \mathbb{F}_2 \}$$

Sau đó chúng ta lại áp dụng phép nhân với ma trận hoán vị cột $A$ như bên trên thì các code $\mathcal{C}'$ cũng thỏa mãn.

Vấn đề ở đây là, những code $\mathcal{C}$ như vậy là không gian vector sinh bởi $i$ vector ($0 \leqslant i \leqslant n$) trong các vector sau:

$$\begin{align*} \boldsymbol{v}_1 & = (1, 0, 0, \ldots, 0, 0) \\ \boldsymbol{v}_2 & = (0, 1, 0, \ldots, 0, 0) \\ \boldsymbol{v}_3 & = (0, 0, 1, \ldots, 0, 0) \\ \cdots & = \cdots \\ \boldsymbol{v}_n & = (0, 0, 0, \ldots, 0, 1) \end{align*}$$

Số cách chọn $i$ vector từ $n$ vector là

$$C_n^0 + C_n^1 + \ldots + C_n^n = 2^n$$
cách. Nói cách khác có $2^n$ code $\mathcal{C}$ thỏa tính chất đề bài và là không gian tuyến tính của $\mathbb{F}_2^n$.

**Ví dụ**. Với $n=3$ thì các code sau thỏa mãn tính chất

$$\begin{align*}
    & \mathcal{C}_1 = \{ 000 \}, \\
    & \mathcal{C}_2 = \{ 000, 001 \}, \\
    & \mathcal{C}_3 = \{ 000, 010 \}, \\
    & \mathcal{C}_4 = \{ 000, 100 \}, \\
    & \mathcal{C}_5 = \{ 000, 001, 010, 011 \}, \\
    & \mathcal{C}_6 = \{ 000, 001, 100, 101 \}, \\
    & \mathcal{C}_7 = \{ 000, 010, 100, 110 \}, \\
    & \mathcal{C}_8 = \{ 000, 001, 010, 011, 100, 101, 110, 111 \}
\end{align*}$$