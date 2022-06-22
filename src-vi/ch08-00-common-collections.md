# Những tập hợp thông dụng

Thư viện chuẩn của Rust bao gồm một lượng lớn cấu trúc dữ liệu rất hữu dụng được gọi là
*tập hợp*. Hầu hết các kiểu dữ liệu khác đại diện cho một giá trị cụ thể nào đó, nhưng
những tập hợp này có thể chứa nhiều loại giá trị. Không giống như mảng (array) được dựng sẵn và loại dữ liệu tích hợp (tuple), dữ liệu mà các tập hợp này trỏ tới được lưu trữ trên vùng heap, có nghĩa là tại thời điểm biên dịch sẽ không cần phải biết lượng dữ liệu như thế nào, nó có thể tăng lên hoặc giảm lại khi chạy chương trình. Mỗi loại tập hợp có những khả năng, chi phí tiêu tốn tài nguyên khác nhau, và để chọn được một loại phù hợp với tình huống sử dụng của bạn là cả một kỹ năng bạn cần phải phát triển liên tục theo thời gian. Trong chương này, chúng ta sẽ thảo luận về ba loại tập hợp được sử dụng rất thường xuyên trong các chương trình Rust:

* Một *vector* cho phép bạn lưu trữ một số giá trị có thể thay đổi nằm gần cạnh nhau.
* Một *string* là một tập hợp các ký tự. Chúng ta đã đề cập đến loại `String`
   trước đây, nhưng trong chương này chúng ta sẽ nói sâu hơn về nó.
* Một *hash map* cho phép bạn liên kết một giá trị với một khóa cụ thể. Nó là một
   triển khai (implement) cụ thể của cấu trúc dữ liệu tổng quát hơn được gọi là *map*.

Để tìm hiểu về các loại tập hợp khác được cung cấp bởi thư viện tiêu chuẩn,
xem [tài liệu hướng dẫn][collections].

Chúng ta cũng sẽ thảo luận về cách tạo và cập nhật vector, string và hash map,
cũng như những gì làm cho mỗi thứ trở nên đặc biệt.

[collections]: ../std/collections/index.html
