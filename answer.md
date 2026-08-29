# Trả lời của Claude

Bạn nói đúng, chúng ta đi xa rồi. Tóm lại.

Đã thêm phần này vào đầu `design/00-BAT-DAU-TU-DAY.md` để lần sau mở ra là thấy ngay.

---

## Điều cuối cùng kidea phải làm được

Chỉ **hai việc**.

### Việc 1 — AI không được nhảy cóc

> *"Nếu ta không có quy trình thì với một project lớn, ta sẽ rất khó control."*
> — lời bạn, buổi đầu tiên

AI không được viết code khi nghiệp vụ chưa rõ. Không được tự nói "xong rồi". Không được tự duyệt cho chính mình.

Chặn bằng **script chạy tự động**, không bằng lời dặn trong prompt.

### Việc 2 — Sửa gì cũng phải làm đến cùng

> *"Khi sửa hoặc xoá thì truy ra các bên liên quan và làm đến cùng."*
> — lời bạn, trong bản nháp

Sửa một luật thì biết ngay còn chỗ nào phải sửa theo, chỗ nào phải test lại. Không sót.

Trả lời bằng cách **tra bản đồ**, không bằng trí nhớ của AI.

### Thứ làm cho cả hai chạy được

Trạng thái nằm **trong repo**, không nằm trong trí nhớ phiên chat. Đóng máy, mở lại tuần sau, gõ một lệnh là biết đang ở đâu.

---

Ba dòng trên là toàn bộ mục tiêu.

Tám trạm, năm loại khối, ba tấm bản đồ, mười bốn loại thay đổi — **tất cả chỉ là cách làm** cho hai việc đó. Không có cái nào là mục tiêu riêng.

---

## Chúng ta đang ở đâu

| Phần | Trạng thái | Phục vụ việc nào |
|---|---|---|
| Cuốn sổ trạng thái | Thiết kế xong | Nền cho cả hai |
| Người gác chặn AI | Thiết kế xong | Việc 1 |
| Bản đồ nghiệp vụ | Thiết kế xong | Việc 2 |
| Lệnh dò ảnh hưởng | Thiết kế xong | Việc 2 |
| Human duyệt, AI không tự duyệt | Thiết kế xong | Việc 1 |

**Phần lõi đã xong.** Đủ để bắt đầu viết.

## Chỗ tôi đã đi quá đà

| Phần | Vì sao thừa lúc này |
|---|---|
| Mười bốn loại thay đổi | Cần khi bạn đã có sản phẩm chạy và bắt đầu sửa. Còn xa |
| Năm trạm cuối (kiến trúc, code, nghiệm thu, phát hành) | Chỉ chạm tới sau khi qua trạm nghiệp vụ. Còn xa |
| Bản đồ CODE và CẦU NỐI | Chỉ cần khi bắt đầu có code. Còn xa |
| Bảng kiểm 21 dòng | Cái này thì cần sớm, nhưng chưa cần duyệt ngay |

Không phí — sau này đều dùng. Nhưng **viết sớm hơn mức cần**, và đó là lý do bạn thấy lan man. Lỗi của tôi.

## Việc thật sự cần làm tiếp

Chỉ có ba, theo thứ tự:

**1. Cuốn sổ và người gác.**
Xong bước này bạn tự tay thử được: bảo tôi "code đi" khi trạm chưa qua, và xem tôi bị chặn. Đây là lúc biết **việc 1** có chạy thật không.

**2. Nhập tài liệu và soát nghiệp vụ.**
Xong bước này bạn đưa tài liệu ChatGPT thật vào và xem kidea báo thiếu gì.

**3. Bản đồ nghiệp vụ và lệnh dò ảnh hưởng.**
Xong bước này bạn đổi một luật và xem máy khoanh vùng. Đây là lúc biết **việc 2** có chạy thật không.

Ba bước này là toàn bộ phần lõi. Xong ba bước, kidea đã làm được hai việc bạn cần. Mọi thứ khác thêm vào sau.

---

Bạn muốn tôi bắt đầu bước 1 chưa, hay còn muốn chốt gì trước?
