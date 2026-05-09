# CHƯƠNG 6: TỐI ƯU HÓA NÂNG CAO — NGHỆ THUẬT TÌM ĐÍCH

## 📋 Metadata Chương

| Mục | Chi tiết |
|-----|---------|
| **Tên chương** | Chương 6: Tối ưu hóa Nâng cao — Nghệ thuật Tìm đích |
| **Hook** | Gradient Descent là la bàn. Chương này cho bạn bản đồ địa hình, kính viễn vọng, và đôi giày chuyên dụng. |
| **Đối tượng** | Sinh viên AI/ML muốn hiểu sâu về training; kỹ sư ML muốn debug khi model không hội tụ |
| **Điều kiện tiên quyết** | Ch.2 (Đạo hàm riêng, Gradient Descent). Ch.1 (Eigenvalue). |
| **Thời gian học** | ~50 phút (Track 🟢🟡) \| ~90 phút (Track 🔵) |
| **Mục tiêu đầu ra** | Sau chương này, người học có thể: |
| | • **Tính và giải thích** Jacobian Matrix — tại sao nó là backprop |
| | • **Dùng Hessian** để phân loại: cực tiểu / cực đại / yên ngựa |
| | • **Nhận ra** vanishing gradient, giải thích tại sao ReLU + ResNet giúp |
| | • **Cài đặt** Lagrange để tối ưu có ràng buộc (ứng dụng SVM, budgeting) |
| | • **Chọn LR Scheduler** phù hợp và giải thích lý do |

---

## 📖 ADVANCED OPTIMIZATION


> *"Gradient Descent là chiếc la bàn. Chương này cho bạn bản đồ địa hình, kính viễn vọng, và đôi giày chuyên dụng — để không chỉ TÌM ĐƯỢC đáy, mà tìm được đáy NHANH NHẤT và ĐÚNG NHẤT."*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 6: "Vượt qua khủng hoảng"
>
> *N-42 đang ở layer thứ 50 trong một mạng GPT. Gradient từ output truyền ngược... nhưng đến N-42 thì gần bằng 0! Nó bị "câm" — không học được gì (vanishing gradient). May thay, mạng lưới ResNet (Mạng phần dư) cho nó một 'đường tắt' bằng bộ chuyển lưu đặc biệt, ReLU (Hàm kích hoạt tuyến tính) giúp khuếch đại tín hiệu để giữ gradient sống sót, và Lagrange dạy nó cách học trong ràng buộc. N-42 sống sót — và mạnh mẽ hơn bao giờ hết.*

---

## 🎯 Ví dụ đa lĩnh vực: Tối ưu hóa ở khắp nơi

Trước khi đi vào chi tiết, hãy nhìn bức tranh lớn — tối ưu hóa có ràng buộc xuất hiện **mọi nơi** trong đời sống:

| Ngành | Bài toán | Ràng buộc |
|-------|---------|----------|
| **📚 Giáo dục** | Phân bổ 24h ôn 5 môn thi để điểm TB cao nhất | Tổng thời gian = 24h, mỗi môn ≥ 2h |
| **🏥 Y tế** | Phân bổ 100 liều thuốc cho 5 bệnh viện | Mỗi BV ≥ 10 liều, tổng = 100 |
| **🌾 Nông nghiệp** | Tối ưu diện tích trồng 3 loại cây | Tổng đất = 10ha, nước tưới ≤ 50m³/ngày |
| **💰 Tài chính** | Tối ưu lợi nhuận danh mục đầu tư | Rủi ro ≤ ngưỡng, tổng vốn = 1 tỷ |
| **📱 Marketing** | Phân bổ ngân sách quảng cáo 3 kênh | Tổng budget = 50 triệu, mỗi kênh ≥ 5 triệu |

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: Tại sao Gradient Descent chưa đủ?

Bạn đã biết GD — "lăn bi xuống đồi trong sương mù". Nhưng trong thực tế:

1. **Bề mặt cong phức tạp** — Có nơi dốc đứng, có nơi gần phẳng → GD bị chậm
2. **Saddle points** — Điểm "yên ngựa" không phải cực tiểu nhưng gradient = 0 → GD bị kẹt
3. **Ràng buộc** — "Tối đa lợi nhuận nhưng ngân sách ≤ 100 triệu" → GD thông thường bất lực
4. **Gradient biến mất/bùng nổ** — Mạng sâu 100 tầng → Gradient nhân 100 lần → quá nhỏ hoặc quá lớn

Chương này giải quyết TẤT CẢ các vấn đề trên.

---

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Khái niệm | Một câu tóm tắt | Tại sao cần cho AI |
|---|-----------|-----------------|---------------------|
| 1 | **Jacobian** | Ma trận đạo hàm cho hàm nhiều output | Backprop = nhân chuỗi Jacobian |
| 2 | **Hessian** | Đạo hàm bậc 2 — Độ cong bề mặt | Phân biệt min/max/saddle |
| 3 | **Taylor** | Xấp xỉ hàm phức tạp bằng đa thức | GD = Taylor bậc 1 |
| 4 | **Lagrange** | Tối ưu có ràng buộc | SVM, Regularization |
| 5 | **Vanishing Gradient** | Gradient → 0 qua nhiều layers | Tại sao cần ReLU, ResNet |
| 6 | **LR Scheduling** | Thay đổi tốc độ học theo thời gian | Training thực tế |
| 7 | **Numerical Stability** | Máy tính cũng sai nếu không cẩn thận | Softmax overflow, log(0) |

> 📋 **Prerequisites:** Ch.1 (Eigenvalue), Ch.2 (Đạo hàm riêng, GD). Nên đọc: Ch.1 §2B (Định thức).
>
> ⏱️ **Thời gian đọc:** ~50 phút (Track A) | ~90 phút (Track B)

---

## 🗺️ BẠN ĐANG Ở ĐÂY

```
  ... ━━▶ Ch.2 Giải tích ━━▶ (Ch.3, 4, 5) ━━▶ 📍 Ch.6 TỐI ƯU HÓA ━━▶ Ch.7 (Cuối)
           (Đã học GD)                         BẠN ĐANG Ở ĐÂY
```

**Phần này cần học trước:** Chương 2 (Đạo hàm riêng, Vector Gradient).
**Chương này mở khóa:** Hiểu sâu về cách train một mô hình Deep Learning cực lớn mà không bị crash. Hiểu được các Optimizer hiện đại (Adam, RMSProp).

---

## 📦 ÔN NHANH TOÁN PHỔ THÔNG

<details>
<summary>🔢 <b>Ôn nhanh 1: Đạo hàm hàm nhiều biến</b> (Bấm để mở)</summary>

Đạo hàm riêng là đạo hàm theo từng biến, coi các biến khác như hằng số.
VD: $f(x, y) = x^2 + 3y$.
- Đạo hàm theo $x$ (gọi là $\frac{\partial f}{\partial x}$): $2x + 0$ = $2x$.
- Đạo hàm theo $y$ (gọi là $\frac{\partial f}{\partial y}$): $0 + 3$ = $3$.
Vector Gradient chính là gom các anh đạo hàm riêng này lại: $\nabla f = [2x, 3]$.

</details>

---

## 🏷️ HƯỚNG DẪN ĐỌC — Hệ thống 3 tầng

| Tầng | Label | Dành cho ai? | Cần gì? |
|------|-------|-------------|--------|
| 🟢 | **Hiểu bằng trực giác** | Tất cả mọi người | Không cần tính Toán |
| 🟡 | **Lý thuyết Toán học** | Muốn phân tích sâu | Biết sơ qua đạo hàm |
| 🔵 | **Hiểu bằng code Python** | Developer / Coder | scipy, numpy cơ bản |

---

## PHẦN 1: JACOBIAN MATRIX — "BẢN ĐỒ ĐẠO HÀM ĐA CHIỀU"

### 🟢 1.1 Từ gradient đến Jacobian

Ở Chương 2, ta đã học **gradient** — đạo hàm của hàm 1 output nhiều input:

$$\nabla f = \begin{pmatrix} \frac{\partial f}{\partial x_1} \\ \vdots \\ \frac{\partial f}{\partial x_n} \end{pmatrix}$$

Nhưng khi hàm có **nhiều output** (ví dụ: neural network layer biến vector 3D → vector 5D)?

**Jacobian** = Ma trận chứa TẤT CẢ đạo hàm riêng:

$$J = \begin{pmatrix} \frac{\partial f_1}{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n} \\ \vdots & \ddots & \vdots \\ \frac{\partial f_m}{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n} \end{pmatrix}$$

- Hàng $i$: Đạo hàm của output $f_i$ theo tất cả input
- Cột $j$: Ảnh hưởng của input $x_j$ lên tất cả output
- Kích thước: $m \times n$ (m outputs, n inputs)

> 🌱 **VÍ DỤ VỠ LÒNG — Tính Jacobian (1 phút)**
> Bạn có một hàm biến đổi 2 con số thành 2 con số khác: $f(x, y) = [x \times y, x + y]$.
> - Phân tích cục: $(x \times y)$ có đạo hàm theo $x$ là $y$, theo $y$ là $x$.
> - Cục 2: $(x + y)$ có đạo hàm theo $x$ là $1$, theo $y$ là $1$.
> 
> Lắp vào ma trận Jacobian: 
> Dòng 1: $[y, x]$
> Dòng 2: $[1, 1]$
> Vậy $J = \begin{pmatrix} y & x \\ 1 & 1 \end{pmatrix}$. Tại điểm $x=2, y=3$, Ma trận $J = \begin{pmatrix} 3 & 2 \\ 1 & 1 \end{pmatrix}$. Dễ mà!

#### 🎯 Liên tưởng: Bàn mixer nâng cấp 🎛️

Ở Chương 2, mixer có 1 loa (1 output). Bây giờ mixer có **5 loa** (5 outputs) và **3 nút vặn** (3 inputs). Jacobian = bảng ghi "vặn nút j ảnh hưởng loa i bao nhiêu" — tổng 15 con số.

#### 🎯 Ví dụ kinh tế: Phân tích tác động chính sách 📊

Chính phủ thay đổi 3 chính sách: Thuế ($x_1$), Lãi suất ($x_2$), Chi tiêu công ($x_3$).

Tác động lên 4 chỉ số: GDP ($f_1$), Lạm phát ($f_2$), Thất nghiệp ($f_3$), Tỷ giá ($f_4$).

$$J_{chính\_sách} = \begin{pmatrix} \frac{\partial GDP}{\partial Thuế} & \frac{\partial GDP}{\partial Lãi\_suất} & \frac{\partial GDP}{\partial Chi\_tiêu} \\ \frac{\partial Lạm\_phát}{\partial Thuế} & \cdots & \cdots \\ \vdots & & \vdots \\ \frac{\partial Tỷ\_giá}{\partial Thuế} & \cdots & \frac{\partial Tỷ\_giá}{\partial Chi\_tiêu} \end{pmatrix}$$

→ Jacobian cho biết "tăng thuế 1% thì GDP, lạm phát, thất nghiệp thay đổi bao nhiêu?"

### 🟡 1.2 Tại sao Jacobian quan trọng trong DL?

**Backpropagation = Nhân chuỗi Jacobian!**

Trong neural network nhiều layer: $\vec{x} \xrightarrow{f_1} \vec{h_1} \xrightarrow{f_2} \vec{h_2} \xrightarrow{f_3} \vec{y}$

$$\frac{\partial \vec{y}}{\partial \vec{x}} = J_3 \cdot J_2 \cdot J_1$$

Đây chính là **Chain Rule** cho vector (Chương 2), nhưng mỗi đạo hàm giờ là MA TRẬN.

### 🔵 1.3 Code Python: Jacobian trực quan

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# JACOBIAN MATRIX
# =============================================

def f(x):
    """Hàm f: R² → R² 
    f1(x,y) = x² + y
    f2(x,y) = x·y + sin(y)
    """
    return np.array([x[0]**2 + x[1], x[0]*x[1] + np.sin(x[1])])

def jacobian_numerical(f, x, eps=1e-5):
    """Tính Jacobian bằng xấp xỉ số"""
    n = len(x)
    m = len(f(x))
    J = np.zeros((m, n))
    for j in range(n):
        x_plus = x.copy(); x_plus[j] += eps
        x_minus = x.copy(); x_minus[j] -= eps
        J[:, j] = (f(x_plus) - f(x_minus)) / (2 * eps)
    return J

# Tính Jacobian tại (1, π/4)
x0 = np.array([1.0, np.pi/4])
J = jacobian_numerical(f, x0)

print("🧮 JACOBIAN MATRIX:")
print(f"   Điểm x = ({x0[0]:.2f}, {x0[1]:.4f})")
print(f"   f(x) = ({f(x0)[0]:.4f}, {f(x0)[1]:.4f})")
print(f"\n   J = ")
print(f"   [{J[0,0]:+.4f}  {J[0,1]:+.4f}]   ← ∂f₁/∂(x,y)")
print(f"   [{J[1,0]:+.4f}  {J[1,1]:+.4f}]   ← ∂f₂/∂(x,y)")

# Jacobian chính xác (tính tay)
J_exact = np.array([
    [2*x0[0], 1],                    # ∂f₁/∂x=2x, ∂f₁/∂y=1
    [x0[1], x0[0] + np.cos(x0[1])]   # ∂f₂/∂x=y, ∂f₂/∂y=x+cos(y)
])
print(f"\n   Kiểm tra (tính tay):")
print(f"   [{J_exact[0,0]:+.4f}  {J_exact[0,1]:+.4f}]")
print(f"   [{J_exact[1,0]:+.4f}  {J_exact[1,1]:+.4f}]")
print(f"   Sai số: {np.max(np.abs(J - J_exact)):.2e} ✅")

# =============================================
# TRỰC QUAN HÓA: Jacobian là "biến dạng cục bộ"
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Lưới gốc
theta = np.linspace(0, 2*np.pi, 50)
circle_x = 0.3 * np.cos(theta) + x0[0]
circle_y = 0.3 * np.sin(theta) + x0[1]

# Áp dụng f lên lưới
mapped = np.array([f(np.array([cx, cy])) for cx, cy in zip(circle_x, circle_y)])

# Hình 1: Không gian gốc
ax1 = axes[0]
ax1.plot(circle_x, circle_y, 'b-', linewidth=2)
ax1.plot(x0[0], x0[1], 'r*', markersize=15, label=f'x₀ = ({x0[0]:.1f}, {x0[1]:.2f})')
ax1.set_title('📥 Không gian INPUT\n(Hình tròn)', fontsize=13, fontweight='bold')
ax1.set_xlabel('x₁'); ax1.set_ylabel('x₂')
ax1.legend(fontsize=10); ax1.grid(True, alpha=0.3)
ax1.set_aspect('equal')

# Hình 2: Sau biến đổi f
ax2 = axes[1]
ax2.plot(mapped[:, 0], mapped[:, 1], 'r-', linewidth=2)
ax2.plot(f(x0)[0], f(x0)[1], 'b*', markersize=15, label=f'f(x₀)')
ax2.set_title('📤 Không gian OUTPUT\n(Hình tròn bị biến dạng bởi Jacobian)', fontsize=13, fontweight='bold')
ax2.set_xlabel('f₁'); ax2.set_ylabel('f₂')
ax2.legend(fontsize=10); ax2.grid(True, alpha=0.3)

plt.suptitle('🧮 Jacobian Matrix: Mô tả cách hàm "biến dạng" không gian cục bộ', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong6_jacobian.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Jacobian xuất hiện "ngầm" trong mọi lần backprop. PyTorch tự tính Jacobian qua autograd. Sinh viên không cần tính tay, nhưng cần hiểu: "Mỗi layer trong neural network có 1 Jacobian, và backprop nhân tất cả lại."

---

> ✅ **CHECKPOINT 1 — Jacobian Matrix**
>
> 1. Vector Gradient và Jacobian Matrix giống và khác nhau ở điểm cốt lõi nào?
> 2. Sự "khủng khiếp" của thuật toán Backpropagation (truyền ngược) nằm ở đâu theo góc độ Jacobian?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Đều là đạo hàm riêng. Điểm khác là: Gradient chỉ thu được khi hàm trả về **1 giá trị (ví dụ Loss function)**. Còn Jacobian là bản tổng quát khi hàm trả về **nhiều giá trị (vd một Layer Neural Network chuyển vector 100 features thành vector 50 features)** thì phải xài ma trận Jacobian 50x100.
> 2. Nó nhân liên tiếp hàng chục/trăm ma trận Jacobian khổng lồ lại với nhau! (Backpropagation = Chain Rule cho ma trận).
>
> </details>

---

## PHẦN 2: HESSIAN MATRIX — "ĐỊA HÌNH CHI TIẾT"

### 🟢 2.1 Từ đạo hàm bậc 1 đến bậc 2

- **Gradient** (bậc 1): Cho biết **hướng** dốc nhất → "Đi đâu?"
- **Hessian** (bậc 2): Cho biết **độ cong** của bề mặt → "Bề mặt phẳng hay cong? Cong bao nhiêu?"

$$H = \begin{pmatrix} \frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2} \\ \frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} \end{pmatrix}$$

Hessian là **ma trận đối xứng** (khi hàm đủ mịn).

#### 🎯 Liên tưởng: Lái xe trên đường cong 🚗

- **Gradient** = Hướng đường đang đi (thẳng, rẽ trái, phải)
- **Hessian** = Độ cong của đường (cua gấp hay cua thoải)
  - Eigenvalue của Hessian dương → "Thung lũng" (cực tiểu) ✅
  - Eigenvalue âm → "Đỉnh đồi" (cực đại)
  - Eigenvalue hỗn hợp → "Yên ngựa" (saddle point) ⚠️

### 2.2 Phân loại điểm dừng

Khi $\nabla f = 0$ (gradient = 0), đó là **điểm dừng**. Nhưng đó là cực tiểu, cực đại, hay yên ngựa?

| Eigenvalue của H | Loại điểm | Liên tưởng |
|-----------------|-----------|------------|
| Tất cả > 0 | **Cực tiểu** (minimum) | 🏔️ Đáy thung lũng |
| Tất cả < 0 | **Cực đại** (maximum) | ⛰️ Đỉnh núi |
| Hỗn hợp (+/-) | **Yên ngựa** (saddle) | 🐎 Lưng ngựa |

#### 🎯 Ví dụ tài chính: Tối ưu portfolio 📈

Hàm rủi ro portfolio $R(\vec{w})$ phụ thuộc tỷ trọng các cổ phiếu. Hessian cho biết:
- Eigenvalue tất cả dương → Có điểm rủi ro tối thiểu duy nhất ✅ (Efficient Frontier)
- Saddle point → Cần cẩn thận, có thể đang ở "ảo tưởng" an toàn ⚠️

### 🟡 2.3 Newton's Method — "GD có bản đồ"

Standard GD chỉ dùng gradient (bậc 1). Newton's Method dùng cả **Hessian** (bậc 2):

$$\vec{x}_{t+1} = \vec{x}_t - H^{-1} \cdot \nabla f$$

| So sánh | Gradient Descent | Newton's Method |
|---------|-----------------|-----------------|
| Dùng | Gradient (bậc 1) | Gradient + Hessian (bậc 2) |
| Tốc độ hội tụ | Tuyến tính | **Bậc 2** (nhanh hơn nhiều) |
| Chi phí mỗi bước | $O(n)$ | $O(n^3)$ (tính $H^{-1}$) |
| Phù hợp | Dữ liệu lớn, nhiều tham số | Dữ liệu nhỏ, ít tham số |

> 💡 **Trong Deep Learning:** Newton's Method quá đắt (triệu tham số → Hessian triệu × triệu!). Thay vào đó dùng các **xấp xỉ**: L-BFGS, Adam (xấp xỉ bậc 2 qua adaptive learning rate).

### 🔵 2.4 Code Python: Hessian & Saddle Points

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# =============================================
# HESSIAN & PHÂN LOẠI ĐIỂM DỪNG
# =============================================

def f_saddle(x, y):
    """Hàm có saddle point tại gốc: f = x² - y²"""
    return x**2 - y**2

def f_minimum(x, y):
    """Hàm có cực tiểu tại gốc: f = x² + y²"""
    return x**2 + y**2

def f_monkey(x, y):
    """Monkey saddle: f = x³ - 3xy²"""
    return x**3 - 3*x*y**2

fig, axes = plt.subplots(1, 3, figsize=(20, 6), subplot_kw={'projection': '3d'})

x = np.linspace(-2, 2, 100)
y = np.linspace(-2, 2, 100)
X, Y = np.meshgrid(x, y)

functions = [
    ("✅ Cực tiểu (x² + y²)\nH eigenvalues: [2, 2] (tất cả +)", f_minimum, '#4CAF50'),
    ("🐎 Yên ngựa (x² - y²)\nH eigenvalues: [2, -2] (hỗn hợp)", f_saddle, '#FF5722'),
    ("🐒 Monkey saddle (x³ - 3xy²)\nH = 0 tại gốc!", f_monkey, '#9C27B0'),
]

for ax, (title, func, color) in zip(axes, functions):
    Z = func(X, Y)
    ax.plot_surface(X, Y, Z, alpha=0.6, cmap='coolwarm')
    ax.scatter(0, 0, func(0, 0), color='red', s=200, zorder=5, marker='*')
    ax.set_title(title, fontsize=10, fontweight='bold')
    ax.set_xlabel('x'); ax.set_ylabel('y'); ax.set_zlabel('f')

plt.suptitle('🏔️ Hessian phân loại: Cực tiểu vs Yên ngựa vs Monkey Saddle', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong6_hessian.png', dpi=150, bbox_inches='tight')
plt.show()

# Tính Hessian
print("🧮 PHÂN LOẠI ĐIỂM DỪNG BẰNG HESSIAN:")
print("=" * 55)

cases = [
    ("x² + y²", np.array([[2, 0], [0, 2]])),
    ("x² - y²", np.array([[2, 0], [0, -2]])),
    ("x² + xy + y²", np.array([[2, 1], [1, 2]])),
]

for name, H in cases:
    eigvals = np.linalg.eigvals(H)
    if all(eigvals > 0):
        loai = "✅ CỰC TIỂU"
    elif all(eigvals < 0):
        loai = "⛰️ CỰC ĐẠI"
    else:
        loai = "🐎 YÊN NGỰA"
    print(f"   f = {name:15s} | H eigenvalues = [{eigvals[0]:+.1f}, {eigvals[1]:+.1f}] → {loai}")
```

> 📌 **Lưu ý cho giảng viên:** Saddle points là vấn đề thực sự trong deep learning (kích thước cao → gần như mọi điểm dừng đều là saddle point). Nhưng SGD + noise giúp "thoát" saddle point. Đây là lý do SGD (có noise) tốt hơn full-batch GD (chính xác nhưng dễ kẹt).

---

> ✅ **CHECKPOINT 2 — Hessian & Điểm Dừng**
>
> 1. Hessian Matrix đo lường điều gì của hàm số?
> 2. Tại sao Saddle Point (điểm yên ngựa) lại làm khổ các thuật toán Gradient thông thường?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Độ cong** của bề mặt hàm số (gradient bậc 2). Nó cho biết bề mặt lồi, lõm, hay võng như yên ngựa.
> 2. Vì tại yên ngựa, đạo hàm bậc 1 $\nabla = 0$ (phẳng lỳ) khiến GD tưởng đó là đáy thung lũng và dừng bước. Cần có đà (Momentum) hoặc nhiễu ngẫu nhiên (SGD) để lăn tuột xuống dốc tiếp!
>
> </details>

---

## PHẦN 3: TAYLOR EXPANSION — "XẤP XỈ MỌI THỨ"

### 🟢 3.1 Ý tưởng cốt lõi

**Taylor expansion** = Xấp xỉ hàm phức tạp bằng **đa thức** — dễ tính hơn!

$$f(x) \approx f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \frac{f'''(a)}{3!}(x-a)^3 + \ldots$$

#### 🎯 Liên tưởng: Zoom vào bản đồ 🗺️

Nhìn từ xa, bề mặt phức tạp. Nhưng **zoom đủ gần**, mọi bề mặt đều gần giống:
- Bậc 0: Điểm → "Tôi đang ở đâu?" (giá trị hàm)
- Bậc 1: Đường thẳng → "Dốc bao nhiêu?" (gradient)
- Bậc 2: Parabol → "Cong bao nhiêu?" (Hessian)

#### 🎯 Tại sao cần trong ML?

| Ứng dụng | Dùng Taylor bậc mấy? |
|----------|----------------------|
| **Gradient Descent** | Bậc 1: $f(x+\delta) \approx f(x) + \nabla f \cdot \delta$ |
| **Newton's Method** | Bậc 2: $f(x+\delta) \approx f(x) + \nabla f \cdot \delta + \frac{1}{2}\delta^T H \delta$ |
| **Adam optimizer** | Xấp xỉ bậc 2 thông qua moment ước lượng |
| **Activation functions** | $\sigma(x) \approx 0.5 + 0.25x$ quanh $x=0$ (tuyến tính!) |

> ⚠️ **SAI LẦM PHỔ BIẾN:** Đừng nghĩ rằng mô hình tính GD là nó giải tích phân, đạo hàm bằng công thức bậc cao như hồi Đại học. Máy tính không biết công thức! Nó xấp xỉ giá trị đạo hàm bằng chuỗi Taylor hoặc hiệu hữu hạn. Nếu bạn đòi tính đạo hàm cực kỳ chính xác (nhiều bậc) thì thời gian chạy sẽ vỡ trận do quá lâu.

### 🔵 3.2 Code Python: Taylor Approximation

```python
import numpy as np
import matplotlib.pyplot as plt
from math import factorial

# =============================================
# TAYLOR EXPANSION — XẤP XỈ HÀM SỐ
# =============================================

def f(x):
    """Hàm gốc: sin(x)"""
    return np.sin(x)

# Các xấp xỉ Taylor tại a = 0
def taylor_sin(x, order):
    """Taylor expansion of sin(x) at a=0"""
    result = np.zeros_like(x, dtype=float)
    for n in range(order + 1):
        k = 2*n + 1  # sin chỉ có bậc lẻ
        if k > 2*order + 1: break
        result += ((-1)**n / factorial(k)) * x**k
    return result

x = np.linspace(-4*np.pi, 4*np.pi, 500)

fig, ax = plt.subplots(figsize=(14, 7))

# Hàm gốc
ax.plot(x, f(x), 'k-', linewidth=3, label='sin(x) (gốc)', zorder=10)

# Các bậc xấp xỉ
colors = ['#f44336', '#FF9800', '#4CAF50', '#2196F3', '#9C27B0']
orders = [1, 3, 5, 7, 9]

for order, color in zip(orders, colors):
    y_approx = taylor_sin(x, order)
    y_approx = np.clip(y_approx, -3, 3)  # Clip cho dễ nhìn
    ax.plot(x, y_approx, '--', color=color, linewidth=1.5, 
            label=f'Taylor bậc {2*order+1}', alpha=0.8)

ax.set_ylim(-3, 3)
ax.set_title('📐 Taylor Expansion: Xấp xỉ sin(x)\nCàng nhiều bậc → Càng chính xác xa tâm', 
             fontsize=14, fontweight='bold')
ax.set_xlabel('x', fontsize=12); ax.set_ylabel('f(x)', fontsize=12)
ax.legend(fontsize=10, loc='upper right'); ax.grid(True, alpha=0.3)
ax.axhline(y=0, color='gray', linewidth=0.5)
ax.axvline(x=0, color='gray', linewidth=0.5)

plt.tight_layout()
plt.savefig('chuong6_taylor.png', dpi=150, bbox_inches='tight')
plt.show()

print("💡 INSIGHT:")
print("   Taylor bậc 1 (đường thẳng) = Gradient Descent")
print("   Taylor bậc 2 (parabol) = Newton's Method")
print("   Bậc càng cao → Chính xác hơn nhưng TÍNH TOÁN ĐẮT HƠN")
print("   → AI thường dùng bậc 1 (GD) hoặc xấp xỉ bậc 2 (Adam)")
```

---

> ✅ **CHECKPOINT 3 — Taylor Expansion**
>
> 1. Ý nghĩa sâu xa của chuỗi Taylor là gì?
> 2. Tại sao Deep Learning không xài Taylor bậc 20 để tính loss cho siêu chính xác?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Dùng các hàm dễ tính (đa thức: $x, x^2, x^3...$) để xấp xỉ, giả dạng các hàm phức tạp (sin, cos, log) tại một vùng không gian nhỏ.
> 2. Tính toán đạo hàm đến bậc 20 cho hàng triệu tham số tốn quá nhiều dung lượng bộ nhớ và thời gian tính. GD (Taylor bậc 1) là đủ tốt và chi phí rẻ.
>
> </details>

---

## PHẦN 4: LAGRANGE MULTIPLIER — "TỐI ƯU CÓ RÀNG BUỘC"

### 🟢 4.1 Vấn đề: Tối ưu khi bị "gò bó"

GD tìm minimum **tự do** — không giới hạn. Nhưng đời thực luôn có ràng buộc:

- 💰 "Tối đa lợi nhuận" nhưng **ngân sách ≤ 100 triệu**
- 📦 "Chở hàng nhiều nhất" nhưng **trọng tải ≤ 10 tấn**
- 🎓 "Phân bổ thời gian ôn thi" cho 5 môn, **tổng = 24 giờ**
- 🤖 "Tối ưu model" nhưng **||w|| ≤ C** (regularization!)

$$\min f(\vec{x}) \quad \text{sao cho} \quad g(\vec{x}) = 0$$

### 🟡 4.2 Ý tưởng Lagrange

Thay vì giải 2 bài riêng (tối ưu + ràng buộc), gộp thành 1:

$$\mathcal{L}(\vec{x}, \lambda) = f(\vec{x}) - \lambda \cdot g(\vec{x})$$

- $\lambda$ (lambda) = **nhân tử Lagrange** = "giá bóng" (shadow price) của ràng buộc
- Giải: $\nabla_{\vec{x}} \mathcal{L} = 0$ và $\nabla_\lambda \mathcal{L} = 0$

#### 🎯 Ví dụ kinh tế: Phân bổ ngân sách marketing ✈️

Công ty có **100 triệu** cho 2 kênh quảng cáo:
- Facebook: Hiệu quả $f_1(x) = 10\sqrt{x}$ (triệu impression)
- Google: Hiệu quả $f_2(y) = 15\sqrt{y}$

Tối đa: $\max 10\sqrt{x} + 15\sqrt{y}$ sao cho $x + y = 100$

#### 🎯 Ví dụ ML: SVM (Support Vector Machine)

SVM chính xác là bài toán Lagrange:

$$\min \frac{1}{2}\|\vec{w}\|^2 \quad \text{sao cho} \quad y_i(\vec{w} \cdot \vec{x}_i + b) \geq 1$$

→ "Tìm đường phân chia 2 class sao cho margin LỚN NHẤT" = constrained optimization!

### 🔵 4.3 Code Python: Lagrange trực quan

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import minimize

# =============================================
# LAGRANGE MULTIPLIER — TỐI ƯU CÓ RÀNG BUỘC
# =============================================

# Bài toán: max f(x,y) = 10√x + 15√y sao cho x + y = 100
# Tương đương: min -f(x,y)

def objective(vars):
    x, y = vars
    return -(10*np.sqrt(max(x, 0.01)) + 15*np.sqrt(max(y, 0.01)))

def constraint(vars):
    return vars[0] + vars[1] - 100

# Giải bằng scipy
result = minimize(objective, [50, 50], 
                  constraints={'type': 'eq', 'fun': constraint},
                  bounds=[(0.01, 99.99), (0.01, 99.99)])

x_opt, y_opt = result.x
f_opt = -result.fun

print("✈️ PHÂN BỔ NGÂN SÁCH MARKETING")
print("=" * 50)
print(f"   Tổng ngân sách: 100 triệu")
print(f"   Facebook: {x_opt:.1f} triệu → {10*np.sqrt(x_opt):.1f} triệu impression")
print(f"   Google:   {y_opt:.1f} triệu → {15*np.sqrt(y_opt):.1f} triệu impression")
print(f"   Tổng hiệu quả: {f_opt:.1f} triệu impression")
print(f"\n   Chia đều 50/50: {10*np.sqrt(50) + 15*np.sqrt(50):.1f} triệu impression")
print(f"   Lagrange tốt hơn chia đều {(f_opt - (10*np.sqrt(50) + 15*np.sqrt(50))) / (10*np.sqrt(50) + 15*np.sqrt(50)) * 100:.1f}%! ✅")

# =============================================
# TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Hình 1: Contour plot + ràng buộc
ax1 = axes[0]
x_range = np.linspace(0.1, 100, 200)
y_range = np.linspace(0.1, 100, 200)
X, Y = np.meshgrid(x_range, y_range)
Z = 10*np.sqrt(X) + 15*np.sqrt(Y)

contour = ax1.contour(X, Y, Z, levels=20, cmap='viridis', alpha=0.7)
ax1.clabel(contour, inline=True, fontsize=8)

# Đường ràng buộc
ax1.plot(x_range, 100 - x_range, 'r-', linewidth=3, label='x + y = 100 (ràng buộc)')

# Điểm tối ưu
ax1.plot(x_opt, y_opt, 'r*', markersize=20, zorder=10, label=f'Tối ưu ({x_opt:.0f}, {y_opt:.0f})')
ax1.plot(50, 50, 'bs', markersize=12, label='Chia đều (50, 50)')

ax1.set_title('🎯 Lagrange: Tìm điểm trên đường đỏ\ncó contour cao nhất', 
              fontsize=12, fontweight='bold')
ax1.set_xlabel('Ngân sách Facebook (triệu)', fontsize=11)
ax1.set_ylabel('Ngân sách Google (triệu)', fontsize=11)
ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)
ax1.set_xlim(0, 100); ax1.set_ylim(0, 100)

# Hình 2: So sánh các phương án
ax2 = axes[1]
phan_bo = np.linspace(1, 99, 100)
hieu_qua = 10*np.sqrt(phan_bo) + 15*np.sqrt(100 - phan_bo)

ax2.plot(phan_bo, hieu_qua, 'b-', linewidth=2.5)
ax2.axvline(x=x_opt, color='red', linestyle='--', linewidth=2, label=f'Lagrange: x={x_opt:.0f}')
ax2.axvline(x=50, color='green', linestyle='--', linewidth=2, label='Chia đều: x=50')
ax2.plot(x_opt, f_opt, 'r*', markersize=20, zorder=10)

ax2.set_title('📊 Hiệu quả theo phân bổ ngân sách\n(Lagrange tìm đỉnh!)', 
              fontsize=12, fontweight='bold')
ax2.set_xlabel('Ngân sách Facebook (triệu)', fontsize=11)
ax2.set_ylabel('Tổng impression (triệu)', fontsize=11)
ax2.legend(fontsize=10); ax2.grid(True, alpha=0.3)

plt.suptitle('💰 Lagrange Multiplier: Tối ưu phân bổ ngân sách marketing', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong6_lagrange.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Lagrange multiplier $\lambda$ có ý nghĩa kinh tế cực kỳ quan trọng: nó cho biết "nếu tăng ngân sách thêm 1 triệu, hiệu quả tăng bao nhiêu?" (shadow price). Trong ML, λ trong regularization chính là Lagrange multiplier!

---

> ✅ **CHECKPOINT 4 — Lagrange Multipliers**
>
> 1. Tại sao Lagrange lại được ứng dụng trong Support Vector Machine (SVM)?
> 2. Nhân tử Lagrange ($\lambda$) trong bài toán tối ưu thường biểu hiện cho khái niệm gì?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. SVM không chỉ tìm ranh giới tách biệt dữ liệu, mà nó tìm ranh giới "rộng nhất" (tối ưu nhất) **với điều kiện** mọi điểm dữ liệu đều nắm ngoài lề (ràng buộc không bị vi phạm). Từ khóa "tối ưu" và "ràng buộc" gọi ngay tên Lagrange.
> 2. Tiền phạt (Penalty) nếu vi phạm luật, hoặc "chi phí/lợi ích biên" (Shadow price) khi nới lỏng ngân sách ràng buộc.
>
> </details>

---

## PHẦN 5: VANISHING & EXPLODING GRADIENTS — "BỆNH CỦA MẠNG SÂU"

### 🟢 5.1 Vấn đề

Khi mạng neural có nhiều layer (deep), gradient phải nhân qua nhiều Jacobian:

$$\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial h_n} \cdot \frac{\partial h_n}{\partial h_{n-1}} \cdot \ldots \cdot \frac{\partial h_2}{\partial h_1} \cdot \frac{\partial h_1}{\partial w_1}$$

Nếu mỗi $\frac{\partial h_i}{\partial h_{i-1}} \approx 0.5$ → Sau 20 layer: $0.5^{20} \approx 10^{-6}$ → **Gradient biến mất!**

Nếu mỗi $\frac{\partial h_i}{\partial h_{i-1}} \approx 2.0$ → Sau 20 layer: $2^{20} \approx 10^{6}$ → **Gradient bùng nổ!**

#### 🎯 Liên tưởng: Truyền tin nhắn qua 20 người 📣

- Nếu mỗi người nhớ 50% → Cuối cùng chỉ còn 0.0001% nội dung → **Vanishing** (mất thông tin)
- Nếu mỗi người thêm thắt 2x → Cuối cùng phóng đại 1 triệu lần → **Exploding** (thông tin bị nhiễu)

### 🟡 5.2 Các giải pháp

| Giải pháp | Chống | Cách hoạt động |
|-----------|-------|----------------|
| **ReLU** (thay sigmoid) | Vanishing | $f'(x) = 1$ khi $x > 0$ (không co gradient) |
| **Residual Connection (ResNet)** | Vanishing | $h_l = f(h_{l-1}) + h_{l-1}$ (đường tắt!) |
| **Batch Normalization** | Cả hai | Chuẩn hóa output mỗi layer |
| **Gradient Clipping** | Exploding | Giới hạn $\|\nabla\| \leq c$ |
| **Weight Init (He/Xavier)** | Cả hai | Khởi tạo weight phù hợp |

### 🔵 5.3 Code Python: Mô phỏng Vanishing/Exploding

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# VANISHING & EXPLODING GRADIENT
# =============================================

def sigmoid(x):
    return 1 / (1 + np.exp(-np.clip(x, -500, 500)))

def sigmoid_deriv(x):
    s = sigmoid(x)
    return s * (1 - s)  # Max = 0.25 tại x=0!

def relu(x):
    return np.maximum(0, x)

def relu_deriv(x):
    return (x > 0).astype(float)  # = 1 khi x > 0

# Mô phỏng gradient qua n layers
n_layers_range = range(1, 51)

gradient_sigmoid = []
gradient_relu = []
gradient_resnet = []

for n in n_layers_range:
    # Sigmoid: mỗi layer nhân gradient với max 0.25
    grad_sig = 0.25 ** n  # Worst case
    gradient_sigmoid.append(grad_sig)
    
    # ReLU: mỗi layer nhân gradient với ~0.5 (50% neuron active)
    grad_relu = 0.5 ** n * (2 ** n) * 0.5  # Active neurons giữ gradient=1
    grad_relu = max(0.5 ** (n * 0.1), 1e-15)  # Simplified
    gradient_relu.append(1.0)  # Lý tưởng: ReLU giữ gradient = 1
    
    # ResNet: Residual connection giúp gradient đi "shortcut"
    grad_resnet = max(1.0 + 0.1 * np.random.randn(), 0.5)
    gradient_resnet.append(grad_resnet)

# Tính thực tế hơn
np.random.seed(42)
sigmoid_realistic = []
relu_realistic = []

for n in n_layers_range:
    # Sigmoid
    grad = 1.0
    for _ in range(n):
        x = np.random.normal(0, 1)
        grad *= sigmoid_deriv(x) * abs(np.random.normal(0, 0.5))
    sigmoid_realistic.append(abs(grad))
    
    # ReLU
    grad = 1.0
    for _ in range(n):
        x = np.random.normal(0, 1)
        grad *= relu_deriv(x) * abs(np.random.normal(1, 0.3))
    relu_realistic.append(abs(grad))

# Trực quan
fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Hình 1: Sigmoid derivative
ax1 = axes[0]
x_range = np.linspace(-6, 6, 200)
ax1.plot(x_range, sigmoid(x_range), 'b-', linewidth=2, label='σ(x)')
ax1.plot(x_range, sigmoid_deriv(x_range), 'r-', linewidth=2, label="σ'(x)")
ax1.axhline(y=0.25, color='red', linestyle='--', alpha=0.5, label='Max = 0.25')
ax1.set_title("🔻 Sigmoid: Đạo hàm MAX = 0.25\n→ Mỗi layer nhân ≤ 0.25 → VANISHING!", 
              fontsize=11, fontweight='bold')
ax1.set_xlabel('x'); ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)

# Hình 2: Gradient magnitude qua layers
ax2 = axes[1]
layers = list(n_layers_range)
ax2.semilogy(layers, sigmoid_realistic, 'r-', linewidth=2, label='Sigmoid (vanishing!)', alpha=0.7)
ax2.semilogy(layers, relu_realistic, 'g-', linewidth=2, label='ReLU (ổn định)', alpha=0.7)
ax2.axhline(y=1, color='black', linestyle='--', alpha=0.3, label='Gradient = 1 (lý tưởng)')
ax2.set_title("📉 Gradient magnitude qua N layers\n(Log scale — Sigmoid sụp đổ!)", 
              fontsize=11, fontweight='bold')
ax2.set_xlabel('Số layers'); ax2.set_ylabel('|Gradient| (log scale)')
ax2.legend(fontsize=9); ax2.grid(True, alpha=0.3)

plt.suptitle('⚠️ Vanishing Gradient: Tại sao Sigmoid thất bại ở Deep Learning', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong6_vanishing_gradient.png', dpi=150, bbox_inches='tight')
plt.show()

print("\n📊 KẾT QUẢ:")
print(f"   Sigmoid qua 20 layers: gradient ≈ {sigmoid_realistic[19]:.2e}")
print(f"   ReLU qua 20 layers:    gradient ≈ {relu_realistic[19]:.2e}")
print(f"\n💡 Đây là lý do:")
print(f"   - ResNet (2015) dùng skip connection → train được 152 layers!")
print(f"   - Transformer (2017) dùng Layer Normalization → train GPT 96 layers!")
```

---

> ✅ **CHECKPOINT 5 — Vanishing Gradient**
>
> 1. Tại sao Sigmoid lại bị ruồng bỏ ở những lớp mạng ẩn (hidden layers) trong Deep Learning?
> 2. Residual connection (Mạng phần dư ResNet) khắc phục Vanishing Gradient bằng cách nào?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Đạo hàm (gradient) tối đa của Sigmoid chỉ là 0.25. Qua 10 layer nhân với nhau, tín hiệu truyền ngược bị "teo tóp" ($0.25^{10} \approx 0.0000009$), mạng nơ-ron lớp đầu hoàn toàn không học được gì!
> 2. Mở một con "đường tắt" ($+x$). Kể cả khi gradient đi qua hàm activation chính bị rơi về 0, nó vẫn đi qua đường vòng ($+x$ đạo hàm là $1$) để truyền tín hiệu về trọn vẹn, không biến mất.
>
> </details>

---

## PHẦN 6: LEARNING RATE SCHEDULING — "TỐC ĐỘ THAY ĐỔI THEO THỜI GIAN"

### 🟢 6.1 Tại sao cần thay đổi learning rate?

- **Đầu training:** Cần lr LỚN → Di chuyển nhanh đến vùng tốt
- **Cuối training:** Cần lr NHỎ → "Hạ cánh" chính xác tại minimum

#### 🎯 Liên tưởng: Lái xe đến nhà hàng 🚗

- Trên cao tốc → Chạy nhanh (lr lớn)
- Vào thành phố → Chạy chậm (lr giảm)
- Tìm chỗ đỗ → Đi rất chậm (lr rất nhỏ)

### 🟡 6.2 Các scheduler phổ biến

| Scheduler | Công thức | Đặc điểm |
|-----------|-----------|----------|
| **Step Decay** | lr × 0.1 mỗi 30 epochs | Đơn giản, hiệu quả |
| **Cosine Annealing** | $lr_t = lr_{min} + \frac{1}{2}(lr_{max} - lr_{min})(1 + \cos\frac{t\pi}{T})$ | Smooth, phổ biến nhất |
| **Warmup + Decay** | Tăng dần → rồi giảm | Dùng trong Transformer |
| **OneCycleLR** | Tăng → peak → giảm | Nhanh hội tụ nhất |

### 🔵 6.3 Code Python: So sánh Scheduler

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# LEARNING RATE SCHEDULING
# =============================================

epochs = 100
lr_init = 0.01

# Step Decay
def step_decay(epoch, lr=lr_init, drop=0.5, every=30):
    return lr * (drop ** (epoch // every))

# Cosine Annealing
def cosine_annealing(epoch, lr_max=lr_init, lr_min=1e-5, T=epochs):
    return lr_min + 0.5 * (lr_max - lr_min) * (1 + np.cos(np.pi * epoch / T))

# Warmup + Linear Decay
def warmup_linear(epoch, lr=lr_init, warmup=10):
    if epoch < warmup:
        return lr * epoch / warmup
    return lr * (1 - (epoch - warmup) / (epochs - warmup))

# OneCycleLR (simplified)
def one_cycle(epoch, lr_max=lr_init, total=epochs):
    pct = epoch / total
    if pct < 0.3:
        return lr_max * pct / 0.3
    else:
        return lr_max * (1 - (pct - 0.3) / 0.7) * 0.9 + lr_max * 0.1

schedulers = {
    "Step Decay": step_decay,
    "Cosine Annealing": cosine_annealing,
    "Warmup + Linear": warmup_linear,
    "OneCycleLR": one_cycle,
}

fig, ax = plt.subplots(figsize=(14, 6))

colors = ['#f44336', '#4CAF50', '#2196F3', '#9C27B0']
for (name, fn), color in zip(schedulers.items(), colors):
    lrs = [fn(e) for e in range(epochs)]
    ax.plot(lrs, linewidth=2.5, label=name, color=color)

ax.set_title('📊 Learning Rate Scheduling — Tốc độ thay đổi theo thời gian\n'
             '(Cao tốc → Thành phố → Đỗ xe)', fontsize=13, fontweight='bold')
ax.set_xlabel('Epoch', fontsize=12)
ax.set_ylabel('Learning Rate', fontsize=12)
ax.legend(fontsize=11); ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('chuong6_lr_schedule.png', dpi=150, bbox_inches='tight')
plt.show()

print("💡 KHUYẾN NGHỊ:")
print("   Bài toán thường → Cosine Annealing (đơn giản, hiệu quả)")
print("   Transformer/LLM → Warmup + Linear Decay")
print("   Muốn nhanh nhất → OneCycleLR")
```

---

> ✅ **CHECKPOINT 6 — Learning Rate**
>
> 1. Điểm cốt yếu của việc dùng Learning Rate Scheduling trong Deep Learning là gì?
> 
> <details><summary>📝 Đáp án</summary>
>
> Không để Learning rate là một hằng số cứng ngắc. Thay vào đó, tăng giảm uyển chuyển (cao ban đầu để xuống dốc nhanh $\rightarrow$ thấp ở cuối để bò lết chính xác vào tâm không bị "văng" ra ngoài do đà tay lái).
>
> </details>

---

## PHẦN 6B: NUMERICAL STABILITY — "MÁY TÍNH CŨNG SAI!"

### 🟢 6B.1 Vấn đề: Máy tính không tính chính xác vô hạn

Máy tính dùng **floating point** — số thực được biểu diễn gần đúng:

| Kiểu | Bits | Phạm vi | Độ chính xác |
|------|------|---------|-------------|
| float16 | 16 | ±65,504 | ~3 chữ số |
| float32 | 32 | ±3.4×10³⁸ | ~7 chữ số |
| float64 | 64 | ±1.8×10³⁰⁸ | ~15 chữ số |

Trong deep learning dùng float16/32 để tiết kiệm bộ nhớ → **DỄ tràn số!**

### 🔵 6B.2 Các bẫy phổ biến & cách fix

#### Bẫy 1: Softmax Overflow

```python
import numpy as np

# ❌ SAI: exp(1000) = inf!
def softmax_bad(x):
    return np.exp(x) / np.sum(np.exp(x))

print(softmax_bad([1000, 1001, 1002]))  # [nan, nan, nan] 💥

# ✅ ĐÚNG: Trừ max trước!
def softmax_stable(x):
    x = np.array(x)
    x_shifted = x - np.max(x)  # Max = 0 → exp không overflow
    return np.exp(x_shifted) / np.sum(np.exp(x_shifted))

print(softmax_stable([1000, 1001, 1002]))  # [0.09, 0.24, 0.67] ✅
```

#### Bẫy 2: Log(0) = -Infinity

```python
# ❌ SAI: Cross-entropy với prediction = 0 hoặc 1
y_pred = np.array([0.0, 1.0])
np.log(y_pred)  # [-inf, 0] 💥

# ✅ ĐÚNG: Clip trước
eps = 1e-7
y_pred_safe = np.clip(y_pred, eps, 1 - eps)
np.log(y_pred_safe)  # [-16.1, -0.0000001] ✅
```

#### Bẫy 3: Log-Sum-Exp Trick

```python
# Tính log(Σ exp(xᵢ)) an toàn:
def logsumexp(x):
    c = np.max(x)  # Hằng số ổn định
    return c + np.log(np.sum(np.exp(x - c)))

# Dùng khi tính log-probability, KL divergence, evidence lower bound
```

> 💡 **Quy tắc vàng:**
> 1. KHÔNG BAO GIỜ truyền 0 hoặc 1 trực tiếp vào log
> 2. LUÔN trừ max trước khi exp
> 3. Dùng `torch.nn.functional.log_softmax()` thay vì tự tính
> 4. Mixed precision (float16 + float32) = tiết kiệm 2x memory nhưng cần `GradScaler`

---

> ✅ **CHECKPOINT 7 — Lặp & Số thực máy tính**
>
> 1. Điều gì sẽ xảy ra nếu ta tính `np.exp(800)` trong python khi kiểu dữ liệu là float64 (giới hạn 308 chữ số)?
> 2. "Log-Sum-Exp Trick" là bí quyết dùng làm gì?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Sẽ báo lỗi tràn số mạng lưới: Giá trị bằng vô cùng (infinity)!
> 2. Giải thoát hàm Softmax khỏi bệnh tràn bộ nhớ khi tính toán chỉ bằng mẹo nhỏ "nhổ" (trừ đi) giá trị cao nhất trong mảng rồi mới tính exp.
>
> </details>

---

## PHẦN 7: TÓM TẮT CHƯƠNG & BẢN ĐỒ TƯ DUY

### 🗺️ Bản đồ kiến thức Chương 6

```
TỐI ƯU HÓA NÂNG CAO — NGHỆ THUẬT TÌM ĐÍCH
├── JACOBIAN (Ma trận đạo hàm đa chiều)
│   ├── Backprop = nhân chuỗi Jacobian
│   └── Ứng dụng: Phân tích chính sách kinh tế
│
├── HESSIAN (Đạo hàm bậc 2 — Độ cong)
│   ├── Phân loại: Minimum / Maximum / Saddle point
│   ├── Newton's Method (GD bậc 2)
│   └── Ứng dụng: Tối ưu portfolio rủi ro
│
├── TAYLOR EXPANSION (Xấp xỉ đa thức)
│   ├── GD = Taylor bậc 1
│   ├── Newton = Taylor bậc 2
│   └── Activation approximation
│
├── LAGRANGE MULTIPLIER (Tối ưu có ràng buộc)
│   ├── L(x,λ) = f(x) - λg(x)
│   ├── SVM = constrained optimization
│   └── Ứng dụng: Phân bổ ngân sách marketing 💰
│
├── VANISHING/EXPLODING GRADIENT
│   ├── Sigmoid → max derivative = 0.25 → vanishing!
│   ├── Giải pháp: ReLU, ResNet, BatchNorm
│   └── Ý nghĩa: Tại sao deep learning cần kiến trúc đặc biệt
│
└── LR SCHEDULING (Thay đổi tốc độ học)
    ├── Step, Cosine, Warmup, OneCycleLR
    └── Ứng dụng: Training thực tế
```

### 💎 Nếu chỉ nhớ 1 điều từ chương này

> **Gradient Descent là la bàn. Jacobian + Hessian là bản đồ địa hình. Lagrange là luật giao thông.**
>
> Chương 2 dạy bạn "đi xuống đồi". Chương này dạy bạn "đi xuống đồi thông minh hơn": Jacobian cho biết backprop hoạt động ra sao. Hessian cảnh báo điểm yên ngựa. Taylor xấp xỉ hàm phức tạp. Lagrange xử lý ràng buộc. Và vanishing gradient giải thích tại sao không thể cứ "đắp thêm layer" mà mong model tốt hơn.

---

### 🔗 Kiến thức này xuất hiện lại ở đâu?

| Chương | Nội dung liên quan | Cần kiến thức gì từ Ch.6? |
|--------|-------------------|--------------------------|
| **Ch.7** Logistic Regression | Tối ưu BCE bằng GD nâng cao | Jacobian, Chain Rule |
| **Ch.7** Monte Carlo | SGD = Monte Carlo sampling | Gradient, Optimizer |
| **Thực tế** Training lớn | Adam, LR scheduling, Gradient clipping | Tất cả Ch.6 |
| **Paper** Bất kỳ paper ML nào | Ký hiệu ∇, H, J | Jacobian, Hessian, Taylor |

---

### ✅ Checklist kiến thức

- [ ] Hiểu Jacobian = ma trận đạo hàm cho hàm nhiều output
- [ ] Hessian phân loại điểm dừng: min / max / saddle
- [ ] Taylor expansion = xấp xỉ hàm phức tạp bằng đa thức
- [ ] Lagrange giải bài toán tối ưu có ràng buộc
- [ ] Giải thích tại sao mạng sâu gặp vanishing gradient
- [ ] Biết ít nhất 3 giải pháp: ReLU, ResNet, BatchNorm
- [ ] Chọn được LR scheduler phù hợp cho bài toán

---

## ❓ FAQ — Câu hỏi thường gặp

**Q: Jacobian và Gradient khác nhau thế nào?**
A: Gradient = Jacobian của hàm 1 output (f: Rⁿ → R). Jacobin tổng quát hơn — nó cho hàm nhiều output (f: Rⁿ → Rᵐ). Khi m=1, Jacobian = gradient.

**Q: Tại sao không dùng Newton's Method thay cho GD?**
A: Newton cần tính và nghịch đảo ma trận Hessian — kích thước n×n với n = số tham số. Model lớn có hàng triệu tham số → Hessian quá lớn để lưu và tính. Đó là lý do GD (và các biến thể như Adam) được ưa chuộng trong deep learning.

**Q: Vanishing gradient có còn là vấn đề không?**
A: Với kiến trúc hiện đại (Transformer, ResNet, LayerNorm) — ít hơn nhiều. Nhưng hiểu nó vẫn quan trọng để debug khi training không hội tụ, và để hiểu tại sao một số kiến trúc được thiết kế như vậy.

**Q: Khi nào dùng Gradient Clipping?**
A: Khi bạn thấy loss "nhảy" lên rất cao hoặc NaN. Thường gặp trong RNN/LSTM và khi training language model. Clip norm = 1.0 là giá trị khởi đầu tốt.

---

## 📝 BÀI TẬP TỰ LUYỆN

### Bài 1: Jacobian ⭐⭐
Cho hàm $f(x,y) = (x^2y, \sin(x) + e^y)$. Tính Jacobian tại (1, 0) bằng tay, rồi kiểm tra bằng numerical approximation.

### Bài 2: Phân loại điểm dừng ⭐⭐
Cho $f(x,y) = x^3 - 3xy^2$ (monkey saddle). Tìm Hessian tại (0,0). Phân loại điểm dừng. Trực quan hóa 3D.

### Bài 3: Lagrange phân bổ thời gian ⭐⭐⭐
Bạn có 24 giờ ôn thi 4 môn. Điểm mỗi môn: $S_i = 10\sqrt{t_i}$ (t = giờ). Dùng Lagrange tìm phân bổ tối ưu. So sánh với chia đều (6h mỗi môn).

### Bài 4: Vanishing Gradient thực nghiệm ⭐⭐⭐
Tạo neural network 10 layers (sigmoid) và 10 layers (ReLU). So sánh gradient magnitude tại layer 1 vs layer 10. Trực quan hóa.

---

## 📌 LƯU Ý CHO GIẢNG VIÊN

### Chiến lược khơi gợi sự tò mò

1. **Demo saddle point 3D** trước khi giải thích Hessian → "Tại sao GD bị kẹt ở đây?"
2. **Bài toán marketing** cho Lagrange → Sinh viên thấy toán = tiết kiệm tiền
3. **Cho sinh viên train mạng 50 layers sigmoid** → Tự phát hiện vanishing gradient
4. **So sánh có/không LR scheduling** trên cùng bài toán → Thấy sự khác biệt rõ rệt

### Sai lầm phổ biến

- Nghĩ Newton's Method luôn tốt hơn GD (đắt quá cho DL!)
- Quên Lagrange khi gặp bài toán có ràng buộc
- Dùng sigmoid cho deep network (hãy dùng ReLU!)

---

## 📝 BÀI TẬP PHÂN TẦNG

### 🟢 Mức A — Nhận biết

**A1.** Jacobian khác Gradient ở điểm gì? Gradient dùng khi nào, Jacobian dùng khi nào?

**A2.** Hessian Matrix có eigenvalues đều dương. Điểm dừng đó là cực tiểu, cực đại, hay yên ngựa?

**A3.** Tại sao không dùng Newton's Method cho Deep Learning dù nó hội tụ nhanh hơn GD?

**A4.** Vanishing gradient: Sigmoid hay ReLU bị ảnh hưởng nặng hơn? Tại sao?

**A5.** Learning rate scheduler "Warmup then Decay" hữu ích nhất trong trường hợp nào? (a) Training từ đầu; (b) Fine-tuning từ pretrained model; (c) Cả hai; (d) Không loại nào.

<details><summary>📝 Đáp án A</summary>

- A1: Gradient = vector đạo hàm cho **hàm 1 output** (scalar). Jacobian = **ma trận** đạo hàm cho **hàm nhiều output** (vector → vector).
- A2: **Cực tiểu (minimum)** — Tất cả eigenvalue H > 0 = bề mặt lồi lên ở mọi hướng.
- A3: Newton tính $H^{-1}$ mỗi bước → **O(n³)** với n tham số. Model 100M tham số → tính H⁻¹ bất khả thi.
- A4: **Sigmoid**. Đạo hàm max = 0.25 → Sau 10 layer: 0.25¹⁰ ≈ 10⁻⁶. ReLU đạo hàm = 1 khi x>0.
- A5: **(c)** — Warmup tránh "bước nhảy" lúc đầu, đặc biệt quan trọng khi fine-tuning pretrained model.

</details>

---

### 🟡 Mức B — Tính toán cơ bản

**B1.** Tính Jacobian của $f(x,y) = [3x + y^2,\ x^2 - 2y]$ tại điểm $(2, 3)$.

**B2.** Cho Hessian $H = \begin{pmatrix} 4 & 2 \\ 2 & 3 \end{pmatrix}$. Tính eigenvalue. Phân loại điểm dừng.

**B3.** Tối ưu Lagrange: Tìm $\max f(x,y) = xy$ với ràng buộc $x + y = 10$. (Gợi ý: $\mathcal{L} = xy - \lambda(x+y-10)$).

**B4.** Taylor Expansion: $\sin(x) \approx x - \frac{x^3}{6}$ (bậc 3). Tính sai số khi $x = 0.5$ radian.

<details><summary>📝 Đáp án B</summary>

**B1:**
$$J = \begin{pmatrix} \partial f_1/\partial x & \partial f_1/\partial y \\ \partial f_2/\partial x & \partial f_2/\partial y \end{pmatrix} = \begin{pmatrix} 3 & 2y \\ 2x & -2 \end{pmatrix}$$
Tại (2,3): $J = \begin{pmatrix} 3 & 6 \\ 4 & -2 \end{pmatrix}$

**B2:** $\det(H - \lambda I) = (4-\lambda)(3-\lambda) - 4 = 0$
$\lambda^2 - 7\lambda + 8 = 0 \Rightarrow \lambda = (7 \pm \sqrt{17})/2 \approx \mathbf{5.56}$ và $\mathbf{1.44}$. Cả hai đều dương → **Cực tiểu!**

**B3:** $\nabla_x \mathcal{L} = y - \lambda = 0 \Rightarrow y = \lambda$; $\nabla_y \mathcal{L} = x - \lambda = 0 \Rightarrow x = \lambda$. Vậy $x = y = 5$. $\max xy = \mathbf{25}$ (chia đều nhất = tối ưu!)

**B4:** $\sin(0.5) \approx 0.5 - 0.5^3/6 = 0.5 - 0.0208 = 0.4792$. Giá trị thực: $\sin(0.5) = 0.4794$. Sai số ≈ **0.0002** (rất nhỏ!)

</details>

---

### 🔵 Mức C — Giải thích và so sánh

**C1.** Saddle point trong không gian chiều cao (nhiều tham số) thực sự phổ biến như thế nào? Tại sao đây lại là vấn đề khác với chiều thấp? SGD (có noise) xử lý saddle point như thế nào?

**C2.** Residual Connection (ResNet) giúp vanishing gradient bằng cách tạo "shortcut". Vẽ sơ đồ và giải thích luồng gradient có và không có residual connection.

**C3.** SVM là bài toán Lagrange constrained optimization. Viết bài toán primal và dual, giải thích $\lambda$ (support vectors) có ý nghĩa toán học và hình học gì.

---

### 🔴 Mức D — Ứng dụng thực tế (Code)

**D1. Newton's Method vs GD:**
Cài đặt Newton's Method cho hàm 2D (tính H⁻¹ bằng `np.linalg.inv`). So sánh số bước hội tụ với GD trên $f(x,y) = x^4 + 3x^2y + y^4 - 2y^2$. Vẽ đường đi của cả 2.

**D2. ResNet Mini:**
Cài đặt 5-layer network với và không có residual connection. In norm gradient cho mỗi layer. Chứng minh residual giúp gradient không biến mất.

**D3. LR Finder:**
Implement LR Finder: Train 1 epoch với lr tăng từ 10⁻⁷ đến 10. Log loss theo lr. Tìm "sweet spot" — lr ngay trước khi loss bắt đầu tăng.

---

## 🆘 HỖ TRỢ NGƯỜI TỰ HỌC

### 📚 Tài nguyên học tiếp

| Nguồn | Nội dung | Khi nào dùng |
|-------|---------|-------------|
| **Deep Learning Book** (Goodfellow) Ch.4,8 | Optimization chuyên sâu | Muốn lý thuyết đầy đủ |
| **Sebastian Ruder — Optimizer Blog** | So sánh optimizer | Nhanh, thực tế |
| **Papers: ResNet, Batch Norm** | Nguồn gốc kỹ thuật | Khi muốn hiểu sâu |
| **PyTorch LR Schedulers Docs** | Tất cả scheduler | Khi code |

### 🗺️ Lộ trình ôn nếu bị hổng

```
Quên Jacobian?          → §1.1 "Bàn mixer nâng cấp" → Bài B1
Quên Hessian/saddle?    → §2.1-2.2 + Code 3D plot → Bài B2
Quên Lagrange?          → §4.2 + Ví dụ Marketing → Bài B3
Quên Vanishing Gradient? → §5.1-5.2 Bảng giải pháp → C2
```

### ✅ Checklist tự đánh giá

- [ ] Giải thích: Jacobian là "Gradient phiên bản nhiều output"
- [ ] Dùng eigenvalue Hessian để phân loại điểm dừng
- [ ] Nhận diện: "model không học được" → Kiểm tra vanishing gradient
- [ ] Cài layer ReLU thay Sigmoid và so sánh gradient norm
- [ ] Cài Lagrange cho bài tối ưu ngân sách đơn giản

> **5 ✅ → Sẵn sàng sang Chương 7 (Chương cuối!).**
> **3–4 ✅ → Xem lại 1–2 mục yếu.**
> **< 3 ✅ → Ôn lại Ch.2 trước.**

---

> 📖 **Chương tiếp theo:** [Chương 7: Thống kê Suy diễn & Mô hình Nâng cao](Chuong7_ThongKeSuyDien.md)
>
> *"Hypothesis testing cho bạn khả năng gọi tên: 'Kết quả này thật sự có ý nghĩa, hay chỉ do may mắn?'"*

