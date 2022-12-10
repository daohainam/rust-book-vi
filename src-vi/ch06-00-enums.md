# Enums and Pattern Matching

Trong chương này, chúng ta sẽ xem xét *enumerations*, còn được gọi là *enums*.
Enum cho phép định nghĩa một kiểu dữ liệu bằng cách liệt kê các biến thể 
(variant) có thể xảy ra. Đầu tiên, chúng ta sẽ định nghĩa và sử dụng một enum
qua đó sẽ cho thấy cách một enum có thể mã hóa dữ liệu. Tiếp theo, chúng ta sẽ
khám phá một enum đặc biệt, gọi là `Option`, thể hiện có thể có giá trị hoặc
không có giá trị. Sau đó, chúng ta sẽ xem xét cách các mẫu (pattern) được khớp
(match) với nhau trong biểu thức `match` qua đó cho phép chúng ta có thể chạy
các code khác nhau phụ thuộc vào giá trị của enum. Cuối cùng, chúng ta sẽ tìm
hiểu cách cấu trúc `if let` là một idiom tiện lợi và ngắn gọn khác có sẵn để xử
lý các enum trong code.