# 🐛 PHỤ LỤC: DEBUG THE MATH — TÌM LỖI SAI

> *"Giỏi code không chỉ là viết đúng. Mà là PHÁT HIỆN sai. 10 bài dưới đây rèn kỹ năng đó."*

---

## HƯỚNG DẪN

Mỗi bài có **1 hoặc nhiều lỗi**. Nhiệm vụ: Tìm lỗi → Giải thích tại sao sai → Viết lại cho đúng.

**Mức độ:** ⭐ Dễ | ⭐⭐ Trung bình | ⭐⭐⭐ Khó

---

## Bài 1: Nhân ma trận ⭐ (Chương 1)

```python
import numpy as np

A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# "Nhân ma trận"
C = A * B  # Kết quả: [[ 5, 12], [21, 32]]
print("A × B =", C)
```

**Câu hỏi:** Kết quả có đúng là nhân ma trận không? Nếu sai, sửa lại.

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** `A * B` là **element-wise multiplication** (nhân từng phần tử), KHÔNG phải matrix multiplication!

```python
# ❌ SAI: Element-wise
C_wrong = A * B  # [[ 5, 12], [21, 32]]

# ✅ ĐÚNG: Matrix multiplication
C_right = A @ B  # [[19, 22], [43, 50]]
# hoặc: C_right = np.dot(A, B)
```

**Kiểm tra:** `A @ B = [[1×5+2×7, 1×6+2×8], [3×5+4×7, 3×6+4×8]] = [[19,22],[43,50]]`

</details>

---

## Bài 2: Gradient Descent ⭐ (Chương 2)

```python
# Tìm minimum của f(w) = (w - 3)²
w = 10.0
lr = 0.01

for i in range(100):
    gradient = 2 * (w - 3)
    w = w + lr * gradient  # ← Cập nhật weight

print(f"w = {w:.4f}")  # Kỳ vọng: w ≈ 3.0
```

**Câu hỏi:** Chạy code, w hội tụ về 3 không? Nếu không, tại sao?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** Dấu **CỘNG** thay vì **TRỪ**! GD phải đi NGƯỢC gradient:

```python
# ❌ SAI: w ra xa minimum
w = w + lr * gradient  # Cộng = đi CÙNG hướng gradient = đi LÊN đồi!

# ✅ ĐÚNG: w tiến về minimum
w = w - lr * gradient  # Trừ = đi NGƯỢC gradient = đi XUỐNG đồi!
```

Đây là lỗi kinh điển — nhớ: GD luôn TRỪ gradient!

</details>

---

## Bài 3: Learning Rate quá lớn ⭐⭐ (Chương 2)

```python
w = 0.5
lr = 2.0  # Learning rate

for i in range(20):
    gradient = 2 * w  # f(w) = w², f'(w) = 2w
    w = w - lr * gradient
    if i < 10:
        print(f"Step {i}: w = {w:.4f}")
```

**Câu hỏi:** Code đúng cú pháp, nhưng w có hội tụ về 0 không? Tại sao?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** Learning rate `lr = 2.0` quá lớn!

Bước 1: `w = 0.5 - 2.0 × 2 × 0.5 = 0.5 - 2.0 = -1.5`
Bước 2: `w = -1.5 - 2.0 × 2 × (-1.5) = -1.5 + 6.0 = 4.5`
→ w nảy qua lại và **BÙ NỔ**!

```python
# ✅ Fix: Giảm learning rate
lr = 0.1  # Hoặc nhỏ hơn
# Quy tắc kinh nghiệm: lr < 1/(2×L) với L = hằng số Lipschitz
```

**Bài học:** lr quá lớn → diverge. lr quá nhỏ → hội tụ chậm. Phải chọn vừa phải!

</details>

---

## Bài 4: Softmax Overflow ⭐⭐ (Chương 3, 5)

```python
import numpy as np

def softmax(x):
    return np.exp(x) / np.sum(np.exp(x))

scores = [1000, 1001, 1002]
probs = softmax(np.array(scores))
print("Probabilities:", probs)
```

**Câu hỏi:** Code chạy nhưng output là gì? Tại sao? Sửa thế nào?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** `np.exp(1000) = inf` → overflow → `inf/inf = nan`!

```python
# ❌ SAI: exp(1000) = inf
def softmax_bad(x):
    return np.exp(x) / np.sum(np.exp(x))

# ✅ ĐÚNG: Trừ max trước (trick QUAN TRỌNG!)
def softmax_stable(x):
    x_shifted = x - np.max(x)  # Max = 0, còn lại âm → exp không overflow
    return np.exp(x_shifted) / np.sum(np.exp(x_shifted))

# Kết quả: [0.0900, 0.2447, 0.6652] ← Đúng!
```

**Bài học:** Numerical stability = KỸ NĂNG SỐNG trong deep learning. Mọi framework đều implement trick này.

</details>

---

## Bài 5: Cross-Entropy siêu bẫy ⭐⭐ (Chương 3)

```python
import numpy as np

y_true = np.array([1, 0, 1, 0])      # Nhãn thật
y_pred = np.array([0.99, 0.01, 1.0, 0.0])  # Dự đoán

# Cross-Entropy Loss
loss = -np.mean(y_true * np.log(y_pred) + (1 - y_true) * np.log(1 - y_pred))
print(f"Loss = {loss:.4f}")
```

**Câu hỏi:** Code đúng cú pháp nhưng sẽ ra kết quả gì? Vấn đề ở đâu?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** `log(0) = -inf` và `log(1-1) = log(0) = -inf`!

y_pred chứa giá trị **0.0** và **1.0** → log(0) = -∞ → Loss = inf hoặc nan.

```python
# ✅ ĐÚNG: Thêm epsilon nhỏ
eps = 1e-7
y_pred = np.clip(y_pred, eps, 1 - eps)  # Ép vào (ε, 1-ε)
loss = -np.mean(y_true * np.log(y_pred) + (1 - y_true) * np.log(1 - y_pred))
# Kết quả: Loss = 0.0050 ← Đúng!
```

**Bài học:** KHÔNG BAO GIỜ truyền 0.0 hoặc 1.0 vào log! Luôn clip/clamp. PyTorch `F.binary_cross_entropy` tự làm điều này.

</details>

---

## Bài 6: Eigenvalue nhầm ⭐⭐ (Chương 1)

```python
import numpy as np

A = np.array([[2, 1], [1, 2]])
eigenvalues, eigenvectors = np.linalg.eig(A)

# Kiểm tra: A @ v = λ * v ?
for i in range(len(eigenvalues)):
    lam = eigenvalues[i]
    v = eigenvectors[i]  # ← Lấy eigenvector thứ i
    check = A @ v - lam * v
    print(f"λ={lam:.1f}, check={np.round(check, 4)}")
```

**Câu hỏi:** Check có ra [0, 0] không?  Nếu không, lỗi ở đâu?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** `eigenvectors[i]` lấy **HÀNG** thứ i, nhưng `np.linalg.eig` trả eigenvector theo **CỘT**!

```python
# ❌ SAI: Lấy hàng
v = eigenvectors[i]         # HÀNG i → SAI!

# ✅ ĐÚNG: Lấy cột
v = eigenvectors[:, i]      # CỘT i → ĐÚNG!
```

**Bài học:** Đây là bẫy NumPy nổi tiếng. `np.linalg.eig` trả `eigenvectors` dạng ma trận, **mỗi CỘT** là 1 eigenvector. Documentation: "The column `eigenvectors[:,i]` is the eigenvector corresponding to the eigenvalue `eigenvalues[i]`."

</details>

---

## Bài 7: Broadcasting bẫy ⭐⭐ (Chương 1)

```python
import numpy as np

# Chuẩn hóa dữ liệu (mỗi cột trừ mean)
X = np.array([[1, 100], 
              [2, 200], 
              [3, 300]])

mean = np.mean(X, axis=0)  # mean = [2, 200]
X_norm = X - mean

print("Mean mới:", np.mean(X_norm, axis=1))  # Kỳ vọng: [0, 0]
```

**Câu hỏi:** Mean mới có bằng [0, 0] không? Tại sao?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** Kiểm tra mean theo **axis SAI**!

```python
# ❌ SAI: axis=1 tính mean theo HÀNG (mỗi sample)
print(np.mean(X_norm, axis=1))  # [-49.5, 0.0, 49.5] — KHÔNG phải [0,0]

# ✅ ĐÚNG: axis=0 tính mean theo CỘT (mỗi feature)
print(np.mean(X_norm, axis=0))  # [0., 0.] — ĐÚNG!
```

**Bài học:** `axis=0` = theo hàng (gộp rows) → kết quả per column. `axis=1` = theo cột (gộp cols) → kết quả per row. Nhớ: axis = chiều bị "collapse".

</details>

---

## Bài 8: Vanishing Gradient ⭐⭐⭐ (Chương 6)

```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-x))

# Mô phỏng gradient qua 10 layers sigmoid
gradient = 1.0
for layer in range(10):
    x = np.random.randn()
    gradient *= sigmoid(x) * (1 - sigmoid(x))  # σ'(x) = σ(x)(1-σ(x))

print(f"Gradient sau 10 layers: {gradient:.10f}")
# Kỳ vọng: gradient ≈ 1.0 (lý tưởng)
```

**Câu hỏi:** Gradient cuối cùng có ≈ 1.0 không? Tại sao? Đề xuất sửa.

<details>
<summary>💡 Đáp án</summary>

**Không phải lỗi code** — đây là **vấn đề bản chất** của sigmoid!

`sigmoid'(x)` max = 0.25 tại x=0. Nhân 10 lần: 0.25¹⁰ ≈ 10⁻⁶ → Gradient gần bằng 0!

```python
# ✅ Fix 1: Dùng ReLU thay sigmoid
def relu_deriv(x):
    return 1.0 if x > 0 else 0.0  # Đạo hàm = 1 khi active → KHÔNG vanish!

# ✅ Fix 2: Residual connection (ResNet)
# gradient = gradient + gradient_skip  # Luôn có "đường tắt" cho gradient

# ✅ Fix 3: Layer Normalization
# Chuẩn hóa output mỗi layer → giữ gradient ổn định
```

**Bài học:** Đây là lý do sigmoid KHÔNG DÙNG cho deep networks (>3 layers). ReLU + ResNet + BatchNorm = bộ 3 cứu tinh!

</details>

---

## Bài 9: A/B Test sai ⭐⭐⭐ (Chương 7)

```python
from scipy import stats

# Website A: 100 khách, 5 mua
# Website B: 100 khách, 8 mua
# Kết luận: "B tốt hơn A vì 8% > 5%!"

# "Chứng minh" bằng t-test:
A = [1]*5  + [0]*95   # 5 người mua
B = [1]*8  + [0]*92   # 8 người mua

t_stat, p_value = stats.ttest_ind(A, B)
print(f"p-value = {p_value:.4f}")
print("Kết luận: B TỐT HƠN A!" if p_value < 0.05 else "Chưa đủ bằng chứng")
```

**Câu hỏi:** Kết luận "B tốt hơn A vì 8% > 5%" có đúng không?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** Với n=100, sự khác biệt 5% vs 8% **KHÔNG có ý nghĩa thống kê**!

p-value ≈ 0.39 > 0.05 → Không bác bỏ H₀ → Chưa đủ bằng chứng!

```python
# Kết quả: p = 0.39 → Không đủ bằng chứng!
# Cần ít nhất n ≈ 1,000 mỗi nhóm để 5% vs 8% có ý nghĩa

# ✅ Tính sample size cần thiết:
from scipy.stats import norm
p1, p2 = 0.05, 0.08
alpha, power = 0.05, 0.80
z_alpha = norm.ppf(1 - alpha/2)  # 1.96
z_beta = norm.ppf(power)          # 0.84
p_avg = (p1 + p2) / 2
n_needed = ((z_alpha + z_beta)**2 * 2 * p_avg * (1-p_avg)) / (p2-p1)**2
print(f"Cần n = {int(n_needed)} mỗi nhóm!")  # ≈ 1,500-2,000
```

**Bài học:** KHÔNG kết luận chỉ vì "con số trông lớn hơn". Phải dùng hypothesis testing! Đây là lỗi #1 trong A/B testing thực tế.

</details>

---

## Bài 10: Overfitting ẩn ⭐⭐⭐ (Chương 7)

```python
import numpy as np
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

np.random.seed(42)
X = np.random.uniform(0, 10, 20).reshape(-1, 1)
y = np.sin(X).ravel() + np.random.normal(0, 0.3, 20)

# Fit đa thức bậc 15
poly = PolynomialFeatures(degree=15)
X_poly = poly.fit_transform(X)
model = LinearRegression()
model.fit(X_poly, y)

# Đánh giá
train_score = model.score(X_poly, y)
print(f"R² trên training: {train_score:.4f}")
print("Model TUYỆT VỜI!" if train_score > 0.95 else "Cần cải thiện")
```

**Câu hỏi:** R² = 0.99 trên training. Model có thật sự tuyệt vời không?

<details>
<summary>💡 Đáp án</summary>

**Lỗi:** Đánh giá TRÊN TRAINING DATA = tự chấm bài mình!

```python
# ❌ SAI: Chỉ đánh giá trên training
train_score = model.score(X_poly, y)  # 0.99 — "tuyệt vời"!

# ✅ ĐÚNG: Đánh giá trên test data
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

poly = PolynomialFeatures(degree=15)
X_train_poly = poly.fit_transform(X_train)
X_test_poly = poly.transform(X_test)

model = LinearRegression()
model.fit(X_train_poly, y_train)

print(f"Train R²: {model.score(X_train_poly, y_train):.4f}")  # ~0.99
print(f"Test R²:  {model.score(X_test_poly, y_test):.4f}")    # ~âm!!! OVERFITTING!
```

**Bài học:** 
- LUÔN đánh giá trên **test data riêng biệt**
- R² cao trên training + thấp trên test = OVERFITTING
- Bậc 15 cho 20 điểm = quá phức tạp → Dùng bậc 3-5 hoặc thêm regularization

</details>

---

## 📊 TỔNG KẾT: BẪY THEO CHƯƠNG

| Chương | Bẫy phổ biến nhất | Bài |
|--------|-------------------|-----|
| Ch.1 ĐSTT | `*` vs `@`, eigenvector theo CỘT, axis nhầm | 1, 6, 7 |
| Ch.2 GT | Sai dấu GD, lr quá lớn | 2, 3 |
| Ch.3 XSTK | log(0), numerical stability | 4, 5 |
| Ch.6 Tối ưu | Vanishing gradient | 8 |
| Ch.7 Suy diễn | Kết luận thiếu bằng chứng, overfitting | 9, 10 |

> *"Người giỏi không phải người không sai. Mà là người PHÁT HIỆN sai trước khi quá muộn."*
