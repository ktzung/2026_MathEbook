# 📋 PHỤ LỤC: FORMULA CHEAT SHEETS

> *"In ra dán lên bàn học. Khi code mà quên công thức — liếc 1 giây thay vì Google 5 phút."*

---

## 📐 CHƯƠNG 1: ĐẠI SỐ TUYẾN TÍNH

```
┌─────────────────────────────────────────────────────────────────┐
│  📐 CHEAT SHEET — Đại số Tuyến tính                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  VECTOR                                                         │
│  ─────                                                          │
│  Dot product:  a·b = Σ aᵢbᵢ = |a||b|cosθ                       │
│  L1 norm:      ‖v‖₁ = Σ|vᵢ|          (Manhattan)               │
│  L2 norm:      ‖v‖₂ = √(Σvᵢ²)       (Euclidean)               │
│  Cosine sim:   cos(θ) = (a·b)/(‖a‖‖b‖)                        │
│                                                                 │
│  MA TRẬN                                                        │
│  ───────                                                        │
│  Nhân:         (A·B)ᵢⱼ = Σₖ Aᵢₖ·Bₖⱼ     (m×n)·(n×p) = (m×p)  │
│  Chuyển vị:    (Aᵀ)ᵢⱼ = Aⱼᵢ                                   │
│  Nghịch đảo:   A·A⁻¹ = I     (chỉ khi det(A) ≠ 0)             │
│  Định thức 2×2: det = ad - bc                                   │
│                                                                 │
│  EIGENVALUE & EIGENVECTOR                                       │
│  ───────────────────────                                        │
│  Av = λv  →  det(A - λI) = 0                                   │
│  λ > 1: phóng to | 0<λ<1: thu nhỏ | λ<0: lật hướng            │
│                                                                 │
│  PHÂN RÃ                                                        │
│  ───────                                                        │
│  SVD:    A = UΣVᵀ    (mọi ma trận)                              │
│  PCA:    Cov → Eigen → Chiếu lên top-k eigenvector              │
│                                                                 │
│  TENSOR (mở rộng)                                               │
│  ──────                                                         │
│  Scalar (0D) → Vector (1D) → Ma trận (2D) → Tensor (nD)        │
│  Ảnh RGB: (H×W×3) | Batch ảnh: (B×H×W×3) | Video: (B×T×H×W×3) │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📈 CHƯƠNG 2: GIẢI TÍCH

```
┌─────────────────────────────────────────────────────────────────┐
│  📈 CHEAT SHEET — Giải tích                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ĐẠO HÀM CƠ BẢN                                               │
│  ──────────────                                                 │
│  d/dx(xⁿ) = nxⁿ⁻¹     d/dx(eˣ) = eˣ     d/dx(ln x) = 1/x    │
│  d/dx(sin x) = cos x   d/dx(cos x) = -sin x                   │
│                                                                 │
│  QUY TẮC                                                        │
│  ──────                                                         │
│  Tích:   (fg)' = f'g + fg'                                     │
│  Thương: (f/g)' = (f'g - fg')/g²                               │
│  Chuỗi:  d/dx f(g(x)) = f'(g(x)) · g'(x)   ← BACKPROP!       │
│                                                                 │
│  GRADIENT DESCENT                                               │
│  ────────────────                                               │
│  Gradient:     ∇f = (∂f/∂x₁, ∂f/∂x₂, ..., ∂f/∂xₙ)            │
│  Update rule:  w ← w − α·∇L      (ĐI NGƯỢC gradient!)         │
│                                                                 │
│  OPTIMIZERS                                                     │
│  ──────────                                                     │
│  SGD:         w ← w − α·∇L                                     │
│  Momentum:    v ← βv + ∇L,  w ← w − αv                        │
│  Adam:        m ← β₁m + (1-β₁)∇L  (mean)                      │
│               s ← β₂s + (1-β₂)(∇L)²  (variance)               │
│               w ← w − α·m̂/√(ŝ + ε)                            │
│                                                                 │
│  HÀM LỒI                                                       │
│  ───────                                                        │
│  f(λx+(1-λ)y) ≤ λf(x)+(1-λ)f(y)  → 1 global minimum           │
│  f''(x) ≥ 0 → lồi                                              │
│                                                                 │
│  TÍCH PHÂN                                                      │
│  ────────                                                       │
│  ∫f(x)dx = Diện tích dưới đường cong                           │
│  ∫₋∞^∞ PDF(x)dx = 1   (Tổng xác suất)                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎲 CHƯƠNG 3: XÁC SUẤT & THỐNG KÊ

```
┌─────────────────────────────────────────────────────────────────┐
│  🎲 CHEAT SHEET — Xác suất & Thống kê                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  XÁC SUẤT CƠ BẢN                                               │
│  ──────────────                                                 │
│  P(A∪B) = P(A) + P(B) - P(A∩B)                                 │
│  P(A∩B) = P(A)·P(B)          (nếu độc lập)                     │
│  P(A|B) = P(A∩B) / P(B)                                        │
│                                                                 │
│  BAYES                                                          │
│  ─────                                                          │
│  P(A|B) = P(B|A)·P(A) / P(B)                                   │
│  Posterior ∝ Likelihood × Prior                                  │
│                                                                 │
│  PHÂN PHỐI                                                      │
│  ────────                                                       │
│  Normal:    N(μ,σ²)  → 68-95-99.7 rule                         │
│  Bernoulli: P(X=1)=p, P(X=0)=1-p                               │
│  Poisson:   P(X=k) = λᵏe⁻λ/k!                                 │
│                                                                 │
│  THỐNG KÊ                                                       │
│  ────────                                                       │
│  E[X] = Σ xᵢP(xᵢ)           (Kỳ vọng)                        │
│  Var(X) = E[(X-μ)²]          (Phương sai)                      │
│  Cov(X,Y) = E[(X-μₓ)(Y-μᵧ)] (Hiệp phương sai)                │
│  ρ = Cov(X,Y)/(σₓσᵧ)        (Hệ số tương quan)               │
│                                                                 │
│  INFORMATION THEORY                                             │
│  ──────────────────                                             │
│  Entropy:     H(X) = -Σ p(x)log p(x)                           │
│  Cross-Ent:   H(p,q) = -Σ p(x)log q(x)    ← LOSS FUNCTION     │
│  KL Div:      D_KL(p‖q) = Σ p(x)log(p(x)/q(x))               │
│                                                                 │
│  LINEAR REGRESSION                                              │
│  ─────────────────                                              │
│  ŷ = wᵀx + b     |  Loss: MSE = (1/n)Σ(y-ŷ)²                 │
│  Normal eq: w* = (XᵀX)⁻¹Xᵀy                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔢 CHƯƠNG 4: TOÁN RỜI RẠC

```
┌─────────────────────────────────────────────────────────────────┐
│  🔢 CHEAT SHEET — Toán Rời rạc                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ĐỒ THỊ                                                        │
│  ──────                                                         │
│  G = (V, E)     V = đỉnh, E = cạnh                             │
│  Degree(v) = số cạnh nối với v                                  │
│  BFS: Tìm đường ngắn nhất (theo cạnh)  → O(V+E)               │
│  DFS: Duyệt sâu / Phát hiện cycle      → O(V+E)               │
│  Dijkstra: Đường ngắn nhất (có trọng số)→ O((V+E)logV)         │
│                                                                 │
│  TỔ HỢP                                                        │
│  ──────                                                         │
│  P(n,k) = n!/(n-k)!        (Hoán vị — có thứ tự)               │
│  C(n,k) = n!/[k!(n-k)!]    (Tổ hợp — không thứ tự)            │
│                                                                 │
│  LOGIC                                                          │
│  ─────                                                          │
│  AND (∧): T∧T=T, còn lại F                                     │
│  OR  (∨): F∨F=F, còn lại T                                     │
│  NOT (¬): ¬T=F, ¬F=T                                            │
│                                                                 │
│  BIG-O                                                          │
│  ─────                                                          │
│  O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!) │
│  Binary Search: O(log n)  |  Sort: O(n log n)                   │
│  Linear Scan: O(n)        |  Nested loop: O(n²)                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🧠 CHƯƠNG 5: TƯ DUY HỆ THỐNG

```
┌─────────────────────────────────────────────────────────────────┐
│  🧠 CHEAT SHEET — Tư duy Hệ thống                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  NEURAL NETWORK                                                 │
│  ──────────────                                                 │
│  Forward:  ŷ = σ(Wₙ·σ(Wₙ₋₁·...σ(W₁·x)))                      │
│  Backward: ∂L/∂Wₖ = ∂L/∂ŷ · ∂ŷ/∂hₙ · ... · ∂hₖ/∂Wₖ          │
│                                                                 │
│  SELF-ATTENTION (Transformer)                                   │
│  ────────────────────────────                                   │
│  Attention(Q,K,V) = softmax(QKᵀ/√dₖ)·V                         │
│  Q = XWq  |  K = XWk  |  V = XWv                               │
│  Multi-Head: concat(head₁,...,headₕ)·Wₒ                         │
│                                                                 │
│  LOSS FUNCTIONS                                                 │
│  ──────────────                                                 │
│  MSE:       (1/n)Σ(y-ŷ)²                (Regression)           │
│  Binary CE: -[y·log(ŷ)+(1-y)·log(1-ŷ)]  (Classification)      │
│  Cat. CE:   -Σ yᵢ·log(ŷᵢ)               (Multi-class)         │
│                                                                 │
│  REGULARIZATION                                                 │
│  ──────────────                                                 │
│  L1: L + λ‖w‖₁     (Sparse, feature selection)                 │
│  L2: L + λ‖w‖₂²    (Smooth, weight decay)                      │
│  Dropout: p=0.2-0.5 (Tắt neuron random)                        │
│                                                                 │
│  FEDERATED LEARNING                                             │
│  ──────────────────                                             │
│  FedAvg: w_global = Σ (nₖ/n)·wₖ                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## ⚡ CHƯƠNG 6: TỐI ƯU HÓA NÂNG CAO

```
┌─────────────────────────────────────────────────────────────────┐
│  ⚡ CHEAT SHEET — Tối ưu hóa Nâng cao                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  JACOBIAN (f: Rⁿ → Rᵐ)                                        │
│  ──────────────────────                                         │
│  J[i,j] = ∂fᵢ/∂xⱼ     Kích thước: m × n                      │
│  Backprop = Chain of Jacobians: J₃·J₂·J₁                       │
│                                                                 │
│  HESSIAN (f: Rⁿ → R)                                           │
│  ────────────────────                                           │
│  H[i,j] = ∂²f/∂xᵢ∂xⱼ  (Đối xứng)                            │
│  All λ > 0 → minimum ✅  |  Mixed → saddle 🐎                  │
│                                                                 │
│  NEWTON'S METHOD                                                │
│  ───────────────                                                │
│  x ← x − H⁻¹·∇f       (GD bậc 2, hội tụ nhanh)               │
│  Chi phí: O(n³)         → Quá đắt cho Deep Learning            │
│                                                                 │
│  TAYLOR EXPANSION                                               │
│  ────────────────                                               │
│  f(x+δ) ≈ f(x) + ∇f·δ + ½δᵀHδ + ...                           │
│  Bậc 1 = GD  |  Bậc 2 = Newton                                 │
│                                                                 │
│  LAGRANGE MULTIPLIER                                            │
│  ───────────────────                                            │
│  min f(x) s.t. g(x)=0  →  L(x,λ) = f(x) − λ·g(x)             │
│  Giải: ∇ₓL = 0, ∇λL = 0                                        │
│  λ = "Shadow price" (giá trị biên của ràng buộc)               │
│                                                                 │
│  VANISHING/EXPLODING GRADIENT                                   │
│  ────────────────────────────                                   │
│  Sigmoid: max σ'(x) = 0.25 → 0.25ⁿ → vanishing!               │
│  Fix: ReLU | ResNet (skip) | BatchNorm | Gradient Clipping      │
│                                                                 │
│  LR SCHEDULING                                                  │
│  ─────────────                                                  │
│  Step: lr×0.1 mỗi N epoch  |  Cosine: lr·(1+cos(πt/T))/2      │
│  Warmup: tăng dần → giảm   |  OneCycle: tăng → peak → giảm    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 CHƯƠNG 7: THỐNG KÊ SUY DIỄN

```
┌─────────────────────────────────────────────────────────────────┐
│  📊 CHEAT SHEET — Thống kê Suy diễn                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  HYPOTHESIS TESTING                                             │
│  ──────────────────                                             │
│  H₀: Không khác biệt  |  H₁: CÓ khác biệt                    │
│  p < α(0.05) → Bác bỏ H₀ ✅ | p ≥ α → Không bác bỏ ❌        │
│  ⚠️ p-value ≠ P(H₀ đúng)!                                      │
│                                                                 │
│  CONFIDENCE INTERVAL                                            │
│  ───────────────────                                            │
│  CI = x̄ ± z_(α/2) · s/√n                                      │
│  95% CI: z = 1.96  |  99% CI: z = 2.576                        │
│  n lớn → CI hẹp → Chính xác hơn                                │
│                                                                 │
│  LOGISTIC REGRESSION                                            │
│  ────────────────────                                           │
│  P(y=1|x) = σ(wᵀx + b) = 1/(1+e^(-(wᵀx+b)))                  │
│  Loss: BCE = -[y·log(ŷ) + (1-y)·log(1-ŷ)]                     │
│                                                                 │
│  MARKOV CHAIN                                                   │
│  ────────────                                                   │
│  P(Xₜ₊₁|Xₜ,...,X₀) = P(Xₜ₊₁|Xₜ)  (Markov property)          │
│  Phân phối dừng: π = πP  (eigenvector λ=1)                     │
│  P(n bước) = Pⁿ  (lũy thừa ma trận)                           │
│                                                                 │
│  MONTE CARLO                                                    │
│  ───────────                                                    │
│  Ước lượng = (1/N)Σ f(xᵢ),  xᵢ ~ random                       │
│  N lớn → Chính xác (Law of Large Numbers)                      │
│  Sai số ∝ 1/√N                                                  │
│                                                                 │
│  BIAS-VARIANCE TRADEOFF                                         │
│  ──────────────────────                                         │
│  Total Error = Bias² + Variance + Noise                         │
│  Bias cao → Underfitting  |  Variance cao → Overfitting         │
│  Sweet spot: Model vừa đủ phức tạp                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

> *"Công thức là BẢN ĐỒ. Code là XE. Hiểu biết là NGƯỜI LÁI. Cả 3 cần nhau."*
