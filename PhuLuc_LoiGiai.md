# 📝 PHỤ LỤC: LỜI GIẢI CHI TIẾT (SOLUTION MANUAL)

> *"Đáp án không phải để chép. Mà để kiểm tra xem bạn đã HIỂU đúng chưa."*

---

## CHƯƠNG 1: ĐẠI SỐ TUYẾN TÍNH

### Bài 1: Vector sở thích ⭐

**Đề:** Tạo 2 vector sở thích (Phim hành động, Phim tình cảm, Phim kinh dị). Tính cosine similarity.

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np

# Vector sở thích (thang 1-10)
an = np.array([9, 3, 7])    # An: thích hành động, kinh dị
binh = np.array([2, 9, 1])  # Bình: thích tình cảm

# Cosine similarity
cos_sim = np.dot(an, binh) / (np.linalg.norm(an) * np.linalg.norm(binh))
print(f"Cosine similarity = {cos_sim:.4f}")  # ≈ 0.30 → Khá khác nhau!

# So sánh với người giống An
cuong = np.array([8, 2, 9])
cos_ac = np.dot(an, cuong) / (np.linalg.norm(an) * np.linalg.norm(cuong))
print(f"An-Cường: {cos_ac:.4f}")  # ≈ 0.97 → Rất giống!
```

**Giải thích:** Cosine similarity đo góc giữa 2 vector. Gần 1 = rất giống, gần 0 = khác biệt. Đây là cách Netflix gợi ý phim: tìm user có vector sở thích cosine cao nhất với bạn.

</details>

---

### Bài 3: Nén ảnh SVD ⭐⭐⭐

**Đề:** Load ảnh, chuyển grayscale, nén bằng SVD với k=5,20,50. So sánh chất lượng.

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

# Tạo ảnh mẫu (gradient + pattern)
np.random.seed(42)
H, W = 200, 300
x = np.linspace(0, 1, W)
y = np.linspace(0, 1, H)
X, Y = np.meshgrid(x, y)
img = (np.sin(5*X) * np.cos(3*Y) * 128 + 128).astype(float)

# SVD
U, S, Vt = np.linalg.svd(img, full_matrices=False)

fig, axes = plt.subplots(1, 4, figsize=(20, 4))
ks = [5, 20, 50, min(H, W)]
titles = ['k=5 (2.5%)', 'k=20 (10%)', 'k=50 (25%)', 'Original']

for ax, k, title in zip(axes, ks, titles):
    img_k = U[:, :k] @ np.diag(S[:k]) @ Vt[:k, :]
    ax.imshow(img_k, cmap='gray')
    
    # Tỷ lệ nén
    original_size = H * W
    compressed_size = k * (H + W + 1)
    ratio = compressed_size / original_size * 100
    ax.set_title(f'{title}\nDữ liệu: {ratio:.1f}%', fontsize=10)
    ax.axis('off')

plt.suptitle('SVD Image Compression', fontsize=14, fontweight='bold')
plt.tight_layout()
plt.show()

# Tỷ lệ năng lượng giữ lại
total_energy = np.sum(S**2)
for k in [5, 20, 50]:
    energy_k = np.sum(S[:k]**2) / total_energy * 100
    print(f"k={k}: Giữ {energy_k:.1f}% thông tin, dùng {k*(H+W+1)/H/W*100:.1f}% bộ nhớ")
```

**Insight:** SVD phân tích ảnh thành tổng các "layer" quan trọng giảm dần. Singular value lớn = chi tiết chính. Singular value nhỏ = nhiễu/chi tiết phụ. Vì vậy bỏ k nhỏ vẫn giữ được ảnh tốt.

</details>

---

## CHƯƠNG 2: GIẢI TÍCH

### Bài 2: Gradient Descent cho f(x,y) = x² + 2y² ⭐⭐

**Đề:** Viết GD 2 biến, trực quan hóa đường đi trên contour plot.

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

def f(x, y): return x**2 + 2*y**2
def grad_f(x, y): return np.array([2*x, 4*y])

# Gradient Descent
x, y = 4.0, 3.0
lr = 0.1
path = [(x, y)]

for _ in range(30):
    g = grad_f(x, y)
    x -= lr * g[0]
    y -= lr * g[1]
    path.append((x, y))

path = np.array(path)

# Contour plot
fig, ax = plt.subplots(figsize=(10, 8))
xx = np.linspace(-5, 5, 100)
yy = np.linspace(-4, 4, 100)
XX, YY = np.meshgrid(xx, yy)
ZZ = f(XX, YY)

ax.contour(XX, YY, ZZ, levels=20, cmap='viridis', alpha=0.7)
ax.plot(path[:, 0], path[:, 1], 'ro-', markersize=4, linewidth=1.5, label='GD path')
ax.plot(path[0, 0], path[0, 1], 'r*', markersize=15, label='Start')
ax.plot(0, 0, 'g*', markersize=15, label='Minimum')
ax.set_title(f'GD: {len(path)} steps, final f = {f(path[-1,0], path[-1,1]):.6f}')
ax.legend(); ax.grid(True, alpha=0.3)
plt.show()

print(f"Xuất phát: ({path[0,0]:.1f}, {path[0,1]:.1f}), f = {f(*path[0]):.1f}")
print(f"Kết thúc:  ({path[-1,0]:.4f}, {path[-1,1]:.4f}), f = {f(*path[-1]):.6f}")
```

**Nhận xét:** GD đi theo đường zigzag vì hàm "dẹt" theo x nhưng "dốc" theo y (hệ số 2y² > x²). Đây là lý do Adam tốt hơn — nó điều chỉnh adaptive learning rate cho mỗi chiều.

</details>

---

## CHƯƠNG 3: XÁC SUẤT & THỐNG KÊ

### Bài 1: Lọc Spam Naive Bayes ⭐⭐

**Đề:** Xây phân loại spam với 3 từ khóa: "free", "winner", "meeting".

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np

# Dữ liệu training
# [free, winner, meeting] → spam(1) / not spam(0)
X = np.array([
    [1, 1, 0],  # "free winner" → spam
    [1, 0, 0],  # "free" → spam
    [0, 1, 0],  # "winner" → spam
    [0, 0, 1],  # "meeting" → not spam
    [0, 0, 1],  # "meeting" → not spam
    [1, 0, 1],  # "free meeting" → not spam
])
y = np.array([1, 1, 1, 0, 0, 0])

# Naive Bayes
P_spam = y.mean()  # P(spam) = 0.5
P_not = 1 - P_spam

# P(word | class) với Laplace smoothing
alpha = 1  # Smoothing
P_word_spam = (X[y==1].sum(axis=0) + alpha) / (y.sum() + 2*alpha)
P_word_not = (X[y==0].sum(axis=0) + alpha) / ((1-y).sum() + 2*alpha)

print("P(word | spam):", np.round(P_word_spam, 2))
print("P(word | not):", np.round(P_word_not, 2))

# Dự đoán: "free winner meeting"
email = np.array([1, 1, 1])
log_spam = np.log(P_spam) + np.sum(email * np.log(P_word_spam) + (1-email) * np.log(1-P_word_spam))
log_not = np.log(P_not) + np.sum(email * np.log(P_word_not) + (1-email) * np.log(1-P_word_not))

P_spam_given_email = np.exp(log_spam) / (np.exp(log_spam) + np.exp(log_not))
print(f"\nEmail: 'free winner meeting'")
print(f"P(spam) = {P_spam_given_email:.1%}")
print(f"Kết luận: {'🚫 SPAM!' if P_spam_given_email > 0.5 else '✅ Not spam'}")
```

**Key insight:** Naive Bayes giả sử các từ INDEPENDENT (naive). Thực tế thì không, nhưng vẫn hoạt động tốt đáng ngạc nhiên! Gmail dùng phiên bản nâng cao của ý tưởng này.

</details>

---

## CHƯƠNG 4: TOÁN RỜI RẠC

### Bài 2: Tìm đường đi ngắn nhất (Dijkstra) ⭐⭐⭐

**Đề:** Implement Dijkstra tìm đường ngắn nhất trên đồ thị có trọng số.

<details>
<summary>📖 Lời giải</summary>

```python
import heapq
import numpy as np

def dijkstra(graph, start, end):
    """Dijkstra: Tìm đường ngắn nhất
    graph: dict {node: [(neighbor, weight), ...]}
    """
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    previous = {node: None for node in graph}
    pq = [(0, start)]  # Priority queue: (distance, node)
    
    while pq:
        d, u = heapq.heappop(pq)
        if d > distances[u]:
            continue
        if u == end:
            break
        for v, w in graph[u]:
            new_dist = d + w
            if new_dist < distances[v]:
                distances[v] = new_dist
                previous[v] = u
                heapq.heappush(pq, (new_dist, v))
    
    # Truy vết đường đi
    path = []
    node = end
    while node is not None:
        path.append(node)
        node = previous[node]
    return distances[end], path[::-1]

# Đồ thị: Các quận ở Hà Nội (khoảng cách km giả lập)
graph = {
    'Hoàn Kiếm': [('Ba Đình', 3), ('Hai Bà Trưng', 2), ('Đống Đa', 4)],
    'Ba Đình':    [('Hoàn Kiếm', 3), ('Cầu Giấy', 5), ('Đống Đa', 3)],
    'Hai Bà Trưng': [('Hoàn Kiếm', 2), ('Thanh Xuân', 6)],
    'Đống Đa':   [('Hoàn Kiếm', 4), ('Ba Đình', 3), ('Thanh Xuân', 2), ('Cầu Giấy', 4)],
    'Cầu Giấy':  [('Ba Đình', 5), ('Đống Đa', 4), ('Thanh Xuân', 3)],
    'Thanh Xuân': [('Hai Bà Trưng', 6), ('Đống Đa', 2), ('Cầu Giấy', 3)],
}

dist, path = dijkstra(graph, 'Hoàn Kiếm', 'Cầu Giấy')
print(f"🗺️ Đường ngắn nhất: {' → '.join(path)}")
print(f"   Khoảng cách: {dist} km")
# Output: Hoàn Kiếm → Đống Đa → Cầu Giấy = 8km (tốt hơn đi thẳng Ba Đình → Cầu Giấy = 8km)
```

**Key insight:** Dijkstra chính là BFS nhưng dùng priority queue thay vì queue thường. Đây là cách Google Maps tìm đường: đồ thị = mạng lưới đường phố, trọng số = thời gian/khoảng cách.

</details>

---

## CHƯƠNG 6: TỐI ƯU HÓA NÂNG CAO

### Bài 1: Jacobian ⭐⭐

**Đề:** Cho f(x,y) = (x²y, sin(x) + eʸ). Tính Jacobian tại (1, 0).

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np

# f(x,y) = (x²y, sin(x) + eʸ)
# Jacobian:
# J = [[∂f₁/∂x, ∂f₁/∂y],   = [[2xy,  x² ],
#      [∂f₂/∂x, ∂f₂/∂y]]      [cos(x), eʸ]]

x0, y0 = 1.0, 0.0

J = np.array([
    [2*x0*y0,    x0**2],           # [2·1·0, 1²] = [0, 1]
    [np.cos(x0), np.exp(y0)]       # [cos(1), e⁰] = [0.5403, 1]
])

print("🧮 Jacobian tại (1, 0):")
print(f"   J = [[{J[0,0]:.4f}, {J[0,1]:.4f}],")
print(f"        [{J[1,0]:.4f}, {J[1,1]:.4f}]]")

# Kiểm tra bằng numerical
def f(x):
    return np.array([x[0]**2 * x[1], np.sin(x[0]) + np.exp(x[1])])

eps = 1e-7
J_num = np.zeros((2, 2))
for j in range(2):
    x_plus = np.array([x0, y0]); x_plus[j] += eps
    x_minus = np.array([x0, y0]); x_minus[j] -= eps
    J_num[:, j] = (f(x_plus) - f(x_minus)) / (2*eps)

print(f"\n   Kiểm tra numerical:")
print(f"   J_num = [[{J_num[0,0]:.4f}, {J_num[0,1]:.4f}],")
print(f"            [{J_num[1,0]:.4f}, {J_num[1,1]:.4f}]]")
print(f"   Sai số max: {np.max(np.abs(J - J_num)):.2e} ✅")
```

**Bước giải tay:**
1. $\frac{\partial f_1}{\partial x} = \frac{\partial (x^2y)}{\partial x} = 2xy$ → tại (1,0): **0**
2. $\frac{\partial f_1}{\partial y} = x^2$ → tại (1,0): **1**
3. $\frac{\partial f_2}{\partial x} = \cos(x)$ → tại (1,0): **0.5403**
4. $\frac{\partial f_2}{\partial y} = e^y$ → tại (1,0): **1**

</details>

---

### Bài 3: Lagrange phân bổ thời gian ⭐⭐⭐

**Đề:** 24 giờ ôn 4 môn. Điểm mỗi môn: Sᵢ = 10√tᵢ. Tối ưu phân bổ.

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np
from scipy.optimize import minimize

# max Σ 10√tᵢ s.t. Σtᵢ = 24
def objective(t):
    return -np.sum(10 * np.sqrt(np.maximum(t, 0.01)))

def constraint(t):
    return np.sum(t) - 24

result = minimize(objective, [6,6,6,6],
                  constraints={'type': 'eq', 'fun': constraint},
                  bounds=[(0.01, 23.99)]*4)

t_opt = result.x
total_opt = -result.fun

print("🎓 PHÂN BỔ THỜI GIAN ÔN THI")
print("=" * 40)
for i, t in enumerate(t_opt):
    print(f"   Môn {i+1}: {t:.1f} giờ → Điểm = {10*np.sqrt(t):.1f}")
print(f"   Tổng điểm: {total_opt:.1f}")

# So sánh chia đều
total_equal = 4 * 10 * np.sqrt(6)
print(f"\n   Chia đều (6h/môn): {total_equal:.1f}")
print(f"   Chênh lệch: {total_opt - total_equal:.2f}")
```

**Giải tay bằng Lagrange:**

$\mathcal{L} = \sum 10\sqrt{t_i} - \lambda(\sum t_i - 24)$

$\frac{\partial \mathcal{L}}{\partial t_i} = \frac{5}{\sqrt{t_i}} - \lambda = 0$ → $t_i = \frac{25}{\lambda^2}$

Tất cả $t_i$ bằng nhau! → $t_i = 24/4 = 6$ giờ mỗi môn.

**Insight:** Khi hàm điểm giống nhau cho mọi môn, Lagrange cho kết quả **chia đều**. Nếu hàm điểm khác nhau (ví dụ: Toán dễ lên điểm hơn Văn), thì phân bổ không đều mới tối ưu!

</details>

---

## CHƯƠNG 7: THỐNG KÊ SUY DIỄN

### Bài 1: A/B Testing Shopee ⭐⭐

**Đề:** Layout A: 5,000 khách, 180 mua. Layout B: 5,000 khách, 220 mua. B tốt hơn?

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np
from scipy import stats

# Dữ liệu
n_A, success_A = 5000, 180  # 3.6%
n_B, success_B = 5000, 220  # 4.4%

p_A = success_A / n_A
p_B = success_B / n_B

# Z-test cho 2 tỷ lệ
p_pool = (success_A + success_B) / (n_A + n_B)
se = np.sqrt(p_pool * (1 - p_pool) * (1/n_A + 1/n_B))
z = (p_B - p_A) / se
p_value = 1 - stats.norm.cdf(z)  # One-sided

print("🛒 A/B TESTING — Shopee Layout")
print("=" * 45)
print(f"   Layout A: {p_A:.1%} conversion ({success_A}/{n_A})")
print(f"   Layout B: {p_B:.1%} conversion ({success_B}/{n_B})")
print(f"   z = {z:.4f}")
print(f"   p-value = {p_value:.4f}")
print(f"\n   Kết luận: ", end="")

if p_value < 0.05:
    lift = (p_B - p_A) / p_A * 100
    print(f"✅ B TỐT HƠN A (p={p_value:.4f} < 0.05)")
    print(f"   Tỷ lệ tăng: {lift:.1f}%")
    print(f"   Ước tính: +{(p_B-p_A)*100000:.0f} đơn/100K khách")
else:
    print(f"❌ Chưa đủ bằng chứng (p={p_value:.4f} ≥ 0.05)")
```

**Key insight:** Với n=5000 mỗi nhóm, sự khác biệt 3.6% vs 4.4% thường đủ ý nghĩa (p ≈ 0.03 < 0.05). Nhưng nếu n=500, cùng tỷ lệ → p ≈ 0.25 → không đủ bằng chứng! **Sample size rất quan trọng.**

</details>

---

### Bài 3: Monte Carlo Integration ⭐⭐

**Đề:** Tính $\int_0^1 e^{-x^2} dx$ bằng Monte Carlo. So sánh với scipy.

<details>
<summary>📖 Lời giải</summary>

```python
import numpy as np
from scipy import integrate

# =============================================
# MONTE CARLO vs NUMERICAL INTEGRATION
# =============================================

def f(x):
    return np.exp(-x**2)

# 1. Monte Carlo
ns = [100, 1000, 10000, 100000, 1000000]
print("🎲 MONTE CARLO INTEGRATION: ∫₀¹ e^(-x²) dx")
print("=" * 55)

np.random.seed(42)
for n in ns:
    x = np.random.uniform(0, 1, n)
    mc_estimate = np.mean(f(x))  # E[f(X)] = ∫f(x)dx cho X~Uniform(0,1)
    print(f"   N={n:>8}: I ≈ {mc_estimate:.6f}")

# 2. Scipy (chính xác)
exact, error = integrate.quad(f, 0, 1)
print(f"\n   Scipy quad: I = {exact:.6f} ± {error:.2e}")
print(f"\n💡 Monte Carlo hội tụ ∝ 1/√N")
print(f"   N×100 → Sai số giảm ×10")
```

**Giải thích:** Monte Carlo biến tích phân thành bài toán lấy trung bình:

$$\int_0^1 f(x)dx = E[f(X)] \approx \frac{1}{N}\sum_{i=1}^N f(x_i), \quad x_i \sim \text{Uniform}(0,1)$$

Sai số giảm theo $O(1/\sqrt{N})$. Phương pháp này đặc biệt mạnh khi tích phân many chiều (curse of dimensionality).

</details>

---

## 📊 BẢNG TỔNG HỢP BÀI TẬP

| Chương | Bài | Mức | Kỹ năng được rèn |
|--------|-----|-----|-------------------|
| Ch.1 | Vector sở thích | ⭐ | Dot product, cosine similarity |
| Ch.1 | Nén ảnh SVD | ⭐⭐⭐ | SVD, energy preservation |
| Ch.2 | GD 2 biến | ⭐⭐ | Gradient, contour visualization |
| Ch.3 | Naive Bayes Spam | ⭐⭐ | Bayes, conditional probability |
| Ch.4 | Dijkstra | ⭐⭐⭐ | Graph, priority queue |
| Ch.6 | Jacobian | ⭐⭐ | Partial derivatives, numerical check |
| Ch.6 | Lagrange thời gian | ⭐⭐⭐ | Constrained optimization |
| Ch.7 | A/B Testing | ⭐⭐ | Z-test, p-value |
| Ch.7 | MC Integration | ⭐⭐ | Random sampling, convergence |

> *"Khi bạn giải được bài tập mà KHÔNG cần nhìn đáp án — đó là lúc bạn thực sự hiểu."*
