# Managing Growing Projects with Packages, Crates, and Modules

Khi bạn viết các chương trình lớn, việc tổ chức code của bạn sẽ trở nên càng
quan trọng hơn. Bằng cách nhóm các tính năng liên quan và tách code với các
tính năng khác, bạn sẽ biết được rõ ràng tính năng được thực hiện bởi code
nào và nơi nào để thay đổi một tính năng.

Những chương trình chúng ta đã viết cho đến nay đều nằm trong một module trong
một file. Khi dự án lớn hơn, bạn nên tổ chức code bằng cách chia nó thành nhiều
module và sau đó nhiều file. Một gói (package) có thể chứa nhiều binary crate và
có thể có thêm các crate thư viện. Khi package lớn hơn, bạn có thể tách các
phần ra thành các crate riêng biệt trở thành các phụ thuộc bên ngoài (external 
dependencies). Chương này sẽ đề cập tất cả các kỹ thuật này. Đối với các dự án
rất lớn bao gồm một tập hợp các package liên quan và phát triển cùng nhau,
Cargo cung cấp  *workspaces*, chúng ta sẽ xem nó trong phần [“Cargo Workspaces”][workspaces]<!-- ignore --> ở chương 14.

Chúng ta cũng sẽ thảo luận về việc đóng gói code, cho phép bạn tái sử dụng code
ở một mức cao hơn: một khi bạn đã triển khai một phép toán, code khác có thể
gọi code của bạn thông qua giao diện công khai (public interface) mà không cần
biết cách thức code đó hoạt động bên trong. Cách bạn viết code xác định phần
nào là công khai (public) để code khác sử dụng và phần nào riêng tư (private)
chỉ code của bạn mới biết.

Một khái niệm liên quan là phạm vi (scope): ngữ cảnh (context) lồng nhau trong
đó code được viết với một tập các tên được định nghĩa gọi là "in scope". Khi
đọc, viết và biên dịch code, các lập trình viên và trình biên dịch cần biết
liệu một tên cụ thể tại một vị trí cụ thể có phải là một biến, hàm, struct,
enum, module, hằng số, hay một item khác và item đó có ý nghĩa gì. Bạn có thể
tạo ra các scope và thay đổi các tên trong scope hoặc ngoài scope. Bạn không
thể có hai item cùng tên trong cùng một scope; luôn có sẵn các công cụ giúp bạn
giải quyết xung đột tên.

Rust có các tính năng cho phép bạn quản lý tổ chức code của bạn, bao gồm
đoạn code nào là public hay private, và tên nào sẽ nằm trong mỗi scope trong
chương trình của bạn. Những tính năng này, thường được gọi là *module system*,
bao gồm:

* **Packages:** Một tính năng của Cargo cho phép bạn xây dựng, kiểm tra và chia
  sẻ các crate
* **Crates:** Một cây của các module tạo ra một thư viện (library) hoặc một chương trình thực thi (executable)
* **Modules** và **use:** Cho phép bạn kiểm soát tổ chức, scope, và quyền riêng tư của đường dẫn (paths)
* **Paths:** Một cách để đặt tên một item, như một struct, hàm, hoặc module

Trong chương này, chúng ta sẽ tìm hiểu tất cả các tính năng này, thảo luận về
cách chúng tương tác với nhau và giải thích cách sử dụng chúng để quản lý scope.
Cuối cùng, bạn sẽ có một cái nhìn chắc chắn về module system và có thể làm việc
với scope như một dân chuyên!

[workspaces]: ch14-03-cargo-workspaces.html
