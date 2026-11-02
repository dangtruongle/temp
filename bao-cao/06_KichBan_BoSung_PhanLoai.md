# Kịch bản bổ sung và phân loại kịch bản

> Orchestrator – bổ sung kịch bản còn thiếu ngoài KB01–KB18 và phân loại toàn bộ thành 7 nhóm. Mức đáp ứng của kịch bản bổ sung do Orchestrator đánh giá theo cùng cách hiểu như 4 báo cáo; mục nào trùng KB-THÊM của báo cáo thì lấy theo báo cáo đó (cột Nguồn). Các kịch bản bổ sung **không** tính vào điểm 8 tiêu chí của 4 báo cáo.

## 1. Cách phân loại

| Nhóm | Tên nhóm | Câu hỏi nhóm trả lời | Kịch bản gốc | Kịch bản bổ sung |
|---|---|---|---|---|
| A | Chức năng & giao diện | Ai được làm gì, thấy màn hình, nút nào? | KB01, KB04, KB06, KB13, KB15 | KB37 |
| B | Phạm vi dữ liệu & tổ chức | Thấy bản ghi nào, trường nào, của tổ chức nào? | KB07, KB08, KB09, KB17 | KB23, KB24, KB25 |
| C | Quy tắc nghiệp vụ động | Quyền đổi theo trạng thái, người lập, thời điểm, mức rủi ro thế nào? | KB02, KB03 | KB19, KB20, KB21, KB22 |
| D | Ủy quyền & quản trị phân cấp | Ai được cấp quyền cho ai, tạm thời hay lâu dài? | KB10, KB12 | KB26, KB27, KB28 |
| E | Vòng đời & nhất quán của quyền | Thay đổi quyền có hiệu lực khi nào, ở mọi service chưa? | KB11 | KB29, KB30 |
| F | Tích hợp & kênh truy cập | Hệ thống ngoài, API, thông báo đẩy, xuất file có bị lọt quyền không? | KB05, KB18 | KB31, KB32, KB33 |
| G | Audit, hiệu năng & vận hành | Giải thích, rà soát, chịu tải, sự cố và thay đổi quy tắc ra sao? | KB14, KB16 | KB34, KB35, KB36 |

Tổng: 18 kịch bản gốc + 19 kịch bản bổ sung = 37 kịch bản.

## 2. Danh sách kịch bản bổ sung

| Mã | Nhóm | Kịch bản | Vì sao cần | Nguồn |
|---|---|---|---|---|
| KB19 | C | Kế hoạch có tàu chở hàng nguy hiểm (IMDG) chỉ Giám đốc được duyệt, Phó giám đốc không được | Tàu hàng nguy hiểm cần cấp phê duyệt cao hơn; người được ủy quyền thông thường không được duyệt. | Orchestrator |
| KB20 | C | Chỉ điều chỉnh kế hoạch trước giờ chốt (ví dụ 6 giờ trước ETA); sau giờ chốt chỉ Điều độ trưởng được sửa | Sát giờ tàu vào, đổi kế hoạch ảnh hưởng hoa tiêu, lai dắt, công nhân; cần khóa theo thời gian. | Orchestrator |
| KB21 | C | Thay đổi lớn (đổi cầu bến, lệch hơn 2 giờ) cần thêm một cấp duyệt; người duyệt cấp 2 khác người lập và người duyệt cấp 1 | Phân tách nhiệm vụ nhiều bước cho thay đổi rủi ro cao. | Orchestrator |
| KB22 | C | Phê duyệt chỉ từ mạng nội bộ và bắt buộc MFA | Giảm rủi ro tài khoản BGĐ bị chiếm dụng. | ABAC KB-THÊM-01, PBAC KB-THÊM-02 |
| KB23 | B | Hãng tàu xem lịch chiếm dụng cầu bến toàn cảng nhưng không thấy tên tàu, tên hãng khác | Hãng tàu cần biết cầu bến bận hay trống để xin giờ, nhưng không được lộ thông tin đối thủ: dòng được thấy, trường bị che tùy chủ sở hữu từng dòng. | Orchestrator |
| KB24 | B | Tàu chuyển sang hãng khác: hãng mới không thấy lịch sử cập cảng trước ngày chuyển, hãng cũ vẫn xem lịch sử của mình | Lịch sử thuộc về hãng khai thác tại thời điểm cập cảng. | Orchestrator |
| KB25 | B | Người làm cho nhiều chi nhánh chọn chi nhánh đang làm việc; quyền chỉ áp theo chi nhánh đã chọn | Tránh thao tác nhầm chi nhánh; tương ứng ràng buộc DSD (Dynamic Separation of Duty) trong một phiên. | Orchestrator |
| KB26 | D | Admin chi nhánh chỉ quản lý user chi nhánh mình; admin hãng tàu tự quản lý user của hãng | Giảm tải Admin cảng mà không mở quyền chéo tổ chức. | RBAC KB-THÊM-01, ReBAC KB-THÊM-01 |
| KB27 | D | Không ai tự cấp hoặc cấp cho người khác quyền cao hơn quyền mình đang có | Chống leo thang quyền qua chính chức năng quản trị. | Orchestrator |
| KB28 | D | Truy cập khẩn cấp (break-glass): sự cố ban đêm, Điều độ trực được mở quyền sửa kế hoạch chi nhánh khác trong 2 giờ, bắt buộc nêu lý do, có hậu kiểm | Sự cố hàng hải không chờ được quy trình cấp quyền thông thường. | Orchestrator |
| KB29 | E | Khóa tài khoản trên Keycloak hoặc tổ chức ngừng hợp tác: mọi user, kể cả token còn hạn, mất quyền ngay | JWT còn hạn vẫn hợp lệ về chữ ký; eBerth phải tự chặn. Mọi mô hình đều cần nhận sự kiện quản trị từ Keycloak. | Orchestrator |
| KB30 | E | Thu hồi rồi thao tác ngay (new enemy problem): quyền vừa thu hồi không được dùng ở request kế tiếp trên service khác | Microservices có cache riêng; thu hồi phải đến mọi nơi trước thao tác kế tiếp. | ReBAC KB-THÊM-02 |
| KB31 | F | Thông báo đẩy realtime: hãng tàu chỉ nhận sự kiện về tàu của mình, hoa tiêu chỉ nhận lượt được phân công | Kafka → WebSocket là kênh rò rỉ dữ liệu hay bị quên kiểm tra quyền. | Orchestrator |
| KB32 | F | Xuất Excel, báo cáo: chỉ xuất dữ liệu trong phạm vi, che trường nội bộ, giới hạn số dòng | Đường xuất file và báo cáo thường viết SQL riêng nên dễ bỏ sót lọc. | Orchestrator |
| KB33 | F | Hỗ trợ “xem như người dùng X”: chỉ đọc, ghi log cả người hỗ trợ và người được xem | Bộ phận hỗ trợ cần tái hiện lỗi “tôi không thấy lượt cập cảng” mà không mượn mật khẩu. | Orchestrator |
| KB34 | G | Rà soát quyền định kỳ: mỗi quý trưởng bộ phận xác nhận quyền của nhân viên; không xác nhận thì quyền tự hết hạn | Yêu cầu kiểm toán và nguyên tắc quyền tối thiểu. | ABAC KB-THÊM-02 |
| KB35 | G | Dịch vụ phân quyền gặp sự cố hoặc quá tải giờ cao điểm: từ chối an toàn, thao tác đọc thiết yếu vẫn chạy | Cảng vận hành 24/7; lỗi phân quyền không được mở quyền và không nên làm dừng toàn bộ hệ thống. | Orchestrator |
| KB36 | G | Thay đổi quy tắc phân quyền an toàn: kiểm thử, chạy thử song song, rollback | Một quy tắc sai có thể mở quyền diện rộng ngay khi phát hành. | PBAC KB-THÊM-01 |
| KB37 | A | Thêm module mới (ví dụ module Lai dắt) dùng chung hệ thống phân quyền mà không sửa lõi | eBerth có nhiều module và sẽ còn thêm; phân quyền phải mở rộng theo cấu hình. | Orchestrator |

## 3. Mức đáp ứng

| Mã | RBAC | ReBAC | ABAC | PBAC | Đề xuất |
|---|---|---|---|---|---|
| KB19 | Kém | Được | Tốt | Tốt | Tốt |
| KB20 | Kém | Được | Tốt | Tốt | Tốt |
| KB21 | Kém | Được | Được | Được | Được |
| KB22 | Kém | Được | Tốt | Tốt | Tốt |
| KB23 | Kém | Được | Tốt | Tốt | Tốt |
| KB24 | Tốt | Được | Tốt | Tốt | Tốt |
| KB25 | Tốt | Được | Tốt | Tốt | Tốt |
| KB26 | Được | Tốt | Được | Được | Được |
| KB27 | Được | Được | Được | Tốt | Được |
| KB28 | Được | Được | Tốt | Tốt | Tốt |
| KB29 | Được | Được | Tốt | Tốt | Tốt |
| KB30 | Được | Được | Được | Được | Được |
| KB31 | Tốt | Kém | Tốt | Được | Tốt |
| KB32 | Được | Kém | Được | Tốt | Tốt |
| KB33 | Được | Được | Tốt | Tốt | Tốt |
| KB34 | Tốt | Được | Kém | Được | Tốt |
| KB35 | Tốt | Kém | Tốt | Được | Được |
| KB36 | Được | Được | Tốt | Tốt | Được |
| KB37 | Tốt | Được | Được | Được | Tốt |

## 4. Cách xử lý theo từng mô hình

### KB19 – Kế hoạch có tàu chở hàng nguy hiểm (IMDG) chỉ Giám đốc được duyệt, Phó giám đốc không được

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Kém | Tách permission `approve-dangerous` chỉ gán cho Giám đốc, nhưng code phải tự chọn kiểm tra permission nào theo cờ hàng nguy hiểm. | RBAC không đọc được cờ hàng nguy hiểm của bản ghi; chọn permission theo cờ là code viết cứng, không cấu hình được và không hiện trong audit. |
| ReBAC | Được | Đồng bộ cờ hàng nguy hiểm thành tuple; duyệt thường `but not` kế hoạch nguy hiểm, cộng nhánh riêng cho Giám đốc. Có độ trễ đồng bộ. | Diễn đạt được bằng quan hệ loại trừ, nhưng cờ phải đồng bộ thành tuple nên có khoảng trễ quyền sai. |
| ABAC | Tốt | Thêm điều kiện `resource.dangerous_goods == false OR subject.position == GIAM_DOC` vào quy tắc duyệt. | Thuộc tính bản ghi và chức danh người dùng có sẵn trong quy tắc; đổi chính sách chỉ cần sửa quy tắc. |
| PBAC | Tốt | Điều kiện `!R.attr.dangerous_goods \|\| "GIAM_DOC" in P.roles` trong `berth_plan.yaml`. | Một điều kiện trong policy, có test, phát hành không cần deploy ứng dụng. |
| Đề xuất | Tốt | Điều kiện thu hẹp `REQUIRE_ROLE_IF(dangerous_goods, GIAM_DOC)` gắn với hành động duyệt. | Điều kiện chỉ thu hẹp (thêm yêu cầu GIAM_DOC) nên đúng mẫu của mô hình; cờ đọc trực tiếp từ bản ghi. |

### KB20 – Chỉ điều chỉnh kế hoạch trước giờ chốt (ví dụ 6 giờ trước ETA); sau giờ chốt chỉ Điều độ trưởng được sửa

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Kém | So sánh giờ hiện tại với ETA phải viết trong code; RBAC chỉ phân biệt được Điều độ và Điều độ trưởng. | Không có khái niệm thời gian so với dữ liệu của bản ghi; phải viết trong code. |
| ReBAC | Được | Điều kiện CEL với ETA và giờ hiện tại do ứng dụng truyền vào; bản chất là thuộc tính, không phải quan hệ. | Làm được bằng điều kiện CEL, nhưng bản chất là thuộc tính nên ReBAC không có lợi thế; ứng dụng phải tự truyền ETA. |
| ABAC | Tốt | `env.now < resource.eta - 6h OR has_function(subject, DIEU_DO_TRUONG, resource.branch_id)`. | Thời gian và thuộc tính bản ghi là đầu vào tự nhiên của quy tắc. |
| PBAC | Tốt | Điều kiện CEL dùng `now()` và `timestamp(R.attr.eta)`. | CEL có now() và đọc được ETA từ resource; viết một lần trong policy. |
| Đề xuất | Tốt | Điều kiện `BEFORE_CUTOFF(eta, 6h)` cho Điều độ; vai trò Điều độ trưởng có hành động sửa không kèm điều kiện này. | BEFORE_CUTOFF là điều kiện thu hẹp có tham số; ngoại lệ cho Điều độ trưởng là một hành động khác không kèm điều kiện. |

### KB21 – Thay đổi lớn (đổi cầu bến, lệch hơn 2 giờ) cần thêm một cấp duyệt; người duyệt cấp 2 khác người lập và người duyệt cấp 1

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Kém | Luồng duyệt nhiều cấp viết trong code; RBAC chỉ cấp được permission cho từng cấp. | Phải tự xây quy trình duyệt nhiều cấp và kiểm tra người duyệt trong code. |
| ReBAC | Được | Ghi tuple `approver_l1` sau bước 1; `can_approve_l2 = director but not approver_l1 but not creator`. Vẫn cần thành phần quy trình. | Phân tách người duyệt diễn đạt được bằng quan hệ, nhưng vẫn cần workflow và đồng bộ tuple sau mỗi bước. |
| ABAC | Được | Quy tắc theo `resource.approval_stage`, `resource.l1_approver != subject.id`; quy trình nhiều bước do workflow quản lý. | Điều kiện theo bước duyệt viết được, nhưng quy trình nhiều bước phải do workflow quản lý. |
| PBAC | Được | Như ABAC, viết trong policy; workflow nằm ngoài engine. | Như ABAC: policy kiểm tra từng bước, workflow nằm ngoài engine. |
| Đề xuất | Được | Hai hành động `approve_l1`, `approve_l2` với điều kiện `NOT_CREATOR`, `NOT_L1_APPROVER`; workflow nằm ngoài. | Điều kiện chặn đúng người ở mỗi bước; quy trình nhiều cấp vẫn cần workflow riêng như mọi mô hình. |

### KB22 – Phê duyệt chỉ từ mạng nội bộ và bắt buộc MFA

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Kém | Không biểu diễn được ngữ cảnh; phải chặn ở gateway hoặc trong code. | RBAC không có thuộc tính ngữ cảnh (mức xác thực, vùng mạng). |
| ReBAC | Được | Điều kiện CEL với mức xác thực và vùng mạng truyền trong context. | Điều kiện CEL làm được, nhưng ngữ cảnh phải truyền vào mỗi lần Check. |
| ABAC | Tốt | Quy tắc Deny `OP-APPROVE-MFA` theo `env.auth_level`, `env.network_zone` (theo báo cáo ABAC). | Thuộc tính ngữ cảnh là một nhóm thuộc tính chuẩn của ABAC (theo báo cáo ABAC). |
| PBAC | Tốt | Quy tắc DENY khi thiếu hoặc khác `mfa` (theo báo cáo PBAC). | Một quy tắc DENY áp cho mọi vai trò (theo báo cáo PBAC). |
| Đề xuất | Tốt | Điều kiện `MFA`, `NETWORK_ZONE` có sẵn trong danh mục điều kiện. | MFA, NETWORK_ZONE là điều kiện có sẵn trong danh mục và chỉ thu hẹp. |

### KB23 – Hãng tàu xem lịch chiếm dụng cầu bến toàn cảng nhưng không thấy tên tàu, tên hãng khác

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Kém | Quyền mức trường gắn theo vai trò, không theo chủ sở hữu từng dòng; phải viết code che trường. | Quyền mức trường của RBAC gắn với vai trò, không phân biệt dòng nào của hãng mình; che theo chủ sở hữu phải viết code. |
| ReBAC | Được | BatchCheck `can_view_detail` từng dòng trong cửa sổ vài ngày; dòng không có quyền chỉ trả trạng thái bận/trống. | Kiểm tra được theo từng dòng, nhưng phải gọi BatchCheck cho mọi dòng trong cửa sổ lịch. |
| ABAC | Tốt | Nghĩa vụ che trường khi `resource.shipping_line_id != subject.org_id`; dịch được thành `CASE WHEN` trong SQL. | Nghĩa vụ che trường theo thuộc tính của dòng, dịch được thành CASE WHEN trong SQL. |
| PBAC | Tốt | Action `view:detail` có điều kiện chủ sở hữu, kiểm tra gom lô cho cửa sổ lịch. | Action mức trường có điều kiện chủ sở hữu; cửa sổ lịch nhỏ nên kiểm tra gom lô được. |
| Đề xuất | Tốt | Vai trò hãng tàu có `view` lịch chiếm dụng toàn cảng; hành động `view:detail` kèm điều kiện `OWN_ORG`. | view:detail kèm điều kiện OWN_ORG trên cột của bản ghi, đúng bất biến 3. |

### KB24 – Tàu chuyển sang hãng khác: hãng mới không thấy lịch sử cập cảng trước ngày chuyển, hãng cũ vẫn xem lịch sử của mình

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Tốt | Phạm vi áp lên `port_call.shipping_line_id` lưu tại thời điểm cập cảng nên lịch sử tự đúng [GIẢ ĐỊNH]. | Phạm vi lọc theo cột lưu trên chính lượt cập cảng nên lịch sử tự đúng, với giả định cột được lưu tại thời điểm cập cảng. |
| ReBAC | Được | Đường suy diễn mặc định `port_call → vessel → owner_line` dùng chủ hiện tại nên hãng mới thấy cả lịch sử; phải thêm tuple `port_call#shipping_line` cho từng lượt. | Đường suy diễn mặc định qua chủ hiện tại của tàu cho kết quả sai; phải thiết kế thêm tuple theo từng lượt. |
| ABAC | Tốt | So `resource.shipping_line_id` của lượt cập cảng với `subject.org_id`. | So thuộc tính của lượt cập cảng, không đi qua chủ hiện tại của tàu. |
| PBAC | Tốt | Như ABAC, điều kiện trên thuộc tính của lượt cập cảng. | Như ABAC, điều kiện trên thuộc tính của lượt cập cảng. |
| Đề xuất | Tốt | Như RBAC: phạm vi theo cột lưu trên lượt cập cảng. | Như RBAC: phạm vi theo cột trên lượt cập cảng. |

### KB25 – Người làm cho nhiều chi nhánh chọn chi nhánh đang làm việc; quyền chỉ áp theo chi nhánh đã chọn

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Tốt | Khái niệm phiên và kích hoạt vai trò có sẵn trong chuẩn NIST; lọc lần gán theo chi nhánh đang chọn. | Chuẩn RBAC có khái niệm phiên và kích hoạt vai trò, đúng với nhu cầu chọn chi nhánh làm việc. |
| ReBAC | Được | Không có khái niệm phiên; ứng dụng truyền chi nhánh đang chọn như contextual tuple hoặc lọc kết quả. | Không có khái niệm phiên; ứng dụng phải tự truyền chi nhánh đang chọn. |
| ABAC | Tốt | Thuộc tính ngữ cảnh `env.active_branch` trong quy tắc. | Chi nhánh đang chọn là một thuộc tính ngữ cảnh. |
| PBAC | Tốt | Thuộc tính ngữ cảnh truyền vào principal hoặc aux data. | Truyền chi nhánh đang chọn vào principal; policy không đổi. |
| Đề xuất | Tốt | Chỉ dùng các lần gán có phạm vi trùng chi nhánh đang chọn. | Chỉ dùng các lần gán có phạm vi trùng chi nhánh đang chọn, vẫn là bước 1 của mô hình. |

### KB26 – Admin chi nhánh chỉ quản lý user chi nhánh mình; admin hãng tàu tự quản lý user của hãng

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | `eliner:org-user:manage` + bảng `role_grantable`, chỉ trong phạm vi ORG của mình (theo báo cáo RBAC). | Làm được với role_grantable, nhưng phải tự xây phần kiểm soát phạm vi quản trị (theo báo cáo RBAC). |
| ReBAC | Tốt | Admin tổ chức ghi tuple `member`, `agent` trong tổ chức mình qua Facade (theo báo cáo ReBAC). | Ủy quyền quản trị phân tán là thế mạnh: admin tổ chức tự ghi quan hệ trong tổ chức mình (theo báo cáo ReBAC). |
| ABAC | Được | Quy tắc cho hành động quản trị: `subject.org_id == target.org_id` và chức năng admin. | Viết được bằng quy tắc, nhưng quản trị quy tắc vẫn tập trung ở admin kỹ thuật. |
| PBAC | Được | Policy cho tài nguyên `admin_user`, `role_assignment` với điều kiện cùng tổ chức, cùng chi nhánh. | Policy quản trị viết được; đối tượng quản trị phải có cột tổ chức, chi nhánh. |
| Đề xuất | Được | `role_grantable` + phạm vi của lần gán quản trị. | Cơ chế giống RBAC (role_grantable + phạm vi), chưa hơn RBAC ở điểm này. |

### KB27 – Không ai tự cấp hoặc cấp cho người khác quyền cao hơn quyền mình đang có

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | `role_grantable` và kiểm tra phạm vi cấp nằm trong phạm vi của người cấp; phải tự viết. | Phải tự viết kiểm tra “phạm vi cấp nằm trong phạm vi người cấp”. |
| ReBAC | Được | Engine không biết ai được ghi tuple nào; Facade phải kiểm tra. | OpenFGA không kiểm soát ai được ghi tuple; toàn bộ phải làm ở Facade. |
| ABAC | Được | Quy tắc cho hành động gán thuộc tính; người sửa quy tắc vẫn là quyền cao nhất, cần phê duyệt 2 người. | Kiểm soát được việc gán thuộc tính, nhưng quyền sửa quy tắc là điểm leo thang lớn nhất, chỉ chặn được bằng quy trình. |
| PBAC | Tốt | Thay đổi policy bắt buộc review 2 người; cấp dữ liệu đi qua policy quản trị với điều kiện tập con. | Quy tắc chỉ đổi qua pull request có 2 người duyệt; cấp dữ liệu đi qua policy quản trị. |
| Đề xuất | Được | Kiểm tra khi ghi lần gán: vai trò nằm trong `role_grantable`, phạm vi là tập con phạm vi người cấp. | Kiểm tra tập con phạm vi khi ghi lần gán như RBAC; danh mục điều kiện vẫn cần quy trình duyệt riêng. |

### KB28 – Truy cập khẩn cấp (break-glass): sự cố ban đêm, Điều độ trực được mở quyền sửa kế hoạch chi nhánh khác trong 2 giờ, bắt buộc nêu lý do, có hậu kiểm

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | Lần gán tạm 2 giờ kích hoạt qua luồng riêng; không kiểm tra được điều kiện “đang trực”. | Có lần gán tạm có thời hạn, nhưng không diễn đạt được điều kiện “đang trực” và “có lý do”. |
| ReBAC | Được | Tuple có điều kiện thời hạn, ghi qua Facade kèm lý do; hậu kiểm từ changelog. | Tuple có điều kiện thời hạn làm được; điều kiện trực ca phải truyền từ ngoài vào. |
| ABAC | Tốt | Quy tắc theo `subject.on_duty`, `env.emergency`, tự hết hạn theo thời gian, nghĩa vụ ghi lý do. | Trạng thái trực, chế độ khẩn cấp và thời hạn đều là thuộc tính; nghĩa vụ ghi lý do có sẵn. |
| PBAC | Tốt | Dữ liệu cấp khẩn cấp có hạn + quy tắc riêng; decision log phục vụ hậu kiểm. | Dữ liệu cấp khẩn cấp + một quy tắc riêng; decision log phục vụ hậu kiểm. |
| Đề xuất | Tốt | Lần gán “đủ điều kiện” do người dùng tự kích hoạt trong 2 giờ, kèm điều kiện `ON_DUTY` và bắt buộc lý do. | Lần gán “đủ điều kiện” tự kích hoạt có hạn, kèm điều kiện ON_DUTY và bắt buộc lý do, vẫn đúng mẫu quy tắc. |

### KB29 – Khóa tài khoản trên Keycloak hoặc tổ chức ngừng hợp tác: mọi user, kể cả token còn hạn, mất quyền ngay

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | Kiểm tra trạng thái user và tổ chức nằm ngoài mô hình (sự kiện Keycloak → Kafka → xóa cache). | Trạng thái tài khoản và tổ chức nằm ngoài mô hình; phải thêm kiểm tra riêng. |
| ReBAC | Được | Thêm quan hệ “bị đình chỉ” và `but not suspended` vào mọi quyền, hoặc xóa hàng loạt tuple thành viên. | Phải thêm quan hệ đình chỉ vào mọi quyền, hoặc xóa hàng loạt tuple thành viên. |
| ABAC | Tốt | Một quy tắc Deny toàn cục theo `subject.employment_status` và trạng thái tổ chức. | Một quy tắc Deny toàn cục theo thuộc tính trạng thái là đủ. |
| PBAC | Tốt | Một quy tắc DENY toàn cục theo thuộc tính trạng thái. | Một quy tắc DENY toàn cục là đủ. |
| Đề xuất | Tốt | Điều kiện toàn cục `USER_ACTIVE`, `ORG_ACTIVE` thu hẹp mọi lần gán. | Điều kiện toàn cục USER_ACTIVE, ORG_ACTIVE thu hẹp mọi lần gán. |

### KB30 – Thu hồi rồi thao tác ngay (new enemy problem): quyền vừa thu hồi không được dùng ở request kế tiếp trên service khác

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | Version trong key cache + Kafka; giữa các service có khoảng trễ ngắn trừ khi đọc version từ Redis mỗi request. | Thu hồi lan tới các service qua Kafka nên có khoảng trễ ngắn; muốn chắc chắn phải đọc version từ Redis mỗi request. |
| ReBAC | Được | ZedToken hoặc mức nhất quán cao, đổi lại độ trễ (theo báo cáo ReBAC). | Có cơ chế nhất quán (ZedToken, mức nhất quán cao) nhưng đổi lại độ trễ (theo báo cáo ReBAC). |
| ABAC | Được | Như RBAC, áp cho thuộc tính chủ thể. | Cùng vấn đề cache phân tán như RBAC. |
| PBAC | Được | Như RBAC, áp cho cache principal. | Cùng vấn đề cache principal như RBAC. |
| Đề xuất | Được | Như RBAC; thao tác nhạy cảm đọc version mới nhất từ Redis. | Cùng vấn đề cache như RBAC; thao tác nhạy cảm đọc version mới nhất. |

### KB31 – Thông báo đẩy realtime: hãng tàu chỉ nhận sự kiện về tàu của mình, hoa tiêu chỉ nhận lượt được phân công

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Tốt | Sự kiện mang cột phạm vi (`shipping_line_id`, `branch_id`); lọc theo tập phạm vi của người nhận ngay trong bộ nhớ. | Lọc bằng so khớp phạm vi trong bộ nhớ, rất rẻ. |
| ReBAC | Kém | Mỗi sự kiện phải Check cho từng người đăng ký (nhân theo số kết nối), hoặc ListObjects trước rồi giữ đồng bộ. | Số lần Check nhân theo sự kiện × số kết nối; giữ danh sách đã nạp đồng bộ là bài toán khó. |
| ABAC | Tốt | Đánh giá từng phần tạo vị từ lọc cho mỗi kết nối, áp lên thuộc tính của sự kiện. | Bộ lọc từ đánh giá từng phần áp được lên sự kiện ngay trong bộ nhớ. |
| PBAC | Được | Gọi PDP cho từng sự kiện × người nhận, hoặc tự đánh giá cây điều kiện của PlanResources trên sự kiện. | Gọi PDP theo từng sự kiện tốn tài nguyên; tự đánh giá cây điều kiện thì phải viết thêm. |
| Đề xuất | Tốt | Tập phạm vi của người nhận + điều kiện chỉ dùng thuộc tính có trên sự kiện. | Phạm vi + điều kiện trên thuộc tính sự kiện, so khớp trong bộ nhớ. |

### KB32 – Xuất Excel, báo cáo: chỉ xuất dữ liệu trong phạm vi, che trường nội bộ, giới hạn số dòng

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | Dùng chung global query filter; che trường theo permission mức trường. | Lọc phạm vi dùng chung, nhưng che trường theo vai trò kém linh hoạt và báo cáo viết SQL riêng dễ bỏ sót. |
| ReBAC | Kém | Xuất lớn quay lại bài toán liệt kê của KB14. | Xuất lớn là bài toán liệt kê, điểm yếu nhất của ReBAC. |
| ABAC | Được | Dùng `BuildFilter` và nghĩa vụ che trường nếu quy tắc dịch được ra SQL. | Đúng nếu mọi quy tắc dịch được ra SQL; nếu không phải lọc sau. |
| PBAC | Tốt | PlanResources + action mức trường như danh sách (cùng lưu ý PoC của KB14). | Dùng chung đường danh sách (PlanResources) và action mức trường; cùng lưu ý PoC của KB14. |
| Đề xuất | Tốt | Phạm vi thành `WHERE`, nhãn trường để che; giới hạn số dòng là tham số điều kiện. | Một WHERE có chỉ mục và nhãn trường như danh sách thường. |

### KB33 – Hỗ trợ “xem như người dùng X”: chỉ đọc, ghi log cả người hỗ trợ và người được xem

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | Đánh giá bằng lần gán của X, thư viện chặn thao tác ghi; mô hình không có khái niệm này. | Làm được trong thư viện, nhưng mô hình không có khái niệm “xem như”. |
| ReBAC | Được | Check với subject là X; Facade chặn ghi. | Check với subject là X; chặn ghi phải làm ở Facade. |
| ABAC | Tốt | Chủ thể là X kèm `env.impersonated_by`; quy tắc Deny mọi hành động ghi. | Người hỗ trợ là một thuộc tính ngữ cảnh; một quy tắc Deny chặn mọi hành động ghi. |
| PBAC | Tốt | Principal là X kèm thuộc tính người hỗ trợ; quy tắc DENY hành động không phải đọc. | Thuộc tính người hỗ trợ trong principal + quy tắc DENY. |
| Đề xuất | Tốt | Dùng lần gán của X cộng điều kiện `READ_ONLY_IMPERSONATION`. | Điều kiện READ_ONLY_IMPERSONATION thu hẹp quyền của X. |

### KB34 – Rà soát quyền định kỳ: mỗi quý trưởng bộ phận xác nhận quyền của nhân viên; không xác nhận thì quyền tự hết hạn

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Tốt | Danh sách lần gán theo người là toàn bộ quyền; xác nhận thì gia hạn `valid_to`. | Danh sách lần gán theo người chính là toàn bộ quyền; xác nhận là gia hạn valid_to. |
| ReBAC | Được | Tuple trực tiếp của user đọc được; quyền suy diễn qua nhóm phải ListObjects nhiều loại. | Quan hệ trực tiếp đọc được, quyền suy diễn qua nhóm phải liệt kê theo nhiều loại đối tượng. |
| ABAC | Kém | Quyền là kết quả tính toán; báo cáo rà soát phải mô phỏng (theo báo cáo ABAC). | Quyền là kết quả tính toán nên báo cáo rà soát phải mô phỏng (theo báo cáo ABAC). |
| PBAC | Được | Phần vai trò rà soát được; phần quyền sinh từ điều kiện thuộc tính khó liệt kê. | Phần vai trò rà soát được; phần quyền sinh từ điều kiện thuộc tính khó liệt kê. |
| Đề xuất | Tốt | Bất biến 1: rà soát lần gán là rà soát toàn bộ quyền. | Bất biến 1: rà soát lần gán là rà soát toàn bộ quyền. |

### KB35 – Dịch vụ phân quyền gặp sự cố hoặc quá tải giờ cao điểm: từ chối an toàn, thao tác đọc thiết yếu vẫn chạy

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Tốt | Quyết định trong process từ cache; Authz Service lỗi thì quyền đã cache vẫn dùng tới TTL, đăng nhập mới bị từ chối. | Quyết định ngay trong process từ cache, không phụ thuộc dịch vụ ngoài lúc chạy. |
| ReBAC | Kém | Mọi Check cần engine qua mạng; engine hoặc PostgreSQL lỗi thì toàn hệ thống bị chặn. | Mọi quyết định cần engine qua mạng; engine lỗi thì cả hệ thống bị chặn. |
| ABAC | Tốt | PDP nhúng; thuộc tính tài nguyên đọc từ DB của module; Attribute Service lỗi thì dùng cache. | PDP nhúng, thuộc tính tài nguyên lấy từ DB của module. |
| PBAC | Được | Sidecar cô lập lỗi theo từng pod và khởi động nhanh, nhưng thêm một thành phần phải sống cùng mỗi service. | Sidecar cô lập lỗi tốt nhưng là thêm một thành phần phải chạy cạnh mỗi service. |
| Đề xuất | Được | Tốt nếu cài bằng thư viện nội bộ; như PBAC nếu dùng engine. | Tốt nếu cài bằng thư viện nội bộ, như PBAC nếu dùng engine; chấm Được để thận trọng. |

### KB36 – Thay đổi quy tắc phân quyền an toàn: kiểm thử, chạy thử song song, rollback

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Được | Quy tắc trong code đi qua CI; thay đổi ma trận vai trò trên Portal không có test và rollback sẵn. | Quy tắc trong code có CI, nhưng thay đổi ma trận vai trò không có test và rollback sẵn. |
| ReBAC | Được | Model có phiên bản và assertion; thay đổi tuple không rollback được theo lô. | Model có phiên bản và test, còn thay đổi tuple không rollback được theo lô. |
| ABAC | Tốt | `rule_set_release`, ca kiểm thử bắt buộc, mô phỏng trên log, rollback theo phát hành. | Thiết kế có phát hành quy tắc, test bắt buộc, mô phỏng và rollback. |
| PBAC | Tốt | Git, CI, shadow diff, rollback theo tag (theo báo cáo PBAC). | Giá trị cốt lõi của PBAC: Git, CI, shadow diff, rollback (theo báo cáo PBAC). |
| Đề xuất | Được | Danh mục điều kiện đi qua CI; tham số điều kiện là dữ liệu cần có phiên bản và phê duyệt. | Danh mục điều kiện đi qua CI, nhưng tham số điều kiện là dữ liệu cần thêm cơ chế phiên bản và phê duyệt. |

### KB37 – Thêm module mới (ví dụ module Lai dắt) dùng chung hệ thống phân quyền mà không sửa lõi

| Mô hình | Mức | Cách xử lý | Vì sao được mức này |
|---|---|---|---|
| RBAC | Tốt | Module tự đăng ký danh mục permission lúc khởi động; admin gán vào vai trò. | Module mới chỉ đăng ký permission; gán vào vai trò là dữ liệu. |
| ReBAC | Được | Phải thêm kiểu đối tượng, quan hệ vào model (phiên bản mới) và luồng đồng bộ cho dữ liệu của module. | Phải thêm kiểu đối tượng vào model và luồng đồng bộ dữ liệu của module mới. |
| ABAC | Được | Khai báo loại tài nguyên, ánh xạ thuộc tính, viết quy tắc mới. | Phải khai báo thuộc tính và viết quy tắc mới cho loại tài nguyên mới. |
| PBAC | Được | Thêm resource policy và test qua pull request; không sửa engine. | Phải viết policy và test mới qua pull request. |
| Đề xuất | Tốt | Đăng ký loại tài nguyên và hành động, gán vào vai trò, chọn điều kiện có sẵn trong danh mục. | Đăng ký tài nguyên, hành động rồi chọn điều kiện có sẵn; không viết quy tắc mới nếu điều kiện đã có trong danh mục. |

## 5. Giả định mới

- [GIẢ ĐỊNH] Kế hoạch và lượt cập cảng có cờ `dangerous_goods` (hàng nguy hiểm IMDG) và cột `eta` (KB19, KB20).
- [GIẢ ĐỊNH] Ban giám đốc phân biệt chức danh Giám đốc và Phó giám đốc (KB19).
- [GIẢ ĐỊNH] `port_call.shipping_line_id` lưu hãng khai thác tại thời điểm cập cảng, không suy ra từ chủ hiện tại của tàu (KB24).
- [GIẢ ĐỊNH] Có lịch trực ca để xác định “đang trực” (KB28) và có bộ phận hỗ trợ người dùng (KB33).
- [GIẢ ĐỊNH] Có kênh thông báo realtime từ Kafka tới Angular, ví dụ WebSocket/SignalR (KB31).
- [GIẢ ĐỊNH] Keycloak phát sự kiện quản trị (khóa user) để eBerth nhận qua Kafka (KB29).

## 6. Nhận xét theo nhóm

- **Quy tắc nghiệp vụ động (C):** ABAC, PBAC và mô hình đề xuất xử lý tự nhiên mọi kịch bản; RBAC yếu nhất vì quy tắc phải nằm trong code. KB21 (duyệt nhiều cấp) cần thành phần workflow ở **mọi** mô hình.
- **Phạm vi dữ liệu (B):** KB24 cho thấy ReBAC dễ sai nếu suy quyền qua chủ hiện tại của tàu; các mô hình lọc theo cột lưu trên bản ghi đúng tự nhiên.
- **Ủy quyền & quản trị (D):** chống leo thang quyền (KB27) phải tự thiết kế ở mọi mô hình; chỉ PBAC có sẵn lớp review 2 người cho thay đổi quy tắc.
- **Vòng đời (E):** KB29, KB30 là vấn đề chung: không mô hình nào tự giải quyết token còn hạn và cache phân tán, cần nhận sự kiện Keycloak và version trong cache.
- **Kênh truy cập (F):** thông báo đẩy và xuất file là hai đường hay lọt quyền; ReBAC yếu ở cả hai vì phải Check theo từng đối tượng.
- **Vận hành (G):** mô hình quyết định ngay trong process (RBAC, ABAC) chịu sự cố tốt nhất; ReBAC phụ thuộc engine qua mạng cho mọi quyết định.
