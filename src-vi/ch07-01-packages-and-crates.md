## Packages and Crates

Phần đầu tiên trong module system mà chúng ta sẽ tìm hiểu là packages và crates.

Một *crate* là một đoạn code nhỏ nhất mà Rust compiler xem xét. Ngay cả khi bạn
chạy `rustc` thay vì `cargo` và truyền một file nguồn code (như chúng ta đã làm
từ đầu trong phần “Writing and Running a Rust Program” của Chapter 1), compiler
sẽ xem file đó là một crate. Crates có thể chứa modules, và các modules có thể
được định nghĩa trong các file khác mà được compile cùng với crate, như chúng
ta sẽ thấy trong các phần tiếp theo.

Một crate có thể có một trong hai dạng: một binary crate hoặc một library crate.
*Binary crates* là các chương trình bạn có thể compile thành một executable mà
bạn có thể chạy, như một chương trình command-line hoặc một server. Mỗi binary
crate đều phải có một function gọi là `main` mà định nghĩa những gì sẽ xảy ra
khi executable chạy. Tất cả các crates mà chúng ta đã tạo cho đến nay đều là
binary crates.

*Library crates* không có một function `main` và không compile thành một
executable. Thay vào đó, chúng định nghĩa các tính năng được dùng chung với
nhiều dự án khác. Ví dụ, crate `rand` mà chúng ta đã sử dụng trong [Chapter
2][rand]<!-- ignore --> cung cấp các chức năng để tạo ra các số ngẫu nhiên.
Hầu hết thời gian khi Rustaceans (người dùng Rust) nói “crate”, họ đang nói về
library crate, và họ sử dụng “crate” thay thế cho khái niệm chung của một
“library".

*Crate root* là một file nguồn mà Rust compiler bắt đầu từ đó và tạo ra một
root module cho crate của bạn (chúng ta sẽ giải thích modules sâu hơn trong
phần [“Defining Modules to Control Scope and Privacy”][modules]<!-- ignore -->).

Một *package* là một bó (bundle) của một hoặc nhiều crates cung cấp một
tập hợp các tính năng. Một package chứa một file *Cargo.toml* mô tả cách
build các crates. Cargo thực ra là một package chứa binary crate
command-line mà bạn đã sử dụng để build code của mình. Package Cargo cũng
chứa một library crate mà binary crate phụ thuộc vào. Các dự án khác có thể
phụ thuộc vào library crate của Cargo để sử dụng cùng logic với command-line
tool của Cargo.

Một package có thể chứa bao nhiêu binary crates bạn muốn, nhưng tối đa chỉ có
một library crate. Một package phải chứa ít nhất một crate, dù đó là một
library crate hay binary crate.

Cùng xem qua những gì sẽ xảy ra khi chúng ta tạo một package. Đầu tiên, chúng ta
nhập lệnh `cargo new`:

```console
$ cargo new my-project
     Created binary (application) `my-project` package
$ ls my-project
Cargo.toml
src
$ ls my-project/src
main.rs
```

Sau khi chúng ta chạy `cargo new`, chúng ta sử dụng lệnh `ls` để xem những gì
Cargo tạo ra. Trong thư mục dự án, có một file *Cargo.toml*, định nghĩa cho
chúng ta một package. Có một thư mục *src* chứa file *main.rs*. Mở file *Cargo.toml* trong trình soạn thảo văn bản, và chú ý rằng không có đề cập nào đến
file *src/main.rs*. Cargo quy ước rằng *src/main.rs* là crate root của
một binary crate cùng tên với package. Tương tự, Cargo biết rằng nếu thư
mục package chứa file *src/lib.rs*, package chứa một library crate với cùng
tên với package, và *src/lib.rs* là crate root của nó. Cargo truyền file crate
root cho `rustc` để build library hoặc binary.

Ở đây, chúng ta có một package chỉ chứa file *src/main.rs*, có nghĩa là nó
chỉ chứa một binary crate có tên là `my-project`. Nếu một package chứa file
*src/main.rs* và *src/lib.rs*, nó có hai crates: một binary và một library, cả
hai có cùng tên với package. Một package có thể có nhiều binary crates bằng cách
đặt file trong thư mục *src/bin*: mỗi file sẽ là một binary crate riêng biệt.

[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
