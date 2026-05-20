# Spec: 6-Slide DMS/OCP Roadmap cho Beamer Presentation

**Ngày:** 2026-05-19  
**File đích:** `beamer/motion_planning.tex`  
**Nguồn tham khảo:** `proposed-method/ocp_dms.tex`

---

## Bối cảnh và mục tiêu

Phần trình bày "Tiếp cận 2: GCS + Direct Multiple Shooting" hiện có 3 slide (DMS-1, DMS-2, DMS-3) chưa làm rõ:
1. Công thức OCP gốc đầy đủ với giải thích từng thành phần.
2. Lý thuyết DMS tổng quát — cơ chế giải OCP bằng cách rời rạc hóa.
3. Lý thuyết log-barrier và lý do chọn thay vì hard constraint.

**Mục tiêu:** Mở rộng thành 6 slide (slide 6 vào Appendix) để nổi bật effort học OC model và áp dụng vào bài toán GCS.

---

## Cấu trúc 6 slide

### Slide 1 — "Mô hình bài toán Điều khiển Tối ưu (OCP)"
**Vị trí:** Thay thế phần mở đầu của DMS-1 hiện tại.  
**Nội dung:**
- Phương trình động lực học: $\dot{x}(t) = f(x(t), u(t))$, $t\in[0,T]$
- Bài toán OCP đầy đủ (4 dòng):
  ```
  min  Φ(x(T), T) + ∫₀ᵀ ℓ(x(t), u(t)) dt
  s.t. ẋ(t) = f(x(t), u(t))
       x(0) = x^s,  x(T) = x^g
       x(t) ∈ Q,    u(t) ∈ U
  ```
- Bảng giải thích từng ký hiệu: Φ (terminal cost), ℓ (running cost), f (dynamics), Q (free space), U (control set), T (biến hoặc cố định)
- **Highlight box:** $Q$ phi lồi (có vật cản) → bài toán NP-hard → cần chiến lược phân hoạch

**Equation refs từ ocp_dms.tex:** `eq:ct_dyn`, `eq:orig_ocp_obj`–`eq:orig_ocp_path`

---

### Slide 2 — "Direct Multiple Shooting: ý tưởng rời rạc hóa OCP"
**Vị trí:** Slide mới, đứng sau Slide 1.  
**Nội dung:**
- **Vấn đề:** OCP tối ưu trên không gian hàm vô hạn chiều → không giải trực tiếp.
- **Ý tưởng DMS:**
  - Chia $[0,T]$ thành $N$ đoạn: $0 = t_0 < t_1 < \cdots < t_N = T$
  - Mỗi đoạn $k$: điều kiện đầu **độc lập** $s_k$ (biến tự do), tích phân dynamics → endpoint map $F_k(s_k, u_k)$
  - Tính liên tục áp đặt bằng **defect constraint**: $s_{k+1} - F_k(s_k, u_k) = 0$
- **Kết quả:** OCP $\infty$-dim → NLP hữu hạn chiều (giải bằng IPOPT/CasADi)
- **Ưu điểm so với single shooting:**
  - Ổn định số (không tích phân qua toàn bộ chân trời)
  - Song song hóa từng đoạn
- Sơ đồ minh họa: $s_0 \xrightarrow{F_0} \overset{\text{defect}}{=} s_1 \xrightarrow{F_1} \cdots \xrightarrow{F_{N-1}} s_N$

---

### Slide 3 — "Apply DMS vào GCS: rời rạc hóa theo vùng lồi"
**Vị trí:** Thay thế DMS-2 hiện tại (mở rộng).  
**Nội dung:**
- **Điểm khác biệt:** Trong GCS-DMS, các "đoạn" không phải khoảng thời gian đều mà là **vùng lồi $Q_v$**.
- Với mỗi vùng $v$: biến quyết định $(s_v^-, s_v^+, \Delta_v, w_v)$
  - $s_v^-$: điểm bắn (shooting point, điều kiện đầu IVP)
  - $s_v^+$: trạng thái ra (biến tự do)
  - $\Delta_v \ge 0$: thời lượng vật lý của đoạn
  - $w_v$: tham số tín hiệu điều khiển
- Chuẩn hóa thời gian $\tau\in[0,1]$:
  $$\frac{dx_v}{d\tau}(\tau) = \Delta_v\,f(x_v(\tau),\,u_v(\tau;\,w_v)), \quad x_v(0) = s_v^-$$
- Endpoint map: $F_v(s_v^-, w_v, \Delta_v) := x_v(1)$
- **Defect constraint:** $s_v^+ - F_v(s_v^-, w_v, \Delta_v) = 0$
- Sơ đồ: $s_v^- \xrightarrow{\text{IVP}_v} x_v(1) \overset{\text{defect}=0}{=} s_v^+ = s_{v+1}^-$

**Equation refs:** `eq:control_param_ocp`, `eq:local_ivp_ocp`, `eq:Fv_ocp`, `eq:defect_ocp`

---

### Slide 4 — "Ghép nối quỹ đạo: interface và network flow"
**Vị trí:** Slide mới, đứng sau Slide 3.  
**Nội dung:**
- **Biến interface** $z_{uv}\in Q_u\cap Q_v$: trạng thái tại khoảnh khắc chuyển vùng
- **Coupling constraints:**
  - $s_u^+ = z_{uv}$ (điểm cuối đoạn trong $Q_u$)
  - $s_v^- = z_{uv}$ (điểm đầu đoạn trong $Q_v$)
- **Điều kiện biên:** $s_v^- = x^s$ (nguồn), $s_v^+ = x^g$ (đích)
- **Network flow** (bảo toàn luồng + loại chu trình):
  $$\sum_{(\sigma,v)\in E} y_{\sigma v} = 1, \quad \sum_{(u,v)\in E} y_{uv} = \sum_{(v,w)\in E} y_{vw} = p_v$$
- **Phân loại bài toán:** Binary $y_{uv}, p_v$ + OCP phi tuyến → **MIOCP** (liên tục) / **MINLP** (sau DMS)

**Equation refs:** `eq:adjacency_ocp`, `eq:edge_coupling_ocp`, `eq:source_sink_flow_ocp`, `eq:flow_balance_ocp`

---

### Slide 5 — "Ràng buộc an toàn: từ hard constraint đến log-barrier"
**Vị trí:** Thay thế phần safety của DMS-3 (mở rộng đáng kể).  
**Nội dung:**
- **Thách thức:** $x_v(\tau)\in Q_v$ $\forall\tau\in[0,1]$ — ràng buộc continuous-time vô hạn chiều.
- **Cách 1 — Hard constraint (sampling):**
  - Kiểm tra tại $M_v+1$ điểm lưới: $A_v x_v(\tau_k) \le b_v - \varepsilon_v$
  - Ưu: đơn giản, không thay đổi hàm mục tiêu
  - Nhược: blind spot giữa các node lưới
- **Cách 2 — Log-barrier (được chọn):**
  $$\phi_v(x) = -\sum_{j=1}^{m_v}\log\!\bigl((b_v)_j - (A_v x)_j\bigr)$$
  - Xác định trên nội của $Q_v$; $\phi_v \to +\infty$ khi $x$ tiếp cận biên → tự nhiên ép quỹ đạo vào trong
  - Thêm vào hàm mục tiêu với trọng số $\mu_v > 0$ — không cần ràng buộc tường minh
  - **Chiến lược annealing:** giảm $\mu_v$ dần ($10^{-2} \to 10^{-5}$) → tiệm cận nghiệm gốc
  - NLP rời rạc hóa sau multiple shooting: $\sum_k \ell(x_k, u_k)\Delta t - \mu_v \sum_j\log(\ldots)$

**Equation refs:** `eq:log_barrier_def`, `eq:nlp_single_region`

---

### Slide 6 (Appendix) — "Mô hình tổng hợp MIOCP/MINLP đầy đủ"
**Vị trí:** Appendix, sau frame `\begin{frame}{Appendix}` (dòng 2012–2016 của `beamer/motion_planning.tex`).  
**Nội dung:**
- Hàm mục tiêu tổng hợp (compact):
  $$\min \sum_{v\in V_r} p_v\!\left[J_v(s_v^-, w_v, \Delta_v) + \int_0^1 \Delta_v\,\mu_v\,\phi_v(x_v(\tau))\,d\tau\right]$$
- Liệt kê 5 nhóm ràng buộc và vai trò:
  1. Network flow — chọn đúng 1 đường đi đơn giản
  2. IVP cục bộ + defect — tính liên tục nội bộ trong vùng
  3. Ràng buộc an toàn — sampling hoặc log-barrier
  4. Coupling + biên — tính liên tục toàn cục
  5. Ràng buộc nhị phân
- **Phân loại:** MIOCP → MINLP (vs GCS-Bézier → MISOCP); lý do không về được SOCP: $F_v$ là hàm ẩn phi tuyến từ IVP
- Chiến lược giải 2 pha: Pha 1 relax → fix path; Pha 2 warm-start → tối ưu NLP

**Equation refs:** `eq:joint_obj`–`eq:joint_binary`, `subsec:layered_decomposition`, `subsec:solver_strategy`

---

## Cấu trúc file sau khi sửa

```
[MAIN SLIDES]
...
\section{Xây dựng bài toán tối ưu hóa}
  Slide: Tiếp cận 1 (GCS-Bézier) — giữ nguyên
  Slide 1: Mô hình bài toán OCP         ← MỚI (thay DMS-1)
  Slide 2: DMS — ý tưởng rời rạc hóa    ← MỚI
  Slide 3: Apply DMS vào GCS            ← cải tiến DMS-2
  Slide 4: Ghép nối và network flow     ← MỚI
  Slide 5: Ràng buộc an toàn + barrier  ← cải tiến DMS-3
...
[APPENDIX]
  \begin{frame}{Appendix}...
  Slide 6: Mô hình tổng hợp đầy đủ      ← MỚI (appendix)
```

---

## Phạm vi không thay đổi

- Các slide GCS-Bézier (Tiếp cận 1) giữ nguyên.
- Frame slide thực nghiệm giữ nguyên.
- Preamble, theme, màu sắc không đổi.
- DMS-1 (overview) **bị thay hoàn toàn** bởi Slide 1 + cấu trúc mới; DMS-2 và DMS-3 **được thay** bởi Slide 3 và Slide 5.
