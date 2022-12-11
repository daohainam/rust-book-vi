## Separating Modules into Different Files

Cho đến nay, tất cả các ví dụ trong chương này đều định nghĩa nhiều module
trong một file. Khi các module trở nên lớn, bạn có thể muốn di chuyển định
nghĩa của chúng sang một file riêng để làm cho code dễ điều hướng hơn.

Lấy ví dụ, hãy bắt đầu từ code trong Listing 7-17 có nhiều module nhà hàng.
Chúng ta sẽ trích xuất các module thành các file thay vì định nghĩa tất cả các
module trong file gốc của crate. Trong trường hợp này, file gốc của crate là
*src/lib.rs*, nhưng quy trình này cũng hoạt động với binary crate mà file gốc
của crate là *src/main.rs*.

Đầu tiên, chúng ta sẽ trích xuất module `front_of_house` thành một file riêng.
Xóa code bên trong dấu ngoặc nhọn cho module `front_of_house`, chỉ để lại
câu lệnh `mod front_of_house;`, vì vậy *src/lib.rs* sẽ chứa code như trong
Listing 7-21. Lưu ý rằng code này sẽ không compile cho đến khi chúng ta tạo
file *src/front_of_house.rs* trong Listing 7-22.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/lib.rs}}
```

<span class="caption">Listing 7-21: Định nghĩa module `front_of_house` mà
thân của nó sẽ nằm trong *src/front_of_house.rs*</span>

Tiếp theo, đặt code bên trong dấu ngoặc nhọn vào một file mới có tên
*src/front_of_house.rs*, như trong Listing 7-22. Compiler sẽ biết để tìm kiếm
file này vì nó đã gặp phải định nghĩa module trong file gốc của crate với tên
`front_of_house`.

<span class="filename">Filename: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/front_of_house.rs}}
```

<span class="caption">Listing 7-22: Định nghĩa bên trong module `front_of_house`
trong *src/front_of_house.rs*</span>

Lưu ý rằng bạn chỉ cần load một file với câu lệnh `mod` *một lần* trong
module tree của bạn. Một khi compiler biết file này là một phần của project
(và biết vị trí của file trong module tree theo trí của câu lệnh `mod`), các
file khác trong project của bạn sẽ tham chiếu đến code của file đã load sử dụng
một đường dẫn tới nơi nó, như được đề cập trong phần
[“Paths for Referring to an Item in the Module Tree”][paths]<!-- ignore -->.
Nói cách khác, `mod` *không* là một câu lệnh “include” mà bạn có thể đã thấy
trong các ngôn ngữ lập trình khác.

Tiếp theo, chúng ta sẽ trích xuất module `hosting` ra một file riêng. Quá trình
khác một chút vì `hosting` là một module con của `front_of_house`, không phải
là module gốc. Chúng ta sẽ đặt file cho `hosting` vào một thư mục mới có tên
là module cha của nó trong module tree, trong trường hợp này
*src/front_of_house/*.

Để bắt đầu di chuyển `hosting`, chúng ta sẽ thay đổi *src/front_of_house.rs*
để chỉ chứa định nghĩa của module `hosting`:

<span class="filename">Filename: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house.rs}}
```

Sau đó, chúng ta sẽ tạo một thư mục *src/front_of_house* và một file
*hosting.rs* để chứa các định nghĩa được tạo trong module `hosting`:

<span class="filename">Filename: src/front_of_house/hosting.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house/hosting.rs}}
```

Nếu chúng ta đặt *hosting.rs* trong thư mục *src*, compiler sẽ mong đợi code
*hosting.rs* nằm trong module `hosting` được định nghĩa ở crate root, chớ
không phải là một module con của module `front_of_house`. Quy tắc của
compiler về việc kiểm tra các file để tìm code của các module có nghĩa là các
thư mục và các file sẽ gần với module tree hơn.

> ### Alternate File Paths
>
> Đến đây chúng ta đã tìm hiểu về đường dẫn file mà Rust compiler sử dụng,
> nhưng Rust cũng hỗ trợ một kiểu cũ hơn của đường dẫn file. Với một module
> được đặt tên là `front_of_house` được định nghĩa ở crate root, compiler sẽ
> tìm code của module trong:
>
> * *src/front_of_house.rs* (như chúng ta đã tìm hiểu)
> * *src/front_of_house/mod.rs* (kiểu cũ, đường dẫn vẫn được hỗ trợ)
>
> Với một module được đặt tên là `hosting` là một module con của
> `front_of_house`, compiler sẽ tìm code của module trong:
>
> * *src/front_of_house/hosting.rs* (như chúng ta đã tìm hiểu)
> * *src/front_of_house/hosting/mod.rs* (kiểu cũ, đường dẫn vẫn được hỗ trợ)
>
> Nếu bạn sử dụng cả hai kiểu đường dẫn cho cùng một module, bạn sẽ nhận được
> một lỗi từ compiler. Sử dụng cả hai kiểu đường dẫn cho các module khác nhau
> trong cùng một dự án được cho phép, nhưng có thể gây khó hiểu cho những người
> làm việc trên dự án.
>
> Điểm yếu chính của kiểu đường dẫn sử dụng các file có tên *mod.rs* là dự án
> của bạn có thể kết thúc với nhiều file có tên *mod.rs*, có thể gây khó hiểu
> khi bạn mở chúng cùng lúc trong trình soạn thảo.

Chúng ta đã chuyển code của mỗi module vào một file riêng biệt, và cây module
vẫn giữ nguyên. Các lời gọi hàm trong `eat_at_restaurant` vẫn hoạt động mà không
cần sửa đổi gì, ngay cả khi các định nghĩa được đặt trong các file khác nhau.
Kỹ thuật này cho phép bạn chuyển các module sang các file mới khi chúng tăng
kích thước.

Lưu ý rằng câu lệnh `pub use crate::front_of_house::hosting` trong *src/lib.rs*
cũng không thay đổi, và `use` cũng không ảnh hưởng gì đến việc file nào được
biên dịch là một phần của crate. Câu lệnh `mod` khai báo module, và Rust sẽ tìm
file có cùng tên với module để tìm code của module đó.

## Summary

Rust cho phép bạn chia một package thành nhiều crate và một crate thành các
module để bạn có thể tham chiếu đến các item được định nghĩa trong một module
từ một module khác. Bạn có thể làm điều này bằng cách chỉ định đường dẫn tuyệt
đối hoặc tương đối. Những đường dẫn này có thể được đưa vào scope bằng
lệnh `use` để bạn có thể sử dụng một đường dẫn ngắn hơn cho nhiều lần sử dụng
của item trong scope đó. Code của module mặc định là private, nhưng bạn có
thể làm các item public bằng cách thêm từ khóa `pub`.

Trong chương tiếp theo, chúng ta sẽ xem một số cấu trúc dữ liệu tập hợp trong
thư viện chuẩn mà bạn có thể sử dụng trong code của bạn được tổ chức một cách
rõ ràng.

[paths]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
