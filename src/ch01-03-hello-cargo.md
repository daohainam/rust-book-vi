## Hello, Cargo!

Cargo là hệ thống build và quản lý các gói của Rust. Hầu hết các Rustacean (cách
chúng tôi gọi những người sử dụng Rust) dùng công cụ này để quản lý các dự án 
Rust vì nó có thể xử lý rất nhiều tác vụ khác nhau, như dịch code, tải các thư viện 
mà chương trình bạn dùng, và dịch các thư viện đó. (Ta gọi các thư viện mà 
chương trình của bạn cần là các phụ thuộc - *dependency*).

Những chương trình đơn giản nhất giống như cái mà chúng ta đã viết, không dùng bất
kỳ dependency nào. Do vậy nếu bạn đã build "Hello, world!" với Cargo, nó có lẽ 
chỉ dùng một phần khả năng để build chương trình của bạn. Khi viết các chương trình
phức tạp hơn, bạn sẽ phải thêm các phụ thuộc, và nếu khởi tạo một dự án bằng cách
dùng Cargo, việc thêm các thư viện phụ thuộc sẽ dễ dàng hơn rất nhiều.

Bởi vì phần lớn các dự án Rust dùng Cargo, phần còn lại trong cuốn sách này cũng
cho rằng bạn đang dùng Cargo. Cargo sẽ được cài đặt chung với Rust về bạn dùng các
bộ cài đặt chuẩn được nói đến trong phần [“Installation”][installation]<!-- ignore -->.
Nếu bạn cài đặt theo những cách khác, hãy kiểm tra thử xem liệu Cargo đã được
cài đặt chưa bằng cách nhập lệnh sau vào terminal:

```console
$ cargo --version
```

Nếu thấy số hiệu phiên bản nghĩa là bạn đã cài đặt nó. Nếu thấy một thông báo lỗi, 
kiểu như `command not found`, hãy đọc tài liệu về phương pháp cài đặt của bạn để
xem cách cài Cargo một cách độc lập.

### Tạo một dự án với Cargo

Hãy bắt đầu một dự án mới dùng Cargo và xem nó khác gì so với dự án “Hello, world!”
lúc đầu. Quay lại thư mục *projects* (hay một nơi nào đó bạn đã lưu mã nguồn). Sau đó
chạy câu lệnh sau:

```console
$ cargo new hello_cargo
$ cd hello_cargo
```

Câu lệnh đầu tiên tạo một thư mục tên là *hello_cargo*. Chúng ta đã đặt tên 
dự án là *hello_cargo*, và Cargo tạo các file của nó trong thư mục cùng tên.

Đi vào bên trong thư mục *hello_cargo* và xem danh sách file, bạn sẽ thấy Cargo 
đã tạo cho chúng ta hai file và một thư mục: file *Cargo.toml* và một thư mục 
có tên *src* với một file *main.rs* ở trong.

Nó cũng khởi tạo một repository Git cùng một file *.gitignore*. Các file Git 
sẽ không được tạo ra nếu bạn chạy `cargo new` bên trong một repository Git sẵn có;
bạn có thể ép Cargo thực hiện bằng câu lệnh `cargo new --vcs=git`.

> Ghi chú: Git là một hệ quản lý phiên bản phổ biến. Bạn có thể thay đổi để 
> `cargo new` dùng một hệ quản lý phiên bản khác hoặc thậm chí không dùng bằng
> cách sử dụng tham số `--vcs`. Chạy `cargo new --help` để xem các tùy chọn được
>hỗ trợ.

Mở *Cargo.toml* trong một trình soạn thảo. Nó sẽ trông tương tự như code trong 
phần Listing 1-2.

<span class="filename">Filename: Cargo.toml</span>

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

[dependencies]
```

<span class="caption">Listing 1-2: Nội dung file *Cargo.toml* được tạo bởi `cargo
new`</span>

File này ở dạng [*TOML*](https://toml.io)<!-- ignore --> (*Tom’s Obvious,
Minimal Language*), là định dạng Cargo dùng để chứa thông tin cấu hình.

Dòng đầu tiên, `[package]`, là phần tiêu đề (section heading) chỉ ra các 
thông tin phía sau là để cấu hình một gói. Khi thêm thông tin vào file này, ta 
cũng sẽ thêm các phần (section) khác.

Ba dòng kế tiếp chứa thông tin Cargo cần để dịch chương trình của bạn: tên, phiên
bản chương trình và phiên bản Rust mà bạn dùng. Chúng ta sẽ nói thêm về `edition` 
trong phần [Phụ lục E][appendix-e]<!-- ignore -->.

The last line, `[dependencies]`, is the start of a section for you to list any
of your project’s dependencies. In Rust, packages of code are referred to as
*crates*. We won’t need any other crates for this project, but we will in the
first project in Chapter 2, so we’ll use this dependencies section then.

Dòng cuối cùng, `[dependencies]`, là nơi bắt đầu danh sách các phụ thuộc mà 
dự án của bạn sử dụng. Trong Rust, các gói mã nguồn được tham chiếu đến như 
các *crates*. Chúng ta sẽ không cần các crate nào khác cho dự án này, nhưng chúng
ta sẽ dùng khi viết dự án đầu tiên ở chương 2, do vậy khi đó ta sẽ sử dụng phần phụ 
thuộc này.

Giờ hãy mở file *src/main.rs* và xem qua:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo đã tạo một chương trình “Hello, world!” cho bạn, giống y như cái chúng ta
đã viết trong phần Listing 1-1! Cho tới giờ, sự khác biệt giữa dự án trước đó 
của chúng ta và dự án do Cargo tạo ra chỉ khác nhau ở chỗ Cargo đưa mã nguồn vào
thư mục *src*, và chúng ta có thêm file *Cargo.toml* trong thư mục gốc của dự án.

Cargo muốn để tất cả các mã nguồn trong thư mục *src*. Thư mục gốc chỉ để chứa các
file README, thông tin cấp phép, các file cấu hình, và những thứ khác không liên 
quan đến mã nguồn. Sử dụng Cargo giúp bạn tổ chức các file trong dự án. Một nơi
cho tất cả, và tất sẽ nằm trong đúng vị trí của nó.

Nếu bắt đầu một dự án mà không sử dụng Cargo, như đã làm với dự án "Hello, world!", 
bạn cũng có thể chuyển nó thành một dự án sử dụng Cargo. Chuyển file dự án vào trong
thư mục src và tạo một file *Cargo.toml* tương ứng.

### Dịch và chạy một dự án Cargo

Giờ cùng xem thử có gì khác khi dịch và chạy chương trình “Hello, world!” với
Cargo! Từ thư mục *hello_cargo*, dịch chương trình của bạn với lệnh sau:

```console
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```

Câu lệnh này tạo ra một file thực thi trong *target/debug/hello_cargo* (hoặc
*target\debug\hello_cargo.exe* nếu chạy trên Windows) thay vì trong thư mục 
hiện tại của bạn. Bạn có thể chạy file này với lệnh sau:

```console
$ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
Hello, world!
```

Nếu mọi thứ đều ổn, dòng `Hello, world!` sẽ được in ra màn hình. Khi chạy câu 
lệnh `cargo build` lần đầu, Cargo cũng sẽ tạo ra một file có tên *Cargo.lock*
trong thư mục gốc của dự án. File này được dùng để theo dõi phiên bản chính xác
của các phụ thuộc mà chương trình của bạn sử dụng, do dự án này không có các phụ thuộc
nên nội dung của nó cũng khá đơn giản. Bạn sẽ không cần sửa đổi file này bằng tay;
Cargo sẽ quản lý nó giúp bạn.

Chúng ta vừa build một dự án với `cargo build` và chạy nó với `./target/debug/hello_cargo`, 
nhưng chúng ta cũng có thể dùng `cargo run` để vừa dịch code vừa chạy trong cùng một câu lệnh:

```console
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Để ý là lần này bạn sẽ không thấy các thông báo khi Cargo biên dịch `hello_cargo`.
Cargo kiểm tra và thấy mã nguồn không bị thay đổi, do vậy nó chỉ chạy file đã
biên dịch. Nếu bạn đã sửa đổi mã nguồn, Cargo sẽ dịch lại trước khi chạy và bạn
sẽ thấy thông báo tương tự như sau:

```console
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Cargo cũng cung cấp một lệnh có tên `cargo check`. Lệnh này được dùng để kiểm tra 
nhanh xem code của bạn có thể dịch được không mà không cần tạo ra một file thực
thi.

```console
$ cargo check
   Checking hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

Why would you not want an executable? Often, `cargo check` is much faster than
`cargo build`, because it skips the step of producing an executable. If you’re
continually checking your work while writing the code, using `cargo check` will
speed up the process! As such, many Rustaceans run `cargo check` periodically
as they write their program to make sure it compiles. Then they run `cargo
build` when they’re ready to use the executable.

Tại sao bạn lại không cần một file thực thi? Thông thường, `cargo check` nhanh
hơn nhiều so với `cargo build`, vì nó bỏ qua bước tạo file thực thi. Nếu bạn có 
thói quen kiểm tra khả năng biên dịch liên tục khi viết code, dùng `cargo check` 
sẽ giúp tăng tốc đáng kể! Do vậy nhiều Rustacean chạy `cargo check` định kỳ khi 
viết làm việc để đảm bảo code có thể dịch được. Sau đó họ sẽ dùng `cargo build`
khi muốn chạy chương trình.

Hãy cũng điểm qua những thứ ta đã học được về Cargo:

* Chúng ta có thể dùng `cargo new` để tạo dự án mới. 
* Chúng ta có thể dịch chương trình bằng cách dùng `cargo build` .
* Chúng ta có thể dịch và chạy chương trình chỉ với một bước bằng `cargo run`.
* Chúng ta có thể dịch mà không cần tạo ra file thực thi bằng `cargo check`.
* Thay vì lưu kết quả dịch vào cùng thư mục code, Cargo lưu chúng trong thư 
mục *target/debug*.

An additional advantage of using Cargo is that the commands are the same no
matter which operating system you’re working on. So, at this point, we’ll no
longer provide specific instructions for Linux and macOS versus Windows.

Một lợi nữa của Cargo là các lệnh của nó hoàn toàn giống nhau, không phụ thuộc 
vào hệ điều hành mà bạn dùng. Do vậy 

### Building for Release

When your project is finally ready for release, you can use `cargo build
--release` to compile it with optimizations. This command will create an
executable in *target/release* instead of *target/debug*. The optimizations
make your Rust code run faster, but turning them on lengthens the time it takes
for your program to compile. This is why there are two different profiles: one
for development, when you want to rebuild quickly and often, and another for
building the final program you’ll give to a user that won’t be rebuilt
repeatedly and that will run as fast as possible. If you’re benchmarking your
code’s running time, be sure to run `cargo build --release` and benchmark with
the executable in *target/release*.

### Cargo as Convention

With simple projects, Cargo doesn’t provide a lot of value over just using
`rustc`, but it will prove its worth as your programs become more intricate.
With complex projects composed of multiple crates, it’s much easier to let
Cargo coordinate the build.

Even though the `hello_cargo` project is simple, it now uses much of the real
tooling you’ll use in the rest of your Rust career. In fact, to work on any
existing projects, you can use the following commands to check out the code
using Git, change to that project’s directory, and build:

```console
$ git clone example.org/someproject
$ cd someproject
$ cargo build
```

For more information about Cargo, check out [its documentation].

[its documentation]: https://doc.rust-lang.org/cargo/

## Summary

You’re already off to a great start on your Rust journey! In this chapter,
you’ve learned how to:

* Install the latest stable version of Rust using `rustup`
* Update to a newer Rust version
* Open locally installed documentation
* Write and run a “Hello, world!” program using `rustc` directly
* Create and run a new project using the conventions of Cargo

This is a great time to build a more substantial program to get used to reading
and writing Rust code. So, in Chapter 2, we’ll build a guessing game program.
If you would rather start by learning how common programming concepts work in
Rust, see Chapter 3 and then return to Chapter 2.

[installation]: ch01-01-installation.html#installation
[appendix-e]: appendix-05-editions.html
