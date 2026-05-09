# CLAUDE_REVIEW_v1 — Tổng review luận văn

> Ngày review: 2026-05-09  
> Reviewer: Claude Sonnet 4.6  
> Trạng thái: Các sửa đổi bắt buộc đã được áp dụng trực tiếp. Tài liệu này ghi lại những gì đã sửa, những điểm còn tồn đọng, và khuyến nghị cho future work.

---

## 1. Tổng quan cấu trúc luận văn

### 1.1 Cây thư mục và include path

```
Report Source/
├── main.tex                    ← entry point
├── lib.sty                     ← custom macros, theorem envs, TikZ libs
├── refs.bib                    ← bibliography
├── abstraction/main.tex        ← abstract (tiếng Việt và Anh)
├── introduction/
│   ├── main.tex
│   ├── motivation.tex
│   ├── goals.tex
│   └── image/
├── theoretical-background/
│   ├── main.tex
│   ├── convex-optimization.tex
│   ├── graph-theory.tex
│   ├── motion-planning.tex
│   ├── OCP.tex
│   └── MIP.tex
├── related-work/
│   ├── main.tex
│   ├── classic_method.tex
│   ├── ocp_dms_method.tex
│   └── discussion.tex          ← TỒN TẠI nhưng KHÔNG được include
├── proposed-method/
│   ├── main.tex
│   ├── ocp_dms.tex
│   └── discussion.tex          ← MỚI TẠO (session này)
├── experiments/                ← chứa cả file của Chapter 4 lẫn Chapter 5
│   ├── prb_statement.tex       ← thuộc Chapter 4 (include từ proposed-method/main.tex)
│   ├── cnx_decomp.tex          ← thuộc Chapter 4
│   ├── GCS.tex                 ← thuộc Chapter 4
│   ├── relaxation.tex          ← thuộc Chapter 4
│   ├── gcs_bezier.tex          ← thuộc Chapter 4
│   ├── results.tex             ← thuộc Chapter 5
│   └── gcs_bezier.tex
├── future-plan/main.tex
└── image/
```

### 1.2 Vấn đề cấu trúc thư mục (ĐÃ GHI NHẬN, CHƯA SỬA)

**Vấn đề:** `proposed-method/main.tex` include 5 file từ thư mục `experiments/`:
```latex
\input{experiments/prb_statement}
\input{experiments/cnx_decomp}
\input{experiments/GCS}
\input{experiments/relaxation}
\input{experiments/gcs_bezier}
```

**Tại sao đây là vấn đề:** Về mặt ngữ nghĩa, các file này thuộc nội dung của **Chương 4 (Phương pháp đề xuất)**, không phải Chương 5 (Thực nghiệm). Cấu trúc hiện tại gây nhầm lẫn về tổ chức thư mục.

**Khuyến nghị:** Di chuyển các file trên vào `proposed-method/`:
```
proposed-method/
├── main.tex
├── prb_statement.tex
├── cnx_decomp.tex
├── gcs.tex               (đổi tên từ GCS.tex)
├── relaxation.tex
├── gcs_bezier.tex
├── ocp_dms.tex
└── discussion.tex
```
Cập nhật tất cả `\input{}` tương ứng. Đây là refactor thuần túy, không ảnh hưởng nội dung.

**Lưu ý:** Chưa thực hiện để tránh rủi ro khi compile; cần kiểm tra thủ công.

## 3. Review từng chương (bỏ qua Chapter 5)

### Chương 1 — Giới thiệu

**Điểm mạnh:**
- Động lực rõ ràng qua ví dụ Atlas robot và Amazon Robin
- Phân tích 3 nhóm phương pháp (graph, sampling, trajectory opt) với hạn chế cụ thể
- Mục tiêu được phát biểu rõ ràng

**Điểm cần chú ý:**
- `motivation.tex:38-39`: "Tuy vậy, mô hình này chủ yếu xử lý các ràng buộc ở cấp độ hình học và **có thể gặp hiện tượng vi phạm biên vùng an toàn tại các điểm chuyển tiếp**" — khẳng định này cần citation hoặc làm rõ cơ chế (khi nào xảy ra, tại sao).
- `goals.tex` đã sửa: mục tiêu 2 (OCP-DMS) vẫn nói "thu được quỹ đạo ... không vi phạm biên của các vùng an toàn tại mọi thời điểm liên tục" — đây là nhờ log-barrier nhưng log-barrier không đảm bảo tuyệt đối (chỉ penalize mạnh). Cần làm rõ đây là "khuyến khích" chứ không phải "đảm bảo tuyệt đối".

---

### Chương 2 — Cơ sở lý thuyết

**Điểm mạnh:**
- Bao phủ đủ nền tảng: convex opt, graph theory, motion planning, OCP, MIP
- Các định nghĩa và định lý được trình bày formal

**Điểm cần chú ý:**
- Chưa xem xét chi tiết trong session này; cần review riêng nếu cần

---

### Chương 3 — Các nghiên cứu liên quan

**Điểm mạnh (sau khi sửa):**
- Nay có section riêng về GCS framework và GCS-MotionPlanning
- 3 nhóm phương pháp kinh điển được phân tích đầy đủ

**Điểm còn tồn đọng:**
- `related-work/discussion.tex` TỒN TẠI nhưng **KHÔNG được include** trong `related-work/main.tex`. File này chứa nội dung về so sánh các phương pháp — nên xem xét có include hay xóa. Nếu nội dung đã outdated, nên xóa để tránh nhầm lẫn.
- `related-work/graph_reasoning.tex` và `related-work/rag.tex` cũng tồn tại nhưng không được include — nên xem xét xóa.
- Section về OCP methods trong related work (`ocp_dms_method.tex`) cần xem xét kỹ để đảm bảo không trùng lặp quá nhiều với Chapter 4.

---

### Chương 4 — Phương pháp đề xuất

**4.1 Mô hình bài toán (`prb_statement.tex`)**

Tốt. Phát biểu tổng quát với phương trình `\eqref{eq:planning_problem}`, tách biệt hình học và thời gian. Đã sửa citation và terminology.

**4.2 Phân hoạch không gian (`cnx_decomp.tex`)**

Chưa review kỹ trong session này. Một lưu ý từ session trước: VCC step count và các label đã được sửa.

**4.3 GCS (`GCS.tex`)**

Tốt về mặt kỹ thuật. Đã xóa `\subsubsubsection`. Lưu ý:
- Phần "Đặc tả Hàm Chi phí Cạnh" với 2 `\paragraph{}` mới (`Chi phí Độ dài...` và `Chi phí Quỹ đạo...`) cần kiểm tra layout sau khi compile.

**4.4 Nới lỏng phối cảnh (`relaxation.tex`)**

Đã sửa hoàn chỉnh trong session trước: attribution, `~\label` bug, manual `\tag` → proper `\label`, `(~\ref{})` → `~\eqref{}`. Đã xóa 3 `\subsubsubsection`.

**4.5 GCS-Bézier (`gcs_bezier.tex`)**

Có 2 figure TikZ đã được viết trong session trước:
- `fig:bezier_convex_hull`: Minh họa tính chất bao lồi với control polygon
- `fig:continuity_conditions`: Minh họa điều kiện C0/C1/C2 (3-panel)

Cần compile để kiểm tra layout thực tế của 2 figure này.

**4.6 OCP-DMS (`ocp_dms.tex`)**

Kỹ thuật chặt chẽ, đặc biệt phần giải thích tại sao không quy về MISOCP (lines 420-432). Phần so sánh đã được chuyển sang `discussion.tex`.

**4.7 Thảo luận (`discussion.tex`) — MỚI**

Section mới với bảng so sánh 8 tiêu chí. Cần review lại caption bảng và đảm bảo `\label{tab:comparison}` không trùng với label khác.

---

### Chương 6 — Hướng phát triển tương lai

**Đã có 4 mục:** Real-time MPC, Dynamic environments, C-IRIS, Log-barrier improvements.

**Thiếu sót cần bổ sung (xem Section 4).**

---

## 4. Danh sách thiếu sót và đề xuất future work

### 4.1 Thiếu sót kỹ thuật trong luận văn

| # | Vị trí | Vấn đề | Mức độ |
|---|--------|---------|--------|
| 1 | `gcs_bezier.tex` | Chưa phân tích độ nhạy theo bậc đa thức Bézier $n$: khi tăng $n$, số biến tăng bình phương, gap có thể thay đổi | Trung bình |
| 2 | `ocp_dms.tex` | Log-barrier penalty không đảm bảo constraint satisfaction trong trường hợp tổng quát (chỉ penalize) — luận văn cần nói rõ hơn | Cao |
| 3 | `cnx_decomp.tex` | IRIS hyperparameters (số vòng lặp, epsilon) ảnh hưởng mạnh đến chất lượng phân hoạch nhưng chưa được phân tích sensitivity | Trung bình |
| 4 | Toàn bộ | Không có benchmark runtime so với SOTA (Drake's GCS implementation, OMPL) | Cao |
| 5 | `prb_statement.tex` | Ràng buộc $T_{\min} \le T \le T_{\max}$ được đặt nhưng không rõ cách xử lý trong MISOCP khi $T$ là biến | Trung bình |
| 6 | Chapter 4 | Chưa có thảo luận về feasibility recovery khi MISOCP infeasible (không có đường đi trong GCS) | Thấp |

### 4.2 Đề xuất future work (để bổ sung vào `future-plan/main.tex`)

**Mục 5: Phân tích độ nhạy và lựa chọn siêu tham số**
- Nghiên cứu ảnh hưởng của bậc Bézier $n$, số vùng lồi $|V|$, và phương pháp phân hoạch (ACD/IRIS/VCC) lên chất lượng nghiệm và thời gian giải.
- Phát triển tiêu chí tự động lựa chọn $n$ dựa trên yêu cầu smoothness và tài nguyên tính toán.

**Mục 6: Đảm bảo an toàn liên tục cho OCP-DMS**
- Nghiên cứu thay thế log-barrier bằng Control Barrier Functions (CBF) hoặc S-procedure để có bảo đảm an toàn liên tục chính xác.
- Tích hợp ràng buộc an toàn dạng InequalityConstraint vào NLP thay vì chỉ dùng penalty.

**Mục 7: Mở rộng sang không gian cấu hình (C-space)**
- Thay thế IRIS trong Cartesian space bằng C-IRIS (`DeitsTedrake2022CIRIS`) để xử lý ràng buộc va chạm phi tuyến trong C-space của tay máy.
- Áp dụng GCS-Bézier trực tiếp trong C-space cho tay máy 6+ DOF.

**Mục 8: Parallel và warm-starting**
- Nghiên cứu giải song song cho các subproblem NLP trong multiple shooting (structure exploitation).
- Warm-starting từ nghiệm GCS-Bézier để cải thiện tốc độ hội tụ của OCP-DMS.

**Mục 9: Tích hợp với model predictive control (MPC)**
- Xây dựng scheme MPC dựa trên GCS để đảm bảo real-time replanning khi có vật cản động.
- Khai thác cấu trúc thưa của multiple shooting trong rolling horizon MPC.

---

## 5. Bảng thuật ngữ thống nhất

| Thuật ngữ tiếng Việt | Tiếng Anh | Ghi chú |
|---------------------|-----------|---------|
| Đồ thị các tập hợp lồi | Graph of Convex Sets (GCS) | Giữ "GCS" trong toàn văn |
| Nới lỏng phối cảnh | Perspective relaxation | Không dùng "perspective cone relaxation" lẫn "perspective relaxation" xen kẽ |
| Tham số hóa Bézier | Bézier parameterization | Luôn dùng accent: Bézier, không phải Bezier |
| Phân hoạch lồi xấp xỉ | Approximate Convex Decomposition (ACD) | |
| Phân hoạch lồi bằng bao lồi | Convex decomposition via Visibility Clique Cover (VCC) | |
| Vùng lồi phồng dựa trên SDP | Inflated Regions via SDP (IRIS) | |
| Điểm điều khiển | Control point | |
| Tính chất bao lồi | Convex hull property | |
| Bài toán điều khiển tối ưu | Optimal Control Problem (OCP) | |
| Bắn nhiều điểm trực tiếp | Direct Multiple Shooting (DMS) | |
| Hàm cản logarit | Log-barrier function | Không dùng "hàm phạt log-barrier" xen kẽ |
| Quy hoạch lồi nguyên hỗn hợp | Mixed-Integer Convex Program (MICP) | |
| Quy hoạch nón bậc hai nguyên hỗn hợp | Mixed-Integer Second-Order Cone Program (MISOCP) | |
| Quy hoạch phi tuyến nguyên hỗn hợp | Mixed-Integer Nonlinear Program (MINLP) | |
| Bài toán điều khiển tối ưu nguyên hỗn hợp | Mixed-Integer Optimal Control Problem (MIOCP) | |
| Kỹ thuật cải biến-tuyến tính hóa | Reformulation-Linearization Technique (RLT) | |
| Ràng buộc bất đẳng thức hợp lệ | Valid inequality | |
| Nón phối cảnh | Perspective cone | |
| Biến đại diện cục bộ | Local proxy variable | |
| Điều kiện tiếp tuyến | Continuity condition (C0/C1/C2) | Không dùng "continuity constraint" lẫn "smoothness condition" xen kẽ |
| Ràng buộc defect | Defect constraint | Giữ nguyên tiếng Anh (thuật ngữ kỹ thuật) |

---

## 6. Kiểm tra sau khi compile

Sau khi compile `pdflatex main.tex` (2 lần) + `biber main` + `pdflatex main.tex`, cần kiểm tra:

- [ ] Không còn `\subsubsubsection` undefined command warning
- [ ] 5 `\paragraph{}` headings hiển thị đúng (bold, inline, không đánh số)
- [ ] Bảng `tab:comparison` trong `discussion.tex` render đúng
- [ ] 2 TikZ figures (`fig:bezier_convex_hull`, `fig:continuity_conditions`) trong `gcs_bezier.tex` compile không lỗi
- [ ] Citation key `MarcucciEtAl2023MotionPlanning` được resolve đúng trong bibliography
- [ ] Section `sec:rw_gcs` xuất hiện trong TOC Chapter 3
- [ ] Section `sec:discussion` xuất hiện trong TOC Chapter 4
- [ ] Cross-references `\ref{sec:gcs_approach}` và `\ref{sec:ocp_dms_approach}` trong `classic_method.tex` resolve đúng
- [ ] Không có `undefined reference` warnings mới

---

## 7. File nên xem xét dọn dẹp

| File | Trạng thái | Khuyến nghị |
|------|-----------|-------------|
| `related-work/discussion.tex` | Tồn tại, KHÔNG được include | Xem xét xóa hoặc merge vào `classic_method.tex` |
| `related-work/graph_reasoning.tex` | Tồn tại, KHÔNG được include | Xem xét xóa |
| `related-work/rag.tex` | Tồn tại, KHÔNG được include | Xem xét xóa |
| `Report Source/main.synctex(busy)` | File tạm của SyncTeX | Xóa (đã bị xóa trong .gitignore?) |
| `papers/RP/report_v4.tex` | Draft cũ | Có thể giữ lại để tham khảo, không ảnh hưởng build |

---

*Hết review. Tất cả sửa đổi bắt buộc đã được áp dụng trực tiếp vào source files.*
