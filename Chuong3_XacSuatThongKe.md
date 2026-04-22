# CHƯƠNG 3: XÁC SUẤT THỐNG KÊ — QUY LUẬT CỦA SỰ BẤT ĐỊNH

## 📋 Metadata Chương

| Mục | Chi tiết |
|-----|---------|
| **Tên chương** | Chương 3: Xác suất Thống kê — Quy luật của sự Bất định |
| **Hook** | Mọi dự đoán của AI đều không phải "đúng" hay "sai" — mà là "95.3% chắc chắn". Đây là chương giải thích tại sao. |
| **Đối tượng** | Sinh viên CNTT, Data Science; người tự học muốn hiểu output của AI là gì |
| **Điều kiện tiên quyết** | Ch.1 §1 (Vector cơ bản). Ch.2 §2 (GD — để hiểu Cross-Entropy Loss). Biết khái niệm phân số, tỷ lệ %. |
| **Thời gian học** | ~50 phút (Track 🟢🟡) \| ~80 phút (Track 🔵) |
| **Mục tiêu đầu ra** | Sau chương này, người học có thể: |
| | • **Tính toán** xác suất cơ bản, kỳ vọng, phương sai, covariance bằng tay và code |
| | • **Giải thích** Định lý Bayes bằng ví dụ cụ thể: spam filter, chẩn đoán bệnh |
| | • **Phân biệt** 6 phân phối quan trọng và biết khi nào dùng cái nào |
| | • **Hiểu** Entropy và Cross-Entropy Loss — tại sao chúng là loss function cho classification |
| | • **Cài đặt** Naive Bayes spam filter từ đầu bằng Python |
| | • **Áp dụng** Linear Regression — mô hình tổng hợp Ch.1+2+3 |

---

## 📖 PROBABILITY & STATISTICS — THE RULES OF UNCERTAINTY


> *"Tại sao dự báo thời tiết nói 'xác suất mưa 80%' chứ không nói '100% sẽ mưa'? Vì thế giới thực đầy bất định — và Xác suất là ngôn ngữ để nói chuyện với sự bất định đó."*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 3: "Nói bằng xác suất"
>
> *N-42 đã biết cách điều chỉnh weight bằng GD. Nhưng output của nó là một con số thô — ví dụ 3.7. Điều đó nghĩa là gì? "Chắc chắn là mèo" hay "có lẽ là mèo"? N-42 cần học cách nói bằng XÁC SUẤT: thông qua hàm biến đổi tỷ lệ phần trăm (softmax), 3.7 sẽ được biến thành "97.3% là mèo". Và một hàm chấm điểm đo lường độ phân kì (cross-entropy) sẽ dạy nó tự đánh giá xem mình tự tin bao nhiêu và sai lệch như thế nào với thực tế.*

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: Gmail vs. Spam — Cuộc chiến không hồi kết

Mỗi ngày, Gmail nhận khoảng **15 tỷ email** trên toàn cầu. Trong đó, gần **50%** là spam — quảng cáo rác, lừa đảo, virus.

Nhưng hộp thư bạn luôn sạch. Tại sao?

Vì Gmail có một **bộ lọc thông minh** hoạt động dựa trên Xác suất. Khi một email đến, Gmail tự hỏi:

> *"Email này chứa từ 'miễn phí', 'trúng thưởng', 'click ngay'... Với những dấu hiệu này, xác suất nó là spam là bao nhiêu?"*

Nếu xác suất > 90% → vào thùng rác. Đó chính là **Định lý Bayes** — một trong những công cụ mạnh nhất trong Data Science.

> 💡 **Takeaway:** Xác suất không phải đánh bạc. Nó là cách **đưa ra quyết định thông minh** khi thông tin không đầy đủ.

---

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Khái niệm | Một câu tóm tắt | Tại sao cần cho AI |
|---|-----------|-----------------|---------------------|
| 1 | **Xác suất cơ bản** | Mức độ tin tưởng (0 → 1) | Mọi output AI đều là xác suất |
| 2 | **Phân phối** | "Hình dạng" của sự ngẫu nhiên | Hiểu dữ liệu, phát hiện anomaly |
| 3 | **Kỳ vọng & Phương sai** | Trung bình & Độ phân tán | Đánh giá model, risk assessment |
| 4 | **Covariance & Correlation** | Đo mối quan hệ giữa 2 biến | PCA, Feature selection |
| 5 | **Định lý Bayes** | Cập nhật niềm tin từ dữ liệu | Spam filter, Chẩn đoán, NLP |
| 6 | **Entropy & Cross-Entropy** | Đo lượng thông tin | Loss function #1 cho classification |
| 7 | **Linear Regression** | Dự đoán giá trị liên tục | ML model #1 — Tổng hợp Ch.1-3 |

> 📋 **Prerequisites:** Ch.1 §1-2 (Vector, Dot Product). Biết khái niệm phân số, tỷ lệ %.
>
> ⏱️ **Thời gian đọc:** ~50 phút (Track A — Developer) | ~80 phút (Track B — Researcher)

---

## 🗺️ BẠN ĐANG Ở ĐÂY

```
  Ch.0 ━━▶ Ch.1 ĐSTT ━━▶ Ch.2 Giải tích ━━▶ 📍 Ch.3 XÁC SUẤT TK ━━▶ Ch.4 Toán RR ━━▶ ...
  (✅ Done)  (✅ Done)      (✅ Done)             BẠN ĐANG Ở ĐÂY       (Tiếp theo)
```

**Phần này cần học trước:** Ch.1 §1 (Vector cơ bản — để hiểu "vector xác suất"), Ch.2 §2 (GD — để hiểu tại sao Cross-Entropy là loss)
**Chương này mở khóa:** Ch.5 (Softmax, Classification), Ch.7 (Hypothesis Testing, Confidence Interval)
**Bạn sẽ cần lại kiến thức này khi:** Chọn loss function, đánh giá model, hiểu output Softmax, làm A/B testing, đọc paper ML.

---

## 📦 ÔN NHANH TOÁN PHỔ THÔNG

> *Phần này dành cho bạn nào cảm thấy chưa vững nền tảng. Nếu bạn tự tin, hãy bỏ qua! Không xấu hổ khi ôn lại — giáo sư cũng phải tra công thức mỗi ngày.*

<details>
<summary>🎲 <b>Ôn nhanh 1: Phân số và Tỷ lệ — "Chia phần bánh"</b> (Bấm để mở)</summary>

Phân số = "một phần của toàn bộ". Tỷ lệ % = phân số × 100.

- 1/4 = 0.25 = 25% → "1 trong 4 phần"
- 3/10 = 0.3 = 30% → "3 trong 10"

**Cộng phân số:** $\frac{1}{4} + \frac{1}{4} = \frac{2}{4} = \frac{1}{2}$

**Nhân phân số:** $\frac{1}{2} \times \frac{1}{3} = \frac{1}{6}$ → "Nửa của một phần ba"

> 💡 Xác suất chính là "phân số sự kiện thuận lợi / tổng sự kiện". Bạn sẽ dùng ở **Phần 1**.

</details>

<details>
<summary>📊 <b>Ôn nhanh 2: Trung bình cộng — "Con số đại diện"</b></summary>

Trung bình = Tổng / Số phần tử:

$$\bar{x} = \frac{x_1 + x_2 + \ldots + x_n}{n}$$

Ví dụ: Điểm thi 5 bạn: 7, 8, 6, 9, 5 → Trung bình = (7+8+6+9+5)/5 = 7.0

**Trung bình có trọng số** (giống phép nhân ma trận ở Ch.1!):

$$\bar{x}_w = w_1 x_1 + w_2 x_2 + \ldots$$

Ví dụ: Điểm TB = 0.4×Toán + 0.3×Lý + 0.3×Tin

> 💡 **Kỳ vọng** ở Phần 2 chính là "trung bình có trọng số" — trọng số là xác suất!

</details>

<details>
<summary>📏 <b>Ôn nhanh 3: Phép đếm — "Bao nhiêu cách?"</b></summary>

**Quy tắc nhân:** Nếu bước 1 có $a$ cách, bước 2 có $b$ cách → Tổng: $a \times b$ cách.

Ví dụ: 3 áo × 4 quần = 12 outfit khác nhau.

**Quy tắc cộng:** Nếu chọn A HOẶC B (không đồng thời) → Tổng: $a + b$ cách.

Ví dụ: Đi xe buýt (5 tuyến) HOẶC taxi (3 app) = 8 cách đi.

> 💡 Bạn sẽ dùng quy tắc nhân ở **Phần 1** (biến cố độc lập: P(A ∩ B) = P(A) × P(B)).

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

## PHẦN 1: XÁC SUẤT CƠ BẢN — "MỨC ĐỘ TIN TƯỞNG"

### 🟢 1.1 Xác suất là gì?

Xác suất là con số từ **0 đến 1** biểu thị **mức độ tin tưởng** vào một sự kiện:

| Xác suất | Ý nghĩa | Ví dụ |
|----------|---------|-------|
| 0 | Không thể xảy ra | Mặt trời mọc hướng Tây |
| 0.01 | Gần như không thể | Trúng Vietlott |
| 0.5 | 50/50, không chắc | Tung đồng xu |
| 0.9 | Gần chắc chắn | Trời mưa khi mây đen kín |
| 1 | Chắc chắn | $1 + 1 = 2$ |

$$P(A) = \frac{\text{Số trường hợp thuận lợi}}{\text{Tổng số trường hợp}}$$

#### 🎯 Liên tưởng: Xác suất = "Lá phiếu bầu"

Tưởng tượng bạn hỏi **100 vũ trụ song song** cùng một câu hỏi: "Ngày mai có mưa không?"

- 80 vũ trụ trả lời "Có" → $P(\text{mưa}) = 0.8$
- 20 vũ trụ trả lời "Không" → $P(\text{không mưa}) = 0.2$

Xác suất 80% = trong 80 "thế giới" thì sự kiện xảy ra, trong 20 thế giới thì không.

### 🟡 1.2 Quy tắc xác suất cơ bản

#### 🟡 Quy tắc cộng — "Hoặc... Hoặc..."

$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

*"Xác suất trúng xổ số **hoặc** được tăng lương = XS trúng + XS tăng lương − XS cả hai cùng xảy ra"*

(Trừ đi phần giao vì đã đếm 2 lần)

#### 🟡 Quy tắc nhân — "Và... Và..."

$$P(A \cap B) = P(A) \cdot P(B|A)$$

*"Xác suất mưa **và** kẹt xe = XS mưa × XS kẹt xe khi trời mưa"*

> 💡 $P(B|A)$ đọc là "Xác suất B **khi biết** A đã xảy ra" — gọi là **xác suất có điều kiện**.

---

> 🌱 **VÍ DỤ VỠ LÒNG — Tính xác suất bằng tay (30 giây)**
>
> Tung 1 xúc xắc. Xác suất ra số chẵn?
>
> - Tổng: 6 mặt {1, 2, 3, 4, 5, 6}
> - Thuận lợi: 3 mặt chẵn {2, 4, 6}
> - P(chẵn) = 3/6 = **1/2 = 50%**
>
> Thử thêm: Xác suất ra số > 4? Thuận lợi: {5, 6} → P = 2/6 = **1/3 ≈ 33%**

---

> ⚠️ **SAI LẦM PHỔ BIẾN:** Nhiều bạn nghĩ "xác suất 50% = chắc chắn xảy ra 1 trong 2 lần". SAI! P=50% nghĩa là **trung bình** trong nhiều lần thử, khoảng nửa số lần sẽ xảy ra. Tung xu 2 lần, hoàn toàn có thể ra 2 lần ngửa!

#### 🟡 Biến cố độc lập

Nếu A và B không ảnh hưởng nhau (tung 2 đồng xu khác nhau):

$$P(A \cap B) = P(A) \cdot P(B)$$

### 🔵 1.3 Code Python: Mô phỏng xác suất

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. MÔ PHỎNG TUNG ĐỒNG XU
# =============================================

np.random.seed(42)

def mo_phong_tung_xu(n_lan):
    """Tung đồng xu n lần, đếm số lần mặt ngửa"""
    ket_qua = np.random.choice(['Ngửa', 'Sấp'], size=n_lan)
    so_ngua = np.sum(ket_qua == 'Ngửa')
    return so_ngua / n_lan

# Tung với số lần tăng dần
so_lan_tung = [10, 50, 100, 500, 1000, 5000, 10000, 50000]
ty_le_ngua = [mo_phong_tung_xu(n) for n in so_lan_tung]

print("🎲 Mô phỏng tung đồng xu:")
print(f"{'Số lần tung':>15} | {'Tỷ lệ Ngửa':>12} | {'Sai số':>10}")
print("-" * 45)
for n, tl in zip(so_lan_tung, ty_le_ngua):
    print(f"{n:>15,} | {tl:>11.4f}  | {abs(tl - 0.5):>9.4f}")

# =============================================
# 2. MÔ PHỎNG BÀI TOÁN SINH NHẬT
# =============================================

def xac_suat_trung_sinh_nhat(n_nguoi, n_simulations=10000):
    """Xác suất có ít nhất 2 người trùng ngày sinh trong nhóm n người"""
    count = 0
    for _ in range(n_simulations):
        sinh_nhat = np.random.randint(1, 366, size=n_nguoi)
        if len(set(sinh_nhat)) < n_nguoi:  # Có trùng
            count += 1
    return count / n_simulations

print("\n🎂 Nghịch lý Sinh nhật (Birthday Paradox):")
print("   Cần bao nhiêu người để XS trùng sinh nhật > 50%?")

so_nguoi_list = [5, 10, 15, 20, 23, 30, 40, 50, 70]
xs_list = [xac_suat_trung_sinh_nhat(n) for n in so_nguoi_list]

for n, xs in zip(so_nguoi_list, xs_list):
    marker = "← 50%!" if 0.48 < xs < 0.55 else ""
    print(f"   {n:>3} người: {xs:.1%} {marker}")

# =============================================
# 3. TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# --- Hình 1: Quy luật số lớn (tung xu) ---
ax1 = axes[0]
# Mô phỏng chi tiết
n_max = 5000
outcomes = np.random.choice([0, 1], size=n_max)
running_avg = np.cumsum(outcomes) / np.arange(1, n_max + 1)

ax1.plot(range(1, n_max + 1), running_avg, color='#2196F3', linewidth=1, alpha=0.7)
ax1.axhline(y=0.5, color='red', linestyle='--', linewidth=2, label='P(Ngửa) = 0.5 (lý thuyết)')
ax1.fill_between(range(1, n_max + 1), 0.5, running_avg, alpha=0.1, color='#2196F3')

ax1.set_title("📊 Quy luật Số lớn\nCàng tung nhiều, tỷ lệ càng gần 50%", 
              fontsize=13, fontweight='bold')
ax1.set_xlabel("Số lần tung", fontsize=11)
ax1.set_ylabel("Tỷ lệ mặt Ngửa", fontsize=11)
ax1.set_xscale('log')
ax1.set_ylim(0.2, 0.8)
ax1.legend(fontsize=11)
ax1.grid(True, alpha=0.3)

# --- Hình 2: Birthday Paradox ---
ax2 = axes[1]
ax2.bar(so_nguoi_list, xs_list, color=['#90CAF9' if x < 0.5 else '#4CAF50' for x in xs_list],
        edgecolor='white', width=3)
ax2.axhline(y=0.5, color='red', linestyle='--', linewidth=2, label='50%')

for n, xs in zip(so_nguoi_list, xs_list):
    ax2.text(n, xs + 0.02, f'{xs:.0%}', ha='center', fontsize=9, fontweight='bold')

ax2.set_title("🎂 Nghịch lý Sinh nhật\nChỉ cần 23 người để XS trùng > 50%!", 
              fontsize=13, fontweight='bold')
ax2.set_xlabel("Số người trong phòng", fontsize=11)
ax2.set_ylabel("Xác suất trùng sinh nhật", fontsize=11)
ax2.legend(fontsize=11)
ax2.grid(axis='y', alpha=0.3)

plt.tight_layout()
plt.savefig('chuong3_xac_suat_co_ban.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Nghịch lý Sinh nhật luôn gây ngạc nhiên. Hãy hỏi cả lớp: "Theo em, cần bao nhiêu người?" trước khi chạy code. Hầu hết sẽ đoán >100 người — kết quả 23 sẽ tạo "khoảnh khắc aha" mạnh mẽ.

---

> ✅ **CHECKPOINT 1 — Xác suất Cơ bản**
>
> 1. Tung 2 đồng xu. Xác suất cả 2 ngửa? (Gợi ý: 2 biến cố độc lập)
> 2. Trong lớp 40 người, xác suất có 2 người trùng sinh nhật khoảng bao nhiêu? (A: ~20%, B: ~50%, C: ~89%)
> 3. Tung xúc xắc, xác suất ra số chẵn HOẶC số > 4 bằng bao nhiêu?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. P(ngửa) × P(ngửa) = 1/2 × 1/2 = **1/4 = 25%** (biến cố độc lập → nhân)
> 2. **C: ~89%!** Chỉ cần 23 người đã > 50%. 40 người thì gần chắc chắn trùng. Trực giác thường sai ở bài này!
> 3. Chẵn = {2,4,6}, > 4 = {5,6}. Hợp = {2,4,5,6} = 4/6 = **2/3 ≈ 67%** (nhớ trừ giao: {6} đã đếm 2 lần)
>
> </details>
>
> **Tự đánh giá:** Đúng 3/3 → 🟢 Tuyệt! | 2/3 → 🟡 Ổn | 0-1/3 → 🔴 Đọc lại Phần 1

---

## PHẦN 2: PHÂN PHỐI XÁC SUẤT — HÌNH DẠNG CỦA SỰ NGẪU NHIÊN

### 🟢 2.1 Biến ngẫu nhiên

**Biến ngẫu nhiên** ($X$) = Đại lượng có giá trị phụ thuộc vào yếu tố ngẫu nhiên.

Ví dụ:
- $X$ = Số chấm khi tung xúc xắc → $X \in \{1, 2, 3, 4, 5, 6\}$ (rời rạc)
- $X$ = Chiều cao sinh viên → $X \in [1.5m, 2.0m]$ (liên tục)

### 🟡 2.2 Phân phối quan trọng nhất: Phân phối Chuẩn (Normal/Gaussian)

$$f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

Đừng sợ công thức! Hãy nhớ:
- $\mu$ (mu) = **trung bình** (đỉnh chuông nằm ở đâu)
- $\sigma$ (sigma) = **độ lệch chuẩn** (chuông rộng hay hẹp)

#### 🎯 Liên tưởng: Phân phối chuẩn ở KHẮP NƠI

| Ví dụ | μ (trung bình) | σ (độ phân tán) |
|-------|----------------|-----------------|
| Chiều cao nam VN | 168 cm | 6 cm |
| Điểm thi THPT | 5.0/10 | 1.5 |
| Nhiệt độ Hà Nội tháng 7 | 32°C | 3°C |
| Thời gian phản hồi server | 200ms | 50ms |

**Quy tắc 68-95-99.7:**
- 68% dữ liệu nằm trong $\mu \pm 1\sigma$ 
- 95% nằm trong $\mu \pm 2\sigma$
- 99.7% nằm trong $\mu \pm 3\sigma$

→ Nếu server phản hồi > $200 + 3 \times 50 = 350$ms → Rất bất thường! Có thể đang bị tấn công DDoS.

### 🟡 2.3 Kỳ vọng và Phương sai

> 🌱 **VÍ DỤ VỠ LÒNG — Kỳ vọng bằng tay (30 giây)**
>
> Trò chơi: Tung xu, ngửa → thắng 10k, sấp → mất 5k. Chơi có lời không?
>
> $E = P(\text{ngửa}) \times 10 + P(\text{sấp}) \times (-5) = 0.5 \times 10 + 0.5 \times (-5) = 5 - 2.5 = \textbf{+2.5k}$
>
> → **Trung bình mỗi lần chơi bạn lời 2.5k!** Đây là cách casino tính toán — họ luôn thiết kế E < 0 cho người chơi.

| Khái niệm | Ý nghĩa | Công thức |
|-----------|---------|-----------|
| **Kỳ vọng** $E[X]$ | Giá trị trung bình "mong đợi" | $E[X] = \sum x_i \cdot P(x_i)$ |
| **Phương sai** $Var(X)$ | Mức độ "phân tán" quanh kỳ vọng | $Var(X) = E[(X - \mu)^2]$ |
| **Độ lệch chuẩn** $\sigma$ | Phương sai nhưng "dễ hiểu hơn" | $\sigma = \sqrt{Var(X)}$ |

#### 🎯 Liên tưởng: Thu nhập 2 người

- Người A: Lương cố định 15 triệu/tháng → $E=15, \sigma=0$ (ổn định)
- Người B: Freelancer, tháng 5 triệu, tháng 30 triệu → $E=15, \sigma=12$ (bấp bênh)

Cùng kỳ vọng 15 triệu, nhưng **phương sai** khác nhau hoàn toàn!

### 🔵 2.4 Code Python: Khám phá các phân phối

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

# =============================================
# CÁC PHÂN PHỐI QUAN TRỌNG TRONG AI
# =============================================

fig, axes = plt.subplots(2, 3, figsize=(18, 10))

# --- 1. Phân phối Chuẩn (Normal) ---
ax = axes[0, 0]
x = np.linspace(-5, 5, 300)
for mu, sigma, label, color in [(0, 1, 'μ=0, σ=1 (chuẩn)', '#2196F3'), 
                                  (0, 2, 'μ=0, σ=2 (rộng)', '#FF5722'),
                                  (2, 0.5, 'μ=2, σ=0.5 (hẹp)', '#4CAF50')]:
    y = stats.norm.pdf(x, mu, sigma)
    ax.plot(x, y, linewidth=2, label=label, color=color)
    ax.fill_between(x, y, alpha=0.1, color=color)

ax.set_title("📐 Phân phối Chuẩn\n(Chiều cao, Điểm thi)", fontsize=11, fontweight='bold')
ax.legend(fontsize=8); ax.grid(True, alpha=0.3)

# --- 2. Phân phối Bernoulli ---
ax = axes[0, 1]
p = 0.7
ax.bar([0, 1], [1-p, p], color=['#f44336', '#4CAF50'], 
       tick_label=['Thất bại (0)', 'Thành công (1)'], width=0.4, edgecolor='white')
ax.set_title(f"🎯 Phân phối Bernoulli\n(Tung xu, Click quảng cáo, p={p})", 
             fontsize=11, fontweight='bold')
ax.set_ylabel("Xác suất"); ax.grid(axis='y', alpha=0.3)

# --- 3. Phân phối Binomial ---
ax = axes[0, 2]
n, p = 20, 0.3
k = np.arange(0, n+1)
prob = stats.binom.pmf(k, n, p)
ax.bar(k, prob, color='#9C27B0', alpha=0.7, edgecolor='white')
ax.axvline(x=n*p, color='red', linestyle='--', linewidth=2, label=f'E[X] = np = {n*p}')
ax.set_title(f"📊 Phân phối Binomial\n(Số lần thành công / {n} lần thử, p={p})", 
             fontsize=11, fontweight='bold')
ax.legend(fontsize=9); ax.grid(axis='y', alpha=0.3)

# --- 4. Phân phối Poisson ---
ax = axes[1, 0]
for lam, color in [(2, '#FF5722'), (5, '#2196F3'), (10, '#4CAF50')]:
    k = np.arange(0, 25)
    prob = stats.poisson.pmf(k, lam)
    ax.plot(k, prob, 'o-', color=color, markersize=5, label=f'λ = {lam}')

ax.set_title("🔔 Phân phối Poisson\n(Số email/giờ, Số bug/tuần)", 
             fontsize=11, fontweight='bold')
ax.legend(fontsize=9); ax.grid(True, alpha=0.3)

# --- 5. Phân phối Đều (Uniform) ---
ax = axes[1, 1]
x = np.linspace(-1, 6, 300)
y = stats.uniform.pdf(x, loc=1, scale=4)  # Uniform(1, 5)
ax.plot(x, y, 'b-', linewidth=2)
ax.fill_between(x, y, alpha=0.2, color='#2196F3')
ax.set_title("📏 Phân phối Đều (Uniform)\n(Random number, Quay số)", 
             fontsize=11, fontweight='bold')
ax.grid(True, alpha=0.3)

# --- 6. Phân phối Mũ (Exponential) ---
ax = axes[1, 2]
x = np.linspace(0, 8, 300)
for lam, color in [(0.5, '#FF5722'), (1.0, '#2196F3'), (2.0, '#4CAF50')]:
    y = stats.expon.pdf(x, scale=1/lam)
    ax.plot(x, y, linewidth=2, color=color, label=f'λ = {lam}')
    ax.fill_between(x, y, alpha=0.1, color=color)

ax.set_title("📉 Phân phối Mũ (Exponential)\n(Thời gian chờ, Tuổi thọ pin)", 
             fontsize=11, fontweight='bold')
ax.legend(fontsize=9); ax.grid(True, alpha=0.3)

plt.suptitle("🎲 6 Phân phối Xác suất quan trọng nhất trong CNTT", 
             fontsize=15, fontweight='bold', y=1.02)
plt.tight_layout()
plt.savefig('chuong3_phan_phoi.png', dpi=150, bbox_inches='tight')
plt.show()

# In tóm tắt
print("\n📋 Tóm tắt các phân phối:")
print("="*60)
data = [
    ("Normal(μ,σ)", "Chiều cao, Điểm thi, Nhiễu", "Liên tục"),
    ("Bernoulli(p)", "Tung xu, Click/No-click", "0 hoặc 1"),
    ("Binomial(n,p)", "Số thành công/n lần", "Rời rạc"),
    ("Poisson(λ)", "Số sự kiện/đơn vị thời gian", "Rời rạc"),
    ("Uniform(a,b)", "Random, Quay số", "Liên tục"),
    ("Exponential(λ)", "Thời gian chờ", "Liên tục"),
]
for name, app, loai in data:
    print(f"   {name:>20} | {app:<35} | {loai}")
```

> 📌 **Lưu ý cho giảng viên:** Hỏi sinh viên: "Em nghĩ số lượng tin nhắn em nhận mỗi giờ tuân theo phân phối nào?" → Poisson. "Chiều cao em?" → Normal. Giúp sinh viên "nhìn thấy" phân phối trong đời sống.

---

### 🟡 2B.1 Covariance — "Hai biến có đi cùng nhau?"

**Covariance** (hiệp phương sai) đo mức độ hai biến **thay đổi cùng nhau**:

$$Cov(X, Y) = E[(X - \mu_X)(Y - \mu_Y)] = \frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})$$

| Cov(X,Y) | Ý nghĩa | Ví dụ |
|----------|---------|-------|
| **> 0** | X tăng → Y tăng | Chiều cao × Cân nặng ↑↑ |
| **< 0** | X tăng → Y giảm | Giá dầu × CP hàng không ↑↓ |
| **≈ 0** | Không liên quan | Số giày × Điểm Toán |

> ⚠️ **SAI LẦM PHỔ BIẾN:** Correlation **KHÔNG** nghĩa là nhân quả! Kem bán chạy ↔ Số vụ đuối nước tăng → ρ > 0 nhưng kem **KHÔNG gây** đuối nước. Cả hai đều do mùa hè (yếu tố ẩn). Trong ML, đây gọi là **confounding variable**.

### 🟡 2B.2 Correlation — Covariance đã "chuẩn hóa"

$$\rho_{XY} = \frac{Cov(X,Y)}{\sigma_X \cdot \sigma_Y} \in [-1, 1]$$

- $\rho = +1$: Hoàn toàn cùng hướng (X tăng 1% → Y tăng 1%)
- $\rho = -1$: Hoàn toàn ngược hướng
- $\rho = 0$: Không tương quan tuyến tính

#### 🎯 Ví dụ tài chính: Đa dạng hóa đầu tư 💰

| Cặp cổ phiếu | ρ | Ý nghĩa đầu tư |
|-------------|---|-----------------------|
| VIC × VHM | +0.85 | Cùng ngành BĐS → Rủi ro tập trung ⚠️ |
| VNM × PNJ | +0.15 | Ít liên quan → Tốt để đa dạng |
| PLX × HVN | -0.40 | Ngược nhau (dầu-hàng không) → Tuyệt vời! ✅ |

> 💡 **Quy tắc vàng:** Mua các cổ phiếu có $\rho < 0$ với nhau → Khi 1 cái lỗ, cái kia lãi → Giảm rủi ro tổng thể!

### 🟡 2B.3 Ma trận Covariance — Cầu nối với PCA

Khi có nhiều biến, ta xây dựng **ma trận Covariance**:

$$C = \begin{pmatrix} Var(X_1) & Cov(X_1, X_2) & \cdots \\ Cov(X_2, X_1) & Var(X_2) & \cdots \\ \vdots & \vdots & \ddots \end{pmatrix}$$

→ **Eigenvector** của ma trận này chính là **Principal Components** (Chương 1 - PCA)!

### 🔵 2B.4 Code Python: Covariance trong thực tế

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. COVARIANCE & CORRELATION TRỰC QUAN
# =============================================

np.random.seed(42)
n = 200

# Tương quan dương: Chiều cao - Cân nặng
chieu_cao = np.random.normal(170, 8, n)
can_nang = 0.6 * chieu_cao + np.random.normal(0, 5, n)

# Tương quan âm: Giá dầu - CP hàng không  
gia_dau = np.random.normal(80, 10, n)
cp_hangkhong = -0.5 * gia_dau + np.random.normal(90, 8, n)

# Không tương quan: Số giày - Điểm Toán
so_giay = np.random.normal(40, 3, n)
diem_toan = np.random.normal(7, 1.5, n)

fig, axes = plt.subplots(1, 3, figsize=(18, 5))

datasets = [
    ("Chiều cao vs Cân nặng\n(ρ > 0: Cùng tăng)", chieu_cao, can_nang, '#4CAF50'),
    ("Giá dầu vs CP Hàng không\n(ρ < 0: Ngược nhau)", gia_dau, cp_hangkhong, '#f44336'),
    ("Số giày vs Điểm Toán\n(ρ ≈ 0: Không liên quan)", so_giay, diem_toan, '#9C27B0'),
]

for ax, (title, x, y, color) in zip(axes, datasets):
    ax.scatter(x, y, alpha=0.4, color=color, s=20)
    
    # Tính và hiển thị
    cov = np.cov(x, y)[0, 1]
    corr = np.corrcoef(x, y)[0, 1]
    
    # Đường hồi quy
    z = np.polyfit(x, y, 1)
    p = np.poly1d(z)
    ax.plot(sorted(x), p(sorted(x)), '--', color='black', linewidth=2, alpha=0.5)
    
    ax.set_title(f"{title}\nCov = {cov:.1f}, ρ = {corr:.2f}", 
                fontsize=11, fontweight='bold')
    ax.grid(True, alpha=0.3)

plt.suptitle('📊 Covariance & Correlation — Đo mối quan hệ giữa 2 biến', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong3_covariance.png', dpi=150, bbox_inches='tight')
plt.show()

print("💡 ỨNG DỤNG:")
print("   ML: Feature có corr cao → có thể bỏ 1 (PCA, Feature Selection)")
print("   Tài chính: Đa dạng hóa = chọn CP có corr âm với nhau")
```

> 📌 **Lưu ý cho giảng viên:** Covariance là cầu nối quan trọng nhất giữa Chương 1 (ĐSTT) và Chương 3 (XSTK). Hãy nhấn mạnh: "Ma trận Covariance là ma trận đối xứng, và eigenvector của nó chính là PCA!"

---

> ✅ **CHECKPOINT 2 — Phân phối, Kỳ vọng & Covariance**
>
> 1. Chiều cao nam Việt Nam tuân theo phân phối gì? Nêu ý nghĩa μ và σ.
> 2. Tung xúc xắc, kỳ vọng E[X] = ? (Gợi ý: Trung bình có trọng số với P = 1/6 mỗi mặt)
> 3. Nếu Cov(X, Y) < 0, X tăng thì Y sẽ... ?
> 4. Tại sao "Kem bán chạy → đuối nước tăng" KHÔNG phải là nhân quả?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Phân phối Chuẩn (Normal)** với μ ≈ 168cm (trung bình), σ ≈ 6cm (độ phân tán). 68% nam cao 162-174cm.
> 2. E[X] = (1+2+3+4+5+6)/6 = **3.5** — Tung xúc xắc rất nhiều lần, trung bình sẽ là 3.5!
> 3. Y sẽ **giảm** (tương quan âm). Ví dụ: Giá dầu tăng → CP hàng không giảm.
> 4. Cả hai đều do **mùa hè** (confounding variable). Correlation ≠ Causation!
>
> </details>
>
> **Tự đánh giá:** Đúng 4/4 → 🟢 Xuất sắc! | 2-3/4 → 🟡 Ổn | 0-1/4 → 🔴 Đọc lại Phần 2

---

## PHẦN 3: ĐỊNH LÝ BAYES — VŨ KHÍ BÍ MẬT CỦA AI

### 🟢 3.1 Xác suất có điều kiện

$P(A|B)$ = "Xác suất A xảy ra, **khi biết** B đã xảy ra"

Ví dụ: $P(\text{kẹt xe} | \text{trời mưa})$ = "Xác suất kẹt xe KHHI biết trời đang mưa"

Rõ ràng: $P(\text{kẹt xe} | \text{trời mưa}) > P(\text{kẹt xe})$ → Trời mưa làm tăng xác suất kẹt xe.

### 🟡 3.2 Định lý Bayes

$$P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$$

Đây là công thức cho phép ta **đảo ngược** xác suất có điều kiện.

#### 🎯 Giải thích từng phần

Giả sử bạn muốn biết: *"Email này có phải spam không?"* khi thấy nó chứa từ "miễn phí".

$$P(\text{spam} | \text{"miễn phí"}) = \frac{P(\text{"miễn phí"} | \text{spam}) \cdot P(\text{spam})}{P(\text{"miễn phí"})}$$

| Thành phần | Ý nghĩa | Giá trị ví dụ |
|-----------|---------|--------------|
| $P(\text{spam})$ | Xác suất tiên nghiệm (prior): Bao nhiêu % email là spam? | 40% |
| $P(\text{"miễn phí"} \| \text{spam})$ | Likelihood: Trong spam, bao nhiêu % chứa "miễn phí"? | 80% |
| $P(\text{"miễn phí"})$ | Evidence: Tổng xác suất thấy "miễn phí" | ? |
| $P(\text{spam} \| \text{"miễn phí"})$ | Posterior: **Sau khi thấy "miễn phí"**, XS spam là? | **Cần tính!** |

Tính $P(\text{"miễn phí"})$:
$$P(\text{"miễn phí"}) = P(\text{"miễn phí"} | \text{spam}) \cdot P(\text{spam}) + P(\text{"miễn phí"} | \text{không spam}) \cdot P(\text{không spam})$$
$$= 0.8 \times 0.4 + 0.1 \times 0.6 = 0.32 + 0.06 = 0.38$$

$$P(\text{spam} | \text{"miễn phí"}) = \frac{0.8 \times 0.4}{0.38} = \frac{0.32}{0.38} \approx 0.842$$

→ Email chứa "miễn phí" có **84.2% xác suất là spam!**

#### 🎯 Liên tưởng: Bác sĩ chẩn đoán bệnh

> 🌱 **VÍ DỤ VỠ LÒNG — Tính Bayes bằng tay (30 giây)**
>
> Giả sử có 1000 email.
> - Spam chiếm 40% → Có **400** email spam, **600** email thường.
> - Trong spam, 80% chứa "miễn phí" → $400 \times 0.8 = \textbf{320}$ email spam có từ này.
> - Trong email thường, 10% chứa "miễn phí" → $600 \times 0.1 = \textbf{60}$ email thường có từ này.
>
> Tổng số email chứa "miễn phí" = $320 + 60 = 380$.
> Thấy từ "miễn phí", tỷ lệ spam là bao nhiêu? = $320 / 380 \approx \textbf{84.2\%}$!

Xét nghiệm COVID cho kết quả dương tính. Bạn có chắc mắc bệnh?

- Tỷ lệ nhiễm trong dân số: $P(\text{bệnh}) = 1\%$
- Độ nhạy xét nghiệm: $P(\text{dương} | \text{bệnh}) = 99\%$
- Tỷ lệ dương tính giả: $P(\text{dương} | \text{không bệnh}) = 5\%$

$$P(\text{bệnh} | \text{dương}) = \frac{0.99 \times 0.01}{0.99 \times 0.01 + 0.05 \times 0.99} = \frac{0.0099}{0.0099 + 0.0495} = 16.7\%$$

**Bất ngờ chưa?** Dù xét nghiệm 99% chính xác, nhưng vì tỷ lệ bệnh thấp (1%), nên khi dương tính, bạn chỉ có 16.7% thật sự mắc bệnh!

→ Đây là lý do bác sĩ thường yêu cầu xét nghiệm lần 2.

> ⚠️ **SAI LẦM PHỔ BIẾN:** Bỏ qua Prior (Base Rate Fallacy). Khi thấy bộ lọc chỉ sai 5%, ta hay nghĩ "chắc chắn đúng 95%!". Nhưng nếu sự kiện quá hiếm (như bệnh hiếm gặp 1%), thì 5% sai kia sẽ tạo ra số lượng "dương tính giả" áp đảo số ca bệnh thật! Luôn phải xét đến **tần suất ban đầu** (Prior).

### 🔵 3.3 Code Python: Bộ lọc Spam Bayes

```python
import numpy as np
import matplotlib.pyplot as plt
from collections import defaultdict

# =============================================
# BỘ LỌC SPAM NAIVE BAYES — TỪ ZERO
# =============================================

class BoLocSpam:
    """
    Bộ lọc Spam dựa trên Naive Bayes.
    
    Naive Bayes giả sử các từ xuất hiện ĐỘC LẬP với nhau
    (giả sử "naive" nhưng hoạt động tốt đáng ngạc nhiên!)
    """
    
    def __init__(self):
        self.tu_trong_spam = defaultdict(int)    # Đếm từ trong spam
        self.tu_trong_ham = defaultdict(int)      # Đếm từ trong ham (không spam)
        self.tong_spam = 0                         # Tổng email spam
        self.tong_ham = 0                          # Tổng email ham
        self.tong_tu_spam = 0                      # Tổng số từ trong spam
        self.tong_tu_ham = 0                       # Tổng số từ trong ham
    
    def tach_tu(self, van_ban):
        """Tách văn bản thành danh sách từ (đơn giản)"""
        return van_ban.lower().split()
    
    def huan_luyen(self, emails, nhan):
        """
        Huấn luyện bộ lọc.
        emails: danh sách email (chuỗi)
        nhan: danh sách nhãn (1=spam, 0=ham)
        """
        for email, label in zip(emails, nhan):
            cac_tu = self.tach_tu(email)
            
            if label == 1:  # Spam
                self.tong_spam += 1
                for tu in cac_tu:
                    self.tu_trong_spam[tu] += 1
                    self.tong_tu_spam += 1
            else:  # Ham
                self.tong_ham += 1
                for tu in cac_tu:
                    self.tu_trong_ham[tu] += 1
                    self.tong_tu_ham += 1
        
        print(f"✅ Huấn luyện xong: {self.tong_spam} spam, {self.tong_ham} ham")
    
    def tinh_xac_suat(self, email):
        """
        Tính P(spam|email) bằng Bayes + Laplace smoothing.
        
        P(spam|từ1, từ2, ...) ∝ P(spam) × P(từ1|spam) × P(từ2|spam) × ...
        """
        cac_tu = self.tach_tu(email)
        
        # Prior: P(spam), P(ham)
        p_spam = self.tong_spam / (self.tong_spam + self.tong_ham)
        p_ham = self.tong_ham / (self.tong_spam + self.tong_ham)
        
        # Log-likelihood (dùng log để tránh underflow khi nhân nhiều số nhỏ)
        log_p_spam = np.log(p_spam)
        log_p_ham = np.log(p_ham)
        
        vocab_size = len(set(self.tu_trong_spam.keys()) | set(self.tu_trong_ham.keys()))
        
        chi_tiet = []
        for tu in cac_tu:
            # Laplace smoothing: +1 để tránh P=0
            p_tu_spam = (self.tu_trong_spam[tu] + 1) / (self.tong_tu_spam + vocab_size)
            p_tu_ham = (self.tu_trong_ham[tu] + 1) / (self.tong_tu_ham + vocab_size)
            
            log_p_spam += np.log(p_tu_spam)
            log_p_ham += np.log(p_tu_ham)
            
            chi_tiet.append((tu, p_tu_spam, p_tu_ham))
        
        # Chuyển từ log về xác suất
        # P(spam|email) = exp(log_p_spam) / (exp(log_p_spam) + exp(log_p_ham))
        max_log = max(log_p_spam, log_p_ham)
        p_spam_final = np.exp(log_p_spam - max_log) / (np.exp(log_p_spam - max_log) + np.exp(log_p_ham - max_log))
        
        return p_spam_final, chi_tiet
    
    def phan_loai(self, email, nguong=0.5):
        """Phân loại email: spam hay ham"""
        p_spam, chi_tiet = self.tinh_xac_suat(email)
        return "🚫 SPAM" if p_spam > nguong else "✅ HAM", p_spam, chi_tiet

# =============================================
# DỮ LIỆU HUẤN LUYỆN
# =============================================

emails_train = [
    # Spam
    "mien phi trung thuong click ngay nhan qua",
    "ban da trung thuong 1 ty dong click link nhan tien",
    "giam gia soc mien phi ship hang hieu",
    "click ngay de nhan uu dai mien phi 100%",
    "trung thuong lon click link mien phi",
    "mua ngay khuyen mai giam gia mien phi",
    "nhan qua mien phi trung thuong ngay",
    "click de nhan tien mien phi uu dai",
    
    # Ham (không spam)
    "xin chao em gui bao cao tuan nay",
    "cuoc hop ngay mai luc 9 gio sang",
    "gui em tai lieu du an moi",
    "nhac nho nop bao cao cuoi thang",
    "lich hop voi khach hang vao thu 5",
    "cap nhat tien do du an sprint 3",
    "gui anh file thiet ke moi nhat",
    "moi tham gia hoi thao cong nghe",
]

nhan_train = [1, 1, 1, 1, 1, 1, 1, 1,   # Spam
              0, 0, 0, 0, 0, 0, 0, 0]    # Ham

# Huấn luyện
bo_loc = BoLocSpam()
bo_loc.huan_luyen(emails_train, nhan_train)

# =============================================
# KIỂM TRA
# =============================================

emails_test = [
    "click ngay de nhan qua mien phi",              # Spam
    "gui em bao cao sie an xong chua",               # Ham
    "trung thuong iPhone mien phi nhan ngay",        # Spam
    "cuoc hop du an vao sang mai",                   # Ham
    "giam gia cuc soc click mua ngay",               # Spam
]

print("\n📧 Kết quả phân loại:")
print("=" * 70)

ket_qua_list = []
for email in emails_test:
    ket_qua, p_spam, chi_tiet = bo_loc.phan_loai(email)
    ket_qua_list.append(p_spam)
    print(f"\n📨 \"{email}\"")
    print(f"   → {ket_qua} (P(spam) = {p_spam:.1%})")

# =============================================
# TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# --- Biểu đồ xác suất spam ---
ax1 = axes[0]
y_pos = range(len(emails_test))
colors = ['#f44336' if p > 0.5 else '#4CAF50' for p in ket_qua_list]
labels_short = [e[:30] + "..." if len(e) > 30 else e for e in emails_test]

bars = ax1.barh(y_pos, ket_qua_list, color=colors, edgecolor='white', height=0.6)
ax1.axvline(x=0.5, color='black', linestyle='--', linewidth=2, label='Ngưỡng 50%')

for i, (bar, p) in enumerate(zip(bars, ket_qua_list)):
    ax1.text(bar.get_width() + 0.02, bar.get_y() + bar.get_height()/2,
             f'{p:.1%}', va='center', fontsize=10, fontweight='bold')

ax1.set_yticks(y_pos)
ax1.set_yticklabels(labels_short, fontsize=9)
ax1.set_xlabel("P(spam)", fontsize=11)
ax1.set_title("📧 Xác suất Spam cho mỗi email", fontsize=13, fontweight='bold')
ax1.legend(fontsize=10)
ax1.set_xlim(0, 1.15)
ax1.grid(axis='x', alpha=0.3)

# --- Từ "đáng ngờ" nhất ---
ax2 = axes[1]
tu_spam_scores = {}
for tu in bo_loc.tu_trong_spam:
    p_tu_spam = bo_loc.tu_trong_spam[tu] / bo_loc.tong_tu_spam
    p_tu_ham = (bo_loc.tu_trong_ham.get(tu, 0) + 0.1) / bo_loc.tong_tu_ham
    tu_spam_scores[tu] = p_tu_spam / p_tu_ham

# Top 10 từ spam nhất
top_spam = sorted(tu_spam_scores.items(), key=lambda x: x[1], reverse=True)[:10]
tu_names = [t[0] for t in top_spam]
tu_scores = [t[1] for t in top_spam]

ax2.barh(tu_names, tu_scores, color='#FF5722', edgecolor='white', height=0.6)
ax2.set_title("🚫 Top 10 từ 'đáng ngờ' nhất\n(Tỉ lệ xuất hiện trong Spam vs Ham)", 
              fontsize=13, fontweight='bold')
ax2.set_xlabel("Spam Score (cao = đáng ngờ)", fontsize=11)
ax2.grid(axis='x', alpha=0.3)

plt.tight_layout()
plt.savefig('chuong3_spam_filter.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 3 — Định lý Bayes & Spam Filter**
>
> 1. Trong công thức Bayes, "Prior" và "Posterior" khác nhau thế nào?
> 2. P(mưa|mây đen) và P(mây đen|mưa) có bằng nhau không? Tại sao?
> 3. Trong Naive Bayes cho Spam, từ "naive" ngụ ý giả định điều gì?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Prior** = Niềm tin ban đầu (chưa có bằng chứng mới). **Posterior** = Niềm tin MỚI (đã cập nhật sau khi thu thập bằng chứng).
> 2. **Không bằng nhau!** Có mưa thì 100% có mây đen. Nhưng có mây đen thì chưa chắc đã mưa.
> 3. Giả định các từ xuất hiện **độc lập** với nhau (VD: XS có từ "miễn" không ảnh hưởng XS có từ "phí"). Đây là giả định "ngây thơ" nhưng tính toán rất nhanh và hiệu quả.
>
> </details>
>
> **Tự đánh giá:** Đúng 3/3 → 🟢 Tốt quá! | 2/3 → 🟡 Ổn | 0-1/3 → 🔴 Đọc lại Phần 3

---

## PHẦN 3B: INFORMATION THEORY — "ĐO LƯỢNG THÔNG TIN"

### 🟢 3B.1 Entropy — "Mức độ bất ngờ"

**Entropy** đo lượng **thông tin** (hay mức độ bất định) của một phân phối:

$$H(X) = -\sum_{i} p(x_i) \log_2 p(x_i)$$

#### 🎯 Liên tưởng: Đồng xu công bằng vs gian lận

- Đồng xu công bằng (50/50): $H = -0.5 \log_2 0.5 - 0.5 \log_2 0.5 = 1$ bit → **Bất định tối đa**
- Đồng xu gian lận (99/1): $H \approx 0.08$ bit → **Gần như chắc chắn** (ít thông tin mới)

> 💡 **Trong ML:** Entropy của output = mức độ "mụ mờ" của model. Muốn model tự tin → giảm entropy!

### 🟡 3B.2 Cross-Entropy Loss — "Loss function #1 trong classification"

$$H(p, q) = -\sum_{i} p(x_i) \log q(x_i)$$

- $p$ = phân phối thực (nhãn đúng: [0, 1, 0])
- $q$ = phân phối dự đoán của model: [0.1, 0.8, 0.1]

Cross-Entropy nhỏ → Model dự đoán gần đúng. Đây là loss function mặc định cho mọi bài **phân loại**!

#### 🎯 Ví dụ đời sống: Dự báo thời tiết 🌦️

Dự báo nói 90% mưa, thực tế mưa → Cross-entropy thấp (dự đoán tốt).
Dự báo nói 10% mưa, thực tế mưa → Cross-entropy cao (dự đoán tệ).

### 🟡 3B.3 KL Divergence — "Đo khoảng cách giữa 2 phân phối"

$$D_{KL}(p \| q) = \sum_{i} p(x_i) \log \frac{p(x_i)}{q(x_i)} = H(p, q) - H(p)$$

- $D_{KL} = 0$: Hai phân phối giống hệt nhau
- $D_{KL} > 0$: Có sự khác biệt (càng lớn càng khác)

| Ứng dụng | Dùng chỉ số |
|----------|-------------|
| **Classification Loss** | Cross-Entropy |
| **VAE** | KL Divergence (ép latent → Normal) |
| **GAN** | Jensen-Shannon Divergence |
| **Nén file** | Entropy = giới hạn nén tối thiểu |

### 🔵 3B.4 Code Python: Information Theory

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# INFORMATION THEORY TRỰC QUAN
# =============================================

def entropy(probs):
    """Tính entropy"""
    probs = np.array(probs)
    probs = probs[probs > 0]  # Bỏ 0 (log(0) = -inf)
    return -np.sum(probs * np.log2(probs))

def cross_entropy(p, q):
    """Tính cross-entropy"""
    p, q = np.array(p), np.array(q)
    return -np.sum(p * np.log2(q + 1e-10))

def kl_divergence(p, q):
    """Tính KL Divergence"""
    return cross_entropy(p, q) - entropy(p)

# 1. Entropy của đồng xu
print("🎲 ENTROPY — ĐO MỨC BẤT ĐỊNH:")
for p in [0.5, 0.7, 0.9, 0.99]:
    h = entropy([p, 1-p])
    bar = "█" * int(h * 20)
    print(f"   Đồng xu P(ngua)={p:.2f}: H = {h:.3f} bit {bar}")

# 2. Cross-Entropy Loss
print(f"\n🎯 CROSS-ENTROPY LOSS:")
y_true = [0, 1, 0]  # Nhãn đúng: class 2

predictions = [
    ("Tự tin đúng ✅", [0.05, 0.90, 0.05]),
    ("Không chắc ⚠️", [0.30, 0.40, 0.30]),
    ("Sai hoàn toàn ❌", [0.80, 0.10, 0.10]),
]

for name, q in predictions:
    ce = cross_entropy(y_true, q)
    print(f"   {name}: dự đoán = {q} → CE = {ce:.3f}")

# 3. Trực quan hóa
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Entropy vs p
p_range = np.linspace(0.01, 0.99, 100)
h_values = [entropy([p, 1-p]) for p in p_range]
axes[0].plot(p_range, h_values, 'b-', linewidth=2.5)
axes[0].axvline(x=0.5, color='red', linestyle='--', label='p=0.5 (max entropy)')
axes[0].set_title('🎲 Entropy vs P(ngửa)\nCông bằng = Bất định nhất', fontsize=12, fontweight='bold')
axes[0].set_xlabel('P(ngửa)'); axes[0].set_ylabel('Entropy (bit)')
axes[0].legend(); axes[0].grid(True, alpha=0.3)

# Cross-Entropy: Dự đoán tốt vs tệ
confidence = np.linspace(0.01, 0.99, 100)
ce_values = [-np.log2(c) for c in confidence]  # CE khi nhãn = 1
axes[1].plot(confidence, ce_values, 'r-', linewidth=2.5)
axes[1].set_title('🎯 Cross-Entropy Loss\nDự đoán đúng (confidence cao) → Loss thấp', 
                  fontsize=12, fontweight='bold')
axes[1].set_xlabel('Confidence cho class đúng'); axes[1].set_ylabel('Cross-Entropy Loss')
axes[1].grid(True, alpha=0.3)

plt.suptitle('📊 Information Theory — Entropy, Cross-Entropy, KL Divergence', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong3_information_theory.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## PHẦN 3C: LINEAR REGRESSION — "ML MODEL #1"

### 🟢 3C.1 Bài toán hồi quy

Dự đoán giá trị **liên tục** từ dữ liệu:

$$\hat{y} = w_1 x_1 + w_2 x_2 + \ldots + w_n x_n + b = \vec{w}^T \vec{x} + b$$

Đây là **sự kết hợp của tất cả các chương**:
- **Chương 1:** $\vec{w}^T \vec{x}$ = tích vô hướng, phép nhân ma trận
- **Chương 2:** Tối ưu Loss bằng Gradient Descent
- **Chương 3:** Loss = MSE (liên quan đến phương sai), Likelihood

#### 🎯 Ví dụ bất động sản: Dự đoán giá nhà 🏠

$$\text{Giá nhà} = w_1 \cdot \text{Diện tích} + w_2 \cdot \text{Số phòng} + w_3 \cdot \text{Khoảng cách trung tâm} + b$$

### 🟡 3C.2 Loss Function: MSE

$$\mathcal{L} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

Nghiệm tối ưu có công thức đóng (Normal Equation):

$$\vec{w}^* = (X^T X)^{-1} X^T \vec{y}$$

→ Chương 1 (ma trận nghịch đảo) + Chương 2 (GD) hội tụ tại đây!

### 🔵 3C.3 Code Python: Linear Regression từ đầu

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# LINEAR REGRESSION — DỰ ĐOÁN GIÁ NHÀ
# =============================================

np.random.seed(42)

# Dữ liệu: Diện tích (m²) → Giá (tỷ VNĐ)
n = 50
dien_tich = np.random.uniform(30, 120, n)
gia_nha = 0.05 * dien_tich + np.random.normal(0, 0.5, n) + 1  # Giá = 0.05 * DT + nhiễu + 1

# Cách 1: Normal Equation
X = np.column_stack([np.ones(n), dien_tich])  # Thêm cột 1 cho bias
y = gia_nha

w_exact = np.linalg.inv(X.T @ X) @ X.T @ y
print("🏠 LINEAR REGRESSION — Dự đoán giá nhà")
print(f"   Normal Equation: Giá = {w_exact[1]:.4f} × Diện_tích + {w_exact[0]:.4f} tỷ")

# Cách 2: Gradient Descent
w_gd = np.array([0.0, 0.0])  # [bias, weight]
lr = 0.0001
losses = []

for epoch in range(500):
    y_pred = X @ w_gd
    loss = np.mean((y - y_pred)**2)
    losses.append(loss)
    
    gradient = -2/n * X.T @ (y - y_pred) 
    w_gd -= lr * gradient

print(f"   Gradient Descent: Giá = {w_gd[1]:.4f} × Diện_tích + {w_gd[0]:.4f} tỷ")
print(f"   Đúng gần giống nhau! ✅")

# Dự đoán
nha_moi = 75  # m²
gia_du_doan = w_exact[1] * nha_moi + w_exact[0]
print(f"\n   Nhà {nha_moi}m² → Dự đoán giá: {gia_du_doan:.2f} tỷ VNĐ")

# Trực quan hóa
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Dữ liệu + đường hồi quy
ax1 = axes[0]
ax1.scatter(dien_tich, gia_nha, alpha=0.5, color='#2196F3', label='Dữ liệu thực')
x_line = np.linspace(20, 130, 100)
ax1.plot(x_line, w_exact[1]*x_line + w_exact[0], 'r-', linewidth=2.5, 
         label=f'Giá = {w_exact[1]:.3f}×DT + {w_exact[0]:.2f}')
ax1.plot(nha_moi, gia_du_doan, 'g*', markersize=20, 
         label=f'Dự đoán: {nha_moi}m² = {gia_du_doan:.1f} tỷ')
ax1.set_title('🏠 Linear Regression — Dự đoán giá nhà', fontsize=13, fontweight='bold')
ax1.set_xlabel('Diện tích (m²)'); ax1.set_ylabel('Giá (tỷ VNĐ)')
ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)

# Loss giảm dần
ax2 = axes[1]
ax2.plot(losses, 'b-', linewidth=1.5)
ax2.set_title('📉 Loss (MSE) giảm dần khi training', fontsize=13, fontweight='bold')
ax2.set_xlabel('Epoch'); ax2.set_ylabel('MSE Loss')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('chuong3_linear_regression.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Linear Regression là "ML Model #1" — từng bước kết nối mọi thứ lại. Cho sinh viên thấy: Normal Equation dùng nghịch đảo ma trận (Ch.1), GD dùng gradient (Ch.2), và MSE liên quan đến phương sai (Ch.3). Đây là "khoảnh khắc aha" lớn khi mọi thứ kết nối!

---

> ✅ **CHECKPOINT 4 — Information Theory & Linear Regression**
>
> 1. Đồng xu nào có Entropy cao nhất? (Gian lận hay Công bằng)
> 2. Target $y=[0, 1, 0]$. Model A đoán $q=[0.1, 0.8, 0.1]$, Model B đoán $q=[0.3, 0.4, 0.3]$. Model nào có Cross-Entropy Loss thấp hơn?
> 3. Linear Regression tổng hợp những kiến thức nào từ Ch.1 và Ch.2?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Đồng xu công bằng!** Độ bất định lớn nhất (không đoán trước được). Đồng xu gian lận rất dễ đoán nên Entropy thấp.
> 2. **Model A** có Cross-Entropy thấp hơn vì nó "tự tin" và đoán chính xác class đúng ($0.8 > 0.4$).
> 3. Ma trận nghịch đảo / tích vô hướng (Ch.1) và Gradient Descent / Tối ưu Loss (Ch.2).
>
> </details>

---

## PHẦN 4: PHÂN LOẠI DỮ LIỆU & XỬ LÝ NHIỄU

### 🟡 4.1 Maximum Likelihood Estimation (MLE)

Khi bạn có dữ liệu, bạn muốn tìm phân phối "khớp" nhất. MLE tìm tham số $\theta$ sao cho:

$$\hat{\theta} = \arg\max_{\theta} \prod_{i=1}^{n} P(x_i | \theta)$$

#### 🎯 Liên tưởng: Dùng thám tử tìm thủ phạm

Bạn là thám tử có các manh mối (dữ liệu). MLE = *"Với các manh mối này, kịch bản nào **hợp lý nhất**?"*

### 🔵 4.2 Xử lý nhiễu bằng Gaussian Filter

Trong thị giác máy tính, ảnh thường bị **nhiễu** (noise). Phân phối chuẩn (Gaussian) giúp "làm mờ" nhiễu:

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# XỬ LÝ NHIỄU ẢNH BẰNG GAUSSIAN FILTER
# =============================================

def tao_anh_mau():
    """Tạo ảnh mẫu đơn giản"""
    img = np.zeros((100, 100))
    img[20:80, 20:80] = 200   # Hình vuông sáng
    img[35:65, 35:65] = 100   # Hình vuông xám
    return img

def them_nhieu(anh, muc_nhieu=30):
    """Thêm nhiễu Gaussian vào ảnh"""
    nhieu = np.random.normal(0, muc_nhieu, anh.shape)
    return np.clip(anh + nhieu, 0, 255)

def gaussian_kernel(size, sigma):
    """Tạo kernel Gaussian 2D để lọc"""
    x = np.arange(size) - size // 2
    kernel_1d = np.exp(-x**2 / (2 * sigma**2))
    kernel_2d = np.outer(kernel_1d, kernel_1d)
    return kernel_2d / kernel_2d.sum()

def loc_gaussian(anh, kernel_size=5, sigma=1.0):
    """Áp dụng bộ lọc Gaussian (convolution)"""
    kernel = gaussian_kernel(kernel_size, sigma)
    pad = kernel_size // 2
    anh_pad = np.pad(anh, pad, mode='reflect')
    result = np.zeros_like(anh)
    
    for i in range(anh.shape[0]):
        for j in range(anh.shape[1]):
            vung = anh_pad[i:i+kernel_size, j:j+kernel_size]
            result[i, j] = np.sum(vung * kernel)
    
    return result

# Tạo và xử lý
anh_goc = tao_anh_mau()
anh_nhieu = them_nhieu(anh_goc, muc_nhieu=40)
anh_loc = loc_gaussian(anh_nhieu, kernel_size=5, sigma=1.5)

# Trực quan hóa
fig, axes = plt.subplots(1, 4, figsize=(18, 4.5))

images = [
    ("Ảnh gốc", anh_goc),
    ("Ảnh + Nhiễu\n(Gaussian noise σ=40)", anh_nhieu),
    ("Sau lọc Gaussian\n(kernel=5, σ=1.5)", anh_loc),
]

for ax, (title, img) in zip(axes[:3], images):
    ax.imshow(img, cmap='gray', vmin=0, vmax=255)
    ax.set_title(title, fontsize=11, fontweight='bold')
    ax.axis('off')

# Hiển thị kernel
kernel = gaussian_kernel(7, 1.5)
im = axes[3].imshow(kernel, cmap='hot', interpolation='nearest')
axes[3].set_title("Gaussian Kernel 7×7\n(Bộ lọc)", fontsize=11, fontweight='bold')
plt.colorbar(im, ax=axes[3], shrink=0.8)

plt.suptitle("🔍 Xử lý nhiễu ảnh bằng Gaussian Filter — Ứng dụng Phân phối Chuẩn", 
             fontsize=14, fontweight='bold', y=1.02)
plt.tight_layout()
plt.savefig('chuong3_gaussian_filter.png', dpi=150, bbox_inches='tight')
plt.show()

# Đánh giá
mse_nhieu = np.mean((anh_goc - anh_nhieu)**2)
mse_loc = np.mean((anh_goc - anh_loc)**2)
print(f"\n📊 Đánh giá chất lượng:")
print(f"   MSE ảnh nhiễu:      {mse_nhieu:.1f}")
print(f"   MSE sau lọc:        {mse_loc:.1f}")
print(f"   Cải thiện:          {(1 - mse_loc/mse_nhieu)*100:.1f}%")
```

---

## PHẦN 5: 🔵 🎓 MINI PROJECT — Bộ lọc Spam hoàn chỉnh

### Đề bài

Xây dựng bộ lọc Spam sử dụng Naive Bayes với các cải tiến:

1. **Dữ liệu lớn hơn:** Tạo dataset 100+ email (50 spam, 50 ham)
2. **Đánh giá:** Tính Accuracy, Precision, Recall, F1-Score
3. **Trực quan hóa:** Confusion Matrix, ROC Curve

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# MINI PROJECT: Đánh giá Bộ lọc Spam
# =============================================

# Sử dụng BoLocSpam đã xây dựng ở trên

# Tạo dữ liệu test lớn hơn
np.random.seed(42)

tu_spam = ['mien phi', 'trung thuong', 'click', 'ngay', 'nhan', 'qua', 
           'giam gia', 'khuyen mai', 'link', 'tien', 'uu dai', 'hot']
tu_ham = ['bao cao', 'du an', 'hop', 'gui', 'tai lieu', 'lich', 
          'thiet ke', 'code', 'sprint', 'deploy', 'review', 'team']

def tao_email(tu_list, n_tu=5):
    """Tạo email ngẫu nhiên từ danh sách từ"""
    return ' '.join(np.random.choice(tu_list, size=n_tu, replace=True))

# Tạo 100 email test
test_emails = []
test_labels = []
for _ in range(50):
    # Spam: 90% từ spam + 10% từ ham
    tu = np.random.choice(tu_spam, size=4).tolist() + np.random.choice(tu_ham, size=1).tolist()
    test_emails.append(' '.join(tu))
    test_labels.append(1)
    
    # Ham: 10% từ spam + 90% từ ham  
    tu = np.random.choice(tu_ham, size=4).tolist() + np.random.choice(tu_spam, size=1).tolist()
    test_emails.append(' '.join(tu))
    test_labels.append(0)

# Phân loại
predictions = []
scores = []
for email in test_emails:
    _, p_spam, _ = bo_loc.phan_loai(email)
    predictions.append(1 if p_spam > 0.5 else 0)
    scores.append(p_spam)

# Tính metrics
y_true = np.array(test_labels)
y_pred = np.array(predictions)

TP = np.sum((y_true == 1) & (y_pred == 1))
TN = np.sum((y_true == 0) & (y_pred == 0))
FP = np.sum((y_true == 0) & (y_pred == 1))
FN = np.sum((y_true == 1) & (y_pred == 0))

accuracy = (TP + TN) / len(y_true)
precision = TP / (TP + FP) if (TP + FP) > 0 else 0
recall = TP / (TP + FN) if (TP + FN) > 0 else 0
f1 = 2 * precision * recall / (precision + recall) if (precision + recall) > 0 else 0

print("📊 ĐÁNH GIÁ BỘ LỌC SPAM")
print("=" * 40)
print(f"   Accuracy:   {accuracy:.1%}")
print(f"   Precision:  {precision:.1%}")
print(f"   Recall:     {recall:.1%}")
print(f"   F1-Score:   {f1:.1%}")
print(f"\n   Confusion Matrix:")
print(f"              Predicted")
print(f"              Ham    Spam")
print(f"   Actual Ham  {TN:>3}    {FP:>3}")
print(f"   Actual Spam {FN:>3}    {TP:>3}")

# Trực quan hóa Confusion Matrix
fig, ax = plt.subplots(figsize=(6, 5))
cm = np.array([[TN, FP], [FN, TP]])
im = ax.imshow(cm, cmap='Blues', interpolation='nearest')

for i in range(2):
    for j in range(2):
        text_color = 'white' if cm[i, j] > cm.max() / 2 else 'black'
        ax.text(j, i, str(cm[i, j]), ha='center', va='center', 
                fontsize=24, fontweight='bold', color=text_color)

ax.set_xticks([0, 1]); ax.set_xticklabels(['Ham', 'Spam'], fontsize=12)
ax.set_yticks([0, 1]); ax.set_yticklabels(['Ham', 'Spam'], fontsize=12)
ax.set_xlabel('Dự đoán', fontsize=12); ax.set_ylabel('Thực tế', fontsize=12)
ax.set_title(f"📊 Confusion Matrix\nAccuracy: {accuracy:.1%} | F1: {f1:.1%}", 
             fontsize=14, fontweight='bold')
plt.colorbar(im)
plt.tight_layout()
plt.savefig('chuong3_confusion_matrix.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 5 — Phân loại & Xử lý nhiễu**
>
> 1. Trong Maximum Likelihood Estimation (MLE), ta có dữ liệu trước hay có tham số mô hình trước?
> 2. Tại sao lại dùng thuật toán phân phối Chuẩn (Gaussian) để xóa nhiễu trên ảnh?
> 3. Trong Spam Filter, Accuracy cao có đủ để đánh giá mô hình không? Tại sao phải dùng thêm Precision và Recall?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Ta **có dữ liệu trước**. Mục tiêu của MLE là tìm tham số phân phối sao cho khớp với dữ liệu đã quan sát được nhất.
> 2. Vì nhiễu trong thế giới thực phần lớn tuân theo phân phối chuẩn (nhiễu ngẫu nhiên phân bố đều quanh giá trị thực). Gaussian filter sẽ tính trung bình có trọng số để "làm phẳng" phân phối đó.
> 3. **Không đủ.** Chẳng hạn 99% email là bình thường, mô hình dự đoán mọi email đều là bình thường → Accuracy 99% nhưng bộ lọc hoàn toàn vô dụng! Cần Precision (báo spam có đúng không) & Recall (có sót thư spam không).
>
> </details>

---

## PHẦN 6: TÓM TẮT CHƯƠNG & BẢN ĐỒ TƯ DUY

### 🗺️ Bản đồ kiến thức Chương 3

```
XÁC SUẤT THỐNG KÊ — QUY LUẬT CỦA SỰ BẤT ĐỊNH
├── XÁC SUẤT CƠ BẢN
│   ├── P(A) = Mức độ tin tưởng (0 → 1)
│   ├── Quy tắc cộng/nhân
│   ├── Nghịch lý Sinh nhật 🎂
│   └── Quy luật Số lớn
│
├── PHÂN PHỐI XÁC SUẤT
│   ├── Normal (Chuẩn) → Chiều cao, Điểm thi
│   ├── Bernoulli → Tung xu, Click/No-click
│   ├── Binomial → Số thành công
│   ├── Poisson → Số sự kiện/thời gian
│   └── Quy tắc 68-95-99.7
│
├── ĐỊNH LÝ BAYES 🧠
│   ├── P(A|B) = P(B|A)·P(A) / P(B)
│   ├── Prior → Likelihood → Posterior
│   ├── Ứng dụng: Lọc Spam, Chẩn đoán bệnh
│   └── Mini Project: Naive Bayes Spam Filter
│
└── XỬ LÝ NHIỄU
    ├── Gaussian Filter
    └── Ứng dụng: Thị giác máy tính
```

### ✅ Checklist kiến thức

- [ ] Giải thích xác suất bằng "100 vũ trụ song song"
- [ ] Phân biệt được 6 phân phối chính và ứng dụng
- [ ] Áp dụng Định lý Bayes cho lọc spam và chẩn đoán
- [ ] Hiểu tại sao Naive Bayes "naive" nhưng vẫn hiệu quả
- [ ] Xây dựng Spam Filter từ đầu bằng Python
- [ ] Giải thích và tính Accuracy, Precision, Recall, F1

---

## 📝 BÀI TẬP TỰ LUYỆN

### Bài 1: Nghịch lý Monty Hall ⭐
Mô phỏng bài toán Monty Hall 10,000 lần. Chứng minh rằng **đổi cửa** có lợi hơn **giữ nguyên**.

### Bài 2: Central Limit Theorem ⭐⭐
Tung xúc xắc 1 lần → Phân phối đều. Tung 30 lần và lấy trung bình → Phân phối gì? Mô phỏng và trực quan hóa.

### Bài 3: Naive Bayes cho phân loại tin nhắn ⭐⭐⭐
Xây dựng bộ phân loại tin nhắn: "Tích cực" vs "Tiêu cực" (Sentiment Analysis). Sử dụng Naive Bayes.

### Bài 4: A/B Testing ⭐⭐⭐
Công ty có 2 giao diện web. Phiên bản A: 1000 khách, 50 click. Phiên bản B: 1000 khách, 65 click. Dùng kiểm định giả thuyết (hypothesis testing) để xác định: "Phiên bản B thực sự tốt hơn hay chỉ do may mắn?"

---

## 📌 LƯU Ý CHO GIẢNG VIÊN

### Chiến lược khơi gợi sự tò mò

1. **Bắt đầu bằng nghịch lý:**
   - Nghịch lý Sinh nhật: "Chỉ 23 người mà XS trùng > 50%?!" → Trực giác sai
   - Xét nghiệm COVID: "99% chính xác mà chỉ 16.7% đúng?!" → Prior quan trọng

2. **Thí nghiệm tương tác:**
   - Cho cả lớp tung xu 10 lần & ghi kết quả → So sánh với lý thuyết
   - Thu thập sinh nhật cả lớp → Kiểm tra Birthday Paradox ngay tại chỗ

3. **Kết nối AI thực tế:**
   - "Gmail lọc 15 tỷ email/ngày bằng Bayes nâng cao"
   - "Netflix dùng xác suất để đoán em thích phim nào"

4. **Sai lầm phổ biến:**
   - Nhầm P(A|B) với P(B|A): "P(mưa|mây đen) ≠ P(mây đen|mưa)"
   - Base Rate Fallacy: Quên Prior khi tính Bayes

5. **Hoạt động nhóm:**
   - Xây Spam Filter theo nhóm, so sánh accuracy → Nhóm nào tốt nhất?
   - A/B Testing: Mỗi nhóm thiết kế 2 poster, khảo sát lớp → Phân tích kết quả

---

## 📝 BÀI TẬP PHÂN TẦNG

### 🟢 Mức A — Nhận biết

**A1.** Xác suất P(A) nằm trong khoảng nào? Điều gì xảy ra khi P(A)=0? P(A)=1?

**A2.** Phân phối Normal (Gaussian) có dạng hình gì? Hai tham số μ và σ có ý nghĩa gì?

**A3.** Định lý Bayes cho phép ta làm gì mà không có nó thì không thể?

**A4.** Cross-Entropy Loss nhỏ có nghĩa là gì về chất lượng model?

**A5.** Correlation ρ = −0.8 nghĩa là gì? (a) X tăng → Y tăng; (b) X tăng → Y giảm; (c) Không liên quan; (d) Cùng giá trị.

<details><summary>📝 Đáp án A</summary>

- A1: P(A) ∈ [0, 1]. P=0: không thể xảy ra. P=1: chắc chắn xảy ra.
- A2: Hình chuông đối xứng. μ = vị trí trung tâm (đỉnh chuông). σ = độ rộng (phân tán).
- A3: Đảo ngược xác suất có điều kiện — tính P(A|B) từ P(B|A). Ví dụ: từ "test dương tính → xác suất bệnh thật" thay vì ngược lại.
- A4: Model dự đoán gần nhãn thật (phân phối dự đoán gần phân phối thật).
- A5: **(b)** — ρ âm mạnh = tương quan âm mạnh = khi X tăng, Y giảm nhiều.

</details>

---

### 🟡 Mức B — Tính toán cơ bản

**B1.** Tung 2 xúc xắc. Tính:
- (a) P(Tổng = 7)
- (b) P(Tổng ≥ 11)
- (c) P(Cả 2 ra số chẵn)

**B2.** Bài toán Bayes — Xét nghiệm bệnh:
- Tỷ lệ bệnh trong dân số: 2%
- Test dương tính khi có bệnh: 95% (sensitivity)
- Test dương tính khi không bệnh: 3% (false positive rate)
- Tính: P(bệnh thật | test dương tính) = ?

**B3.** Tung xúc xắc 6 mặt.
- Tính E[X] (kỳ vọng)
- Tính Var(X) = E[X²] − (E[X])² (phương sai)
- Gợi ý: E[X²] = (1² + 2² + ... + 6²)/6

**B4.** Cho X = chiều cao nam sinh viên, X ~ Normal(170, 8²). Dùng quy tắc 68-95-99.7:
- P(162 < X < 178) ≈ ?
- P(X > 186) ≈ ?

<details><summary>📝 Đáp án B</summary>

**B1:**
- (a) Tổng cặp ra 7: (1,6),(2,5),(3,4),(4,3),(5,2),(6,1) → **6/36 = 1/6 ≈ 16.7%**
- (b) Tổng ≥ 11: (5,6),(6,5),(6,6) → **3/36 = 1/12 ≈ 8.3%**
- (c) P(cả 2 chẵn) = P(1 ch)**² = (3/6)² = **1/4 = 25%**

**B2:**
- P(D|B) = 0.02, P(+|B) = 0.95, P(+|¬B) = 0.03
- P(+) = 0.95×0.02 + 0.03×0.98 = 0.019 + 0.0294 = 0.0484
- P(B|+) = 0.019/0.0484 ≈ **39.3%** — Dù test khá tốt, chỉ 39% dương tính thật! (Vì bệnh hiếm)

**B3:**
- E[X] = (1+2+3+4+5+6)/6 = **3.5**
- E[X²] = (1+4+9+16+25+36)/6 = 91/6 ≈ 15.17
- Var(X) = 15.17 − 3.5² = 15.17 − 12.25 ≈ **2.92**

**B4:**
- μ=170, σ=8. Khoảng 162-178 = μ±σ → **68%**
- X>186 = X > μ+2σ → 5% ngoài 2σ, rÆ 1 phía → **≈ 2.5%**

</details>

---

### 🔵 Mức C — Giải thích và so sánh

**C1.** Giải thích "Correlation không phải Causation" (tương quan ≠ nhân quả) bằng 2 ví dụ thực tế khác nhau. Trong ML, tại sao nhầm lẫn này gây ra model bị bias?

**C2.** So sánh Bayes Classifier (Naïve Bayes) với Decision Tree: (a) Giả định nền tảng của mỗi loại; (b) Khi nào Naïve Bayes tốt hơn? Khi nào kém hơn? (c) Tại sao "naïve" (ngây thơ)?

**C3.** Entropy $H(X) = -\sum p(x) \log_2 p(x)$ đo gì? Tìm phân phối có entropy lớn nhất trong không gian n kết quả. Tìm phân phối có entropy nhỏ nhất. Liên hệ với Cross-Entropy Loss.

**C4.** Tại sao trong phân tích rủi ro đầu tư, ta muốn portfolio có ma trận covariance chéo gần bằng 0? Kết nối với PCA từ Chương 1.

---

### 🔴 Mức D — Ứng dụng thực tế (Code)

**D1. Spam Filter nâng cao:**
Cải tiến BoLocSpam trong chương: Thêm bigram (cặp từ). Thêm TF-IDF weighting. Test trên 20 email thực (tự viết). Tính precision, recall, F1.

**D2. A/B Testing cho website:**
Giả lập: Group A xem nút xanh (CTR 5%), Group B xem nút đỏ (CTR 7.5%). Mỗi group 1000 lượt. Dùng chi-square test để kiểm định: Sự khác biệt có ý nghĩa thống kê không?

**D3. Monte Carlo Estimation:**
Ước lượng π bằng phương pháp Monte Carlo: Sinh ngẫu nhiên N điểm (x,y) ∈ [0,1]². Tính tỉ lệ điểm nằm trong hình tròn. Vẽ biểu đồ hội tụ (N từ 100 đến 100,000).

**D4. Linear Regression từ đầu:**
Cài đặt Linear Regression sử dụng GD (không dùng sklearn): Loss = MSE, cập nhật (w, b) theo gradient. Áp dụng trên dataset nhà đất tự tạo (diện tích → giá). So sánh với hồi quy giải tích (Normal Equation: $w = (X^TX)^{-1}X^Ty$).

---

## 🆘 HỖ TRỢ NGƯỜI TỰ HỌC

### 📚 Tài nguyên học tiếp

| Nguồn | Nội dung | Khi nào dùng |
|-------|---------|-------------|
| **StatQuest** (YouTube) | XSTK giải thích trực quan nhất | Sau mỗi phần |
| **Khan Academy — Statistics** | Từ cơ bản, nhiều bài tập | Muốn ôn bài |
| **Think Stats** (O'Reilly, miễn phí) | Thống kê bằng Python | Muốn code nhiều |
| **Bayesian Methods for Hackers** | Bayes thực tế, code PyMC3 | Sau khi hiểu Bayes |

### 🗺️ Lộ trình ôn nếu bị hổng

```
Quên xác suất cơ bản? → §1.1 "Lá phiếu bầu" → Bài B1
Quên công thức Bayes?  → §3.2 + VÍ DỤ email spam → Bài B2
Không nhớ phân phối?   → §2.2 Bảng 6 phân phối → A2
Quên Cross-Entropy?    → §4 trong chương → C3
```

### ✅ Checklist tự đánh giá

- [ ] Giải thích P(mưa)=0.8 nghĩa là gì, không dùng "50-50"
- [ ] Tính tay: P(A∩B) khi A và B độc lập, có điều kiện
- [ ] Áp dụng Bayes cho bài toán chẩn đoán bệnh hoặc spam
- [ ] Giải thích kỳ vọng và phương sai với ví dụ đầu tư
- [ ] Phân biệt: Correlation vs Causation với ví dụ cụ thể
- [ ] Viết Naive Bayes spam filter từ đầu
- [ ] Giải thích Cross-Entropy Loss là gì và tại sao dùng

> **6–7 ✅ → Sẵn sàng sang Chương 4.**
> **4–5 ✅ → Xem lại 1–2 mục yếu.**
> **< 4 ✅ → Làm lại Checkpoint trước khi tiếp tục.**

---

> 📖 **Chương tiếp theo:** [Chương 4: Toán Rời rạc — Cấu trúc của Thế giới số](Chuong4_ToanRoiRac.md)
>
> *"Google Maps tìm đường ngắn nhất cho 2 tỷ chuyến đi mỗi ngày — tất cả nhờ lý thuyết đồ thị mà Euler phát minh năm 1736."*

