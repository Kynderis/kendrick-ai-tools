Theo mình, ta chỉ cần **6 bước** để làm ra Kidea hoàn chỉnh. Mỗi bước phải được chốt xong trước khi sang bước kế tiếp.

## 1. Chắt lọc yêu cầu thật sự

Dùng đúng hai nguồn:

- Hội thoại gốc `kidea-workflow-chat-2026-08-24-25.md`.
- Bản draft về nạp lại context và truy vết quan hệ.

Tạo một tài liệu yêu cầu duy nhất, chia nội dung thành:

- **Bắt buộc:** Kidea chắc chắn phải làm được.
- **Cần làm rõ:** ý đúng nhưng chưa đủ rõ để triển khai.
- **Loại bỏ:** phức tạp hoặc không phục vụ trực tiếp nhu cầu.

Ở bước này chỉ chốt **Kidea phải giúp người dùng đạt được gì**, chưa bàn file, field, enum hay công nghệ.

## 2. Chốt luồng sử dụng Kidea

Thiết kế một luồng chính, dùng tên dễ hiểu:

```text
Nhận ý tưởng
→ Làm rõ sản phẩm
→ Chốt yêu cầu và tiêu chí kiểm thử
→ Chốt giao diện nếu có và kiến trúc
→ Xây dựng
→ Kiểm chứng trên DEV
→ Chuẩn bị production
→ Phát hành và theo dõi
```

Mỗi chặng chỉ cần định nghĩa năm thứ:

1. Cần đầu vào gì.
2. Kidea phải làm gì.
3. Kết quả bắt buộc là gì.
4. Điều kiện nào khiến phải dừng.
5. Khi nào cần Human quyết định hoặc phê duyệt.

Các thay đổi sau này như thêm tính năng, sửa bug hoặc đổi kiến trúc sẽ quay lại đúng chặng bị ảnh hưởng, thay vì tạo hàng loạt flow riêng ngay từ đầu.

## 3. Chốt cơ chế context và truy vết trong bản draft

Đây là yêu cầu quan trọng, nhưng cần triển khai tối giản:

- Trước mỗi task con, Kidea dựng lại một **gói context mới** từ trạng thái hiện tại của repository.
- Gói context chỉ chứa yêu cầu, quyết định, module, contract, test và thay đổi gần đây có liên quan trực tiếp.
- Sau mỗi task con, Kidea ghi lại phần đã thay đổi và những bên liên quan.
- Trước khi sửa hoặc xóa, Kidea phải tìm các code, test, tài liệu và contract có thể bị ảnh hưởng.
- Quan hệ nào có thể tìm lại chính xác từ code thì tìm khi cần; chỉ lưu lâu dài những quan hệ hoặc quyết định không thể tự suy ra đáng tin cậy.

Ta cũng cần làm rõ khái niệm “version của session” trong draft. Khuyến nghị của mình là dùng **task ID và Git revision** làm mốc độ mới của thông tin, vì chúng ổn định hơn phiên trò chuyện của AI.

Mục tiêu không phải tạo một cơ sở dữ liệu kiểu Doxygen khổng lồ, mà là bảo đảm AI luôn đọc đúng context và không bỏ sót ảnh hưởng khi thay đổi.

## 4. Suy ra bộ file và cách gọi tối thiểu

Chỉ đến đây ta mới quyết định Kidea cần những file nào. Một file chỉ được tạo nếu nó trả lời “có” cho ít nhất một câu hỏi:

- Có cần nó để tiếp tục công việc trong một phiên Codex mới không?
- Có cần nó để ngăn workflow đi sai hoặc bỏ qua quyết định của Human không?
- Có cần nó để truy ra ảnh hưởng khi sửa hoặc xóa không?

Nếu thông tin có thể tái tạo nhanh và chính xác từ repository thì không lưu thêm.

Skill sẽ được gọi rõ ràng bằng `$kidea`. Ta chưa cần hàng chục command; vòng làm việc cốt lõi chỉ là:

```text
Nạp context → Lập plan → Thực hiện → Kiểm chứng → Ghi nhận → Chỉ ra bước tiếp theo
```

## 5. Viết các tình huống nghiệm thu

Trước khi xây Skill, phải mô tả các tình huống Kidea bắt buộc vượt qua, tối thiểu gồm:

- Bắt đầu từ một bộ tài liệu ý tưởng.
- Gặp yêu cầu thiếu hoặc mâu thuẫn và dừng đúng lúc để hỏi Human.
- Tiếp tục chính xác trong một phiên Codex hoàn toàn mới.
- Lập plan và hoàn thành trọn vẹn một task con.
- Cập nhật bản đồ liên quan sau khi task hoàn tất.
- Truy ra đủ ảnh hưởng trước một lần sửa hoặc xóa.
- Không tiến bước khi kiểm chứng thất bại.
- Đi được trọn luồng từ ý tưởng tới production.

Đây chính là định nghĩa khách quan của chữ **“hoàn chỉnh”**: Kidea chỉ hoàn thành khi vượt qua toàn bộ tình huống đã chốt.

## 6. Xây dựng, thử thật rồi lược bỏ lần cuối

- Viết `SKILL.md` ngắn gọn.
- Chỉ thêm tài liệu tham chiếu khi nội dung thực sự quá dài để đặt trong Skill.
- Chỉ thêm script cho việc cần kết quả máy móc, ổn định và không nên giao AI tự suy đoán.
- Chạy toàn bộ tình huống nghiệm thu trên một project thử tạm thời.
- Thử ngắt giữa chừng rồi tiếp tục bằng phiên mới.
- Thử sửa và xóa một module để kiểm tra truy vết ảnh hưởng.
- Loại bỏ mọi file, field, command hoặc quy tắc không chứng minh được giá trị.
- Sau khi bạn nghiệm thu, mới coi Kidea là hoàn thành.

## Những thứ chưa nên thiết kế lúc này

- Schema và enum chi tiết.
- Một state machine lớn.
- Danh mục hàng chục command.
- Cơ sở dữ liệu toàn bộ function call.
- Hệ thống version riêng cho Kidea.
- Folder ví dụ hoặc tài liệu phụ chưa phục vụ kiểm chứng.

**Bước nên làm ngay bây giờ là bước 1:** mình sẽ chắt lọc hội thoại gốc và bản draft thành một tài liệu yêu cầu ngắn, trong đó chỉ có ba nhóm **Bắt buộc – Cần làm rõ – Loại bỏ**, rồi đưa cho bạn duyệt trước khi thiết kế bất kỳ file hay workflow nào.
