# CHƯƠNG 4: TOÁN RỜI RẠC — CẤU TRÚC CỦA THẾ GIỚI SỐ

## 📋 Metadata Chương

| Mục | Chi tiết |
|-----|---------|
| **Tên chương** | Chương 4: Toán Rời rạc — Cấu trúc của Thế giới số |
| **Hook** | GPS, mạng xã hội, blockchain, bộ nhớ máy tính — tất cả đều xây dựng trên một ý tưởng: biến thế giới thành chấm và đường nối. |
| **Đối tượng** | Sinh viên CNTT, Software Engineering; người học thuật toán và cấu trúc dữ liệu |
| **Điều kiện tiên quyết** | Ch.0 (tư duy logic), Ch.1 §1.3 (Ma trận kề). |
| **Thời gian học** | ~60 phút (Track 🟢🟡) \| ~90 phút (Track 🔵) |
| **Mục tiêu đầu ra** | Sau chương này, người học có thể: |
| | • **Xây dựng** đồ thị bằng ma trận kề và danh sách kề, biết khi nào dùng cái nào |
| | • **Cài đặt** BFS và DFS — giải thích sự khác biệt và ứng dụng mỗi loại |
| | • **Chạy tay** thuật toán Dijkstra trên đồ thị nhỏ, code GPS Navigator |
| | • **Áp dụng** logic mệnh đề để viết điều kiện phức tạp trong code |
| | • **Tính toán** tổ hợp, chỉnh hợp — ứng dụng trong phân tích độ phức tạp và bảo mật |

---

## 📖 DISCRETE MATHEMATICS — THE STRUCTURE OF THE DIGITAL WORLD


> *"Google Maps tìm đường đi ngắn nhất cho 2 tỷ chuyến đi mỗi ngày. Facebook kết nối 3 tỷ người. Tất cả dựa trên một ý tưởng: biến thế giới thành các chấm và đường nối — gọi là Đồ thị."*

---

> 🧬 **HÀNH TRÌNH NEURON N-42** — Tập 4: "Kết bạn thành mạng lưới"
>
> *N-42 giờ đã mạnh — biết nhân ma trận, biết GD, biết softmax. Nhưng MỘT neuron thì yếu. Sức mạnh thật sự đến khi N-42 KẾT NỐI với hàng nghìn neuron khác thành MẠNG NEURAL — một đồ thị có hướng (DAG - Directed Acyclic Graph, nghĩa là dòng dữ liệu chỉ chảy xuôi về phía trước không quay đầu). Chương này, N-42 sẽ hiểu cấu trúc mạng lưới mà mình đang sống trong đó.*

---

## 🎬 CÂU CHUYỆN MỞ ĐẦU: GPS và bài toán 300 năm tuổi

Sáng nay, bạn mở Google Maps. Gõ "Nhà → Trường". Trong 0.3 giây, nó hiển thị 3 tuyến đường, kẹt xe ở đâu, mất bao lâu.

Đằng sau sự "nhanh gọn" đó là một bài toán mà nhà toán học Euler đã đặt ra năm 1736: **"Bài toán Cầu Königsberg"** — Có thể đi qua tất cả 7 cây cầu trong thành phố mà không đi qua cầu nào hai lần không?

Euler chứng minh: KHÔNG THỂ. Và trong quá trình chứng minh, ông phát minh ra **Lý thuyết Đồ thị** — nền tảng của mọi hệ thống mạng hiện đại.

> 💡 **Takeaway:** Toán rời rạc không phải "toán cổ xưa". Nó là nền tảng của Internet, GPS, mạng xã hội, blockchain, và mọi thuật toán bạn sẽ viết.

---

## ⚡ 5-MINUTE REVIEW — Chương này bạn sẽ học gì?

| # | Khái niệm | Một câu tóm tắt | Tại sao cần cho AI/CNTT |
|---|-----------|-----------------|---------------------|
| 1 | **Đồ thị (Graph)** | Mạng lưới các đỉnh nối với nhau qua cạnh | Biểu diễn mạng Nơ-ron, Mạng xã hội |
| 2 | **BFS & DFS** | Hai thuật toán duyệt cấu trúc đồ thị cơ bản | Tìm đường, Phân tích dữ liệu mạng |
| 3 | **Dijkstra** | Thuật toán tìm đường đi ngắn nhất có cực trị | Nền tảng của Routing và Navigation |
| 4 | **Logic Mệnh Đề** | Toán học của thao tác True/False | Toán học đằng sau if/else, Bitwise |
| 5 | **Tổ Hợp** | Đếm số lượng tùy chọn / trạng thái mô hình | Feature selection, Parameter tunning |

> 📋 **Prerequisites:** Ch.0 (Tư duy logic cơ bản), Ch.1 §1.2 (Ma trận cơ bản)
>
> ⏱️ **Thời gian đọc:** ~60 phút (Track A — Developer) | ~90 phút (Track B — Researcher)

---

## 🗺️ BẠN ĐANG Ở ĐÂY

```
  Ch.2 Giải tích ━━▶ Ch.3 Xác suất ━━▶ 📍 Ch.4 TOÁN RỜI RẠC ━━▶ Ch.5 Tư duy HT ━━▶ ...
  (✅ Done)         (✅ Done)             BẠN ĐANG Ở ĐÂY       (Tiếp theo)
```

**Phần này cần học trước:** Ch.1 (Ma trận để hiểu Ma trận kề), logic cơ bản từ cấp 3.
**Chương này mở khóa:** Ch.5 (Biểu diễn Mạng Nơ-ron thành một Graph lớn). Cấu trúc dữ liệu và giải thuật 101.
**Bạn sẽ cần lại kiến thức này khi:** Phỏng vấn thuật toán (LeetCode), thiết kế Data pipeline, thiết kế Navigation GPS, đọc Graph Neural Networks.

---

## 📦 ÔN NHANH TOÁN PHỔ THÔNG

> *Không nhớ gì về tập hợp hay hoán vị? Đừng lo. Hãy mở các hộp kiến thức bên dưới nhé!*

<details>
<summary>🔢 <b>Ôn nhanh 1: Tập hợp (Set) — "Cái túi đựng đồ"</b> (Bấm để mở)</summary>

Tập hợp là một bộ sưu tập các phần tử phân biệt. Không quan trọng thứ tự.

- Tập hợp A là tập các số chẵn nhỏ hơn 6: $A = \{2, 4\}$
- Kí hiệu thuộc: $4 \in A$ (4 thuộc A)
- Kí hiệu Tập hợp con: Nếu mọi thứ trong B đều ở trong A thì $B \subset A$

> 💡 Trong đồ thị, Tập Đỉnh và Tập Cạnh đều là các tập hợp!

</details>

<details>
<summary>📏 <b>Ôn nhanh 2: Phép Đếm, Giai thừa và Hoán vị</b></summary>

**Giai thừa ($n!$)**: Tích các số từ 1 đến n. 
- $3! = 3 \times 2 \times 1 = 6$
- Giả sử bạn có hộp gồm 3 quả bóng xanh, đỏ, vàng. Hỏi có bao nhiêu cách xếp liên tiếp? Lần thứ nhất chọn: có 3 quả. Lần thứ hai còn 2 quả. Lần thứ 3 còn 1 quả. Tổng cộng: $3 \times 2 \times 1 = 6$ cách xếp. Đây chính là **Số Hoán Vị**.

> 💡 Tổ hợp và chỉnh hợp sẽ được làm rõ hơn ở phần 4! Nếu bạn biết tính $n!$ là đủ dùng.

</details>

---

## 🏷️ HƯỚNG DẪN ĐỌC — Hệ thống 3 tầng

| Tầng | Label | Dành cho ai? | Cần gì? |
|------|-------|-------------|--------|
| 🟢 | **Hiểu bằng trực giác** | Tất cả mọi người | Tư duy logic cơ bản |
| 🟡 | **Hiểu bằng định nghĩa Toán** | Muốn phân tích sâu | Có kiến thức về Tập Hợp |
| 🔵 | **Hiểu bằng code Python** | Developer / Coder | Cần biết Python List, Dict |

> 💡 **Mẹo:** Đọc tầng 🟢, thử giải trong trí tưởng tượng các Ví dụ vỡ lòng, trước khi đi qua 🟡 lý thuyết hàn lâm. Code phần 🔵 có thể chạy trực tiếp.

---

## PHẦN 1: ĐỒ THỊ (GRAPH) — MẠNG LƯỚI KẾT NỐI

### 🟢 1.1 Đồ thị là gì?

Đồ thị gồm 2 thành phần:
- **Đỉnh (Vertex/Node):** Các đối tượng — người, thành phố, trang web
- **Cạnh (Edge):** Các mối quan hệ — đường đi, kết bạn, hyperlink

$$G = (V, E)$$

Trong đó $V$ = tập đỉnh, $E$ = tập cạnh.

#### 🎯 Liên tưởng: Đồ thị ở khắp nơi

| Hệ thống | Đỉnh (Node) | Cạnh (Edge) |
|----------|-------------|-------------|
| **Facebook** | Người dùng | Kết bạn |
| **Google Maps** | Ngã tư | Con đường |
| **Internet** | Trang web | Hyperlink |
| **Não bộ** | Neuron | Synapse |
| **Vũ trụ MCU** | Nhân vật | Cùng xuất hiện |
| **🏥 Truy vết COVID** | Người bệnh | Tiếp xúc gần |
| **📚 Tiên quyết môn** | Môn học | Môn A là tiên quyết của B |
| **🌾 Chuỗi cung ứng** | Nông trại, Kho, Chợ | Đường vận chuyển |
| **📈 Thị trường** | Công ty | Quan hệ cung-cầu |

> 🌱 **VÍ DỤ VỠ LÒNG — Vẽ tay 1 đồ thị (30 giây)**
>
> 1. Vẽ 3 chấm nhỏ lên giấy. Đặt tên chúng là: A, B, C.
> 2. Dùng bút gạch 1 đường nối A với B. Dùng bút gạch 1 đường nối B với C.
>
> Xin chúc mừng! Bạn vừa tạo ra đồ thị: Đỉnh V={A, B, C}. Cạnh E={(A,B), (B,C)}.

### 🟡 1.2 Các loại đồ thị

| Loại | Đặc điểm | Ví dụ |
|------|----------|-------|
| **Vô hướng** | Cạnh 2 chiều | Facebook (A kết bạn B ↔ B kết bạn A) |
| **Có hướng** | Cạnh 1 chiều | Twitter (A follow B ≠ B follow A) |
| **Có trọng số** | Cạnh có "giá trị" | Google Maps (khoảng cách km) |
| **Cây (Tree)** | Đồ thị không chu trình | Cấu trúc thư mục, DOM HTML |

> ⚠️ **SAI LẦM PHỔ BIẾN:** Đừng nhầm lẫn giữa Đồ thị có hướng (Directed) và Đồ thị vô hướng (Undirected) khi viết code! Trong vô hướng, Cạnh A-B có nghĩa là từ A sang B VÀ B sang A. Trong có hướng, mũi tên chỉ đi 1 chiều. Khai báo danh sách kề sai chiều sẽ làm toán toàn bộ thuật toán đi chệch hướng!

### 🟡 1.3 Biểu diễn đồ thị trong máy tính

#### Ma trận kề (Adjacency Matrix)

$$A_{ij} = \begin{cases} 1 & \text{nếu có cạnh từ i đến j} \\ 0 & \text{ngược lại} \end{cases}$$

```
     A  B  C  D
A  [ 0  1  1  0 ]
B  [ 1  0  1  1 ]
C  [ 1  1  0  0 ]
D  [ 0  1  0  0 ]
```

→ Chính là **ma trận** từ Chương 1! Đại số tuyến tính lại đây rồi! 😄

#### Danh sách kề (Adjacency List)

```
A → [B, C]
B → [A, C, D]
C → [A, B]
D → [B]
```

→ Tiết kiệm bộ nhớ hơn khi đồ thị "thưa" (ít cạnh).

### 🔵 1.4 Code Python: Xây dựng đồ thị

```python
import numpy as np
import matplotlib.pyplot as plt
import heapq
from collections import deque, defaultdict

# =============================================
# 1. XÂY DỰNG ĐỒ THỊ
# =============================================

class DoThi:
    """Đồ thị có trọng số (vô hướng)"""
    
    def __init__(self):
        self.danh_sach_ke = defaultdict(list)
        self.dinh = set()
    
    def them_canh(self, u, v, trong_so=1):
        """Thêm cạnh giữa u và v"""
        self.danh_sach_ke[u].append((v, trong_so))
        self.danh_sach_ke[v].append((u, trong_so))
        self.dinh.add(u)
        self.dinh.add(v)
    
    def in_do_thi(self):
        """In đồ thị"""
        print("📊 Đồ thị:")
        for dinh in sorted(self.dinh):
            ke = [(v, w) for v, w in self.danh_sach_ke[dinh]]
            print(f"   {dinh} → {ke}")

# Tạo bản đồ thành phố
ban_do = DoThi()
ban_do.them_canh("Nhà", "Trường", 3)
ban_do.them_canh("Nhà", "Siêu thị", 2)
ban_do.them_canh("Nhà", "Công viên", 5)
ban_do.them_canh("Trường", "Thư viện", 1)
ban_do.them_canh("Trường", "Quán cà phê", 2)
ban_do.them_canh("Siêu thị", "Bệnh viện", 4)
ban_do.them_canh("Siêu thị", "Quán cà phê", 3)
ban_do.them_canh("Công viên", "Bệnh viện", 2)
ban_do.them_canh("Thư viện", "Quán cà phê", 1)
ban_do.them_canh("Bệnh viện", "Quán cà phê", 6)

ban_do.in_do_thi()

# =============================================
# 2. TRỰC QUAN HÓA ĐỒ THỊ
# =============================================

# Vị trí các đỉnh (layout thủ công cho đẹp)
positions = {
    "Nhà": (0, 0),
    "Trường": (3, 2),
    "Siêu thị": (2, -1),
    "Công viên": (-1, 3),
    "Thư viện": (5, 3),
    "Quán cà phê": (4, 0),
    "Bệnh viện": (1, -3)
}

fig, ax = plt.subplots(figsize=(12, 8))

# Vẽ cạnh
for u in ban_do.danh_sach_ke:
    for v, w in ban_do.danh_sach_ke[u]:
        x1, y1 = positions[u]
        x2, y2 = positions[v]
        ax.plot([x1, x2], [y1, y2], 'gray', linewidth=2, alpha=0.5)
        # Trọng số
        mx, my = (x1+x2)/2, (y1+y2)/2
        ax.annotate(f'{w}km', xy=(mx, my), fontsize=10, 
                   ha='center', va='center',
                   bbox=dict(boxstyle='round,pad=0.3', facecolor='yellow', alpha=0.8))

# Vẽ đỉnh
colors_map = {
    "Nhà": '#4CAF50', "Trường": '#2196F3', "Siêu thị": '#FF9800',
    "Công viên": '#8BC34A', "Thư viện": '#9C27B0', 
    "Quán cà phê": '#795548', "Bệnh viện": '#f44336'
}

icons = {
    "Nhà": "🏠", "Trường": "🏫", "Siêu thị": "🛒",
    "Công viên": "🌳", "Thư viện": "📚", 
    "Quán cà phê": "☕", "Bệnh viện": "🏥"
}

for dinh, (x, y) in positions.items():
    color = colors_map.get(dinh, '#607D8B')
    ax.plot(x, y, 'o', markersize=30, color=color, markeredgecolor='white', 
            markeredgewidth=2, zorder=5)
    ax.annotate(f'{icons[dinh]} {dinh}', xy=(x, y-0.5), fontsize=10,
               ha='center', va='top', fontweight='bold')

ax.set_title("🗺️ Bản đồ thành phố — Đồ thị có trọng số", 
             fontsize=15, fontweight='bold')
ax.set_xlim(-2.5, 6.5); ax.set_ylim(-4.5, 4.5)
ax.set_aspect('equal')
ax.grid(True, alpha=0.2)
ax.axis('off')
plt.tight_layout()
plt.savefig('chuong4_do_thi.png', dpi=150, bbox_inches='tight')
plt.show()
```

> 📌 **Lưu ý cho giảng viên:** Hãy cho sinh viên tự vẽ bản đồ khu vực quanh trường (5-7 địa điểm) bằng đồ thị, rồi chuyển sang biểu diễn ma trận kề. Hoạt động thực tế này giúp hiểu sâu hơn.

---

> ✅ **CHECKPOINT 1 — Đồ thị Cơ Bản**
>
> 1. Mạng lưới Follower trên Instagram (A follow B không có nghĩa là B follow A) là đồ thị Vô Hướng hay Có Hướng?
> 2. Sự khác nhau giữa Ma Trận Kề và Danh Sách Kề là gì? Chén bộ nhớ nhiều hơn đối với cái nào?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **Có Hướng (Directed Graph).** Vì Follow là quan hệ một chiều.
> 2. Ma trận kề lưu một bảng kích thước VxV, bất kể có bao nhiêu cạnh → Tốn $O(V^2)$ bộ nhớ. Danh sách kề chỉ lưu những đỉnh có cạnh kết nối thực sự → Ít tốn bộ nhớ hơn rất nhiều nếu đồ thị **thưa thớt**.
>
> </details>
>
> **Tự đánh giá:** Đúng 2/2 → 🟢 Tuyệt! | 1/2 → 🟡 Ổn | 0/2 → 🔴 Đọc lại Phần 1

---

## PHẦN 2: BFS & DFS — HAI CÁCH "KHÁM PHÁ" ĐỒ THỊ

### 🟢 2.1 BFS — Duyệt theo chiều rộng

#### 🎯 Liên tưởng: Ném đá xuống ao

Khi bạn ném đá xuống ao, sóng lan ra theo **vòng tròn**:
- Vòng 1: Các điểm cách tâm 1 bước
- Vòng 2: Các điểm cách tâm 2 bước
- ...

BFS cũng vậy: từ điểm xuất phát, duyệt **tất cả láng giềng** trước, rồi mới đi xa hơn.

→ **Ứng dụng:** Tìm đường đi ngắn nhất (không trọng số), "Gợi ý bạn chung" trên Facebook.

### 🟢 2.2 DFS — Duyệt theo chiều sâu

#### 🎯 Liên tưởng: Đi trong mê cung

Khi lạc trong mê cung, bạn:
1. Chọn 1 ngã rẽ, đi **sâu nhất** có thể
2. Khi đến ngõ cụt → **quay lại** (backtrack)
3. Chọn ngã rẽ khác chưa đi

→ **Ứng dụng:** Kiểm tra tính liên thông, phát hiện chu trình, giải Sudoku.

> 🌱 **VÍ DỤ VỠ LÒNG — BFS/DFS thủ công (30 giây)**
>
> Đồ thị đơn giản: $A \rightarrow B, A \rightarrow C, B \rightarrow D$.
> Bạn xuất phát từ $A$.
> - **BFS:** Lan rộng ra khỏi A, thăm B và C trước. Sau đó mới đi tiếp thăm D. (Thứ tự thăm: A, B, C, D)
> - **DFS:** Đi sâu vào B, tự nhiên thấy tới cụt đường D, quay lại C. (Thứ tự thăm: A, B, D, C)

> ⚠️ **SAI LẦM PHỔ BIẾN:** BFS và DFS dễ bị chạy thành "Vòng lặp Vô hạn" nếu đồ thị có chu trình tròn. Bạn phải KHẮC CỐT GHI TÂM luôn duy trì một Danh Sách "Đã Thăm" (Visited Set).

### 🔵 2.3 Code Python: BFS & DFS

```python
from collections import deque

# =============================================
# BFS & DFS TRỰC QUAN
# =============================================

def bfs(do_thi, bat_dau):
    """
    Breadth-First Search — Duyệt theo chiều rộng
    Như sóng lan trên mặt nước 🌊
    """
    da_tham = set()
    hang_doi = deque([bat_dau])  # Queue (FIFO)
    da_tham.add(bat_dau)
    thu_tu = []
    cap_do = {bat_dau: 0}
    
    while hang_doi:
        dinh = hang_doi.popleft()  # Lấy từ đầu queue
        thu_tu.append((dinh, cap_do[dinh]))
        
        for ke, _ in sorted(do_thi.danh_sach_ke[dinh]):
            if ke not in da_tham:
                da_tham.add(ke)
                hang_doi.append(ke)
                cap_do[ke] = cap_do[dinh] + 1
    
    return thu_tu

def dfs(do_thi, bat_dau):
    """
    Depth-First Search — Duyệt theo chiều sâu
    Như đi trong mê cung 🏰
    """
    da_tham = set()
    ngan_xep = [bat_dau]  # Stack (LIFO)
    thu_tu = []
    
    while ngan_xep:
        dinh = ngan_xep.pop()  # Lấy từ đỉnh stack
        
        if dinh not in da_tham:
            da_tham.add(dinh)
            thu_tu.append(dinh)
            
            # Thêm các đỉnh kề (đảo thứ tự để duyệt alphabetical)
            for ke, _ in sorted(do_thi.danh_sach_ke[dinh], reverse=True):
                if ke not in da_tham:
                    ngan_xep.append(ke)
    
    return thu_tu

# Chạy BFS và DFS
print("🌊 BFS (Sóng lan ra):")
bfs_result = bfs(ban_do, "Nhà")
for dinh, cap in bfs_result:
    indent = "  " * cap
    print(f"   {indent}[Cấp {cap}] {icons[dinh]} {dinh}")

print(f"\n🏰 DFS (Đi sâu nhất có thể):")
dfs_result = dfs(ban_do, "Nhà")
for i, dinh in enumerate(dfs_result):
    print(f"   Bước {i+1}: {icons[dinh]} {dinh}")

# Trực quan hóa so sánh
fig, axes = plt.subplots(1, 2, figsize=(16, 7))

for ax, (title, result, color_scheme) in zip(axes, [
    ("🌊 BFS — Sóng lan dần", bfs_result, plt.cm.Blues),
    ("🏰 DFS — Đi sâu nhất", [(d, i) for i, d in enumerate(dfs_result)], plt.cm.Reds)
]):
    # Vẽ cạnh
    for u in ban_do.danh_sach_ke:
        for v, w in ban_do.danh_sach_ke[u]:
            x1, y1 = positions[u]
            x2, y2 = positions[v]
            ax.plot([x1, x2], [y1, y2], 'gray', linewidth=1.5, alpha=0.3)
    
    # Vẽ đỉnh với thứ tự đánh số
    if title.startswith("🌊"):
        order_dict = {d: i for i, (d, _) in enumerate(result)}
    else:
        order_dict = {d: i for i, (d, _) in enumerate(result)}
    
    n = len(result)
    for dinh, (x, y) in positions.items():
        idx = order_dict.get(dinh, 0)
        c = color_scheme(0.3 + 0.7 * idx / max(n-1, 1))
        ax.plot(x, y, 'o', markersize=35, color=c, 
                markeredgecolor='white', markeredgewidth=2, zorder=5)
        ax.annotate(f'{idx+1}', xy=(x, y), fontsize=14, ha='center', va='center',
                   fontweight='bold', color='white', zorder=6)
        ax.annotate(f'{icons[dinh]} {dinh}', xy=(x, y-0.6), fontsize=9,
                   ha='center', va='top')
    
    ax.set_title(title, fontsize=14, fontweight='bold')
    ax.set_xlim(-2.5, 6.5); ax.set_ylim(-4.5, 4.5)
    ax.axis('off')

plt.suptitle("So sánh BFS vs DFS — Cùng đồ thị, khác cách duyệt", 
             fontsize=15, fontweight='bold')
plt.tight_layout()
plt.savefig('chuong4_bfs_dfs.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## PHẦN 3: DIJKSTRA — TÌM ĐƯỜNG ĐI NGẮN NHẤT

### 🟢 3.1 Bài toán

Cho đồ thị có trọng số (mỗi cạnh có "chi phí"), tìm đường đi từ A đến B sao cho **tổng trọng số nhỏ nhất**.

Đây chính là bài toán mà **Google Maps giải mỗi ngày**.

### 🟡 3.2 Thuật toán Dijkstra — Ý tưởng

1. Bắt đầu: Khoảng cách đến điểm xuất phát = 0, tất cả điểm khác = ∞
2. Chọn đỉnh **gần nhất** chưa xét
3. Cập nhật khoảng cách đến các đỉnh kề (nếu tìm được đường ngắn hơn)
4. Lặp lại cho đến khi xét hết hoặc đến đích

#### 🎯 Liên tưởng: Ngọn lửa lan trên bản đồ

Hãy tưởng tượng bạn đốt lửa tại điểm xuất phát. Lửa lan dọc theo các con đường — đường ngắn hơn lửa đến trước. Khi lửa đến đích, **con đường mà lửa đi** chính là đường ngắn nhất!

> 🌱 **VÍ DỤ VỠ LÒNG — Chạy Dijkstra bằng tay (1 phút)**
> 
> Đồ thị: A nối B (dài 3), A nối C (dài 5). B nối C (dài 1). Tìm đường ngắn nhất từ A tới C?
> - Xuất phát tại A (0km). Xung quanh A có B(3km) và C(5km).
> - Chọn đỉnh gần nhất là B. Khoảng cách hiện tại đến B là 3km.
> - Từ B, ta có thể đến C bằng 1km. Vậy đường A $\rightarrow$ B $\rightarrow$ C dài 3 + 1 = **4km**.
> - 4km ngắn hơn 5km (nếu đi thẳng A $\rightarrow$ C). Do đó, con đường ngắn nhất đến C là đi qua B!

### 🔵 3.3 🎓 MINI PROJECT: GPS Navigator

```python
import numpy as np
import matplotlib.pyplot as plt
import heapq

# =============================================
# THUẬT TOÁN DIJKSTRA — GPS NAVIGATOR
# =============================================

def dijkstra(do_thi, bat_dau, ket_thuc):
    """
    Thuật toán Dijkstra — Tìm đường đi ngắn nhất
    
    Tham số:
        do_thi: đối tượng DoThi
        bat_dau: đỉnh xuất phát
        ket_thuc: đỉnh đích
    
    Trả về:
        khoang_cach: dict {đỉnh: khoảng cách ngắn nhất}
        duong_di: danh sách đỉnh trên đường ngắn nhất
    """
    # Bước 1: Khởi tạo
    khoang_cach = {dinh: float('inf') for dinh in do_thi.dinh}
    khoang_cach[bat_dau] = 0
    truoc_do = {}  # Để truy vết đường đi
    da_xet = set()
    
    # Priority Queue (Min-Heap)
    pq = [(0, bat_dau)]  # (khoảng cách, đỉnh)
    
    buoc = 0
    print(f"\n🗺️ Dijkstra: {bat_dau} → {ket_thuc}")
    print("=" * 60)
    
    while pq:
        dist_u, u = heapq.heappop(pq)
        
        if u in da_xet:
            continue
        
        da_xet.add(u)
        buoc += 1
        print(f"   Bước {buoc}: Xét {icons.get(u, '📍')} {u} (khoảng cách = {dist_u})")
        
        if u == ket_thuc:
            break
        
        # Bước 2: Cập nhật khoảng cách đến các đỉnh kề
        for v, w in do_thi.danh_sach_ke[u]:
            if v not in da_xet:
                kc_moi = dist_u + w
                if kc_moi < khoang_cach[v]:
                    khoang_cach[v] = kc_moi
                    truoc_do[v] = u
                    heapq.heappush(pq, (kc_moi, v))
                    print(f"         ↳ Cập nhật {v}: {khoang_cach[v]} km")
    
    # Truy vết đường đi
    duong_di = []
    current = ket_thuc
    while current in truoc_do:
        duong_di.append(current)
        current = truoc_do[current]
    duong_di.append(bat_dau)
    duong_di.reverse()
    
    return khoang_cach, duong_di

# Chạy Dijkstra
khoang_cach, duong_di = dijkstra(ban_do, "Nhà", "Thư viện")

print(f"\n🏆 Kết quả:")
print(f"   Đường ngắn nhất: {' → '.join(duong_di)}")
print(f"   Tổng khoảng cách: {khoang_cach['Thư viện']} km")

# =============================================
# TRỰC QUAN HÓA ĐƯỜNG ĐI NGẮN NHẤT
# =============================================

fig, ax = plt.subplots(figsize=(12, 8))

# Vẽ tất cả cạnh (mờ)
for u in ban_do.danh_sach_ke:
    for v, w in ban_do.danh_sach_ke[u]:
        x1, y1 = positions[u]
        x2, y2 = positions[v]
        ax.plot([x1, x2], [y1, y2], color='lightgray', linewidth=2, zorder=1)
        mx, my = (x1+x2)/2, (y1+y2)/2
        ax.annotate(f'{w}km', xy=(mx, my), fontsize=9, alpha=0.5,
                   ha='center', va='center',
                   bbox=dict(boxstyle='round,pad=0.2', facecolor='white', alpha=0.7))

# Vẽ đường đi ngắn nhất (nổi bật)
for i in range(len(duong_di) - 1):
    u, v = duong_di[i], duong_di[i+1]
    x1, y1 = positions[u]
    x2, y2 = positions[v]
    ax.annotate('', xy=(x2, y2), xytext=(x1, y1),
               arrowprops=dict(arrowstyle='->', color='#f44336', lw=4),
               zorder=3)

# Vẽ đỉnh
for dinh, (x, y) in positions.items():
    if dinh in duong_di:
        color = '#4CAF50' if dinh in [duong_di[0], duong_di[-1]] else '#FF9800'
        size = 40
    else:
        color = '#E0E0E0'
        size = 25
    
    ax.plot(x, y, 'o', markersize=size, color=color, 
            markeredgecolor='white', markeredgewidth=2, zorder=5)
    
    label = f'{icons[dinh]} {dinh}'
    if dinh in khoang_cach and khoang_cach[dinh] != float('inf'):
        label += f'\n({khoang_cach[dinh]}km)'
    
    ax.annotate(label, xy=(x, y-0.6), fontsize=10,
               ha='center', va='top', fontweight='bold')

# Tiêu đề
duong_str = ' → '.join(f'{icons[d]}' for d in duong_di)
ax.set_title(f"🗺️ GPS Navigator — Đường ngắn nhất: {duong_str}\n"
             f"Tổng: {khoang_cach['Thư viện']}km | Thuật toán: Dijkstra", 
             fontsize=14, fontweight='bold')
ax.set_xlim(-2.5, 6.5); ax.set_ylim(-4.5, 4.5)
ax.axis('off')

plt.tight_layout()
plt.savefig('chuong4_dijkstra.png', dpi=150, bbox_inches='tight')
plt.show()

# In bảng khoảng cách
print(f"\n📊 Bảng khoảng cách từ {icons['Nhà']} Nhà:")
for dinh in sorted(khoang_cach.keys()):
    kc = khoang_cach[dinh]
    bar = "█" * int(kc if kc != float('inf') else 0)
    print(f"   {icons[dinh]} {dinh:>15}: {kc:>5} km  {bar}")
```

---

> ✅ **CHECKPOINT 2 — BFS/DFS và Dijkstra**
>
> 1. Trò chơi dò mìn (Minesweeper) mở ô kề nhau khi ô hiện tại bằng 0 là ứng dụng của BFS hay DFS?
> 2. Dijkstra có áp dụng được với đồ thị có cạnh mang trọng số bị âm (\*ví dụ như được tặng quỹ thời gian/chi phí) không?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. **BFS (Breadth-First Search)**. Sóng lan tròn từ ô hiện tại ra xung quanh đến khi gặp ô có giá trị lớn hơn 0 thì dừng lại.
> 2. **Không hoạt động!** Dijkstra luôn giả định rằng "đi tiếp sẽ chỉ luôn làm đường dài ra chứ không làm đường ngắn lại". Với trọng số âm, giả định này phá sản. Khi đó bạn cần thuật toán khác như Bellman-Ford.
>
> </details>
>
> **Tự đánh giá:** Đúng 2/2 → 🟢 Tuyệt! | 1/2 → 🟡 Ổn | 0/2 → 🔴 Đọc lại Phần 2 và 3.

---

## PHẦN 4: LOGIC MỆNH ĐỀ — NỀN TẢNG CỦA MỌI CHƯƠNG TRÌNH

### 🟢 4.1 Logic trong cuộc sống

Mỗi khi bạn viết `if...else` trong code, bạn đang dùng **logic mệnh đề**.

Mệnh đề = Câu khẳng định có giá trị **Đúng (True)** hoặc **Sai (False)**.

Ví dụ:
- "Trời đang mưa" → True hoặc False
- "2 + 2 = 5" → False
- "Python là ngôn ngữ lập trình" → True

### 🟡 4.2 Các phép toán logic

| Phép toán | Ký hiệu | Python | Ý nghĩa | Ví dụ |
|-----------|---------|--------|---------|-------|
| **AND** (Và) | $\wedge$ | `and` | Cả hai đúng | "Trời đẹp **VÀ** đường vắng" → Đi picnic |
| **OR** (Hoặc) | $\vee$ | `or` | Ít nhất 1 đúng | "Có ô **HOẶC** trời tạnh" → Đi ra ngoài |
| **NOT** (Phủ định) | $\neg$ | `not` | Đảo ngược | "**KHÔNG** mưa" |
| **Kéo theo** | $\Rightarrow$ | `if` | Nếu...thì | "**NẾU** mưa **THÌ** mang ô" |

### 🔵 4.3 Bảng chân trị

```python
# =============================================
# BẢNG CHÂN TRỊ VÀ MẠCH LOGIC
# =============================================

import itertools

def bang_chan_tri():
    """In bảng chân trị cho các phép logic cơ bản"""
    print("📋 BẢNG CHÂN TRỊ")
    print("=" * 65)
    print(f"{'A':>5} {'B':>5} │ {'A∧B':>5} {'A∨B':>5} {'¬A':>5} {'A⇒B':>5} {'A⇔B':>5}")
    print("-" * 65)
    
    for a, b in itertools.product([True, False], repeat=2):
        a_and_b = a and b
        a_or_b = a or b
        not_a = not a
        a_implies_b = (not a) or b  # A ⇒ B ≡ ¬A ∨ B
        a_iff_b = a == b            # A ⇔ B
        
        def icon(val): return "✅ T" if val else "❌ F"
        
        print(f"{icon(a):>5} {icon(b):>5} │ {icon(a_and_b):>5} {icon(a_or_b):>5} "
              f"{icon(not_a):>5} {icon(a_implies_b):>5} {icon(a_iff_b):>5}")

bang_chan_tri()

# =============================================
# ỨNG DỤNG: HỆ THỐNG QUYẾT ĐỊNH ĐƠN GIẢN
# =============================================

print("\n\n🤖 HỆ THỐNG QUYẾT ĐỊNH THÔNG MINH")
print("=" * 50)

# Dữ liệu đầu vào
thoi_tiet = "mưa"         # mưa, nắng, âm u
nhiet_do = 28              # độ C
co_oto = False
bai_tap = True

# Logic quyết định
mang_o = (thoi_tiet == "mưa") or (thoi_tiet == "âm u")
mac_ao_lanh = nhiet_do < 20
phuong_tien = "🚗 Ô tô" if co_oto else ("🏠 Ở nhà" if thoi_tiet == "mưa" and nhiet_do < 15 else "🚌 Xe buýt")
can_di_hoc = bai_tap and (thoi_tiet != "bão")

print(f"   Thời tiết: {thoi_tiet}, Nhiệt độ: {nhiet_do}°C")
print(f"   Có ô tô: {co_oto}, Có bài tập: {bai_tap}")
print(f"\n   Quyết định:")
print(f"   {'🌂 Mang ô' if mang_o else '☀️ Không cần ô'}")
print(f"   {'🧥 Mặc áo lạnh' if mac_ao_lanh else '👕 Áo bình thường'}")
print(f"   Phương tiện: {phuong_tien}")
print(f"   {'📚 Đi học' if can_di_hoc else '😴 Nghỉ ở nhà'}")

print(f"\n   💡 Logic phía sau:")
print(f"   mang_ô = (mưa) OR (âm_u) = {thoi_tiet == 'mưa'} OR {thoi_tiet == 'âm u'} = {mang_o}")
print(f"   đi_học = bài_tập AND NOT(bão) = {bai_tap} AND {thoi_tiet != 'bão'} = {can_di_hoc}")
```

---

## PHẦN 5: TỔ HỢP — ĐẾM MÀ KHÔNG CẦN LIỆT KÊ

### 🟢 5.1 Tại sao cần tổ hợp?

Bạn có 10 bạn bè và muốn chọn 3 người đi du lịch. Có bao nhiêu cách chọn?

Liệt kê??? Không khả thi khi số lớn. **Tổ hợp** cho công thức tính ngay.

### 🟡 5.2 Công thức cốt lõi

| Khái niệm | Công thức | Ví dụ |
|-----------|-----------|-------|
| **Giai thừa** | $n! = n \times (n-1) \times \ldots \times 1$ | $5! = 120$ |
| **Hoán vị** | $P(n) = n!$ | Xếp 5 người: 120 cách |
| **Chỉnh hợp** | $A_n^k = \frac{n!}{(n-k)!}$ | Chọn 3/10 (có thứ tự): 720 |
| **Tổ hợp** | $C_n^k = \frac{n!}{k!(n-k)!}$ | Chọn 3/10 (không thứ tự): 120 |

#### 🎯 Liên tưởng: Pizza vs Xếp hàng

- **Chỉnh hợp** = Chọn 3 người xếp theo thứ tự (ai đứng đầu, ai đứng giữa, ai đứng cuối) → Thứ tự **quan trọng**
- **Tổ hợp** = Chọn 3 topping pizza (phô mai, thịt, nấm → hay nấm, phô mai, thịt đều giống nhau) → Thứ tự **không quan trọng**

> ⚠️ **SAI LẦM PHỔ BIẾN:** Mọi người hay nhầm tổ hợp và chỉnh hợp. 
> "Cặp mật khẩu điện thoại 1-2-3-4". Có cần đúng thứ tự không? CÓ. Nhập 4-3-2-1 thì không mở được. Do vậy Password là **chỉnh hợp**, không phải tổ hợp! (Dù tiếng Anh hay xưng là Combination).

### 🔵 5.3 Code Python: Tổ hợp trong thực tế

```python
import numpy as np
import matplotlib.pyplot as plt
from math import factorial, comb, perm
from itertools import combinations

# =============================================
# TỔ HỢP TRONG BẢO MẬT
# =============================================

print("🔐 ỨNG DỤNG TỔ HỢP TRONG BẢO MẬT")
print("=" * 50)

# Mật khẩu 4 số (0-9)
so_mat_khau_4_so = 10**4
print(f"\n📱 Mật khẩu 4 số (như PIN điện thoại):")
print(f"   Số cách: 10⁴ = {so_mat_khau_4_so:,}")
print(f"   Thử 1 mã/giây → Phá trong: {so_mat_khau_4_so} giây = {so_mat_khau_4_so/60:.0f} phút")

# Mật khẩu 8 ký tự (a-z, A-Z, 0-9)
so_ky_tu = 26 + 26 + 10  # 62
so_mat_khau_8 = so_ky_tu**8
print(f"\n🔑 Mật khẩu 8 ký tự (a-z, A-Z, 0-9):")
print(f"   Số cách: 62⁸ = {so_mat_khau_8:,.0f}")
print(f"   Thử 1 tỷ mã/giây → Phá trong: {so_mat_khau_8/1e9:.0f} giây = {so_mat_khau_8/1e9/3600:.1f} giờ")

# Mật khẩu 12 ký tự (+ ký tự đặc biệt)
so_ky_tu_full = 62 + 20  # thêm !@#$... 
so_mat_khau_12 = so_ky_tu_full**12
print(f"\n🛡️ Mật khẩu 12 ký tự (+ đặc biệt):")
print(f"   Số cách: 82¹² = {so_mat_khau_12:.2e}")
print(f"   Thử 1 tỷ mã/giây → Phá trong: {so_mat_khau_12/1e9/3600/24/365:.0f} năm!")

# Tổ hợp: Chọn feature trong ML
print(f"\n\n🤖 TỔ HỢP TRONG MACHINE LEARNING")
print("=" * 50)
n_features = 20
print(f"   Có {n_features} đặc trưng (features)")
print(f"   Chọn k features tốt nhất:")
for k in [1, 3, 5, 10, 15]:
    so_cach = comb(n_features, k)
    print(f"   C({n_features},{k:>2}) = {so_cach:>10,} cách → {'✅ Khả thi' if so_cach < 1e6 else '❌ Quá nhiều!'}")

# Trực quan hóa
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Biểu đồ 1: Tam giác Pascal
ax1 = axes[0]
n_rows = 10
for n in range(n_rows):
    for k in range(n + 1):
        val = comb(n, k)
        ax1.text(k - n/2, -n, str(val), ha='center', va='center',
                fontsize=8, fontweight='bold',
                bbox=dict(boxstyle='round,pad=0.3', facecolor=plt.cm.YlOrRd(val/max(comb(n_rows-1, (n_rows-1)//2), 1)), alpha=0.7))

ax1.set_title("🔺 Tam giác Pascal\n(Mỗi số = C(n,k) = Tổ hợp)", fontsize=12, fontweight='bold')
ax1.set_xlim(-6, 6)
ax1.set_ylim(-n_rows, 1)
ax1.axis('off')

# Biểu đồ 2: Độ khó phá mật khẩu
ax2 = axes[1]
do_dai = [4, 6, 8, 10, 12, 16]
log_cach = [np.log10(82**d) for d in do_dai]

bars = ax2.bar(do_dai, log_cach, color=plt.cm.RdYlGn_r(np.linspace(0.2, 0.8, len(do_dai))),
               edgecolor='white', width=1.2)

for bar, d, l in zip(bars, do_dai, log_cach):
    if 82**d / 1e9 / 3600 / 24 / 365 > 1:
        label = f"{82**d / 1e9 / 3600 / 24 / 365:.0f} năm"
    elif 82**d / 1e9 / 3600 > 1:
        label = f"{82**d / 1e9 / 3600:.0f} giờ"
    else:
        label = f"{82**d / 1e9:.0f} giây"
    ax2.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.3,
             label, ha='center', fontsize=9, fontweight='bold')

ax2.set_xlabel("Độ dài mật khẩu", fontsize=11)
ax2.set_ylabel("log₁₀(Số cách)", fontsize=11)
ax2.set_title("🔐 Độ khó phá mật khẩu\n(82 ký tự, brute force 1 tỷ/giây)", 
              fontsize=12, fontweight='bold')
ax2.grid(axis='y', alpha=0.3)

plt.tight_layout()
plt.savefig('chuong4_to_hop.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

> ✅ **CHECKPOINT 3 — Logic và Tổ hợp**
>
> 1. Trạng thái $A \Rightarrow B$ (Nếu A thì B) sẽ bị Sai (False) trong trường hợp duy nhất nào?
> 2. Chọn 3 lớp học từ danh sách 10 lớp học để đăng ký tín chỉ là Tổ Hợp hay Chỉnh Hợp? 
> 3. Chọn 3 số để làm mật khẩu tủ khóa là Tổ Hợp hay Chỉnh Hợp?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Khi **A Đúng (True) nhưng B Sai (False)**. Ví dụ: "Nếu trời mưa (True) thì đường ướt (False)". Điều này vô lý nên mệnh đề Sai. Các trường hợp khác đều Đúng.
> 2. **Tổ Hợp**, vì thứ tự bạn đăng ký các lớp học không quan trọng.
> 3. **Chỉnh Hợp**. Phải nhập đúng thứ tự mới mở được khóa.
>
> </details>

---

## PHẦN 5B: ĐỘ PHỨC TẠP THUẬT TOÁN (BIG-O) — "ĐO TỐC ĐỘ"

### 🟢 5B.1 Tại sao cần Big-O?

Không phải thuật toán nào cũng chạy nhanh khi dữ liệu lớn. Big-O cho biết thuật toán **chậm đi** bao nhiêu khi dữ liệu **tăng lên**:

$$T(n) = O(f(n))$$

#### 🎯 Ví dụ đời sống: Tìm sản phẩm trên Shopee 🛒

| Cách tìm | Big-O | 1 triệu SP | 1 tỷ SP |
|-----------|-------|------------|---------|
| **Duyệt từng cái** | $O(n)$ | 1 giây | 1,000 giây (17 phút!) |
| **Binary Search** (đã sắp xếp) | $O(\log n)$ | 0.00002 giây | 0.00003 giây |
| **Hash Table** (dict) | $O(1)$ | 0.000001 giây | 0.000001 giây |

> 💡 Từ $O(n)$ sang $O(\log n)$: 1 triệu lần nhanh hơn! Đó là sức mạnh của thuật toán.

### 🟡 5B.2 Các mức độ phức tạp

```
NHANH ←───────────────────────────────────────→ CHẬM

O(1)    O(log n)   O(n)     O(n log n)   O(n²)     O(2ⁿ)
│         │          │          │           │          │
Dict    Binary    Duyệt    Merge Sort   Bubble    Brute
Lookup  Search    tuần tự   Quick Sort   Sort      Force
```

### 🔵 5B.3 Code Python: So sánh tốc độ

```python
import numpy as np
import matplotlib.pyplot as plt
import time

# =============================================
# SO SÁNH TỐC ĐỘ THUẬT TOÁN
# =============================================

ns = [100, 500, 1000, 5000, 10000, 50000]

def linear_search(arr, target):
    """O(n) — Duyệt tuần tự"""
    for i, x in enumerate(arr):
        if x == target:
            return i
    return -1

def binary_search(arr, target):
    """O(log n) — Tìm nhị phân"""
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target: return mid
        elif arr[mid] < target: lo = mid + 1
        else: hi = mid - 1
    return -1

times_linear, times_binary = [], []

for n in ns:
    arr = sorted(np.random.randint(0, n*10, n))
    target = arr[-1]  # Worst case cho linear
    
    # O(n)
    t0 = time.perf_counter()
    for _ in range(100): linear_search(arr, target)
    times_linear.append((time.perf_counter() - t0) / 100)
    
    # O(log n)
    t0 = time.perf_counter()
    for _ in range(100): binary_search(arr, target)
    times_binary.append((time.perf_counter() - t0) / 100)

# Trực quan
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(ns, [t*1000 for t in times_linear], 'r-o', linewidth=2, markersize=8, label='O(n) Linear Search')
ax.plot(ns, [t*1000 for t in times_binary], 'g-o', linewidth=2, markersize=8, label='O(log n) Binary Search')
ax.set_xlabel('Kích thước dữ liệu (n)', fontsize=12)
ax.set_ylabel('Thời gian (ms)', fontsize=12)
ax.set_title('⏱️ Big-O: Tốc độ thuật toán khi dữ liệu tăng\n(Linear chậm tuyến tính, Binary gần như không đổi!)', 
             fontsize=13, fontweight='bold')
ax.legend(fontsize=11); ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('chuong4_big_o.png', dpi=150, bbox_inches='tight')
plt.show()

print("\n💡 BÀI HỌC:")
print(f"   n=50,000: Linear = {times_linear[-1]*1000:.2f}ms, Binary = {times_binary[-1]*1000:.4f}ms")
print(f"   Binary nhanh hơn {times_linear[-1]/times_binary[-1]:.0f}x!")
print(f"   Trong ML: Chọn thuật toán đúng quan trọng hơn máy nhanh!")
```

> 📌 **Lưu ý cho giảng viên:** Big-O giải thích tại sao một số thuật toán ML scale tốt (SGD: O(n)), còn một số thì không (KNN: O(n) mỗi query, SVM: O(n²-n³)). Đây là nền tảng để sinh viên đánh giá tính khả thi của thuật toán trên dữ liệu thực.

---

> ✅ **CHECKPOINT 4 — Big O Notation**
>
> 1. Có danh sách tên học sinh đã được sắp xếp bảng chữ cái. Bạn muốn tìm tên "Hoa". Dùng cách lật từng trang từ đầu (Linear Search) hay chia đôi tìm (Binary Search) thì Big-O là gì?
> 2. Khi dữ liệu tăng từ 1 triệu lên 2 triệu (gấp đôi), thuật toán có độ phức tạp $O(n^2)$ sẽ chạy chậm đi bao nhiêu lần?
>
> <details><summary>📝 Đáp án</summary>
>
> 1. Lật từng trang: $O(n)$. Chia đôi: $O(\log n)$ (rất nhanh).
> 2. Chậm đi $(2n)^2 / n^2 = \textbf{4 lần!}$. Thuật toán $O(n^2)$ mở rộng (scale) rất kém. 
>
> </details>

---

## PHẦN 6: TÓM TẮT CHƯƠNG & BẢN ĐỒ TƯ DUY

### 🗺️ Bản đồ kiến thức Chương 4

```
TOÁN RỜI RẠC — CẤU TRÚC CỦA THẾ GIỚI SỐ
├── ĐỒ THỊ (Graph)
│   ├── Đỉnh (Node) + Cạnh (Edge)
│   ├── Biểu diễn: Ma trận kề, Danh sách kề
│   ├── BFS (Sóng lan) → Đường ngắn nhất (không trọng số)
│   ├── DFS (Mê cung) → Liên thông, Chu trình
│   └── Dijkstra → Đường ngắn nhất (có trọng số) 🗺️
│       └── Mini Project: GPS Navigator
│
├── LOGIC MỆNH ĐỀ
│   ├── AND, OR, NOT, ⇒
│   ├── Bảng chân trị
│   └── Ứng dụng: if/else trong mọi chương trình
│
└── TỔ HỢP
    ├── Giai thừa, Hoán vị, Chỉnh hợp, Tổ hợp
    ├── Ứng dụng: Bảo mật mật khẩu 🔐
    └── Feature Selection trong ML
```

### 💎 Nếu chỉ nhớ 1 điều từ chương này

> **Thế giới số = Chấm và Đường nối.**
>
> Mọi thứ phức tạp trong CNTT — mạng xã hội, Internet, GPS, blockchain, thậm chí mạng neural — đều có thể biểu diễn bằng đồ thị G = (V, E). BFS tìm đường ngắn nhất theo số bước. Dijkstra tìm đường ngắn nhất theo trọng số. Big-O cho biết thuật toán nhanh hay chậm. Hiểu được đồ thị, bạn hiểu cấu trúc bên trong của mọi hệ thống số.

---

### 🔗 Kiến thức này xuất hiện lại ở đâu?

| Chương | Nội dung liên quan | Cần kiến thức gì từ Ch.4? |
|--------|-------------------|--------------------------|
| **Ch.5** Neural Network Architecture | DAG = Directed Acyclic Graph | Đồ thị có hướng |
| **Ch.5** PageRank | Duyệt đồ thị web | BFS/DFS, Ma trận kề |
| **Ch.5** Federated Learning | Topology mạng | Đồ thị, Ma trận kề |
| **Ch.6** Computation Graph | Backprop trên đồ thị | DAG, Đồ thị có hướng |
| **Ch.7** Markov Chain | Graph xác suất chuyển trạng thái | Đồ thị có trọng số |

---

### ✅ Checklist kiến thức

- [ ] Biểu diễn bài toán thực tế bằng đồ thị
- [ ] Phân biệt BFS và DFS, biết khi nào dùng cái nào
- [ ] Viết Dijkstra tìm đường ngắn nhất
- [ ] Viết bảng chân trị và áp dụng logic vào if/else
- [ ] Tính tổ hợp, chỉnh hợp và ứng dụng trong bảo mật

---

## ❓ FAQ — Câu hỏi thường gặp

**Q: Khi nào dùng BFS, khi nào dùng DFS?**
A: BFS = tìm đường ngắn nhất (theo số cạnh), duyệt theo tầng. DFS = khám phá sâu, kiểm tra chu trình, giải mê cung. Nếu cần "ngắn nhất" → BFS. Nếu cần "đi hết" → DFS.

**Q: Dijkstra có hoạt động với cạnh âm không?**
A: KHÔNG! Dijkstra giả định mọi trọng số ≥ 0. Nếu có cạnh âm, dùng Bellman-Ford thay thế.

**Q: Big-O quan trọng đến mức nào trong phỏng vấn?**
A: Rất quan trọng. Big-O cho biết code của bạn có "chạy nổi" khi dữ liệu lớn hay không. O(n²) với n=10⁶ = 10¹² phép tính → quá chậm. O(n log n) với n=10⁶ ≈ 2×10⁷ → chấp nhận được.

**Q: Ma trận kề và danh sách kề — dùng cái nào?**
A: Đồ thị đặc (nhiều cạnh) → Ma trận kề (truy cập O(1)). Đồ thị thưa (ít cạnh) → Danh sách kề (tiết kiệm bộ nhớ). Trong thực tế, hầu hết đồ thị thực tế đều thưa.

---

## 📝 BÀI TẬP TỰ LUYỆN

### Bài 1: Mạng xã hội mini ⭐
Tạo đồ thị biểu diễn mối quan hệ 10 người trong lớp. Dùng BFS tìm "bạn chung" giữa 2 người bất kỳ.

### Bài 2: Bài toán người giao hàng ⭐⭐
5 điểm giao hàng, tìm đường đi qua tất cả các điểm với quãng đường ngắn nhất (brute force). Có bao nhiêu cách (dùng hoán vị)?

### Bài 3: A* Pathfinding ⭐⭐⭐
Nâng cấp Dijkstra thành A* bằng cách thêm heuristic (khoảng cách đường chim bay đến đích). So sánh tốc độ với Dijkstra.

### Bài 4: Phát hiện cộng đồng ⭐⭐⭐
Cho đồ thị mạng xã hội 30 nodes, dùng thuật toán phát hiện các "nhóm bạn" (connected components). Trực quan hóa với màu khác nhau cho mỗi nhóm.

---

## 📌 LƯU Ý CHO GIẢNG VIÊN

### Chiến lược khơi gợi sự tò mò

1. **Bài toán thực tế trước, lý thuyết sau:**
   - Mở Google Maps, tìm đường → "Nó làm thế nào?" → Dijkstra
   - Mở Facebook → "Gợi ý bạn bè hoạt động ra sao?" → BFS

2. **Hoạt động thể chất:**
   - Cho sinh viên đứng thành đồ thị (mỗi người = 1 node, giơ dây = edge)
   - Chạy BFS "thủ công": truyền bóng từ node gốc → rất trực quan

3. **Liên hệ lập trình:**
   - Cấu trúc dữ liệu Tree, Graph → Đồ thị
   - Router tìm đường mạng → Dijkstra biến thể
   - Git branch → Đồ thị có hướng không chu trình (DAG)

4. **Sai lầm phổ biến:**
   - Nhầm đồ thị có hướng/vô hướng
   - Dijkstra không hoạt động với trọng số âm → cần Bellman-Ford
   - Nhầm tổ hợp và chỉnh hợp (có/không thứ tự)

---

## 📝 BÀI TẬP PHÂN TẦNG

### 🟢 Mức A — Nhận biết

**A1.** Đồ thị vô hướng (undirected) khác đồ thị có hướng (directed) ở điểm nào? Đưa ra 1 ví dụ cho mỗi loại.

**A2.** BFS và DFS: Loại nào dùng Queue (FIFO)? Loại nào dùng Stack (LIFO)?

**A3.** Dijkstra có hoạt động với trọng số âm không? Tại sao?

**A4.** $C(10, 3)$ = ? (Tổ hợp chọn 3 từ 10, không kể thứ tự). Công thức?

**A5.** Trong logic mệnh đề, $P \Rightarrow Q$ (P kéo theo Q) tương đương với biểu thức nào? (a) $P \wedge Q$; (b) $\neg P \vee Q$; (c) $P \vee \neg Q$; (d) $\neg Q \Rightarrow \neg P$.

<details><summary>📝 Đáp án A</summary>

- A1: Vô hướng: cạnh A-B = đi được 2 chiều (Facebook). Có hướng: cạnh A→B = chỉ 1 chiều (Twitter follow).
- A2: BFS → **Queue** (FIFO: vào trước ra trước). DFS → **Stack** (LIFO: vào sau ra trước).
- A3: **Không**. Dijkstra giả định đi thêm chỉ tăng chi phí. Trọng số âm phá vỡ giả định này → Dùng Bellman-Ford.
- A4: $C(10,3) = \frac{10!}{3! \cdot 7!} = \frac{10 \times 9 \times 8}{3 \times 2 \times 1} = \mathbf{120}$
- A5: **(b) và (d)** đều đúng. $P \Rightarrow Q \equiv \neg P \vee Q \equiv \neg Q \Rightarrow \neg P$ (contrapositive).

</details>

---

### 🟡 Mức B — Tính toán cơ bản

**B1.** Cho đồ thị: A→B(3), A→C(5), B→D(2), C→D(1), B→C(1). Chạy Dijkstra từ A để tìm đường ngắn nhất đến tất cả đỉnh. Điền vào bảng: Đỉnh → Khoảng cách ngắn nhất.

**B2.** Làm BFS tay trên đồ thị: 1-{2,3}, 2-{4,5}, 3-{6}. Xuất phát từ 1. Thứ tự duyệt? Gợi ý: Dùng hàng đợi và đánh dấu đã thăm.

**B3.** Tính:
- (a) Số cách chọn 4 người từ 10 người (không kể thứ tự)
- (b) Số mật khẩu 6 chữ số (mỗi chữ số 0-9, có thể trùng)
- (c) Số anagram (hoán vị) của từ "TOÁN"

**B4.** Xây dựng bảng chân trị cho biểu thức: $\neg(P \wedge Q) \equiv \neg P \vee \neg Q$ (Định luật De Morgan). Dùng tất cả 4 trường hợp T/F.

<details><summary>📝 Đáp án B</summary>

**B1:** Dijkstra từ A:
- A: 0 (xuất phát)
- B: 3 (A→B)
- C: 4 (A→B→C, ngắn hơn A→C=5)
- D: 5 (A→B→C→D = 3+1+1)

**B2:** BFS từ 1: Hàng đợi [1] → thăm 1, thêm 2,3 → thăm 2, thêm 4,5 → thăm 3, thêm 6 → thăm 4,5,6. **Thứ tự: 1, 2, 3, 4, 5, 6**

**B3:**
- (a) $C(10,4) = \frac{10!}{4! \cdot 6!} = \mathbf{210}$
- (b) $10^6 = \mathbf{1,000,000}$ (mỗi vị trí 10 lựa chọn, có thể trùng)
- (c) 4 chữ khác nhau → $4! = \mathbf{24}$ hoán vị

**B4:** Cả hai cột đều cho cùng kết quả → ¬(P∧Q) ≡ ¬P∨¬Q ✅ (Định luật De Morgan)

| P | Q | P∧Q | ¬(P∧Q) | ¬P | ¬Q | ¬P∨¬Q |
|---|---|-----|--------|----|----|-------|
| T | T | T   | **F**  | F  | F  | **F** |
| T | F | F   | **T**  | F  | T  | **T** |
| F | T | F   | **T**  | T  | F  | **T** |
| F | F | F   | **T**  | T  | T  | **T** |

</details>

---

### 🔵 Mức C — Giải thích và so sánh

**C1.** Tại sao BFS tìm đường ngắn nhất theo số cạnh, nhưng Dijkstra mới đúng cho đồ thị có trọng số? Cho ví dụ phản chứng: đồ thị mà BFS cho đường sai nhưng Dijkstra đúng.

**C2.** Ma trận kề vs Danh sách kề: (a) Độ phức tạp thời gian và không gian của mỗi cách; (b) Khi nào ma trận kề tốt hơn? (c) Tại sao đồ thị mạng xã hội (Facebook) thường dùng danh sách kề?

**C3.** Giải thích tại sao mạng neural (neural network) là một đồ thị có hướng không chu trình (DAG). Tại sao "không chu trình" lại quan trọng cho backpropagation?

---

### 🔴 Mức D — Ứng dụng thực tế (Code)

**D1. GPS Navigator nâng cao:**
Thêm tính năng: Optimize cho nhiều mục tiêu (thời gian ngắn nhất VÀ ít đèn đỏ nhất). Implement A* algorithm (GD* heuristic). So sánh A* vs Dijkstra về tốc độ.

**D2. Phân tích mạng xã hội:**
Tạo đồ thị Facebook giả 50 người dùng (random edges, p=0.1). Tính: (a) Degree distribution; (b) Clustering coefficient; (c) Shortest path trung bình (six degrees of separation). Vẽ các biểu đồ.

**D3. Phát hiện cộng đồng (Community Detection):**
Implement thuật toán Girvan-Newman đơn giản: Tính betweenness centrality của mỗi cạnh → Xóa cạnh có betweenness cao nhất → Lặp lại → Tìm các cluster.

**D4. Logic Circuit Simulator:**
Xây dựng simulator mạch logic: Các cổng AND, OR, NOT, XOR. Input = boolean vector. Tính output. Áp dụng: Thiết kế mạch cộng 4-bit (half adder + full adder).

---

## 🆘 HỖ TRỢ NGƯỜI TỰ HỌC

### 📚 Tài nguyên học tiếp

| Nguồn | Nội dung | Khi nào dùng |
|-------|---------|-------------|
| **CS50 — Algorithms** (Harvard) | Graph algorithms trực quan | Sau Phần 2-3 |
| **LeetCode — Graph Problems** | Bài tập luyện BFS/DFS/Dijkstra | Chuẩn bị phỏng vấn |
| **NetworkX Docs** (Python) | Thư viện đồ thị đầy đủ | Khi làm project |
| **Rosen — Discrete Math** | Sách giáo khoa chuẩn | Muốn chứng minh nghiêm ngặt |

### 🗺️ Lộ trình ôn nếu bị hổng

```
Quên BFS/DFS?       → §2.1-2.2 + VÍ DỤ "đá ném xuống ao / mê cung" → Bài B2
Quên Dijkstra?      → §3.2 "Ngọn lửa lan trên bản đồ" → Bài B1
Quên tổ hợp?        → §5.2 Bảng công thức → Bài B3
Không nhớ logic?    → §4.2 Bảng phép toán → Bài B4
```

### ✅ Checklist tự đánh giá

- [ ] Xây dựng đồ thị từ bài toán thực tế (ví dụ: bản đồ trường học)
- [ ] Chạy tay BFS hoặc DFS trên đồ thị 5-7 đỉnh
- [ ] Tính đường ngắn nhất bằng Dijkstra tay
- [ ] Code BFS/DFS/Dijkstra từ đầu không xem tài liệu
- [ ] Tính tổ hợp và chỉnh hợp cho bài toán mật khẩu
- [ ] Phân biệt: BFS vs DFS, Tổ hợp vs Chỉnh hợp

> **5–6 ✅ → Sẵn sàng sang Chương 5.**
> **3–4 ✅ → Xem lại 1–2 mục yếu.**
> **< 3 ✅ → Làm lại Checkpoint trước khi tiếp tục.**

---

> 📖 **Chương tiếp theo:** [Chương 5: Tư duy Hệ thống & Công nghệ mới](Chuong5_TuDuyHeThong.md)
>
> *"Khi Đại số, Giải tích, Xác suất và Toán rời rạc gặp nhau — AI ra đời."*

