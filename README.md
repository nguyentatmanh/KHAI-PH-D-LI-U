# Giải đề Khai phá dữ liệu

## Bài 1. Apriori với `min_sup = 60%`, `min_conf = 80%`

### Cơ sở dữ liệu giao dịch

- T100 = {M, O, N, K, E, Y}
- T200 = {D, O, N, K, E, Y}
- T300 = {M, A, K, E}
- T400 = {M, U, C, K, Y}
- T500 = {C, O, K, I, E}

Tổng số giao dịch: `5`

Ta có:

- `min_sup_count = 5 × 60% = 3`

Nghĩa là itemset nào có `support count ≥ 3` thì là tập mục thường xuyên.

---

### a) Tìm tất cả tập mục thường xuyên bằng Apriori

#### Bước 1: C1 và L1

Đếm số lần xuất hiện từng mục:

- {A}: 1
- {C}: 2
- {D}: 1
- {E}: 4
- {I}: 1
- {K}: 5
- {M}: 3
- {N}: 2
- {O}: 3
- {U}: 1
- {Y}: 3

Suy ra:

- `L1 = {{E}, {K}, {M}, {O}, {Y}}`

với support count tương ứng:

- {E}: 4
- {K}: 5
- {M}: 3
- {O}: 3
- {Y}: 3

---

#### Bước 2: Sinh C2 từ L1

Các 2-itemset ứng viên và support count:

- {E, K}: 4
- {E, M}: 2
- {E, O}: 3
- {E, Y}: 2
- {K, M}: 3
- {K, O}: 3
- {K, Y}: 3
- {M, O}: 1
- {M, Y}: 2
- {O, Y}: 2

Giữ lại các tập có `count ≥ 3`:

- `L2 = {{E, K}, {E, O}, {K, M}, {K, O}, {K, Y}}`

---

#### Bước 3: Sinh C3 từ L2

Từ phép nối:

- {E, K} ⋈ {E, O} ⇒ {E, K, O}
- {K, M} ⋈ {K, O} ⇒ {K, M, O}
- {K, O} ⋈ {K, Y} ⇒ {K, O, Y}

Dùng bước prune:

- {K, M, O} bị loại vì {M, O} không thuộc L2
- {K, O, Y} bị loại vì {O, Y} không thuộc L2

Chỉ còn:

- `C3 = {{E, K, O}}`

Đếm support:

- {E, K, O}: 3

Vì `3 ≥ 3`, suy ra:

- `L3 = {{E, K, O}}`

---

#### Bước 4: Sinh C4

Không thể nối tiếp được nữa:

- `C4 = ∅`
- `L4 = ∅`

---

### Kết luận câu a

Tất cả các tập mục thường xuyên là:

#### 1-itemset thường xuyên
- `L1 = {{E}, {K}, {M}, {O}, {Y}}`

#### 2-itemset thường xuyên
- `L2 = {{E, K}, {E, O}, {K, M}, {K, O}, {K, Y}}`

#### 3-itemset thường xuyên
- `L3 = {{E, K, O}}`

Không có tập mục thường xuyên có số phần tử lớn hơn 3.

---

### b) Tìm tất cả các luật mạnh

Điều kiện:

- `min_conf = 80% = 0.8`

Ta xét lần lượt các frequent itemset có từ 2 phần tử trở lên.

---

#### Từ {E, K} (support = 4/5 = 0.8)

- `conf(E => K) = sup(E, K) / sup(E) = 4 / 4 = 1`
- `conf(K => E) = 4 / 5 = 0.8`

Cả hai đều là luật mạnh.

---

#### Từ {E, O} (support = 3/5 = 0.6)

- `conf(E => O) = 3 / 4 = 0.75 < 0.8` → không mạnh
- `conf(O => E) = 3 / 3 = 1` → luật mạnh

---

#### Từ {K, M} (support = 3/5 = 0.6)

- `conf(K => M) = 3 / 5 = 0.6 < 0.8` → không mạnh
- `conf(M => K) = 3 / 3 = 1` → luật mạnh

---

#### Từ {K, O} (support = 3/5 = 0.6)

- `conf(K => O) = 3 / 5 = 0.6 < 0.8` → không mạnh
- `conf(O => K) = 3 / 3 = 1` → luật mạnh

---

#### Từ {K, Y} (support = 3/5 = 0.6)

- `conf(K => Y) = 3 / 5 = 0.6 < 0.8` → không mạnh
- `conf(Y => K) = 3 / 3 = 1` → luật mạnh

---

#### Từ {E, K, O} (support = 3/5 = 0.6)

Xét các luật:

- `conf(EO => K) = 3 / 3 = 1` → mạnh
- `conf(EK => O) = 3 / 4 = 0.75 < 0.8` → không mạnh
- `conf(KO => E) = 3 / 3 = 1` → mạnh
- `conf(E => KO) = 3 / 4 = 0.75 < 0.8` → không mạnh
- `conf(O => EK) = 3 / 3 = 1` → mạnh
- `conf(K => EO) = 3 / 5 = 0.6 < 0.8` → không mạnh

---

### Kết luận câu b: Các luật mạnh

- `E => K` (sup = 0.8, conf = 1)
- `K => E` (sup = 0.8, conf = 0.8)
- `O => E` (sup = 0.6, conf = 1)
- `M => K` (sup = 0.6, conf = 1)
- `O => K` (sup = 0.6, conf = 1)
- `Y => K` (sup = 0.6, conf = 1)
- `EO => K` (sup = 0.6, conf = 1)
- `KO => E` (sup = 0.6, conf = 1)
- `O => EK` (sup = 0.6, conf = 1)

---

## Bài 2. Naive Bayes

### Tập huấn luyện

| Ứng viên | Học vấn   | Kinh nghiệm | Nhận việc |
|---------:|-----------|-------------|-----------|
| 1        | Cao đẳng  | Ít          | No        |
| 2        | Cao đẳng  | Nhiều       | No        |
| 3        | Đại học   | Ít          | Yes       |
| 4        | Đại học   | Nhiều       | Yes       |
| 5        | Đại học   | Nhiều       | Yes       |
| 6        | Cao đẳng  | Nhiều       | Yes       |

Cần dự đoán cho ứng viên:

- `X = (Cao đẳng, Nhiều)`

Theo Naive Bayes:

- `ŷ = argmax P(c) × P(Học vấn = Cao đẳng | c) × P(Kinh nghiệm = Nhiều | c)`, với `c ∈ {Yes, No}`

---

### Bước 1: Xác suất tiên nghiệm

Có 6 mẫu, trong đó:

- Yes: 4 mẫu
- No: 2 mẫu

Nên:

- `P(Yes) = 4/6 = 2/3`
- `P(No) = 2/6 = 1/3`

---

### Bước 2: Xác suất điều kiện theo lớp Yes

Các mẫu Yes là: 3, 4, 5, 6.

Trong 4 mẫu Yes:

- Học vấn = Cao đẳng: chỉ có mẫu 6  
  `P(Cao đẳng | Yes) = 1/4`

- Kinh nghiệm = Nhiều: các mẫu 4, 5, 6  
  `P(Nhiều | Yes) = 3/4`

Do đó:

- `P1 = P(Yes) × P(Cao đẳng | Yes) × P(Nhiều | Yes)`
- `P1 = (2/3) × (1/4) × (3/4) = 1/8 = 0.125`

---

### Bước 3: Xác suất điều kiện theo lớp No

Các mẫu No là: 1, 2.

Trong 2 mẫu No:

- Học vấn = Cao đẳng: có cả 2 mẫu  
  `P(Cao đẳng | No) = 2/2 = 1`

- Kinh nghiệm = Nhiều: chỉ có mẫu 2  
  `P(Nhiều | No) = 1/2`

Do đó:

- `P2 = P(No) × P(Cao đẳng | No) × P(Nhiều | No)`
- `P2 = (1/3) × 1 × (1/2) = 1/6 ≈ 0.1667`

---

### So sánh

- `P2 = 1/6 > P1 = 1/8`

Suy ra:

- `ŷ = No`

---

### Kết luận bài 2

Ứng viên có trình độ **Cao đẳng** và kinh nghiệm **Nhiều** được dự đoán là:

- **No**

Tức là **không được nhận việc**.

---

## Bài 3. Phân cụm theo mật độ DBSCAN với `ε = 2`, `minpts = 3`

Ta dùng khoảng cách Euclid, và một điểm là **điểm lõi** nếu số điểm trong `ε`-lân cận của nó thỏa:

- `|Nε(x)| ≥ 3`

Mình đọc các tọa độ nguyên từ hình như sau.

---

### Tập điểm đọc từ hình

#### Nhóm bên trái - giữa
- (2,4)
- (3,4)
- (3,3)
- (5,8)
- (5,6)
- (5,4)
- (6,7)
- (6,5)
- (6,4)
- (7,4)
- (7,3)
- (8,2)
- (9,4)

#### Nhóm bên phải
- (10,8)
- (10,7)
- (10,6)
- (11,8)
- (11,5)
- (12,7)
- (13,7)
- (13,6)
- (14,6)
- (15,5)
- (15,4)

---

### a) Chỉ ra các điểm lõi / biên / nhiễu

Ta xét nhanh các điểm đặc biệt:

- (2,4): lân cận có (2,4), (3,4), (3,3)  
  ⇒ `|N2| = 3` ⇒ điểm lõi

- (5,8): lân cận có (5,8), (5,6), (6,7)  
  ⇒ `|N2| = 3` ⇒ điểm lõi

- (8,2): chỉ gần (7,3) và chính nó  
  ⇒ `|N2| = 2` ⇒ không phải lõi, nhưng nằm trong lân cận của (7,3) là điểm lõi  
  ⇒ điểm biên

- (9,4): chỉ gần (7,4) và chính nó  
  ⇒ `|N2| = 2` ⇒ điểm biên

- (11,5): chỉ gần (10,6) và chính nó  
  ⇒ `|N2| = 2` ⇒ điểm biên

- (15,4): chỉ gần (15,5) và chính nó  
  ⇒ `|N2| = 2` ⇒ điểm biên

Các điểm còn lại đều có ít nhất 3 điểm trong `N2(x)`, nên là điểm lõi.

---

### Kết luận phân loại điểm

#### Điểm biên
- (8,2)
- (9,4)
- (11,5)
- (15,4)

#### Điểm nhiễu
Không có điểm nào vừa không lõi vừa không thuộc lân cận của điểm lõi.

- `Tập điểm nhiễu = ∅`

---

### b) Chỉ ra các cụm nhận được và các điểm nhiễu

Do các điểm bên trái và giữa nối với nhau theo chuỗi mật độ:

- (2,4) ↔ (3,4) ↔ (5,4) ↔ (6,4) ↔ (7,4)

và phần trên:

- (5,8) ↔ (6,7) ↔ (5,6) ↔ (6,5)

nên toàn bộ các điểm bên trái - giữa tạo thành **một cụm**.

Tương tự, các điểm bên phải nối với nhau theo chuỗi:

- (10,8) ↔ (10,7) ↔ (10,6) ↔ (11,8) ↔ (12,7) ↔ (13,7) ↔ (13,6) ↔ (14,6) ↔ (15,5)

nên tạo thành **một cụm thứ hai**.

---

### Kết luận bài 3

#### Cụm C1
- {(2,4), (3,4), (3,3), (5,8), (5,6), (5,4), (6,7), (6,5), (6,4), (7,4), (7,3), (8,2), (9,4)}

Trong đó các điểm biên là:

- (8,2)
- (9,4)

---

#### Cụm C2
- {(10,8), (10,7), (10,6), (11,8), (11,5), (12,7), (13,7), (13,6), (14,6), (15,5), (15,4)}

Trong đó các điểm biên là:

- (11,5)
- (15,4)

---

#### Điểm nhiễu
- `∅`

---

## Đáp án ngắn gọn để chép

### Bài 1

- `L1 = {{E}, {K}, {M}, {O}, {Y}}`
- `L2 = {{E, K}, {E, O}, {K, M}, {K, O}, {K, Y}}`
- `L3 = {{E, K, O}}`
- Không có `L4`

Luật mạnh:

- `E => K`
- `K => E`
- `O => E`
- `M => K`
- `O => K`
- `Y => K`
- `EO => K`
- `KO => E`
- `O => EK`

---

### Bài 2

- `P(Yes) = 4/6`, `P(No) = 2/6`
- `P1 = (2/3) × (1/4) × (3/4) = 1/8`
- `P2 = (1/3) × 1 × (1/2) = 1/6`

Suy ra:

- `ŷ = No`

---

### Bài 3

- Có `2` cụm
- `C1`: các điểm bên trái - giữa
- `C2`: các điểm bên phải
- Điểm biên: `(8,2), (9,4), (11,5), (15,4)`
- Điểm nhiễu: `∅`
