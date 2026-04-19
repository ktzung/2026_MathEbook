# CHƯƠNG 5: TƯ DUY HỆ THỐNG & CÔNG NGHỆ MỚI

## 📖 SYSTEMS THINKING & ADVANCED TOPICS

> *"Bạn đã học 4 viên gạch: Đại số Tuyến tính, Giải tích, Xác suất, Toán Rời rạc. Bây giờ hãy xem cách chúng ghép lại thành TÒA NHÀ — Trí tuệ Nhân tạo."*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 5: "Nhìn thấy bức tranh toàn cảnh"
>
> *N-42 đã học xong 4 kỹ năng: ĐSTT (ngôn ngữ), Giải tích (la bàn), XSTK (cảm giác), Toán rời rạc (cấu trúc). Giờ nó đứng trong một neural network khổng lồ, phân loại chó mèo, dịch ngôn ngữ, và hiểu được cơ chế tập trung nhận thức (Self-Attention). Mọi thứ HỘI TỤ tại đây — và N-42 bắt đầu thấy mình không chỉ là 1 neuron, mà là một phần của TRÍ TUỆ NHÂN TẠO.*

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: Cỗ máy phân biệt chó mèo

Bạn mở điện thoại, chụp tấm ảnh con mèo. Google Photos tự động gắn tag "cat". Chuyện này đơn giản đến mức bạn không thèm nghĩ. Nhưng phía sau, hàng triệu phép toán vừa diễn ra trong 0.1 giây:

1. **Đại số tuyến tính:** Ảnh = Ma trận pixel → Nhân qua hàng trăm ma trận (convolutional filters)
2. **Giải tích:** Gradient descent tối ưu triệu trọng số → Neural Network học cách phân biệt
3. **Xác suất:** Output = P(mèo) = 97.3%, P(chó) = 2.1% → Chọn nhãn "mèo"
4. **Toán rời rạc:** Kiến trúc mạng neural = Đồ thị có hướng (DAG)

→ **TẤT CẢ 4 chương bạn đã học đều hội tụ tại đây!**

> 💡 **Takeaway:** AI không phải phép thuật. Nó là sự kết hợp TINH TẾ của toán học cơ bản mà bạn đã nắm vững.

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Hệ thống AI | Môn Toán Ứng Dụng | Ý nghĩa |
|---|-------------|------------------|---------|
| 1 | **Computer Vision** | Convolution (Ma trận), Gradient Descent | Giúp AI "nhìn" ảnh |
| 2 | **Recommender Systems** | Vector Embedding, Dot Product | Giúp AI gợi ý phim/nhạc |
| 3 | **NLP (ChatGPT)** | Self-Attention, Xác suất Markov | Giúp AI "hiểu" và "nói" |

> 📋 **Prerequisites:** Tất cả Chương 1-4.
>
> ⏱️ **Thời gian đọc:** ~45 phút.

---

## 🗺️ BẠN ĐANG Ở ĐÂY

```
  Ch.1-4 (Cơ bản) ━━▶ 📍 Ch.5 HỆ THỐNG AI TỔNG HỢP ━━▶ Ch.6-7 (Nâng cao)
                        BẠN ĐANG Ở ĐÂY
```

**Phần này cần học trước:** Chương 1, 2, 3, 4. Bạn không thể lắp một cỗ máy nếu không biết từng con ốc vít.
**Bạn sẽ đạt được gì:** Thấy được sự vĩ đại của toán học khi kết hợp lại, hiểu CÁCH 1 mô hình AI thực thụ như ChatGPT vận hành ngoài đời thực.

---

## 📦 ÔN NHANH TOÁN PHỔ THÔNG

<details>
<summary>🔗 <b>Ôn nhanh 1: Trí tuệ của toán học</b> (Bấm để mở)</summary>

Ở cấp 3, ta học các công thức rời rạc, làm ta hay hỏi: "Học Tích phân, Ma trận để làm cái quần gì?".
Hôm nay bạn sẽ thấy: Không có Ma trận (Ch.1) không có ảnh pixel 2D; Không có Tích phân (Ch.2) không thể tối ưu sai số; Không có Xác suất (Ch.3) không có dự đoán thông minh; Không có Toán rời rạc (Ch.4) không có thuật toán lan truyền.

</details>

---

## 🏷️ HƯỚNG DẪN ĐỌC — Hệ thống 3 tầng

| Tầng | Label | Dành cho ai? | Cần gì? |
|------|-------|-------------|--------|
| 🟢 | **Hiểu bằng trực giác** | Tất cả mọi người | Trí tưởng tượng |
| 🟡 | **Lý thuyết Toán học** | Muốn hiểu nguyên lý | Toán đại cương cơ bản |
| 🔵 | **Hiểu bằng code Python** | Developer / Coder | scipy, numpy |

---

## PHẦN 1: BẢN ĐỒ LIÊN KẾT — TOÁN HỌC TRONG AI

### 🟢 1.1 Bức tranh toàn cảnh

```
                        🤖 TRÍ TUỆ NHÂN TẠO
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
     📸 Computer        📝 NLP             📊 Data
       Vision        (Xử lý ngôn ngữ)    Science
            │                 │                 │
     ┌──────┴──────┐   ┌─────┴─────┐    ┌──────┴──────┐
     │             │   │           │    │             │
  Chương 1      Chương 2  Chương 3   Chương 4    Chương 1-4
(Ma trận,    (Gradient   (Bayes,    (Graph      (Tất cả!)
  SVD)       Descent)  Distribution)  Neural)
```

### 🟡 1.2 Ma trận liên hệ chi tiết

| Thành phần AI | Chương 1 (ĐSTT) | Chương 2 (GT) | Chương 3 (XSTK) | Chương 4 (TRR) |
|--------------|-----------------|---------------|-----------------|----------------|
| **Neural Network** | Weight matrix | Backprop | Loss function | DAG architecture |
| **CNN (ảnh)** | Convolution = Nhân ma trận | Gradient descent | Softmax probability | Filter graph |
| **Recommendation** | Dot product | Optimization | Collaborative filtering | User-item graph |
| **NLP (ChatGPT)** | Embedding vectors | Attention gradient | Token probabilities | Sequence graph |
| **PageRank** | Eigenvector | Power iteration | Markov chain | Web graph |

### 🔵 1.3 Code Python: Tổng hợp tất cả

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# TỔNG HỢP: HỆ THỐNG PHÂN LOẠI ẢNH ĐƠN GIẢN
# "Mini CNN" kết hợp cả 4 chương
# =============================================

print("🤖 HỆ THỐNG PHÂN LOẠI ẢNH ĐƠN GIẢN")
print("=" * 60)
print("Kết hợp: ĐSTT + Giải tích + Xác suất + Toán rời rạc")
print("=" * 60)

# ========= CHƯƠNG 1: Đại số tuyến tính =========
print("\n📐 [Chương 1] Ảnh = Ma trận")

def tao_anh_chu_X(size=8):
    """Tạo ảnh chữ X đơn giản"""
    img = np.zeros((size, size))
    for i in range(size):
        img[i, i] = 1          # Đường chéo chính
        img[i, size-1-i] = 1   # Đường chéo phụ
    return img

def tao_anh_chu_O(size=8):
    """Tạo ảnh chữ O đơn giản"""
    img = np.zeros((size, size))
    for i in range(size):
        for j in range(size):
            dist = np.sqrt((i - size/2)**2 + (j - size/2)**2)
            if 2 < dist < 4:
                img[i, j] = 1
    return img

# Tạo dataset
np.random.seed(42)
X_imgs = [tao_anh_chu_X() + np.random.normal(0, 0.1, (8,8)) for _ in range(20)]
O_imgs = [tao_anh_chu_O() + np.random.normal(0, 0.1, (8,8)) for _ in range(20)]

# Flatten thành vector (mỗi ảnh = 1 vector 64 chiều)
X_data = np.array([img.flatten() for img in X_imgs + O_imgs])  # 40 × 64
y_data = np.array([1]*20 + [0]*20)  # 1 = X, 0 = O

print(f"   Dataset: {X_data.shape[0]} ảnh, mỗi ảnh = vector {X_data.shape[1]} chiều")

# ========= CHƯƠNG 2: Giải tích (Gradient Descent) =========
print("\n📐 [Chương 2] Huấn luyện bằng Gradient Descent")

def sigmoid(z):
    return 1 / (1 + np.exp(-np.clip(z, -500, 500)))

# Logistic Regression (Neural Network 0 tầng ẩn)
W = np.random.randn(64) * 0.01  # Weight vector
b = 0.0                          # Bias
lr = 0.1
losses = []

for epoch in range(200):
    # Forward pass (Chương 1: Tích vô hướng)
    z = X_data @ W + b           # 40 × 1
    y_pred = sigmoid(z)          # 40 × 1
    
    # Loss (Chương 3: Cross-entropy từ xác suất)
    loss = -np.mean(y_data * np.log(y_pred + 1e-8) + (1 - y_data) * np.log(1 - y_pred + 1e-8))
    losses.append(loss)
    
    # Backward pass - Gradient (Chương 2: Đạo hàm)
    dz = y_pred - y_data         # ∂L/∂z
    dW = X_data.T @ dz / len(y_data)  # ∂L/∂W
    db = np.mean(dz)             # ∂L/∂b
    
    # Cập nhật (Gradient Descent)
    W -= lr * dW
    b -= lr * db
    
    if epoch % 50 == 0:
        accuracy = np.mean((y_pred > 0.5) == y_data)
        print(f"   Epoch {epoch:>4}: Loss = {loss:.4f}, Accuracy = {accuracy:.1%}")

# ========= CHƯƠNG 3: Xác suất =========
print("\n📊 [Chương 3] Dự đoán = Xác suất")

# Tạo ảnh test mới
test_X = tao_anh_chu_X() + np.random.normal(0, 0.15, (8,8))
test_O = tao_anh_chu_O() + np.random.normal(0, 0.15, (8,8))

prob_X = sigmoid(test_X.flatten() @ W + b)
prob_O = sigmoid(test_O.flatten() @ W + b)

print(f"   Ảnh test 1: P(X) = {prob_X:.1%}, P(O) = {1-prob_X:.1%} → {'X' if prob_X > 0.5 else 'O'}")
print(f"   Ảnh test 2: P(X) = {prob_O:.1%}, P(O) = {1-prob_O:.1%} → {'X' if prob_O > 0.5 else 'O'}")

# ========= TRỰC QUAN HÓA TỔNG HỢP =========
fig, axes = plt.subplots(2, 3, figsize=(16, 10))

# 1. Ảnh mẫu
axes[0, 0].imshow(tao_anh_chu_X(), cmap='Blues', interpolation='nearest')
axes[0, 0].set_title("Ảnh chữ X\n(Ma trận 8×8)", fontsize=11, fontweight='bold')
axes[0, 0].axis('off')

axes[0, 1].imshow(tao_anh_chu_O(), cmap='Oranges', interpolation='nearest')
axes[0, 1].set_title("Ảnh chữ O\n(Ma trận 8×8)", fontsize=11, fontweight='bold')
axes[0, 1].axis('off')

# 2. Weight visualization
axes[0, 2].imshow(W.reshape(8, 8), cmap='RdYlBu_r', interpolation='nearest')
axes[0, 2].set_title("Trọng số W đã học\n(Mô hình 'nhìn thấy' gì?)", fontsize=11, fontweight='bold')
plt.colorbar(axes[0, 2].images[0], ax=axes[0, 2], shrink=0.8)
axes[0, 2].axis('off')

# 3. Loss curve
axes[1, 0].plot(losses, 'b-', linewidth=2)
axes[1, 0].set_title("📉 Loss giảm dần\n(Chương 2: Gradient Descent)", fontsize=11, fontweight='bold')
axes[1, 0].set_xlabel("Epoch"); axes[1, 0].set_ylabel("Cross-Entropy Loss")
axes[1, 0].grid(True, alpha=0.3)

# 4. Xác suất dự đoán
labels = ['Ảnh X', 'Ảnh O']
probs_X = [prob_X, prob_O]
probs_O = [1-prob_X, 1-prob_O]

x_pos = np.arange(len(labels))
axes[1, 1].bar(x_pos - 0.15, probs_X, 0.3, color='#2196F3', label='P(X)', edgecolor='white')
axes[1, 1].bar(x_pos + 0.15, probs_O, 0.3, color='#FF5722', label='P(O)', edgecolor='white')
axes[1, 1].set_xticks(x_pos); axes[1, 1].set_xticklabels(labels)
axes[1, 1].set_title("📊 Xác suất dự đoán\n(Chương 3: Xác suất)", fontsize=11, fontweight='bold')
axes[1, 1].legend(fontsize=10); axes[1, 1].grid(axis='y', alpha=0.3)
axes[1, 1].set_ylim(0, 1.1)

# 5. Kiến trúc mạng (DAG)
axes[1, 2].text(0.5, 0.9, "Input Layer\n(64 pixel)", ha='center', fontsize=10,
               bbox=dict(boxstyle='round,pad=0.5', facecolor='#E3F2FD', edgecolor='#2196F3'))
axes[1, 2].text(0.5, 0.5, "Linear Layer\nW·x + b", ha='center', fontsize=10,
               bbox=dict(boxstyle='round,pad=0.5', facecolor='#FFF3E0', edgecolor='#FF9800'))
axes[1, 2].text(0.5, 0.1, "Sigmoid\nP(X) ∈ [0,1]", ha='center', fontsize=10,
               bbox=dict(boxstyle='round,pad=0.5', facecolor='#E8F5E9', edgecolor='#4CAF50'))
axes[1, 2].annotate('', xy=(0.5, 0.6), xytext=(0.5, 0.82),
                    arrowprops=dict(arrowstyle='->', lw=2, color='gray'))
axes[1, 2].annotate('', xy=(0.5, 0.2), xytext=(0.5, 0.42),
                    arrowprops=dict(arrowstyle='->', lw=2, color='gray'))
axes[1, 2].set_title("🧠 Kiến trúc mạng\n(Chương 4: DAG)", fontsize=11, fontweight='bold')
axes[1, 2].axis('off')

plt.suptitle("🤖 TỔNG HỢP 4 CHƯƠNG → Hệ thống phân loại ảnh X/O", 
             fontsize=15, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong5_tong_hop.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 1 — Bản đồ liên kết AI**
>
> 1. Neural Network cần bao nhiêu trong số 4 môn toán cơ bản (Đại số tuyến tính, Giải tích, Xác suất, Toán Rời rạc) để hoạt động?
> 2. Phép tính nào trong Đại số tuyến tính là nền tảng cốt lõi của Computer Vision khi áp filter lên ảnh?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Cả 4**. Ma trận/Vector trọng số + Đạo hàm Tích phân để tối ưu Gradient Gradient + Xác suất Softmax để ra kết luận định lượng + Cấu trúc Đồ thị chuỗi để lập layer.
> 2. **Phép nhân ma trận (Tích chập - Convolution)**.
>
> </details>

---

## PHẦN 2: COMPUTER VISION — "MẮT" CỦA MÁY TÍNH

### 🟢 2.1 Pipeline xử lý ảnh

```
📸 Ảnh gốc        →  🔢 Ma trận pixel    →  🔍 Trích xuất        →  🧠 Phân loại
(Camera)              (Chương 1)            Feature (Ch.1+3)        (Ch.2+3)
                                                                        │
                   Convolution filter     Edge detection            Gradient
                   = Nhân ma trận        = Đạo hàm ảnh            Descent
                                                                        │
                                                                   P(mèo) = 97%
                                                                   (Chương 3)
```

### 🟡 2.2 Convolution = Nhân ma trận cục bộ

Convolution trong CNN chính là **phép nhân ma trận** nhưng áp dụng cục bộ:

```python
import numpy as np
import matplotlib.pyplot as plt

### 🔵 2.3 Code Python: Mô phỏng Convolution

# =============================================
# CONVOLUTION — "MẮT" CỦA CNN
# =============================================

def convolution_2d(anh, kernel):
    """
    Phép tích chập 2D (convolution) — Nền tảng của CNN
    
    Đây chính là phép NHÂN MA TRẬN cục bộ (Chương 1)
    áp dụng lên từng vùng nhỏ của ảnh.
    """
    h, w = anh.shape
    kh, kw = kernel.shape
    pad_h, pad_w = kh // 2, kw // 2
    
    # Padding
    anh_pad = np.pad(anh, ((pad_h, pad_h), (pad_w, pad_w)), mode='reflect')
    result = np.zeros_like(anh)
    
    for i in range(h):
        for j in range(w):
            # Nhân từng vùng nhỏ với kernel (Dot Product!) 
            vung = anh_pad[i:i+kh, j:j+kw]
            result[i, j] = np.sum(vung * kernel)  # ← Tích vô hướng!
    
    return result

# Tạo ảnh mẫu
def tao_anh_test(size=64):
    img = np.zeros((size, size))
    # Hình chữ nhật
    img[15:50, 20:45] = 200
    # Đường chéo
    for i in range(size):
        if 0 <= i < size:
            img[i, max(0, min(i, size-1))] = 250
    # Hình tròn
    for i in range(size):
        for j in range(size):
            if (i-40)**2 + (j-45)**2 < 100:
                img[i, j] = 180
    return img

anh = tao_anh_test()

# CÁC KERNEL QUAN TRỌNG
kernels = {
    "Phát hiện cạnh\n(Edge Detection)": np.array([[-1,-1,-1],[-1,8,-1],[-1,-1,-1]]),
    "Làm sắc nét\n(Sharpen)": np.array([[0,-1,0],[-1,5,-1],[0,-1,0]]),
    "Làm mờ\n(Gaussian Blur)": np.array([[1,2,1],[2,4,2],[1,2,1]]) / 16,
    "Cạnh dọc\n(Sobel X)": np.array([[-1,0,1],[-2,0,2],[-1,0,1]]),
    "Cạnh ngang\n(Sobel Y)": np.array([[-1,-2,-1],[0,0,0],[1,2,1]]),
}

# Áp dụng và trực quan hóa
fig, axes = plt.subplots(2, 3, figsize=(16, 10))
axes = axes.flatten()

axes[0].imshow(anh, cmap='gray')
axes[0].set_title("🖼️ Ảnh gốc", fontsize=12, fontweight='bold')
axes[0].axis('off')

for i, (name, kernel) in enumerate(kernels.items()):
    result = convolution_2d(anh, kernel)
    axes[i+1].imshow(result, cmap='gray')
    axes[i+1].set_title(name, fontsize=11, fontweight='bold')
    axes[i+1].axis('off')

plt.suptitle("🔍 Convolution = Phép nhân ma trận cục bộ — Nền tảng của CNN", 
             fontsize=14, fontweight='bold', y=1.02)
plt.tight_layout()
plt.savefig('chuong5_convolution.png', dpi=150, bbox_inches='tight')
plt.show()

# Giải thích
print("\n📖 GIẢI THÍCH:")
print("=" * 50)
print("Mỗi kernel (ma trận 3×3) là một 'bộ lọc':")
print("  • Edge Detection: Tìm biên ảnh (nơi pixel thay đổi đột ngột)")
print("  • Sharpen: Làm rõ chi tiết")
print("  • Blur: Làm mờ (giảm nhiễu)")
print("  • Sobel: Tìm cạnh theo một hướng cụ thể")
print("\n💡 CNN học TỰ ĐỘNG giá trị kernel tốt nhất bằng Gradient Descent!")
```

---

> ✅ **CHECKPOINT 2 — Computer Vision**
>
> 1. Ma trận Convolution Kernel thường có kích thước rất nhỏ (như 3x3) so với bức ảnh lớn (như 1000x1000). Tại sao lại vậy? 
>
> <details><summary>📝 Đáp án</summary>
>
> Kernel (bộ lọc) quét (trượt) qua từng mảng nhỏ của tấm ảnh lớn để trích xuất đặc trưng cục bộ (như nét ngang, nét dọc, góc cạnh). Ý tưởng cốt lõi của Convolution là "sự phụ thuộc cục bộ" — những pixel gần nhau chia sẻ thông tin với nhau, từ biên đến hình khối đến cả gương mặt.
>
> </details>

---

## PHẦN 3: FEDERATED LEARNING — HỌC MÀ KHÔNG XEM DỮ LIỆU

### 🟢 3.1 Vấn đề quyền riêng tư

Bạn có 1 triệu bức ảnh y khoa từ 100 bệnh viện. Muốn train AI chẩn đoán bệnh. Nhưng:

- Bệnh viện **không được** gửi ảnh bệnh nhân ra ngoài (luật HIPAA, GDPR)
- Mỗi bệnh viện chỉ có ít dữ liệu → Train riêng lẻ thì kém

→ **Federated Learning** = Train AI mà **dữ liệu KHÔNG bao giờ rời khỏi thiết bị**.

### 🟡 3.2 Cách hoạt động

```
🏥 Bệnh viện A        🏥 Bệnh viện B        🏥 Bệnh viện C
   Dữ liệu A            Dữ liệu B            Dữ liệu C
   (KHÔNG gửi đi)       (KHÔNG gửi đi)       (KHÔNG gửi đi)
       │                     │                     │
  Train local           Train local           Train local
  → Gradient A          → Gradient B          → Gradient C
       │                     │                     │
       └─────────────────────┼─────────────────────┘
                             │
                    ☁️ Server trung tâm
                    Tổng hợp Gradient
                    (FedAvg Algorithm)
                             │
                    📤 Gửi model cập nhật
                    về tất cả bệnh viện
                             │
       ┌─────────────────────┼─────────────────────┐
       │                     │                     │
🏥 Bệnh viện A        🏥 Bệnh viện B        🏥 Bệnh viện C
  Model tốt hơn!        Model tốt hơn!        Model tốt hơn!
```

### 3.3 Toán học đằng sau

**FedAvg (Federated Averaging):**

$$W_{global}^{t+1} = \sum_{k=1}^{K} \frac{n_k}{n} \cdot W_k^{t+1}$$

Trong đó:
- $K$ = số client (bệnh viện)
- $n_k$ = số mẫu tại client $k$
- $n = \sum n_k$ = tổng số mẫu
- $W_k^{t+1}$ = trọng số sau khi client $k$ train local

→ Chỉ là **trung bình có trọng số** (Chương 1: phép nhân ma trận + Chương 3: kỳ vọng)!

### 🔵 3.4 Code Python: Mô phỏng Federated Learning

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# MÔ PHỎNG FEDERATED LEARNING
# =============================================

def sigmoid(z):
    return 1 / (1 + np.exp(-np.clip(z, -500, 500)))

class FederatedLearning:
    """
    Mô phỏng Federated Learning với FedAvg.
    
    Toán học sử dụng:
    - Chương 1: Nhân ma trận (forward pass)
    - Chương 2: Gradient Descent (local training)
    - Chương 3: Trung bình có trọng số (aggregation)
    - Chương 4: Topology mạng (communication graph)
    """
    
    def __init__(self, n_clients, n_features):
        self.n_clients = n_clients
        self.n_features = n_features
        # Model toàn cục
        self.W_global = np.random.randn(n_features) * 0.01
        self.b_global = 0.0
    
    def tao_du_lieu_client(self, client_id, n_samples=50):
        """Mỗi client có dữ liệu RISêng (Non-IID)"""
        np.random.seed(client_id * 100)
        
        # Dữ liệu Non-IID: mỗi client có phân phối khác nhau
        shift = client_id * 0.5
        X_pos = np.random.randn(n_samples // 2, self.n_features) + shift + 1
        X_neg = np.random.randn(n_samples // 2, self.n_features) + shift - 1
        
        X = np.vstack([X_pos, X_neg])
        y = np.array([1] * (n_samples // 2) + [0] * (n_samples // 2))
        
        # Shuffle
        idx = np.random.permutation(n_samples)
        return X[idx], y[idx]
    
    def train_local(self, X, y, W, b, lr=0.1, epochs=5):
        """Train cục bộ tại 1 client (Chương 2: GD)"""
        W_local = W.copy()
        b_local = b
        
        for _ in range(epochs):
            z = X @ W_local + b_local
            pred = sigmoid(z)
            
            dz = pred - y
            dW = X.T @ dz / len(y)
            db = np.mean(dz)
            
            W_local -= lr * dW
            b_local -= lr * db
        
        return W_local, b_local
    
    def fedavg(self, client_weights, client_biases, client_sizes):
        """
        FedAvg: Tổng hợp model (Chương 3: Trung bình có trọng số)
        
        W_global = Σ (n_k / n) * W_k
        """
        total_samples = sum(client_sizes)
        W_avg = np.zeros(self.n_features)
        b_avg = 0.0
        
        for W_k, b_k, n_k in zip(client_weights, client_biases, client_sizes):
            weight = n_k / total_samples
            W_avg += weight * W_k
            b_avg += weight * b_k
        
        return W_avg, b_avg
    
    def train_federated(self, n_rounds=20, local_epochs=5, lr=0.1):
        """Huấn luyện Federated Learning"""
        history = {"global_loss": [], "client_losses": []}
        
        print(f"\n🌐 FEDERATED LEARNING — {self.n_clients} Clients")
        print("=" * 60)
        
        for round_t in range(n_rounds):
            client_weights = []
            client_biases = []
            client_sizes = []
            round_losses = []
            
            # Mỗi client train local
            for k in range(self.n_clients):
                X_k, y_k = self.tao_du_lieu_client(k)
                
                # Train local với model toàn cục hiện tại
                W_k, b_k = self.train_local(X_k, y_k, self.W_global, self.b_global, lr, local_epochs)
                
                # Tính loss local
                pred = sigmoid(X_k @ W_k + b_k)
                loss = -np.mean(y_k * np.log(pred + 1e-8) + (1-y_k) * np.log(1-pred + 1e-8))
                round_losses.append(loss)
                
                client_weights.append(W_k)
                client_biases.append(b_k)
                client_sizes.append(len(y_k))
            
            # Server tổng hợp (FedAvg)
            self.W_global, self.b_global = self.fedavg(client_weights, client_biases, client_sizes)
            
            # Tính global loss trung bình
            global_loss = np.mean(round_losses)
            history["global_loss"].append(global_loss)
            history["client_losses"].append(round_losses)
            
            if round_t % 5 == 0:
                print(f"   Round {round_t:>3}: Global Loss = {global_loss:.4f}, "
                      f"Client Losses = [{', '.join(f'{l:.3f}' for l in round_losses)}]")
        
        return history

# =============================================
# CHẠY VÀ SO SÁNH
# =============================================

# 1. Federated Learning
fl = FederatedLearning(n_clients=4, n_features=5)
history_fl = fl.train_federated(n_rounds=30, local_epochs=5)

# 2. Centralized Learning (để so sánh)
print(f"\n📊 CENTRALIZED LEARNING (để so sánh)")
print("=" * 60)

# Gom tất cả dữ liệu (giả sử được phép)
all_X = []
all_y = []
for k in range(4):
    X_k, y_k = fl.tao_du_lieu_client(k)
    all_X.append(X_k)
    all_y.append(y_k)
all_X = np.vstack(all_X)
all_y = np.concatenate(all_y)

W_central = np.random.randn(5) * 0.01
b_central = 0.0
losses_central = []

for epoch in range(30):
    z = all_X @ W_central + b_central
    pred = sigmoid(z)
    loss = -np.mean(all_y * np.log(pred + 1e-8) + (1-all_y) * np.log(1-pred + 1e-8))
    losses_central.append(loss)
    
    dz = pred - all_y
    dW = all_X.T @ dz / len(all_y)
    db = np.mean(dz)
    
    W_central -= 0.1 * dW
    b_central -= 0.1 * db
    
    if epoch % 10 == 0:
        print(f"   Epoch {epoch:>3}: Loss = {loss:.4f}")

# =============================================
# TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1. So sánh hội tụ
axes[0].plot(history_fl["global_loss"], 'b-', linewidth=2, label='Federated (FedAvg)')
axes[0].plot(losses_central, 'r--', linewidth=2, label='Centralized')
axes[0].set_title("📉 So sánh: Federated vs Centralized", fontsize=13, fontweight='bold')
axes[0].set_xlabel("Round/Epoch"); axes[0].set_ylabel("Loss")
axes[0].legend(fontsize=11); axes[0].grid(True, alpha=0.3)

# 2. Loss từng client
client_losses_over_time = np.array(history_fl["client_losses"])
for k in range(4):
    axes[1].plot(client_losses_over_time[:, k], linewidth=1.5, 
                label=f'🏥 Client {k+1}', alpha=0.7)
axes[1].set_title("🏥 Loss từng Client qua các Round", fontsize=13, fontweight='bold')
axes[1].set_xlabel("Round"); axes[1].set_ylabel("Loss")
axes[1].legend(fontsize=9); axes[1].grid(True, alpha=0.3)

# 3. Sơ đồ FL
ax3 = axes[2]
# Server
ax3.add_patch(plt.Circle((0.5, 0.85), 0.08, color='#2196F3', zorder=5))
ax3.text(0.5, 0.85, '☁️', fontsize=20, ha='center', va='center', zorder=6)
ax3.text(0.5, 0.72, 'Server\n(FedAvg)', ha='center', fontsize=9, fontweight='bold')

# Clients
client_pos = [(0.15, 0.25), (0.38, 0.25), (0.62, 0.25), (0.85, 0.25)]
for i, (cx, cy) in enumerate(client_pos):
    ax3.add_patch(plt.Circle((cx, cy), 0.06, color='#4CAF50', zorder=5))
    ax3.text(cx, cy, '🏥', fontsize=14, ha='center', va='center', zorder=6)
    ax3.text(cx, cy - 0.12, f'Client {i+1}\n(Data riêng)', ha='center', fontsize=8)
    
    # Arrows
    ax3.annotate('', xy=(0.5, 0.77), xytext=(cx, cy + 0.06),
                arrowprops=dict(arrowstyle='->', color='#FF5722', lw=1.5))
    ax3.annotate('', xy=(cx, cy + 0.06), xytext=(0.5, 0.77),
                arrowprops=dict(arrowstyle='->', color='#2196F3', lw=1.5, linestyle='--'))

ax3.text(0.5, 0.52, '↑ Gửi Gradient\n↓ Nhận Model', ha='center', fontsize=9, 
         color='gray', style='italic')
ax3.set_title("🌐 Kiến trúc Federated Learning", fontsize=13, fontweight='bold')
ax3.set_xlim(0, 1); ax3.set_ylim(0, 1)
ax3.axis('off')

plt.suptitle("🔒 Federated Learning — Train AI mà KHÔNG chia sẻ dữ liệu", 
             fontsize=15, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong5_federated_learning.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## PHẦN 3B: LOSS FUNCTIONS & REGULARIZATION — "VŨ KHÍ CHỐNG OVERFITTING"

### 🟢 3B.1 Loss Functions — "Thước đo sai số"

Loss function cho model biết nó **sai bao nhiêu**. Mỗi bài toán cần loss phù hợp:

| Loss Function | Công thức | Dùng khi | Liên tưởng |
|---------------|-----------|----------|------------|
| **MSE** | $\frac{1}{n}\sum(y-\hat{y})^2$ | Regression (giá nhà) | 📏 Đo khoảng cách bình phương |
| **MAE** | $\frac{1}{n}\sum|y-\hat{y}|$ | Regression (ít nhạy outlier) | 📏 Đo khoảng cách tuyệt đối |
| **Binary CE** | $-[y\log\hat{y} + (1-y)\log(1-\hat{y})]$ | Phân loại 2 nhóm (spam/not) | 🎯 Phạt nặng khi TỰ TIN SAI |
| **Categorical CE** | $-\sum y_i \log \hat{y}_i$ | Phân loại nhiều nhóm | 🎯 Entropy (Ch.3) |
| **Hinge** | $\max(0, 1 - y\hat{y})$ | SVM | ⚔️ Phạt khi gần ranh giới |

> 💡 **Quy tắc nhớ:** Regression → MSE/MAE. Classification → Cross-Entropy. Đây là 2 loss function bạn dùng 90% thời gian!

### 🟡 3B.2 Regularization — "Thuốc chống học vẹt"

**Overfitting** = Model "học thuộc" dữ liệu training nhưng dự đoán tệ trên dữ liệu mới.

#### 🎯 Liên tưởng: Sinh viên học vẹt vs hiểu

- **Underfitting** = Đọc lướt, không hiểu → Thi điểm kém cả training lẫn test
- **Good fit** = Hiểu bản chất → Làm tốt cả bài quen VÀ bài lạ
- **Overfitting** = Học thuộc đề cương → Làm tốt bài cũ nhưng gặp bài mới là bó tay

**3 vũ khí chống overfitting:**

| Kỹ thuật | Ý tưởng | Công thức |
|----------|---------|-----------|
| **L1 (Lasso)** | Ép weight = 0 (feature selection) | $\mathcal{L} + \lambda\|\vec{w}\|_1$ |
| **L2 (Ridge)** | Ép weight nhỏ đều | $\mathcal{L} + \lambda\|\vec{w}\|_2^2$ |
| **Dropout** | Tắt ngẫu nhiên neuron khi train | p = 0.2-0.5 |

### 🔵 3B.3 Code Python: Overfitting & Regularization

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# OVERFITTING vs REGULARIZATION
# =============================================

np.random.seed(42)

# Dữ liệu thực: y = sin(x) + nhiễu
n = 15
X = np.sort(np.random.uniform(0, 2*np.pi, n))
y = np.sin(X) + np.random.normal(0, 0.3, n)

X_test = np.linspace(0, 2*np.pi, 200)
y_true = np.sin(X_test)

fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1. Underfitting (degree = 1)
ax1 = axes[0]
coef1 = np.polyfit(X, y, 1)
y_pred1 = np.polyval(coef1, X_test)
ax1.scatter(X, y, color='#2196F3', s=80, zorder=5, label='Dữ liệu')
ax1.plot(X_test, y_true, 'g--', linewidth=1.5, alpha=0.5, label='Hàm thực')
ax1.plot(X_test, y_pred1, 'r-', linewidth=2.5, label=f'Đa thức bậc 1')
ax1.set_title('😢 UNDERFITTING\n(Quá đơn giản → Không nắm được pattern)', 
              fontsize=11, fontweight='bold', color='red')
ax1.legend(fontsize=8); ax1.grid(True, alpha=0.3)

# 2. Good fit (degree = 4)
ax2 = axes[1]
coef4 = np.polyfit(X, y, 4)
y_pred4 = np.polyval(coef4, X_test)
ax2.scatter(X, y, color='#2196F3', s=80, zorder=5, label='Dữ liệu')
ax2.plot(X_test, y_true, 'g--', linewidth=1.5, alpha=0.5, label='Hàm thực')
ax2.plot(X_test, y_pred4, '#4CAF50', linewidth=2.5, label=f'Đa thức bậc 4')
ax2.set_title('✅ GOOD FIT\n(Vừa đủ phức tạp → Nắm được pattern)', 
              fontsize=11, fontweight='bold', color='green')
ax2.legend(fontsize=8); ax2.grid(True, alpha=0.3)

# 3. Overfitting (degree = 14)
ax3 = axes[2]
coef14 = np.polyfit(X, y, min(14, n-1))
y_pred14 = np.polyval(coef14, X_test)
ax3.scatter(X, y, color='#2196F3', s=80, zorder=5, label='Dữ liệu')
ax3.plot(X_test, y_true, 'g--', linewidth=1.5, alpha=0.5, label='Hàm thực')
ax3.plot(X_test, np.clip(y_pred14, -3, 3), '#FF5722', linewidth=2.5, label=f'Đa thức bậc {min(14, n-1)}')
ax3.set_title('😱 OVERFITTING\n(Quá phức tạp → Học thuộc noise)', 
              fontsize=11, fontweight='bold', color='#FF5722')
ax3.legend(fontsize=8); ax3.grid(True, alpha=0.3)

for ax in axes:
    ax.set_ylim(-2, 2)

plt.suptitle('🎓 Underfitting vs Good Fit vs Overfitting — Bias-Variance Tradeoff', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong5_overfitting.png', dpi=150, bbox_inches='tight')
plt.show()

print("\n💡 BIAS-VARIANCE TRADEOFF:")
print("   Bias cao (underfitting)  = Model quá đơn giản, bỏ sót pattern")
print("   Variance cao (overfitting) = Model quá phức tạp, nhạy với noise")  
print("   Mục tiêu: Tìm ĐIỂM CÂN BẰNG → Regularization giúp điều này!")
```

#### 🎯 Ví dụ FinTech: Fraud Detection 🏦

Hệ thống phát hiện gian lận ngân hàng kết hợp TẤT CẢ kiến thức:

| Thành phần | Chương | Vai trò |
|-----------|--------|---------|
| Mạng giao dịch | Ch.4 (Graph) | Phát hiện cluster đáng ngờ |
| Feature extraction | Ch.1 (ĐSTT) | PCA giảm 50 feature → 10 |
| Training model | Ch.2 (GD) | Adam optimizer, Cross-Entropy loss |
| Output xác suất | Ch.3 (Bayes) | P(fraud \| features) |
| Chống overfit | Ch.5 (Regularization) | L2 + Dropout |

> 📌 **Lưu ý cho giảng viên:** Đây là phần "aha moment" lớn nhất khi sinh viên thấy tất cả 4 chương hội tụ trong 1 ứng dụng thực tế. Cho làm mini project: phát hiện giao dịch gian lận trên dữ liệu giả lập.

---

> ✅ **CHECKPOINT 4 — Loss & Regularization**
>
> 1. Dropout là một kỹ thuật mạnh mẽ để chống Overfitting. Mường tượng nó hoạt động thế nào trong bài thi thực tế?
>
> <details><summary>📝 Đáp án</summary>
>
> Nó giống như bạn đang ôm cả quyển tập để làm bài thi, thì bỗng nhiên giám thị che 30% chữ ở các trang. Bạn phải dựa vào 70% dữ liệu còn lại cùng kiến thức đã học để suy luận. Quá trình bắt ép bộ não không được học vẹt cục bộ này làm kết quả thi cuối cùng thực tế hơn.
>
> </details>

---

## PHẦN 3C: TOÁN HỌC CỦA ATTENTION — "BÍ MẬT ĐẰNG SAU CHATGPT"

### 🟢 3C.1 Self-Attention = 3 phép nhân ma trận!

Mọi mô hình ngôn ngữ lớn (GPT, Gemini, Claude...) đều dùng **Self-Attention**:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) \cdot V$$

Nghe phức tạp? Thực ra mỗi phần bạn đã biết:

| Thành phần | Công thức | Bạn đã học ở | Ý nghĩa |
|-----------|-----------|-------------|---------|
| $Q$ (Query) | $Q = X \cdot W_Q$ | Ch.1 — Phép nhân ma trận | "Tôi đang TÌM gì?" |
| $K$ (Key) | $K = X \cdot W_K$ | Ch.1 — Phép nhân ma trận | "Tôi CÓ gì?" |
| $V$ (Value) | $V = X \cdot W_V$ | Ch.1 — Phép nhân ma trận | "Nội dung thực" |
| $QK^T$ | Tích vô hướng | Ch.1 — Dot product | "Điểm tương đồng" |
| $\sqrt{d_k}$ | Chia cho căn bậc 2 | Ch.1 — Norm | "Chuẩn hóa" |
| softmax | $\frac{e^{z_i}}{\sum e^{z_j}}$ | Ch.3 — Xác suất | "Chuyển thành XS" |

#### 🎯 Liên tưởng: Google Search 🔍

| Bước | Google Search | Self-Attention |
|------|-------------|----------------|
| 1 | Bạn gõ "quán phở ngon" | Query Q: Token hiện tại hỏi "Tôi liên quan đến token nào?" |
| 2 | Google so sánh với tiêu đề mọi website | Nhân $QK^T$: Tính điểm tương đồng với MỌI token khác |
| 3 | Xếp hạng kết quả (%) | softmax: Chuyển điểm → xác suất chú ý |
| 4 | Hiển thị nội dung top kết quả | Nhân với V: Lấy thông tin token quan trọng nhất |

### 🔵 3C.2 Code Python: Attention từ đầu

```python
import numpy as np

# =============================================
# SELF-ATTENTION — BÍ MẬT CỦA CHATGPT
# =============================================

def softmax(x, axis=-1):
    e_x = np.exp(x - np.max(x, axis=axis, keepdims=True))
    return e_x / np.sum(e_x, axis=axis, keepdims=True)

def self_attention(X, W_Q, W_K, W_V):
    """Self-Attention mechanism
    X: (seq_len, d_model) — Input embeddings
    W_Q, W_K, W_V: (d_model, d_k) — Weight matrices
    """
    Q = X @ W_Q  # Queries  (Ch.1: Nhân ma trận)
    K = X @ W_K  # Keys
    V = X @ W_V  # Values
    
    d_k = Q.shape[-1]
    
    # Attention scores (Ch.1: Dot product)
    scores = Q @ K.T / np.sqrt(d_k)  
    
    # Attention weights (Ch.3: Softmax → Xác suất)
    weights = softmax(scores)  
    
    # Output (Ch.1: Nhân ma trận)
    output = weights @ V
    
    return output, weights

# Ví dụ: 4 từ, mỗi từ embedding 8 chiều
np.random.seed(42)
seq_len, d_model, d_k = 4, 8, 4

tokens = ["Tôi", "yêu", "toán", "học"]
X = np.random.randn(seq_len, d_model)

# Weight matrices (HỌC được trong training)
W_Q = np.random.randn(d_model, d_k) * 0.1
W_K = np.random.randn(d_model, d_k) * 0.1
W_V = np.random.randn(d_model, d_k) * 0.1

output, attn_weights = self_attention(X, W_Q, W_K, W_V)

print("🤖 SELF-ATTENTION — Cách ChatGPT 'hiểu' ngôn ngữ")
print("=" * 55)
print(f"   Input: {tokens}")
print(f"   Embedding: {seq_len} tokens × {d_model} chiều")
print(f"\n   📊 Ma trận Attention (mỗi hàng = token chú ý đến token nào):")
print(f"   {'':>8}", end="")
for t in tokens:
    print(f"{t:>8}", end="")
print()

for i, token in enumerate(tokens):
    print(f"   {token:>8}", end="")
    for j in range(seq_len):
        val = attn_weights[i, j]
        bar = "█" * int(val * 20)
        print(f"  {val:.1%}", end="")
    print()

print(f"\n💡 Nhận xét:")
print(f"   Mỗi token 'nhìn' vào các token khác với mức chú ý khác nhau")
print(f"   Đây là lý do LLM hiểu ngữ cảnh: 'bank' trong 'river bank' vs 'money bank'")
print(f"\n🧮 Toán đằng sau = Ch.1 (Ma trận) + Ch.3 (Softmax) — Bạn đã biết hết!")
```

> 📌 **Lưu ý cho giảng viên:** Self-Attention là "ngôi sao" của AI hiện đại. Sinh viên thấy rằng GPT không phải phép thuật — chỉ là dot product + softmax + matrix multiply. Mọi thứ đã học ở Ch.1 và Ch.3! Multi-Head Attention = chạy nhiều attention song song rồi concat.

---

> ✅ **CHECKPOINT 5 — Attention Mechanism**
>
> 1. Trong Self-Attention, phép tính Toán cốt lõi nào giúp tìm "Điểm tương đồng" giữa 2 token xem chúng có liên quan tới nhau không?
>
> <details><summary>📝 Đáp án</summary>
>
> Đó chính là **Tích vô hướng (Dot Product)** giữa Query $Q$ và Key $K$. Vector nào cùng hướng với nhau (liên quan đến nhau) thì tích vô hướng cao.
>
> </details>

---

## PHẦN 4: CÁCH ĐỌC KÝ HIỆU TOÁN HỌC TRONG PAPER NGHIÊN CỨU

### 🟢 4.1 Bảng "Giải mã" ký hiệu

Đây là bảng tra cứu nhanh khi đọc paper — hãy bookmark lại!

| Ký hiệu | Đọc là | Ý nghĩa | Ví dụ thực tế |
|---------|--------|---------|---------------|
| $\sum$ | Sigma | Tổng | $\sum_{i=1}^{n} x_i$ = Cộng tất cả |
| $\prod$ | Pi (tích) | Tích | $\prod_{i=1}^{n} x_i$ = Nhân tất cả |
| $\in$ | Thuộc | Phần tử thuộc tập hợp | $x \in \mathbb{R}$ = x là số thực |
| $\forall$ | Với mọi | Mọi phần tử | $\forall x > 0$ = Với mọi x dương |
| $\exists$ | Tồn tại | Có ít nhất 1 | $\exists x: f(x) = 0$ = Có x làm f = 0 |
| $\nabla$ | Nabla | Gradient |$\nabla f$ = Vector đạo hàm riêng |
| $\partial$ | Partial | Đạo hàm riêng | $\frac{\partial f}{\partial x}$ = Đạo hàm theo x |
| $\arg\max$ | Arg max | Giá trị cho max | $\arg\max_x f(x)$ = x nào cho f lớn nhất |
| $\|x\|$ | Norm | Độ dài vector | $\|x\|_2 = \sqrt{\sum x_i^2}$ |
| $\sim$ | Tuân theo | Phân phối | $X \sim N(\mu, \sigma^2)$ = X theo Chuẩn |
| $\propto$ | Tỷ lệ | Tỉ lệ thuận | $P(A|B) \propto P(B|A)P(A)$ |
| $\mathbb{E}$ | Expectation | Kỳ vọng | $\mathbb{E}[X] = \mu$ |
| $\mathcal{L}$ | Loss | Hàm mất mát | $\mathcal{L} = -\sum y\log\hat{y}$ |
| $\theta$ | Theta | Tham số mô hình | $\theta^* = \arg\min \mathcal{L}(\theta)$ |
| $\otimes$ | Tensor product | Tích tensor | Dùng trong deep learning |

### 🟡 4.2 Chiến lược đọc Paper

```
BƯỚC ĐỌC PAPER NGHIÊN CỨU
├── 1. Đọc TIÊU ĐỀ + TÓM TẮT (Abstract)
│   └── Hiểu bài toán gì? Giải quyết bằng gì?
│
├── 2. Nhảy xuống HÌNH VẼ + BẢNG KẾT QUẢ  
│   └── Kết quả có tốt hơn baseline không?
│
├── 3. Đọc INTRODUCTION (phần cuối)
│   └── "Our contributions" — đóng góp chính là gì?
│
├── 4. Đọc METHOD (chọn lọc)
│   ├── Tìm công thức CHÍNH (thường là 1-2 công thức)
│   └── Dùng BẢNG GIẢI MÃ ở trên để hiểu
│
└── 5. ĐỌC LẠI toàn bộ (nếu cần)
    └── Lần này sẽ dễ hơn nhiều!
```

### 🔵 4.3 Ví dụ: Đọc công thức từ paper thật

**Paper:** "Attention Is All You Need" (Transformer)

Công thức chính:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

**Giải mã:**

| Phần | Ý nghĩa | Chương liên quan |
|------|---------|-----------------|
| $Q, K, V$ | Ma trận Query, Key, Value | Chương 1 (Ma trận) |
| $QK^T$ | Nhân ma trận | Chương 1 (Phép nhân ma trận) |
| $\sqrt{d_k}$ | Chia để normalize | Chương 3 (Variance scaling) |
| $\text{softmax}$ | Chuyển thành xác suất | Chương 3 (Phân phối) |
| $\times V$ | Nhân với giá trị | Chương 1 (Phép nhân ma trận) |

→ Toàn bộ Transformer chỉ là **nhân ma trận + xác suất** — thứ bạn đã biết!

```python
import numpy as np

# =============================================
# TÁI TẠO SELF-ATTENTION (đơn giản)
# =============================================

def self_attention(X, d_model=4):
    """
    Self-Attention từ paper "Attention Is All You Need"
    
    Tham số:
        X: Ma trận input (seq_len × d_model)
        d_model: Kích thước embedding
    """
    seq_len = X.shape[0]
    
    # Tạo ma trận trọng số (trong thực tế, chúng được TRAIN bởi GD)
    np.random.seed(42)
    W_Q = np.random.randn(d_model, d_model) * 0.1
    W_K = np.random.randn(d_model, d_model) * 0.1
    W_V = np.random.randn(d_model, d_model) * 0.1
    
    # Bước 1: Tính Q, K, V (Chương 1: Nhân ma trận)
    Q = X @ W_Q
    K = X @ W_K
    V = X @ W_V
    
    # Bước 2: Attention scores (Chương 1: Tích vô hướng)
    scores = Q @ K.T / np.sqrt(d_model)  # Chương 3: Scaling
    
    # Bước 3: Softmax (Chương 3: Chuyển thành xác suất)
    def softmax(x):
        exp_x = np.exp(x - np.max(x, axis=-1, keepdims=True))
        return exp_x / np.sum(exp_x, axis=-1, keepdims=True)
    
    attention_weights = softmax(scores)
    
    # Bước 4: Output (Chương 1: Nhân ma trận)
    output = attention_weights @ V
    
    return output, attention_weights

# Demo: Câu "Tôi yêu Toán học"
# Giả sử mỗi từ đã được embedding thành vector 4 chiều
cau = {
    "Tôi": [1.0, 0.2, 0.5, 0.1],
    "yêu": [0.3, 1.0, 0.8, 0.2],
    "Toán": [0.1, 0.3, 0.2, 1.0],
    "học": [0.2, 0.5, 0.3, 0.9]
}

X = np.array(list(cau.values()))
output, weights = self_attention(X)

print("🧠 SELF-ATTENTION — 'Attention Is All You Need'")
print("=" * 50)
print(f"\nInput: {list(cau.keys())}")
print(f"\nAttention Weights (Mỗi từ 'chú ý' đến từ nào?):")
tu_list = list(cau.keys())
for i, tu in enumerate(tu_list):
    print(f"   {tu:>5} → ", end="")
    for j, tu2 in enumerate(tu_list):
        w = weights[i, j]
        bar = "█" * int(w * 20)
        print(f"{tu2}({w:.2f}){bar} ", end="")
    print()

# Trực quan
fig, ax = plt.subplots(figsize=(7, 6))
im = ax.imshow(weights, cmap='YlOrRd', interpolation='nearest')
ax.set_xticks(range(len(tu_list))); ax.set_xticklabels(tu_list, fontsize=12)
ax.set_yticks(range(len(tu_list))); ax.set_yticklabels(tu_list, fontsize=12)

for i in range(len(tu_list)):
    for j in range(len(tu_list)):
        ax.text(j, i, f'{weights[i,j]:.2f}', ha='center', va='center',
               fontsize=11, fontweight='bold',
               color='white' if weights[i,j] > 0.3 else 'black')

ax.set_title('🧠 Self-Attention Weights\n"Mỗi từ chú ý đến từ nào?"', 
             fontsize=14, fontweight='bold')
ax.set_xlabel("Key (Nguồn chú ý)", fontsize=11)
ax.set_ylabel("Query (Từ đang xét)", fontsize=11)
plt.colorbar(im, shrink=0.8, label="Attention Weight")
plt.tight_layout()
plt.savefig('chuong5_attention.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 6 — Paper Reading**
>
> 1. Ký hiệu $\arg\max_x f(x)$ nghĩa là gì?
>
> <details><summary>📝 Đáp án</summary>
>
> Nó không phải là đi tìm "giá trị cực đại" (max) của hàm $f(x)$! Giá trị đó có thể là $f(x) = 1.05$. Thay vào đó, nó là việc **chỉ ra cái Input x nào** đã tạo nên được giá trị cực đại kia. Nghĩa là kết quả của hàm Argmax là giá trị của $x$.
>
> </details>

---

## PHẦN 5: TÓM TẮT TOÀN BỘ GIÁO TRÌNH

### 🗺️ Bản đồ kiến thức TOÀN BỘ

```
╔══════════════════════════════════════════════════════════════╗
║            🧮 TOÁN HỌC KHÔNG RÀO CẢN                       ║
║            Giáo trình dành cho Sinh viên CNTT                ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  📐 Chương 1: ĐẠI SỐ TUYẾN TÍNH                            ║
║  ├── Vector → "Mũi tên" biểu diễn mọi thứ                  ║
║  ├── Ma trận → "Máy biến hình" xử lý dữ liệu               ║
║  ├── Eigenvalue → "Linh hồn" của ma trận                    ║
║  └── SVD → Nén ảnh, giảm chiều dữ liệu                     ║
║                                                              ║
║  📈 Chương 2: GIẢI TÍCH                                     ║
║  ├── Đạo hàm → "Độ dốc" / Tốc độ thay đổi                  ║
║  ├── Gradient Descent → Thuật toán TỐI ƯU #1                ║
║  └── Backpropagation → Cách AI HỌC (Chain Rule)             ║
║                                                              ║
║  🎲 Chương 3: XÁC SUẤT THỐNG KÊ                            ║
║  ├── Phân phối → "Hình dạng" của sự ngẫu nhiên              ║
║  ├── Định lý Bayes → "Vũ khí bí mật" của AI                 ║
║  └── Naive Bayes → Lọc Spam, Phân loại                      ║
║                                                              ║
║  🌐 Chương 4: TOÁN RỜI RẠC                                  ║
║  ├── Đồ thị → Mạng lưới kết nối vạn vật                     ║
║  ├── BFS/DFS → Khám phá mạng lưới                           ║
║  ├── Dijkstra → GPS Navigator                                ║
║  └── Logic & Tổ hợp → Nền tảng lập trình                    ║
║                                                              ║
║  🤖 Chương 5: TƯ DUY HỆ THỐNG                              ║
║  ├── Tổng hợp 4 chương → Computer Vision, NLP               ║
║  ├── Federated Learning → AI bảo mật                         ║
║  └── Đọc Paper nghiên cứu → Không còn sợ hãi                ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

### ✅ Checklist cuối cùng

Sau toàn bộ giáo trình, bạn cần:

- [ ] Hiểu **ý nghĩa** của toán, không chỉ công thức
- [ ] Viết code Python cho **mọi** khái niệm
- [ ] Giải thích toán bằng **ngôn ngữ đời thường**
- [ ] Đọc được **công thức toán** trong paper AI
- [ ] Xây dựng được **hệ thống AI đơn giản** từ đầu
- [ ] Không còn **sợ hãi** khi gặp ký hiệu toán học

---

## 📌 LƯU Ý CUỐI CHO GIẢNG VIÊN

### Triết lý xuyên suốt

1. **"Tại sao" trước "Thế nào":** Luôn giải thích WHY trước HOW
2. **"Cảm nhận" trước "Chứng minh":** Trực quan → Hình thức hóa
3. **"Code" trước "Bài tập":** Chạy thử → Hiểu → Làm bài
4. **"Thực tế" trước "Lý thuyết":** Instagram → Ma trận → SVD

### Lộ trình dạy gợi ý

| Tuần | Nội dung | Hoạt động chính |
|------|---------|----------------|
| 1-2 | Chương 1 (Vector, Ma trận) | Lab: Xử lý ảnh với NumPy |
| 3 | Chương 1 (Eigenvalue, SVD) | Mini Project: Nén ảnh |
| 4-5 | Chương 2 (Đạo hàm, GD) | Lab: Gradient Descent trực quan |
| 6 | Chương 2 (Neural Network) | Mini Project: XOR Network |
| 7-8 | Chương 3 (Xác suất, Bayes) | Lab: Mô phỏng & Spam Filter |
| 9 | Chương 3 (Phân phối, MLE) | Mini Project: Spam Classifier |
| 10-11 | Chương 4 (Đồ thị, BFS/DFS) | Lab: Xây đồ thị thực tế |
| 12 | Chương 4 (Dijkstra, Logic) | Mini Project: GPS Navigator |
| 13-14 | Chương 5 (Tổng hợp) | Final Project: Phân loại ảnh |
| 15 | Chương 5 (Paper reading) | Seminar: Đọc 1 paper thật |

---

> *"Toán học không phải là điều bạn NHỚ. Toán học là điều bạn HIỂU — và khi hiểu rồi, bạn sẽ không bao giờ quên."*
> 
> — Triết lý của giáo trình "Toán học không rào cản"
