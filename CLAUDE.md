# Project instructions

Đây là file quy tắc chuẩn của repo. `AGENTS.md` chỉ trỏ về đây.

## Project này là gì

Repo thiết kế **`kidea`** (kendrick + idea): một Skill cho Claude Code biến ý tưởng thành MVP rồi thành sản phẩm hoàn chỉnh, thông qua một quy trình có state và có quality gate, để AI không tự nhảy cóc các bước.

**Repo này chưa có code sản phẩm.** Nó chứa tài liệu thiết kế của chính `kidea`.

| Thư mục | Nội dung |
|---|---|
| `design/` | Thiết kế `kidea` đang có hiệu lực |
| `conversations/` | Lịch sử thảo luận thiết kế, dạng nguyên văn |
| `drafts/` | Yêu cầu thô từ Human, chưa phân tích |
| `answer.md` | Bản mirror câu trả lời cuối cùng (xem bên dưới) |

## Working branch

Cho đến khi Human nói rõ rằng quy tắc này dừng lại:

- Toàn bộ công việc của project được thực hiện trên nhánh `claude`.
- Mọi commit và push đều đi lên `origin/claude`, không đẩy lên `main`.

## Remote answer mirror

Cho đến khi Human nói rõ rằng quy tắc này dừng lại:

- Trước mỗi câu trả lời cuối cùng, ghi nguyên nội dung câu trả lời đó vào `/answer.md`, thay thế câu trả lời cũ.
- Commit và push `answer.md` lên `origin/claude` trước khi gửi câu trả lời cuối cùng.
- Chỉ mirror câu trả lời cuối cùng; không mirror các commentary cập nhật tiến độ tạm thời.
- Git history là lịch sử các câu trả lời cũ, vì vậy không append nhiều câu trả lời vào cùng file.
- Có thể commit `answer.md` cùng thay đổi chính của task hoặc bằng commit tiếp theo, miễn là remote có nội dung mới trước final response.
- Repository là public: không ghi secret, credential, token hoặc thông tin riêng tư không cần thiết vào `answer.md`.
- Công việc chính của project vẫn được thực hiện, commit và push bình thường.

## Ngôn ngữ

Trả lời Human bằng tiếng Việt. Tài liệu thiết kế viết bằng tiếng Việt; ID, tên file, tên trạng thái và code giữ tiếng Anh.
