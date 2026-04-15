# CHƯƠNG 1: ĐẠI SỐ TUYẾN TÍNH — NGÔN NGỮ CỦA KHÔNG GIAN

## 📖 LINEAR ALGEBRA — THE LANGUAGE OF SPACE

> *"Nếu bạn muốn hiểu cách Instagram lọc màu ảnh, cách Netflix gợi ý phim, hay cách ChatGPT hiểu ngôn ngữ — thì tất cả bắt đầu từ đây: Đại số Tuyến tính."*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 1: "Được sinh ra"
>
> *Neuron N-42 vừa được tạo ra trong một neural network. Nó chỉ là một nút nhỏ, nhận input qua các "dây nối" (weights) và xuất output. Nhưng nó chưa biết weights là gì, matrix là gì, hay tại sao dot product lại quan trọng. Chương này, N-42 sẽ học NGÔN NGỮ ĐẦU TIÊN của mình — Đại số Tuyến tính.*

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: Bức ảnh 12 triệu pixel

Bạn có bao giờ tự hỏi: Khi bạn mở một bức ảnh trên điện thoại — bức ảnh selfie chụp hôm qua chẳng hạn — thì điện thoại "nhìn thấy" gì?

Bạn thấy khuôn mặt, bầu trời, hàng cây. Nhưng máy tính thì không. Với nó, bức ảnh chỉ là **một bảng số khổng lồ**. Mỗi pixel là một con số (hoặc bộ 3 số RGB). Bức ảnh 4000×3000 pixel chính là một **ma trận** có 12 triệu phần tử.

Và khi Instagram áp filter "Clarendon" lên ảnh? Nó không phải "phép thuật" — nó là **phép nhân ma trận**. Mỗi filter là một ma trận biến đổi, nhân với ma trận ảnh gốc để tạo ra ảnh mới.

Đó chính là Đại số Tuyến tính — bộ môn toán học mà **mọi** phần mềm xử lý dữ liệu đều phải dùng.

> 💡 **Takeaway:** Đại số tuyến tính = Ngôn ngữ mà máy tính dùng để "nhìn", "nghe", "hiểu" thế giới.

---

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Khái niệm | Một câu tóm tắt | Tại sao cần cho AI |
|---|-----------|-----------------|---------------------|
| 1 | **Vector** | Mũi tên có hướng & độ lớn | Input/output của neural network |
| 2 | **Dot Product** | Đo độ "giống nhau" giữa 2 vector | Recommendation, Attention |
| 3 | **Ma trận** | Bảng 2D — Phép biến đổi không gian | Weight matrix trong mọi layer |
| 4 | **Định thức & Nghịch đảo** | Đo co giãn, giải hệ phương trình | Kiểm tra weight suy biến |
| 5 | **Norm (L1, L2)** | Đo "kích thước" vector | Regularization (chống overfit) |
| 6 | **Eigenvalue/vector** | Hướng bất biến khi nhân ma trận | PCA, Google PageRank |
| 7 | **SVD & PCA** | Phân rã & giảm chiều dữ liệu | Nén ảnh, Feature extraction |
| 8 | **Tensor** | Ma trận mở rộng nhiều chiều | Cấu trúc dữ liệu cốt lõi của PyTorch |

> 📋 **Prerequisites:** Kiến thức toán phổ thông (cộng, trừ, nhân). Không cần biết trước ĐSTT.
>
> ⏱️ **Thời gian đọc:** ~60 phút (Track A — Developer) | ~90 phút (Track B — Researcher)

---

## PHẦN 1: VECTOR — MŨI TÊN ĐỊNH HƯỚNG

### 1.1 Vector KHÔNG phải là dãy số

Khi nghe "vector", nhiều sinh viên nghĩ đến một dãy số khô khan: `[3, 5, 2]`. Nhưng thực tế, vector giàu ý nghĩa hơn nhiều.

#### 🎯 Liên tưởng 1: Vector là mũi tên

Hãy tưởng tượng bạn đứng giữa sân trường. Bạn bè hỏi đường đến thư viện. Bạn nói:

> *"Đi thẳng 100 mét về hướng Bắc, rồi rẽ phải 50 mét."*

Chỉ dẫn đó chính là một **vector** — nó cho biết **hướng** và **độ lớn** (khoảng cách). Trong toán học:

$$\vec{v} = \begin{pmatrix} 50 \\ 100 \end{pmatrix}$$

- `50` là khoảng cách theo trục Đông-Tây (hướng phải)
- `100` là khoảng cách theo trục Bắc-Nam (hướng lên)

#### 🎯 Liên tưởng 2: Vector là "tọa độ tâm trạng"

Giả sử bạn muốn mô tả tâm trạng của mình bằng số:

| Thuộc tính | Giá trị |
|-----------|---------|
| Vui vẻ | 8/10 |
| Mệt mỏi | 3/10 |
| Tập trung | 7/10 |

Bộ số `[8, 3, 7]` chính là **vector tâm trạng** của bạn trong không gian 3 chiều. Ngày mai, tâm trạng bạn là `[5, 7, 4]` — vector khác, vị trí khác trong "không gian cảm xúc".

> 💡 **Nhận ra chưa?** Vector không nhất thiết sống trong không gian vật lý. Nó có thể sống trong **bất kỳ không gian nào** mà bạn định nghĩa: không gian màu sắc, không gian từ vựng, không gian sở thích phim...

### 1.2 Các phép toán trên Vector

#### Cộng vector — "Ghép hai chỉ dẫn lại"

Nếu bạn đi theo vector $\vec{a} = (3, 2)$ rồi tiếp tục đi theo vector $\vec{b} = (1, 4)$, thì tổng quãng đường bạn đi là:

$$\vec{a} + \vec{b} = (3+1, 2+4) = (4, 6)$$

Giống như ghép hai chặng bay: Hà Nội → Đà Nẵng → TP.HCM = Hà Nội → TP.HCM.

#### Nhân vô hướng — "Phóng to/thu nhỏ mũi tên"

$$2 \cdot \vec{a} = 2 \cdot (3, 2) = (6, 4)$$

Vector cùng hướng nhưng dài gấp đôi. Như khi bạn zoom ảnh 200%.

#### Tích vô hướng (Dot Product) — "Đo sự giống nhau"

$$\vec{a} \cdot \vec{b} = a_1 b_1 + a_2 b_2 + \ldots + a_n b_n$$

Đây là phép toán **quan trọng bậc nhất** trong AI. Tại sao?

Hãy tưởng tượng: Netflix biểu diễn sở thích phim của bạn bằng vector `[Hành_động, Tình_cảm, Kinh_dị]`:

- Sở thích của bạn: $\vec{u} = [9, 2, 1]$ (Mê phim hành động, không thích kinh dị)
- Bộ phim A: $\vec{f_A} = [8, 3, 2]$ (Phim hành động pha tình cảm)
- Bộ phim B: $\vec{f_B} = [1, 2, 9]$ (Phim kinh dị)

Tích vô hướng:
- $\vec{u} \cdot \vec{f_A} = 9 \times 8 + 2 \times 3 + 1 \times 2 = 72 + 6 + 2 = 80$ ← **Rất phù hợp!**
- $\vec{u} \cdot \vec{f_B} = 9 \times 1 + 2 \times 2 + 1 \times 9 = 9 + 4 + 9 = 22$ ← Ít phù hợp

> Netflix sẽ gợi ý Phim A cho bạn. **Đó chính là Recommendation System.**

### 1.3 Code Python: Khám phá Vector

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. TẠO VECTOR VÀ CÁC PHÉP TOÁN CƠ BẢN
# =============================================

# Vector 2D - Mũi tên chỉ đường
huong_thu_vien = np.array([50, 100])   # Đi phải 50m, lên 100m
huong_canteen  = np.array([80, -30])   # Đi phải 80m, xuống 30m

# Cộng vector: Đi đến thư viện rồi đi tiếp đến canteen
tong_hanh_trinh = huong_thu_vien + huong_canteen
print(f"🧭 Hướng thư viện:  {huong_thu_vien}")
print(f"🍔 Hướng canteen:   {huong_canteen}")
print(f"📍 Tổng hành trình: {tong_hanh_trinh}")

# Nhân vô hướng: Phóng to gấp đôi
print(f"\n🔍 Gấp đôi hướng thư viện: {2 * huong_thu_vien}")

# =============================================
# 2. TÍCH VÔ HƯỚNG - ĐO SỰ GIỐNG NHAU
# =============================================

# Sở thích phim: [Hành_động, Tình_cảm, Kinh_dị]
so_thich_ban   = np.array([9, 2, 1])
phim_avengers  = np.array([8, 3, 2])
phim_conjuring = np.array([1, 2, 9])
phim_titanic   = np.array([2, 9, 1])

diem_avengers  = np.dot(so_thich_ban, phim_avengers)
diem_conjuring = np.dot(so_thich_ban, phim_conjuring)
diem_titanic   = np.dot(so_thich_ban, phim_titanic)

print(f"\n🎬 Recommendation System đơn giản:")
print(f"   Avengers:    {diem_avengers} điểm")
print(f"   Conjuring:   {diem_conjuring} điểm")
print(f"   Titanic:     {diem_titanic} điểm")

# Sắp xếp gợi ý
danh_sach = {"Avengers": diem_avengers, 
             "Conjuring": diem_conjuring, 
             "Titanic": diem_titanic}
goi_y = sorted(danh_sach.items(), key=lambda x: x[1], reverse=True)
print(f"\n🏆 Gợi ý cho bạn: {goi_y[0][0]} (điểm: {goi_y[0][1]})")

# =============================================
# 3. TRỰC QUAN HÓA VECTOR 2D
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# --- Hình 1: Cộng vector ---
ax1 = axes[0]
ax1.set_xlim(-10, 150)
ax1.set_ylim(-50, 130)
ax1.set_aspect('equal')
ax1.grid(True, alpha=0.3)
ax1.set_title("Cộng Vector: Ghép hai hành trình", fontsize=13, fontweight='bold')

# Vẽ vector a (thư viện)
ax1.quiver(0, 0, 50, 100, angles='xy', scale_units='xy', scale=1, 
           color='#2196F3', width=0.008, label='Hướng thư viện')
ax1.annotate('Thư viện\n(50, 100)', xy=(50, 100), fontsize=9, 
             color='#2196F3', fontweight='bold')

# Vẽ vector b (canteen) bắt đầu từ đầu vector a
ax1.quiver(50, 100, 80, -30, angles='xy', scale_units='xy', scale=1, 
           color='#FF5722', width=0.008, label='Hướng canteen')
ax1.annotate('Canteen\n(80, -30)', xy=(130, 70), fontsize=9, 
             color='#FF5722', fontweight='bold')

# Vẽ vector tổng
ax1.quiver(0, 0, 130, 70, angles='xy', scale_units='xy', scale=1, 
           color='#4CAF50', width=0.008, linestyle='dashed', 
           label='Tổng hành trình')
ax1.annotate('Đích = (130, 70)', xy=(130, 70), xytext=(90, 40),
             fontsize=9, color='#4CAF50', fontweight='bold',
             arrowprops=dict(arrowstyle='->', color='#4CAF50'))

ax1.plot(0, 0, 'ko', markersize=8)
ax1.annotate('Bạn đứng đây', xy=(0, 0), xytext=(5, -20), fontsize=9)
ax1.legend(loc='upper left', fontsize=9)

# --- Hình 2: Tích vô hướng - Recommendation ---
ax2 = axes[1]
phim_names = ['Avengers', 'Conjuring', 'Titanic']
phim_scores = [diem_avengers, diem_conjuring, diem_titanic]
colors = ['#4CAF50', '#f44336', '#FF9800']

bars = ax2.barh(phim_names, phim_scores, color=colors, edgecolor='white', height=0.5)
ax2.set_xlabel('Điểm phù hợp (Dot Product)', fontsize=11)
ax2.set_title('🎬 Gợi ý phim bằng Dot Product', fontsize=13, fontweight='bold')

for bar, score in zip(bars, phim_scores):
    ax2.text(bar.get_width() + 1, bar.get_y() + bar.get_height()/2, 
             f'{score}', va='center', fontsize=12, fontweight='bold')

ax2.set_xlim(0, max(phim_scores) + 15)
ax2.grid(axis='x', alpha=0.3)

plt.tight_layout()
plt.savefig('chuong1_vector_visualization.png', dpi=150, bbox_inches='tight')
plt.show()
print("\n✅ Đã lưu hình minh họa: chuong1_vector_visualization.png")
```

**Kết quả chạy code:**

```
🧭 Hướng thư viện:  [ 50 100]
🍔 Hướng canteen:   [ 80 -30]
📍 Tổng hành trình: [130  70]

🔍 Gấp đôi hướng thư viện: [100 200]

🎬 Recommendation System đơn giản:
   Avengers:    80 điểm
   Conjuring:   22 điểm
   Titanic:     20 điểm

🏆 Gợi ý cho bạn: Avengers (điểm: 80)
```

> 📌 **Lưu ý cho giảng viên:** Hãy để sinh viên tự thay đổi vector sở thích và vector phim, sau đó nhận xét kết quả. Hoạt động này tạo ra "khoảnh khắc aha" khi sinh viên nhận ra tích vô hướng thực sự đo lường sự tương đồng.

---

## PHẦN 2: MA TRẬN — BẢNG SỐ BIẾN HÌNH

### 2.1 Ma trận là gì?

Bạn đã dùng bảng tính Excel chưa? **Ma trận chính là bảng tính** — nhưng thay vì ghi tên nhân viên hay doanh thu, nó chứa các con số có ý nghĩa toán học.

$$A = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{pmatrix}$$

Ma trận $A$ có 3 hàng, 3 cột → Ta gọi nó là ma trận **3×3**.

#### 🎯 Liên tưởng: Ma trận là "Máy biến hình"

Hãy nghĩ ma trận như một **cái máy**: bạn đưa vector vào, máy "xoay", "kéo giãn", "lật" nó, rồi trả ra vector mới.

Ví dụ: Ma trận xoay 90° ngược chiều kim đồng hồ:

$$R = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$

Nếu bạn đưa vector $\vec{v} = (1, 0)$ (mũi tên chỉ sang phải) qua "máy" $R$:

$$R \cdot \vec{v} = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix} \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

Kết quả: mũi tên bây giờ chỉ lên trên! Vector đã bị **xoay 90°**. Đó là "phép thuật" của phép nhân ma trận.

### 2.2 Phép nhân Ma trận — Tại sao quan trọng?

Phép nhân ma trận **không phải** nhân từng phần tử tương ứng (nhiều sinh viên nhầm điểm này).

Công thức: Nếu $C = A \times B$ thì phần tử ở hàng $i$, cột $j$ của $C$ là:

$$C_{ij} = \sum_{k} A_{ik} \cdot B_{kj}$$

#### Liên tưởng thực tế: Bảng điểm sinh viên

Giả sử bạn có **điểm thi** (ma trận A) và **trọng số môn học** (ma trận B):

**Ma trận A** — Điểm thi (3 sinh viên × 3 môn):

| | Toán | Lý | Tin |
|---|---|---|---|
| An | 8 | 7 | 9 |
| Bình | 6 | 9 | 7 |
| Chi | 9 | 6 | 8 |

**Ma trận B** — Trọng số (3 môn × 1):

| Môn | Trọng số |
|---|---|
| Toán | 0.4 |
| Lý | 0.3 |
| Tin | 0.3 |

$$A \times B = \begin{pmatrix} 8 & 7 & 9 \\ 6 & 9 & 7 \\ 9 & 6 & 8 \end{pmatrix} \times \begin{pmatrix} 0.4 \\ 0.3 \\ 0.3 \end{pmatrix} = \begin{pmatrix} 8 \times 0.4 + 7 \times 0.3 + 9 \times 0.3 \\ 6 \times 0.4 + 9 \times 0.3 + 7 \times 0.3 \\ 9 \times 0.4 + 6 \times 0.3 + 8 \times 0.3 \end{pmatrix} = \begin{pmatrix} 8.0 \\ 7.2 \\ 7.8 \end{pmatrix}$$

→ An: 8.0 điểm TB, Bình: 7.2, Chi: 7.8. **Phép nhân ma trận** cho ta ngay bảng điểm trung bình có trọng số!

### 2.3 Ma trận chuyển vị (Transpose)

"Lật" ma trận: hàng thành cột, cột thành hàng.

$$A = \begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix} \quad \Rightarrow \quad A^T = \begin{pmatrix} 1 & 3 & 5 \\ 2 & 4 & 6 \end{pmatrix}$$

Hình dung: bạn cầm tờ giấy ghi bảng, rồi nghiêng 90° — hàng trở thành cột.

### 2.4 Code Python: Ma trận trong thực tế

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. TẠO MA TRẬN VÀ PHÉP TOÁN CƠ BẢN
# =============================================

# Điểm thi 3 sinh viên, 3 môn
diem_thi = np.array([
    [8, 7, 9],   # An
    [6, 9, 7],   # Bình
    [9, 6, 8]    # Chi
])

trong_so = np.array([[0.4], [0.3], [0.3]])

# Tính điểm trung bình có trọng số bằng phép nhân ma trận
diem_tb = diem_thi @ trong_so  # Toán tử @ = np.matmul

sinh_vien = ['An', 'Bình', 'Chi']
print("📊 Bảng điểm trung bình có trọng số:")
for ten, diem in zip(sinh_vien, diem_tb):
    print(f"   {ten}: {diem[0]:.1f}")

# =============================================
# 2. MA TRẬN LÀ "MÁY BIẾN HÌNH"
# =============================================

# Ma trận xoay 45 độ
goc = np.radians(45)
ma_tran_xoay = np.array([
    [np.cos(goc), -np.sin(goc)],
    [np.sin(goc),  np.cos(goc)]
])

# Vector gốc: Hình vuông
hinh_vuong = np.array([
    [0, 1, 1, 0, 0],  # x
    [0, 0, 1, 1, 0]   # y
])

# Xoay hình vuông
hinh_xoay = ma_tran_xoay @ hinh_vuong

# Ma trận co giãn (scale)
ma_tran_scale = np.array([
    [2.0, 0],
    [0, 0.5]
])
hinh_scale = ma_tran_scale @ hinh_vuong

# Ma trận biến dạng (shear)
ma_tran_shear = np.array([
    [1, 0.5],
    [0, 1]
])
hinh_shear = ma_tran_shear @ hinh_vuong

# =============================================
# 3. TRỰC QUAN HÓA: Phép biến đổi hình học
# =============================================

fig, axes = plt.subplots(1, 4, figsize=(18, 4.5))

bien_doi = [
    ("Gốc", hinh_vuong, '#2196F3'),
    ("Xoay 45°", hinh_xoay, '#4CAF50'),
    ("Co giãn (2x, 0.5x)", hinh_scale, '#FF5722'),
    ("Biến dạng (Shear)", hinh_shear, '#9C27B0')
]

for ax, (ten, hinh, mau) in zip(axes, bien_doi):
    ax.fill(hinh[0], hinh[1], alpha=0.3, color=mau)
    ax.plot(hinh[0], hinh[1], color=mau, linewidth=2)
    ax.set_xlim(-1.5, 3)
    ax.set_ylim(-1.5, 2)
    ax.set_aspect('equal')
    ax.grid(True, alpha=0.3)
    ax.axhline(y=0, color='gray', linewidth=0.5)
    ax.axvline(x=0, color='gray', linewidth=0.5)
    ax.set_title(ten, fontsize=12, fontweight='bold', color=mau)

plt.suptitle("🔄 Ma trận = Máy biến hình: Xoay, Co giãn, Biến dạng", 
             fontsize=14, fontweight='bold', y=1.02)
plt.tight_layout()
plt.savefig('chuong1_ma_tran_bien_hinh.png', dpi=150, bbox_inches='tight')
plt.show()
print("\n✅ Đã lưu hình minh họa: chuong1_ma_tran_bien_hinh.png")
```

> 📌 **Lưu ý cho giảng viên:** Cho sinh viên thay đổi giá trị trong ma trận xoay (từ 45° thành 90°, 180°) để "cảm" được tác dụng. Sau đó hỏi: "Nếu nhân hai ma trận xoay liên tiếp thì sao?" → Dẫn dắt đến tính chất kết hợp.

---

## PHẦN 2B: ĐỊNH THỨC & MA TRẬN NGHỊCH ĐẢO — "UNDO PHÉP BIẾN HÌNH"

### 2B.1 Định thức (Determinant) — "Hệ số co giãn diện tích"

Chúng ta đã biết ma trận là "máy biến hình". **Định thức** cho biết máy đó **phóng to hay thu nhỏ diện tích** bao nhiêu lần.

$$\det(A) = |A|$$

Với ma trận 2×2:

$$\det\begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc$$

#### 🎯 Liên tưởng: Kéo giãn miếng bột pizza

Bạn có miếng bột pizza hình vuông 1×1. Khi "nhào" nó qua ma trận $A$:
- $\det(A) = 2$: Miếng bột giãn ra gấp đôi diện tích
- $\det(A) = 0.5$: Miếng bột co lại còn nửa
- $\det(A) = 0$: Miếng bột bị ép dẹp thành **đường thẳng** → Mất thông tin!
- $\det(A) < 0$: Miếng bột bị **lật ngược** (như soi gương)

> 💡 **Quy tắc quan trọng:** $\det(A) = 0$ → Ma trận **suy biến** (singular) → KHÔNG CÓ nghịch đảo → Hệ phương trình vô nghiệm hoặc vô số nghiệm.

#### 🎯 Ví dụ kinh tế: Kiểm tra tính khả thi của hệ thống kinh tế

Trong mô hình kinh tế Leontief, nếu $\det(I - A) = 0$ thì hệ thống sản xuất **không khả thi** — các ngành kinh tế phụ thuộc vòng tròn, không thể tự cung tự cấp.

### 2B.2 Ma trận nghịch đảo — "Nút Undo"

Nếu ma trận $A$ là "máy biến hình", thì $A^{-1}$ là **nút Undo** — đưa vector về trạng thái ban đầu:

$$A \cdot A^{-1} = A^{-1} \cdot A = I \quad \text{(Ma trận đơn vị)}$$

Với ma trận 2×2:

$$A^{-1} = \frac{1}{\det(A)} \begin{pmatrix} d & -b \\ -c & a \end{pmatrix}$$

#### 🎯 Liên tưởng: Giải mã tin nhắn

Alice mã hóa tin nhắn bằng ma trận $A$ (nhân vector tin gốc với $A$). Bob muốn đọc tin → nhân với $A^{-1}$ để **giải mã**. Nhưng nếu $\det(A) = 0$ → tin nhắn đã bị "nén mất" thông tin → KHÔNG THỂ giải mã!

#### 🎯 Ví dụ tài chính: Cân bằng danh mục đầu tư

Bạn muốn tìm tỷ trọng đầu tư sao cho danh mục đạt lợi nhuận mục tiêu:

$$A \cdot \vec{w} = \vec{r}$$

- $A$ = ma trận lợi suất các kịch bản
- $\vec{w}$ = vector tỷ trọng (cần tìm)
- $\vec{r}$ = vector lợi nhuận mong muốn

→ $\vec{w} = A^{-1} \cdot \vec{r}$ (Nếu $A$ khả nghịch)

### 2B.3 Code Python: Định thức & Nghịch đảo

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. ĐỊNH THỨC — ĐO "SỰ CO GIÃN"
# =============================================

# Ma trận biến đổi khác nhau
matrices = {
    "Xoay 45°": np.array([[np.cos(np.pi/4), -np.sin(np.pi/4)],
                            [np.sin(np.pi/4),  np.cos(np.pi/4)]]),
    "Co giãn 2x,0.5x": np.array([[2, 0], [0, 0.5]]),
    "Shear": np.array([[1, 0.5], [0, 1]]),
    "Suy biến ⚠️": np.array([[1, 2], [2, 4]]),  # det = 0!
}

print("📐 ĐỊNH THỨC CÁC MA TRẬN:")
print("=" * 50)
for name, M in matrices.items():
    d = np.linalg.det(M)
    print(f"   {name:20s}: det = {d:+.3f}", end="")
    if abs(d) < 1e-10:
        print(" ← ⚠️ SỤP ĐỔ! Không có nghịch đảo")
    elif abs(d) == 1:
        print(" ← Bảo toàn diện tích")
    elif abs(d) > 1:
        print(f" ← Phóng to {abs(d):.1f}x diện tích")
    else:
        print(f" ← Thu nhỏ còn {abs(d):.1%} diện tích")

# =============================================
# 2. MA TRẬN NGHỊCH ĐẢO — "NÚT UNDO"
# =============================================

A = np.array([[3, 1], [2, 4]])
A_inv = np.linalg.inv(A)

print(f"\n🔄 MA TRẬN NGHỊCH ĐẢO:")
print(f"   A =\n{A}")
print(f"   A⁻¹ =\n{np.round(A_inv, 4)}")
print(f"   A × A⁻¹ =\n{np.round(A @ A_inv, 10)}")
print(f"   → Kết quả = Ma trận đơn vị I ✅")

# =============================================
# 3. ỨNG DỤNG: GIẢI MÃ TIN NHẮN
# =============================================

print(f"\n🔐 ỨNG DỤNG: MÃ HÓA/GIẢI MÃ TIN NHẮN")
# Mã hóa: mỗi chữ cái = số (A=1, B=2, ...)
tin_goc = np.array([8, 9])  # "HI"
ma_tran_ma_hoa = np.array([[3, 1], [2, 4]])

tin_ma_hoa = ma_tran_ma_hoa @ tin_goc
print(f"   Tin gốc: {tin_goc} ('HI')")
print(f"   Sau mã hóa: {tin_ma_hoa}")

tin_giai_ma = np.linalg.inv(ma_tran_ma_hoa) @ tin_ma_hoa
print(f"   Sau giải mã: {np.round(tin_giai_ma)} → 'HI' ✅")

# =============================================
# 4. TRỰC QUAN HÓA: ĐỊNH THỨC = THAY ĐỔI DIỆN TÍCH
# =============================================

fig, axes = plt.subplots(1, 4, figsize=(20, 5))

# Hình vuông đơn vị
hinh_goc = np.array([[0, 1, 1, 0, 0], [0, 0, 1, 1, 0]])

for ax, (name, M) in zip(axes, matrices.items()):
    det_val = np.linalg.det(M)
    hinh_moi = M @ hinh_goc
    
    # Vẽ hình gốc
    ax.fill(hinh_goc[0], hinh_goc[1], alpha=0.2, color='blue')
    ax.plot(hinh_goc[0], hinh_goc[1], 'b--', linewidth=1, label='Gốc (S=1)')
    
    # Vẽ hình biến đổi
    color = '#f44336' if abs(det_val) < 0.01 else '#4CAF50'
    ax.fill(hinh_moi[0], hinh_moi[1], alpha=0.3, color=color)
    ax.plot(hinh_moi[0], hinh_moi[1], color=color, linewidth=2, 
            label=f'Biến đổi (S={abs(det_val):.2f})')
    
    ax.set_title(f"{name}\ndet = {det_val:+.2f}", fontsize=11, fontweight='bold')
    ax.set_xlim(-1.5, 3.5); ax.set_ylim(-1.5, 2.5)
    ax.set_aspect('equal'); ax.grid(True, alpha=0.3)
    ax.legend(fontsize=8)

plt.suptitle("📐 Định thức = Hệ số thay đổi diện tích khi biến đổi", 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong1_dinh_thuc.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Điểm nhấn quan trọng nhất: $\det(A) = 0$ nghĩa là ma trận "ép dẹp" không gian → mất thông tin → không thể undo. Điều này có hệ quả lớn trong ML khi ma trận gradient bị suy biến.

---

## PHẦN 2C: HỆ PHƯƠNG TRÌNH TUYẾN TÍNH — "GIẢI ĐỐ BẰNG MA TRẬN"

### 2C.1 Từ bài toán đời thường đến ma trận

#### 🎯 Câu chuyện: Quán trà sữa và bài toán nguyên liệu

Bạn mở quán trà sữa, có 2 loại:
- **Trà sữa truyền thống**: 2 muỗng trà + 3 muỗng sữa
- **Trà sữa đặc biệt**: 4 muỗng trà + 1 muỗng sữa

Hôm nay bạn dùng hết **20 muỗng trà** và **15 muỗng sữa**. Hỏi: Bạn đã pha bao nhiêu ly mỗi loại?

Gọi $x$ = số ly truyền thống, $y$ = số ly đặc biệt:

$$\begin{cases} 2x + 4y = 20 \\ 3x + 1y = 15 \end{cases}$$

Viết dưới dạng ma trận:

$$\underbrace{\begin{pmatrix} 2 & 4 \\ 3 & 1 \end{pmatrix}}_{A} \cdot \underbrace{\begin{pmatrix} x \\ y \end{pmatrix}}_{\vec{x}} = \underbrace{\begin{pmatrix} 20 \\ 15 \end{pmatrix}}_{\vec{b}}$$

Giải: $\vec{x} = A^{-1} \cdot \vec{b}$

#### 🎯 Ví dụ kinh tế: Cân bằng cung-cầu

Thị trường có 2 sản phẩm phụ thuộc lẫn nhau (ví dụ: xăng và ô tô):

$$\begin{cases} Q_{d1} = 100 - 2P_1 + P_2 \\ Q_{d2} = 80 + P_1 - 3P_2 \end{cases}$$

Tại điểm cân bằng $Q_d = Q_s$ → Giải hệ phương trình tuyến tính để tìm giá cân bằng!

### 2C.2 Ba trường hợp nghiệm

| Trường hợp | $\det(A)$ | Ý nghĩa | Ví dụ đời thường |
|------------|-----------|---------|-------------------|
| **Nghiệm duy nhất** | $\neq 0$ | Hai đường thẳng cắt nhau | Cung gặp cầu → giá cân bằng |
| **Vô nghiệm** | $= 0$ (mâu thuẫn) | Hai đường song song | Yêu cầu mâu thuẫn → bất khả thi |
| **Vô số nghiệm** | $= 0$ (đồng nhất) | Hai đường trùng nhau | Thừa điều kiện → nhiều lựa chọn |

### 2C.3 Code Python: Giải hệ phương trình

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. GIẢI BÀI TOÁN QUÁN TRÀ SỮA
# =============================================

print("🧋 BÀI TOÁN QUÁN TRÀ SỮA")
print("=" * 50)

A = np.array([[2, 4], [3, 1]])
b = np.array([20, 15])

# Cách 1: Dùng nghịch đảo
x_inv = np.linalg.inv(A) @ b
print(f"   Cách 1 (A⁻¹·b): x = {x_inv}")

# Cách 2: Dùng np.linalg.solve (nhanh và ổn định hơn)
x_solve = np.linalg.solve(A, b)
print(f"   Cách 2 (solve):  x = {x_solve}")
print(f"   → Truyền thống: {x_solve[0]:.0f} ly, Đặc biệt: {x_solve[1]:.0f} ly")

# Kiểm tra
print(f"\n   Kiểm tra: 2×{x_solve[0]:.0f} + 4×{x_solve[1]:.0f} = {2*x_solve[0]+4*x_solve[1]:.0f} muỗng trà ✅")
print(f"            3×{x_solve[0]:.0f} + 1×{x_solve[1]:.0f} = {3*x_solve[0]+1*x_solve[1]:.0f} muỗng sữa ✅")

# =============================================
# 2. VÍ DỤ KINH TẾ: CÂN BẰNG CUNG CẦU
# =============================================

print(f"\n📈 CÂN BẰNG CUNG CẦU (2 sản phẩm)")
print("=" * 50)

# Qd1 = 100 - 2P1 + P2 = Qs1 = 10 + P1  → 3P1 - P2 = 90
# Qd2 = 80 + P1 - 3P2 = Qs2 = 5 + 2P2  → -P1 + 5P2 = 75

A_eco = np.array([[3, -1], [-1, 5]])
b_eco = np.array([90, 75])

gia_can_bang = np.linalg.solve(A_eco, b_eco)
print(f"   Giá cân bằng P1 = {gia_can_bang[0]:.1f}, P2 = {gia_can_bang[1]:.1f}")
print(f"   det(A) = {np.linalg.det(A_eco):.0f} ≠ 0 → Có nghiệm duy nhất ✅")

# =============================================
# 3. TRỰC QUAN HÓA: 3 TRƯỜNG HỢP NGHIỆM
# =============================================

fig, axes = plt.subplots(1, 3, figsize=(18, 5))
x_range = np.linspace(-2, 10, 300)

# Nghiệm duy nhất (2 đường cắt nhau)
ax1 = axes[0]
y1_a = (20 - 2*x_range) / 4  # 2x + 4y = 20
y1_b = 15 - 3*x_range        # 3x + y = 15
ax1.plot(x_range, y1_a, 'b-', linewidth=2, label='2x + 4y = 20 (Trà)')
ax1.plot(x_range, y1_b, 'r-', linewidth=2, label='3x + y = 15 (Sữa)')
ax1.plot(x_solve[0], x_solve[1], 'g★', markersize=20, label=f'Nghiệm ({x_solve[0]:.0f}, {x_solve[1]:.0f})')
ax1.set_title("✅ Nghiệm duy nhất\n(det ≠ 0)", fontsize=12, fontweight='bold', color='green')
ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)
ax1.set_xlim(-1, 8); ax1.set_ylim(-2, 8)

# Vô nghiệm (2 đường song song)
ax2 = axes[1]
y2_a = (10 - 2*x_range) / 4  # 2x + 4y = 10
y2_b = (20 - 2*x_range) / 4  # 2x + 4y = 20
ax2.plot(x_range, y2_a, 'b-', linewidth=2, label='2x + 4y = 10')
ax2.plot(x_range, y2_b, 'r-', linewidth=2, label='2x + 4y = 20')
ax2.set_title("❌ Vô nghiệm\n(Song song, det = 0)", fontsize=12, fontweight='bold', color='red')
ax2.legend(fontsize=9); ax2.grid(True, alpha=0.3)
ax2.set_xlim(-1, 8); ax2.set_ylim(-2, 8)

# Vô số nghiệm (2 đường trùng)
ax3 = axes[2]
y3_a = (10 - 2*x_range) / 4
ax3.plot(x_range, y3_a, 'b-', linewidth=4, alpha=0.5, label='2x + 4y = 10')
ax3.plot(x_range, y3_a, 'r--', linewidth=2, label='4x + 8y = 20 (cùng đường)')
ax3.set_title("♾️ Vô số nghiệm\n(Trùng nhau, det = 0)", fontsize=12, fontweight='bold', color='orange')
ax3.legend(fontsize=9); ax3.grid(True, alpha=0.3)
ax3.set_xlim(-1, 8); ax3.set_ylim(-2, 8)

for ax in axes:
    ax.set_xlabel('x'); ax.set_ylabel('y')

plt.suptitle("📐 Hệ phương trình tuyến tính — 3 trường hợp nghiệm", 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong1_he_phuong_trinh.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Sử dụng `np.linalg.solve()` thay vì tính nghịch đảo trong thực tế — nhanh hơn, ổn định hơn về mặt số học. Giải thích: "Trong cuộc sống, bạn không cần chế tạo máy Undo toàn bộ (nghịch đảo) chỉ để giải 1 bài (solve). Giải thẳng nhanh hơn!"

---

## PHẦN 3: KHÔNG GIAN VECTOR & CƠ SỞ

### 3.1 Không gian Vector — "Vũ trụ" mà vector sống

Hãy tưởng tượng: Tất cả những vector 2D mà bạn có thể vẽ trên mặt phẳng tạo thành một "vũ trụ" — đó là **không gian vector** $\mathbb{R}^2$.

- $\mathbb{R}^2$: mặt phẳng (2 chiều) — Bản đồ trên giấy
- $\mathbb{R}^3$: không gian 3 chiều — Thế giới thực
- $\mathbb{R}^n$: không gian n chiều — Nơi dữ liệu AI sống

> 💡 ChatGPT biểu diễn mỗi từ bằng một vector **768 chiều** hoặc hơn. Không ai "hình dung" được 768 chiều, nhưng toán học vẫn hoạt động hoàn hảo!

### 3.2 Cơ sở (Basis) — "Hệ trục tọa độ"

Trong $\mathbb{R}^2$, hai vector cơ bản là:

$$\vec{e_1} = (1, 0) \quad \text{và} \quad \vec{e_2} = (0, 1)$$

Mọi vector đều có thể được biểu diễn qua 2 "viên gạch" này:

$$\vec{v} = (3, 5) = 3 \cdot \vec{e_1} + 5 \cdot \vec{e_2}$$

**Liên tưởng: Bảng chữ cái**

- Cơ sở giống như **bảng chữ cái** trong tiếng Việt
- Từ 29 chữ cái (cơ sở), bạn có thể tạo ra **mọi** từ, mọi câu
- Tương tự, từ vài vector cơ sở, bạn có thể biểu diễn **mọi** vector trong không gian

### 3.3 Độc lập tuyến tính — Không "thừa"

Một tập vector là **độc lập tuyến tính** nếu không vector nào có thể "xây" từ các vector còn lại.

Ví dụ:
- $\vec{a} = (1, 0)$, $\vec{b} = (0, 1)$ → **Độc lập** ✅ (Không thể tạo $\vec{a}$ từ $\vec{b}$ hoặc ngược lại)
- $\vec{a} = (1, 2)$, $\vec{b} = (2, 4) = 2\vec{a}$ → **Phụ thuộc** ❌ ($\vec{b}$ chỉ là $\vec{a}$ phóng to gấp đôi, không thêm thông tin mới)

**Liên tưởng: Nguyên liệu nấu ăn**

Nếu trong bếp bạn có muối, đường, và nước mắm — 3 gia vị **độc lập** (mỗi thứ cho vị khác nhau). Nhưng nếu bạn thêm "muối biển" — nó cũng chỉ là muối → **phụ thuộc**, không thêm giá trị mới.

---

## PHẦN 3B: NORM VECTOR — "THƯỚC ĐO KHOẢNG CÁCH"

### 3B.1 Norm là gì?

**Norm** (chuẩn) = "Độ dài" của vector. Nó cho biết vector **lớn bao nhiêu**.

Nhưng "lớn" có nhiều cách đo, giống như đo khoảng cách từ nhà đến trường:
- Đi bộ theo vỉa hè (đường ziczac) → L1 Norm
- Bay thẳng đường chim bay → L2 Norm

### 3B.2 Ba loại Norm quan trọng

| Norm | Công thức | Ý nghĩa | Liên tưởng |
|------|-----------|---------|------------|
| **L1** (Manhattan) | $\|\vec{x}\|_1 = \sum_{i} |x_i|$ | Tổng trị tuyệt đối | 🚕 Đi taxi ở Manhattan (chỉ đi thẳng/rẽ) |
| **L2** (Euclidean) | $\|\vec{x}\|_2 = \sqrt{\sum_{i} x_i^2}$ | Đường chim bay | ✈️ Bay thẳng từ A đến B |
| **L∞** (Max) | $\|\vec{x}\|_\infty = \max_{i} |x_i|$ | Giá trị lớn nhất | 🎯 Chỉ quan tâm sai số lớn nhất |

#### 🎯 Ví dụ: Đo "khoảng cách" giữa 2 thành phố

Từ giao lộ (0,0) đến (3,4):
- L1: $|3| + |4| = 7$ km (đi theo lưới đường)
- L2: $\sqrt{3^2 + 4^2} = 5$ km (bay thẳng — định lý Pythagoras!)
- L∞: $\max(3, 4) = 4$ km (chiều dài nhất)

#### 🎯 Ví dụ tài chính: Quản lý danh mục đầu tư 💰

Bạn đầu tư vào 5 cổ phiếu với tỷ trọng $\vec{w} = [0.3, 0.25, 0.2, 0.15, 0.1]$:

- **L1 Norm** $\|\vec{w}\|_1 = 1.0$ → Tổng tỷ trọng = 100% (budget constraint)
- **L2 Norm** $\|\vec{w}\|_2 = 0.48$ → Đo mức "tập trung" của danh mục
- L2 nhỏ = phân tán đều (an toàn), L2 lớn = tập trung ít cổ phiếu (rủi ro)

### 3B.3 Tại sao Norm CỰC KỲ QUAN TRỌNG trong ML?

**Regularization** — "Thuốc chống overfitting":

Khi mô hình ML quá phức tạp, nó "học thuộc" dữ liệu training nhưng dự đoán tệ trên dữ liệu mới. Để ngăn chặn:

$$\mathcal{L}_{total} = \mathcal{L}_{data} + \lambda \cdot \|\vec{w}\|$$

| Kỹ thuật | Dùng Norm | Tác dụng | Liên tưởng |
|----------|-----------|----------|------------|
| **L1 (Lasso)** | $\|\vec{w}\|_1$ | Ép nhiều weight = 0 → "Tự chọn feature" | 🔪 Cắt bỏ nguyên liệu thừa |
| **L2 (Ridge)** | $\|\vec{w}\|_2^2$ | Ép tất cả weight nhỏ đều → "Cân bằng" | ⚖️ Chia đều tải trọng |
| **Elastic Net** | $\alpha L1 + (1-\alpha) L2$ | Kết hợp cả hai | 🎯 Linh hoạt |

> 💡 **Nhận ra chưa?** L1 giống nhà đầu tư quyết liệt: "Bỏ hẳn 3 cổ phiếu, chỉ giữ 2 tốt nhất". L2 giống nhà đầu tư thận trọng: "Giữ cả 5 nhưng giảm đều tỷ trọng".

### 3B.4 Code Python: Norm trong thực tế

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. CÁC LOẠI NORM
# =============================================

v = np.array([3, -4, 1, -2, 5])

l1 = np.linalg.norm(v, ord=1)      # L1: Tổng trị tuyệt đối
l2 = np.linalg.norm(v, ord=2)      # L2: Đường chim bay
linf = np.linalg.norm(v, ord=np.inf) # L∞: Giá trị max

print("📏 NORM CỦA VECTOR:")
print(f"   v = {v}")
print(f"   L1  = |3|+|-4|+|1|+|-2|+|5| = {l1:.1f} (taxi Manhattan)")
print(f"   L2  = √(9+16+1+4+25)       = {l2:.2f} (đường chim bay)")
print(f"   L∞  = max(|3|,|-4|,...,|5|) = {linf:.1f} (sai số lớn nhất)")

# =============================================
# 2. ỨNG DỤNG: REGULARIZATION TRONG ML
# =============================================

print(f"\n🧠 REGULARIZATION — ĐO 'ĐỘ PHỨC TẠP' CỦA MODEL:")

# Model A: Nhiều weight lớn (phức tạp, dễ overfit)
w_complex = np.array([10.5, -8.3, 7.2, -6.1, 5.4, 0.1, 0.0, -0.2])

# Model B: Weight nhỏ, đều (đơn giản, tổng quát tốt)
w_simple = np.array([1.2, -0.8, 1.1, -0.7, 0.9, 0.3, -0.5, 0.6])

for name, w in [("Phức tạp (overfit) ⚠️", w_complex), ("Đơn giản (tốt) ✅", w_simple)]:
    print(f"\n   Model {name}:")
    print(f"     L1 = {np.linalg.norm(w, 1):.1f} (tổng |weight|)")
    print(f"     L2 = {np.linalg.norm(w, 2):.2f} (√Σweight²)")

# =============================================
# 3. TRỰC QUAN HÓA: HÌNH DẠNG "QUẢ BÓNG" NORM
# =============================================

fig, axes = plt.subplots(1, 3, figsize=(16, 5))

theta = np.linspace(0, 2*np.pi, 1000)

# L1: Hình thoi (Diamond)
ax1 = axes[0]
l1_x = np.cos(theta)
l1_y = np.sin(theta)
# Tập điểm ||x||_1 = 1
t = np.linspace(0, 2*np.pi, 1000)
l1_x = np.sign(np.cos(t)) * (1 - np.abs(np.sin(t)))
l1_y = np.sign(np.sin(t)) * (1 - np.abs(np.cos(t)))
# Đơn giản: vẽ hình thoi
l1_x = [1, 0, -1, 0, 1]
l1_y = [0, 1, 0, -1, 0]
ax1.fill(l1_x, l1_y, alpha=0.3, color='#2196F3')
ax1.plot(l1_x, l1_y, '#2196F3', linewidth=2)
ax1.set_title("L1 Norm = 1\n(Hình thoi — Lasso)", fontsize=12, fontweight='bold')
ax1.set_aspect('equal'); ax1.grid(True, alpha=0.3)
ax1.set_xlim(-1.5, 1.5); ax1.set_ylim(-1.5, 1.5)
ax1.annotate("Đỉnh nhọn → weight = 0\n(Feature selection!)", 
             xy=(1, 0), xytext=(0.5, 0.8), fontsize=9,
             arrowprops=dict(arrowstyle='->', color='red'), color='red')

# L2: Hình tròn (Circle)
ax2 = axes[1]
circle_x = np.cos(theta)
circle_y = np.sin(theta)
ax2.fill(circle_x, circle_y, alpha=0.3, color='#4CAF50')
ax2.plot(circle_x, circle_y, '#4CAF50', linewidth=2)
ax2.set_title("L2 Norm = 1\n(Hình tròn — Ridge)", fontsize=12, fontweight='bold')
ax2.set_aspect('equal'); ax2.grid(True, alpha=0.3)
ax2.set_xlim(-1.5, 1.5); ax2.set_ylim(-1.5, 1.5)
ax2.annotate("Tròn đều → weight nhỏ đều\n(Smooth)", 
             xy=(0.707, 0.707), xytext=(-0.5, 1.1), fontsize=9,
             arrowprops=dict(arrowstyle='->', color='green'), color='green')

# L∞: Hình vuông (Square)
ax3 = axes[2]
square_x = [1, 1, -1, -1, 1]
square_y = [1, -1, -1, 1, 1]
ax3.fill(square_x, square_y, alpha=0.3, color='#FF5722')
ax3.plot(square_x, square_y, '#FF5722', linewidth=2)
ax3.set_title("L∞ Norm = 1\n(Hình vuông)", fontsize=12, fontweight='bold')
ax3.set_aspect('equal'); ax3.grid(True, alpha=0.3)
ax3.set_xlim(-1.5, 1.5); ax3.set_ylim(-1.5, 1.5)

plt.suptitle("📐 Hình dạng 'quả bóng đơn vị' theo từng Norm\n(Tập hợp tất cả vector có norm = 1)", 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong1_norm_vector.png', dpi=150, bbox_inches='tight')
plt.show()

print("\n💡 Trong ML: Regularization = tìm weight NẰM TRONG quả bóng norm")
print("   L1 (thoi): dễ chạm đỉnh nhọn → weight = 0 → sparse!")
print("   L2 (tròn): weight phân bố đều → smooth!")
```

> 📌 **Lưu ý cho giảng viên:** Trực quan hóa "quả bóng norm" giúp sinh viên hiểu tại sao L1 tạo sparse solution. Khi contour loss "chạm" hình thoi, nó thường chạm ở đỉnh nhọn (trục tọa độ) → 1 weight = 0.

---

## PHẦN 4: GIÁ TRỊ RIÊNG & VECTOR RIÊNG — "LINH HỒN" CỦA MA TRẬN

### 4.1 Ý tưởng trực quan

Chúng ta đã biết ma trận là "máy biến hình" — nó xoay, kéo giãn vector. Nhưng có những vector đặc biệt mà khi đi qua "máy", chúng **không đổi hướng** — chỉ bị kéo dài hoặc thu ngắn. Đó là **vector riêng**.

$$A \cdot \vec{v} = \lambda \cdot \vec{v}$$

- $\vec{v}$ là **vector riêng** (eigenvector): hướng không đổi
- $\lambda$ là **giá trị riêng** (eigenvalue): hệ số co giãn

#### 🎯 Liên tưởng: Dòng nước và cọng rơm

Hãy tưởng tượng bạn thả một đám cọng rơm xuống dòng suối có xoáy nước:
- Hầu hết cọng rơm bị cuốn, xoay tứ tung → vector thường
- Nhưng một vài cọng rơm nằm đúng **dòng chảy chính** — chúng chỉ di chuyển theo dòng mà không bị xoay → đó là **vector riêng**
- Tốc độ dòng chảy chính = **giá trị riêng**

### 4.2 Tại sao quan trọng trong CNTT?

| Ứng dụng | Vai trò của Eigenvalue/Eigenvector |
|-----------|-----------------------------------|
| **Google PageRank** | Vector riêng chỉ ra trang web quan trọng nhất |
| **PCA (Giảm chiều)** | Eigenvector = Hướng có nhiều thông tin nhất |
| **Nén ảnh** | Giữ lại eigenvalue lớn = giữ thông tin chính |
| **Phân tích rung động** | Eigenvalue = tần số rung tự nhiên |

### 4.3 Code Python: Eigenvalue trong thực tế

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. TÌM EIGENVALUE VÀ EIGENVECTOR
# =============================================

# Ma trận biểu diễn phép biến đổi
A = np.array([
    [3, 1],
    [0, 2]
])

eigenvalues, eigenvectors = np.linalg.eig(A)

print("🔑 Ma trận A:")
print(A)
print(f"\n📐 Eigenvalues (giá trị riêng):  {eigenvalues}")
print(f"📏 Eigenvectors (vector riêng):")
for i, (val, vec) in enumerate(zip(eigenvalues, eigenvectors.T)):
    print(f"   λ{i+1} = {val:.1f}  →  v{i+1} = {vec}")

# Kiểm tra: A @ v = λ * v
print("\n✅ Kiểm tra A·v = λ·v:")
for val, vec in zip(eigenvalues, eigenvectors.T):
    Av = A @ vec
    lv = val * vec
    print(f"   A·v = {Av},  λ·v = {lv},  Bằng nhau? {np.allclose(Av, lv)}")

# =============================================
# 2. TRỰC QUAN HÓA: Eigenvector không đổi hướng
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Tạo nhiều vector ngẫu nhiên
np.random.seed(42)
vectors = np.random.randn(2, 20)  # 20 vector ngẫu nhiên

# Biến đổi
vectors_transformed = A @ vectors

# Hình 1: Trước biến đổi
ax1 = axes[0]
ax1.set_title("Trước biến đổi (Vector gốc)", fontsize=13, fontweight='bold')
for i in range(vectors.shape[1]):
    ax1.quiver(0, 0, vectors[0, i], vectors[1, i], angles='xy', 
               scale_units='xy', scale=1, color='#90CAF9', alpha=0.6, width=0.005)

# Vẽ eigenvector
for val, vec, color in zip(eigenvalues, eigenvectors.T, ['#f44336', '#4CAF50']):
    ax1.quiver(0, 0, vec[0], vec[1], angles='xy', scale_units='xy', scale=1, 
               color=color, width=0.012, label=f'Eigenvector (λ={val:.0f})')

ax1.set_xlim(-3, 3); ax1.set_ylim(-3, 3)
ax1.set_aspect('equal'); ax1.grid(True, alpha=0.3); ax1.legend(fontsize=9)

# Hình 2: Sau biến đổi
ax2 = axes[1]
ax2.set_title("Sau biến đổi A·v (Vector bị xoay & kéo)", fontsize=13, fontweight='bold')
for i in range(vectors_transformed.shape[1]):
    ax2.quiver(0, 0, vectors_transformed[0, i], vectors_transformed[1, i], 
               angles='xy', scale_units='xy', scale=1, color='#90CAF9', 
               alpha=0.6, width=0.005)

# Eigenvector sau biến đổi (cùng hướng, chỉ dài hơn!)
for val, vec, color in zip(eigenvalues, eigenvectors.T, ['#f44336', '#4CAF50']):
    transformed = A @ vec
    ax2.quiver(0, 0, transformed[0], transformed[1], angles='xy', 
               scale_units='xy', scale=1, color=color, width=0.012,
               label=f'A·v (λ={val:.0f}, cùng hướng!)')

ax2.set_xlim(-6, 6); ax2.set_ylim(-6, 6)
ax2.set_aspect('equal'); ax2.grid(True, alpha=0.3); ax2.legend(fontsize=9)

plt.suptitle("🧲 Eigenvector: Vector duy nhất KHÔNG bị xoay bởi ma trận", 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong1_eigenvector.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## PHẦN 4B: PCA — GIẢM CHIỀU DỮ LIỆU (ỨNG DỤNG #1 CỦA EIGENVECTOR)

### 4B.1 Vấn đề: "Lời nguyền đa chiều"

Dữ liệu thực tế có hàng trăm, hàng nghìn chiều (features). Ví dụ:
- Ảnh 100×100 = **10,000 chiều**
- Hồ sơ khách hàng ngân hàng = **50+ thuộc tính** (tuổi, thu nhập, lịch sử, ...)
- Gene expression = **20,000+ gen**

Nhưng nhiều chiều lại **phụ thuộc nhau** (chiều cao ↔ cân nặng, thu nhập ↔ chi tiêu). PCA tìm ra những **hướng quan trọng nhất** và bỏ phần "thừa".

### 4B.2 Ý tưởng trực quan

**PCA (Principal Component Analysis)** = Tìm **hướng có nhiều thông tin nhất** → Chiếu dữ liệu lên hướng đó.

#### 🎯 Liên tưởng: Chụp ảnh bóng

Bạn có đám mây 3D các điểm dữ liệu. Bạn muốn chụp "ảnh" 2D của nó. PCA tìm **góc chụp tốt nhất** — góc mà ảnh bóng **phân tán nhất** (giữ nhiều thông tin nhất).

- **PC1** = Hướng có phương sai lớn nhất (ảnh bóng rộng nhất)
- **PC2** = Hướng vuông góc PC1, phương sai lớn thứ hai
- ...

#### 🎯 Ví dụ tài chính: Phân tích rủi ro thị trường 📈

Bạn theo dõi 100 cổ phiếu mỗi ngày. Nhưng hầu hết chúng **cùng tăng/giảm** theo thị trường chung:

- **PC1** ≈ "Xu hướng thị trường chung" (giải thích 60% biến động)
- **PC2** ≈ "Ngành công nghệ vs Ngành tiêu dùng" (giải thích 15%)
- **PC3** ≈ "Cổ phiếu lớn vs nhỏ" (giải thích 8%)

→ Chỉ 3 yếu tố giải thích 83% biến động của 100 cổ phiếu! Giảm từ 100 chiều → 3 chiều.

### 4B.3 Thuật toán PCA — Từng bước

1. **Chuẩn hóa dữ liệu** (trung bình = 0)
2. **Tính ma trận Covariance** $C = \frac{1}{n-1} X^T X$
3. **Tìm Eigenvector** của $C$ → Đó là các **Principal Components**
4. **Sắp xếp** theo eigenvalue giảm dần
5. **Chọn k hướng** → Chiếu dữ liệu xuống k chiều

> 💡 **Covariance** đo mức độ hai biến "đi cùng nhau". $Cov(X,Y) > 0$ → X tăng thì Y tăng (chiều cao-cân nặng). $Cov(X,Y) < 0$ → X tăng thì Y giảm (giá dầu-cổ phiếu hàng không).

### 4B.4 Code Python: PCA trực quan

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# PCA — GIẢM CHIỀU DỮ LIỆU
# =============================================

np.random.seed(42)

# Tạo dữ liệu 2D có tương quan (mô phỏng chiều cao vs cân nặng)
n = 200
chieu_cao = np.random.normal(170, 8, n)  # cm
can_nang = 0.6 * chieu_cao + np.random.normal(0, 5, n)  # Tương quan với chiều cao

X = np.column_stack([chieu_cao, can_nang])

# Bước 1: Chuẩn hóa (trung bình = 0)
X_centered = X - X.mean(axis=0)

# Bước 2: Ma trận Covariance
C = np.cov(X_centered.T)
print("📊 MA TRẬN COVARIANCE:")
print(f"   Var(chiều cao) = {C[0,0]:.1f}")
print(f"   Var(cân nặng)  = {C[1,1]:.1f}")
print(f"   Cov(cao, nặng) = {C[0,1]:.1f} {'(dương → cùng tăng!)' if C[0,1] > 0 else ''}")

# Bước 3: Eigenvalue & Eigenvector
eigenvalues, eigenvectors = np.linalg.eig(C)

# Sắp xếp giảm dần
idx = eigenvalues.argsort()[::-1]
eigenvalues = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]

print(f"\n🔑 PRINCIPAL COMPONENTS:")
print(f"   PC1: eigenvalue = {eigenvalues[0]:.1f} ({eigenvalues[0]/eigenvalues.sum()*100:.1f}% thông tin)")
print(f"   PC2: eigenvalue = {eigenvalues[1]:.1f} ({eigenvalues[1]/eigenvalues.sum()*100:.1f}% thông tin)")
print(f"   → PC1 giữ {eigenvalues[0]/eigenvalues.sum()*100:.0f}% thông tin! Chỉ 1 chiều thay vì 2.")

# Bước 4: Chiếu xuống 1D (chỉ giữ PC1)
X_1d = X_centered @ eigenvectors[:, 0:1]  # Chiếu lên PC1

# =============================================
# TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 1. Dữ liệu gốc + PC directions
ax1 = axes[0]
ax1.scatter(X_centered[:, 0], X_centered[:, 1], alpha=0.4, color='#2196F3', s=20)

# Vẽ hướng PC
mean = X_centered.mean(axis=0)
for i, (val, vec, color, name) in enumerate(zip(
    eigenvalues, eigenvectors.T, ['#f44336', '#4CAF50'], ['PC1', 'PC2'])):
    scale = np.sqrt(val) * 2  # Scale theo eigenvalue
    ax1.annotate('', xy=mean + scale*vec, xytext=mean,
                arrowprops=dict(arrowstyle='->', color=color, lw=3))
    ax1.annotate(f'{name} ({val/eigenvalues.sum()*100:.0f}%)', 
                xy=mean + scale*vec, fontsize=10, color=color, fontweight='bold')

ax1.set_xlabel('Chiều cao (centered)', fontsize=11)
ax1.set_ylabel('Cân nặng (centered)', fontsize=11)
ax1.set_title('📊 Dữ liệu gốc 2D\n+ Hướng Principal Components', fontsize=12, fontweight='bold')
ax1.set_aspect('equal'); ax1.grid(True, alpha=0.3)

# 2. Chiếu xuống PC1 (1D)
ax2 = axes[1]
ax2.scatter(X_1d, np.zeros_like(X_1d), alpha=0.4, color='#f44336', s=20)
ax2.set_xlabel('PC1', fontsize=11)
ax2.set_title(f'📉 Sau PCA: 2D → 1D\n(Giữ {eigenvalues[0]/eigenvalues.sum()*100:.0f}% thông tin)', 
              fontsize=12, fontweight='bold')
ax2.set_yticks([]); ax2.grid(True, alpha=0.3)

# 3. Explained variance
ax3 = axes[2]
explained = eigenvalues / eigenvalues.sum() * 100
cumulative = np.cumsum(explained)
bars = ax3.bar(['PC1', 'PC2'], explained, color=['#f44336', '#4CAF50'], 
               edgecolor='white', alpha=0.7, label='Từng PC')
ax3.plot(['PC1', 'PC2'], cumulative, 'ko-', linewidth=2, markersize=8, label='Tích lũy')
for bar, val in zip(bars, explained):
    ax3.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1,
             f'{val:.1f}%', ha='center', fontsize=11, fontweight='bold')
ax3.set_ylabel('% Thông tin giữ lại', fontsize=11)
ax3.set_title('📊 Explained Variance\n(PC1 đã giữ gần hết thông tin)', fontsize=12, fontweight='bold')
ax3.legend(fontsize=10); ax3.grid(axis='y', alpha=0.3)
ax3.set_ylim(0, 110)

plt.suptitle('🔬 PCA: Giảm chiều dữ liệu — Tìm hướng quantrọng nhất', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong1_pca.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** PCA là "ngã tư" nơi ĐSTT (eigenvector) gặp XSTK (covariance). Hãy nhấn mạnh: "Eigenvector của ma trận covariance = hướng mà dữ liệu phân tán nhất = hướng chứa nhiều thông tin nhất". Đây là aha moment lớn!

---

## PHẦN 5: ỨNG DỤNG THỰC TẾ — NÉN ẢNH BẰNG SVD

### 5.1 SVD là gì?

**SVD (Singular Value Decomposition)** — Phân rã Giá trị Kì dị — là cách "tháo rời" bất kỳ ma trận nào thành 3 phần đơn giản hơn:

$$A = U \cdot \Sigma \cdot V^T$$

- $U$ : Ma trận các hướng quan trọng theo **hàng** (các pattern theo chiều dọc)
- $\Sigma$ : Ma trận đường chéo chứa **giá trị kì dị** (σ₁ ≥ σ₂ ≥ ... ≥ 0) — đo mức độ quan trọng
- $V^T$ : Ma trận các hướng quan trọng theo **cột** (các pattern theo chiều ngang)

#### 🎯 Liên tưởng: Công thức nấu phở

Tưởng tượng bạn có một bát phở hoàn hảo (ma trận A). SVD giống như phân tích bát phở thành:
- $U$ = Các loại **nguyên liệu** (thịt, rau, bánh phở)
- $\Sigma$ = **Lượng** từng nguyên liệu (thịt nhiều nhất, hành ít hơn...)
- $V^T$ = **Cách chế biến** (luộc, hầm, xào)

Nếu bạn chỉ giữ lại vài nguyên liệu quan trọng nhất (σ lớn nhất), bạn vẫn có bát phở gần giống bản gốc.

→ Đó chính là nguyên lý **nén ảnh**: Giữ lại k giá trị kì dị lớn nhất, bỏ phần còn lại.

### 5.2 Vì sao nén được?

Ảnh gốc 1000×1000 pixel = 1,000,000 số cần lưu.

Nếu dùng SVD với k=50 thành phần:
- $U$ : 1000 × 50 = 50,000 số
- $\Sigma$ : 50 số
- $V^T$ : 50 × 1000 = 50,000 số
- **Tổng: 100,050 số** → Nén được **~90%!** mà ảnh gần như không thay đổi.

### 5.3 🎓 MINI PROJECT: Nén ảnh bằng SVD

```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import urllib.request
import os

# =============================================
# 1. TẠO ẢNH MẪU (nếu không có ảnh sẵn)
# =============================================

def tao_anh_mau(kich_thuoc=256):
    """Tạo ảnh mẫu với gradient và hình dạng để demo nén"""
    img = np.zeros((kich_thuoc, kich_thuoc))
    
    # Gradient nền
    for i in range(kich_thuoc):
        for j in range(kich_thuoc):
            img[i, j] = (i + j) / (2 * kich_thuoc) * 255
    
    # Vẽ hình tròn
    center = kich_thuoc // 2
    for i in range(kich_thuoc):
        for j in range(kich_thuoc):
            if (i - center)**2 + (j - center)**2 < (kich_thuoc // 4)**2:
                img[i, j] = 255
    
    # Vẽ đường chéo
    for i in range(kich_thuoc):
        if 0 <= i < kich_thuoc:
            img[i, max(0, i-2):min(kich_thuoc, i+3)] = 0
    
    return img

# =============================================
# 2. NÉN ẢNH BẰNG SVD
# =============================================

def nen_anh_svd(anh, k):
    """
    Nén ảnh bằng SVD: chỉ giữ lại k giá trị kì dị lớn nhất.
    
    Tham số:
        anh: Ma trận ảnh (2D numpy array)
        k: Số thành phần giữ lại (rank xấp xỉ)
    
    Trả về:
        anh_nen: Ảnh đã nén (2D numpy array)
        ti_le_nen: Tỉ lệ nén (%)
    """
    # Bước 1: Phân rã SVD
    U, sigma, Vt = np.linalg.svd(anh, full_matrices=False)
    
    # Bước 2: Giữ lại k thành phần đầu tiên
    U_k = U[:, :k]         # m × k
    sigma_k = sigma[:k]     # k
    Vt_k = Vt[:k, :]        # k × n
    
    # Bước 3: Tái tạo ảnh
    anh_nen = U_k @ np.diag(sigma_k) @ Vt_k
    
    # Tính tỉ lệ nén
    m, n = anh.shape
    so_goc = m * n
    so_nen = k * (m + n + 1)
    ti_le_nen = (1 - so_nen / so_goc) * 100
    
    return np.clip(anh_nen, 0, 255), ti_le_nen

# =============================================
# 3. DEMO TRỰC QUAN
# =============================================

# Tạo ảnh mẫu
anh_goc = tao_anh_mau(256)

# Nén với các mức k khác nhau
cac_muc_k = [1, 5, 10, 20, 50, 100]

fig, axes = plt.subplots(2, 4, figsize=(20, 10))
axes = axes.flatten()

# Ảnh gốc
axes[0].imshow(anh_goc, cmap='gray')
axes[0].set_title(f"🖼️ Ảnh gốc\n{anh_goc.shape[0]}×{anh_goc.shape[1]} = {anh_goc.size:,} pixels", 
                  fontsize=11, fontweight='bold')
axes[0].axis('off')

# Các mức nén
for i, k in enumerate(cac_muc_k):
    anh_nen, ti_le = nen_anh_svd(anh_goc, k)
    ax = axes[i + 1]
    ax.imshow(anh_nen, cmap='gray')
    
    # Tính sai số
    mse = np.mean((anh_goc - anh_nen) ** 2)
    
    ax.set_title(f"k = {k}\nNén: {ti_le:.0f}% | MSE: {mse:.1f}", 
                 fontsize=11, fontweight='bold')
    ax.axis('off')

# Biểu đồ giá trị kì dị
ax_last = axes[-1]
U, sigma, Vt = np.linalg.svd(anh_goc, full_matrices=False)
ax_last.bar(range(50), sigma[:50], color='#2196F3', alpha=0.7)
ax_last.set_title("📊 50 Giá trị kì dị đầu tiên\n(Giảm rất nhanh!)", 
                  fontsize=11, fontweight='bold')
ax_last.set_xlabel("Thứ tự (k)")
ax_last.set_ylabel("Giá trị σ")
ax_last.grid(True, alpha=0.3)

plt.suptitle("🎨 NÉN ẢNH BẰNG SVD — Giữ ít thành phần, vẫn thấy được ảnh!", 
             fontsize=15, fontweight='bold', y=1.02)
plt.tight_layout()
plt.savefig('chuong1_nen_anh_svd.png', dpi=150, bbox_inches='tight')
plt.show()

# =============================================
# 4. IN BÁO CÁO
# =============================================

print("\n" + "="*60)
print("📊 BÁO CÁO NÉN ẢNH SVD")
print("="*60)
print(f"Kích thước ảnh gốc: {anh_goc.shape[0]}×{anh_goc.shape[1]}")
print(f"Tổng số pixel: {anh_goc.size:,}")
print("-"*60)
print(f"{'k':>5} | {'Nén (%)':>10} | {'MSE':>10} | {'Chất lượng':>15}")
print("-"*60)

for k in cac_muc_k:
    anh_nen, ti_le = nen_anh_svd(anh_goc, k)
    mse = np.mean((anh_goc - anh_nen) ** 2)
    
    if mse < 10:
        chat_luong = "⭐⭐⭐ Tuyệt vời"
    elif mse < 100:
        chat_luong = "⭐⭐ Tốt"
    elif mse < 500:
        chat_luong = "⭐ Chấp nhận"
    else:
        chat_luong = "❌ Kém"
    
    print(f"{k:>5} | {ti_le:>9.1f}% | {mse:>10.1f} | {chat_luong}")

print("-"*60)
print("💡 Nhận xét: Chỉ cần k=20-50 đã giữ được hầu hết thông tin!")
print("   Đây là nguyên lý nền tảng của nén dữ liệu trong CNTT.")
```

---

## PHẦN 6: TÓM TẮT CHƯƠNG & BẢN ĐỒ TƯ DUY

### 🗺️ Bản đồ kiến thức Chương 1

```
ĐẠI SỐ TUYẾN TÍNH — NGÔN NGỮ CỦA KHÔNG GIAN
├── VECTOR (Mũi tên định hướng)
│   ├── Cộng vector → Ghép hành trình
│   ├── Nhân vô hướng → Phóng to/thu nhỏ
│   └── Tích vô hướng → Đo sự giống nhau 🎯
│       └── Ứng dụng: Recommendation System, Doanh thu
│
├── MA TRẬN (Máy biến hình)
│   ├── Phép nhân → Biến đổi dữ liệu
│   ├── Chuyển vị → Lật bảng
│   └── Ứng dụng: Filter ảnh, Bảng điểm
│
├── ĐỊNH THỨC & NGHỊCH ĐẢO 🆕
│   ├── Định thức → Hệ số co giãn diện tích
│   ├── det(A) = 0 → Suy biến → Mất thông tin!
│   ├── Ma trận nghịch đảo → "Nút Undo"
│   └── Ứng dụng: Mã hóa, Cân bằng danh mục đầu tư
│
├── HỆ PHƯƠNG TRÌNH TUYẾN TÍNH 🆕
│   ├── Ax = b → Giải bằng nghịch đảo hoặc solve
│   ├── 3 trường hợp: Duy nhất / Vô nghiệm / Vô số
│   └── Ứng dụng: Cung-cầu, Bài toán nguyên liệu
│
├── KHÔNG GIAN VECTOR
│   ├── Cơ sở → "Bảng chữ cái" của không gian
│   └── Độc lập tuyến tính → Không thừa thông tin
│
├── NORM VECTOR 🆕
│   ├── L1 (Manhattan) → Tổng trị tuyệt đối
│   ├── L2 (Euclidean) → Đường chim bay
│   └── Regularization: L1 = Lasso (sparse), L2 = Ridge (smooth)
│       └── Ứng dụng: Chống overfitting, Quản lý danh mục đầu tư
│
├── EIGENVALUE & EIGENVECTOR (Linh hồn ma trận)
│   ├── Vector riêng → Hướng không đổi
│   ├── Giá trị riêng → Mức co giãn
│   └── Ứng dụng: Google PageRank, PCA
│
├── PCA (Giảm chiều dữ liệu) 🆕
│   ├── Covariance matrix → Eigenvector = hướng quan trọng nhất
│   ├── Giảm n chiều → k chiều (giữ phần lớn thông tin)
│   └── Ứng dụng: Phân tích rủi ro thị trường, Nén đặc trưng
│
└── SVD (Phân rã giá trị kì dị)
    ├── A = U · Σ · Vᵀ
    └── Ứng dụng: NÉN ẢNH 🎨
        └── Mini Project: Nén ảnh bằng Python
```

### ✅ Checklist kiến thức

Sau chương này, bạn cần:

- [ ] Hiểu vector là gì và tại sao nó không chỉ là "dãy số"
- [ ] Tính được tích vô hướng và giải thích ý nghĩa
- [ ] Hiểu ma trận là "phép biến đổi", không chỉ là "bảng số"
- [ ] Nhân được hai ma trận và hiểu ý nghĩa
- [ ] 🆕 Tính định thức và giải thích ý nghĩa hình học (co giãn diện tích)
- [ ] 🆕 Tìm ma trận nghịch đảo và hiểu khi nào KHÔNG tồn tại
- [ ] 🆕 Giải hệ phương trình tuyến tính bằng `np.linalg.solve`
- [ ] 🆕 Tính L1, L2 Norm và giải thích vai trò trong Regularization
- [ ] Giải thích eigenvalue/eigenvector bằng ngôn ngữ đời thường
- [ ] 🆕 Thực hiện PCA từ đầu: Covariance → Eigen → Chiếu → Giảm chiều
- [ ] Hiểu SVD ở mức trực quan và chạy được code nén ảnh
- [ ] Viết code Python với NumPy cho mọi phép toán trên

---

## 📝 BÀI TẬP TỰ LUYỆN

### Bài 1: Recommendation nâng cao ⭐
Mở rộng hệ thống gợi ý phim ở Phần 1:
- Thêm 5 thể loại phim: [Hành_động, Tình_cảm, Kinh_dị, Hài, Tài_liệu]
- Thêm 10 bộ phim với vector thể loại tương ứng
- Viết hàm `goi_y_phim(so_thich, danh_sach_phim, top_n=3)` trả về top-n phim phù hợp nhất

### Bài 2: Biến đổi hình học ⭐⭐
- Viết ma trận biến đổi để: (a) Phóng to 3 lần, (b) Lật theo trục x, (c) Xoay 60°
- Áp dụng lần lượt lên một hình tam giác
- Trực quan hóa kết quả bằng Matplotlib

### Bài 3: Nén ảnh thực tế ⭐⭐⭐
- Tải một bức ảnh thật (ảnh xám, 500×500+)
- Áp dụng SVD để nén với các mức k = 5, 10, 20, 50, 100, 200
- Vẽ biểu đồ: Trục x = k, Trục y = MSE (sai số)
- Tìm giá trị k "tối ưu" (ít thành phần nhất mà sai số chấp nhận được)

### Bài 4: Google PageRank mini ⭐⭐⭐
Cho 4 trang web A, B, C, D liên kết với nhau:
- A → B, C
- B → C
- C → A
- D → A, B, C

Xây dựng ma trận chuyển tiếp, tìm eigenvector ứng với eigenvalue = 1. Đó chính là "điểm quan trọng" của mỗi trang!

### Bài 5: 🆕 Mã hóa/Giải mã tin nhắn ⭐⭐
Mã hóa chuỗi "HELLO" (H=8, E=5, L=12, O=15) bằng ma trận 2×2. Ghép thành các cặp vector [8,5], [12,12], [15,0]. Nhân với ma trận khóa. Giải mã bằng nghịch đảo. Thử dùng ma trận có det=0 → Giải thích tại sao thất bại.

### Bài 6: 🆕 Phân tích danh mục đầu tư bằng PCA ⭐⭐⭐
Tạo dữ liệu giả lập lợi suất hàng ngày của 10 cổ phiếu (252 ngày). Chạy PCA:
- Bao nhiêu PC giải thích 90% biến động?
- PC1 đại diện cho "yếu tố" gì? (gợi ý: thị trường chung)
- Trực quan hóa dữ liệu trên PC1 vs PC2

### Bài 7: 🆕 So sánh L1 vs L2 Regularization ⭐⭐⭐
Cho hàm loss $L(w) = \sum(y_i - w \cdot x_i)^2$. Thêm penalty L1 và L2 với $\lambda = 0.1, 1, 10$. Tìm weight tối ưu cho mỗi trường hợp. Trực quan hóa: L1 tạo sparse weight (nhiều = 0), L2 tạo smooth weight (đều nhỏ).

---

## 📌 LƯU Ý CHO GIẢNG VIÊN

### Chiến lược khơi gợi sự tò mò

1. **Mở đầu bằng demo, không phải định nghĩa:**
   - Chiếu code nén ảnh SVD trước → sinh viên thấy kết quả → rồi mới giải thích "tại sao"
   - Hỏi: "Làm sao nén được 90% mà ảnh vẫn đẹp?" → Tạo tò mò

2. **Dùng "Khoảnh khắc Aha":**
   - Cho sinh viên thay đổi vector sở thích Netflix → nhận ra kết quả gợi ý thay đổi
   - Cho thay đổi góc xoay ma trận → "cảm" được phép biến đổi

3. **Liên hệ thực tế liên tục:**
   - "Các em có biết TikTok dùng gì để gợi ý video?" (→ Dot product trên vector đặc trưng)
   - "Tại sao gửi ảnh qua Zalo bị mờ?" (→ Nén ảnh, giảm rank)

4. **Sai lầm phổ biến cần chú ý:**
   - Nhầm tích vô hướng (dot product) với cross product
   - Nghĩ phép nhân ma trận như nhân từng phần tử
   - Quên rằng thứ tự nhân ma trận quan trọng (A×B ≠ B×A)

5. **Hoạt động nhóm gợi ý:**
   - Nhóm 2 người: Mỗi người chọn vector sở thích → so sánh dot product → tìm xem 2 người có "hợp nhau" không
   - Cả lớp: Xây dựng ma trận khoảng cách giữa tất cả sinh viên, rồi dùng đồ thị trực quan hóa

---

## PHẦN 4C: TENSOR — "MA TRẬN NHIỀU CHIỀU"

### 4C.1 Từ Scalar đến Tensor

Bạn đã biết:
- **Scalar** (0D): Một con số → `5`
- **Vector** (1D): Dãy số → `[1, 2, 3]`
- **Ma trận** (2D): Bảng số → `[[1,2],[3,4]]`

**Tensor** = Mở rộng lên **nhiều chiều hơn**:

```
Scalar (0D)     Vector (1D)      Ma trận (2D)       Tensor 3D          Tensor 4D
    5           [1, 2, 3]       [[1, 2],           [[[1,2],            Batch ảnh
                                 [3, 4]]            [3,4]],           (32 ảnh RGB
                                                    [[5,6],            224×224)
                                                     [7,8]]]
```

#### 🎯 Ví dụ thực tế: Dữ liệu AI luôn là Tensor!

| Dữ liệu | Kích thước | Ý nghĩa mỗi chiều |
|----------|-----------|-------------------|
| Ảnh xám | (H, W) = (224, 224) | Cao × Rộng |
| Ảnh RGB | (H, W, 3) = (224, 224, 3) | Cao × Rộng × Kênh_màu |
| Batch ảnh | (B, H, W, 3) = (32, 224, 224, 3) | Batch × Cao × Rộng × Kênh |
| Video | (B, T, H, W, 3) | + Thời gian |
| Câu văn | (B, L, D) = (16, 128, 768) | Batch × Tokens × Embedding |
| Batch weights | (out, in) = (512, 256) | Output × Input neurons |

### 4C.2 Code Python: Tensor operations

```python
import numpy as np

# =============================================
# TENSOR — Ma trận nhiều chiều
# =============================================

# Scalar (0D)
scalar = np.array(42)
print(f"Scalar: shape = {scalar.shape}, ndim = {scalar.ndim}")  # (), 0

# Vector (1D)  
vector = np.array([1, 2, 3])
print(f"Vector: shape = {vector.shape}, ndim = {vector.ndim}")  # (3,), 1

# Ma trận (2D)
matrix = np.array([[1, 2], [3, 4]])
print(f"Matrix: shape = {matrix.shape}, ndim = {matrix.ndim}")  # (2,2), 2

# Tensor 3D — Ảnh RGB
image = np.random.randint(0, 255, (224, 224, 3))
print(f"Ảnh RGB: shape = {image.shape}, ndim = {image.ndim}")  # (224,224,3), 3

# Tensor 4D — Batch ảnh (input cho CNN)
batch_images = np.random.randint(0, 255, (32, 224, 224, 3))
print(f"Batch 32 ảnh: shape = {batch_images.shape}, ndim = {batch_images.ndim}")  # (32,224,224,3), 4

# Tensor 3D — Câu văn (input cho Transformer)
batch_text = np.random.randn(16, 128, 768)  # 16 câu, 128 tokens, 768-dim embedding
print(f"Batch text: shape = {batch_text.shape}, ndim = {batch_text.ndim}")  # (16,128,768), 3

print("\n💡 Trong PyTorch:")
print("   tensor = torch.randn(32, 224, 224, 3)  # Tạo batch ảnh")
print("   tensor = tensor.permute(0, 3, 1, 2)     # → (32, 3, 224, 224) cho PyTorch CNN")
print("   # PyTorch dùng format (B, C, H, W) — Channel trước!")
```

> 💡 **Key insight:** Mọi dữ liệu trong deep learning đều là Tensor. Khi bạn nói "training 1 batch" = bạn đang nhân ma trận lên 1 tensor 4D. Tất cả phép toán ĐSTT (nhân, chuyển vị, norm...) đều áp dụng cho tensor!

---

## ❌ SAI LẦM PHỔ BIẾN — ĐỪNG MẮC BẪY!

> Các lỗi dưới đây cực kỳ phổ biến. Nếu bạn đọc và nghĩ "Ơ, mình cũng nghĩ vậy!" — thì bạn vừa tránh được 1 bẫy!

| # | ❌ Sai lầm | ✅ Sự thật |
|---|-----------|-----------|
| 1 | "Nhân 2 ma trận = nhân từng phần tử" | Nhân ma trận = tổ hợp hàng × cột. Nhân từng phần tử là `A * B` (element-wise), KHÁC `A @ B` |
| 2 | "A×B = B×A (giao hoán)" | Nhân ma trận KHÔNG giao hoán! A×B ≠ B×A (trừ trường hợp đặc biệt) |
| 3 | "Ma trận khác kích thước thì không nhân được" | (3×4) × (4×2) = (3×2). Chỉ cần SỐ CỘT A = SỐ HÀNG B |
| 4 | "Eigenvalue luôn là số thực" | Eigenvalue CÓ THỂ là số phức! (Nhưng ma trận đối xứng → luôn thực) |
| 5 | "det(A) lớn = ma trận quan trọng" | det chỉ cho biết co giãn diện tích. det lớn hay nhỏ KHÔNG liên quan đến "quan trọng" |
| 6 | "PCA bỏ feature" | PCA tạo feature MỚI (principal components) = tổ hợp tuyến tính của feature cũ. Không bỏ cột nào! |
| 7 | "L1 và L2 norm giống nhau, chỉ khác công thức" | L1 tạo sparse weights (nhiều = 0), L2 tạo smooth weights (đều nhỏ). Hậu quả RẤT KHÁC! |

---

## 🔗 CẦU NỐI: TỪ NUMPY ĐẾN PYTORCH

Bạn đã thành thạo NumPy. Trong thực tế AI, bạn sẽ dùng **PyTorch**. Tin vui: Gần như giống nhau!

| Phép toán | NumPy (đã học) | PyTorch (thực tế) | Ghi chú |
|-----------|----------------|--------------------|---------|
| Tạo vector | `np.array([1,2,3])` | `torch.tensor([1,2,3])` | |
| Tạo ma trận zero | `np.zeros((3,4))` | `torch.zeros(3,4)` | |
| Ma trận random | `np.random.randn(3,4)` | `torch.randn(3,4)` | |
| Nhân ma trận | `A @ B` | `A @ B` hoặc `torch.mm(A,B)` | Giống! |
| Tích vô hướng | `np.dot(a, b)` | `torch.dot(a, b)` | Giống! |
| Chuyển vị | `A.T` | `A.T` hoặc `A.mT` | Giống! |
| Norm | `np.linalg.norm(v)` | `torch.norm(v)` | |
| Eigenvalue | `np.linalg.eig(A)` | `torch.linalg.eig(A)` | |
| SVD | `np.linalg.svd(A)` | `torch.linalg.svd(A)` | |
| Nghịch đảo | `np.linalg.inv(A)` | `torch.linalg.inv(A)` | |
| Chuyển NP↔PT | — | `torch.from_numpy(arr)` | |
| **Gradient** | ❌ Tính tay | ✅ `loss.backward()` | Tự động! |
| **GPU** | ❌ CPU only | ✅ `tensor.cuda()` | Nhanh 100x |

```python
import torch

# Mọi thứ bạn học đều dùng được ngay!
A = torch.tensor([[1., 2.], [3., 4.]])
v = torch.tensor([1., 0.])

# Tất cả giống NumPy:
print(A @ v)           # Nhân ma trận-vector
print(torch.det(A))    # Định thức
print(torch.linalg.eig(A))  # Eigenvalue

# Nhưng PyTorch có thêm: TỰ ĐỘNG TÍNH GRADIENT!
w = torch.tensor([1., 2.], requires_grad=True)
loss = (w**2).sum()    # f(w) = w₁² + w₂²
loss.backward()        # Tự tính gradient!
print(w.grad)          # tensor([2., 4.]) ← Đúng! ∂f/∂w = 2w
```

> 💡 **Key insight:** PyTorch = NumPy + Auto Gradient + GPU. Mọi kiến thức ĐSTT bạn học đều transfer 100%!

---

> 📖 **Chương tiếp theo:** [Chương 2: Giải tích — La bàn Tối ưu hóa](Chuong2_GiaiTich.md) — *"Đạo hàm là đôi mắt giúp AI biết hướng nào là đi xuống dốc."*

