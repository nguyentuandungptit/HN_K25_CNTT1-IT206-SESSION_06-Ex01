# Viết Acceptance Criteria và Definition of Done cho User Story

- Môn học: IT206 (Quản lý dự án với Agile/Scrum)
- Bài học: Session 06 - Thực hành: Acceptance Criteria, Prioritized Product Backlog
- Dự án: Hệ thống mượn sách thư viện (độc giả đặt mượn online, nhận sách giấy tại quầy trong 48 giờ)

---

## 1. Giả định nghiệp vụ

Để các tiêu chí chấp nhận cụ thể và có thể kiểm tra được, nhóm thống nhất một số quy ước trước khi làm việc với Product Owner:

1. Hạn mức mượn: Mỗi độc giả được giữ tối đa 3 cuốn sách cùng lúc, tính cả sách đang cầm về nhà và sách đang đặt giữ chỗ ở quầy.
2. Thời gian giữ sách: Thư viện giữ sách tại quầy trong 48 giờ kể từ lúc đặt thành công hoặc lúc sách đặt trước về kho.
3. Kênh thông báo: Hệ thống gửi thông báo qua tài khoản cá nhân trên web và email tự động.
4. Trạng thái tài khoản: Độc giả phải có thẻ thư viện còn hiệu lực, không bị khóa do giữ sách quá hạn hoặc nợ tiền phạt.

---

## 2. Bảng tiêu chí chấp nhận (Acceptance Criteria)

Các kịch bản nghiệm thu được viết theo định dạng Given - When - Then cho từng story:

| Story | Mã AC | Phân loại | Given (Điều kiện tiền đề) | When (Hành động người dùng) | Then (Kết quả kỳ vọng) |
| :--- | :---: | :--- | :--- | :--- | :--- |
| Story 1:<br>"Là một độc giả, tôi muốn đặt mượn online một cuốn sách đang có sẵn để tôi không mất công đến thư viện rồi mới biết sách đã có người mượn." | AC 1.1 | Happy path | Độc giả đã đăng nhập, tài khoản đang giữ dưới 3 cuốn sách, và cuốn "Lập trình Agile" còn bản in trong kho. | Độc giả bấm nút "Đặt mượn sách" trên trang chi tiết và xác nhận. | Hệ thống tạo phiếu mượn với mã nhận sách riêng, giảm 1 cuốn khả dụng trong kho, đổi trạng thái sách sang "Đang giữ chỗ" và báo hạn nhận sách tại quầy trong 48 giờ tới. |
| | AC 1.2 | Negative case | Độc giả mở trang sách "Clean Architecture", nhưng người khác vừa mượn cuốn cuối cùng khiến số lượng tồn về 0. | Độc giả bấm nút "Đặt mượn sách". | Hệ thống từ chối thao tác, báo sách vừa hết bản in có sẵn và đổi nút bấm sang "Đặt trước sách". |
| | AC 1.3 | Edge case | Độc giả đặt mượn thành công cuốn "Refactoring" lúc 08:00 sáng thứ Hai với hạn nhận là 08:00 sáng thứ Tư, nhưng không đến nhận. | Thời gian vượt qua mốc 48 giờ (08:01 sáng thứ Tư) mà thủ thư chưa xác nhận giao sách. | Hệ thống tự hủy phiếu mượn quá hạn, chuyển sách về lại trạng thái có sẵn trong kho và gửi thông báo nhắc nhở vào tài khoản của độc giả. |
| | AC 1.4 | Ràng buộc nghiệp vụ | Độc giả đang có sách mượn quá hạn chưa trả hoặc còn tiền phạt chưa đóng tại thư viện. | Độc giả bấm nút "Đặt mượn sách" cho một cuốn bất kỳ. | Hệ thống chặn thao tác, thông báo tài khoản đang bị tạm khóa và hướng dẫn gặp thủ thư tại quầy để xử lý. |
| Story 2:<br>"Là một độc giả, tôi muốn nhận thông báo khi sách tôi đặt trước đã có sẵn để tôi kịp đến nhận sách trước khi hết thời gian giữ chỗ." | AC 2.1 | Happy path | Độc giả đứng đầu danh sách chờ của cuốn "Design Patterns" khi sách đang hết bản in. | Một người khác mang trả cuốn sách này tại quầy và thủ thư quét mã nhập kho. | Hệ thống chuyển sách sang trạng thái giữ chỗ cho độc giả này, kích hoạt hạn 48 giờ và gửi thông báo qua tài khoản kèm email báo sách đã có sẵn tại quầy. |
| | AC 2.2 | Negative case | Độc giả đầu danh sách đã nhận thông báo nhưng sau 48 giờ không đến nhận, trong khi vẫn còn người thứ hai trong hàng đợi. | Hết 48 giờ giữ sách của người thứ nhất. | Hệ thống hủy quyền giữ sách của người thứ nhất, chuyển quyền giữ chỗ sang cho người thứ hai và gửi email thông báo kèm hạn 48 giờ mới cho người thứ hai. |
| | AC 2.3 | Edge case | Cuốn sách "Scrum Guide" có 2 người xếp hàng chờ; cùng lúc có 2 bạn đọc mang trả 2 cuốn sách này trong vòng 5 phút. | Thủ thư quét mã nhập kho liên tiếp cả 2 cuốn. | Hệ thống phân bổ 2 cuốn cho 2 người theo đúng thứ tự hàng đợi, tính hạn 48 giờ riêng cho từng người và gửi thông báo đến từng tài khoản. |
| | AC 2.4 | Edge case | Độc giả đang trong hàng đợi đặt trước nhưng đã mượn được sách ở nơi khác và không còn nhu cầu. | Độc giả vào mục cá nhân và bấm "Hủy đặt trước". | Hệ thống xóa tên độc giả khỏi danh sách chờ, nâng thứ tự của những người phía sau lên một bậc và không gửi thông báo khi sách về kho sau đó. |
| Story 3:<br>"Là một thủ thư, tôi muốn giới hạn số sách một độc giả được mượn cùng lúc để tránh tình trạng một người giữ hết sách." | AC 3.1 | Happy path | Thư viện quy định mượn tối đa 3 cuốn; độc giả hiện đang giữ 2 cuốn và tài khoản bình thường. | Độc giả đặt mượn thêm 1 cuốn sách có sẵn. | Hệ thống cho phép đặt mượn, cập nhật số sách đang giữ lên 3/3 và ghi chú độc giả đã dùng hết hạn mức mượn. |
| | AC 3.2 | Negative case | Độc giả đã mượn đủ 3 cuốn (gồm cả sách cầm về nhà và sách đang đặt giữ chỗ ở quầy). | Độc giả bấm nút "Đặt mượn sách" cho cuốn thứ tư. | Nút đặt mượn bị vô hiệu hóa, hệ thống báo lỗi đã đạt giới hạn 3 cuốn cùng lúc và nhắc trả bớt sách hoặc hủy đơn giữ chỗ cũ nếu muốn mượn thêm. |
| | AC 3.3 | Edge case | Độc giả đang giữ 2 cuốn (chỉ còn hạn ngạch 1 cuốn), mở 2 tab trình duyệt cho 2 cuốn sách khác nhau. | Độc giả bấm xác nhận đặt mượn trên cả 2 tab gần như cùng thời điểm. | Máy chủ chỉ tiếp nhận yêu cầu gửi đến trước (chạm mức 3/3), yêu cầu gửi đến sau vài mili giây bị từ chối với thông báo vượt quá số lượng cho phép. |
| | AC 3.4 | Ràng buộc nghiệp vụ | Thủ thư có quyền quản trị vào trang cấu hình mượn trả của thư viện. | Thủ thư đổi giới hạn mượn từ 3 lên 5 cuốn rồi bấm lưu. | Hệ thống cập nhật hạn mức mới cho các lượt đặt mượn từ thời điểm đó trở đi, không làm thay đổi các lượt mượn đã tạo trước đó. |

---

## 3. Câu hỏi làm rõ gửi Product Owner

Nhóm lập danh sách câu hỏi cần trao đổi thêm với Product Owner trước khi đưa story vào Sprint:

Cho Story 1 (Đặt mượn online):
- Thời hạn 48 giờ giữ sách tính theo giờ liên tục 24/7 hay chỉ tính theo ngày mở cửa của thư viện? Nếu đặt chiều thứ Bảy mà Chủ nhật thư viện nghỉ thì thời gian tính thế nào?
- Một độc giả có được đặt mượn cùng lúc 2 bản in của cùng một đầu sách không?
- Độc giả có được chủ động hủy đơn giữ chỗ trên hệ thống khi chưa hết hạn 48 giờ không?

Cho Story 2 (Thông báo khi sách đặt trước có sẵn):
- Kênh thông báo chính thức gồm những kênh nào (email, thông báo trên web, hay có SMS không)? Trường hợp gửi email bị lỗi mạng thì hệ thống có gửi lại không?
- Nếu sách đặt trước về kho nhưng lúc đó độc giả đã mượn đủ 3 cuốn khác, hệ thống sẽ giữ chỗ 48 giờ để họ kịp trả sách cũ, hay chuyển ngay cho người kế tiếp trong hàng đợi?
- Thời gian giữ chỗ cho sách đặt trước là 48 giờ hay ngắn hơn (chẳng hạn 24 giờ để sách nhanh đến tay người sau)?

Cho Story 3 (Giới hạn số sách mượn cùng lúc):
- Giới hạn 3 cuốn áp dụng chung cho tất cả độc giả hay có phân chia theo đối tượng (chẳng hạn giảng viên được mượn nhiều hơn sinh viên)?
- Số sách tính vào hạn mức gồm những trạng thái nào: chỉ tính sách mang về nhà, hay tính cả sách đang giữ chỗ chờ nhận và sách trong danh sách chờ đặt trước?
- Thủ thư tại quầy có quyền tăng tạm thời số lượng mượn cho độc giả có nhu cầu đặc biệt (như làm đồ án tốt nghiệp) không?

---

## 4. Definition of Done của nhóm

Bộ tiêu chuẩn chất lượng áp dụng chung cho mọi hạng mục công việc trong Sprint:

1. Toàn bộ tiêu chí nghiệm thu (Acceptance Criteria) của story được kiểm thử đạt 100% trên môi trường Staging.
2. Mã nguồn mới có unit test với độ bao phủ từ 80% trở lên; các bài test tích hợp chạy thành công trên pipeline CI/CD.
3. Pull request được ít nhất 1 lập trình viên khác review và phê duyệt, không còn cảnh báo lỗi từ linter hoặc công cụ quét mã tĩnh.
4. Mã nguồn đã merge vào nhánh chính và triển khai thành công lên môi trường Staging mà không làm hỏng các tính năng cũ.
5. Tài liệu API, tài liệu hệ thống và hướng dẫn sử dụng cho thủ thư hoặc độc giả được cập nhật đầy đủ.
6. Product Owner kiểm tra trực tiếp trên môi trường Staging và ký nhận nghiệm thu trước buổi Sprint Review.

---

## 5. Đánh giá trạng thái hoàn thành của Story 1

Câu hỏi: "Story 1 đã đạt đủ các AC bạn viết, nhưng code chưa được review theo DoD của nhóm. Story 1 đã được coi là Done chưa? Vì sao?"

Trả lời:
Story 1 chưa được coi là Done. Trong Scrum, một công việc chỉ hoàn thành khi đáp ứng đủ các Acceptance Criteria riêng của nó và thỏa mãn toàn bộ tiêu chí trong Definition of Done của nhóm. Code review là bước bắt buộc trong Definition of Done để kiểm soát chất lượng mã nguồn. Nếu bỏ qua bước này, tính năng chưa đủ điều kiện để đóng gói vào phần tăng trưởng sản phẩm (Increment) của Sprint.
