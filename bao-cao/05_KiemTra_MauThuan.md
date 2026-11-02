# Kết quả kiểm tra theo checklist và danh sách mâu thuẫn giữa 4 báo cáo

> Orchestrator – thực hiện PHẦN 5 và bước "tổng hợp danh sách mâu thuẫn" của PHẦN 7. Tài liệu này là đầu vào cho Agent Tổng hợp (PHẦN 6). Phần "Đề xuất xử lý" ở mục B chỉ là gợi ý; quyết định cuối cùng thuộc về Agent Tổng hợp.

Báo cáo được kiểm tra:
- `01_RBAC.md` (Agent 1)
- `02_ReBAC.md` (Agent 2)
- `03_ABAC.md` (Agent 3)
- `04_PBAC.md` (Agent 4)

---

## A. KIỂM TRA THEO CHECKLIST PHẦN 5

### A.1. Kết quả

Các mục đếm và tính được (số mục, số kịch bản, số sơ đồ, điểm có trọng số) được kiểm tra bằng script đọc trực tiếp file. Cú pháp Mermaid được kiểm tra bằng cách render thật (mermaid-cli).

| # | Tiêu chí checklist | RBAC | ReBAC | ABAC | PBAC |
|---|---|---|---|---|---|
| 1 | Đủ 9 mục, đúng thứ tự | Đạt | Đạt | Đạt | Đạt |
| 2 | Đủ 18 kịch bản, mỗi kịch bản có mức đáp ứng | Đạt (18/18 + 2 KB-THÊM) | Đạt (18/18 + 2) | Đạt (18/18 + 2) | Đạt (18/18 + 2) |
| 3 | ≥ 3 sequence diagram và 1 ERD, Mermaid hợp lệ | Đạt (3 seq, 1 ERD, 1 flowchart; render 5/5) | Đạt (4 seq, 1 ERD, 1 flowchart; render 6/6; model OpenFGA qua trình kiểm tra cú pháp chính thức) | Đạt (4 seq, 1 ERD, 1 flowchart; render 6/6) | Đạt (4 seq, 1 ERD, 1 flowchart; render 6/6) |
| 4 | Danh sách service/module cần thêm, kèm trách nhiệm | Đạt (11 thành phần) | Đạt (14 thành phần) | Đạt (11 thành phần) | Đạt (13 thành phần) |
| 5 | Mục "không làm được / làm không tốt" ≥ 3 trường hợp | Đạt (6) | Đạt (6) | Đạt (6) | Đạt (6) |
| 6 | Bảng chấm điểm đủ 8 tiêu chí và điểm tổng có trọng số | Đạt; tính lại 3,75 khớp | Đạt; tính lại 3,35 khớp | Đạt; tính lại 3,40 khớp | Đạt; tính lại 3,80 khớp |
| 7 | Giả định được đánh dấu rõ | Đạt | Đạt | **Bổ sung lần 1** → Đạt | **Bổ sung lần 1** → Đạt |
| 8 | Không mâu thuẫn rõ ràng với agent khác | Có mâu thuẫn, ghi ở mục B | Có mâu thuẫn, ghi ở mục B | Có mâu thuẫn, ghi ở mục B | Có mâu thuẫn, ghi ở mục B |

**Kết luận checklist:** cả 4 báo cáo **đạt** sau một lần bổ sung cho ABAC và PBAC. Không báo cáo nào phải chạy lại toàn bộ.

### A.2. Bổ sung đã yêu cầu và đã thực hiện

| Báo cáo | Lần | Thiếu gì | Đã bổ sung |
|---|---|---|---|
| ABAC | 1/2 | Quy tắc `PT-PLAN-VIEW` giới hạn hoa tiêu chỉ xem kế hoạch **đã duyệt**, và thuộc tính `served_branch_ids` (đơn vị hoa tiêu chỉ phục vụ một số chi nhánh), là quy tắc nghiệp vụ PHẦN 1 không nêu, nhưng không đánh dấu [GIẢ ĐỊNH]. | Thêm GĐ-A11, kèm phân tích "nếu sai thì sao". Không đổi mức đáp ứng. |
| PBAC | 1/2 | Như trên: quy tắc `view` cho `HOA_TIEU` trong `berth_plan.yaml` và `served_branch_ids` chưa đánh dấu giả định. | Thêm GĐ-P12. Không đổi mức đáp ứng. |
| PBAC (tự soát trước khi nộp) | – | Quy tắc DENY bắt buộc MFA bị bỏ qua khi thiếu thuộc tính; quy tắc tài khoản dịch vụ bị thừa; sơ đồ phát hành policy để người viết tự duyệt lần hai. | Đã sửa trong lúc viết: thêm `has()` và test cho trường hợp thiếu thuộc tính; đổi thành quy tắc DENY theo vùng mạng; tách vai trò kỹ sư bảo mật. |

### A.3. Ma trận mức đáp ứng 18 × 4 (trích từ 4 báo cáo, chưa chuẩn hóa)

| Mã | Kịch bản (rút gọn) | RBAC | ReBAC | ABAC | PBAC |
|---|---|---|---|---|---|
| KB01 | Điều độ tạo/sửa kế hoạch | Tốt | Tốt | Tốt | Tốt |
| KB02 | Không sửa kế hoạch đã duyệt | Kém | Được | Tốt | Tốt |
| KB03 | Người lập không tự duyệt | Kém | Tốt | Tốt | Tốt |
| KB04 | BGĐ xem tổng quan, phê duyệt | Tốt | Được | Tốt | Tốt |
| KB05 | Hoa tiêu từ hệ thống ngoài xem kế hoạch | Được | Được | Được | Được |
| KB06 | Màn hình hoa tiêu, không vào màn hình Điều độ | Tốt | Được | Được | Tốt |
| KB07 | Hoa tiêu chỉ thấy lượt được phân công/có yêu cầu | Kém | Tốt | Tốt | Tốt |
| KB08 | Hãng A không thấy dữ liệu hãng B | Được | Tốt | Tốt | Tốt |
| KB09 | Ẩn trường nội bộ với hãng tàu | Được | Được | Tốt | Tốt |
| KB10 | Ủy quyền đại lý theo tàu/chuyến | Kém | Tốt | Được | Được |
| KB11 | Chuyển phòng, quyền cũ mất ngay | Tốt | Tốt | Tốt | Tốt |
| KB12 | Quyền tạm thời có thời hạn | Được | Được | Tốt | Tốt |
| KB13 | Thêm màn hình, gán quyền không deploy | Tốt | Được | Được | Được |
| KB14 | Danh sách lớn, phân trang, lọc, sắp xếp | Tốt | Kém | Được | Tốt |
| KB15 | Ẩn/hiện nút, menu theo quyền | Tốt | Được | Được | Được |
| KB16 | Audit "vì sao" và "ai có quyền" | Được | Tốt | Kém | Được |
| KB17 | Tách dữ liệu theo chi nhánh | Tốt | Tốt | Tốt | Tốt |
| KB18 | Tài khoản dịch vụ quyền hạn chế | Được | Được | Tốt | Tốt |
| | **Tốt / Được / Kém / Không** | **8 / 6 / 4 / 0** | **8 / 9 / 1 / 0** | **11 / 6 / 1 / 0** | **13 / 5 / 0 / 0** |

---

## B. DANH SÁCH MÂU THUẪN CHUYỂN CHO AGENT TỔNG HỢP

Mức độ ảnh hưởng:
- **Cao**: có thể đổi xếp hạng hoặc đổi phương án đề xuất.
- **Trung bình**: đổi mức đáp ứng của một vài kịch bản.
- **Thấp**: chỉ cần thống nhất cách trình bày.

### B.1. Thang điểm và cách chấm (ảnh hưởng: **Cao**)

**M01 – Thang quy đổi "số kịch bản đạt" sang điểm 1–5 của tiêu chí 1 không thống nhất.**

| Báo cáo | Tốt / Được / Kém | Điểm tiêu chí 1 đã chấm |
|---|---|---|
| RBAC | 8 / 6 / 4 | 3 |
| ReBAC | 8 / 9 / 1 | 4 |
| ABAC | 11 / 6 / 1 | 4 |
| PBAC | 13 / 5 / 0 | 5 |

Mỗi agent tự chọn điểm nguyên theo cảm nhận, không theo công thức chung. RBAC bị chấm khắt khe: 3 điểm, trong khi tỷ lệ đạt thực tế gần với ReBAC (4 điểm). PBAC được làm tròn lên 5.

**Đề xuất xử lý:** quy đổi tuyến tính 4 mức sang thang 1–5, lấy trung bình 18 kịch bản: Tốt = 5; Được = 3,67; Kém = 2,33; Không làm được = 1.

| Mô hình | Tiêu chí 1 (đã chấm) | Tiêu chí 1 (chuẩn hóa) | Tổng (đã chấm) | Tổng (chuẩn hóa tiêu chí 1) | Tổng nếu bỏ KB10 (hiện chưa có đại lý) |
|---|---|---|---|---|---|
| RBAC | 3 | 3,96 | 3,75 | **3,99** | 4,01 |
| ReBAC | 4 | 4,19 | 3,35 | **3,40** | 3,38 |
| ABAC | 4 | 4,41 | 3,40 | **3,50** | 3,51 |
| PBAC | 5 | 4,63 | 3,80 | **3,71** | 3,72 |

**Hệ quả quan trọng:** sau khi chuẩn hóa, **RBAC vượt PBAC** (3,99 so với 3,71); thứ hạng 1–2 đảo chiều. Như vậy kết quả xếp hạng **rất nhạy** với cách chấm tiêu chí 1. Agent Tổng hợp cần:
1. công bố công thức chuẩn hóa đã dùng;
2. cân nhắc có đánh trọng số theo **mức quan trọng của kịch bản** hay không. KB02, KB03, KB07, KB08, KB13 gắn trực tiếp với "đặc điểm khó" ở PHẦN 1.3, còn KB10 hiện không có nhu cầu (PHẦN 1.4). Đánh trọng số đều cho 18 kịch bản đang có lợi cho RBAC, vì RBAC mạnh ở các kịch bản "dễ";
3. nêu rõ rằng điểm tổng có trọng số **không** phải là căn cứ duy nhất cho phương án đề xuất. Cả 4 báo cáo đều kết luận "không dùng một mình".

**M02 – Tiêu chí "Phân quyền động" chấm cùng 4 điểm cho cả 4 mô hình dù năng lực khác nhau.**
- RBAC: gán quyền là dữ liệu, nhưng quy tắc trạng thái nằm trong code nên đổi phải deploy. Theo chính báo cáo RBAC thì 3 điểm hợp lý hơn.
- ABAC: quy tắc sửa được lúc chạy trên Portal Admin, chỉ cần code khi thêm thuộc tính mới.
- PBAC: quy tắc đổi qua pipeline (không deploy ứng dụng); cũng cần code khi thêm thuộc tính mới.

Như vậy ABAC ≥ PBAC > RBAC về năng lực này.

**Đề xuất:** RBAC 3, ReBAC 4, ABAC 4, PBAC 4 (hoặc ABAC 5 nếu chấp nhận cho admin sửa quy tắc trên giao diện, xem M10).

**M03 – Tiêu chí "Hiệu năng" và KB14: ABAC và PBAC dựa trên cùng một kỹ thuật nhưng chấm khác nhau.**
- ABAC: KB14 **Được**, hiệu năng **3**. Lý do: phải tự xây bộ đánh giá từng phần và bộ dịch sang SQL.
- PBAC: KB14 **Tốt**, hiệu năng **4**. Engine có sẵn PlanResources, nhưng vẫn phải tự viết bộ chuyển cây điều kiện sang EF Core (chưa có adapter .NET chính thức). Ngoài ra mỗi quyết định thêm một bước gọi sang PDP, trong khi PDP của ABAC chạy ngay trong process.

Khác biệt có cơ sở (engine có sẵn partial evaluation) nhưng đang bị phóng đại. Báo cáo ABAC cũng đã ghi rằng nếu dùng engine thì KB14 có thể lên Tốt.

**Đề xuất:** đánh giá KB14 theo **phương án cài đặt** chứ không theo tên mô hình:
- Phương án kết hợp có engine: Tốt, nhưng chỉ khi PoC chứng minh được bộ chuyển sang EF Core trên SQL Server/Oracle; chưa có thì là Được.
- ABAC tự xây: Được.

Điểm hiệu năng của ABAC và PBAC nên chênh nhau không quá 1 và có giải thích.

**M04 – Tiêu chí "Audit" và KB16: ABAC (Kém, 2 điểm) và PBAC (Được, 3 điểm) có cùng giới hạn.**
- Cả hai đều trả lời tốt câu "vì sao" qua log quyết định, và đều không có truy vấn ngược "ai có quyền trên Y".
- Cả hai đều có dữ liệu chức năng/role trong bảng: ABAC có `user_function`, PBAC có `role_assignment`.
- Cả hai đều có lịch sử thay đổi quy tắc: ABAC có `rule_version`/`rule_change_log`, PBAC có Git.
- Báo cáo ABAC còn mô tả cụ thể cách truy vấn ngược bằng SQL (mục 5.9).

Lý do chênh lệch ("PBAC có lịch sử Git") không đủ để khác một bậc.

**Đề xuất:** cùng mức cho KB16 (gợi ý **Được**, vì chiều xuôi tốt và chiều ngược có cách làm khi quy tắc giới hạn), cùng điểm audit (gợi ý 3).

### B.2. Cùng cơ chế nhưng mức đáp ứng khác nhau (ảnh hưởng: **Trung bình**)

| Mã | Kịch bản | Mâu thuẫn | Đề xuất xử lý |
|---|---|---|---|
| **M05** | KB06 – quyền màn hình | Cơ chế của PBAC (`screen.allowed_roles` lấy từ bảng `screen_role`) gần như trùng cơ chế của ABAC (`screen.audiences`), nhưng PBAC chấm **Tốt** còn ABAC chấm **Được**. Lý do ABAC nêu (admin khó hiểu biểu thức) không áp dụng nếu ABAC cũng có bảng ánh xạ màn hình – vai trò. | Nếu phương án cuối có bảng ma trận "màn hình × role" trên Portal Admin thì chấm Tốt cho RBAC, ABAC, PBAC; ReBAC giữ Được (mô phỏng RBAC, tốn nhiều Check). |
| **M06** | KB09 – quyền mức trường | PBAC dùng action riêng cho từng nhóm trường (`view:internal_fields`), về bản chất giống **permission mức trường** của RBAC. Nhưng PBAC chấm **Tốt**, RBAC chấm **Được**. ABAC dùng **nhãn phân loại trường**: thêm trường nhạy cảm chỉ cần gắn nhãn, nên Tốt là có cơ sở. | Cùng cơ chế thì cùng mức: RBAC và PBAC (theo cách đã viết) cùng Được, hoặc cùng Tốt. Gợi ý: phương án cuối dùng nhãn phân loại trường (như ABAC) bên trong policy thì chấm Tốt. |
| **M07** | KB12 – quyền tạm thời | Cơ chế chính PBAC mô tả (dịch vụ dữ liệu chỉ trả role còn hiệu lực theo `valid_from/valid_to`) **trùng** với "RBAC có thời hạn" của RBAC. Nhưng PBAC chấm **Tốt**, RBAC chấm **Được**. RBAC hạ mức vì không diễn đạt được "chỉ ủy quyền một số kế hoạch/dưới hạn mức"; PBAC làm được điều đó nhờ điều kiện trong policy nhưng không nêu rõ. | Giữ chênh lệch nhưng ghi đúng lý do: PBAC/ABAC Tốt vì diễn đạt được ủy quyền có điều kiện (thời gian và phạm vi bản ghi), không phải vì cơ chế thời hạn. RBAC Được là hợp lý. |
| **M08** | KB13 – thêm màn hình không deploy | RBAC **Tốt**; PBAC **Được**, dù PBAC cũng có màn hình và gán quyền dạng dữ liệu (không đổi policy). PBAC tự hạ mức vì "chức năng mới cần quy tắc mới phải qua pipeline". Nhưng với RBAC, chức năng mới có quy tắc trạng thái còn phải sửa code, tệ hơn. Hai agent hiểu phạm vi kịch bản khác nhau. | Thống nhất phạm vi KB13 là "**thêm màn hình/chức năng và gán quyền**", không gồm "thêm quy tắc nghiệp vụ mới". Theo phạm vi đó: RBAC Tốt, PBAC Tốt, ABAC Tốt (màn hình là dữ liệu), ReBAC Được (có thể phải đổi model). Phần "thêm quy tắc mới" chuyển thành KB-THÊM chung (xem M13). |

### B.3. Giả định nghiệp vụ khác nhau giữa các báo cáo (ảnh hưởng: **Trung bình**)

**M09 – Phạm vi xem của hoa tiêu không thống nhất.**

| Điểm | RBAC | ReBAC | ABAC | PBAC |
|---|---|---|---|---|
| Hoa tiêu xem kế hoạch ở trạng thái nào? | Mọi trạng thái (KB05). "Chỉ xem đã duyệt" được nêu là kịch bản bổ sung KB-THÊM-02 và chấm **Kém** | Mọi trạng thái (`pilot_viewer from branch`) | **Chỉ đã duyệt** (GĐ-A11) | **Chỉ đã duyệt** (GĐ-P12) |
| Yêu cầu hoa tiêu chưa phân công hiện cho ai? | **Mọi** đơn vị hoa tiêu (truy vấn `pilot_required = 1 AND pilot_unit_id IS NULL`, không lọc chi nhánh) | Các đơn vị **phục vụ chi nhánh** đó ([GIẢ ĐỊNH]) | Các đơn vị phục vụ chi nhánh (GĐ-A11) | Các đơn vị phục vụ chi nhánh (GĐ-P12) |

Hệ quả: so sánh KB05/KB07 giữa các mô hình không cùng đề bài. RBAC trông yếu hơn ở "chỉ xem đã duyệt" vì đó là kịch bản riêng của RBAC; ReBAC chưa phải xử lý điều kiện trạng thái cho hoa tiêu.

**Đề xuất:**
1. Đưa hai câu hỏi này vào **Câu hỏi mở** (mục 10 của báo cáo Word): "Hoa tiêu có được xem kế hoạch chưa duyệt không?" và "Một đơn vị hoa tiêu có phục vụ mọi chi nhánh không?".
2. Trong báo cáo, dùng một giả định chung cho cả 4 mô hình: chỉ xem kế hoạch đã duyệt; yêu cầu chưa phân công chỉ hiện cho đơn vị phục vụ chi nhánh.
3. Với giả định chung đó, chấm lại: ReBAC KB05 vẫn Được nhưng cần thêm tuple trạng thái (cùng vấn đề đồng bộ như KB02); RBAC KB07 giữ Kém.

### B.4. Khuyến nghị thiết kế trái ngược (ảnh hưởng: **Cao** – ảnh hưởng phương án đề xuất)

**M10 – Ai được sửa quy tắc, và sửa ở đâu.**
- **ABAC**: quy tắc lưu trong DB, sửa trên **Portal Admin** (trình soạn dạng mẫu, mô phỏng, test bắt buộc), giới hạn cho admin kỹ thuật (GĐ-A09).
- **PBAC**: quy tắc lưu trong **Git**, đổi qua pull request + CI; Portal Admin **chỉ đọc** policy, chỉ sửa *dữ liệu*. Báo cáo PBAC coi việc cho sửa policy trên giao diện là rủi ro (mục 6, trường hợp 3).

Hai khuyến nghị này loại trừ nhau. Chúng ảnh hưởng trực tiếp đến cách hiểu yêu cầu "phân quyền động, không sửa code hay deploy lại" (PHẦN 1.3).

**Đề xuất:** Agent Tổng hợp chọn một và nêu lý do. Gợi ý: tách 3 lớp thay đổi.
- (1) Dữ liệu (gán role, phạm vi, màn hình, ủy quyền): Portal Admin, hiệu lực ngay.
- (2) Tham số của quy tắc (danh sách trạng thái được sửa, thời hạn sau duyệt…): dữ liệu có kiểm duyệt.
- (3) Logic quy tắc: Git + CI.

Đồng thời đưa vào câu hỏi mở: "Ai được phép thay đổi quy tắc phân quyền và chấp nhận thời gian triển khai bao lâu?".

**M11 – Tự xây bộ đánh giá quy tắc hay dùng policy engine.**
- ABAC chọn **tự xây** (thư viện .NET nhúng, System.Linq.Dynamic.Core, bộ dịch SQL); các giai đoạn GĐ3 và GĐ4 được đánh giá **Cao**.
- PBAC chọn **engine có sẵn** (Cerbos, so sánh OPA).
- Chính báo cáo ABAC đã kết luận "nên cân nhắc engine có sẵn".

Đồng thời điểm "Độ phức tạp triển khai" của hai báo cáo bằng nhau (3), dù ABAC tự xây có hai giai đoạn độ phức tạp Cao.

**Đề xuất:** coi đây không phải mâu thuẫn về kết luận mà là chênh lệch về độ phức tạp. Phương án đề xuất nên dùng engine; điểm độ phức tạp của "ABAC tự xây" có thể hạ xuống 2–3 và cần nêu rõ.

**M12 – Kiểm tra trạng thái trong domain có cần giữ hay không.**
- ReBAC: **giữ** kiểm tra trạng thái trong domain, vì tuple `approved` đồng bộ có độ trễ.
- RBAC: kiểm tra trạng thái **chỉ** trong domain.
- PBAC: mã giả ghi "không còn if phân quyền trong domain".
- ABAC: không nêu.

Không phải mâu thuẫn về sự kiện kỹ thuật (PBAC/ABAC đọc trạng thái trực tiếp nên không có độ trễ), nhưng là khác biệt về nguyên tắc phòng thủ nhiều lớp.

**Đề xuất:** phương án đề xuất nêu rõ một nguyên tắc chung. Gợi ý: quyết định phân quyền ở engine; domain vẫn giữ **bất biến nghiệp vụ** ("kế hoạch đã duyệt không thể bị sửa" là quy tắc nghiệp vụ, không chỉ là quy tắc quyền).

### B.5. Kịch bản bổ sung không đồng nhất (ảnh hưởng: **Thấp**)

**M13 – Mỗi agent tự thêm KB-THÊM khác nhau nên không so sánh được.**

| Nội dung | RBAC | ReBAC | ABAC | PBAC |
|---|---|---|---|---|
| Admin hãng tàu tự quản lý user/ủy quyền của hãng | KB-THÊM-01: Được | KB-THÊM-01: Tốt | – | – |
| Hoa tiêu chỉ xem kế hoạch đã duyệt | KB-THÊM-02: Kém | – | (trong PT-PLAN-VIEW) | (trong policy) |
| Thu hồi rồi thao tác ngay (new enemy problem) | – | KB-THÊM-02: Được | – | – |
| Phê duyệt bắt buộc MFA / mạng nội bộ | – | – | KB-THÊM-01: Tốt | KB-THÊM-02: Tốt |
| Rà soát quyền định kỳ | – | – | KB-THÊM-02: Kém | – |
| Thay đổi quy tắc an toàn (test, shadow, rollback) | – | – | – | KB-THÊM-01: Tốt |

**Đề xuất:** Agent Tổng hợp gom thành một danh sách KB-THÊM chung, chấm cho cả 4 mô hình ở phụ lục (không đưa vào ma trận 18 × 4 và không tính điểm), hoặc chỉ ghi chú định tính.

### B.6. Khẳng định kỹ thuật chưa kiểm chứng (không phải mâu thuẫn, nhưng ảnh hưởng kết luận)

Các khẳng định sau được agent nêu theo hiểu biết, đánh dấu "cần kiểm tra" trong báo cáo. Agent Tổng hợp nên giữ nguyên dấu "cần kiểm tra" và đưa các mục quan trọng vào phạm vi PoC.

| # | Khẳng định | Báo cáo | Ảnh hưởng nếu sai |
|---|---|---|---|
| K01 | OpenFGA, SpiceDB, Permify không hỗ trợ SQL Server/Oracle làm kho lưu, nên phải thêm PostgreSQL/MySQL. | ReBAC (GĐ-B10) | Nếu có hỗ trợ thì điểm độ phức tạp/vận hành của ReBAC tăng. |
| K02 | Cerbos có SDK .NET chính thức; PlanResources trả cây điều kiện; **chưa có adapter EF Core chính thức**. | PBAC | Là cơ sở chấm KB14 = Tốt. Nếu bộ chuyển sang EF Core không khả thi trên Oracle thì KB14 giảm. |
| K03 | Trong Cerbos, biểu thức lỗi (thiếu thuộc tính) làm quy tắc không kích hoạt, nên quy tắc DENY phải dùng `has()`. | PBAC | Rủi ro bảo mật; cần test trong PoC. Nguyên tắc tương tự cho bộ đánh giá tự xây của ABAC (`OP-APPROVE-MFA`): hành vi khi thiếu thuộc tính chưa được định nghĩa. |
| K04 | Độ trễ PDP dạng sidecar "dưới 1–2 ms". | PBAC | Ảnh hưởng điểm hiệu năng PBAC; phải đo ở PoC. |
| K05 | Giới hạn kết quả/thời gian của ListObjects (OpenFGA); SpiceDB hỗ trợ quan hệ hết hạn; OpenFGA bỏ qua tuple trùng khi ghi. | ReBAC | Ảnh hưởng KB10, KB14 và thiết kế đồng bộ của ReBAC. |
| K06 | OPA có SDK C# (Styra), dịch sang SQL cho SQL Server/Oracle; Cedar không có SDK .NET, partial evaluation còn thử nghiệm. | PBAC | Ảnh hưởng lựa chọn engine (Cerbos so với OPA so với Cedar). |
| K07 | EF Core (đặc biệt provider Oracle) dịch đúng `Contains`/`IN`, global query filter và biểu thức từ System.Linq.Dynamic.Core. | RBAC, ABAC, PBAC | Ảnh hưởng mọi phương án lọc danh sách bằng SQL. |
| K08 | Policy mẫu Cerbos, Rego, Cedar và bộ test **chưa được chạy thật** (môi trường không cho tải engine). Riêng model OpenFGA của ReBAC đã qua trình kiểm tra cú pháp chính thức. | PBAC, ReBAC | Có thể còn lỗi cú pháp nhỏ trong ví dụ; cần chạy lại khi PoC. |

### B.7. Những điểm 4 báo cáo **nhất quán** (Agent Tổng hợp dùng được ngay)

- Keycloak chỉ xác thực; quyền eBerth **không** đặt trong JWT. Mọi báo cáo dùng cache Redis theo version cộng sự kiện Kafka để thu hồi quyền ngay (KB11).
- Tên role/chức năng thống nhất: `DIEU_DO`, `BGD`, `HOA_TIEU`, `HANG_TAU_NHAN_VIEN`, `HANG_TAU_QUAN_TRI`, `ADMIN_HT`, `SVC_HOA_TIEU`.
- Trạng thái kế hoạch thống nhất: `NHAP`, `CHO_DUYET`, `DA_DUYET`, `TU_CHOI`. `port_call` có `branch_id`, `shipping_line_id`, `pilot_unit_id`, `pilot_required`.
- Hệ thống Hoa tiêu gọi API bằng client credentials; hoa tiêu dùng màn hình bằng tài khoản Keycloak hoặc identity brokering. Gọi thay mặt từng hoa tiêu thì cần Token Exchange.
- Hiện không có đại lý; KB10 được đánh giá cho tương lai.
- Tách dữ liệu một tầng chi nhánh; quy mô giả định khoảng 200.000 lượt cập cảng/năm.
- Phân công hoa tiêu phải được đồng bộ về eBerth, nếu không thì KB07 không mô hình nào làm được.
- **Cả 4 báo cáo đều kết luận "không dùng một mình"**. Hướng kết hợp được ủng hộ chung: RBAC (role có phạm vi) cho màn hình/chức năng + ABAC (thuộc tính bản ghi) cho phạm vi dữ liệu và trạng thái, đặt trong khung PBAC (policy engine). ReBAC chỉ thêm khi ủy quyền chéo tổ chức (đại lý) thành nhu cầu thật.

---

## C. TÓM TẮT CHO AGENT TỔNG HỢP

1. Cả 4 báo cáo đạt checklist (ABAC, PBAC đã bổ sung giả định một lần).
2. **Phải chuẩn hóa điểm tiêu chí 1 (M01)**: sau chuẩn hóa tuyến tính, RBAC 3,99 > PBAC 3,71 > ABAC 3,50 > ReBAC 3,40. Thứ hạng nhạy với cách chấm, cần công bố công thức và cân nhắc trọng số theo mức quan trọng của kịch bản.
3. Thống nhất lại các mức đáp ứng có cùng cơ chế: KB06, KB09, KB12, KB13, KB14, KB16 (M03–M08), và điểm "Phân quyền động", "Hiệu năng", "Audit" (M02–M04).
4. Dùng một giả định chung về phạm vi xem của hoa tiêu (M09) và đưa vào câu hỏi mở.
5. Chọn một mô hình quản trị quy tắc (M10) và nguyên tắc giữ bất biến nghiệp vụ trong domain (M12).
6. Giữ các khẳng định "cần kiểm tra" (K01–K08) và đưa K02, K03, K04, K07 vào phạm vi PoC.
