Đã đọc xong toàn bộ báo cáo (Tóm tắt, Giới thiệu, Cơ sở lý thuyết, Liên quan, Phương pháp đề xuất, Thực nghiệm, Kết luận). Dưới đây là phần phản biện được tổ chức theo từng chương, với từng điểm đều   
  kèm (a) lập luận yếu / lỗi logic, (b) câu hỏi phản biện và (c) đề xuất cải thiện.                                                                                                                        
                                                                                                                                                                                                           
  ---                                                                                                                                                                                                      
  A. Vấn đề tổng thể & nhất quán xuyên suốt báo cáo                                                                                                                                                        
                                                                                                                                                                                                           
  A1. Mâu thuẫn lớn nhất: Tóm tắt "phóng đại" vs nội dung thực tế                                                                                                                                          
                                                                                                                                                                                                           
  Lỗi logic: Phần Tóm tắt (abstraction/main.tex) khẳng định: "GCS giảm thời gian truy vấn lên tới một bậc so với PRM tiêu chuẩn, ngắn hơn 40%...", "validate trên 7-DoF manipulator, quadrotor" — nhưng đây
   là kết quả của bài báo gốc Marcucci 2023 (~\cite{DeitsTedrake2022}), không phải kết quả thực nghiệm của các tác giả. Thực nghiệm thật (Chương 5) chỉ chạy 2D + maze 25×25, không có quadrotor, không có 
  7-DoF, không có so sánh PRM.

  Câu hỏi phản biện:
  - Phần Tóm tắt đang trình bày kết quả của ai? Nếu là kết quả của luận văn này, vì sao Chương 5 không chứa các thực nghiệm đó?
  - Hội đồng đọc Tóm tắt sẽ kỳ vọng thấy 7-DoF/quadrotor — điều này có gây hiểu lầm về phạm vi đóng góp không?

  Đề xuất: Viết lại Tóm tắt chỉ bám sát những gì thực sự thực hiện: 2D, maze, (Dubins nếu kịp). Tách biệt rõ "kết quả luận văn" và "kết quả tham khảo từ Marcucci 2023".

  A2. Hai cụm chữ viết tắt xung đột: OCP-DMS vs OCP-MMS

  - proposed-method/main.tex ban đầu dùng OCP-MMS (commit c14552e), experiments/setup.tex lại dùng OCP-DMS, experiments/main.tex dùng GCS+DMS. Trong cùng một câu (experiments/setup.tex baseline list) có
  \ref{sec:ocp_DMS_approach} nhưng nhãn thật là \ref{sec:ocp_mms_approach} — đây là lỗi compile/cross-ref bị broken.
  Đề xuất: Thống nhất 1 viết tắt (đề nghị OCP-DMS) trên toàn báo cáo; sửa nhãn ref bị sai.

  A3. Mâu thuẫn giữa Goals/Scope và nội dung thực hiện

  Mục tiêu (goals.tex) hứa: phân hoạch bằng ACD, IRIS, VCC. Scope lặp lại 3 phương pháp này. Nhưng trong Thực nghiệm:
  - IRIS được mô tả ở Chương 2/4 nhưng không có thực nghiệm IRIS riêng (chỉ ACD vs VCC vs manual).
  - Kịch bản C (Dubins) được "thiết kế" nhưng resutls.tex không có kết quả Dubins (chỉ 2D đơn giản + maze).

  Câu hỏi:
  - Tại sao IRIS được mô tả chi tiết trong Chương 2 nhưng không có một thí nghiệm đối chứng nào?
  - Kịch bản C có thực sự đã chạy, hay vẫn ở dạng "thiết kế"?

  A4. contribution.tex rỗng — chương 1 thiếu phần "Đóng góp"

  File introduction/contribution.tex chỉ có 1 dòng. Đây là một trong những phần quan trọng nhất của Chương 1.

  A5. Hai tác giả, vai trò chia rời rạc – cơ sở lý thuyết & hiện thực — nhưng nội dung lý thuyết và thực nghiệm vẫn chưa thật sự ăn khớp với nhau ở các chỗ trọng yếu (xem A1, B5, D5).

  ---
  B. Chương 1 — Giới thiệu (motivation, goals, scope)

  B1. Bullet "Atlas / Robin" dẫn dắt sai trọng tâm luận văn

  Lập luận chưa chặt: Atlas (parkour) và Robin (chia hàng) là bài toán task-and-motion planning kết hợp tổ hợp+hình học cấp cao (footstep planning, bin assignment), trong khi luận văn chỉ giải bài toán
  shortest path qua các vùng lồi với điểm đầu–điểm cuối cố định. Khoảng cách giữa "động lực" và "phạm vi" là rất lớn.

  Câu hỏi phản biện: Luận văn này thực sự giải quyết được khía cạnh nào của Atlas/Robin? Nếu không, chọn ví dụ động lực gần với phạm vi thực hiện hơn (ví dụ AGV trong kho có hành lang, drone delivery
  3D).

  B2. So sánh ba nhóm phương pháp thiếu ngữ pháp logic

  "Phương pháp dựa trên đồ thị... đảm bảo tính đầy đủ theo độ phân giải" — Dijkstra/A* không phải resolution-complete trên không gian liên tục; đây là tính chất của graph search trên grid. Câu này nhập
  nhằng giữa thuật toán và sự rời rạc hóa.

  B3. Mục tiêu 2 có một khẳng định không có cơ sở chứng minh

  "Mục tiêu là thu được quỹ đạo vừa tối ưu toàn cục về mặt hình học, vừa khả thi về mặt động lực học, vừa không vi phạm biên... tại mọi thời điểm liên tục."

  Điều này bất khả thi về lý thuyết với khung MIOCP/MINLP đã thừa nhận là phi lồi (xem ocp_mms.tex mục subsec:problem_class). Sao có thể đảm bảo "tối ưu toàn cục" với MINLP phi lồi NP-hard?

  Câu hỏi: "Tối ưu toàn cục" đối với OCP-DMS được hiểu theo nghĩa nào — global của bài toán đầy đủ, hay global trong từng homotopy class, hay chỉ là local minimum của NLP?

  B4. Scope tự loại trừ "mô phỏng MPC thời gian thực" nhưng Future-Plan lại đặt nó là task tuần 1-3

  Mâu thuẫn: scope.tex nói "không bao gồm MPC", nhưng future-plan/main.tex lại đặt MPC là hướng phát triển và lịch 15 tuần đặt task "Kiểm thử và tích hợp Multiple Shooting" ở W1–3. Cần làm rõ đây là kế
  hoạch sau đồ án, không phải trong đồ án.

  ---
  C. Chương 2 — Cơ sở lý thuyết

  C1. Convex-optimization: Phần "Dual cone" bị comment-out nhưng vẫn được tham chiếu trong Chương 4 (relaxation.tex)

  relaxation.tex mục "Cơ sở Lý thuyết: Tính Đối ngẫu giữa Nón Phối cảnh và Bất đẳng thức Hợp lệ" dùng nón đối ngẫu trong khi mục Dual cone trong Chương 2 (convex-optimization.tex dòng 215–226) đã bị
  comment-out. Người đọc sẽ không có nền tảng.

  Đề xuất: Khôi phục mục Dual cone, hoặc forward-reference rõ ràng.

  C2. Định nghĩa "đường đi" không đồng nhất giữa Chương 2 và Chương 4

  graph.tex Định nghĩa def:path_cycle yêu cầu các đỉnh phân biệt (đường đi đơn). Nhưng trong GCS.tex (subsec gcs_complexity), tác giả lập luận GCS-SPP có thể "tương đương Hamiltonian" → tức đi qua tất cả
   các đỉnh. Hai khái niệm này không xung đột nhưng cần được kết nối tường minh: trong GCS, ràng buộc \sum y_e \le 1 ở mỗi đỉnh ép path là simple, nên Hamiltonian là trường hợp giới hạn chứ không phải
  "có thể".

  C3. Total Unimodularity được khẳng định nhưng không chứng minh hoặc trích dẫn cho biến thể có ràng buộc node-capacity (5.5c)

  graph.tex dòng 264-272 nói ma trận incidence là TU và bài toán nới lỏng LP có nghiệm nguyên — đây là sự thật cho classical SPP. Nhưng:
  - Khi thêm \sum_{w} x_{wu} \le 1 (node capacity, eq 5.4c), ma trận nói chung không còn TU.
  - Trong GCS, chi phí trở thành hàm của $x_v$ → vế phải không còn nguyên → kể cả TU cũng không đảm bảo nghiệm LP nguyên.

  Câu hỏi phản biện: Tại sao đoạn này nói "LP nới lỏng tự động có nghiệm nguyên" trong khi mục gcs_complexity lại nói GCS-SPP là NP-hard? Hai khẳng định này có trật tự lôgic ra sao?

  C4. Trong motion-planning.tex, mục đạo hàm Bézier viết nhầm

  Công thức (eq:bezier_derivative) viết:
  $$\dot{\gamma}(s) = \sum_{k=0}^{d-1} \beta_{k,d-1}(s),\dot{\gamma}_k$$

  Đúng phải là $\dot\gamma(s) = \frac{d\gamma}{ds}$, nhưng các điểm điều khiển sai phân của hodograph là $d(\gamma_{k+1} - \gamma_k)$ chứ không phải vận tốc theo thời gian. Trong gcs_bezier.tex đoạn
  $C^1$ tiếp tục dùng $\dot\gamma$ với hai nghĩa khác nhau (đạo hàm theo $s$ và đạo hàm theo $t$).

  Đề xuất: Tách rõ ký hiệu: $\gamma'(s)$ cho derivative theo $s$, $\dot\gamma(t)$ cho theo thời gian thực. Quy tắc dây chuyền: $\dot\gamma(t) = \gamma'(s) \cdot \dot s$.

  C5. multiple-shooting.tex lặp lại nội dung với proposed-method/ocp_mms.tex

  Cả hai file viết từ đầu khái niệm DMS, ký hiệu hơi khác (q_i vs w_v, \Delta_i vs \Delta_v). Nên Chương 2 chỉ trình bày DMS tổng quát, Chương 4 chỉ áp dụng và viện dẫn lại.

  ---
  D. Chương 3 — Các nghiên cứu liên quan

  D1. Thiếu công trình GCS gốc và GCS-MotionPlanning

  Marcucci 2023 (GCS gốc) và Marcucci 2024 thesis có được trích nhưng chỉ được giới thiệu ở Mục thảo luận (1 dòng). Cần một subsection riêng phân tích đóng góp của bản gốc và sự khác biệt của luận văn
  này.

  D2. Phần "tối ưu hoá quỹ đạo trực tiếp" mâu thuẫn nội tại

  Cùng một đoạn vừa nói "DTO cho phép nhúng động lực học của hệ thống... xử lý hiệu quả các hệ thống có ràng buộc vi phân phức tạp" vừa thừa nhận "không thể vượt qua hoàn toàn rào cản về cực tiểu cục
  bộ". Cần phát biểu rõ: DTO giải tốt khi non-convexity chỉ đến từ động lực học; gặp khó khi non-convexity đến từ tránh va chạm hình học.

  D3. Câu cuối khẳng định "MICP kết hợp đầy đủ của SBP với chính xác của TO" — quá mạnh

  Lập luận này lặp lại trong cả Tóm tắt, Discussion, Motivation. Thực tế GCS:
  - Không có probabilistic completeness của PRM (GCS phụ thuộc chất lượng phân hoạch).
  - Không xử lý động lực học phi tuyến trực tiếp (chính vì vậy mới cần OCP-DMS).

  Câu hỏi: GCS-Bézier xử lý được loại ràng buộc động lực học nào (chỉ kinematic giới hạn vận tốc/gia tốc), và không xử lý được loại nào (động lực học phi tuyến của manipulator, nonholonomic)?

  D4. discussion.tex chỉ là 1 đoạn dài 1 paragraph, không có subsection — khó đọc, lập luận chồng chéo.

  D5. Không có bảng so sánh các phương pháp đối thủ

  Một chương Related Work tốt thường có bảng tổng kết: SBP / DTO / MICP / GCS-Bézier / OCP-DMS theo các tiêu chí (completeness, optimality, kinodynamic, complexity). Đây là chỗ để đặt đóng góp luận văn.

  ---
  E. Chương 4 — Phương pháp đề xuất (cốt lõi)

  E1. Cấu trúc include khó hiểu — phương pháp đề xuất \input{experiments/...}

  proposed-method/main.tex \input{experiments/prb_statement}, GCS, relaxation, gcs_bezier, cnx_decomp — các file phương pháp lý thuyết nằm trong thư mục thực nghiệm. Đây là ngầm định gây nhầm lẫn cho
  người đọc và bảo trì. Refactor đưa các file này về proposed-method/.

  E2. Phát biểu bài toán chung (prb_statement.tex) thiếu tính khả vi của hàm thời gian

  Định nghĩa \mathbf{x}(t): [0,T] \to \mathbb{R}^d khả vi đến bậc K, nhưng GCS-Bézier ghép nhiều đoạn lại — tại điểm nối, chỉ được đảm bảo $C^2$ (theo gcs_bezier.tex). Như vậy với $K \ge 3$ thì các ràng
  buộc ở (eq:planning_problem) không thể thỏa mãn trong khung GCS-Bézier hiện hành. Cần phát biểu nhất quán: $K = 2$ trong GCS-Bézier; $K$ tùy ý cho OCP-DMS.

  E3. Trong GCS.tex subsec:gcs_vertex_edge, ràng buộc \dot{h}_{v,k} \ge \dot h_{\min} > 0

  Trước đó (eq:bgcs_mono) chỉ yêu cầu \dot h_{v,k} \ge \varepsilon. Hai nơi dùng ký hiệu khác nhau (\dot h_{\min} vs \varepsilon) cùng cho một thứ. Thống nhất.

  E4. relaxation.tex chứa nhiều khẳng định không chứng minh và một lập luận sai

  Lỗi nghiêm trọng (dòng 32 eq 5.5d):
  $$\sum_{e\in E_{in}(v)} (z'e, y_e) = \sum{e\in E_{out}(v)} (z_e, y_e)$$

  Cách viết vector cặp này không chuẩn — thực chất là 2 ràng buộc (spatial flow + binary flow). Nhưng tác giả viết: "ràng buộc này được suy ra bằng cách nhân phương trình bảo toàn luồng tại đỉnh v với
  biến trạng thái x_v". Đây là lập luận không đúng: ràng buộc spatial flow cho $z$ không thu được bằng phép nhân (nhân lại phá vỡ tính lồi), mà thu được bằng kỹ thuật RLT lift hoặc bằng việc tự định  nghĩa $z_{e}$ là biến đại diện. Cần viết lại lý do toán học chặt chẽ.

  E5. Dán Định lý 5.7, Mệnh đề... mà thiếu chứng minh

  - "Định lý 5.7" được nhắc nhưng không có chứng minh.
  - Mệnh đề $(\tilde X)^* \equiv X^\circ$ chứng minh có nhưng giả thiết "$k = (\lambda x, \lambda)$ với $x \in X, \lambda \ge 0$" cần điều kiện $X$ đóng & lồi để bao đóng của $\tilde X$ có dạng này —
  luận văn không nêu.

  E6. gcs_bezier.tex chứa nhiều "SKELETON" và placeholder chưa điền

  "[MÔ TẢ BẢN ĐỒ: kích thước, số vật cản,...]", "\textbf{Đề xuất nội dung hình}", comment ảnh % \includegraphics{} — báo cáo còn skeleton chưa hoàn thiện, không được phép có ở bản nộp.

  E7. OCP-DMS: Logic implication y_{uv}=1 ⟹ ... không phải ràng buộc lồi MIP-tractable

  Trong (eq:joint_edge), tác giả viết:
  $$y_{uv}=1 \Rightarrow \begin{cases} z_{uv}\in Q_u\cap Q_v,\ s_u^+=z_{uv},\ s_v^-=z_{uv} \end{cases}$$

  Điều này là logical/disjunctive constraint, không thể đưa thẳng vào solver. Cần cụ thể hoá: dùng big-M, perspective lifting, hoặc indicator constraint Gurobi. Mục subsec:layered_decomposition có nhắc
  tới homogenization nhưng chỉ là sơ lược. Đây là lỗ hổng kỹ thuật chính của Chương 4.

  E8. Lập luận log-barrier không tương thích với multiple shooting nguyên bản

  subsubsec:log_barrier: hàm cản $\phi_v(x)$ chỉ xác định trên interior của $Q_v$. Nhưng trong DMS, các trạng thái nội bộ $x_v(\tau_k)$ là kết quả của tích phân, nên có thể rơi ra ngoài $Q_v$ trong các
  bước lặp NLP đầu tiên → barrier value $= +\infty$ → IPOPT fail. Tác giả không bàn vấn đề khởi tạo (warm-start) hay homotopy continuation cho $\mu_v$ chi tiết.

  Câu hỏi phản biện:
  - Có chiến lược initialization nào đảm bảo $x_v(\tau_k)$ ban đầu nằm trong int $Q_v$?
  - Khi giải chuỗi bài toán $\mu_v: 10^{-2} \to 10^{-5}$, có cần điều kiện bắt buộc dùng nghiệm trước làm warm-start không? Nếu không hội tụ ở mức $\mu$ trung gian thì sao?

  E9. So sánh GCS-Bézier vs OCP-DMS thiếu một tiêu chí then chốt: bảo đảm an toàn liên tục thời gian

  GCS-Bézier với convex-hull property đảm bảo an toàn liên tục ($\forall t$). OCP-DMS với sampling lưới chỉ đảm bảo tại $\tau_k$. Log-barrier mới đảm bảo "đẩy ra xa biên" nhưng vẫn không đảm bảo
  continuous-time safety giữa các điểm lưới. Đây là trade-off ngược: OCP-DMS được "động lực học chặt", nhưng "an toàn yếu hơn".

  Câu hỏi: Trong subsec:safety_constraints, bạn nhận định OCP-DMS đảm bảo an toàn "tại mọi thời điểm liên tục" — điều này có đúng với cả Cách 1 (sampling) và Cách 2 (log-barrier) không?

  E10. Phân tích phức tạp NP-hard: chưa nói rõ tham số

  Mệnh đề $\text{prop:gcs_not_classical_spp}$ chứng minh rất gọn nhưng dùng kết quả NP-hard từ Marcucci 2024 mà không nêu tham số nào (số đỉnh? chiều biến? cấu hình $\mathcal X_v$?). Sinh viên hội đồng
  có thể hỏi "Hardness theo tham số nào?".

  ---
  F. Chương 5 — Thực nghiệm

  F1. Thiết lập (setup.tex) hứa 3 kịch bản, kết quả (resutls.tex) chỉ có 2

  Kịch bản C (Dubins) không có kết quả. Đây là kịch bản trọng tâm "khẳng định đóng góp của hướng tiếp cận thứ hai" theo lời tác giả.
  Hệ quả: OCP-DMS thực ra chưa có thực nghiệm số trong báo cáo.

  F2. Bảng so sánh (tab:comparison_all) trộn metric và đơn vị không nhất quán

  - "Path cost (rounded)" có đơn vị "?" — không rõ scale.
  - "Time cost" và "Tổng thời gian" có thể nhầm với nhau (compute time vs travel time).
  - VCC: 33 vùng, 269.94s — số quá lớn cho 2D đơn giản; cần check bộ giải hay setup có hợp lý không (Mosek với 33 vùng × 2D × bậc 6 thường dưới vài chục giây).

  F3. Lập luận "VCC tối ưu toán học hơn ACD"

  "VCC đạt path cost thấp nhất → nghiệm tối ưu nhất về mặt toán học" — sai: ba phương pháp giải bài toán trên các đồ thị khác nhau (số vùng khác nhau), nên path cost không thể trực tiếp so sánh "tối ưu
  toán học". Mỗi bài có nghiệm tối ưu riêng của bài đó, không phải nghiệm của bài chung.

  F4. "Tổng thời gian = phân hoạch + giải" — bỏ qua tham số $K$ top-paths cho OCP-DMS

  Bảng software stack nói OCP-DMS dùng "Gurobi MILP top-K". Vậy thời gian thật của OCP-DMS = $K \times$ NLP time + MILP time. Bảng không có cột tương ứng cho OCP-DMS.

  F5. Maze 25×25 = 625 ô, "loại bỏ ngẫu nhiên 100 bức tường" — số vùng lồi thực tế?

  Khi mỗi ô là 1 vùng và tường được "phá", các ô kề nhau gộp lại thành vùng lồi lớn hơn — số vùng cuối cùng không được nêu. Quan trọng vì nó quyết định kích thước bài toán MICP.

  F6. Nhận định "xác suất nhị phân $\phi_e$ hội tụ về {0,1} → tự động đạt tối ưu toàn cục mà không cần làm tròn"

  Đây là kết quả của Marcucci 2023 (relaxation gap thường = 0). Nhưng:
  - Cần định nghĩa rõ $\phi_e$ — tác giả chưa định nghĩa chính thức trong báo cáo.
  - "10/10 mê cung không cần rounding" là minh chứng mạnh; nhưng cần nói rõ tiêu chí (ngưỡng dung sai $\phi_e \in [0.5\pm\epsilon]$).

  F7. Câu "giảm số biến nhị phân từ $|I|^2$ xuống $2|I|$"

  Không rõ "$|I|^2$" này nói về phương pháp nào. MICP cổ điển obstacle-by-obstacle có $|I| \cdot N_{obs}$, không phải $|I|^2$. Cần trích dẫn cụ thể.

  F8. Không có thông số độ chính xác động lực học (jerk, độ giật, $\rho_{vio}$, $d_{\min}$)

  Setup hứa metric (B), (C) nhưng resutls.tex chỉ báo cáo "Time cost" và "Path cost". Các metric an toàn $d_{\min}$, $\rho_{vio}$ không xuất hiện ở mục kết quả → không thể đánh giá an toàn của
  log-barrier so với hard constraint.

  F9. Tham số mê cung: "tải bóc 100 tường" và "DFS sinh maze" — cần seed cụ thể, code hoặc supplementary

  Chương 5 không có link repo / commit hash; reproducibility yếu.

  ---
  G. Chương 6 — Kết luận & Hướng phát triển

  G1. Kết luận tự nhận "đề xuất và triển khai thành công OCP+DMS+log-barrier"

  Lỗi: Theo resutls.tex, không có một con số nào chứng minh OCP-DMS chạy thành công. Toàn bộ "thành công" này dựa trên thiết kế lý thuyết, không có dữ liệu.

  G2. "hàm log-barrier giải quyết triệt để vấn đề vi phạm biên"

  - "Triệt để" là phát biểu quá mạnh; log-barrier là kỹ thuật soft không đảm bảo tuyệt đối với mọi lưới rời rạc hóa.

  G3. Future plan đặt trong cùng chương "Kết luận" nhưng kế hoạch lại 15 tuần

  Một chương "Kết luận" thường tổng hợp những gì đã làm. 15 tuần kế hoạch chi tiết là dấu hiệu báo cáo thuộc dạng midterm/proposal, không phải báo cáo cuối. Nếu là báo cáo cuối, cần xóa bảng kế hoạch và
  biến thành mô tả "next step" gọn.

  ---
  H. Câu hỏi phản biện trọng tâm hội đồng nhiều khả năng sẽ hỏi

  1. "Đóng góp khoa học mới của luận văn là gì so với Marcucci 2023?" — toàn bộ GCS-Bézier formulation gần như sao chép (kể cả perspective relaxation). Đóng góp duy nhất "mới" có thể là OCP-DMS, nhưng OCP-DMS chưa có kết quả số.
  2. "Tại sao hai hướng đề xuất, mà không chứng minh được khi nào chọn cái nào?" — báo cáo nói "tùy ứng dụng" nhưng không có thí nghiệm so sánh trực tiếp 1-1 GCS-Bézier vs OCP-DMS trên cùng môi trường.
  3. "Trong Kịch bản A, manual partition có path cost cao nhất. Vậy mục đích manual là gì?" — manual được giới thiệu là "chuẩn đối sánh số vùng tối thiểu", nhưng trong bảng kết luận lại bị chê. Cần làm
  rõ vai trò.
  4. "Phương pháp xử lý $C^1$ liên tục bilinear (eq c1_bilinear) cụ thể như thế nào trong MISOCP?" — hiện báo cáo nói "trở thành tuyến tính sau khi co giãn bởi $y_e$" mà không cụ thể.
  5. "Khi $\mu_v$ giảm về 0, log-barrier tiến tới hard constraint, nhưng tại sao bài toán giới hạn lại có thể hội tụ?" — interior-point classical theory cần điều kiện strict feasibility; bạn đảm bảo điều
   kiện này như thế nào trong DMS?
  6. "Bài toán MIOCP của bạn là MINLP phi lồi → các solver thương mại không hỗ trợ. Bạn dùng solver gì cho thực nghiệm?" — setup.tex nói "Gurobi top-K + IPOPT" nhưng đó là chiến lược 2-tầng, không phải
  solver MINLP nguyên bản. Cần trình bày rõ chiến lược trong Chương 4 (mục subsec:solver_strategy được hứa nhưng không thấy trong báo cáo).
  7. "Tổng thời gian Bezier-GCS trên maze ~ 85s — quá lâu cho real-time. Bạn nói luận văn nhằm tốc độ tính toán... vậy đóng góp về tốc độ là gì?"

  ---
  I. Đề xuất ưu tiên cải thiện (theo thứ tự độ ảnh hưởng)

  ┌─────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────┐
  │  #  │                                                 Việc cần làm                                                 │  Mức độ  │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 1   │ Viết lại Tóm tắt loại bỏ kết quả không thực hiện (7-DoF, quadrotor, PRM 40%)                                 │ Critical │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 2   │ Hoàn thành thực nghiệm OCP-DMS (Kịch bản C — Dubins) hoặc loại bỏ khỏi đóng góp                              │ Critical │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 3   │ Viết contribution.tex (rỗng) liệt kê 3-5 đóng góp cụ thể                                                     │ Critical │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 4   │ Bổ sung mục subsec:solver_strategy đã được hứa trong proposed-method/main.tex                                │ High     │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 5   │ Thống nhất viết tắt OCP-DMS/OCP-MMS/GCS+DMS; sửa label \ref{sec:ocp_DMS_approach} bị sai                     │ High     │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 6   │ Khôi phục mục Dual cone trong Chương 2                                                                       │ High     │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 7   │ Xóa toàn bộ "SKELETON / Đề xuất nội dung hình" trong gcs_bezier.tex                                          │ High     │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 8   │ Thêm bảng so sánh tổng kết cuối Chương 3 (SBP/DTO/MICP/GCS-Bézier/OCP-DMS)                                   │ Medium   │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 9   │ Viết lại lập luận TU trong graph.tex cho khớp với GCS thực tế                                                │ Medium   │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 10  │ Bổ sung metric an toàn $(d_{\min}, \rho_{vio})$ vào resutls.tex                                              │ Medium   │
  ├─────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────┤
  │ 11  │ Refactor: chuyển experiments/{prb_statement, GCS, relaxation, gcs_bezier, cnx_decomp}.tex → proposed-method/ │ Low      │
  └─────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────┘

● Tóm lại: vấn đề lớn nhất là (1) Tóm tắt phóng đại kết quả, (2) OCP-DMS được trình bày là đóng góp chính nhưng chưa có thực nghiệm số, (3) các viết tắt không thống nhất + label sai, và (4) 
  gcs_bezier.tex còn nhiều SKELETON chưa điền. Các điểm này nên ưu tiên xử lý trước khi bảo vệ.
 