# CHƯƠNG 7: THỐNG KÊ SUY DIỄN & MÔ HÌNH NÂNG CAO

## 📋 Metadata Chương

| Mục | Chi tiết |
|-----|---------|
| **Tên chương** | Chương 7: Thống kê Suy diễn & Mô hình Nâng cao — Chương Cuối |
| **Hook** | 95% chính xác — Nhưng có thật không? Hay chỉ may mắn? Đây là chương dạy bạn phân biệt sự thật và ảo tưởng. |
| **Đối tượng** | Data Scientists, ML Engineers; người muốn đánh giá model đúng cách |
| **Điều kiện tiên quyết** | Ch.3 (Phân phối chuẩn, Xác suất). Ch.1 (Eigenvector — cho Markov). |
| **Thời gian học** | ~45 phút (Track 🟢🟡) \| ~75 phút (Track 🔵) |
| **Mục tiêu đầu ra** | Sau chương này, người học có thể: |
| | • **Thực hiện** Hypothesis Testing 5 bước — A/B testing thực tế |
| | • **Tính và giải thích** Confidence Interval cho model accuracy |
| | • **Cài đặt** Logistic Regression từ đầu với Binary Cross-Entropy |
| | • **Xây dựng** Markov Chain và tìm phân phối dừng (stationary distribution) |
| | • **Áp dụng** Monte Carlo để ước lượng và mô phỏng rủi ro |
| | • **Nhận diện** Bias-Variance Tradeoff và chọn đúng giải pháp |

---

## 📖 STATISTICAL INFERENCE & ADVANCED MODELS


> *"Ai cũng biết trung bình, biết đếm. Nhưng khả năng nói 'Kết quả này THẬT SỰ có ý nghĩa, không phải do may mắn' — đó mới là sức mạnh thực sự của thống kê."*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 7 (Tập cuối): "Chứng minh mình có giá trị"
>
> *N-42 đã dự đoán đúng 95% trên training data. Nhưng "sếp" (Data Scientist) hoài nghi: "Đó là do giỏi thật, hay do THUỘC BÀI (overfitting)?" N-42 phải dùng hypothesis testing để chứng minh kết quả không phải may mắn, dùng confidence interval để nói "accuracy nằm trong 93-97%", và hiểu bias-variance tradeoff để không bao giờ tự tin quá mức. Hành trình của N-42 hoàn thành — từ 1 neuron trống rỗng thành 1 phần của AI thực sự.*

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: Viên thuốc có thật sự hiệu quả?

Một công ty dược phát triển thuốc mới. Thử nghiệm trên 200 người:
- **Nhóm uống thuốc (100 người):** 65 người khỏi
- **Nhóm uống giả dược (100 người):** 55 người khỏi

65% vs 55% — Thuốc có thật sự tốt hơn? Hay chỉ do **may mắn**?

> → Đây chính là bài toán mà **Hypothesis Testing** giải quyết. Không cần đoán — có TOÁN CHỨNG MINH.

---

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Khái niệm | Một câu tóm tắt | Tại sao cần cho AI/CNTT |
|---|-----------|-----------------|---------------------|
| 1 | **Hypothesis Testing** | Dùng toán để kiểm tra xem một nhận định có đúng không (A/B testing) | Đánh giá tính năng mới, A/B testing hiệu quả thuật toán |
| 2 | **Confidence Interval** | Ước lượng khoảng giá trị có khả năng chứa giá trị thực (thay vì 1 số) | Báo cáo khoảng độ chính xác thay vì 1 con số ảo |
| 3 | **Logistic Regression** | Phân loại nhị phân bằng hàm Sigmoid → output xác suất | Credit scoring, chẩn đoán y tế, phân loại spam |
| 4 | **Markov Chain** | Tương lai chỉ phụ thuộc hiện tại (memory-less) | Dự báo thời tiết, PageRank, credit migration |
| 5 | **Monte Carlo** | Mô phỏng ngẫu nhiên → Ước lượng kết quả | Option pricing, SGD, mô phỏng khí hậu |
| 6 | **Bias-Variance** | Sự đánh đổi giữa học thuộc lòng và học chung chung | Giải quyết Underfitting và Overfitting trong AI |

> 📋 **Prerequisites:** Ch.3 (Phân phối chuẩn, Xác suất căn bản)
>
> ⏱️ **Thời gian đọc:** ~45 phút (Đã đến chương cuối rồi, cố lên!)

---

## 🗺️ BẠN ĐANG Ở ĐÂY

```
  ... ━━▶ Ch.3 Xác suất ━━▶ Ch.4-6 (Nền tảng) ━━▶ 📍 Ch.7 THỐNG KÊ SUY DIỄN
         (✅ Done)         (✅ Done)             BẠN ĐANG Ở ĐÂY (TRẠM CUỐI)
```

**Phần này cần học trước:** Chương 3 là điều kiện bắt buộc. Bạn cần biết thế nào là Phân phối chuẩn.
**Bạn sẽ cần lại kiến thức này khi:** Làm Data Scientist (A/B testing trên hàng triệu user), đánh giá thuật toán AI ở công đoạn cuối trước khi đưa vào thực tế (production).

---

## 📦 ÔN NHANH TOÁN PHỔ THÔNG

<details>
<summary>🔔 <b>Ôn nhanh 1: Phân phối Chuẩn (Nhắc lại Chương 3)</b> (Bấm để mở)</summary>

- Phân phối chuẩn có hình chuông.
- Ý nghĩa: Phần lớn dữ liệu hội tụ quanh giá trị trung bình $\mu$.
- Quy tắc 68-95-99.7: 95% dữ liệu rơi vào khoảng chênh lệch không quá $2\sigma$ so với $\mu$.
- Khái niệm này là linh hồn của Confidence Interval!

</details>

---

## 🏷️ HƯỚNG DẪN ĐỌC — Hệ thống 3 tầng

| Tầng | Label | Dành cho ai? | Cần gì? |
|------|-------|-------------|--------|
| 🟢 | **Hiểu bằng trực giác** | Tất cả mọi người | Không cần Toán |
| 🟡 | **Lý thuyết Thống kê** | Muốn hiểu sâu | Công thức, Đại số căn bản |
| 🔵 | **Hiểu bằng code Python** | Developer / Coder | scipy, numpy |

---

## PHẦN 1: HYPOTHESIS TESTING — "KHOA HỌC TRONG QUYẾT ĐỊNH"

### 🟢 1.1 Ý tưởng cốt lõi

#### 🎯 Liên tưởng: Xử án — Vô tội cho đến khi chứng minh có tội ⚖️

| Khái niệm | Trong tòa án | Trong thống kê |
|-----------|-------------|----------------|
| **H₀ (Null Hypothesis)** | Bị cáo VÔ TỘI | Không có sự khác biệt |
| **H₁ (Alternative)** | Bị cáo CÓ TỘI | CÓ sự khác biệt |
| **Bằng chứng** | Nhân chứng, vật chứng | Dữ liệu, thống kê |
| **p-value** | Xác suất bằng chứng xảy ra nếu vô tội | XS dữ liệu xảy ra nếu H₀ đúng |
| **α = 0.05** | Ngưỡng "beyond reasonable doubt" | Ngưỡng bác bỏ H₀ |
| **Kết luận** | Có tội / Không đủ bằng chứng | Bác bỏ H₀ / Không bác bỏ |

> 💡 **p-value** = "Nếu thuốc THẬT SỰ vô dụng (H₀), thì xác suất thấy kết quả ≥ 65% khỏi là bao nhiêu?" Nếu XS này rất nhỏ (< 5%) → Kết luận thuốc CÓ hiệu quả!

> 🌱 **VÍ DỤ VỠ LÒNG — Tung 1 đồng xu (1 phút)**
> 
> Bạn đánh cược tung đồng xu. Bạn của bạn tung ra Ngửa 3 lần liên tiếp. Bạn phớt lờ. 5 lần liên tiếp? Trực giác bạn bảo: "Có gì sai!".
> - H₀: Đồng xu bình thường.
> - Bằng chứng: 5 Ngửa liên tiếp ($P = (1/2)^5 = 1/32 \approx 3\%$).
> - Vì $3\% < 5\%$ ($\alpha=0.05$), bạn bắt đầu TỪ CHỐI H₀: Đồng xu này có vấn đề!

### 🟡 1.2 Quy trình 5 bước

1. **Đặt giả thuyết:** H₀: $\mu_1 = \mu_2$ (không khác biệt), H₁: $\mu_1 \neq \mu_2$
2. **Chọn mức ý nghĩa:** $\alpha = 0.05$ (chấp nhận 5% sai lầm)
3. **Thu thập dữ liệu & tính test statistic** (z-score, t-score, chi-square...)
4. **Tính p-value:** XS dữ liệu xảy ra nếu H₀ đúng
5. **Kết luận:** p < α → Bác bỏ H₀ ✅ | p ≥ α → Không đủ bằng chứng ❌

> ⚠️ **SAI LẦM PHỔ BIẾN:** p-value KHÔNG PHẢI là xác suất H₀ (giả thuyết vô tội) đúng. Đây là lỗi cực kỳ hay gặp, ngay cả trong nghiên cứu. Nó là "Xác suất thấy bằng chứng này, DƯỚI GIẢ ĐỊNH H₀ ĐÃ ĐÚNG NGUYÊN BẢN".

### 🟡 1.3 Các loại test phổ biến

| Test | Khi nào dùng | Ví dụ |
|------|-------------|-------|
| **Z-test** | So sánh trung bình, n lớn, biết σ | Điểm thi 2 lớp |
| **t-test** | So sánh trung bình, n nhỏ, không biết σ | Thuốc mới vs giả dược |
| **Chi-square** | So sánh tần suất/tỷ lệ | Giới tính ↔ Sở thích mua hàng |
| **ANOVA** | So sánh ≥ 3 nhóm | 3 chiến lược marketing |
| **Paired t-test** | Cùng nhóm, trước/sau | Trước/sau dùng app học |

#### 🎯 Ví dụ A/B Testing: Website thương mại 🛒

| | Phiên bản A (cũ) | Phiên bản B (mới) |
|-|-------------------|-------------------|
| Số khách | 10,000 | 10,000 |
| Số mua | 320 (3.2%) | 380 (3.8%) |

H₀: $p_A = p_B$ (không khác biệt) vs H₁: $p_B > p_A$ (B tốt hơn)

#### 🎯 Ví dụ giáo dục: A/B Testing phương pháp dạy mới 📚

Một trường đại học muốn kiểm tra: "Phương pháp dạy bằng dự án (project-based) có tốt hơn phương pháp truyền thống (lecture-based)?"

| | Truyền thống (A) | Dự án (B) |
|-|-----------------|----------|
| Số sinh viên | 150 | 150 |
| Điểm TB | 6.8 | 7.4 |
| Đậu tỷ lệ | 78% | 88% |

H₀: $\mu_A = \mu_B$ (không khác biệt) vs H₁: $\mu_B > \mu_A$

Sau khi chạy t-test: $p = 0.02 < 0.05$ → **Bác bỏ H₀** → Phương pháp B có hiệu quả hơn!

→ Nhưng khoan! Giáo viên cần kiểm thêm: có phải nhóm B giỏi hơn ban đầu không? (confounding variable). Đó là lý do cần **randomized controlled experiment**.

#### 🎯 Ví dụ nông nghiệp: Hypothesis Testing giống lúa mới 🌾

Nông dân muốn biết: "Giống lúa mới cho năng suất cao hơn giống cũ không?"

- **Giống cũ (100 mẫu):** Trung bình 65 tạ/ha, σ = 8
- **Giống mới (100 mẫu):** Trung bình 68 tạ/ha, σ = 7

H₀: $\mu_{mới} = \mu_{cũ}$ vs H₁: $\mu_{mới} > \mu_{cũ}$

Chạy t-test: $p = 0.008 < 0.01$ → **Bác bỏ H₀** → Giống mới tốt hơn có ý nghĩa thống kê!

→ Viện lúa giống dùng kết quả này để quyết định có nên khuyến nghị nông dân chuyển đổi giống hay không.

### 🔵 1.4 Code Python: Hypothesis Testing thực tế

#### 🎯 Ví dụ thể thao: Markov Chain dự đoán kết quả ⚽

Giải bóng đá có 3 trạng thái: Thắng (W), Hòa (D), Thua (L). Ma trận chuyển tiếp dựa trên lịch sử:

$$P = \begin{pmatrix} 0.6 & 0.3 & 0.1 \\ 0.4 & 0.4 & 0.2 \\ 0.2 & 0.3 & 0.5 \end{pmatrix}$$

Đọc: Nếu trận trước thắng (hàng 1), trận sau: 60% thắng tiếp, 30% hòa, 10% thua.

→ Phân phối dừng cho biết: về dài hạn, đội này thắng ~43%, hòa ~33%, thua ~24%. Đây là cách các trang dự đoán thể thao tính "phong độ dài hạn" của đội bóng!

#### 🎯 Ví dụ môi trường: Monte Carlo mô phỏng biến đổi khí hậu 🌍

Nhà khoa học muốn ước lượng: "Nhiệt độ trung bình toàn cầu năm 2100 sẽ là bao nhiêu?"

- Chạy mô phỏng 10,000 lần với các tham số ngẫu nhiên: phát thải, phản hồi khí hậu, hoạt động núi lửa...
- Mỗi lần cho ra 1 nhiệt độ dự đoán
- Trung bình = dự đoán tốt nhất, độ lệch chuẩn = mức không chắc chắn

→ Kết quả: "Nhiệt độ tăng 2.5°C ± 0.8°C" (95% CI). Đây chính là cách IPCC (Liên Hợp Quốc) đưa ra các báo cáo biến đổi khí hậu — tất cả dựa trên Monte Carlo!

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# =============================================
# 1. T-TEST: THUỐC MỚI CÓ HIỆU QUẢ?
# =============================================

np.random.seed(42)

# Dữ liệu thử nghiệm (mô phỏng)
nhom_thuoc = np.random.binomial(1, 0.65, 100)     # 65% khỏi
nhom_gia_duoc = np.random.binomial(1, 0.55, 100)  # 55% khỏi

# T-test
t_stat, p_value = stats.ttest_ind(nhom_thuoc, nhom_gia_duoc)

print("💊 HYPOTHESIS TESTING: Thuốc mới có hiệu quả?")
print("=" * 55)
print(f"   H₀: Thuốc = Giả dược (không hiệu quả)")
print(f"   H₁: Thuốc ≠ Giả dược (có hiệu quả)")
print(f"   α = 0.05")
print(f"\n   Nhóm thuốc:     {nhom_thuoc.mean():.1%} khỏi (n=100)")
print(f"   Nhóm giả dược:  {nhom_gia_duoc.mean():.1%} khỏi (n=100)")
print(f"\n   t-statistic = {t_stat:.4f}")
print(f"   p-value = {p_value:.4f}")
print(f"\n   Kết luận: {'✅ BÁC BỎ H₀ — Thuốc CÓ hiệu quả!' if p_value < 0.05 else '❌ Không đủ bằng chứng'}")

# =============================================
# 2. A/B TESTING: WEBSITE
# =============================================

print(f"\n\n🛒 A/B TESTING: Website mới có tốt hơn?")
print("=" * 55)

n_A, success_A = 10000, 320  # Phiên bản cũ: 3.2%
n_B, success_B = 10000, 380  # Phiên bản mới: 3.8%

# Z-test cho tỷ lệ
p_A = success_A / n_A
p_B = success_B / n_B
p_pool = (success_A + success_B) / (n_A + n_B)
se = np.sqrt(p_pool * (1 - p_pool) * (1/n_A + 1/n_B))
z = (p_B - p_A) / se
p_val_ab = 1 - stats.norm.cdf(z)  # One-sided

print(f"   Phiên bản A: {p_A:.1%} mua ({success_A}/{n_A})")
print(f"   Phiên bản B: {p_B:.1%} mua ({success_B}/{n_B})")
print(f"   z = {z:.4f}, p-value = {p_val_ab:.4f}")
print(f"\n   Kết luận: {'✅ B TỐT HƠN A (có ý nghĩa thống kê)!' if p_val_ab < 0.05 else '❌ Chưa đủ bằng chứng B tốt hơn'}")

if p_val_ab < 0.05:
    tang = (p_B - p_A) / p_A * 100
    print(f"   → Nên chuyển sang B! Tăng {tang:.1f}% tỷ lệ mua.")
    print(f"   → Ước tính thêm {(p_B - p_A) * 100000:.0f} đơn hàng/100k khách!")

# =============================================
# 3. TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Hình 1: p-value trực quan
ax1 = axes[0]
x = np.linspace(-4, 4, 300)
ax1.plot(x, stats.norm.pdf(x), 'b-', linewidth=2, label='Phân phối nếu H₀ đúng')
ax1.fill_between(x, stats.norm.pdf(x), where=x >= z, alpha=0.4, color='red', 
                  label=f'p-value = {p_val_ab:.4f}')
ax1.axvline(x=z, color='red', linestyle='--', linewidth=2, label=f'z = {z:.2f} (quan sát)')
ax1.axvline(x=1.645, color='green', linestyle='--', linewidth=2, alpha=0.5, label='z = 1.645 (α=0.05)')
ax1.set_title('🎯 p-value = Diện tích vùng đỏ\n(Nhỏ = bác bỏ H₀)', fontsize=12, fontweight='bold')
ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)

# Hình 2: A/B Testing kết quả
ax2 = axes[1]
categories = ['Phiên bản A\n(cũ)', 'Phiên bản B\n(mới)']
values = [p_A * 100, p_B * 100]
colors = ['#FF5722', '#4CAF50']
bars = ax2.bar(categories, values, color=colors, edgecolor='white', width=0.5)

for bar, val in zip(bars, values):
    ax2.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.05,
             f'{val:.1f}%', ha='center', fontsize=14, fontweight='bold')

ax2.set_ylabel('Tỷ lệ mua (%)', fontsize=12)
ax2.set_title(f'🛒 A/B Testing: B tốt hơn A\np-value = {p_val_ab:.4f} {"< 0.05 ✅" if p_val_ab < 0.05 else "≥ 0.05 ❌"}', 
              fontsize=12, fontweight='bold')
ax2.grid(axis='y', alpha=0.3)

plt.suptitle('📊 Hypothesis Testing — Kết quả có ý nghĩa hay chỉ do may mắn?', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_hypothesis_testing.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Nhấn mạnh p-value KHÔNG phải "xác suất H₀ đúng". Nó là "xác suất thấy dữ liệu ≥ cực đoan NẾU H₀ đúng". Sai lầm phổ biến nhất! Dùng ví dụ tòa án: "Bằng chứng mạnh ≠ chắc chắn có tội".

---

> ✅ **CHECKPOINT 1 — Hypothesis Testing**
>
> 1. Trong xét xử án, "Coi người ta vô tội cho tới khi bị chứng minh là có tội" ứng với khái niệm nào trong Hypothesis testing?
> 2. P-value là xác suất mà Null hypothesis đúng. Đúng hay sai?
> 3. T-test và Z-test khác nhau ở điểm cốt lõi nào?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Đó chính là **H₀ (Null Hypothesis)**. Ta xuất phát từ giả định rỗng: "Mọi sự khác biệt chỉ là do trùng hợp ngẫu nhiên / không có lỗi / bình thường", trừ khi có số liệu rành rành.
> 2. **Sai**. Nhắc lại: P-value là Xác suất thu được dữ liệu thế này (hoặc cực đoan hơn), **nếu H₀ đúng**. Nó trả lời câu hỏi: Sự trùng hợp này có vô lý quá không?
> 3. Z-test cho $n$ lớn, biết phương sai quần thể hoặc phương sai tính gần đúng tốt. T-test (phân phối student-T) dùng khi $n$ nhỏ (<30) để cho đánh giá bảo thủ hơn (khoảng chênh rộng hơn tránh tự tin quá mức).
>
> </details>
>
> **Tự đánh giá:** Đúng 3/3 → 🟢 Tanh tưởi! | 2/3 → 🟡 Làm tốt | < 2/3 → 🔴 Xem lại Phần 1.

---

## PHẦN 2: CONFIDENCE INTERVAL — "KHOẢNG ƯỚC LƯỢNG"

### 🟢 2.1 Từ điểm đến khoảng

Thay vì nói "tỷ lệ khỏi = 65%", nói **"tỷ lệ khỏi nằm trong 55.6% — 74.4% (95% tin tưởng)"**.

$$CI = \bar{x} \pm z_{\alpha/2} \cdot \frac{s}{\sqrt{n}}$$

- $\bar{x}$ = Trung bình mẫu
- $z_{\alpha/2}$ = 1.96 (cho 95% CI)
- $s$ = Độ lệch chuẩn mẫu
- $n$ = Kích thước mẫu

#### 🎯 Liên tưởng: Bắn cung 🎯

- **Điểm ước lượng** = Viên đạn (có thể trúng hoặc trật)
- **Confidence Interval** = Hộp mục tiêu (95% lần bắn rơi trong hộp)
- n lớn hơn → Hộp nhỏ hơn (chính xác hơn)

#### 🎯 Ví dụ thăm dò bầu cử 🗳️

Khảo sát 1,000 người: 52% chọn ứng viên A.

$$CI = 52\% \pm 1.96 \times \sqrt{\frac{0.52 \times 0.48}{1000}} = 52\% \pm 3.1\%$$

→ **48.9% — 55.1%** → Chưa thể khẳng định A thắng! (vì chứa 50%)

Nếu khảo sát 10,000 người: CI = 52% ± 1.0% → **51% — 53%** → A nhiều khả năng thắng! ✅

### 🔵 2.2 Code Python: Confidence Interval

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# =============================================
# CONFIDENCE INTERVAL
# =============================================

np.random.seed(42)

# Ví dụ: Ước lượng thu nhập trung bình
thu_nhap_mau = np.random.normal(15, 5, 200)  # 200 người, trung bình 15 triệu/tháng

mean = thu_nhap_mau.mean()
se = thu_nhap_mau.std() / np.sqrt(len(thu_nhap_mau))

# CI các mức
cis = {}
for confidence in [0.90, 0.95, 0.99]:
    z = stats.norm.ppf(1 - (1-confidence)/2)
    ci_low = mean - z * se
    ci_high = mean + z * se
    cis[confidence] = (ci_low, ci_high)

print("💰 CONFIDENCE INTERVAL: Thu nhập trung bình")
print("=" * 55)
print(f"   Mẫu: n = {len(thu_nhap_mau)}, x̄ = {mean:.2f} triệu/tháng")
print(f"   SE = {se:.4f}")

for conf, (lo, hi) in cis.items():
    print(f"   {conf:.0%} CI: [{lo:.2f}, {hi:.2f}] triệu")

# Ảnh hưởng của kích thước mẫu
print(f"\n📊 ẢNH HƯỞNG CỦA KÍCH THƯỚC MẪU:")
for n in [10, 50, 200, 1000, 10000]:
    se_n = 5 / np.sqrt(n)
    width = 2 * 1.96 * se_n
    bar = "█" * int(width * 3)
    print(f"   n = {n:>6}: CI width = ±{1.96*se_n:.2f} triệu {bar}")

# Trực quan
fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Hình 1: CI các mức
ax1 = axes[0]
colors_ci = {'90%': '#FF9800', '95%': '#4CAF50', '99%': '#2196F3'}
for i, (conf, (lo, hi)) in enumerate(cis.items()):
    label = f'{conf:.0%}'
    ax1.barh(i, hi - lo, left=lo, height=0.3, color=list(colors_ci.values())[i],
             alpha=0.7, edgecolor='white', label=f'{label}: [{lo:.2f}, {hi:.2f}]')
ax1.axvline(x=mean, color='red', linewidth=2, linestyle='--', label=f'x̄ = {mean:.2f}')
ax1.set_yticks(range(3)); ax1.set_yticklabels(['90%', '95%', '99%'])
ax1.set_title('🎯 Confidence Interval — Mức tin cậy\n(Cao hơn → Rộng hơn)', 
              fontsize=12, fontweight='bold')
ax1.set_xlabel('Thu nhập (triệu VNĐ/tháng)')
ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)

# Hình 2: Ảnh hưởng n
ax2 = axes[1]
ns = [10, 30, 50, 100, 200, 500, 1000, 5000]
widths = [2 * 1.96 * 5 / np.sqrt(n) for n in ns]
ax2.plot(ns, widths, 'bo-', linewidth=2, markersize=8)
ax2.set_title('📉 CI width vs Sample size\n(n lớn → CI hẹp → Chính xác hơn)', 
              fontsize=12, fontweight='bold')
ax2.set_xlabel('Kích thước mẫu (n)', fontsize=11)
ax2.set_ylabel('Độ rộng CI (triệu VNĐ)', fontsize=11)
ax2.grid(True, alpha=0.3)

plt.suptitle('📊 Confidence Interval — Khoảng ước lượng đáng tin', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_confidence_interval.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 2 — Confidence Interval**
>
> 1. Nếu tôi muốn Confidence Interval hẹp lại (ước lượng chính xác hơn) thì tôi cần tăng hay giảm số lượng mẫu (khảo sát nhiều người hơn hay ít đi)?
> 2. Có chắc 100% tỷ lệ bầu cử thực tế nằm trong Confidence Interval 95% không?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Phải **tăng số lượng mẫu ($n$)**. Formula cho thấy độ rộng $\approx 1/\sqrt{n}$. Càng phỏng vấn nhiều người, độ tin cậy càng cao khoảng sai số càng hẹp lại.
> 2. **Không.** Có 5% rủi ro giá trị thực tế lại nằm NGOÀI khoảng đó!
>
> </details>
>
> **Tự đánh giá:** Đúng 2/2 → 🟢 Tốt! | Nhỏ hơn 2 → 🔴 Xem lại Phần 2.

---

## PHẦN 3: LOGISTIC REGRESSION — "MÔ HÌNH PHÂN LOẠI #1"

### 🟢 3.1 Từ Linear đến Logistic

Linear Regression dự đoán giá trị liên tục ($-\infty$ đến $+\infty$). Nhưng ta muốn dự đoán **xác suất** (0 đến 1):

$$P(y=1 | \vec{x}) = \sigma(\vec{w}^T \vec{x} + b) = \frac{1}{1 + e^{-(\vec{w}^T \vec{x} + b)}}$$

$\sigma$ (sigmoid) "ép" output vào khoảng (0, 1) → Xác suất!

#### 🎯 Liên tưởng: Phòng tuyển dụng 🏢

- **Input** $\vec{x}$: [GPA, Kinh nghiệm, Kỹ năng] (hồ sơ ứng viên)
- **Linear** $\vec{w}^T\vec{x} + b$: Điểm ấn tượng tổng hợp (có thể là -5 đến +10)
- **Sigmoid**: Chuyển thành xác suất nhận: 95%, 30%, 2%...
- **Ngưỡng 0.5**: > 50% → Nhận! ≤ 50% → Từ chối.

#### 🎯 Ví dụ tài chính: Credit Scoring 🏦

$$P(\text{vỡ nợ}) = \sigma(w_1 \cdot \text{Thu\_nhập} + w_2 \cdot \text{Nợ\_hiện\_tại} + w_3 \cdot \text{Lịch\_sử\_tín\_dụng} + b)$$

> 🌱 **VÍ DỤ VỠ LÒNG — Tính nhẩm xác suất đậu phỏng vấn (1 phút)**
> 
> Một cỗ máy Logistic tính tổng điểm ứng viên $Z = 0.5 \times ĐiểmGPA - 1$.
> Nếu GPA = 2.0 $\rightarrow Z = 0$. Hàm Sigmoid $\sigma(0) = 0.5 = 50\%$.
> Nếu GPA = 6.0 $\rightarrow Z = 2.0$. Hàm Sigmoid của 2 là khoảng $88\%$.
> Tóm lại, Sigmoid luôn bóp điểm số Z bất kỳ (dù âm vô cực đến dương vô cực) về dạng phần trăm từ 0% đến 100%!

### 🟡 3.2 Loss: Binary Cross-Entropy

$$\mathcal{L} = -\frac{1}{n}\sum_{i=1}^{n} [y_i \log(\hat{y}_i) + (1-y_i)\log(1-\hat{y}_i)]$$

> 💡 **Tại sao không dùng MSE?** MSE + sigmoid tạo bề mặt non-convex (nhiều cực tiểu cục bộ). Cross-Entropy + sigmoid luôn CONVEX → GD luôn tìm được nghiệm tối ưu!

### 🔵 3.3 Code Python: Logistic Regression từ đầu

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# LOGISTIC REGRESSION — CreditScoring
# =============================================

np.random.seed(42)

def sigmoid(z):
    return 1 / (1 + np.exp(-np.clip(z, -500, 500)))

# Dữ liệu: Thu nhập & Nợ → Vỡ nợ (0/1)
n = 300
thu_nhap = np.random.uniform(5, 50, n)      # 5-50 triệu/tháng
no_hien_tai = np.random.uniform(0, 200, n)  # 0-200 triệu nợ

# Rule: XS vỡ nợ tăng khi nợ cao, thu nhập thấp
z_true = -0.1 * thu_nhap + 0.02 * no_hien_tai - 1
prob_true = sigmoid(z_true)
vo_no = (np.random.random(n) < prob_true).astype(float)

# Chuẩn bị dữ liệu
X = np.column_stack([np.ones(n), thu_nhap, no_hien_tai])
y = vo_no

# Train Logistic Regression bằng GD
w = np.zeros(3)
lr = 0.001
losses = []

for epoch in range(1000):
    z = X @ w
    y_pred = sigmoid(z)
    
    # Binary Cross-Entropy Loss
    loss = -np.mean(y * np.log(y_pred + 1e-8) + (1-y) * np.log(1-y_pred + 1e-8))
    losses.append(loss)
    
    # Gradient
    gradient = X.T @ (y_pred - y) / n
    w -= lr * gradient

# Kết quả
print("🏦 LOGISTIC REGRESSION — Credit Scoring")
print("=" * 55)
print(f"   w = [{w[0]:.4f}, {w[1]:.4f}, {w[2]:.4f}]")
print(f"   Ý nghĩa:")
print(f"     Thu nhập ↑ 1 triệu → log-odds vỡ nợ thay đổi {w[1]:+.4f}")
print(f"     Nợ ↑ 1 triệu      → log-odds vỡ nợ thay đổi {w[2]:+.4f}")

# Accuracy
y_final = (sigmoid(X @ w) > 0.5).astype(float)
accuracy = (y_final == y).mean()
print(f"\n   Accuracy: {accuracy:.1%}")

# Dự đoán mẫu
khach_moi = [1, 30, 50]  # Bias, 30 triệu/tháng, 50 triệu nợ
prob = sigmoid(np.dot(khach_moi, w))
print(f"\n   Khách mới (thu nhập 30tr, nợ 50tr):")
print(f"   → P(vỡ nợ) = {prob:.1%} {'⚠️ Rủi ro!' if prob > 0.5 else '✅ An toàn'}")

# =============================================
# TRỰC QUAN HÓA
# =============================================

fig, axes = plt.subplots(1, 3, figsize=(20, 5))

# 1. Sigmoid function
ax1 = axes[0]
z_range = np.linspace(-6, 6, 200)
ax1.plot(z_range, sigmoid(z_range), 'b-', linewidth=2.5)
ax1.axhline(y=0.5, color='red', linestyle='--', alpha=0.5, label='Ngưỡng 0.5')
ax1.axhline(y=0, color='gray', linewidth=0.5)
ax1.axhline(y=1, color='gray', linewidth=0.5)
ax1.fill_between(z_range, sigmoid(z_range), 0.5, where=sigmoid(z_range)>0.5, 
                  alpha=0.2, color='green', label='Dự đoán: VỠ NỢ')
ax1.fill_between(z_range, sigmoid(z_range), 0.5, where=sigmoid(z_range)<0.5, 
                  alpha=0.2, color='red', label='Dự đoán: AN TOÀN')
ax1.set_title('🔄 Hàm Sigmoid\nÉp output vào (0, 1)', fontsize=11, fontweight='bold')
ax1.legend(fontsize=8); ax1.grid(True, alpha=0.3)

# 2. Decision boundary
ax2 = axes[1]
# Vẽ dữ liệu
mask_pos = y == 1
mask_neg = y == 0
ax2.scatter(thu_nhap[mask_neg], no_hien_tai[mask_neg], alpha=0.4, color='#4CAF50', s=20, label='An toàn (0)')
ax2.scatter(thu_nhap[mask_pos], no_hien_tai[mask_pos], alpha=0.4, color='#f44336', s=20, label='Vỡ nợ (1)')

# Decision boundary: w0 + w1*x + w2*y = 0 → y = -(w0 + w1*x) / w2
if abs(w[2]) > 1e-6:
    x_db = np.linspace(5, 50, 100)
    y_db = -(w[0] + w[1] * x_db) / w[2]
    ax2.plot(x_db, y_db, 'k-', linewidth=2.5, label='Ranh giới quyết định')

ax2.set_title('🏦 Decision Boundary\n(Ranh giới phân loại)', fontsize=11, fontweight='bold')
ax2.set_xlabel('Thu nhập (triệu/tháng)'); ax2.set_ylabel('Nợ hiện tại (triệu)')
ax2.legend(fontsize=8); ax2.grid(True, alpha=0.3)

# 3. Loss giảm dần
ax3 = axes[2]
ax3.plot(losses, 'b-', linewidth=1.5)
ax3.set_title(f'📉 Cross-Entropy Loss\nAccuracy = {accuracy:.1%}', fontsize=11, fontweight='bold')
ax3.set_xlabel('Epoch'); ax3.set_ylabel('Loss')
ax3.grid(True, alpha=0.3)

plt.suptitle('🏦 Logistic Regression: Credit Scoring — Dự đoán rủi ro vỡ nợ', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_logistic_regression.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Logistic Regression là "cầu nối" giữa thống kê truyền thống và deep learning. Neural network 0 hidden layers = Logistic Regression! Khi thêm hidden layers → Neural Network. Hãy nhấn mạnh sự liên tục này.

---

> ✅ **CHECKPOINT 3 — Logistic Regression**
>
> 1. Hàm Sigmoid làm nhiệm vụ gì đối với kết quả của Linear Regression?
> 2. Người ta dùng MSE để train Logistic Regression có được không? Vì sao?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Nó **"bóp" (squash)** kết quả tuyến tính (có thể là cực lớn hoặc cực âm) vào khoảng rất đẹp từ 0 đến 1. Điểm chính giữa 0 được bóp thành 50%.
> 2. Về mặt lý thuyết là tính được, nhưng **không nên dùng**! MSE qua hàm Sigmoid sẽ làm cho trũng hàm Loss bị gồ ghề (non-convex), khiến thuật toán tối ưu Gradient Descent bị kẹt ở các cực tiểu địa phương. Phải dùng Binary Cross-Entropy.
>
> </details>

---

## PHẦN 4: MARKOV CHAIN — "TƯƠNG LAI CHỈ PHỤ THUỘC HIỆN TẠI"

### 🟢 4.1 Tính chất Markov

**Markov property:** Trạng thái tương lai chỉ phụ thuộc **trạng thái hiện tại**, không phụ thuộc quá khứ:

$$P(X_{t+1} | X_t, X_{t-1}, \ldots, X_0) = P(X_{t+1} | X_t)$$

#### 🎯 Liên tưởng: Trò chơi cờ ♟️

Nước đi tiếp theo chỉ phụ thuộc **bàn cờ hiện tại**, không cần biết 50 nước trước đó.

### 🟡 4.2 Ma trận chuyển tiếp

| Trạng thái | Sang Nắng | Sang Mưa | Sang Mây |
|-----------|-----------|----------|----------|
| **Nắng** | 0.6 | 0.2 | 0.2 |
| **Mưa** | 0.3 | 0.4 | 0.3 |
| **Mây** | 0.4 | 0.3 | 0.3 |

→ Ma trận chuyển tiếp $P$: Hàng = trạng thái hiện tại, Cột = trạng thái kế tiếp.

#### 🎯 Ví dụ tài chính: Credit Rating Transition 🏦

| | AAA | AA | A | BBB | Default |
|-|-----|----|---|-----|---------|
| **AAA** | 0.90 | 0.08 | 0.01 | 0.005 | 0.005 |
| **AA** | 0.05 | 0.85 | 0.07 | 0.02 | 0.01 |
| **A** | 0.01 | 0.05 | 0.85 | 0.07 | 0.02 |

→ Ngân hàng dùng Markov Chain dự đoán: "Khách hàng xếp hạng A hôm nay, 5 năm sau XS vỡ nợ?"

### 🟡 4.3 Phân phối dừng (Stationary Distribution)

Sau nhiều bước, Markov Chain hội tụ về **phân phối dừng** $\vec{\pi}$:

$$\vec{\pi} = \vec{\pi} \cdot P \quad \text{(eigenvector với eigenvalue = 1!)}$$

→ Đây chính là **PageRank của Google!** (Chương 1, Bài tập 4)

### 🔵 4.4 Code Python: Markov Chain

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# MARKOV CHAIN — DỰ BÁO THỜI TIẾT
# =============================================

states = ['☀️ Nắng', '🌧️ Mưa', '☁️ Mây']

# Ma trận chuyển tiếp
P = np.array([
    [0.6, 0.2, 0.2],  # Từ Nắng
    [0.3, 0.4, 0.3],  # Từ Mưa
    [0.4, 0.3, 0.3],  # Từ Mây
])

# Mô phỏng chuỗi Markov
def simulate_markov(P, n_steps, start_state=0):
    """Mô phỏng Markov Chain"""
    n_states = len(P)
    chain = [start_state]
    for _ in range(n_steps):
        current = chain[-1]
        next_state = np.random.choice(n_states, p=P[current])
        chain.append(next_state)
    return chain

# Chạy mô phỏng
np.random.seed(42)
chain = simulate_markov(P, 100, start_state=0)

print("🌤️ MARKOV CHAIN — DỰ BÁO THỜI TIẾT")
print("=" * 55)
print(f"   Ma trận chuyển tiếp P:")
for i, row in enumerate(P):
    print(f"   {states[i]}: → {' | '.join([f'{p:.1f}' for p in row])}")

print(f"\n   10 ngày đầu: {' → '.join([states[s] for s in chain[:10]])}")

# Tần suất thực tế (xấp xỉ phân phối dừng)
for i, state in enumerate(states):
    freq = sum(1 for s in chain if s == i) / len(chain)
    print(f"   Tần suất {state}: {freq:.1%}")

# Phân phối dừng (eigenvalue = 1)
eigenvalues, eigenvectors = np.linalg.eig(P.T)
idx = np.argmin(np.abs(eigenvalues - 1))
pi = np.real(eigenvectors[:, idx])
pi = pi / pi.sum()

print(f"\n   📊 Phân phối dừng (lý thuyết):")
for i, state in enumerate(states):
    print(f"   {state}: {pi[i]:.1%}")
print(f"   (= Eigenvector của Pᵀ với eigenvalue = 1)")

# =============================================
# VÍ DỤ TÀI CHÍNH: Credit Migration
# =============================================

print(f"\n\n🏦 MARKOV CHAIN — CREDIT RATING MIGRATION")
print("=" * 55)

ratings = ['AAA', 'AA', 'A', 'BBB', 'Default']
P_credit = np.array([
    [0.90, 0.08, 0.01, 0.005, 0.005],
    [0.02, 0.85, 0.10, 0.02,  0.01],
    [0.00, 0.03, 0.85, 0.09,  0.03],
    [0.00, 0.01, 0.05, 0.82,  0.12],
    [0.00, 0.00, 0.00, 0.00,  1.00],  # Default = hấp thụ
])

# Xác suất vỡ nợ sau N năm cho khách hạng A
start = np.array([0, 0, 1, 0, 0])  # Bắt đầu ở hạng A
print(f"   Khách hàng bắt đầu hạng A:")
for years in [1, 3, 5, 10]:
    P_n = np.linalg.matrix_power(P_credit, years)
    prob_default = (start @ P_n)[4]
    print(f"   Sau {years:>2} năm: P(Default) = {prob_default:.2%}")

# Trực quan
fig, axes = plt.subplots(1, 2, figsize=(16, 6))

# Hình 1: Markov Chain convergence
ax1 = axes[0]
n_sims = 1000
n_steps = 50
freqs = np.zeros((n_steps, 3))

for step in range(n_steps):
    chains = [simulate_markov(P, step, start_state=0) for _ in range(n_sims)]
    ends = [c[-1] for c in chains]
    for i in range(3):
        freqs[step, i] = sum(1 for e in ends if e == i) / n_sims

colors = ['#FFC107', '#2196F3', '#9E9E9E']
for i, (state, color) in enumerate(zip(states, colors)):
    ax1.plot(freqs[:, i], linewidth=2, color=color, label=f'{state}: {pi[i]:.1%}')
    ax1.axhline(y=pi[i], linestyle='--', color=color, alpha=0.5)

ax1.set_title('📊 Markov Chain hội tụ → Phân phối dừng\n(Bắt đầu từ Nắng)', 
              fontsize=11, fontweight='bold')
ax1.set_xlabel('Số bước'); ax1.set_ylabel('Tần suất')
ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)

# Hình 2: Credit migration
ax2 = axes[1]
years_range = range(1, 21)
start_states = {'AAA': [1,0,0,0,0], 'A': [0,0,1,0,0], 'BBB': [0,0,0,1,0]}
colors_credit = {'AAA': '#4CAF50', 'A': '#FF9800', 'BBB': '#f44336'}

for rating, start_vec in start_states.items():
    probs = [np.dot(start_vec, np.linalg.matrix_power(P_credit, y))[4] for y in years_range]
    ax2.plot(years_range, [p*100 for p in probs], linewidth=2, color=colors_credit[rating],
             label=f'Từ {rating}', marker='o', markersize=4)

ax2.set_title('🏦 XS vỡ nợ theo thời gian\n(Hạng thấp → vỡ nợ nhanh hơn)', 
              fontsize=11, fontweight='bold')
ax2.set_xlabel('Năm'); ax2.set_ylabel('P(Default) %')
ax2.legend(fontsize=10); ax2.grid(True, alpha=0.3)

plt.suptitle('⛓️ Markov Chain — Tương lai chỉ phụ thuộc hiện tại', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_markov_chain.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 4 — Markov Chain**
>
> 1. "Sáng nay ăn phở thì trưa nay ăn cơm". Đây có phải là tính chất Markov không?
> 2. Phân phối dừng (Stationary Distribution) của Markov Chain có ý nghĩa gì thực tiễn ví dụ trong dự báo thời tiết?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Đúng**. Vì bữa trưa (tương lai) chỉ phụ thuộc vào bữa sáng (hiện tại), không cần biết hôm qua ăn gì. Tính chất "quên quá khứ" là cốt lõi của Markov.
> 2. Nó cho biết tỷ lệ dài hạn. Dù hôm nay là Nắng hay Mưa, thì vế lâu dài (sau rất nhiều ngày), ví dụ tỷ lệ thời tiết trung bình luôn là 50% Nắng, 30% Mưa, 20% Mây ở khu vực đó.
>
> </details>

---

## PHẦN 5: MONTE CARLO METHODS — "SỨC MẠNH CỦA NGẪU NHIÊN"

### 🟢 5.1 Ý tưởng: Giải bài toán bằng... tung xúc xắc!

Nhiều bài toán quá phức tạp để giải chính xác. Monte Carlo dùng **random sampling** để ước lượng.

#### 🎯 Ví dụ kinh điển: Tính π bằng bắn súng 🎯

Vẽ hình vuông 1×1 chứa 1/4 hình tròn. Bắn ngẫu nhiên N điểm:

$$\pi \approx 4 \times \frac{\text{Số điểm trong tròn}}{\text{Tổng điểm}}$$

#### 🎯 Ví dụ tài chính: Định giá quyền chọn (Option Pricing) 📈

Mô phỏng 10,000 kịch bản giá cổ phiếu tương lai → Tính trung bình payoff → Giá option!

$$\text{Option Price} = e^{-rT} \cdot \frac{1}{N}\sum_{i=1}^{N} \max(S_i(T) - K, 0)$$

### 🟡 5.2 Ứng dụng trong ML

| Ứng dụng | Vai trò Monte Carlo |
|----------|---------------------|
| **SGD** | Random sampling mini-batch = Monte Carlo cho gradient! |
| **Dropout** | Random tắt neuron = Monte Carlo ensemble |
| **Bayesian NN** | MCMC sampling posterior |
| **Reinforcement Learning** | Monte Carlo Tree Search (AlphaGo!) |
| **Cross-validation** | Random split train/test |

### 🔵 5.3 Code Python: Monte Carlo

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# 1. MONTE CARLO: TÍNH π
# =============================================

np.random.seed(42)

ns = [100, 1000, 10000, 100000]
pi_estimates = []

fig, axes = plt.subplots(1, 4, figsize=(20, 5))

for ax, N in zip(axes, ns):
    x = np.random.uniform(0, 1, N)
    y = np.random.uniform(0, 1, N)
    inside = x**2 + y**2 <= 1
    pi_est = 4 * inside.sum() / N
    pi_estimates.append(pi_est)
    
    ax.scatter(x[inside], y[inside], s=0.5, color='#4CAF50', alpha=0.3)
    ax.scatter(x[~inside], y[~inside], s=0.5, color='#f44336', alpha=0.3)
    
    theta = np.linspace(0, np.pi/2, 100)
    ax.plot(np.cos(theta), np.sin(theta), 'b-', linewidth=2)
    
    error = abs(pi_est - np.pi)
    ax.set_title(f'N = {N:,}\nπ ≈ {pi_est:.4f} (sai {error:.4f})', 
                fontsize=10, fontweight='bold')
    ax.set_aspect('equal'); ax.set_xlim(0, 1); ax.set_ylim(0, 1)

plt.suptitle('🎯 Monte Carlo: Tính π bằng bắn ngẫu nhiên — N lớn hơn → Chính xác hơn!', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_monte_carlo_pi.png', dpi=150, bbox_inches='tight')
plt.show()

# =============================================
# 2. MONTE CARLO: OPTION PRICING
# =============================================

print("📈 MONTE CARLO: ĐỊNH GIÁ OPTION")
print("=" * 55)

S0 = 100        # Giá cổ phiếu hiện tại
K = 105         # Strike price
T = 1.0         # 1 năm
r = 0.05        # Lãi suất phi rủi ro
sigma = 0.20    # Volatility
N_sims = 100000 # Số kịch bản

# Mô phỏng giá cổ phiếu cuối kỳ (Geometric Brownian Motion)
Z = np.random.normal(0, 1, N_sims)
ST = S0 * np.exp((r - 0.5*sigma**2)*T + sigma*np.sqrt(T)*Z)

# Call option payoff
payoff = np.maximum(ST - K, 0)

# Giá option = trung bình payoff chiết khấu
option_price = np.exp(-r*T) * payoff.mean()
option_se = np.exp(-r*T) * payoff.std() / np.sqrt(N_sims)

print(f"   Cổ phiếu hiện tại: S₀ = {S0}")
print(f"   Strike price: K = {K}")
print(f"   Volatility: σ = {sigma:.0%}")
print(f"   Mô phỏng: {N_sims:,} kịch bản")
print(f"\n   💰 Giá Call Option = {option_price:.2f} ± {1.96*option_se:.2f}")
print(f"   (95% CI: [{option_price - 1.96*option_se:.2f}, {option_price + 1.96*option_se:.2f}])")

# Phân phối giá cuối kỳ
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

ax1 = axes[0]
ax1.hist(ST, bins=100, density=True, alpha=0.7, color='#2196F3', edgecolor='white')
ax1.axvline(x=K, color='red', linewidth=2, linestyle='--', label=f'Strike K={K}')
ax1.axvline(x=S0, color='green', linewidth=2, linestyle='--', label=f'Hiện tại S₀={S0}')
ax1.set_title('📊 Phân phối giá cổ phiếu cuối kỳ\n(10,000 kịch bản Monte Carlo)', 
              fontsize=11, fontweight='bold')
ax1.set_xlabel('Giá S(T)'); ax1.legend(fontsize=9); ax1.grid(True, alpha=0.3)

# Monte Carlo convergence
ax2 = axes[1]
running_mean = np.exp(-r*T) * np.cumsum(payoff) / np.arange(1, N_sims+1)
ax2.plot(running_mean[:10000], 'b-', linewidth=1, alpha=0.7)
ax2.axhline(y=option_price, color='red', linewidth=2, linestyle='--', label=f'Giá ước lượng: {option_price:.2f}')
ax2.set_title('📉 MC Convergence\n(Ước lượng hội tụ khi N tăng)', fontsize=11, fontweight='bold')
ax2.set_xlabel('Số kịch bản'); ax2.set_ylabel('Giá option ước lượng')
ax2.legend(fontsize=9); ax2.grid(True, alpha=0.3)

plt.suptitle('💰 Monte Carlo Option Pricing — Định giá quyền chọn bằng mô phỏng', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_monte_carlo_finance.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 5 — Monte Carlo**
>
> 1. Sức mạnh chính của Monte Carlo nằm ở việc gì?
> 2. Làm thế nào để kết quả của mô phỏng Monte Carlo chính xác hơn?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Dùng **lấy mẫu ngẫu nhiên (random sampling)** số lượng lớn để xấp xỉ kết quả của những bài toán quá bức thiết không thể giải bằng công thức toán học chính xác (như tính tích phân phức tạp).
> 2. **Tăng số lượng mẫu (N lớn hơn)**. Luật Số Lớn đảm bảo rằng khi N tiến đến vô cùng, kết quả trung bình của mẫu sẽ tiến đến kết quả thực tế.
>
> </details>

---

## PHẦN 6: BIAS-VARIANCE TRADEOFF — "BÀI HỌC LỚN NHẤT CỦA ML"

### 🟢 6.1 Tổng lỗi dự đoán

$$\text{Total Error} = \text{Bias}^2 + \text{Variance} + \text{Irreducible Noise}$$

| Thành phần | Nghĩa | Nguyên nhân |
|-----------|-------|-------------|
| **Bias** | Sai lệch hệ thống | Model quá đơn giản (underfitting) |
| **Variance** | Dao động giữa các lần train | Model quá phức tạp (overfitting) |
| **Noise** | Nhiễu trong dữ liệu | Không thể giảm |

#### 🎯 Liên tưởng: Bắn cung 🏹

| | Bias thấp | Bias cao |
|-|-----------|----------|
| **Variance thấp** | 🎯 Tập trung đúng tâm (LÝ TƯỞNG) | ⬛ Tập trung nhưng lệch tâm (underfitting) |
| **Variance cao** | 🔵 Phân tán quanh tâm (overfitting) | 💨 Phân tán VÀ lệch tâm (tệ nhất!) |

#### 🎯 Ví dụ đời sống: Đầu bếp học nấu phở 🍜

- **Bias cao:** Nấu theo 1 công thức cứng nhắc → Luôn cùng vị, nhưng KHÔNG NGON (underfitting)
- **Variance cao:** Mỗi lần nấu khác nhau → Ngon lúc dở lúc, KHÔNG ỔN ĐỊNH (overfitting)
- **Lý tưởng:** Nắm nguyên tắc + điều chỉnh hợp lý → Luôn ngon! 🎯

### 🔵 6.2 Code Python: Trực quan Bias-Variance

```python
import numpy as np
import matplotlib.pyplot as plt

# =============================================
# BIAS-VARIANCE TRADEOFF
# =============================================

np.random.seed(42)

def true_function(x):
    return np.sin(2*x) + 0.5*x

# Sinh nhiều dataset
n_datasets = 100
n_points = 30
noise_std = 0.5

x_test = np.linspace(0, 5, 200)
y_true = true_function(x_test)

degrees = [1, 3, 5, 15]
fig, axes = plt.subplots(1, 4, figsize=(22, 5))

for ax, degree in zip(axes, degrees):
    all_predictions = []
    
    for _ in range(n_datasets):
        # Tạo dataset mới (nhiễu khác nhau)
        x_train = np.random.uniform(0, 5, n_points)
        y_train = true_function(x_train) + np.random.normal(0, noise_std, n_points)
        
        # Fit model
        coef = np.polyfit(x_train, y_train, min(degree, n_points-1))
        y_pred = np.polyval(coef, x_test)
        all_predictions.append(np.clip(y_pred, -5, 8))
    
    all_preds = np.array(all_predictions)
    mean_pred = all_preds.mean(axis=0)
    
    # Tính Bias² và Variance
    bias_sq = np.mean((mean_pred - y_true)**2)
    variance = np.mean(np.var(all_preds, axis=0))
    total_error = bias_sq + variance
    
    # Vẽ
    ax.plot(x_test, y_true, 'g-', linewidth=2.5, label='Hàm thực', zorder=10)
    for i in range(min(20, n_datasets)):
        ax.plot(x_test, all_preds[i], 'b-', alpha=0.1, linewidth=0.5)
    ax.plot(x_test, mean_pred, 'r--', linewidth=2, label='Trung bình dự đoán')
    
    ax.set_title(f'Đa thức bậc {degree}\nBias² = {bias_sq:.2f}, Var = {variance:.2f}\n'
                f'Total = {total_error:.2f}', fontsize=10, fontweight='bold')
    ax.set_ylim(-3, 6); ax.legend(fontsize=7); ax.grid(True, alpha=0.3)

plt.suptitle('🏹 Bias-Variance Tradeoff: Đường xanh mờ = model fit trên các dataset khác nhau', 
             fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong7_bias_variance.png', dpi=150, bbox_inches='tight')
plt.show()

# Bias-Variance vs Complexity
fig, ax = plt.subplots(figsize=(10, 6))

all_degrees = range(1, 16)
biases, variances, totals = [], [], []

for degree in all_degrees:
    all_predictions = []
    for _ in range(50):
        x_train = np.random.uniform(0, 5, n_points)
        y_train = true_function(x_train) + np.random.normal(0, noise_std, n_points)
        coef = np.polyfit(x_train, y_train, min(degree, n_points-1))
        y_pred = np.clip(np.polyval(coef, x_test), -10, 15)
        all_predictions.append(y_pred)
    
    all_preds = np.array(all_predictions)
    mean_pred = all_preds.mean(axis=0)
    
    bias_sq = np.mean((mean_pred - y_true)**2)
    variance = np.mean(np.var(all_preds, axis=0))
    
    biases.append(bias_sq)
    variances.append(variance)
    totals.append(bias_sq + variance)

ax.plot(list(all_degrees), biases, 'r-o', linewidth=2, label='Bias²', markersize=6)
ax.plot(list(all_degrees), variances, 'b-o', linewidth=2, label='Variance', markersize=6)
ax.plot(list(all_degrees), totals, 'k-o', linewidth=2.5, label='Total Error', markersize=6)

best_degree = list(all_degrees)[np.argmin(totals)]
ax.axvline(x=best_degree, color='green', linestyle='--', linewidth=2, 
           label=f'Best: bậc {best_degree}')

ax.set_title('🎯 Bias-Variance Tradeoff\nBài học lớn nhất: Model phức tạp ≠ Model tốt!', 
             fontsize=14, fontweight='bold')
ax.set_xlabel('Độ phức tạp model (bậc đa thức)', fontsize=12)
ax.set_ylabel('Error', fontsize=12)
ax.legend(fontsize=11); ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('chuong7_bias_variance_curve.png', dpi=150, bbox_inches='tight')
plt.show()

print("\n💡 BÀI HỌC LỚN NHẤT CỦA ML:")
print("   ❌ Model phức tạp hơn ≠ Model tốt hơn")
print("   ✅ Model TỐT NHẤT = Điểm cân bằng Bias vs Variance")
print(f"   Trong ví dụ này: Bậc {best_degree} là tối ưu!")
```

---

> ✅ **CHECKPOINT 6 — Bias-Variance Tradeoff**
>
> 1. Một mô hình học thuộc lòng dữ liệu train đạt 99% accuracy nhưng test chỉ đạt 50%. Nó đang bị Bias cao hay Variance cao?
> 2. Nguyên nhân phổ biến nhất của Bias cao (Underfitting) là gì?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Variance cao (Overfitting).** Mô hình quá nhạy cảm với dữ liệu huấn luyện cụ thể, không tổng quát hóa được.
> 2. **Mô hình quá đơn giản.** Ví dụ: Dùng Linear Regression (đường thẳng) để đi dự đoán dữ liệu phức tạp phân bố hình cong. Mô hình không đủ sức diễn đạt dữ liệu thực tế.
>
> </details>

---

## PHẦN 7: TÓM TẮT CHƯƠNG & BẢN ĐỒ TƯ DUY

### 🗺️ Bản đồ kiến thức Chương 7

```
THỐNG KÊ SUY DIỄN & MÔ HÌNH NÂNG CAO
├── HYPOTHESIS TESTING ⚖️
│   ├── H₀ vs H₁, p-value, α = 0.05
│   ├── t-test, chi-square, ANOVA
│   └── Ứng dụng: A/B Testing, Thuốc mới
│
├── CONFIDENCE INTERVAL 🎯
│   ├── CI = x̄ ± z × SE
│   ├── n lớn → CI hẹp → Chính xác hơn
│   └── Ứng dụng: Thăm dò bầu cử, Thu nhập
│
├── LOGISTIC REGRESSION 🏦
│   ├── σ(wᵀx + b) → Xác suất
│   ├── Binary Cross-Entropy Loss
│   └── Ứng dụng: Credit Scoring, Tuyển dụng
│
├── MARKOV CHAIN ⛓️
│   ├── P(tương lai | hiện tại)
│   ├── Ma trận chuyển tiếp → Phân phối dừng
│   └── Ứng dụng: Credit migration, Thời tiết, PageRank
│
├── MONTE CARLO 🎲
│   ├── Random sampling → Ước lượng
│   ├── N lớn → Chính xác hơn
│   └── Ứng dụng: Option pricing, SGD, AlphaGo
│
└── BIAS-VARIANCE TRADEOFF 🏹
    ├── Total Error = Bias² + Variance + Noise
    ├── Underfitting (Bias cao) vs Overfitting (Variance cao)
    └── Bài học: Model tốt nhất ≠ Model phức tạp nhất
```

### 💎 Nếu chỉ nhớ 1 điều từ chương này

> **"95% chính xác" không có nghĩa là "95% đúng" — bạn cần thống kê để phân biệt thật và ảo.**
>
> Hypothesis Testing cho bạn công cụ để nói "kết quả này không phải do may mắn". Confidence Interval cho bạn khoảng ước lượng thay vì một con số ảo. Logistic Regression là cầu nối giữa xác suất và phân loại. Markov và Monte Carlo là hai kỹ thuật mô phỏng thế giới thực. Và Bias-Variance tradeoff dạy bạn: model tốt nhất không phải model phức tạp nhất.

---

### 🔗 Kiến thức này xuất hiện lại ở đâu?

| Kiến thức | Xuất hiện trong | Ứng dụng thực tế |
|----------|-----------------|------------------|
| **Hypothesis Testing** | A/B Testing mọi sản phẩm tech | Google, Facebook, Netflix đều dùng |
| **Confidence Interval** | Báo cáo kết quả ML | "Model accuracy = 93% ± 2%" |
| **Logistic Regression** | Credit scoring, Medical diagnosis | Ngân hàng, Bệnh viện |
| **Markov Chain** | Google PageRank, Weather prediction | Xác suất chuyển trạng thái |
| **Monte Carlo** | SGD, Option pricing, AlphaGo | Random sampling → Ước lượng |
| **Bias-Variance** | Mọi dự án ML | Chọn model, Tuning hyperparameter |

---

### ✅ Checklist kiến thức

- [ ] Thiết lập Hypothesis Testing: H₀, H₁, α, p-value
- [ ] Giải thích p-value đúng: "XS dữ liệu nếu H₀ đúng" (KHÔNG phải "XS H₀ đúng")
- [ ] Tính Confidence Interval và giải thích
- [ ] Xây dựng Logistic Regression từ đầu bằng Python
- [ ] Hiểu Markov property và tính phân phối dừng
- [ ] Dùng Monte Carlo ước lượng π và giải thích convergence
- [ ] Vẽ và giải thích Bias-Variance tradeoff curve
- [ ] Chọn model complexity phù hợp (không quá đơn giản, không quá phức tạp)

---

## ❓ FAQ — Câu hỏi thường gặp

**Q: p-value = 0.03 có nghĩa là 3% khả năng H₀ đúng?**
A: KHÔNG! p-value = 0.03 nghĩa là: "Nếu H₀ đúng, thì xác suất thấy dữ liệu ≥ extreme như này là 3%." Đây là lỗi hiểu sai phổ biến nhất trong thống kê.

**Q: Confidence Interval 95% có nghĩa là 95% dữ liệu nằm trong khoảng đó?**
A: KHÔNG! CI 95% nghĩa là: "Nếu lặp lại thí nghiệm 100 lần, khoảng CI sẽ chứa giá trị thật ~95 lần." Nó nói về quá trình, không phải dữ liệu.

**Q: Logistic Regression chỉ dùng cho phân loại nhị phân?**
A: Mặc định là nhị phân, nhưng mở rộng được thành đa lớp (multiclass) bằng Softmax (tổng quát hóa sigmoid). Khi đó, nó gọi là Multinomial Logistic Regression hoặc Softmax Regression.

**Q: Bias-Variance tradeoff có còn quan trọng với deep learning không?**
A: CÓ, nhưng theo cách khác. Deep learning thường có bias thấp, variance cao. Giải pháp: nhiều data, regularization (dropout, weight decay), early stopping. Hiểu tradeoff này giúp bạn chọn đúng chiến thuật.

---

## 📝 BÀI TẬP TỰ LUYỆN

### Bài 1: A/B Testing thực tế ⭐⭐
Shopee có 2 layout mới. Layout A: 5,000 khách, 180 mua. Layout B: 5,000 khách, 220 mua. Dùng z-test kiểm tra B có thật sự tốt hơn?

### Bài 2: Credit Scoring ⭐⭐⭐
Xây dựng Logistic Regression phân loại khách "vỡ nợ" vs "an toàn". Dùng 3 features: Thu nhập, Nợ, Tuổi. Tính Accuracy, Precision, Recall, F1. Trực quan hóa decision boundary.

### Bài 3: Monte Carlo Integration ⭐⭐
Tính $\int_0^1 e^{-x^2} dx$ bằng Monte Carlo (sampling). So sánh với scipy.integrate.quad.

### Bài 4: Markov Chain — Dự báo thời tiết ⭐⭐
Mở rộng ví dụ thời tiết: Thêm trạng thái "Bão". Xây ma trận chuyển tiếp, mô phỏng 365 ngày. So sánh tần suất với phân phối dừng.

### Bài 5: Bias-Variance thực nghiệm ⭐⭐⭐
Cho dataset Boston Housing. Fit Linear Regression (bias cao), Random Forest depth=3 (balanced), Random Forest depth=unlimited (variance cao). So sánh train error vs test error. Trực quan hóa.

### Bài 6: Option Pricing nâng cao ⭐⭐⭐
Mở rộng Monte Carlo pricing: thêm Put option, Asian option (trung bình), và American option (exercise sớm). So sánh giá với Black-Scholes formula.

---

## 📌 LƯU Ý CHO GIẢNG VIÊN

### Chiến lược khơi gợi sự tò mò

1. **Khảo sát lớp:** "Hôm qua em ngủ bao nhiêu giờ?" → Thu thập → Tính CI → "95% sinh viên lớp mình ngủ 5.8-7.2 giờ"
2. **A/B Test thật:** Cho lớp vote 2 poster → Kiểm tra sự khác biệt có ý nghĩa?
3. **Monte Carlo π:** Cho sinh viên tung 100 điểm lên giấy → Ước lượng π
4. **Casino:** "Tại sao casino luôn thắng?" → Law of Large Numbers + Expected Value

### Sai lầm phổ biến

- p-value < 0.05 KHÔNG nghĩa là "chắc chắn đúng" (vẫn có 5% sai)
- Correlation ≠ Causation (kem bán chạy ↔ đuối nước tăng → không phải nhân quả!)
- Overfitting: "92% accuracy" trên training KHÔNG có ý nghĩa nếu test chỉ 60%

### Lộ trình bổ sung

| Tuần | Nội dung | Lab |
|------|---------|-----|
| 16-17 | Chương 6: Tối ưu hóa nâng cao | Saddle point + Optimizer |
| 18 | Chương 7: Hypothesis Testing + CI | A/B Testing thực tế |
| 19 | Chương 7: Logistic Regression + Markov | Credit Scoring |
| 20 | Chương 7: Monte Carlo + Bias-Variance | Final Project |

---

> *"Toán học không phải là điều bạn NHỚ. Toán học là điều bạn HIỂU — và khi hiểu rồi, bạn sẽ không bao giờ quên."*
>
> — Triết lý của giáo trình "Toán học không rào cản"

---

## 📝 BÀI TẬP PHÂN TẦNG (TỔNG KẾT CUỐI SÁCH)

### 🟢 Mức A — Nhận biết

**A1.** p-value = 0.02 và ngưỡng α = 0.05. Kết luận: (a) Bác bỏ H₀; (b) Không bác bỏ H₀; (c) Chấp nhận H₀; (d) Không có kết luận.

**A2.** Confidence Interval 95% cho học lực trung bình là [6.8, 7.2]. Điều đó có nghĩa là 95% học sinh đạt điểm trong khoảng này không? Giải thích.

**A3.** Logistic Regression và Linear Regression: Cái nào cho output là xác suất (0-1)? Hàm nào giúp "ép" output vào khoảng này?

**A4.** "Trạng thái tương lai chỉ phụ thuộc trạng thái hiện tại" — đây là tính chất gì? Ứng dụng trong ML là gì?

**A5.** Monte Carlo dùng để làm gì? Lợi thế chính của nó so với giải tích phân chính xác là gì?

<details><summary>📝 Đáp án A</summary>

- A1: **(a)** — p < α → Bác bỏ H₀. Chú ý: không "chấp nhận H₀", chỉ "không đủ bằng chứng bác bỏ H₀".
- A2: **Không.** CI 95% nghĩa là: Nếu lặp lại mẫu nhiều lần, 95% các CI tính được sẽ chứa giá trị thật. Không phải "95% cá nhân trong khoảng".
- A3: **Logistic Regression**. Hàm **Sigmoid** $\sigma(z) = 1/(1+e^{-z})$ ép output vào (0,1).
- A4: **Tính chất Markov**. Trong ML: Language Models (biết từ hiện tại → dự đoán từ tiếp theo).
- A5: **Ước lượng bằng mô phỏng ngẫu nhiên**. Lợi thế: Giải được khi không có công thức chính xác (tích phân cao chiều, bài toán phức tạp).

</details>

---

### 🟡 Mức B — Tính toán cơ bản

**B1.** A/B Testing: 1000 người thấy nút xanh (click 80), 1000 người thấy nút đỏ (click 110). Tính z-score và kết luận (α=0.05, z_crit=1.645 cho one-sided).

**B2.** Confidence Interval: Survey 400 người, 52% ủng hộ chính sách. Tính 95% CI. Có thể kết luận đa số ủng hộ chưa?

**B3.** Logistic Regression: $w = [0.5, -0.3]$, $b = -1$. Input $x = [4, 3]$. Tính $P(y=1|x)$.

**B4.** Markov Chain 2 trạng thái (Tốt/Xấu): $P = \begin{pmatrix} 0.8 & 0.2 \\ 0.3 & 0.7 \end{pmatrix}$. Bắt đầu ở Tốt. Xác suất ở Tốt sau 2 bước?

<details><summary>📝 Đáp án B</summary>

**B1:** $p_A = 80/1000 = 0.08$, $p_B = 110/1000 = 0.11$. $p_{pool} = 190/2000 = 0.095$
$SE = \sqrt{0.095 \times 0.905 \times (1/1000 + 1/1000)} = \sqrt{0.000172} = 0.01312$
$z = (0.11 - 0.08)/0.01312 = \mathbf{2.29} > 1.645 \Rightarrow$ **Bác bỏ H₀ — Nút đỏ tốt hơn!**

**B2:** $p = 0.52$, $SE = \sqrt{0.52 \times 0.48/400} = 0.025$. CI = $0.52 \pm 1.96 \times 0.025 = \mathbf{[0.471, 0.569]}$. Chứa 0.5 → **Chưa thể kết luận đa số.**

**B3:** $z = 0.5 \times 4 + (-0.3) \times 3 + (-1) = 2 - 0.9 - 1 = 0.1$. $P(y=1) = \sigma(0.1) = \frac{1}{1+e^{-0.1}} \approx \mathbf{0.525}$ → Biên giới, 52.5% positive.

**B4:** $P^2 = P \times P$. $(P^2)_{11} = 0.8 \times 0.8 + 0.2 \times 0.3 = 0.64 + 0.06 = \mathbf{0.70}$. XS ở Tốt sau 2 bước = **70%**.

</details>

---

### 🔵 Mức C — Giải thích và so sánh

**C1. Kiểm tra kiến thức tổng hợp:** Cho scenario: Công ty chạy A/B test 2 tuần. Nhóm A (cũ): 10,000 user, 320 mua (3.2%). Nhóm B (mới): 10,000 user, 345 mua (3.45%). p-value = 0.23 > 0.05.
- (a) Kết luận thống kê là gì?
- (b) Từ góc độ business, liệu nên deploy B không? Phân tích tradeoff.
- (c) Bao nhiêu user mỗi nhóm mới đủ power để phát hiện sự khác biệt này (statistical power analysis)?

**C2.** Tại sao Logistic Regression dùng Cross-Entropy chứ không phải MSE? Chứng minh bằng lập luận convexity.

**C3.** Phân phối dừng của Markov Chain = eigenvector với eigenvalue = 1. Tại sao? Kết nối với Google PageRank (Ch.1).

---

### 🔴 Mức D — Dự án tổng kết & Capstone

**D-Capstone: Đánh giá ML Model hoàn chỉnh**

Bạn đã train 3 models (Logistic Regression, SVM, Simple Neural Network) trên dataset phân loại email spam. Thực hiện đánh giá đầy đủ:

1. **Hypothesis Testing:** Dùng paired t-test để kiểm tra: "Model A và Model B có accuracy khác nhau có ý nghĩa thống kê không?" (Bootstrap với 1000 lần sampling)

2. **Confidence Interval:** Tính 95% CI cho accuracy của model tốt nhất

3. **A/B Testing:** Giả lập deploy model mới cho 50% user. Sau 1 tuần, một model sai 8.3% email, model kia sai 7.1%. Kiểm định thống kê.

4. **Bias-Variance Analysis:** Vẽ learning curve (training size vs train/val error) để chẩn đoán underfitting hay overfitting.

5. **Model Card:** Viết 1 trang "Model Card" theo chuẩn Google AI: Mô tả model, điều kiện sử dụng, hạn chế, và bias tiềm năng.

---

## 🆘 HỖ TRỢ NGƯỜI TỰ HỌC

### 📚 Tài nguyên học tiếp — Sau khi hoàn thành sách

| Bước tiếp theo | Tài nguyên | Thời gian |
|---------------|-----------|----------|
| **Deep Learning** | fast.ai / CS231n / DL Book | 3-6 tháng |
| **Statistics** | Statistical Learning (ESL/ISL) | 2-4 tháng |
| **Kaggle** | Competitions + Notebooks | Liên tục |
| **Research Papers** | ArXiv / Papers With Code | Hàng tuần |
| **MLOps** | MLflow, DVC, Evidently AI | Khi làm việc thực tế |

### 🏆 Checklist tự đánh giá — TỔNG KẾT TOÀN BỘ SÁCH

Sau khi học xong 8 chương (Ch.0–7), bạn có thể:

**📐 Đại số Tuyến tính (Ch.1)**
- [ ] Tính dot product, nhân ma trận, norm, det, nghịch đảo
- [ ] Giải thích eigenvector/eigenvalue bằng ngôn ngữ đời thường
- [ ] Chạy PCA và SVD, giải thích kết quả

**📉 Giải tích (Ch.2)**
- [ ] Tính đạo hàm riêng, áp dụng Chain Rule
- [ ] Cài Gradient Descent từ đầu cho hàm 2D
- [ ] Giải thích Adam = GD + adaptive lr + momentum

**🎲 Xác suất (Ch.3)**
- [ ] Tính P(A|B) bằng Bayes cho bài toán thực tế
- [ ] Phân biệt 6 phân phối và khi nào dùng
- [ ] Giải thích Cross-Entropy là gì và tại sao dùng

**🔗 Toán Rời rạc (Ch.4)**
- [ ] Code BFS, DFS, Dijkstra từ đầu
- [ ] Tính tổ hợp cho bài toán thực tế

**🤖 Hệ thống AI (Ch.5)**
- [ ] Truy vết từng thành phần AI về đúng chương Toán
- [ ] Code Convolution từ đầu, giải thích FedAvg

**🎯 Tối ưu Nâng cao (Ch.6)**
- [ ] Tính Jacobian, dùng Hessian phân loại điểm dừng
- [ ] Nhận ra vanishing gradient, biết cách fix

**📊 Thống kê Suy diễn (Ch.7)**
- [ ] Thực hiện A/B testing và hypothesis testing đầy đủ
- [ ] Tính CI và giải thích đúng ý nghĩa

> **20+ ✅ → Xuất sắc! Bạn đã làm chủ nền tảng Toán học cho AI.**
> **15+ ✅ → Rất tốt! Ôn thêm 5–6 mục còn thiếu.**
> **< 15 ✅ → Xem lại checklist từng chương.**

---

> 🎓 **Chúc mừng bạn đã hoàn thành giáo trình "Toán học không rào cản"!**
>
> *Bạn đã đi 8 chặng đường từ vector đầu tiên đến hypothesis testing. Mỗi công thức không còn là ký hiệu xa lạ — mà là ngôn ngữ bạn đọc được, hiểu được, và dùng được.*
>
> **Bước tiếp theo:** Hãy mở 1 Kaggle competition, đọc 1 paper ML, hoặc build 1 project nhỏ. Toán học chỉ thật sự sống khi bạn dùng nó!

