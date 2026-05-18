# Script thuyết trình: GCS-DMS — Tiếp cận MINLP cho Hoạch định Chuyển động

---

## Slide DMS-1 — GCS-DMS gộp graph problem và OCP cục bộ thành một bài toán MINLP duy nhất

### Nội dung slide

- **Cột trái — graph problem (network flow):**
  - `y_{uv} ∈ {0,1}`: cạnh (u,v) được chọn
  - `p_v ∈ {0,1}`: vùng v được kích hoạt
  - Interface `z_{uv} ∈ Q_u ∩ Q_v`, `s_u⁺ = s_v⁻ = z_{uv}`
  - Hàm chi phí tổng: `J_total = Σ_{v∈V_r} p_v · J_v(s_v⁻, w_v, Δ_v)`

- **Cột phải — OCP trên vùng v (khi p_v = 1):**
  - `J_v = a·Δ_v` (thời lượng) + `w_L·∫(1/Δ_v)|dq_v/dτ|² dτ` (vận tốc hình học) + `w_E·∫Δ_v|u_v|² dτ` (năng lượng điều khiển)
  - `q_v(τ) := π(x_v(τ))` là hình chiếu vị trí; `1/Δ_v · dq_v/dτ = q̇_v`

- **Alert box — Phân loại bài toán:**
  - Biến nhị phân `p_v, y_{uv}` + OCP phi tuyến → **MIOCP** (liên tục theo thời gian) / **MINLP** (sau khi DMS rời rạc hóa điều khiển)

---

### Script

**[Bắt đầu slide, chỉ vào tiêu đề]**

Cho đến đây, ta đã thấy cách GCS mô hình hóa bài toán chọn đường đi bằng convex relaxation. Tiếp cận thứ hai — gọi là **GCS-DMS** — đi xa hơn: thay vì giải rời rạc bài toán graph và bài toán quỹ đạo tuần tự, ta **gộp cả hai thành một bài toán tối ưu duy nhất**.

**[Chỉ vào cột trái — graph problem]**

Nhìn vào cột trái, đây là phần **graph problem** hay còn gọi là network flow. Ta có hai loại biến nhị phân: `y_{uv} ∈ {0,1}` cho biết cạnh `(u,v)` có thuộc đường đi hay không, và `p_v ∈ {0,1}` cho biết vùng `v` có được kích hoạt — tức là robot có đi qua vùng đó không.

Điều quan trọng là **interface constraint**: tại ranh giới giữa hai vùng `u` và `v`, điểm chuyển tiếp `z_{uv}` phải nằm trong giao `Q_u ∩ Q_v`. Cụ thể hơn, trạng thái ra của vùng `u` phải bằng trạng thái vào của vùng `v` — đây là điều kiện ghép nối đảm bảo quỹ đạo liên tục qua ranh giới.

Hàm chi phí tổng là tổng chi phí cục bộ trên tất cả các vùng được kích hoạt, mỗi chi phí `J_v` nhân với cờ `p_v`. Nếu vùng không được chọn, `p_v = 0` và chi phí đó bị loại bỏ hoàn toàn.

**[Chỉ vào cột phải — OCP và công thức J_v]**

Cột phải là bài toán **OCP cục bộ** trên mỗi vùng `v` khi nó được kích hoạt. Hàm chi phí `J_v` có ba số hạng, được chú thích trực tiếp trong công thức:

Số hạng đầu tiên `a·Δ_v` phạt **thời lượng** của đoạn — robot không được đứng yên quá lâu trong một vùng.

Số hạng thứ hai tích phân bình phương tốc độ hình học của hình chiếu vị trí `q_v`, chuẩn hóa theo thời gian. Đây là tiêu chí **vận tốc hình học** — ưu tiên quỹ đạo ngắn và đều.

Số hạng thứ ba tích phân bình phương tín hiệu điều khiển `u_v`, nhân với `Δ_v` để đổi về thời gian thực. Đây là tiêu chí **năng lượng điều khiển** — robot không dùng lực quá mức cần thiết.

Lưu ý quan trọng về biến `τ ∈ [0,1]`: đây là thời gian chuẩn hóa. Quan hệ `Δ_v dτ = dt` cho phép ta cố định miền tích phân trên `[0,1]` bất kể thời lượng thực `Δ_v` là bao nhiêu — đây là kỹ thuật chuẩn của Multiple Shooting.

**[Chỉ vào alert box ở cuối slide]**

Điều này dẫn đến phân loại bài toán quan trọng ở đây: sự kết hợp giữa **biến nhị phân** `p_v, y_{uv}` và **OCP phi tuyến** tạo ra một bài toán thuộc lớp **MIOCP** — Mixed-Integer Optimal Control Problem — khi điều khiển còn liên tục theo thời gian. Sau khi áp dụng DMS để rời rạc hóa điều khiển, bài toán trở thành **MINLP** — Mixed-Integer Nonlinear Program — một lớp bài toán hữu hạn chiều có thể giải bằng các solver hiện đại.

**[Chuyển tiếp sang slide sau]**

Vậy DMS rời rạc hóa điều khiển như thế nào, và tính liên tục của quỹ đạo được đảm bảo bằng cơ chế nào? Đó là nội dung của slide tiếp theo.

---

## Slide DMS-2 — DMS tham số hóa đoạn quỹ đạo, buộc tính liên tục qua defect constraint

### Nội dung slide

- **Cột trái — biến quyết định trên mỗi vùng v:**
  - `s_v⁻ ∈ ℝⁿˣ` — trạng thái vào (điểm bắn)
  - `s_v⁺ ∈ ℝⁿˣ` — trạng thái ra (độc lập với s_v⁻)
  - `Δ_v ≥ 0` — thời lượng đoạn
  - `w_v` — tham số hóa điều khiển `u_v(τ) = U_v(τ; w_v)`
  - **IVP cục bộ:** `dx_v/dτ = Δ_v · f(x_v(τ), u_v(τ))`,  `x_v(0) = s_v⁻`

- **Cột phải — ràng buộc defect (then chốt DMS):**
  - `s_v⁺ − F_v(s_v⁻, w_v, Δ_v) = 0`
  - `F_v(...)` := `x_v(1)` là trạng thái cuối thu được sau khi tích phân IVP
  - Tính liên tục áp đặt *sau* tích phân, không cần tích phân tuần tự

---

### Script

**[Bắt đầu slide, chỉ vào cột trái]**

Slide này mô tả cách DMS — **Direct Multiple Shooting** — tham số hóa quỹ đạo trên từng vùng được kích hoạt.

Ý tưởng cốt lõi của Multiple Shooting là: thay vì tích phân liên tục từ đầu đến cuối toàn bộ quỹ đạo, ta **chia nhỏ** quỹ đạo thành từng đoạn độc lập — mỗi vùng `v` tương ứng một đoạn — rồi **tối ưu đồng thời** tất cả các đoạn, chỉ yêu cầu chúng nối liền nhau thông qua ràng buộc defect.

Trên mỗi vùng `v`, có bốn nhóm biến quyết định. Đầu tiên là `s_v⁻` — **trạng thái vào**, hay còn gọi là điểm bắn: đây là trạng thái khởi đầu cho bài toán IVP cục bộ. Thứ hai là `s_v⁺` — **trạng thái ra**, được khai báo **độc lập** với `s_v⁻`: đây là điểm mà đoạn tiếp theo sẽ xuất phát từ, và ban đầu không nhất thiết phải bằng trạng thái cuối của tích phân. Thứ ba là `Δ_v ≥ 0` — **thời lượng** của đoạn. Cuối cùng là `w_v` — **tham số hóa điều khiển**: thay vì lưu điều khiển tại mọi thời điểm, ta tham số hóa hàm điều khiển `u_v(τ)` bởi một vector hữu hạn chiều `w_v`, chẳng hạn bằng hàm hằng từng khúc hoặc đa thức bậc thấp.

**[Chỉ vào phương trình IVP]**

Bài toán IVP cục bộ trên vùng `v` được viết theo thời gian chuẩn hóa `τ ∈ [0,1]`: đạo hàm trạng thái bằng `Δ_v` nhân với động học `f`, xuất phát từ `s_v⁻`. Vì `τ ∈ [0,1]` cố định, ta có thể tích phân song song tất cả các đoạn mà không phụ thuộc vào kết quả của đoạn trước.

**[Chỉ vào alert box — defect constraint]**

Đây là **then chốt** của phương pháp DMS. Gọi `F_v(s_v⁻, w_v, Δ_v)` là trạng thái cuối `x_v(1)` thu được sau khi tích phân IVP từ `s_v⁻` với điều khiển `w_v` trong thời lượng `Δ_v`. Ràng buộc defect yêu cầu:

> `s_v⁺ − F_v(s_v⁻, w_v, Δ_v) = 0`

Hay nói khác đi: trạng thái ra `s_v⁺` mà ta khai báo độc lập phải **trùng khớp** với trạng thái cuối thực sự của tích phân. Đây là ràng buộc đẳng thức phi tuyến trong bài toán MINLP.

Điều này mang lại một lợi thế lớn về **tính song song**: ta có thể tích phân tất cả các đoạn đồng thời, mỗi đoạn bắt đầu từ `s_v⁻` hiện tại, rồi sau đó mới kiểm tra và áp đặt ràng buộc defect. Không cần chờ kết quả đoạn trước để tích phân đoạn sau — đây là ưu thế quyết định của Multiple Shooting so với Single Shooting truyền thống.

**[Tổng kết slide và chuyển tiếp]**

Tóm lại, DMS biến bài toán MIOCP liên tục thành một MINLP hữu hạn chiều: tập biến là `{s_v⁻, s_v⁺, Δ_v, w_v}` cho tất cả các vùng, với ràng buộc defect đảm bảo tính liên tục động học. Câu hỏi còn lại là: làm thế nào để giải MINLP này, và làm thế nào để đảm bảo quỹ đạo luôn nằm trong vùng an toàn ở mọi thời điểm — kể cả giữa các node tích phân?

---

## Slide DMS-3 — Giải MINLP; ràng buộc an toàn qua lấy mẫu hoặc log-barrier

### Nội dung slide

- **Cột trái — quy trình giải 2 pha và cơ chế an toàn:**
  - **Pha 1 — Chọn đường đi:** relax `y_{uv} ∈ {0,1}` sang `[0,1]`, giải convex problem trên toàn GCS, làm tròn để cố định chuỗi vùng `Q_{v₁} → ··· → Q_{v_K}`
  - **Pha 2 — Tối ưu quỹ đạo:** warm-start từ pha 1, tối ưu đồng thời `(s_v±, w_v, Δ_v)` trên các vùng active
  - **Cơ chế an toàn continuous-time:**
    - *Lấy mẫu:* `A_v x_v(τ_k) ≤ b_v − ε_v` — đơn giản, có blind spot giữa node
    - *Log-barrier:* `−μ_v ∫ Σ_j log(b_j − aⱼᵀ x_v) dτ` — liên tục, nhạy với `μ_v`

- **Cột phải — đầu ra và ba bảo đảm:**
  - **Đầu ra:** chuỗi vùng `Q_{v₁},…,Q_{v_K}`; quỹ đạo `x*(·)` liên tục, an toàn; điều khiển `u*(·)` hợp lệ; thời gian tổng `T* = Σ_v Δ_v*`
  - **Ba bảo đảm đồng thời:**
    - *Liên tục:* defect = 0 tại mọi giao điểm
    - *An toàn:* `x*(t) ∈ Q_{v(t)}` với mọi `t`
    - *Động lực học:* IVP thỏa trên từng vùng

---

### Script

**[Bắt đầu slide, chỉ vào cột trái — Pha 1]**

Bài toán MINLP thu được từ GCS-DMS có cấu trúc đặc thù: phần **rời rạc** chọn đường đi trên đồ thị, phần **liên tục** tối ưu quỹ đạo cục bộ. Ta khai thác cấu trúc này bằng chiến lược giải **hai pha**.

**Pha 1** giải quyết phần rời rạc. Ta relax biến nhị phân `y_{uv}` sang khoảng thực `[0,1]` — đây là convex relaxation tương tự như trong GCS-SCP đã trình bày trước. Bài toán relaxed là một convex program trên toàn đồ thị GCS, có thể giải hiệu quả bằng solver lồi. Sau khi có nghiệm liên tục, ta **làm tròn** để cố định chuỗi vùng `Q_{v₁} → ··· → Q_{v_K}` — đây là quyết định nhị phân về đường đi.

**[Chỉ vào Pha 2]**

**Pha 2** tối ưu quỹ đạo trên đường đi đã cố định. Ta **warm-start** — khởi động nóng — từ nghiệm pha 1: dùng quỹ đạo relaxed làm điểm khởi đầu cho solver phi tuyến. Trên các vùng active, ta tối ưu đồng thời toàn bộ biến `(s_v⁻, s_v⁺, w_v, Δ_v)` với đầy đủ ràng buộc defect và ràng buộc an toàn. Warm-start từ pha 1 giúp tránh các cực tiểu địa phương xấu và tăng tốc hội tụ đáng kể.

**[Chỉ vào cơ chế an toàn continuous-time]**

Một vấn đề tế nhị với Multiple Shooting là: ràng buộc an toàn `x_v(τ) ∈ Q_v` phải thỏa tại **mọi thời điểm** `τ ∈ [0,1]`, không chỉ tại các node tích phân rời rạc. Ta có hai lựa chọn.

**Lấy mẫu** là cách đơn giản nhất: kiểm tra ràng buộc `A_v x_v(τ_k) ≤ b_v − ε_v` tại một tập hữu hạn các thời điểm `τ_k`. Dễ implement, nhưng có **blind spot**: giữa hai node lấy mẫu liên tiếp, quỹ đạo có thể thoát ra ngoài vùng mà không bị phát hiện. Khoảng cách `ε_v > 0` là lề an toàn phần nào bù đắp điều này.

**Log-barrier** là cách chính xác hơn: thay vì kiểm tra tại điểm rời rạc, ta thêm vào hàm chi phí một số hạng phạt âm `−μ_v ∫ Σ_j log(b_j − aⱼᵀ x_v) dτ`. Hàm log-barrier tiến đến dương vô cực khi quỹ đạo áp sát biên vùng, tạo rào cản **liên tục theo thời gian**. Nhược điểm là nhạy cảm với tham số `μ_v`: giá trị quá nhỏ không ngăn vi phạm, quá lớn làm bài toán khó tối ưu.

**[Chỉ vào cột phải — đầu ra]**

Kết quả của toàn bộ quy trình là bốn thành phần đầu ra. Trước tiên là **chuỗi vùng** `Q_{v₁}, …, Q_{v_K}` — đường đi trên đồ thị. Tiếp theo là **quỹ đạo** `x*(·)`: liên tục và an toàn trên toàn bộ horizon thời gian. Kèm theo là **tín hiệu điều khiển** `u*(·)` hợp lệ — thu được từ tham số `w_v*` của từng đoạn. Cuối cùng là **thời gian tổng** `T* = Σ_v Δ_v*` — tổng thời lượng của tất cả các vùng được kích hoạt.

**[Chỉ vào ba bảo đảm]**

GCS-DMS cung cấp ba bảo đảm **đồng thời** — đây là điểm phân biệt so với các phương pháp đơn giản hơn.

**Bảo đảm thứ nhất — liên tục:** ràng buộc defect bằng không tại mọi giao điểm giữa các vùng, nên không có bước nhảy trạng thái tại ranh giới.

**Bảo đảm thứ hai — an toàn:** quỹ đạo `x*(t)` luôn nằm trong vùng `Q_{v(t)}` tương ứng với thời điểm `t`, nhờ cơ chế lấy mẫu hoặc log-barrier.

**Bảo đảm thứ ba — động lực học:** IVP được thỏa trên từng vùng, tức là quỹ đạo tôn trọng mô hình động học của robot — không phải chỉ là đường nối điểm tùy ý.

**[Tổng kết toàn bộ phần DMS]**

Như vậy, GCS-DMS cung cấp một framework thống nhất: từ một bài toán MINLP duy nhất, ta thu được đồng thời cả đường đi trên đồ thị lẫn quỹ đạo tối ưu tôn trọng động học và an toàn. Chi phí là độ phức tạp tính toán cao hơn — đặc biệt là pha 2 với solver phi tuyến — nhưng chất lượng nghiệm và tính đảm bảo lý thuyết vượt trội so với cách tiếp cận hai bước tuần tự.
