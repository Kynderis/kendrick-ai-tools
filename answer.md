# Trả lời của Claude

Đã tạo `CLAUDE.md`, và vì bạn cho tôi quyền tự đề xuất, tôi viết luôn bản thiết kế `kidea` cho Claude Code thay vì port nguyên si bản Codex.

## 1. File quy tắc

- `CLAUDE.md` — file chuẩn, Claude Code tự nạp mỗi session mới. Chứa: project này là gì, bố cục thư mục, quy tắc nhánh `claude`, quy tắc mirror `answer.md`.
- `AGENTS.md` — rút gọn thành con trỏ về `CLAUDE.md`.

Tôi không symlink vì trên Windows symlink cần dev mode. Con trỏ đạt cùng mục đích và tránh hai nguồn sự thật lệch nhau.

## 2. Bản thiết kế mới

Nằm ở `design/KIDEA_DESIGN_V0.1.md`. Tôi giữ **triết lý** của bản Codex nhưng thay **cơ chế** và cắt mạnh **quy mô**. Sáu điều chỉnh nền tảng:

### 2.1. Gate phải chặn cứng, không phải nhắc nhở

Đây là lý do gốc khiến bản Codex không cho bạn cảm giác kiểm soát. Trên Codex, luật "AI không được nhảy cóc" chỉ là chữ trong prompt. AI đọc, gật đầu, rồi vài nghìn token sau nó quên và bắt đầu viết code. Bạn không có cách nào ngăn — bạn chỉ có thể hy vọng.

Claude Code có **hook**. `PreToolUse` chạy **trước** mỗi lần AI gọi Write/Edit/Bash, đọc state, và **từ chối** thao tác không hợp lệ:

```text
AI muốn sửa services/order/place_order.py
        ↓ PreToolUse: kidea_guard.py
FEAT-MVP-ORDER-LIMIT.requirements = NEEDS_CLARIFICATION
        ↓
DENY: "Gate REQUIREMENTS chưa pass. Thiếu: chính sách slippage."
```

AI không bỏ qua được vì nó không phải bên quyết định. Nguyên tắc rút ra:

> **Luật nào quan trọng thì viết thành script, không viết thành prompt.**

Đây là thứ Codex không có, và là lý do chính đáng nhất để chuyển sang Claude Code.

### 2.2. Đơn vị trạng thái là feature, không phải project

Bản cũ tự mâu thuẫn: vừa đặt `current_phase` cho cả project, vừa bảo làm theo vertical slice. Hai thứ này không cùng tồn tại được.

Sửa: project chỉ có phase khi bootstrap (`P1_SCOPE → P2_REQUIREMENTS → P3_FOUNDATION`), sau đó đứng yên vĩnh viễn ở `P4_BUILD`. Từ đó trở đi mọi trạng thái thuộc về từng feature, và nhiều feature ở nhiều bước khác nhau cùng lúc là **đúng**, không phải lỗi.

### 2.3. Bốn file thay cho một cây thư mục

`kidea.yaml` (Human sở hữu) / `state.yaml` (script sở hữu) / `graph.json` (sinh ra) / `log.jsonl` (append-only).

Quan trọng nhất: **AI không được ghi trực tiếp vào `state.yaml`** — hook chặn. Nếu AI ghi tay được thì nó sẽ tự phong `HUMAN_APPROVED` cho chính mình, đúng cái bạn sợ.

### 2.4. Traceability phải trích xuất, không được kê khai

Đây là chỗ tôi **sửa yêu cầu trong draft của bạn**.

Bạn viết: *"Sau khi xong 1 task con thì update trong 1 file index... để biết các file/module đang gọi đến đâu và được đâu gọi đến."*

Nếu **AI tự tay update** index đó, nó sẽ lệch khỏi thực tế trong vài ngày, rồi AI sẽ tin vào index sai — tức là hallucinate, đúng cái bạn đang chống. Index do AI kê khai không đáng tin hơn trí nhớ AI.

Bạn đã nói đúng từ khoá mà chưa khai thác hết: **Doxygen**. Doxygen không bắt ai duy trì file index — nó **đọc source có annotation rồi tự sinh index**. Đó mới là mô hình đúng.

Nên chia hai lớp:

**Lớp dẫn xuất** (máy đọc, không ai viết): call graph, import graph — trích bằng tree-sitter từ source. Không thể sai, vì code đổi thì lần chạy sau ra kết quả mới.

**Lớp ngữ nghĩa** (viết *trong code*, không ra file riêng):

```python
# @kidea:feature FEAT-MVP-ORDER-LIMIT
# @kidea:implements BR-BAL-003, BR-BAL-004
def reserve_balance(user_id: str, amount: Decimal) -> ReservationId:
```

Annotation **nằm cạnh code** nên ai sửa hàm cũng nhìn thấy. File index tách rời thì không ai nhìn thấy, nên không ai update. Và câu hỏi *"sửa BR-BAL-003 thì ảnh hưởng đâu"* thành một truy vấn graph do script trả lời.

### 2.5. Đơn vị version là CHANGE, không phải session

Logic của bạn đúng, nhưng "session" là khái niệm sai để lưu — nó chết khi bạn đóng terminal, và một tháng sau `session_id: 8f3a` không nói lên gì.

Thay bằng **CHANGE record**, thứ tồn tại lâu dài và review được. Quy tắc của bạn giữ nguyên, chỉ đổi trục: `synced_with` bằng change hiện tại thì bỏ qua; cũ hơn thì **bắt buộc** xem lại. Upstream đổi hash thì downstream chuyển `STALE` và approval bị thu hồi — script làm, không phải AI.

### 2.6. Context sạch cho sub-task chính là subagent

Yêu cầu đầu tiên trong draft của bạn. Trên Codex phải làm tay; trên Claude Code là cơ chế có sẵn — **subagent chạy trong context window riêng, hoàn toàn sạch**.

Thiết kế: script tính **context pack** từ `graph.json` cho mỗi sub-task (đúng BR nó implement, đúng LT nó phải pass, đúng file nó được sửa, neighbor trong call graph), rồi mỗi sub-task chạy trong một subagent chỉ nhận pack đó. AI không phải đoán cần đọc file nào — nó được đưa đúng thứ cần. Vừa sạch, vừa tiết kiệm token, đúng như bạn mô tả.

## 3. Cắt quy mô

Bản cũ có ~40 lệnh và ~50 thư mục `docs/`. Số đó không bao giờ xây xong. Tôi thu về **8 lệnh**:

`init` · `status` · `check` · `index` · `next` · `approve` · `impact` · `change <type>`

9 lệnh `change *` cũ thành một lệnh có tham số `type` — mỗi type nạp checklist riêng, nhưng chỉ một đường code.

## 4. Cần bạn chốt 4 điểm

1. **Ngôn ngữ script** — tôi đề xuất Python (có tree-sitter, chạy được trong hook). Bạn có ràng buộc gì không?
2. **Ngôn ngữ của project đầu tiên dùng kidea** — parser phải viết theo ngôn ngữ, v0.1 nên hỗ trợ một cái trước.
3. **Mức chặt của hook** — tôi đề xuất chặn cứng với code, cảnh báo với docs.
4. **Có xây kidea bằng chính kidea không?** — tôi đề xuất **không** cho v0.1. Xây tay, dùng thật, rồi mới tính.

Chốt xong 4 điểm, bước kế tiếp là đặc tả chính xác `state.yaml`, `graph.json` và hành vi 6 lệnh v0.1 — rồi bắt đầu code.

---

Một lưu ý cuối: bạn nói đặt hết niềm tin vào tôi. Tôi nhận, nhưng xin nói thẳng một điều — thiết kế trên là **đề xuất của tôi, chưa được kiểm chứng bằng việc chạy thật**. Điểm 2.4 và 2.6 tôi tự tin vì chúng dựa trên cơ chế có sẵn. Điểm 2.1 (hook chặn cứng) tôi tin đúng hướng nhưng chi tiết contract của hook phải xác minh khi code. Cứ phản biện chỗ nào bạn thấy sai — bạn hiểu bài toán của mình hơn tôi.
