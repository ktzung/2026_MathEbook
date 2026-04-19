# CHƯƠNG 2: GIẢI TÍCH — LA BÀN TỐI ƯU HÓA

## 📖 CALCULUS — THE COMPASS OF OPTIMIZATION

> *"Nếu AI là con tàu, thì Giải tích là la bàn giúp nó không đi lạc. Mỗi bước đi, đạo hàm chỉ cho AI biết: 'Đi hướng này sẽ tốt hơn.'"*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 2: "Học cách điều chỉnh"
>
> *N-42 đã biết rằng mình nhận tín hiệu đầu vào bằng cách tính tổng và nhân các trọng số (biểu diễn qua phép tích vô hướng - dot product w·x). Nhưng vấn đề là kết quả dự đoán sai bét! Nó cần biết: "Vặn nút trọng số (weights) theo hướng nào để bớt sai?" Đạo hàm sẽ là đôi mắt giúp N-42 nhìn thấy đường xuống đồi — và Gradient Descent chính là bước chân đưa nó đến đáy.*

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: Chiếc xe tự lái và cú phanh hoàn hảo

Bạn ngồi trên chiếc Tesla đang chạy 80 km/h. Phía trước 100 mét là đèn đỏ. Chiếc xe bắt đầu giảm tốc.

Nhưng nó không phanh gấp (bạn sẽ bị giật văng). Nó cũng không phanh chậm quá (sẽ vượt đèn đỏ). Nó phanh **vừa đủ**, giảm dần đều, dừng đúng vạch.

Làm sao xe biết khi nào phanh và phanh mạnh bao nhiêu?

Câu trả lời nằm ở **đạo hàm** — phép tính cho biết "tốc độ thay đổi" tại mỗi thời điểm. Tốc độ xe giảm dần → đạo hàm (gia tốc) âm → xe đang chậm lại. Khi đạo hàm = 0, xe dừng.

> 💡 **Takeaway:** Đạo hàm = "cảm biến tốc độ thay đổi". AI dùng nó để biết hướng nào sẽ giảm sai số nhanh nhất.

---

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Khái niệm | Một câu tóm tắt | Tại sao cần cho AI |
|---|-----------|-----------------|---------------------|
| 1 | **Đạo hàm** | Tốc độ thay đổi — Độ dốc | Biết hướng giảm sai số |
| 2 | **Đạo hàm riêng** | Đạo hàm từng biến, giữ cố định biến khác | Gradient cho weight riêng lẻ |
| 3 | **Gradient** | Vector tất cả đạo hàm riêng | "La bàn" chỉ hướng dốc nhất |
| 4 | **Chain Rule** | Đạo hàm hàm hợp | Backpropagation! |
| 5 | **Tích phân** | Diện tích dưới đường cong | Xác suất, Tổng tích lũy |
| 6 | **Hàm lồi** | Chỉ có 1 đáy duy nhất | Đảm bảo GD tìm được global min |
| 7 | **SGD & Adam** | Các biến thể GD thông minh | Optimizer thực tế |

> 📋 **Prerequisites:** Ch.1 §1-2 (Vector, Ma trận cơ bản). Biết khái niệm hàm số f(x).
>
> ⏱️ **Thời gian đọc:** ~45 phút (Track A) | ~80 phút (Track B)

---

## 🗺️ BẠN ĐANG Ở ĐÂY

```
  Ch.0 ━━▶ Ch.1 ĐSTT ━━▶ 📍 Ch.2 GIẢI TÍCH ━━▶ Ch.3 XS-TK ━━▶ Ch.4 Toán RR ━━▶ ...
  (✅ Done)                    BẠN ĐANG Ở ĐÂY      (Tiếp theo)
```

**Phần này cần học trước:** Ch.1 §1-2 (Vector, Dot Product, Ma trận cơ bản)
**Chương này mở khóa:** Ch.5 (Neural Network hoàn chỉnh), Ch.6 (Jacobian/Hessian, Tối ưu nâng cao)
**Bạn sẽ cần lại kiến thức này khi:** Huấn luyện bất kỳ mô hình AI nào (GD là nền tảng), debug "tại sao loss không giảm?", đọc paper ML.

---

## 📦 ÔN NHANH TOÁN PHỔ THÔNG

> *Phần này dành cho bạn nào cảm thấy chưa vững nền tảng. Nếu bạn tự tin, hãy bỏ qua! Không xấu hổ khi ôn lại — giáo sư cũng phải tra công thức mỗi ngày.*

<details>
<summary>📈 <b>Ôn nhanh 1: Hàm số y = f(x) — "Cỗ máy biến đổi số"</b> (Bấm để mở)</summary>

Hàm số là "cỗ máy": bạn đưa số vào (input = x), máy cho ra số khác (output = y).

```
    x = 3  ──→ ┌──────────────┐ ──→  y = f(3) = 11
                │ f(x) = x² + 2 │
    x = -2 ──→ └──────────────┘ ──→  y = f(-2) = 6
```

- $f(x) = 2x + 1$: Hàm bậc nhất (đồ thị là đường thẳng)
- $f(x) = x^2$: Hàm bậc hai (đồ thị hình parabol ∪)
- $f(x) = e^x$: Hàm mũ (tăng rất nhanh — mô hình tăng trưởng)

> 💡 Bạn sẽ dùng kiến thức này ở **Phần 1** (Đạo hàm) — đạo hàm cho biết "tốc độ thay đổi" của hàm số.

</details>

<details>
<summary>📉 <b>Ôn nhanh 2: Đồ thị hàm bậc 2 — "Hình parabol và điểm cực trị"</b></summary>

Hàm $f(x) = ax^2 + bx + c$ có đồ thị hình parabol:

```
    a > 0: ∪ (bát ngửa)         a < 0: ∩ (bát úp)
    
        │    *                   *    *
        │   * *                 *      *
        │  *   *               *        *
        │ *     *             ─┼──────────
        ─┼───────
         Đỉnh = cực tiểu       Đỉnh = cực đại
```

- Đỉnh parabol: $x = -\frac{b}{2a}$ → Đây là điểm **cực trị** (min hoặc max)
- Ví dụ: $f(x) = x^2 - 4x + 5$ → Đỉnh tại $x = \frac{4}{2} = 2$, $f(2) = 1$ → Cực tiểu = 1

> 💡 Gradient Descent ở **Phần 2** chính là thuật toán **tìm đáy parabol** — nhưng cho hàm phức tạp hơn nhiều!

</details>

<details>
<summary>🔄 <b>Ôn nhanh 3: Giới hạn — "Tiến gần vô hạn nhưng không bao giờ chạm"</b></summary>

Giới hạn = giá trị mà hàm số **tiến đến** khi x tiến đến một giá trị nào đó.

Ví dụ đơn giản: $\lim_{x \to 2} (x^2 + 1) = 2^2 + 1 = 5$

Ví dụ thú vị: $\lim_{x \to 0} \frac{\sin x}{x} = 1$ (dù $\sin(0)/0 = 0/0$, nhưng giới hạn = 1!)

**Liên tưởng:** Bạn đứng cách tường 1 mét. Mỗi bước bạn tiến thêm 1/2 khoảng cách còn lại:
- Bước 1: còn 0.5m
- Bước 2: còn 0.25m
- Bước 3: còn 0.125m
- ...

Bạn không bao giờ chạm tường, nhưng bạn **tiến gần vô hạn** → Giới hạn = tường!

> 💡 Đạo hàm chính là **giới hạn** của tỉ số $\frac{f(x+\Delta x) - f(x)}{\Delta x}$ khi $\Delta x \to 0$. Bạn sẽ thấy ở **Phần 1**.

</details>

---

## 🏷️ HƯỚNG DẪN ĐỌC — Hệ thống 3 tầng

Mỗi phần trong chương này được đánh dấu bằng **3 tầng** — bạn chọn mức phù hợp:

| Tầng | Label | Dành cho ai? | Cần gì? |
|------|-------|-------------|--------|
| 🟢 | **Hiểu bằng trực giác** | Tất cả mọi người | Chỉ cần biết đọc tiếng Việt! |
| 🟡 | **Hiểu bằng toán** | Muốn tính toán được | Cần biết cộng/trừ/nhân/chia |
| 🔵 | **Hiểu bằng code & ứng dụng** | Developer / Researcher | Cần biết Python cơ bản |

> 💡 **Mẹo:** Đọc tầng 🟢 trước cho MỌI phần. Nếu hiểu rồi, nhảy sang 🟡. Nếu vẫn OK, đọc 🔵. Bạn **KHÔNG CẦN** đọc hết cả 3 tầng mới được đi tiếp!

---

## PHẦN 1: ĐẠO HÀM — ĐO "ĐỘ DỐC"

### 🟢 1.1 Đạo hàm KHÔNG phải công thức thuộc lòng

Nhiều sinh viên nhớ: "$f'(x^n) = nx^{n-1}$" nhưng không hiểu **nó nghĩa là gì**. Hãy quên công thức đi. Hiểu ý tưởng trước.

#### 🎯 Liên tưởng 1: Leo núi trong sương mù

Bạn đang đứng trên một ngọn đồi, sương mù dày đặc, không nhìn thấy gì. Bạn muốn xuống chân đồi (điểm thấp nhất).

Bạn có thể làm gì? **Cảm nhận độ dốc dưới chân!**

- Nếu chân trái thấp hơn chân phải → nghiêng sang trái → đi sang trái
- Nếu chân phải thấp hơn → đi sang phải
- Nếu bằng phẳng → bạn đang ở đáy hoặc đỉnh

**Đạo hàm chính là "độ dốc dưới chân" đó!**

$$f'(x) = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x}$$

Dịch ngôn ngữ đời thường:

> *"Nếu tôi nhích một bước rất nhỏ ($\Delta x$), thì độ cao ($f$) thay đổi bao nhiêu?"*

- $f'(x) > 0$: Đang lên dốc → Nếu muốn xuống, **đi ngược lại**
- $f'(x) < 0$: Đang xuống dốc → Đúng hướng, tiếp tục đi
- $f'(x) = 0$: Bằng phẳng → Có thể là đáy (cực tiểu) hoặc đỉnh (cực đại)

#### 🎯 Liên tưởng 2: Tốc độ xe trên đồng hồ

| Khái niệm | Ý nghĩa thực tế |
|-----------|-----------------|
| $f(t)$ = vị trí | Xe đang ở km thứ mấy |
| $f'(t)$ = vận tốc | Xe đang chạy nhanh bao nhiêu |
| $f''(t)$ = gia tốc | Xe đang tăng tốc hay giảm tốc |

→ Đạo hàm bậc 1 = "tốc độ thay đổi". Đạo hàm bậc 2 = "tốc độ của tốc độ thay đổi".

### 🟡 1.2 Các quy tắc đạo hàm cốt lõi

Chỉ cần nhớ **4 quy tắc** là đủ cho 80% bài toán:

| Quy tắc | Công thức | Ví dụ |
|---------|-----------|-------|
| Hàm lũy thừa | $(x^n)' = n \cdot x^{n-1}$ | $(x^3)' = 3x^2$ |
| Hằng số nhân | $(c \cdot f)' = c \cdot f'$ | $(5x^2)' = 10x$ |
| Tổng | $(f + g)' = f' + g'$ | $(x^2 + 3x)' = 2x + 3$ |
| Quy tắc chuỗi (Chain Rule) | $(f(g(x)))' = f'(g) \cdot g'(x)$ | $((2x+1)^3)' = 3(2x+1)^2 \cdot 2$ |

> 💡 **Chain Rule** là quan trọng nhất trong AI — nó là nền tảng của **Backpropagation** (lan truyền ngược), cách mọi mạng neural học.

---

> 🌱 **VÍ DỤ VỠ LÒNG — Tính đạo hàm bằng tay (30 giây)**
>
> Cho $f(x) = x^3$. Đạo hàm: $(x^n)' = n \cdot x^{n-1}$
>
> $$f'(x) = 3x^2$$
>
> Tại $x = 2$: $f'(2) = 3 \times 2^2 = 3 \times 4 = 12$
>
> Nghĩa là: Tại $x=2$, hàm đang **tăng** với tốc độ 12 đơn vị/bước. Dốc lên khá mạnh!
>
> Thử thêm: $f'(0) = 3 \times 0^2 = 0$ → Tại $x=0$, hàm **bằng phẳng** (không tăng không giảm).

---

> 🧮 **TÍNH THỬ BẰNG TAY — Chain Rule (1 phút)**
>
> Cho $h(x) = (2x + 1)^3$. Tính $h'(x)$.
>
> - Hàm ngoài: $f(u) = u^3$ → $f'(u) = 3u^2$
> - Hàm trong: $g(x) = 2x + 1$ → $g'(x) = 2$
> - Chain Rule: $h'(x) = f'(g(x)) \cdot g'(x) = 3(2x+1)^2 \cdot 2 = 6(2x+1)^2$
>
> Tại $x = 1$: $h'(1) = 6 \times (2 \times 1 + 1)^2 = 6 \times 9 = 54$
>
> 💡 Chain Rule = "nhân đạo hàm từng lớp lại". Backpropagation trong AI chính là Chain Rule áp dụng qua nhiều lớp!

---

### 🔵 1.3 Code Python: "Cảm nhận" đạo hàm

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. TRỰC QUAN HÓA ĐẠO HÀM = ĐỘ DỐC
# =============================================

def f(x):
    """Hàm ví dụ: Đồi núi"""
    return x**3 - 6*x**2 + 9*x + 2

def f_dao_ham(x):
    """Đạo hàm: Độ dốc tại mỗi điểm"""
    return 3*x**2 - 12*x + 9

x = np.linspace(-1, 5, 300)

fig, axes = plt.subplots(2, 1, figsize=(12, 10), sharex=True)

# --- Hình 1: Hàm gốc ---
ax1 = axes[0]
ax1.plot(x, f(x), 'b-', linewidth=2.5, label='f(x) = x³ - 6x² + 9x + 2')
ax1.fill_between(x, f(x), alpha=0.1, color='blue')

# Điểm cực trị
cuc_dai_x, cuc_tieu_x = 1.0, 3.0
ax1.plot(cuc_dai_x, f(cuc_dai_x), 'r^', markersize=15, label=f'Cực đại: x={cuc_dai_x}')
ax1.plot(cuc_tieu_x, f(cuc_tieu_x), 'gv', markersize=15, label=f'Cực tiểu: x={cuc_tieu_x}')

# Vẽ độ dốc (tiếp tuyến) tại vài điểm
for xi, color in [(0, '#FF5722'), (1, '#f44336'), (2, '#9C27B0'), (3, '#4CAF50'), (4, '#2196F3')]:
    slope = f_dao_ham(xi)
    tangent_x = np.linspace(xi - 0.8, xi + 0.8, 10)
    tangent_y = slope * (tangent_x - xi) + f(xi)
    ax1.plot(tangent_x, tangent_y, '--', color=color, linewidth=1.5, alpha=0.7)
    ax1.annotate(f"Dốc = {slope:.0f}", xy=(xi, f(xi)), xytext=(xi+0.3, f(xi)+1.5),
                fontsize=8, color=color, fontweight='bold')

ax1.set_ylabel("f(x) — Độ cao", fontsize=12)
ax1.set_title("🏔️ Hàm số = Địa hình (Đồi núi)", fontsize=14, fontweight='bold')
ax1.legend(fontsize=10)
ax1.grid(True, alpha=0.3)

# --- Hình 2: Đạo hàm ---
ax2 = axes[1]
ax2.plot(x, f_dao_ham(x), 'r-', linewidth=2.5, label="f'(x) = 3x² - 12x + 9")
ax2.fill_between(x, f_dao_ham(x), where=(f_dao_ham(x) > 0), alpha=0.15, 
                  color='green', label='Đang lên dốc (f\' > 0)')
ax2.fill_between(x, f_dao_ham(x), where=(f_dao_ham(x) < 0), alpha=0.15, 
                  color='red', label='Đang xuống dốc (f\' < 0)')
ax2.axhline(y=0, color='black', linewidth=1)

ax2.plot(cuc_dai_x, 0, 'r^', markersize=12)
ax2.plot(cuc_tieu_x, 0, 'gv', markersize=12)
ax2.annotate("f'=0 → Cực đại", xy=(cuc_dai_x, 0), xytext=(0.2, -8),
            fontsize=10, fontweight='bold', color='red',
            arrowprops=dict(arrowstyle='->', color='red'))
ax2.annotate("f'=0 → Cực tiểu", xy=(cuc_tieu_x, 0), xytext=(3.5, -8),
            fontsize=10, fontweight='bold', color='green',
            arrowprops=dict(arrowstyle='->', color='green'))

ax2.set_xlabel("x", fontsize=12)
ax2.set_ylabel("f'(x) — Độ dốc", fontsize=12)
ax2.set_title("📐 Đạo hàm = Độ dốc tại mỗi điểm", fontsize=14, fontweight='bold')
ax2.legend(fontsize=10)
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('chuong2_dao_ham.png', dpi=150, bbox_inches='tight')
plt.show()
print("✅ Đã lưu hình: chuong2_dao_ham.png")
```

> 📌 **Lưu ý cho giảng viên:** Cho sinh viên thay đổi hàm f(x) (ví dụ: $x^4 - 4x^2$) và quan sát đạo hàm tương ứng. Hỏi: "Nhìn đạo hàm, em có thể đoán được hình dạng hàm gốc không?"

---

> ✅ **CHECKPOINT 1 — Đạo hàm & Ý nghĩa**
>
> Trả lời nhanh (không cần code):
>
> 1. Nếu $f'(x) > 0$ tại điểm $x_0$, hàm đang lên dốc hay xuống dốc tại đó?
> 2. Tính tay: $(5x^3 + 2x)' = ?$
> 3. Khi $f'(x) = 0$, điều đó có nghĩa gì? (Gợi ý: nghĩ về đỉnh đồi hoặc đáy thung lũng)
> 4. Tại sao Chain Rule quan trọng trong AI?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Lên dốc** — Đạo hàm dương = hàm đang tăng. Muốn tìm cực tiểu → đi ngược lại (đó là ý tưởng GD!)
> 2. $15x^2 + 2$ (áp dụng: $(x^n)' = nx^{n-1}$ cho từng hạng)
> 3. Hàm **bằng phẳng** tại đó → Có thể là **cực đại** (đỉnh đồi), **cực tiểu** (đáy thung lũng), hoặc **điểm yên ngựa**
> 4. Vì neural network là chuỗi hàm hợp: $f_3(f_2(f_1(x)))$. Chain Rule cho phép tính gradient **ngược từ output về input** = Backpropagation
>
> </details>
>
> **Tự đánh giá:** Đúng 4/4 → 🟢 Xuất sắc! | 2-3/4 → 🟡 Ổn | 0-1/4 → 🔴 Đọc lại Phần 1

---

## PHẦN 1B: TÍCH PHÂN — "TÍNH TỔNG LIÊN TỤC"

### 1B.1 Tích phân KHÔNG phải ký hiệu đáng sợ

Nếu đạo hàm là "tốc độ thay đổi", thì **tích phân** là "tổng tích lũy" — phép tính ngược lại.

$$\int_a^b f(x) \, dx = \text{"Diện tích dưới đường cong f(x) từ a đến b"}$$

#### 🎯 Liên tưởng 1: Đồng hồ tốc độ và quãng đường

| Đạo hàm (từ quãng đường → tốc độ) | Tích phân (từ tốc độ → quãng đường) |
|--------|----------|
| Vị trí $s(t)$ → Tốc độ $v(t) = s'(t)$ | Tốc độ $v(t)$ → Quãng đường $s = \int v(t) dt$ |
| "Xe đang chạy nhanh bao nhiêu?" | "Xe đã đi tổng bao xa?" |

Nếu xe chạy đều 60 km/h trong 2 giờ: $\int_0^2 60 \, dt = 120$ km. Nhưng nếu tốc độ thay đổi liên tục thì sao? Tích phân tính được!

#### 🎯 Ví dụ kinh tế: Tính GDP từ hàm sản xuất 💰

Nếu tốc độ sản xuất tại thời điểm $t$ là $P(t) = 100 + 5t$ (triệu VNĐ/giờ), thì tổng sản lượng trong 8 giờ:

$$\text{Tổng sản lượng} = \int_0^8 (100 + 5t) \, dt = [100t + 2.5t^2]_0^8 = 800 + 160 = 960 \text{ triệu}$$

### 1B.2 Tại sao cần tích phân trong ML?

| Ứng dụng | Vai trò tích phân |
|----------|-------------------|
| **Xác suất liên tục** | $P(a < X < b) = \int_a^b f(x) dx$ (diện tích dưới PDF) |
| **Kỳ vọng** | $E[X] = \int x \cdot f(x) dx$ |
| **Cross-Entropy Loss** | $H = -\int p(x) \log q(x) dx$ |
| **Normalizing Flows** | Biến đổi phân phối qua tích phân |
| **Bayesian Inference** | $P(\theta | D) = \frac{P(D|\theta) P(\theta)}{\int P(D|\theta) P(\theta) d\theta}$ |

> 💡 **Nhận ra chưa?** Mỗi khi bạn thấy $P(X > 5)$ trên phân phối liên tục (Normal, Exponential...), bạn đang **tính tích phân** dưới đường cong!

### 1B.3 Các quy tắc tích phân cốt lõi

| Quy tắc | Công thức | Ví dụ |
|---------|-----------|-------|
| Hàm lũy thừa | $\int x^n dx = \frac{x^{n+1}}{n+1} + C$ | $\int x^2 dx = \frac{x^3}{3} + C$ |
| Hằng số nhân | $\int c \cdot f(x) dx = c \int f(x) dx$ | $\int 5x dx = \frac{5x^2}{2} + C$ |
| Tổng | $\int (f+g) dx = \int f dx + \int g dx$ | |
| $e^x$ | $\int e^x dx = e^x + C$ | Tăng trưởng liên tục |

### 1B.4 Code Python: Tích phân trực quan

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import integrate

# =============================================
# 1. TÍCH PHÂN = DIỆN TÍCH DƯỚI ĐƯỜNG CONG
# =============================================

def f(x):
    """Hàm sản xuất: tốc độ sản xuất theo giờ"""
    return 100 + 5*x

x = np.linspace(0, 8, 300)

# Tính tích phân bằng scipy
result, error = integrate.quad(f, 0, 8)
print(f"💰 VÍ DỤ KINH TẾ: Tổng sản lượng 8 giờ")
print(f"   P(t) = 100 + 5t (triệu VNĐ/giờ)")
print(f"   ∫₀⁸ P(t) dt = {result:.0f} triệu VNĐ")

# =============================================
# 2. XÁC SUẤT = TÍCH PHÂN DƯỚI PDF
# =============================================

from scipy.stats import norm

mu, sigma = 170, 8  # Chiều cao: trung bình 170cm, σ = 8cm
x_norm = np.linspace(140, 200, 300)
pdf = norm.pdf(x_norm, mu, sigma)

# P(165 < X < 175) = ?
prob, _ = integrate.quad(norm.pdf, 165, 175, args=(mu, sigma))
print(f"\n📊 VÍ DỤ XÁC SUẤT: Chiều cao VN (μ=170, σ=8)")
print(f"   P(165 < X < 175) = ∫₁₆₅¹⁷⁵ f(x) dx = {prob:.1%}")

# =============================================
# 3. TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Hình 1: Tích phân = diện tích (kinh tế)
ax1 = axes[0]
ax1.plot(x, f(x), 'b-', linewidth=2.5, label='P(t) = 100 + 5t')
ax1.fill_between(x, f(x), alpha=0.3, color='#4CAF50')
ax1.set_title(f'💰 Tổng sản lượng = Diện tích\n∫₀⁸ P(t) dt = {result:.0f} triệu', 
              fontsize=13, fontweight='bold')
ax1.set_xlabel('Giờ (t)', fontsize=11)
ax1.set_ylabel('Tốc độ sản xuất P(t)', fontsize=11)
ax1.legend(fontsize=11); ax1.grid(True, alpha=0.3)

# Hình 2: Xác suất = diện tích dưới PDF
ax2 = axes[1]
ax2.plot(x_norm, pdf, 'b-', linewidth=2.5, label=f'Normal(μ={mu}, σ={sigma})')
ax2.fill_between(x_norm, pdf, where=(x_norm >= 165) & (x_norm <= 175), 
                  alpha=0.4, color='#FF5722', label=f'P(165<X<175) = {prob:.1%}')
ax2.set_title('📊 Xác suất = Diện tích dưới PDF\n(Đây chính là TÍCH PHÂN!)', 
              fontsize=13, fontweight='bold')
ax2.set_xlabel('Chiều cao (cm)', fontsize=11)
ax2.set_ylabel('Mật độ xác suất f(x)', fontsize=11)
ax2.legend(fontsize=10); ax2.grid(True, alpha=0.3)

plt.suptitle('📐 Tích phân = Tính tổng liên tục = Diện tích dưới đường cong', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong2_tich_phan.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Không cần dạy kỹ thuật tính tích phân phức tạp. Trong ML, gần như luôn dùng **tích phân số** (numerical integration). Mục tiêu: sinh viên hiểu **ý nghĩa** (diện tích dưới đường cong) và biết dùng `scipy.integrate.quad()`.

---

## PHẦN 2: GRADIENT DESCENT — "XUỐNG ĐỒI TRONG SƯƠNG MÙ"

### 2.1 Ý tưởng cốt lõi

**Gradient Descent** (GD) là thuật toán **quan trọng nhất** trong Machine Learning. Mọi mạng neural, mọi mô hình AI đều dùng nó.

Ý tưởng đơn giản đến ngạc nhiên:

1. Bắt đầu ở vị trí ngẫu nhiên trên "ngọn đồi"
2. Nhìn xuống chân: "Hướng nào dốc nhất?" (= đạo hàm)
3. Bước một bước nhỏ theo hướng xuống dốc
4. Lặp lại cho đến khi đến đáy

Công thức:

$$x_{new} = x_{old} - \alpha \cdot f'(x_{old})$$

- $x_{old}$: vị trí hiện tại
- $f'(x_{old})$: độ dốc (gradient) → **hướng đi**
- $\alpha$: **learning rate** (tốc độ học) → **kích thước bước**
- $x_{new}$: vị trí mới

#### 🎯 Liên tưởng: Viên bi lăn xuống lòng chảo

Thả viên bi trên mặt cong:
- Viên bi tự lăn xuống chỗ thấp nhất (theo gradient)
- Nếu lòng chảo trơn → bi lăn nhanh ($\alpha$ lớn), có thể lăn qua đáy
- Nếu lòng chảo nhám → bi lăn chậm ($\alpha$ nhỏ), chậm nhưng chính xác
- Bi dừng lại khi đến đáy (gradient ≈ 0)

### 2.2 Learning Rate — Không quá nhanh, không quá chậm

| Learning Rate | Hành vi | Kết quả |
|--------------|---------|---------|
| $\alpha$ quá lớn | Bước nhảy quá dài | Nhảy qua nhảy lại, không hội tụ 🤯 |
| $\alpha$ quá nhỏ | Bước nhích rất chậm | Mất quá lâu mới đến đáy 🐌 |
| $\alpha$ vừa phải | Bước đều, giảm dần | Hội tụ mượt mà ✅ |

### 2.3 Code Python: Gradient Descent trực quan

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import FancyArrowPatch

# =============================================
# 1. GRADIENT DESCENT 1 CHIỀU
# =============================================

def f(x):
    """Hàm mục tiêu: Tìm cực tiểu"""
    return (x - 3)**2 + 2 * np.sin(2*x) + 5

def f_prime(x):
    """Đạo hàm"""
    return 2*(x - 3) + 4 * np.cos(2*x)

def gradient_descent(x_start, learning_rate, n_steps):
    """
    Thuật toán Gradient Descent
    
    Tham số:
        x_start: Điểm xuất phát
        learning_rate: Tốc độ học (alpha)
        n_steps: Số bước
    
    Trả về:
        history: Danh sách các vị trí đã đi qua
    """
    x = x_start
    history = [x]
    
    for step in range(n_steps):
        gradient = f_prime(x)         # Đo độ dốc
        x = x - learning_rate * gradient  # Bước xuống dốc
        history.append(x)
    
    return history

# Chạy với 3 learning rate khác nhau
x_start = -1  # Bắt đầu từ x = -1

lr_small = gradient_descent(x_start, 0.01, 100)   # Quá chậm
lr_good  = gradient_descent(x_start, 0.1, 30)     # Vừa phải
lr_big   = gradient_descent(x_start, 0.5, 30)     # Quá nhanh

# =============================================
# 2. TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 3, figsize=(18, 6))
x_plot = np.linspace(-2, 7, 300)

configs = [
    ("α = 0.01 (Quá chậm 🐌)", lr_small, '#FF9800'),
    ("α = 0.1 (Vừa phải ✅)", lr_good, '#4CAF50'),
    ("α = 0.5 (Quá nhanh 🤯)", lr_big, '#f44336'),
]

for ax, (title, history, color) in zip(axes, configs):
    # Vẽ hàm
    ax.plot(x_plot, f(x_plot), 'b-', linewidth=2, alpha=0.5)
    
    # Vẽ đường đi
    history = np.array(history)
    for i in range(min(len(history)-1, 25)):
        ax.plot([history[i], history[i+1]], 
                [f(history[i]), f(history[i+1])], 
                'o-', color=color, markersize=5, alpha=0.6)
    
    # Điểm bắt đầu
    ax.plot(history[0], f(history[0]), 's', color='black', 
            markersize=12, label=f'Bắt đầu: x={history[0]:.1f}')
    # Điểm kết thúc
    ax.plot(history[-1], f(history[-1]), '★', color=color, 
            markersize=20, label=f'Kết thúc: x={history[-1]:.2f}')
    
    ax.set_title(title, fontsize=13, fontweight='bold', color=color)
    ax.set_xlabel('x')
    ax.set_ylabel('f(x)')
    ax.legend(fontsize=9)
    ax.grid(True, alpha=0.3)
    ax.set_ylim(-5, 30)

plt.suptitle("🎯 Gradient Descent: Ảnh hưởng của Learning Rate", 
             fontsize=15, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong2_gradient_descent.png', dpi=150, bbox_inches='tight')
plt.show()

# In kết quả
print("\n📊 Kết quả Gradient Descent:")
print(f"   α=0.01: x = {lr_small[-1]:.4f}, f(x) = {f(lr_small[-1]):.4f} (100 bước)")
print(f"   α=0.1:  x = {lr_good[-1]:.4f}, f(x) = {f(lr_good[-1]):.4f} (30 bước)")
print(f"   α=0.5:  x = {lr_big[-1]:.4f}, f(x) = {f(lr_big[-1]):.4f} (30 bước)")
```

---

> ✅ **CHECKPOINT 2 — Gradient Descent & Learning Rate**
>
> 1. Trong Gradient Descent, tại sao ta đi **ngược** hướng gradient (dấu trừ)?
> 2. Learning rate = 0.5 quá lớn → Chuyện gì xảy ra? (Chọn 1: hội tụ nhanh / nhảy qua nhảy lại / bước chậm)
> 3. Viết công thức cập nhật GD: $x_{new} = ?$
> 4. Hàm $f(x) = (x-5)^2$ có cực tiểu tại $x = ?$. Nếu xuất phát từ $x=0$, GD sẽ đi về hướng nào?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Gradient chỉ hướng **tăng nhanh nhất**. Muốn giảm (tìm cực tiểu) → đi **ngược** = trừ gradient
> 2. **Nhảy qua nhảy lại** — lr quá lớn khiến bước nhảy vượt qua đáy, rồi quay lại, rồi vượt tiếp → không hội tụ
> 3. $x_{new} = x_{old} - \alpha \cdot f'(x_{old})$
> 4. Cực tiểu tại $x=5$. Tại $x=0$: $f'(0) = 2(0-5) = -10 < 0$, nên $x_{new} = 0 - \alpha \times (-10) = 0 + 10\alpha$ → Đi sang **phải** (về phía x=5) ✅
>
> </details>
>
> **Tự đánh giá:** Đúng 4/4 → 🟢 Tiến lên! | 2-3/4 → 🟡 Đọc lại §2.1-2.2 | 0-1/4 → 🔴 Xem lại ví dụ "leo núi sương mù"

---

## PHẦN 3: ĐẠO HÀM RIÊNG & GRADIENT — MỞ RỘNG SANG NHIỀU CHIỀU

### 3.1 Vấn đề: Thế giới thực có nhiều biến

Trong ví dụ trên, ta chỉ tối ưu 1 biến ($x$). Nhưng trong AI:
- Mạng neural có **hàng triệu** tham số (weights)
- Mỗi tham số là một "nút vặn" cần điều chỉnh

Vậy khi hàm có 2 biến trở lên → $f(x, y)$ → ta cần **đạo hàm riêng**.

### 3.2 Đạo hàm riêng — "Điều chỉnh từng nút một"

$$\frac{\partial f}{\partial x} \quad \text{= Đạo hàm theo x (giữ y cố định)}$$

$$\frac{\partial f}{\partial y} \quad \text{= Đạo hàm theo y (giữ x cố định)}$$

#### 🎯 Liên tưởng: Bàn mixer âm thanh

Hãy tưởng tượng bạn là DJ với bàn mixer có 2 nút vặn:
- Nút **Bass** ($x$): Điều chỉnh âm trầm
- Nút **Treble** ($y$): Điều chỉnh âm bổng

Đạo hàm riêng $\frac{\partial f}{\partial x}$ = "Nếu tôi vặn nút Bass thêm một chút mà giữ nguyên Treble, thì chất lượng âm thanh thay đổi bao nhiêu?"

**Gradient** = Tập hợp tất cả đạo hàm riêng:

$$\nabla f = \left(\frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}\right)$$

→ Gradient chỉ ra **hướng tăng nhanh nhất** của hàm số. Muốn giảm? Đi ngược hướng gradient!

### 3.3 Code Python: Gradient Descent 2D

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# =============================================
# 1. GRADIENT DESCENT 2 CHIỀU
# =============================================

def f_2d(x, y):
    """Hàm 2 biến: Bề mặt lõm (lòng chảo)"""
    return (x - 2)**2 + 3*(y - 1)**2 + x*y - 4

def gradient_2d(x, y):
    """Gradient (vector đạo hàm riêng)"""
    df_dx = 2*(x - 2) + y        # ∂f/∂x
    df_dy = 6*(y - 1) + x        # ∂f/∂y
    return np.array([df_dx, df_dy])

def gradient_descent_2d(start, lr, n_steps):
    """GD trong 2 chiều"""
    pos = np.array(start, dtype=float)
    history = [pos.copy()]
    
    for _ in range(n_steps):
        grad = gradient_2d(pos[0], pos[1])
        pos = pos - lr * grad
        history.append(pos.copy())
    
    return np.array(history)

# Chạy GD
start = np.array([-3.0, 4.0])
history = gradient_descent_2d(start, lr=0.05, n_steps=50)

# =============================================
# 2. TRỰC QUAN HÓA 3D + CONTOUR
# =============================================

fig = plt.figure(figsize=(18, 7))

# --- Hình 1: Bề mặt 3D ---
ax1 = fig.add_subplot(131, projection='3d')
x_range = np.linspace(-5, 6, 100)
y_range = np.linspace(-3, 6, 100)
X, Y = np.meshgrid(x_range, y_range)
Z = f_2d(X, Y)

ax1.plot_surface(X, Y, Z, cmap='coolwarm', alpha=0.6, edgecolor='none')
ax1.plot(history[:, 0], history[:, 1], 
         [f_2d(h[0], h[1]) for h in history],
         'r.-', linewidth=2, markersize=5, label='Đường đi GD')
ax1.set_title("🏔️ Bề mặt 3D\n(Tìm đáy lòng chảo)", fontsize=12, fontweight='bold')
ax1.set_xlabel('x'); ax1.set_ylabel('y'); ax1.set_zlabel('f(x,y)')

# --- Hình 2: Đường đồng mức (Contour) ---
ax2 = fig.add_subplot(132)
contour = ax2.contour(X, Y, Z, levels=30, cmap='coolwarm', alpha=0.7)
ax2.clabel(contour, inline=True, fontsize=7)

# Vẽ đường đi
ax2.plot(history[:, 0], history[:, 1], 'r.-', linewidth=2, markersize=5)
ax2.plot(history[0, 0], history[0, 1], 'ks', markersize=12, label='Bắt đầu')
ax2.plot(history[-1, 0], history[-1, 1], 'r★', markersize=15, label='Kết thúc')

# Vẽ mũi tên gradient tại vài điểm
for i in range(0, len(history)-1, 5):
    grad = gradient_2d(history[i, 0], history[i, 1])
    grad_norm = grad / (np.linalg.norm(grad) + 1e-8) * 0.5
    ax2.annotate('', xy=(history[i, 0] - grad_norm[0], history[i, 1] - grad_norm[1]),
                xytext=(history[i, 0], history[i, 1]),
                arrowprops=dict(arrowstyle='->', color='#FF5722', lw=1.5))

ax2.set_title("📍 Đường đồng mức + Đường đi GD\n(Nhìn từ trên xuống)", 
              fontsize=12, fontweight='bold')
ax2.set_xlabel('x'); ax2.set_ylabel('y')
ax2.legend(fontsize=9)
ax2.grid(True, alpha=0.3)

# --- Hình 3: Biểu đồ hội tụ ---
ax3 = fig.add_subplot(133)
losses = [f_2d(h[0], h[1]) for h in history]
ax3.plot(losses, 'b-', linewidth=2)
ax3.fill_between(range(len(losses)), losses, alpha=0.1, color='blue')
ax3.axhline(y=min(losses), color='green', linestyle='--', alpha=0.5, label=f'Min = {min(losses):.2f}')
ax3.set_title("📉 Biểu đồ hội tụ\n(Loss giảm dần)", fontsize=12, fontweight='bold')
ax3.set_xlabel('Bước (iteration)'); ax3.set_ylabel('f(x,y) — Loss')
ax3.legend(fontsize=10)
ax3.grid(True, alpha=0.3)

plt.suptitle("🎯 Gradient Descent 2D: Tìm đáy lòng chảo", 
             fontsize=15, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong2_gradient_descent_2d.png', dpi=150, bbox_inches='tight')
plt.show()

print(f"\n📊 Kết quả GD 2D:")
print(f"   Bắt đầu:  ({start[0]:.1f}, {start[1]:.1f}), f = {f_2d(start[0], start[1]):.2f}")
print(f"   Kết thúc: ({history[-1, 0]:.4f}, {history[-1, 1]:.4f}), f = {f_2d(history[-1, 0], history[-1, 1]):.4f}")
```

---

## PHẦN 3B: HÀM LỒI — "KHI NÀO GD CHẮC CHẮN TÌM ĐƯỢC ĐÁY?"

### 3B.1 Vấn đề: Local Minima vs Global Minimum

Gradient Descent tìm "đáy gần nhất" — nhưng đó có phải đáy **thấp nhất** không?

#### 🎯 Liên tưởng: Lăn bi trên địa hình

- **Địa hình lồi** (1 đáy duy nhất): Bi luôn lăn đến đáy → GD luôn thành công ✅
- **Địa hình gồ ghề** (nhiều đáy): Bi có thể mắc kẹt ở đáy cục bộ → GD có thể thất bại ⚠️

### 3B.2 Hàm lồi là gì?

Hàm $f$ là **lồi (convex)** nếu đường nối 2 điểm bất kỳ trên đồ thị luôn nằm **TRÊN** đồ thị:

$$f(\alpha x + (1-\alpha) y) \leq \alpha f(x) + (1-\alpha) f(y) \quad \forall \alpha \in [0,1]$$

| Hàm | Lồi? | Dùng ở đâu |
|-----|------|------------|
| $f(x) = x^2$ | ✅ Lồi | MSE Loss |
| $f(x) = ax + b$ | ✅ Lồi | Linear function |
| $f(x) = -\log(x)$ | ✅ Lồi | Cross-Entropy Loss |
| $f(x) = x^4 - 3x^2$ | ❌ Không lồi | Nhiều cực tiểu |

> 💡 **Tin vui cho ML:** Hầu hết loss function trong ML (MSE, Cross-Entropy) là **lồi** với linear model → GD luôn tìm được nghiệm tối ưu toàn cục. Với neural network (không lồi), GD vẫn hoạt dynamics tốt nhờ các kỹ thuật như SGD, Adam.

#### 🎯 Ví dụ đời sống: Chi phí vận chuyển 📦

Chi phí giao hàng = hàm lồi của khoảng cách: $C(d) = 10 + 2d + 0.01d^2$. Dù cửa hàng ở đâu, luôn tìm được vị trí kho **tối ưu duy nhất** giảm chi phí!

---

## PHẦN 3C: CÁC BIẾN THỂ GRADIENT DESCENT — "NÂNG CẤP VIÊN BI"

### 3C.1 Vấn đề với Vanilla GD

GD cơ bản có nhược điểm:
- **Chậm** khi dữ liệu lớn (tính gradient trên TOÀN BỘ dataset mỗi bước)
- **Nhảy ziczac** khi bề mặt hẹp dài (gradient dao động)
- **Learning rate cố định** — quá lớn thì phát tán, quá nhỏ thì chậm

### 3C.2 SGD, Momentum, Adam — "Viên bi thông minh"

| Optimizer | Ý tưởng | Liên tưởng |
|-----------|---------|------------|
| **SGD** | Dùng 1 mẫu ngẫu nhiên thay vì toàn bộ | 🎲 "Nghe lời 1 người thay vì khảo sát cả nước" |
| **Momentum** | Nhớ hướng đi trước → giảm dao động | 🏀 "Viên bi có quán tính — lăn qua ổ gà nhỏ" |
| **RMSProp** | Tự chỉnh lr cho từng chiều | ⚙️ "Mỗi nút vặn có tốc độ riêng" |
| **Adam** | Kết hợp Momentum + RMSProp | 🏆 "Optimizer mặc định #1 trong DL" |

**Công thức Adam (đơn giản hóa):**

$$m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t \quad \text{(Momentum — trung bình gradient)}$$
$$v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2 \quad \text{(RMSProp — trung bình gradient²)}$$
$$w_{t+1} = w_t - \frac{\alpha}{\sqrt{v_t} + \epsilon} m_t \quad \text{(Cập nhật thông minh)}$$

### 3C.3 Code Python: So sánh Optimizer

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# SO SÁNH CÁC OPTIMIZER
# =============================================

def f_2d(x, y):
    """Hàm Rosenbrock — thử thách kinh điển"""
    return (1 - x)**2 + 100*(y - x**2)**2

def grad_f(x, y):
    """Gradient"""
    dx = -2*(1-x) - 400*x*(y - x**2)
    dy = 200*(y - x**2)
    return np.array([dx, dy])

def run_optimizer(name, lr=0.001, n_steps=500, beta1=0.9, beta2=0.999):
    """Chạy optimizer và trả về lịch sử"""
    pos = np.array([-1.5, 1.5])
    history = [pos.copy()]
    m = np.zeros(2)  # Momentum
    v = np.zeros(2)  # RMSProp
    eps = 1e-8
    
    for t in range(1, n_steps + 1):
        g = grad_f(pos[0], pos[1])
        
        # Thêm nhiễu (mô phỏng SGD)
        if name != "GD":
            g += np.random.normal(0, 0.5, 2)
        
        if name == "GD":
            pos = pos - lr * 10 * g
        elif name == "SGD":
            pos = pos - lr * 10 * g
        elif name == "Momentum":
            m = beta1 * m + (1-beta1) * g
            pos = pos - lr * 20 * m
        elif name == "Adam":
            m = beta1 * m + (1-beta1) * g
            v = beta2 * v + (1-beta2) * g**2
            m_hat = m / (1 - beta1**t)
            v_hat = v / (1 - beta2**t)
            pos = pos - lr * 20 * m_hat / (np.sqrt(v_hat) + eps)
        
        history.append(pos.copy())
    
    return np.array(history)

# Chạy tất cả optimizer
np.random.seed(42)
results = {}
for name in ["GD", "SGD", "Momentum", "Adam"]:
    results[name] = run_optimizer(name)

# =============================================
# TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 4, figsize=(22, 5))

x_range = np.linspace(-2, 2, 200)
y_range = np.linspace(-1, 3, 200)
X, Y = np.meshgrid(x_range, y_range)
Z = np.log1p(f_2d(X, Y))  # Log scale để dễ nhìn

colors = {'GD': '#2196F3', 'SGD': '#FF5722', 'Momentum': '#4CAF50', 'Adam': '#9C27B0'}
emojis = {'GD': '🐌', 'SGD': '🎲', 'Momentum': '🏀', 'Adam': '🏆'}

for ax, (name, history) in zip(axes, results.items()):
    ax.contour(X, Y, Z, levels=30, cmap='coolwarm', alpha=0.5)
    
    # Đường đi
    ax.plot(history[:100, 0], history[:100, 1], '.-', 
            color=colors[name], linewidth=1, markersize=2, alpha=0.7)
    
    # Điểm đầu và cuối
    ax.plot(history[0, 0], history[0, 1], 'ks', markersize=10)
    ax.plot(history[-1, 0], history[-1, 1], color=colors[name], 
            marker='*', markersize=15)
    ax.plot(1, 1, 'r★', markersize=12)  # Global minimum
    
    final_loss = f_2d(history[-1, 0], history[-1, 1])
    ax.set_title(f"{emojis[name]} {name}\nFinal loss: {final_loss:.4f}", 
                fontsize=12, fontweight='bold', color=colors[name])
    ax.set_xlim(-2, 2); ax.set_ylim(-1, 3)

plt.suptitle('🏁 So sánh Optimizer trên hàm Rosenbrock — Adam thường tốt nhất!', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong2_optimizers.png', dpi=150, bbox_inches='tight')
plt.show()

print("\n📊 KẾT QUẢ:")
for name, history in results.items():
    final = f_2d(history[-1, 0], history[-1, 1])
    print(f"   {emojis[name]} {name:10s}: final loss = {final:.6f}, final pos = ({history[-1, 0]:.3f}, {history[-1, 1]:.3f})")
print("\n💡 Trong thực tế: Adam là optimizer MẶC ĐỊNH trong Deep Learning!")
```

> 📌 **Lưu ý cho giảng viên:** Sinh viên không cần nhớ công thức Adam. Chỉ cần hiểu: (1) Momentum = nhớ hướng cũ → giảm dao động, (2) RMSProp = tự chỉnh lr → nhanh hơn, (3) Adam = cả hai → tốt nhất. Cho sinh viên thay hàm khác nhau để thấy Adam "đáng tin cậy" nhất.

---

## PHẦN 4: CHAIN RULE & BACKPROPAGATION — CÁCH AI HỌC

### 4.1 Chain Rule — Quy tắc chuỗi

Khi hàm lồng nhau: $h(x) = f(g(x))$

$$h'(x) = f'(g(x)) \cdot g'(x)$$

#### 🎯 Liên tưởng: Hiệu ứng domino

Hãy tưởng tượng 3 quân domino xếp hàng:
- Quân 1 ($x$) ngã → đẩy Quân 2 ($g$) → đẩy Quân 3 ($f$)
- Tốc độ ngã cuối cùng = tốc độ từng cú đẩy nhân lại

Nếu Quân 1 ngã nhanh gấp 2 (g'=2), và Quân 2 đẩy mạnh gấp 3 (f'=3), thì Quân 3 ngã nhanh gấp $2 \times 3 = 6$.

### 4.2 Backpropagation = Chain Rule trên mạng Neural

Mạng neural là chuỗi các hàm lồng nhau:

$$\text{Output} = f_3(f_2(f_1(\text{Input})))$$

Để cập nhật trọng số, ta cần tính đạo hàm **ngược từ output về input** (Backpropagation):

$$\frac{\partial \text{Loss}}{\partial w_1} = \frac{\partial \text{Loss}}{\partial f_3} \cdot \frac{\partial f_3}{\partial f_2} \cdot \frac{\partial f_2}{\partial w_1}$$

→ Đó chính là Chain Rule áp dụng liên tiếp!

### 4.3 Code Python: Mạng Neural đơn giản "from scratch"

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# MẠNG NEURAL 1 TẦNG ẨN — HỌC HÀM XOR
# =============================================

# --- Hàm sigmoid ---
def sigmoid(x):
    return 1 / (1 + np.exp(-np.clip(x, -500, 500)))

def sigmoid_dao_ham(x):
    s = sigmoid(x)
    return s * (1 - s)

# --- Dữ liệu XOR ---
X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])  # Input
y = np.array([[0], [1], [1], [0]])                 # Output mong muốn

print("📊 Bài toán XOR:")
print("   Input → Output")
for xi, yi in zip(X, y):
    print(f"   {xi} → {yi[0]}")

# --- Khởi tạo trọng số ngẫu nhiên ---
np.random.seed(42)
W1 = np.random.randn(2, 4) * 0.5   # Input(2) → Hidden(4)
b1 = np.zeros((1, 4))
W2 = np.random.randn(4, 1) * 0.5   # Hidden(4) → Output(1)
b2 = np.zeros((1, 1))

learning_rate = 1.0
losses = []

# --- Huấn luyện ---
print(f"\n🏋️ Bắt đầu huấn luyện ({5000} epochs)...")

for epoch in range(5000):
    # ========= FORWARD PASS =========
    # Tầng 1: Input → Hidden
    z1 = X @ W1 + b1          # Tổng có trọng số
    a1 = sigmoid(z1)            # Hàm kích hoạt
    
    # Tầng 2: Hidden → Output
    z2 = a1 @ W2 + b2
    a2 = sigmoid(z2)            # Dự đoán cuối cùng
    
    # Tính Loss (MSE)
    loss = np.mean((y - a2) ** 2)
    losses.append(loss)
    
    # ========= BACKWARD PASS (Backpropagation) =========
    # Đây chính là Chain Rule!
    
    # Đạo hàm tầng output
    dL_da2 = -2 * (y - a2) / len(y)          # ∂Loss/∂a2
    da2_dz2 = sigmoid_dao_ham(z2)              # ∂a2/∂z2
    delta2 = dL_da2 * da2_dz2                  # Chain rule
    
    dL_dW2 = a1.T @ delta2                     # ∂Loss/∂W2
    dL_db2 = np.sum(delta2, axis=0, keepdims=True)
    
    # Đạo hàm tầng hidden (tiếp tục chain rule!)
    delta1 = (delta2 @ W2.T) * sigmoid_dao_ham(z1)
    dL_dW1 = X.T @ delta1
    dL_db1 = np.sum(delta1, axis=0, keepdims=True)
    
    # ========= CẬP NHẬT TRỌNG SỐ (Gradient Descent) =========
    W2 -= learning_rate * dL_dW2
    b2 -= learning_rate * dL_db2
    W1 -= learning_rate * dL_dW1
    b1 -= learning_rate * dL_db1
    
    if epoch % 1000 == 0:
        print(f"   Epoch {epoch:>5}: Loss = {loss:.6f}")

# --- Kết quả ---
print(f"\n✅ Sau huấn luyện:")
print(f"   Loss cuối: {losses[-1]:.6f}")
print(f"\n   Dự đoán:")
for xi, yi, pred in zip(X, y, a2):
    correct = "✅" if round(pred[0]) == yi[0] else "❌"
    print(f"   {xi} → {pred[0]:.4f} (mong đợi: {yi[0]}) {correct}")

# --- Trực quan hóa ---
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Loss curve
axes[0].plot(losses, 'b-', linewidth=1.5)
axes[0].set_title("📉 Loss giảm dần khi huấn luyện", fontsize=13, fontweight='bold')
axes[0].set_xlabel("Epoch"); axes[0].set_ylabel("Loss (MSE)")
axes[0].set_yscale('log')
axes[0].grid(True, alpha=0.3)

# Decision boundary
xx, yy = np.meshgrid(np.linspace(-0.5, 1.5, 200), np.linspace(-0.5, 1.5, 200))
grid = np.c_[xx.ravel(), yy.ravel()]
z1_grid = grid @ W1 + b1
a1_grid = sigmoid(z1_grid)
z2_grid = a1_grid @ W2 + b2
predictions = sigmoid(z2_grid).reshape(xx.shape)

axes[1].contourf(xx, yy, predictions, levels=50, cmap='RdYlBu_r', alpha=0.7)
axes[1].contour(xx, yy, predictions, levels=[0.5], colors='black', linewidths=2)

for xi, yi in zip(X, y):
    color = '#4CAF50' if yi[0] == 1 else '#f44336'
    marker = '★' if yi[0] == 1 else 'o'
    axes[1].plot(xi[0], xi[1], marker, color=color, markersize=15, 
                markeredgecolor='black', markeredgewidth=1.5)

axes[1].set_title("🧠 Decision Boundary (Ranh giới quyết định)", 
                  fontsize=13, fontweight='bold')
axes[1].set_xlabel("x₁"); axes[1].set_ylabel("x₂")
axes[1].grid(True, alpha=0.3)

plt.suptitle("🤖 Mạng Neural học hàm XOR — Được xây hoàn toàn bằng Đạo hàm!", 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong2_neural_network_xor.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 3 — Chain Rule, Backpropagation & Optimizer**
>
> 1. Nếu $h(x) = f(g(x))$, Chain Rule cho: $h'(x) = ?$
> 2. Backpropagation tính gradient theo hướng nào? (Từ input → output, hay output → input?)
> 3. Adam = sự kết hợp của 2 kỹ thuật nào?
> 4. Tại sao SGD (Stochastic GD) nhanh hơn Vanilla GD khi dữ liệu lớn?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. $h'(x) = f'(g(x)) \cdot g'(x)$ — "Đạo hàm ngoài × Đạo hàm trong"
> 2. Từ **output → input** (ngược lại!) — Đó là lý do gọi là "back" propagation. Tính gradient loss theo từng layer từ cuối về đầu
> 3. **Momentum** (nhớ hướng đi cũ, giảm dao động) + **RMSProp** (tự chỉnh learning rate cho từng chiều)
> 4. Vanilla GD tính gradient trên TOÀN BỘ dataset mỗi bước (rất chậm với triệu mẫu). SGD chỉ dùng 1 mẫu ngẫu nhiên → nhanh hơn nhiều, dù "ồn" hơn
>
> </details>
>
> **Tự đánh giá:** Đúng 4/4 → 🟢 Xuất sắc! | 2-3/4 → 🟡 Ổn | 0-1/4 → 🔴 Đọc lại Phần 4

---

## PHẦN 4C: FOURIER TRANSFORM — "PHÂN TÍCH TẦN SỐ"

### 4C.1 Ý tưởng cốt lõi

**Joseph Fourier** phát hiện: **Mọi tín hiệu đều là tổng các sóng sin/cos ở tần số khác nhau.**

Giống như ánh sáng trắng = tổng tất cả màu (tần số ánh sáng). Fourier Transform "tách" tín hiệu thành các thành phần tần số.

```
Tín hiệu gốc (miền thời gian)     →  FFT  →     Phổ tần số (miền tần số)
       ╱╲  ╱╲  ╱╲                              │ ▓▓
      ╱  ╲╱  ╲╱  ╲                             │ ▓▓░░
     ╱              ╲                           │ ▓▓░░▒▒
                                               ─┼──────── tần số
                                                │ 1Hz 3Hz 5Hz
```

### 4C.2 Tại sao AI cần Fourier?

| Ứng dụng | Cách dùng Fourier | Chương liên quan |
|----------|-------------------|-----------------|
| **Nén ảnh JPEG** | Ảnh → DCT → Bỏ tần số cao (chi tiết nhỏ) | Ch.1 (Ma trận) |
| **Xử lý âm thanh** | Giọng nói → FFT → Mel spectrogram → Input cho AI | Ch.3 (Features) |
| **Positional Encoding** | Transformer dùng sin/cos để mã hóa vị trí token | Ch.5 (Attention) |
| **Lọc nhiễu** | Ảnh nhiễu → FFT → Bỏ tần số cao → IFFT → Ảnh sạch | Ch.1 (SVD) |

### 4C.3 Positional Encoding trong Transformer

Transformer không có khái niệm "thứ tự" — các token được xử lý song song. Vậy làm sao biết "Tôi yêu Toán" ≠ "Toán yêu Tôi"?

**Positional Encoding** dùng sóng sin/cos:

$$PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d}}\right), \quad PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d}}\right)$$

Mỗi vị trí (pos) có "mã tần số" riêng biệt — giống như mỗi nốt nhạc có tần số riêng!

### 4C.4 Code Python: Fourier Transform & Positional Encoding

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# FOURIER TRANSFORM — PHÂN TÍCH TẦN SỐ
# =============================================

# 1. Tạo tín hiệu = tổng 3 sóng sin
t = np.linspace(0, 1, 1000)
signal = 3*np.sin(2*np.pi*5*t) + 1.5*np.sin(2*np.pi*15*t) + 0.5*np.sin(2*np.pi*30*t)
signal_noisy = signal + np.random.normal(0, 0.5, len(t))

# 2. FFT
fft_vals = np.fft.fft(signal_noisy)
freqs = np.fft.fftfreq(len(t), t[1]-t[0])
magnitude = np.abs(fft_vals[:len(t)//2])
freqs_pos = freqs[:len(t)//2]

fig, axes = plt.subplots(2, 2, figsize=(16, 10))

# Tín hiệu gốc
axes[0,0].plot(t, signal, 'b-', linewidth=1.5, label='Clean')
axes[0,0].plot(t, signal_noisy, 'gray', alpha=0.3, label='Noisy')
axes[0,0].set_title('📊 Tín hiệu (Miền thời gian)', fontsize=11, fontweight='bold')
axes[0,0].set_xlabel('Thời gian (s)'); axes[0,0].legend()
axes[0,0].grid(True, alpha=0.3)

# Phổ tần số
axes[0,1].plot(freqs_pos, magnitude, 'r-', linewidth=1.5)
axes[0,1].set_title('🌈 Phổ tần số (FFT)\n→ 3 đỉnh = 3 sóng sin!', fontsize=11, fontweight='bold')
axes[0,1].set_xlabel('Tần số (Hz)'); axes[0,1].set_ylabel('Amplitude')
axes[0,1].set_xlim(0, 50); axes[0,1].grid(True, alpha=0.3)

# 3. Positional Encoding (Transformer)
d_model = 64
max_len = 50

PE = np.zeros((max_len, d_model))
for pos in range(max_len):
    for i in range(0, d_model, 2):
        PE[pos, i] = np.sin(pos / (10000 ** (i / d_model)))
        PE[pos, i+1] = np.cos(pos / (10000 ** (i / d_model)))

axes[1,0].imshow(PE, aspect='auto', cmap='RdBu_r', interpolation='nearest')
axes[1,0].set_title('🤖 Positional Encoding (Transformer)\nMỗi hàng = mã vị trí 1 token', 
                     fontsize=11, fontweight='bold')
axes[1,0].set_xlabel('Chiều embedding (d)'); axes[1,0].set_ylabel('Vị trí token (pos)')

# So sánh PE các vị trí
for pos in [0, 5, 10, 25, 49]:
    axes[1,1].plot(PE[pos, :20], label=f'pos={pos}', linewidth=1.5)
axes[1,1].set_title('📐 PE cho từng vị trí\n(8 chiều đầu — mỗi vị trí có pattern riêng)', 
                     fontsize=11, fontweight='bold')
axes[1,1].set_xlabel('Chiều'); axes[1,1].legend(fontsize=8)
axes[1,1].grid(True, alpha=0.3)

plt.suptitle('🎵 Fourier Transform & Positional Encoding', fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong2_fourier.png', dpi=150, bbox_inches='tight')
plt.show()

print("💡 INSIGHTS:")
print("   1. FFT phân tích tín hiệu → tìm tần số ẩn (5Hz, 15Hz, 30Hz)")
print("   2. JPEG nén ảnh = bỏ tần số cao (chi tiết nhỏ)")  
print("   3. Transformer PE dùng sin/cos ở tần số khác nhau")
print("      → Mỗi vị trí có 'fingerprint' riêng!")
print("   4. PE cho phép Transformer biết THỨ TỰ từ")
```

> 📌 **Lưu ý cho giảng viên:** Fourier Transform là cầu nối giữa Giải tích → Signal Processing → NLP (Transformer). Sinh viên cần hiểu: (1) Mọi tín hiệu = tổng sóng sin/cos, (2) FFT chuyển từ miền thời gian → miền tần số, (3) Positional Encoding dùng ý tưởng này để encode vị trí. Không cần chứng minh công thức — hiểu intuition là đủ.

---

## PHẦN 5: 🎓 MINI PROJECT — Mô phỏng cách AI học

### Đề bài

Viết chương trình Python mô phỏng quá trình **AI tìm điểm thấp nhất** của một hàm số bất kỳ bằng Gradient Descent:

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# =============================================
# MÔ PHỎNG GRADIENT DESCENT ĐỘNG (Animation)
# =============================================

def ham_mat_mat(x):
    """Hàm có nhiều cực trị cục bộ (bề mặt gồ ghề)"""
    return x**4 - 5*x**2 + 4 + 2*np.sin(3*x)

def dao_ham(x):
    """Đạo hàm"""
    return 4*x**3 - 10*x + 6*np.cos(3*x)

# Gradient Descent
x = -2.5  # Điểm xuất phát
lr = 0.01
history_x = [x]
history_y = [ham_mat_mat(x)]

for _ in range(200):
    grad = dao_ham(x)
    x = x - lr * grad
    history_x.append(x)
    history_y.append(ham_mat_mat(x))

# Trực quan hóa quá trình
x_plot = np.linspace(-3, 3, 500)
y_plot = ham_mat_mat(x_plot)

fig, ax = plt.subplots(figsize=(12, 6))
ax.plot(x_plot, y_plot, 'b-', linewidth=2, label='f(x)', alpha=0.5)
ax.fill_between(x_plot, y_plot, min(y_plot) - 2, alpha=0.05, color='blue')

# Vẽ từng bước với màu gradient
n = len(history_x)
colors = plt.cm.hot(np.linspace(0.2, 0.8, n))

for i in range(n - 1):
    ax.plot(history_x[i], history_y[i], 'o', color=colors[i], 
            markersize=4, alpha=0.7)

# Điểm đầu và cuối
ax.plot(history_x[0], history_y[0], 'rs', markersize=15, label='Bắt đầu', zorder=5)
ax.plot(history_x[-1], history_y[-1], 'g★', markersize=20, label='Kết thúc', zorder=5)

ax.set_title("🎯 Gradient Descent: Viên bi lăn xuống đáy", fontsize=14, fontweight='bold')
ax.set_xlabel("x (vị trí)", fontsize=12)
ax.set_ylabel("f(x) (độ cao)", fontsize=12)
ax.legend(fontsize=11)
ax.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig('chuong2_mini_project.png', dpi=150, bbox_inches='tight')
plt.show()

print(f"\n📍 Kết quả: x = {history_x[-1]:.4f}, f(x) = {history_y[-1]:.4f}")
print(f"   Sau {len(history_x)} bước với learning rate α = {lr}")
```

---

## PHẦN 6: TÓM TẮT CHƯƠNG & BẢN ĐỒ TƯ DUY

### 🗺️ Bản đồ kiến thức Chương 2

```
GIẢI TÍCH — LA BÀN TỐI ƯU HÓA
├── ĐẠO HÀM (Derivative)
│   ├── = Độ dốc / Tốc độ thay đổi
│   ├── f'(x) > 0 → Lên dốc
│   ├── f'(x) < 0 → Xuống dốc
│   └── f'(x) = 0 → Cực trị (đỉnh/đáy)
│
├── GRADIENT DESCENT
│   ├── x_new = x_old - α · f'(x_old)
│   ├── Learning Rate (α) → Kích thước bước
│   └── Ứng dụng: Huấn luyện MỌI mô hình AI
│
├── ĐẠO HÀM RIÊNG & GRADIENT
│   ├── Partial Derivative → "Vặn từng nút một"
│   ├── Gradient ∇f → Hướng tăng nhanh nhất
│   └── GD nhiều chiều: Tìm đáy lòng chảo
│
└── CHAIN RULE & BACKPROPAGATION
    ├── Đạo hàm hàm lồng = Tích các đạo hàm
    ├── Backprop = Chain Rule trên mạng neural
    └── Mini Project: Neural Network from scratch 🧠
```

### ✅ Checklist kiến thức

- [ ] Giải thích đạo hàm bằng "leo núi trong sương mù"
- [ ] Viết Gradient Descent 1D và 2D bằng Python
- [ ] Hiểu Learning Rate ảnh hưởng thế nào
- [ ] Giải thích Chain Rule bằng "hiệu ứng domino"
- [ ] Hiểu Backpropagation là Chain Rule trên mạng neural
- [ ] Xây dựng neural network đơn giản học hàm XOR

---

## 📝 BÀI TẬP TỰ LUYỆN

### Bài 1: Tối ưu hàm chi phí ⭐
Một công ty sản xuất có hàm chi phí: $C(x) = 0.01x^3 - 0.6x^2 + 15x + 100$ (x = số sản phẩm/ngày). Dùng GD tìm số lượng sản xuất tối ưu (chi phí thấp nhất).

### Bài 2: So sánh Learning Rate ⭐⭐
Với hàm $f(x) = x^4 - 3x^3 + 2$, chạy GD với 5 learning rate khác nhau (0.001, 0.01, 0.05, 0.1, 0.5), mỗi cái 100 bước. Vẽ biểu đồ so sánh tốc độ hội tụ.

### Bài 3: Neural Network cho hàm AND, OR ⭐⭐⭐
Sửa lại neural network XOR để học hàm AND và OR. So sánh: Hàm nào cần ít epoch hơn? Giải thích tại sao.

### Bài 4: Gradient Descent 3D ⭐⭐⭐
Viết GD cho hàm Rosenbrock: $f(x,y) = (1-x)^2 + 100(y-x^2)^2$. Đây là hàm "thử thách" kinh điển trong tối ưu hóa. Trực quan hóa đường đi trên bề mặt 3D.

---

## 📌 LƯU Ý CHO GIẢNG VIÊN

### Chiến lược khơi gợi sự tò mò

1. **Bắt đầu bằng "thí nghiệm" trước khi dạy lý thuyết:**
   - Cho sinh viên chạy code GD trước → Quan sát viên bi lăn xuống dốc → Rồi mới hỏi "Nó hoạt động như thế nào?"
   - Hoạt động: Thay đổi learning rate → Dự đoán kết quả trước khi chạy

2. **Liên hệ với sản phẩm công nghệ:**
   - "Mỗi lần các em lướt TikTok, thuật toán đang chạy gradient descent để tối ưu feed cho em"
   - "Khi em nói 'OK Google', mạng neural dùng backpropagation hàng triệu lần để hiểu giọng em"

3. **Sai lầm phổ biến:**
   - Nhầm gradient (vector) với đạo hàm (scalar): Gradient = tập hợp tất cả đạo hàm riêng
   - Quên dấu trừ trong GD: $x_{new} = x_{old} - \alpha \nabla f$ (đi ngược gradient!)
   - Learning rate quá lớn → "nổ" (diverge): Cho sinh viên thử α = 10 để thấy

4. **Hoạt động nhóm:**
   - "Ai là viên bi?" — Sinh viên A nói vị trí, sinh viên B tính gradient, sinh viên C quyết định bước đi tiếp
   - Cuộc thi: Nhóm nào tìm được learning rate hội tụ nhanh nhất cho hàm Rosenbrock?

---

## ❓ CÂU HỎI THƯỜNG GẶP (FAQ)

### "Đạo hàm và Gradient khác nhau thế nào?"

Đạo hàm dùng cho hàm **1 biến**: $f'(x)$ → cho ra **1 số** (scalar). Gradient dùng cho hàm **nhiều biến**: $\nabla f(x, y) = (\frac{\partial f}{\partial x}, \frac{\partial f}{\partial y})$ → cho ra **1 vector**. Gradient là "tập hợp các đạo hàm riêng". Khi ai nói "tính gradient" trong ML, họ đang tính đạo hàm riêng theo **tất cả** tham số.

### "Em không hiểu Tích phân, có ảnh hưởng lớn đến việc học AI không?"

Trong thực hành ML, bạn **hiếm khi** phải tự tính tích phân bằng tay. Phần lớn được tính bằng `scipy.integrate.quad()` hoặc Monte Carlo. Nhưng bạn CẦN hiểu **ý nghĩa**: Tích phân = diện tích dưới đường cong = tổng tích lũy. Vì mỗi khi bạn thấy $P(a < X < b)$ trên phân phối liên tục, đó chính là tích phân!

### "Learning rate tốt nhất là bao nhiêu?"

Không có con số "tốt nhất" cố định — nó phải được **điều chỉnh** (tuning) cho từng bài toán. Nhưng một số heuristic phổ biến:
- Bắt đầu thử: $10^{-3}$ (0.001)
- Nếu loss giảm quá chậm → tăng lr
- Nếu loss dao động mạnh hoặc tăng → giảm lr
- Dùng **learning rate scheduler** (giảm dần lr theo epoch)
- Hoặc đơn giản: dùng **Adam** với lr=0.001 — hoạt động tốt cho ~80% bài toán!

### "Tại sao phải học Backpropagation khi PyTorch có `loss.backward()` tự động?"

Đúng, PyTorch tự tính gradient cho bạn. Nhưng khi model **không hội tụ**, bạn cần debug: "Gradient có bị triệt tiêu không?" (vanishing gradient), "Gradient có bị nổ không?" (exploding gradient), "Layer nào gây vấn đề?". Không hiểu Backpropagation = không debug được = bế tắc!

### "Adam luôn tốt nhất phải không?"

Không hẳn! Adam hội tụ **nhanh** nhưng đôi khi **generalize kém** hơn SGD + Momentum trên test set. Nhiều paper SOTA (ResNet, ViT) dùng SGD + Momentum + Cosine LR schedule. Quy tắc: Adam cho prototype nhanh, SGD cho kết quả production tốt nhất.

---

## ❌ SAI LẦM PHỔ BIẾN — ĐỪNG MẮC BẪY!

| # | ❌ Sai lầm | ✅ Sự thật |
|---|-----------|-----------|
| 1 | "Gradient và đạo hàm là một" | Đạo hàm = scalar (1 biến). Gradient = VECTOR đạo hàm riêng (nhiều biến) |
| 2 | "GD cập nhật: w = w + α·∇f" | SAI DẤU! Phải là w = w **−** α·∇f (đi NGƯỢC gradient) |
| 3 | "Learning rate lớn = hội tụ nhanh" | lr quá lớn → phát tán → KHÔNG hội tụ! Phải chọn vừa phải |
| 4 | "Adam luôn tốt nhất" | Không! SGD + momentum thường tốt hơn cho generalization. Adam tốt cho hội tụ nhanh |
| 5 | "Hàm lồi (convex) = hàm lõm" | Hoàn toàn NGƯỢC! Lồi: ∪ (bát úp). Lõm: ∩ (bát ngửa) |
| 6 | "Tích phân = ngược đạo hàm, vậy dễ" | Tính tay thì khó! Trong ML gần như luôn dùng **tích phân số** (numerical) |
| 7 | "Chain rule chỉ dùng cho hàm toán học" | Backpropagation trong neural network chính LÀ chain rule! Mỗi layer = 1 hàm. |

---

## 🔗 CẦU NỐI: TỪ NUMPY ĐẾN PYTORCH

| Phép toán | NumPy (đã học) | PyTorch (thực tế) |
|-----------|----------------|--------------------|
| **Gradient thủ công** | `gradient = 2*w` (tính tay) | ❌ Không cần! |
| **Gradient tự động** | ❌ Không có | `loss.backward()` → `w.grad` |
| **GD thủ công** | `w -= lr * gradient` | `optimizer.step()` |
| **Adam** | Code 20 dòng (Ch.2) | `torch.optim.Adam(params, lr=1e-3)` |
| **Cosine LR** | Code 5 dòng | `torch.optim.lr_scheduler.CosineAnnealingLR(...)` |
| **Tích phân** | `scipy.integrate.quad()` | `torch.trapezoid()` |

```python
import torch
import torch.optim as optim

# Ví dụ: GD trong PyTorch = 3 dòng!
w = torch.tensor([5.0], requires_grad=True)
optimizer = optim.Adam([w], lr=0.1)

for step in range(100):
    loss = (w - 3)**2         # Hàm loss: minimum tại w=3
    optimizer.zero_grad()     # Xóa gradient cũ
    loss.backward()           # Tự tính gradient (Chain Rule!)
    optimizer.step()          # Cập nhật w

print(f"w = {w.item():.4f}")  # ≈ 3.0000 ✅
# Toàn bộ GD, gradient, Adam — PyTorch lo hết!
```

> 💡 **Key insight:** Giải tích (đạo hàm, chain rule, GD) là **nền tảng lý thuyết**. PyTorch autograd **tự động hóa** nó. Hiểu lý thuyết → debug được khi model không hội tụ!

---

> 📖 **Chương tiếp theo:** [Chương 3: Xác suất Thống kê — Quy luật của sự Bất định](Chuong3_XacSuatThongKe.md) — *"Làm sao Gmail biết email nào là Spam? Đó là nhờ Định lý Bayes."*

