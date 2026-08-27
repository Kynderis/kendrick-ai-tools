# KIDEA — Bản nháp yêu cầu về context và traceability

Trạng thái: Bản nháp nguyên văn từ Human, chưa phân tích hoặc chốt thành yêu cầu chính thức.

"Mỗi step sẽ têu cầu lên plan và thực thi cho trọn vẹn. xem xét mỗi task con trong đó khi bắt đầu thì load lại context để hiểu cho trọn vẹn, tránh hanllucinate. Sau khi xong 1 task con thì update trong 1 file index hoặc mapping nào đó để biết các file/module đã làm gì, đang gọi đến đâu và được đâu gọi đến. Từ đó khi sửa hoặc xoá thì truy ra các bên liên quan và làm đến cùng. Tất nhiên mỗi lần sửa đổi kèm version của session đang sửa. Nếu session file cũ thì buộc phải sửa. Nếu session trùng session hiện tại thì xem xét update, vì có thể phần sửa ngay trước mới chỉ update logic của nó mà chưa có cho mình. Code cũng tương tự vậy, phải có index như kiểu Doxygen bên c++ ấy, dễ dàng trace các bên liên quan và update. Mỗi task load lại context thì AI cũng dễ nắm bắt hơn và tiết kiệm token trong khi context sạch"
