# Nội dung Poster — Bản nháp để review

> **Hướng dẫn dùng file này:** Đây là dự thảo nội dung dành riêng cho poster A0 (template `252_DATN_Poster_Shared/poster.tex`). Bạn chỉnh sửa trực tiếp các mục bên dưới (giữ nguyên cấu trúc heading), khi nào hài lòng thì báo lại để mình build poster LaTeX chính thức.
>
> **Quy tắc nội dung của khoa (bắt buộc đủ 6 mục):** Mã đề tài • Tên đề tài • GV hướng dẫn • SV thực hiện • Vấn đề giải quyết • Giải pháp • Kết quả.
>
> **Ngân sách chữ:** Toàn poster giữ ở mức ~500–700 từ. Mỗi section ~80–120 từ. Càng ngắn gọn càng tốt — poster phải đọc được từ 1.5–2m.

---

## 1. Header (Tiêu đề & thông tin nhóm)

- **Mã đề tài:** `HK252-DATN-028` *(điền chính xác mã do khoa cấp)*
- **Tên đề tài:** Hoạch định chuyển động cho rô-bốt sử dụng tối ưu lồi
- **Tên đề tài (EN, tuỳ chọn):** Robot Motion Planning via Convex Optimization
- **Giảng viên hướng dẫn:**
  - PGS. TS. Lê Hồng Trang
  - ThS. Trương Quỳnh Chi
- **Sinh viên thực hiện:**
  - Nguyễn Trần Đăng Khoa — MSSV 2211635
  - Hồ Đăng Khoa — MSSV 2211588
- **Đơn vị:** Khoa Khoa học và Kỹ thuật Máy tính, Trường ĐH Bách Khoa — ĐHQG-HCM
- **Học kỳ:** 252 — Tháng 5/2026
- **QR code link tới:** *(điền 1 trong các tuỳ chọn)*
  - [ ] Repo GitHub source code
  - [x] Video demo mô phỏng quỹ đạo
  - [ ] PDF luận văn đầy đủ
  - [ ] Khác: ___________

---

## 2. Đặt vấn đề (Problem Statement)

**Mục đích đoạn này:** giải thích trong ~80 từ tại sao bài toán quan trọng và thách thức gì.

### Bullet đề xuất (chỉnh tuỳ ý)

- **Bài toán:** Hoạch định quỹ đạo cho rô-bốt trong môi trường nhiều vật cản, sao cho quỹ đạo vừa **tránh va chạm**, vừa **trơn**, vừa **khả thi về mặt động lực học**.
- **Thách thức cốt lõi:** không gian tự do $\mathcal{C}_{\mathrm{free}}$ là **phi lồi** → bài toán trở thành **NP-hard** với phương pháp tổng quát.
- **Hạn chế của các nhóm phương pháp hiện có:**
  - **Đồ thị (A\*, Dijkstra):** quỹ đạo zigzag, không khai thác cấu trúc liên tục.
  - **Lấy mẫu (RRT, PRM):** kém tối ưu, khó nhúng ràng buộc kinodynamic.
  - **Tối ưu quỹ đạo trực tiếp (DMS):** dễ kẹt cực tiểu cục bộ ở vùng có vật cản.
- **Câu hỏi nghiên cứu:** Có thể kết hợp **lựa chọn đường đi rời rạc** với **tối ưu quỹ đạo liên tục** trong một khung tối ưu lồi/nguyên-hỗn hợp duy nhất không?


## 3. Giải pháp & Kiến trúc đề xuất (Methodology)

**Mục đích đoạn này:** mô tả pipeline trong ~120 từ, kèm 1 sơ đồ kiến trúc lớn.

### 3.1. Sơ đồ kiến trúc tổng thể (hình chính của poster)

```
[Môi trường có vật cản]
        ↓
[ Phân hoạch không gian tự do ]
   (ACD / IRIS / VCC)
        ↓
[ Đồ thị các vùng lồi G = (V, E) ]
        ↓
   ┌────────┴────────┐
   ↓                 ↓
[ GCS-Bézier ]   [ GCS-DMS ]
 (MISOCP)        (MIOCP / MINLP)
   ↓                 ↓
[Quỹ đạo hình học] [Quỹ đạo động lực học]
```

> *Mình sẽ render sơ đồ này thành hình TikZ đẹp khi build poster, hoặc sinh hình AI nếu bạn thích.*

### 3.2. Hai hướng tiếp cận đề xuất

**(A) GCS-Bézier — tối ưu hình học**
- Tham số hoá quỹ đạo bằng **đường cong Bézier bậc $d$** trong từng vùng lồi.
- Tính chất bao lồi → ràng buộc an toàn liên tục $\mathbf{x}(t)\in\mathcal{C}_{\mathrm{free}}$ trở thành ràng buộc tuyến tính trên điểm điều khiển.
- **Nới lỏng phối cảnh (perspective relaxation)** + **RLT** chuyển bài toán MICP về **MISOCP** giải hiệu quả bằng Mosek.

**(B) GCS-DMS — tối ưu động lực học**
- Trên mỗi vùng lồi: **Direct Multiple Shooting** rời rạc hoá $\dot x = f(x,u)$ thành các đoạn shooting.
- Ràng buộc an toàn xử lý theo 2 cơ chế: **lưới hữu hạn điểm** hoặc **hàm cản logarit** (log-barrier).
- Bài toán hợp nhất là **MIOCP** (liên tục) / **MINLP** (sau rời rạc hoá), giải bằng **CasADi + IPOPT**.

### 3.3. Đóng góp chính

1. Tích hợp **3 phương pháp phân hoạch** (ACD, IRIS, VCC) vào cùng pipeline GCS và phân tích đánh đổi.
2. Đề xuất **GCS-DMS với log-barrier** mở rộng GCS sang lớp bài toán có động lực học phi tuyến.
3. Hệ thống thực nghiệm so sánh trên 3 môi trường có độ phức tạp tăng dần.

---

## 4. Kết quả thực nghiệm (Results)

**Mục đích đoạn này:** trình bày 2–3 con số / bảng / hình ấn tượng nhất. Tránh nhồi mọi bảng.

### 4.1. Số liệu nổi bật (3 con số chủ đạo cho poster)

| Chỉ số | GCS-Bézier | GCS-DMS |
|---|---|---|
| Quy mô lớn nhất kiểm chứng | Mê cung **25×25** = 625 ô | Mê cung **15×15** + 2D đơn giản |
| Thời gian giải trung bình | **85,2 s** (Bézier-GCS, 10 mê cung) | **55,5 s** (4 seed, 15×15) |
| Defect động lực học | — | $10^{-10} \sim 10^{-8}$ (khả thi số học) |
| Bộ giải | Mosek (SOCP relaxation) | CasADi + IPOPT |

### 4.2. Thực nghiệm 1 — Môi trường 2D đơn giản (so sánh phân hoạch)

| Phân hoạch | Số vùng | Thời gian giải | Path cost |
|---|---|---|---|
| Thủ công | 12 | 3,90 s | 27,88 |
| **ACD** | **15** | **4,31 s** | **24,63** |
| VCC | 33 | 269,94 s | 24,44 |

> **Take-away:** ACD đạt cân bằng tốt nhất giữa số vùng và thời gian giải; VCC cho path cost thấp hơn rất nhỏ (~1%) nhưng đắt gấp **~64×** về thời gian.

### 4.3. Thực nghiệm 2 — Mê cung 25×25 (10 seed, GCS-Bézier)

- **Linear-GCS:** 1,28 s · cost 71,88 (chỉ tối ưu độ dài hình học)
- **Bezier-GCS:** 85,22 s · cost 88,03 (thoả ràng buộc $C^2$, vận tốc, gia tốc)
- Relaxation cho ra **xấp xỉ binary nghiệm** trong mọi mẫu thử → không cần rounding.

### 4.4. Thực nghiệm 3 — Xe Unicycle nonholonomic (GCS-DMS)

- 4 mê cung 15×15 (seed 4, 17, 18, 20). Tất cả đều hội tụ.
- Defect $\le 1{,}33\times10^{-9}$, vi phạm hình học $\le 10^{-8}$ → **quỹ đạo khả thi động lực học**.
- Vận tốc giảm tự nhiên ở các góc cua hẹp → đúng với mô hình Unicycle.

### 4.5. Hình ảnh nên đưa vào poster (chọn 2–3 cái)

- [ ] Quỹ đạo Bézier trên mê cung 25×25 (file: `experiments/image/maze-1.png`)
- [ ] So sánh phân hoạch ACD vs VCC trên môi trường 2D (file: `experiments/image/ACD_decompose.png`, `VCC_decompose.png`)
- [ ] Quỹ đạo GCS-DMS với hồ sơ vận tốc Unicycle (file: `experiments/image/dms/seed4_vel.png`)
- [ ] Sơ đồ pipeline GCS (sinh mới bằng TikZ hoặc AI)

---

## 5. Kết luận & Hướng phát triển (Conclusions)

**Mục đích đoạn này:** ~60 từ tổng kết + 2–3 hướng tương lai.

### 5.1. Kết luận

- Đã xây dựng pipeline GCS hoàn chỉnh từ **phân hoạch không gian** → **đồ thị vùng lồi** → **tối ưu quỹ đạo** với 2 hướng bổ sung nhau.
- **GCS-Bézier** ưu thế về tốc độ và cấu trúc lồi (MISOCP), phù hợp các bài toán hình học quy mô lớn.
- **GCS-DMS** mô hình hoá trực tiếp động lực học, đảm bảo quỹ đạo khả thi cho rô-bốt nonholonomic.
- **ACD** là lựa chọn phân hoạch thực dụng nhất cho môi trường 2D.

### 5.2. Hướng phát triển

- Tích hợp GCS-DMS vào **MPC trực tuyến** (warm-start giữa các chu kỳ điều khiển).
- Mở rộng sang **GCS không-thời gian** cho môi trường có vật cản di chuyển.
- Áp dụng **C-IRIS / IRIS-NP** để xử lý cánh tay rô-bốt nhiều bậc tự do (7-DoF).

---

## 6. Tài liệu tham khảo (chỉ chọn 3–5 tài liệu cốt lõi)

1. T. Marcucci, J. Umenberger, P. A. Parrilo, R. Tedrake. *Shortest Paths in Graphs of Convex Sets*, arXiv:2101.11565, 2023.
2. T. Marcucci, M. Petersen, D. von Wrangell, R. Tedrake. *Motion Planning around Obstacles with Convex Optimization*, Science Robotics, 2023.
3. R. Deits, R. Tedrake. *Approximating Robot Configuration Spaces with Few Convex Sets using Clique Covers of Visibility Graphs*, 2023.
4. J. M. Lien, N. M. Amato. *Approximate Convex Decomposition of Polygons*, CGTA, 2006.
5. M. Diehl, H. G. Bock, H. Diedam, P. B. Wieber. *Fast Direct Multiple Shooting Algorithms for Optimal Robot Control*, 2006.

---

## 7. Phong cách & Bố cục Poster (để mình lên đúng layout)

### 7.1. Bố cục 2 cột (theo template)

```
┌─────────────────── HEADER ────────────────────────────┐
│  [HCMUT]   Tên đề tài + thông tin nhóm   [QR]         │
├──────────────────┬────────────────────────────────────┤
│ Cột 1            │ Cột 2                              │
│  • Đặt vấn đề    │  • Kết quả thực nghiệm             │
│  • Giải pháp &   │     - Bảng số liệu                 │
│    Kiến trúc     │     - 2 hình quỹ đạo               │
│    (sơ đồ chính) │  • Kết luận & Hướng phát triển     │
│                  │  • Tài liệu tham khảo              │
└──────────────────┴────────────────────────────────────┘
```

### 7.2. Màu chủ đạo (đã có sẵn trong `beamerthemesharelatex.sty`)

- **Section header:** `RGB(3, 43, 145)` — xanh đậm HCMUT
- **Title & subsection:** `RGB(20, 136, 219)` — xanh sáng
- **Background:** `RGB(255, 253, 250)` — trắng ngà
- → giữ nguyên, không đổi.

### 7.3. Tự kiểm tra trước khi build (checklist)

- [x] Có đủ 6 mục bắt buộc của khoa.
- [x] Tổng số chữ < 700.
- [x] Mã đề tài và QR code đã thay đúng (xoá placeholder `???`).
- [x] Hình ảnh dùng có sẵn ở `image/`
- [x] Bảng kết quả: chốt 1 bảng quan trọng nhất (đề xuất Bảng 4.2 — so sánh phân hoạch) thay vì 3 bảng nhỏ.

---

## 8. Câu hỏi cần bạn quyết định trước khi build poster

1. **Mã đề tài chính xác** là gì? (placeholder hiện đang `HK252-DATN-???`) - 028
2. **QR code** sẽ link tới đâu? (GitHub repo, video demo, PDF luận văn, hay logo khoa?) - video demo
3. **Hình ảnh chủ đạo** muốn dùng (tick các ô ở mục 4.5) — nếu thiếu, mình có thể sinh mới bằng AI / vẽ bằng TikZ. - ưu tiên sử dụng kết quả thực nghiệm có sẵn (conv_2d.png,...)
4. Có muốn **giữ tiếng Việt 100%** hay xen tiếng Anh cho thuật ngữ kỹ thuật (GCS, MISOCP, DMS, …)? *(template hiện hỗ trợ Tiếng Việt T5 + babel)* - xen tiếng Anh cho thuật ngữ kỹ thuật
5. Có cần **2 phiên bản** poster (1 chi tiết để in, 1 rút gọn để post mạng xã hội) hay chỉ cần 1? - Cho 3 bản để lựa chọn

Sau khi bạn duyệt/chỉnh sửa file này, mình sẽ:
1. Cập nhật `poster.tex` với nội dung đã chốt.
2. Render sơ đồ kiến trúc bằng TikZ (hoặc sinh ảnh AI nếu bạn yêu cầu).
3. Build PDF và kiểm tra overflow / kích thước A0.
