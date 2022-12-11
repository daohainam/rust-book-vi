# Error Handling

Lỗi (error) là một sự thực tế trong lập trình, vì vậy Rust có nhiều tính năng để
xử lý các tình huống mà có thể xảy ra lỗi. Trong nhiều trường hợp, Rust yêu cầu
bạn phải nhận thức được khả năng xảy ra lỗi và thực hiện một hành động nào đó
trước khi code của bạn được biên dịch. Yêu cầu này làm cho chương trình của bạn
trở nên bền bỉ hơn bằng cách đảm bảo rằng bạn sẽ phát hiện lỗi và xử lý chúng
đúng cách trước khi bạn đã triển khai code của mình vào môi trường production!

Rust phân loại lỗi thành hai loại chính: lỗi *khôi phục được* (recoverable) và
lỗi *không khôi phục được* (unrecoverable). Ví dụ về lỗi khôi phục được là lỗi
*không tìm thấy tệp* (file not found), chúng ta có thể chỉ cần báo cho người
dùng biết về lỗi này và thử lại thao tác. Lỗi không khôi phục được luôn là dấu
hiệu của bug, ví dụ như truy cập một vị trí nằm ngoài phạm vi của một mảng, và
vì vậy chúng ta muốn ngay lập tức dừng chương trình.

Hầu hết các ngôn ngữ không phân biệt giữa hai loại lỗi này và xử lý cả hai theo
cùng một cách, sử dụng các cơ chế như ngoại lệ (exceptions). Rust không có
ngoại lệ. Thay vào đó, nó có kiểu `Result<T, E>` cho lỗi khôi phục được và
biểu thức `panic!` dừng việc thực thi khi chương trình gặp lỗi không khôi phục
được. Chương này sẽ bắt đầu bằng việc gọi `panic!` và sau đó sẽ nói về việc
trả về giá trị `Result<T, E>`. Ngoài ra, chúng ta sẽ khám phá các điều cần
xem xét khi quyết định có nên cố gắng khôi phục lỗi hay dừng việc thực thi.
