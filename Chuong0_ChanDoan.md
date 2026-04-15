# CHƯƠNG 0: BÀI TEST CHẨN ĐOÁN — BẠN ĐANG Ở ĐÂU?

## 📖 DIAGNOSTIC TEST — KNOW YOUR STARTING POINT

> *"Đừng đọc 7 chương nếu bạn chỉ cần 3. Bài test 5 phút này sẽ cho bạn lộ trình CÁ NHÂN HÓA."*

---

## 🎯 HƯỚNG DẪN

- **Thời gian:** 10-15 phút
- **Cách làm:** Chọn đáp án bạn nghĩ đúng. Không tra cứu. Trả lời bằng trực giác.
- **Mục đích:** Xác định điểm mạnh/yếu → Lộ trình đọc tối ưu
- **Tính điểm:** Cuối bài có bảng chấm và hướng dẫn lộ trình

---

## PHẦN A: ĐẠI SỐ TUYẾN TÍNH (Chương 1)

### A1. Vector [3, 4] có độ dài (norm) bằng bao nhiêu?
- (a) 7
- (b) 5
- (c) 12
- (d) √7

### A2. Tích vô hướng (dot product) của [1, 2, 3] và [4, 5, 6] bằng?
- (a) [4, 10, 18]
- (b) 32
- (c) 15
- (d) [5, 7, 9]

### A3. Ma trận A có kích thước 3×4, ma trận B có kích thước 4×2. Kết quả A×B có kích thước?
- (a) 3×2
- (b) 4×4
- (c) Không nhân được
- (d) 3×4

### A4. Nếu det(A) = 0, điều gì đúng?
- (a) A là ma trận đơn vị
- (b) A không có ma trận nghịch đảo
- (c) A có nghịch đảo bằng chính nó
- (d) A là ma trận đường chéo

### A5. Eigenvector của ma trận A là vector v sao cho:
- (a) A + v = λv
- (b) Av = λv (A nhân v chỉ thay đổi độ dài, không đổi hướng)
- (c) A = vλ
- (d) det(A) = v

---

## PHẦN B: GIẢI TÍCH (Chương 2)

### B1. Đạo hàm của f(x) = x³ + 2x là?
- (a) 3x² + 2
- (b) x² + 2
- (c) 3x + 2
- (d) x⁴/4 + x²

### B2. Đạo hàm riêng ∂f/∂x của f(x, y) = x²y + 3y² là?
- (a) 2x + 6y
- (b) 2xy
- (c) x² + 6y
- (d) 2xy + 3y²

### B3. Gradient Descent cập nhật weight theo công thức nào?
- (a) w = w + α × gradient
- (b) w = w − α × gradient
- (c) w = gradient / α
- (d) w = w × gradient

### B4. Ý nghĩa hình học của tích phân $\int_0^3 f(x) dx$ là?
- (a) Độ dốc của f tại x = 3
- (b) Diện tích dưới đường cong f(x) từ 0 đến 3
- (c) Giá trị f(3) − f(0)
- (d) Tiếp tuyến tại x = 0

### B5. Hàm f(x) = x² là hàm lồi (convex) vì:
- (a) Đạo hàm luôn dương
- (b) Đường nối 2 điểm bất kỳ trên đồ thị luôn nằm TRÊN đồ thị
- (c) Hàm luôn tăng
- (d) Hàm có đúng 1 nghiệm

---

## PHẦN C: XÁC SUẤT & THỐNG KÊ (Chương 3)

### C1. Tung đồng xu 2 lần. Xác suất ra 2 mặt ngửa là?
- (a) 1/2
- (b) 1/4
- (c) 1/3
- (d) 1/6

### C2. Phân phối chuẩn (Normal) có dạng hình gì?
- (a) Đường thẳng
- (b) Hình chuông đối xứng
- (c) Đường cong tăng đều
- (d) Hình chữ U

### C3. Theo định lý Bayes, P(A|B) =?
- (a) P(A) + P(B)
- (b) P(B|A) × P(A) / P(B)
- (c) P(A) × P(B)
- (d) P(A) / P(B)

### C4. Cross-Entropy Loss nhỏ có nghĩa là?
- (a) Model dự đoán SAI nhiều
- (b) Model dự đoán GẦN ĐÚNG
- (c) Dữ liệu bị nhiễu
- (d) Learning rate quá lớn

### C5. p-value = 0.03 (α = 0.05) nghĩa là?
- (a) Có 3% khả năng H₀ đúng
- (b) Bác bỏ H₀ — Kết quả có ý nghĩa thống kê
- (c) Không bác bỏ H₀
- (d) Cần thêm dữ liệu

---

## PHẦN D: TOÁN RỜI RẠC (Chương 4)

### D1. Trong đồ thị (graph), "bậc" (degree) của một đỉnh là?
- (a) Số đỉnh trong đồ thị
- (b) Số cạnh nối với đỉnh đó
- (c) Khoảng cách đến đỉnh xa nhất
- (d) Giá trị lớn nhất trên đỉnh

### D2. BFS (Breadth-First Search) tìm đường ngắn nhất theo:
- (a) Số cạnh (không trọng số)
- (b) Tổng trọng số nhỏ nhất
- (c) Chiều sâu đệ quy
- (d) Số đỉnh trên đường đi

### D3. Big-O của binary search là?
- (a) O(1)
- (b) O(n)
- (c) O(log n)
- (d) O(n²)

---

## PHẦN E: TỐI ƯU HÓA NÂNG CAO (Chương 6)

### E1. Jacobian matrix có kích thước m×n khi hàm f: Rⁿ → Rᵐ. Nó chứa gì?
- (a) Giá trị của hàm f
- (b) Tất cả đạo hàm riêng ∂fᵢ/∂xⱼ
- (c) Eigenvalue của f
- (d) Nghịch đảo của f

### E2. Hessian matrix dương xác định (all eigenvalues > 0) tại điểm dừng nghĩa là?
- (a) Cực đại
- (b) Yên ngựa (saddle point)
- (c) Cực tiểu
- (d) Không xác định

### E3. Vanishing gradient xảy ra vì?
- (a) Learning rate quá lớn
- (b) Sigmoid có đạo hàm max = 0.25, nhân nhiều lần → gradient ≈ 0
- (c) Dữ liệu bị thiếu
- (d) Ma trận weight quá lớn

---

## PHẦN F: THỐNG KÊ SUY DIỄN (Chương 7)

### F1. Confidence Interval 95% nghĩa là?
- (a) 95% dữ liệu nằm trong khoảng
- (b) Nếu lặp lại thí nghiệm 100 lần, 95 lần CI chứa giá trị thật
- (c) Xác suất đúng = 95%
- (d) Sai số = 5%

### F2. Logistic Regression dùng hàm gì để output xác suất?
- (a) ReLU
- (b) Sigmoid (ép output vào 0-1)
- (c) Softmax
- (d) Linear

### F3. Tính chất Markov nói rằng:
- (a) Tương lai phụ thuộc toàn bộ quá khứ
- (b) Tương lai chỉ phụ thuộc hiện tại, không phụ thuộc quá khứ
- (c) Tương lai hoàn toàn ngẫu nhiên
- (d) Mọi trạng thái đều bằng nhau

---

## 📝 BẢNG ĐÁP ÁN

<details>
<summary>👉 Click để xem đáp án</summary>

| Câu | Đáp án | Giải thích ngắn |
|-----|--------|-----------------|
| A1 | **(b) 5** | √(3² + 4²) = √(9+16) = √25 = 5 |
| A2 | **(b) 32** | 1×4 + 2×5 + 3×6 = 4+10+18 = 32 |
| A3 | **(a) 3×2** | (m×n) × (n×p) = (m×p) |
| A4 | **(b)** | det=0 → Singular → Không có nghịch đảo |
| A5 | **(b)** | Định nghĩa: Av = λv |
| B1 | **(a) 3x²+2** | Power rule: d/dx(xⁿ) = nxⁿ⁻¹ |
| B2 | **(b) 2xy** | Giữ y cố định, đạo hàm theo x |
| B3 | **(b)** | w = w − α × ∇f (đi NGƯỢC gradient) |
| B4 | **(b)** | Tích phân = Diện tích dưới đường cong |
| B5 | **(b)** | Định nghĩa hàm lồi |
| C1 | **(b) 1/4** | 1/2 × 1/2 = 1/4 (independent events) |
| C2 | **(b)** | Bell curve / Hình chuông |
| C3 | **(b)** | Công thức Bayes |
| C4 | **(b)** | CE nhỏ → dự đoán gần nhãn thật |
| C5 | **(b)** | p < α → Bác bỏ H₀ |
| D1 | **(b)** | Degree = số cạnh kề |
| D2 | **(a)** | BFS = đường ngắn nhất (theo số cạnh) |
| D3 | **(c)** | Chia đôi mỗi bước → O(log n) |
| E1 | **(b)** | Jacobian = ma trận đạo hàm riêng |
| E2 | **(c)** | All eigenvalues > 0 → Cực tiểu |
| E3 | **(b)** | σ'(x) ≤ 0.25, nhân qua n layers → 0 |
| F1 | **(b)** | Frequentist interpretation |
| F2 | **(b)** | σ(z) = 1/(1+e⁻ᶻ) ∈ (0,1) |
| F3 | **(b)** | P(Xₜ₊₁|Xₜ, Xₜ₋₁,...) = P(Xₜ₊₁|Xₜ) |

</details>

---

## 🗺️ LỘ TRÌNH CÁ NHÂN HÓA

### Cách tính điểm

| Phần | Số câu đúng | Trình độ |
|------|-------------|----------|
| A (ĐSTT) | 4-5/5 | ✅ Vững — Đọc lướt Ch.1 |
| A (ĐSTT) | 2-3/5 | ⚠️ Trung bình — Đọc kỹ phần yếu |
| A (ĐSTT) | 0-1/5 | 🔴 Cần học kỹ — Đọc từ đầu Ch.1 |

### Lộ trình theo kết quả

#### 🟢 Nếu đúng ≥ 18/22: "Bạn đã có nền tảng tốt!"
```
Lộ trình: Ch.5 (Tổng hợp) → Ch.6 (Tối ưu nâng cao) → Ch.7 (Suy diễn)
Đọc lướt: Ch.1-4 (chỉ xem phần 🆕)
Thời gian: ~10 giờ
```

#### 🟡 Nếu đúng 10-17/22: "Nền tảng ổn, cần bổ sung"
```
Lộ trình: Đọc kỹ các chương BỊ SAI → Ch.5 → Ch.6 → Ch.7
Ưu tiên phần có ❌ nhiều nhất
Thời gian: ~20 giờ
```

#### 🔴 Nếu đúng < 10/22: "Không sao — giáo trình này viết cho bạn!"
```
Lộ trình: Ch.1 → Ch.2 → Ch.3 → Ch.4 → Ch.5 → Ch.6 → Ch.7 (tuần tự)
Mỗi chương làm đủ bài tập trước khi qua chương tiếp
Thời gian: ~40 giờ
```

---

## 📋 BẢN ĐỒ PHỤ THUỘC KIẾN THỨC

```
                    ┌──────────────────┐
                    │ Ch.0: Chẩn đoán  │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
      ┌───────────┐  ┌───────────┐  ┌───────────┐
      │ Ch.1 ĐSTT │  │ Ch.3 XSTK │  │ Ch.4 TRR  │
      │ (Vector,  │  │ (XS, Phân │  │ (Graph,   │
      │  Ma trận) │  │  phối)    │  │  Logic)   │
      └─────┬─────┘  └─────┬─────┘  └───────────┘
            │               │
            ▼               │
      ┌───────────┐         │
      │ Ch.2 GT   │◄────────┘
      │ (Đạo hàm, │
      │  GD, Tích  │
      │  phân)    │
      └─────┬─────┘
            │
     ┌──────┼──────────────────────┐
     ▼      ▼                      ▼
┌─────────┐ ┌──────────────┐ ┌─────────────┐
│ Ch.5    │ │ Ch.6 Tối ưu  │ │ Ch.7 Suy    │
│ Tổng hợp│ │ nâng cao     │ │ diễn        │
└─────────┘ │ (Jacobian,   │ │ (Testing,   │
            │  Lagrange)   │ │  Logistic)  │
            └──────────────┘ └─────────────┘
```

> 💡 **Quy tắc:** Mũi tên A → B nghĩa là "cần hiểu A trước khi đọc B". Ch.1 (ĐSTT) và Ch.3 (XSTK) có thể đọc song song.

---

> *"Bạn không cần giỏi toán để bắt đầu. Bạn chỉ cần biết BẮT ĐẦU TỪ ĐÂU."*
