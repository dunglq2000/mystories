# 4. Quantum encryption

Đây là bài 8 của round 1 và bài 10 của round 2. Bài này mình không chắc chắn lắm vì mình học quantum crypto mới 1 tuần đã vác đi thi :((

## Đề bài

Bob tạo một thuật toán mã hóa encrypt 4 bit $(x_1, x_2, x_3, x_4)$ bằng key cũng 4 bit $(k_1, k_2, k_3, k_4)$ với mạch sau:

```{figure} ./images/quanpics-1.png
:name: init-circuit
```

Plaintext 4 bit $(x_1, x_2, x_3, x_4)$ được biểu diễn ở dạng 4-qubit "plainstate" $\lvert x1, x2, x3, x4 \rangle$. Quantum state này là input cho mạch ở dạng qubit đơn đi qua các cổng.

Ở đây hai loại cổng được sửa dụng là CNOT và Hadamard.

Ký hiệu $H^b$ với $b \in \{ 0, 1 \}$ có nghĩa là, nếu $b = 0$ thì cổng đồng nhất $I$ được sử dụng (không thay đổi), còn nếu $b = 1$ thì cổng Hadamard sẽ được sử dụng.

Kết quả sau khi qua mạch là "cipherstate" $\lvert \psi \rangle$.

Bob có nhiệm vụ tăng số qubit đầu vào lên nhằm giảm các sai số khi tính toán và truyền dữ liệu trên kênh quantum. Do đó Bob biến đổi thành mạch sau:

```{figure} ./images/quanpics-2.png
:name: cipher-circuit
```

Alice nói rằng cô ấy có thể tìm lại được key nếu biết $N$ amplitude của kết quả $\lvert \psi \rangle$. Do có 8 qubits ở kết quả nên số lượng amplitude tối đa là $2^8 = 256$, nói cách khác $N \leqslant 256$. Vậy Alice cần ít nhất bao nhiêu amplitude là đủ để tìm lại key?

## Giải

Lần đầu tiếp xúc với quantum, mình sẽ nói về một số ý chính của quantum computing.

### Quantum computing

Trên máy tính hiện nay, đơn vị xử lý cơ bản là bit (0 hoặc 1). Trong máy tính lượng tử, đơn vị tính toán là qubit (quantum bit).

Mỗi qubit $\lvert \psi \rangle$ được biểu diễn dưới dạng tổ hợp tuyến tính của cơ sở gồm $\lvert 0 \rangle = (1, 0)$ và $\lvert 1 \rangle = (0, 1)$. Khi đó qubit $\lvert \psi \rangle = \alpha \lvert 0 \rangle + \beta \lvert 1 \rangle$. Ở đây $\alpha, \beta \in \mathbb{C}$ (tập số phức).

Tích của $n$ qubit là các tổ hợp $\lvert 0, 0, \ldots, 0 \rangle$, $\lvert 0, 0, \ldots, 1 \rangle$, ..., $\lvert 1, 1, \ldots, 1 \rangle$. Ta cũng ký hiệu $\lvert 0 \rangle \otimes \lvert 1 \rangle = \lvert 01 \rangle$. Ví dụ nếu $\lvert \psi \rangle = \alpha \lvert 0 \rangle + \beta \lvert 1 \rangle$ và $\lvert \Psi \rangle = \gamma \lvert 0 \rangle + \delta \lvert 1 \rangle$ thì

$$\lvert \psi \rangle \otimes \lvert \Psi \rangle = (\alpha \lvert 0 \rangle + \beta \lvert 1 \rangle) \otimes (\gamma \lvert 0 \rangle + \delta \lvert 1 \rangle) = \alpha \gamma \lvert 00 \rangle + \alpha \delta \lvert 0 1 \rangle + \beta \gamma \lvert 10 \rangle + \beta \delta \lvert 11 \rangle$$

Tiếp theo là **toán tử quantum** và tương ứng với nó là các cổng (gate) trên mạch.

**Toán tử quantum** tác động lên một qubit hoặc tích của nhiều qubit. Sau đây chúng ta xem xét một vài toán tử sử dụng cho bài này.

Qubit có dạng $\lvert \psi \rangle = a \lvert 0 \rangle + b \lvert 1 \rangle$. Ta có thể viết hệ số dưới dạng vector cột $\begin{pmatrix} a \\ b \end{pmatrix}$. Khi đó, toán tử quantum sẽ là một ma trận 2x2 biến đổi vector trên thành vector mới $\begin{pmatrix} c \\ d \end{pmatrix}$ tương ứng với qubit $\lvert \Psi \rangle = c \lvert 0 \rangle + d \lvert 1 \rangle$.

Nói cách khác, đặt toán tử quantum là ma trận $\mathcal{U} = \begin{pmatrix} c_{11} & c_{12} \\ c_{21} & c_{22} \end{pmatrix}$ thì ta có

$$\lvert \psi \rangle \to \lvert \Psi \rangle = \mathcal{U} \lvert \psi \rangle, \quad \begin{pmatrix} c_{11} & c_{12} \\ c_{21} & c_{22} \end{pmatrix} \cdot \begin{pmatrix} a \\ b \end{pmatrix} = \begin{pmatrix} c \\ d \end{pmatrix}$$

#### 1. Toán tử đồng nhất (identity)

Toán tử này giữ nguyên qubit đầu vào. Ma trận tương ứng là ma trận đơn vị $I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}$.

#### 2. Toán tử NOT

Toán tử NOT có ma trận tương ứng là $\text{NOT} = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$. Khi đó $\text{NOT} \lvert \psi \rangle = b \lvert 0 \rangle + a \lvert 1 \rangle$ với $x \in \{ 0, 1 \}$.

Khi qubit là $\lvert 0 \rangle$ hoặc $\lvert 1 \rangle$, tác dụng của toán tử NOT là phép XOR nên ta có $\text{NOT} \lvert x \rangle = \lvert x \oplus 1 \rangle$.

#### 3. Toán tử Hadamard

Đây là một toán tử đặc biệt và được quan tâm nhiều.

Ma trận của toán tử Hadamard là $H = \dfrac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & - 1 \end{pmatrix}$. Ví dụ, qubit $\lvert \psi \rangle = a \lvert 0 \rangle + b \lvert 1 \rangle$, toán tử Hadamard tương ứng với phép nhân ma trận

$$\dfrac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & - 1 \end{pmatrix} \cdot \begin{pmatrix} a \\ b \end{pmatrix} = \begin{pmatrix} \dfrac{1}{\sqrt{2}} (a + b) \\ \dfrac{1}{\sqrt{2}} (a - b) \end{pmatrix}$$

Ta chuyển cột kết quả về lại dạng tổ hợp tuyến tính thì cổng Hadamard hoạt động trên qubit $\lvert \psi \rangle = a \lvert 0 \rangle + b \lvert 1 \rangle$ cho kết quả là

$$H \lvert \psi \rangle = H(a \lvert 0 \rangle + b \lvert 1 \rangle) = \dfrac{1}{\sqrt{2}} (a + b) \lvert 0 \rangle + \dfrac{1}{\sqrt{2}} (a - b) \lvert 1 \rangle$$

Nếu $\lvert \psi \rangle \equiv \lvert 0 \rangle$ thì tương đương với $a = 1, b = 0$. Ta có $H \lvert 0 \rangle = \dfrac{\lvert 0 \rangle + \lvert 1 \rangle}{\sqrt{2}}$.

Nếu $\lvert \psi \rangle \equiv \lvert 1 \rangle$ thì tương đương với $a = 0, b = 1$. Ta có $H \lvert 1 \rangle = \dfrac{\lvert 0 \rangle - \lvert 1 \rangle}{\sqrt{2}}$.

Tổng quát ta có chú thích trong đề, với $x \in \{ 0, 1 \}$ thì $H \lvert x \rangle = \dfrac{\lvert 0 \rangle + (-1)^x \lvert 1 \rangle}{\sqrt{2}}$.

#### 4. Toán tử control

Đây là toán tử thường được dùng nhất khi tính toán trên tích của nhiều qubit.

Như đã xem xét ở trên, tích của $n$ qubit sẽ có $2^n$ phần tử tương ứng các bộ $\lvert 0, 0, \ldots, 0, 0 \rangle$, $\lvert 0, 0, \ldots, 0, 1 \rangle$, ... Do đó toán tử control sẽ là ma trận kích thước $2^n \times 2^n$.

Gọi $\mathcal{U} = \begin{pmatrix} c_{11} & c_{12} \\ c_{21} & c_{22} \end{pmatrix}$ là toán tử tác động lên một qubit (ví dụ như 3 toán tử đã đề cập). Xét hai qubit là $\lvert x \rangle = a \lvert 0 \rangle + b \lvert 1 \rangle$ và $\lvert y \rangle = c \lvert 0 \rangle + d \lvert 1 \rangle$. Mình đã ghi phía trên

$$\lvert x \rangle \otimes \lvert y \rangle = ac \lvert 00 \rangle + ad \lvert 01 \rangle + bc \lvert 10 \rangle + bd \lvert 11 \rangle$$

Khi đó toán tử control có dạng ma trận là

$$c \mathcal{U} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & c_{11} & c_{12} \\ 0 & 0 & c_{21} & c_{22} \end{pmatrix}$$

Hay viết dưới dạng ma trận khối là $c \mathcal{U} = \begin{pmatrix} I & \mathcal{O} \\ \mathcal{O} & \mathcal{U} \end{pmatrix}$.

Ta cũng viết tích $\lvert x \rangle \otimes \lvert y \rangle$ dưới dạng vector cột (4 phần tử). Khi đó

$$ \mathcal{U} (\lvert x \rangle \otimes \lvert y \rangle) = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & c_{11} & c_{12} \\ 0 & 0 & c_{21} & c_{22} \end{pmatrix} \cdot \begin{pmatrix} ac \\ ad \\ bc \\ bd \end{pmatrix} = \begin{pmatrix} ac \\ ad \\ c_{11} \cdot bc + c_{12} \cdot bd \\ c_{21} \cdot bc + c_{22} \cdot bd \end{pmatrix}$$

Hai phần tử đầu của vector kết quả không thay đổi, còn phần sau có "một phần" là $\mathcal{U} \lvert y \rangle$. Khi viết lại kết quả dưới dạng qubit thì

$$ac \lvert 00 \rangle + ad \lvert 01 \rangle + (c_{11} \cdot bc + c_{12} \cdot bd) \lvert 10 \rangle + (c_{21} \cdot bc + c_{22} \cdot bd) \lvert 11 \rangle$$

**BÂY GIỜ**, tập trung vào chứ không lú :))

Nếu $\lvert x \rangle \equiv \lvert 0 \rangle$, tức là $a = 1, b = 0$ thì tích trên tương ứng với $c \lvert 00 \rangle + d \lvert 01 \rangle + 0 \lvert 10 \rangle + 0 \lvert 11 \rangle = \lvert 0 \rangle \otimes (c \lvert 0 \rangle + d \lvert 1 \rangle) = \lvert x \rangle \otimes \lvert y \rangle$.

Nếu $\lvert x \rangle \equiv \lvert 1 \rangle$, tức là $a = 0, b = 1$ thì tích trên tương ứng với $0 \lvert 00 \rangle + 0 \lvert 01 \rangle + (c_{11} c + c_{12} d) \lvert 10 \rangle + (c_{21} c + c_{22} d) \lvert 11 \rangle = \lvert 1 \rangle \otimes ((c_{11} c + c_{12} d) \lvert 0 \rangle + (c_{21} c + c_{22} d) \lvert 1 \rangle) = \lvert 1 \rangle \otimes \mathcal{U} \lvert y \rangle = \lvert x \rangle \otimes \mathcal{U} \lvert y \rangle$.

Tổng kết lại, với $x \in \{ 0, 1\}$ thì

- nếu $x = 0$ thì $\lvert x \rangle \otimes \lvert y \rangle \to \lvert x \rangle \otimes \lvert y \rangle$.
- nếu $x = 1$ thì $\lvert x \rangle \otimes \lvert y \rangle \to \lvert x \rangle \otimes \mathcal{U} \lvert y \rangle$.

Tùy vào $x$ là 0 hay 1 mà toán tử quantum $\mathcal{U}$ sẽ bị bỏ qua hoặc xem xét. Ở đây qubit $\lvert x \rangle$ đóng vai trò điều khiển nên đây được gọi là toán tử control.

#### 5. Toán tử control CNOT (Control NOT)

Toán tử quantum CNOT có ma trận là

$$\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix} = \begin{pmatrix} I & \mathcal{O} \\ \mathcal{O} & \text{NOT} \end{pmatrix}$$

Qubit $\lvert x \rangle$ với $x \in \{ 0, 1 \}$ đóng vai trò control cho qubit $\lvert y \rangle$. Khi $x \equiv 0$ thì $y$ giữ nguyên, hay $\lvert y \oplus 0 \rangle = \lvert y \oplus x \rangle$. Khi $x \equiv 1$ thì áp dụng cổng NOT bên trên, khi đó $y$ biến đổi thành $y \oplus 1 = y \oplus x$. Điều này giải thích ký hiệu trong đề $\lvert x \rangle \to \lvert x \rangle$ và $\lvert y \rangle \to \lvert y \oplus x \rangle$.

### Quay lại bài toán

Chúng ta xét dây 1 và 4 của mạch. Áp dụng cổng CNOT liên tiếp 3 lần ta có

$$\lvert x_1 \rangle \otimes \lvert x_2 \rangle \to \lvert x_1 \oplus x_2 \rangle \otimes \lvert x_2 \rangle \to \lvert x_1 \oplus x_2 \rangle \otimes \lvert x_1 \rangle \to \lvert x_2 \rangle \otimes \lvert x_1 \rangle$$

Nói cách khác là đảo bit :v :v :v

Tương tự cho các cặp dây (2, 3), (5, 8) và (6, 7). Do đó khi tới trước các cổng Hadamard thì thứ tự các qubit từ trên xuống dưới là $\lvert x_2 \rangle, \lvert x_2 \rangle, \lvert x_1 \rangle, \lvert x_1 \rangle, \lvert x_4 \rangle, \lvert x_4 \rangle, \lvert x_3 \rangle, \lvert x_3 \rangle$.

Mạch ở dây 1 và 2 đều có dạng $\lvert x_2 \rangle$ đi qua $H^{k_1}$ nên sau khi qua cổng mình đặt $\lvert z_2 \rangle = H^{k_1} \lvert x_2 \rangle$.

Tương tự, $\lvert z_1 \rangle = H^{k_2} \lvert x_1 \rangle$ cho dây 3 và 4, $\lvert z_4 \rangle = H^{k_3} \lvert x_4 \rangle$ cho dây 5 và 6, $\lvert z_3 \rangle = H^{k_4} \lvert x_3 \rangle$ cho dây 7 và 8.

Ở đây chúng ta có một lưu ý nhỏ có thể giúp ích trong việc giới hạn số lượng amplitude theo đề bài. Nếu $k_1 = 0$ thì $\lvert z_2 \rangle = \lvert x_2 \rangle$. Nếu $k_1 = 1$ thì $\lvert z_2 \rangle = \dfrac{\lvert 0 \rangle + (-1)^{x_2} \lvert 1 \rangle}{\sqrt{2}}$. Như vậy, hệ số trước $\lvert 0 \rangle$ của $\lvert z_2 \rangle$ có thể là $0, 1, \dfrac{1}{\sqrt{2}}$ đều không âm.

Bây giờ chúng ta quay lại toán tử CNOT. Ma trận tương ứng của toán tử CNOT là $\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}$. Kết quả sau khi thực hiện toán tử CNOT là hệ số trước $\lvert 00 \rangle$ và $\lvert 01 \rangle$ giữ nguyên, còn hệ số trước $\lvert 10 \rangle$ và $\lvert 11 \rangle$ đổi chỗ cho nhau.

Đối với 3 qubit, mình **dự đoán** tương tự. Ở cổng CNOT đầu tiên, dây 1 control dây 3. Nếu mình chỉ xét 3 dây đầu thì tích các qubit gồm $\lvert 000 \rangle$, $\lvert 001 \rangle$, $\lvert 010 \rangle$, $\lvert 011 \rangle$, $\lvert 100 \rangle$, $\lvert 101 \rangle$, $\lvert 110 \rangle$, $\lvert 111 \rangle$. Áp dụng "chiến thuật" tương tự, mình chỉ quan tâm vị trí 1 và 3. Nghĩa là hệ số của $\lvert 0 x 0 \rangle$ và $\lvert 0 x 1 \rangle$ giữ nguyên, còn hệ số trước $\lvert 1 x 0 \rangle$ và $\lvert 1 x 1 \rangle$ đổi chỗ cho nhau, với $x \in \{ 0, 1 \}$. Nói cách khác, 8 hệ số trước amplitude chỉ thay đổi vị trí chứ không nhiều hơn hay ít đi, hay tập hợp hệ số giữ nguyên.

Như vậy, giả sử $\lvert z_2 \rangle = a \lvert 0 \rangle + b \lvert 1 \rangle$, $\lvert z_1 \rangle = c \lvert 0 \rangle + \lvert 1 \rangle$, $\lvert z_4 \rangle = e \lvert 0 \rangle + f \lvert 1 \rangle$, $\lvert z_3 \rangle = g \lvert 0 \rangle + h \lvert 1 \rangle$. Khi đó kết quả cipherstate là

$$\lvert \psi \rangle = \lvert z_2 \rangle \otimes \lvert z_2 \rangle \otimes \lvert z_1 \rangle \otimes \lvert z_1 \rangle \otimes \lvert z_4 \rangle \otimes \lvert z_4 \rangle \otimes \lvert z_3 \rangle \otimes \lvert z_3 \rangle$$

Xét $\lvert z_2 \rangle \otimes \lvert z_2 \rangle = a^2 \lvert 00 \rangle + ab \lvert 01 \rangle + ab \lvert 10 \rangle + b^2 \lvert 11 \rangle$. Ở đây có 3 hệ số khác nhau là $(a^2, ab, b^2)$, ta cần cả 3 để xác định $\lvert z_2 \rangle$. Với lưu ý bên trên $a \geqslant 0$ và ta cần $ab$ để xác định $b$ hay $-b$.

Như vậy mình cần $3^4 = 81$ hệ số để tìm lại các key ban đầu.