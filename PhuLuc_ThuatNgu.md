# 📖 PHỤ LỤC: BẢNG THUẬT NGỮ ĐỐI CHIẾU

## TIẾNG VIỆT ↔ ENGLISH ↔ KÝ HIỆU TOÁN HỌC

> *"Khi đọc paper tiếng Anh, đây là bảng tra cứu nhanh giúp bạn kết nối thuật ngữ quốc tế với kiến thức đã học."*

---

## 📐 Chương 1: Đại Số Tuyến Tính

| Tiếng Việt | English | Ký hiệu | Ghi chú |
|-----------|---------|---------|---------|
| Vector | Vector | $\vec{v}$, **v**, $\mathbf{v}$ | Mũi tên có hướng & độ lớn |
| Vô hướng | Scalar | $a$, $\alpha$ | Chỉ có độ lớn, không có hướng |
| Ma trận | Matrix | $A$, $M$ | Bảng 2D số |
| Tensor | Tensor | $\mathcal{T}$ | Ma trận nhiều chiều (≥3D) |
| Tích vô hướng | Dot product / Inner product | $\vec{a} \cdot \vec{b}$, $\langle a, b \rangle$ | Kết quả là scalar |
| Tích ngoài | Outer product | $\vec{a} \otimes \vec{b}$ | Kết quả là ma trận |
| Chuyển vị | Transpose | $A^T$ | Lật hàng ↔ cột |
| Nghịch đảo | Inverse | $A^{-1}$ | $AA^{-1} = I$ |
| Định thức | Determinant | $\det(A)$, $|A|$ | Hệ số co giãn diện tích |
| Ma trận suy biến | Singular matrix | $\det(A) = 0$ | Không có nghịch đảo |
| Ma trận đơn vị | Identity matrix | $I$, $I_n$ | Đường chéo = 1, còn lại = 0 |
| Giá trị riêng | Eigenvalue | $\lambda$ | Hệ số co giãn theo eigenvector |
| Vector riêng | Eigenvector | $\vec{v}$ | Hướng không thay đổi khi nhân A |
| Chuẩn / Độ dài | Norm | $\|\vec{v}\|$, $\|\vec{v}\|_2$ | Khoảng cách / Độ lớn |
| Chuẩn L1 | L1 norm / Manhattan | $\|\vec{v}\|_1 = \sum|v_i|$ | Tổng trị tuyệt đối |
| Chuẩn L2 | L2 norm / Euclidean | $\|\vec{v}\|_2 = \sqrt{\sum v_i^2}$ | Đường chim bay |
| Phân rã SVD | Singular Value Decomposition | $A = U\Sigma V^T$ | Phân tích ma trận |
| Giảm chiều | Dimensionality reduction | PCA | Giữ thông tin, bỏ nhiễu |
| Phân tích thành phần chính | Principal Component Analysis | PCA | Eigenvector của Cov matrix |
| Độc lập tuyến tính | Linear independence | | Không vector nào thừa |
| Không gian hàng | Row space | | |
| Không gian cột | Column space / Range | Im(A) | |
| Hạng ma trận | Rank | rank(A) | Số hàng/cột độc lập |

---

## 📈 Chương 2: Giải Tích

| Tiếng Việt | English | Ký hiệu | Ghi chú |
|-----------|---------|---------|---------|
| Đạo hàm | Derivative | $f'(x)$, $\frac{df}{dx}$ | Tốc độ thay đổi |
| Đạo hàm riêng | Partial derivative | $\frac{\partial f}{\partial x}$ | Đạo hàm theo 1 biến, giữ cố định biến khác |
| Gradient | Gradient | $\nabla f$ | Vector đạo hàm riêng |
| Tích phân | Integral | $\int f(x) dx$ | Diện tích dưới đường cong |
| Quy tắc chuỗi | Chain rule | $\frac{dz}{dx} = \frac{dz}{dy} \cdot \frac{dy}{dx}$ | Đạo hàm hàm hợp |
| Lan truyền ngược | Backpropagation | Backprop | Chain rule trên graph |
| Hạ gradient | Gradient descent | GD | $w = w - \alpha \nabla L$ |
| Tốc độ học | Learning rate | $\alpha$, $\eta$, lr | Bước nhảy mỗi lần cập nhật |
| Hàm mất mát | Loss function | $L$, $\mathcal{L}$, $J$ | Thước đo sai số |
| Hàm lồi | Convex function | | Có 1 cực tiểu duy nhất |
| Cực tiểu cục bộ | Local minimum | | GD có thể bị kẹt |
| Cực tiểu toàn cục | Global minimum | | Đáy thấp nhất |
| Tối ưu | Optimizer | SGD, Adam... | Thuật toán cập nhật weight |
| Động lượng | Momentum | $\beta$ | Nhớ hướng đi trước |

---

## 🎲 Chương 3: Xác Suất & Thống Kê

| Tiếng Việt | English | Ký hiệu | Ghi chú |
|-----------|---------|---------|---------|
| Xác suất | Probability | $P(A)$ | 0 ≤ P ≤ 1 |
| Xác suất có điều kiện | Conditional probability | $P(A|B)$ | XS của A khi biết B |
| Biến ngẫu nhiên | Random variable | $X$ | Kết quả của thí nghiệm |
| Kỳ vọng | Expected value | $E[X]$, $\mu$ | Trung bình dài hạn |
| Phương sai | Variance | $Var(X)$, $\sigma^2$ | Đo mức phân tán |
| Độ lệch chuẩn | Standard deviation | $\sigma$ | $\sqrt{Var(X)}$ |
| Hiệp phương sai | Covariance | $Cov(X,Y)$ | Đo mức đồng biến |
| Hệ số tương quan | Correlation coefficient | $\rho_{XY}$ | Cov chuẩn hóa, ∈ [-1, 1] |
| Phân phối chuẩn | Normal / Gaussian distribution | $\mathcal{N}(\mu, \sigma^2)$ | Hình chuông |
| Phân phối Bernoulli | Bernoulli distribution | $Ber(p)$ | Nhị phân: 0 hoặc 1 |
| Phân phối Poisson | Poisson distribution | $Poi(\lambda)$ | Đếm sự kiện hiếm |
| Định lý Bayes | Bayes' theorem | $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$ | Cập nhật niềm tin |
| Ước lượng hợp lý cực đại | Maximum Likelihood Estimation | MLE | Tìm θ tốt nhất cho dữ liệu |
| Entropy | Entropy | $H(X)$ | Đo mức bất định |
| Entropy chéo | Cross-entropy | $H(p, q)$ | Loss function #1 |
| Phân kỳ KL | KL Divergence | $D_{KL}(p\|q)$ | Khoảng cách 2 phân phối |
| Hồi quy tuyến tính | Linear regression | | $\hat{y} = w^Tx + b$ |

---

## 🔢 Chương 4: Toán Rời Rạc

| Tiếng Việt | English | Ký hiệu | Ghi chú |
|-----------|---------|---------|---------|
| Đồ thị | Graph | $G = (V, E)$ | Đỉnh + Cạnh |
| Đỉnh | Vertex / Node | $v \in V$ | |
| Cạnh | Edge | $e \in E$ | |
| Bậc | Degree | $\deg(v)$ | Số cạnh kề đỉnh |
| Duyệt theo chiều rộng | Breadth-First Search | BFS | Sóng lan ra |
| Duyệt theo chiều sâu | Depth-First Search | DFS | Đi sâu trước |
| Đường đi ngắn nhất | Shortest path | | Dijkstra, BFS |
| Hoán vị | Permutation | $P(n,k)$ | Có thứ tự |
| Tổ hợp | Combination | $C(n,k)$, $\binom{n}{k}$ | Không thứ tự |
| Độ phức tạp | Time complexity | $O(f(n))$ | Tốc độ thuật toán |

---

## ⚡ Chương 6: Tối Ưu Hóa Nâng Cao

| Tiếng Việt | English | Ký hiệu | Ghi chú |
|-----------|---------|---------|---------|
| Ma trận Jacobian | Jacobian matrix | $J$ | Đạo hàm riêng đa output |
| Ma trận Hessian | Hessian matrix | $H$ | Đạo hàm bậc 2 |
| Điểm yên ngựa | Saddle point | | Gradient = 0 nhưng không phải min/max |
| Khai triển Taylor | Taylor expansion | | Xấp xỉ hàm bằng đa thức |
| Nhân tử Lagrange | Lagrange multiplier | $\lambda$ | Tối ưu có ràng buộc |
| Giá bóng | Shadow price | $\lambda$ | Giá trị Lagrange multiplier |
| Gradient biến mất | Vanishing gradient | | Mạng sâu + Sigmoid |
| Gradient bùng nổ | Exploding gradient | | Weight lớn × nhiều layers |
| Cắt gradient | Gradient clipping | $\|\nabla\| \leq c$ | Giới hạn gradient |
| Chuẩn hóa batch | Batch normalization | BN | Ổn định output mỗi layer |
| Kết nối tắt | Residual / Skip connection | $h_l = f(h_{l-1}) + h_{l-1}$ | ResNet |
| Lịch trình tốc độ học | Learning rate scheduling | | Thay đổi lr theo epoch |

---

## 📊 Chương 7: Thống Kê Suy Diễn

| Tiếng Việt | English | Ký hiệu | Ghi chú |
|-----------|---------|---------|---------|
| Kiểm định giả thuyết | Hypothesis testing | | H₀ vs H₁ |
| Giả thuyết không | Null hypothesis | $H_0$ | "Không có sự khác biệt" |
| Giả thuyết đối | Alternative hypothesis | $H_1$ | "CÓ sự khác biệt" |
| Mức ý nghĩa | Significance level | $\alpha$ | Thường = 0.05 |
| Giá trị p | p-value | $p$ | XS dữ liệu nếu H₀ đúng |
| Khoảng tin cậy | Confidence interval | CI | Khoảng ước lượng |
| Sai số chuẩn | Standard error | SE = $\frac{s}{\sqrt{n}}$ | Sai số của trung bình mẫu |
| Hồi quy logistic | Logistic regression | | Phân loại nhị phân |
| Hàm sigmoid | Sigmoid function | $\sigma(z) = \frac{1}{1+e^{-z}}$ | Ép output vào (0,1) |
| Chuỗi Markov | Markov chain | | Trạng thái → Trạng thái |
| Ma trận chuyển tiếp | Transition matrix | $P$ | XS chuyển trạng thái |
| Phân phối dừng | Stationary distribution | $\vec{\pi}$ | $\vec{\pi} = \vec{\pi}P$ |
| Phương pháp Monte Carlo | Monte Carlo method | MC | Ước lượng bằng sampling |
| Thiên lệch | Bias | | Sai lệch hệ thống |
| Phương sai (model) | Variance | | Dao động giữa các lần train |
| Quá khớp | Overfitting | | Fit noise thay vì pattern |
| Dưới khớp | Underfitting | | Model quá đơn giản |
| Chính quy hóa | Regularization | L1, L2 | Chống overfitting |

---

## 🔤 KÝ HIỆU TOÁN HỌC THƯỜNG GẶP

| Ký hiệu | Đọc là | Ý nghĩa |
|---------|--------|---------|
| $\sum$ | Sigma | Tổng |
| $\prod$ | Pi (hoa) | Tích |
| $\int$ | Integral | Tích phân |
| $\partial$ | Partial / Del | Đạo hàm riêng |
| $\nabla$ | Nabla / Del | Gradient |
| $\forall$ | For all | Với mọi |
| $\exists$ | There exists | Tồn tại |
| $\in$ | Belongs to | Thuộc |
| $\subset$ | Subset | Tập con |
| $\approx$ | Approximately | Xấp xỉ |
| $\propto$ | Proportional to | Tỷ lệ với |
| $\mathbb{R}$ | R | Tập số thực |
| $\mathbb{R}^n$ | R-n | Không gian n chiều |
| $\arg\min$ | Argmin | Giá trị cho min |
| $\arg\max$ | Argmax | Giá trị cho max |
| $\|x\|$ | Norm of x | Độ dài / Chuẩn |
| $x^T$ | x transpose | Chuyển vị |
| $\hat{y}$ | y-hat | Giá trị dự đoán |
| $\theta$ | Theta | Tham số model |
| $\epsilon$ | Epsilon | Số rất nhỏ |

---

> *"Mỗi khi đọc paper và gặp ký hiệu lạ, quay lại đây. Dần dần, bạn sẽ 'đọc' toán như đọc code."*
