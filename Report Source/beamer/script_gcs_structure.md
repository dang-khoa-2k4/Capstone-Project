# Script thuyết trình: Hoạch định chuyển động sử dụng mô hình SPP trong GCS

---

## Slide 1 — Thiết lập bài toán trên GCS

### Nội dung slide
- **Mục tiêu:** tìm quỹ đạo tối ưu từ σ đến τ trên GCS
- Phân hoạch không gian thành các vùng an toàn lồi {Q_v ⊂ ℝ²}
- Thiết lập đồ thị liền kề GCS G = (V, ε)
- Gán biến trạng thái x_v cho mọi đỉnh v ∈ V
- *(Bên phải: hình gcs-1 và gcs-2)*

---

### Script

Ở phần này, tôi sẽ trình bày cách bài toán motion planning được mô hình hóa thành một bài toán tối ưu trên Graph of Convex Sets — gọi tắt là GCS.

**[Chỉ vào hình A — bên trái của gcs-1]**

Nhìn vào hình A, đây là bài toán ban đầu: robot cần di chuyển từ điểm xuất phát q₀ ở góc dưới trái đến điểm đích q_T ở góc trên phải. Không gian có hai vật cản lồi màu hồng chặn đường đi. Tại bước này, không gian tự do có hình dạng phi lồi — rất khó tối ưu trực tiếp.

**[Chỉ vào hình B — bên phải của gcs-1]**

Hình B cho thấy bước đầu tiên của phương pháp: ta **phân hoạch** không gian tự do thành năm vùng lồi Q₁ đến Q₅ — màu xanh nhạt. Mỗi vùng Q_v là một tập lồi, đóng, không chứa vật cản. Điểm quan trọng: các vùng này phủ kín toàn bộ không gian tự do, và các vùng liền kề nhau có vùng giao khác rỗng — đây là điều kiện để có thể đi xuyên qua từ vùng này sang vùng kia.

**[Chỉ vào hình C — bên trái của gcs-2]**

Từ tập hợp các vùng lồi đó, ta xây dựng **đồ thị GCS** G = (V, ε). Mỗi vùng Q_v trở thành một đỉnh v trong đồ thị — ở đây ta có các đỉnh 1 đến 5. Ngoài ra, ta thêm hai đỉnh đặc biệt: σ là đỉnh nguồn đại diện cho điểm xuất phát, và τ là đỉnh đích đại diện cho điểm kết thúc. Cạnh giữa hai đỉnh u và v tồn tại khi và chỉ khi hai vùng Q_u và Q_v giao nhau — tức là robot có thể đi từ vùng này sang vùng kia. Ta thấy σ kết nối vào đỉnh 1, các đỉnh 1–2–3–4–5 liên kết nhau theo cấu trúc liền kề, và cuối cùng đỉnh 5 kết nối đến τ.

**[Chỉ vào hình D — bên phải của gcs-2]**

Hình D minh họa điều quan trọng hơn: trên mỗi đỉnh v, ta không chỉ lưu một điểm — mà lưu một **đoạn quỹ đạo cục bộ** q_v nằm bên trong vùng Q_v. Đường màu xanh trong mỗi vùng là đoạn quỹ đạo Bézier bậc 2, được xác định bởi ba điểm điều khiển: điểm vào q_v^s, điểm giữa q_v^m, và điểm ra q_v^t — tất cả đều phải nằm trong Q_v. Đây là lý do biến trạng thái x_v có sáu thành phần:

> x_v = (q_v^{s,x}, q_v^{s,y}, q_v^{m,x}, q_v^{m,y}, q_v^{t,x}, q_v^{t,y}) ∈ ℝ⁶

Tập khả thi X_v của đỉnh v chính là ràng buộc ba điểm q_v^s, q_v^m, q_v^t phải thuộc Q_v. Đối với hai đỉnh đặc biệt σ và τ, tập X_σ = X_τ = ∅ vì chúng không gắn với vùng lồi nào — chúng chỉ là điểm neo đầu và cuối.

**[Tổng kết slide 1]**

Tóm lại, ta đã chuyển bài toán từ tìm đường trong không gian phi lồi sang bài toán tối ưu luồng trên đồ thị GCS, trong đó mỗi đỉnh mang theo một đoạn quỹ đạo cục bộ hoàn toàn nằm trong vùng lồi.

---

## Slide 2 — Ràng buộc kết nối và hàm chi phí

### Nội dung slide
- Ràng buộc cạnh X_e: điều kiện C⁰ và C¹ giữa hai vùng liền kề
- Hàm chi phí cạnh lồi ℓ_e
- *(Bên phải: hình gcs-2 và gcs-3)*

---

### Script

**[Bắt đầu slide, chỉ vào phương trình X_e]**

Sau khi đã có biến trạng thái trên từng đỉnh, ta cần xác định **ràng buộc cạnh** để đảm bảo các đoạn quỹ đạo nối liền nhau thành một quỹ đạo trơn. Với mỗi cạnh e = (v, u), ràng buộc X_e áp đặt hai điều kiện giữa điểm ra của vùng v và điểm vào của vùng u:

Điều kiện thứ nhất là **C⁰** — tính liên tục vị trí:

> q_v^t = q_u^s

Nghĩa là điểm kết thúc của đoạn quỹ đạo ở vùng v phải trùng với điểm bắt đầu của đoạn quỹ đạo ở vùng u. Không có bước nhảy vị trí tại ranh giới hai vùng.

Điều kiện thứ hai là **C¹** — tính liên tục đạo hàm bậc nhất, hay tốc độ:

> q_v^t − q_v^m = q_u^m − q_u^s

Đây là điều kiện đặc thù của đường Bézier bậc 2: vector tiếp tuyến tại điểm cuối của đoạn v phải bằng vector tiếp tuyến tại điểm đầu của đoạn u. Kết quả là quỹ đạo tổng hợp không bị gãy góc tại điểm chuyển tiếp — điều cần thiết để robot di chuyển mượt.

**[Chỉ vào hàm chi phí ℓ_e]**

Hàm chi phí trên mỗi cạnh được định nghĩa là tích phân bình phương tốc độ dọc theo đoạn quỹ đạo:

> ℓ_e(x_u, x_v) = ∫₀^{T_e} ‖q̇_e(t)‖² dt

Đây là tiêu chí tối thiểu hóa năng lượng di chuyển — hay nói cách khác, ưu tiên quỹ đạo ngắn nhất với tốc độ thấp nhất. Quan trọng là hàm này **lồi** theo các biến x_u, x_v — đây là tính chất sống còn để toàn bộ bài toán có thể giải bằng convex optimization.

**[Chỉ vào hình D của gcs-2]**

Nhìn lại hình D: ta thấy trong mỗi vùng Q_v, đoạn quỹ đạo màu xanh có ba điểm cam. Điều kiện C⁰ và C¹ vừa nêu chính là ràng buộc tại ranh giới giữa hai vùng liền kề: điểm cam cuối của vùng này phải khớp với điểm cam đầu của vùng kia, và hai vector dẫn vào ra tại đó phải song song và cùng chiều.

**[Chỉ vào hình E của gcs-3 — đồ thị với đường đi được chọn]**

Hình E cho thấy kết quả sau khi bài toán tối ưu chọn ra **đường đi tối ưu**: σ → 1 → 2 → 3 → 5 → τ. Các cạnh trên đường đi này được giữ lại; các cạnh còn lại — ví dụ qua đỉnh 4 — bị loại bỏ. Đây là thành phần **rời rạc** của bài toán: chọn tập hợp vùng nào robot sẽ đi qua.

**[Chỉ vào hình F của gcs-3 — quỹ đạo liên tục]**

Hình F là kết quả cuối cùng: quỹ đạo liên tục màu xanh chạy từ điểm xuất phát góc dưới trái đến điểm đích góc trên phải, lần lượt đi qua các vùng Q₁, Q₂, Q₃, Q₅. Tại mỗi ranh giới — các điểm cam — quỹ đạo nối liên tục và trơn nhờ ràng buộc C⁰ và C¹. Không có gãy góc, không có bước nhảy. Đây là quỹ đạo tối ưu mà bài toán SPP trên GCS tìm được.

**[Tổng kết và chuyển tiếp]**

Như vậy, toàn bộ bài toán hoạch định chuyển động đã được đưa về dạng:

> Tối thiểu hóa ∑_{e ∈ ε} ℓ_e(x_u, x_v) · y_e, với y_e ∈ {0, 1} là biến chỉ thị cạnh có thuộc đường đi hay không.

Đây là một bài toán **Mixed Integer Convex Program** — hỗn hợp biến nhị phân rời rạc và biến liên tục lồi. Thách thức là giải hiệu quả bài toán này. Phần tiếp theo tôi sẽ trình bày kỹ thuật **convex relaxation** cho phép ta xử lý phần rời rạc mà không cần duyệt tổ hợp.
