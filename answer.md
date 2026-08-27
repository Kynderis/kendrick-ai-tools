# Trả lời của Codex

Mình hiểu đúng ý bạn.

Từ bây giờ, cho đến khi bạn nói dừng:

- Mỗi **câu trả lời cuối cùng** của mình sẽ đồng thời được ghi vào file `answer.md` ở root repository.
- `answer.md` luôn chứa câu trả lời mới nhất; câu cũ vẫn xem được qua Git history. Cách này giúp file dễ đọc và không dài mãi theo thời gian.
- Mình sẽ commit và push `answer.md` lên `origin/main` trước khi gửi câu trả lời trong Codex.
- Các cập nhật tiến độ ngắn trong lúc mình đang thao tác sẽ không được ghi vào file; chỉ câu trả lời hoàn chỉnh cuối cùng mới được mirror.
- Công việc chính của Kidea vẫn tiếp tục bình thường. Code, tài liệu và các thay đổi khác vẫn được commit/push như trước; `answer.md` có thể đi cùng commit đó hoặc một commit ngay sau nó.
- Vì repository public, mình sẽ không ghi secret, credential, token hoặc thông tin riêng tư không cần thiết vào `answer.md`. Nếu câu trả lời có dữ liệu nhạy cảm, mình sẽ báo và lược phần đó khỏi bản public thay vì làm lộ thông tin.

Mình cũng đã lưu quy tắc này trong `AGENTS.md` để các phiên Codex sau không quên khi context thay đổi. Quy tắc được áp dụng ngay từ câu trả lời này.
