## Hello, World!

Giờ bạn đã hoàn thành cài đặt Rust, chúng ta sẽ bắt đầu chương trình đầu tiên.
Một truyền thống khi học ngôn ngữ mới là viết một chương trình in ra dòng chữ
`Hello, world!` lên màn hình, và chúng ta cũng sẽ làm như vậy.

> Ghi chú: Quyển sách này cho làm bạn đã quen thuộc với dòng lệnh. Rust không có
> yêu cầu gì đặc biệt về các công cụ hay trình soạn thảo mà bạn sử dụng, vậy nên
> bạn cứ thoải mái nếu muốn dùng một IDE nào khác thay vì dòng lệnh. Nhiều IDE 
> cung cấp hỗ trợ cho Rust ở những cấp độ khác nhau; hãy kiểm tra tài liệu về 
> IDE của bạn để có thêm thông tin. Gần đây nhóm Rust đã tập trung vào việc cung 
> cấp các hỗ trợ tuyệt vời cho các IDE, và quá trình này đã hoàn thành rất nhanh
> chóng.

### Tạo ra một thư mục cho dự án

Bạn sẽ bắt đầu bằng việc tạo ra một thư mục để lưu mã nguồn Rust của bạn. Việc
nó nằm ở đâu không quan trọng, nhưng để dễ dàng cho việc sử dụng các bài tập hoặc
dự án trong cuốn sách này, chúng tôi đề nghị bạn nên tạo một thư mục *projects* 
trong thư mục home của bạn và lưu tất cả các dự án trong đó.

Mở một terminal và nhập vào các câu lệnh sau để tạo một thư mục *projects* và một
thư mục con cho chương trình “Hello, world!” bên trong đó.

Với Linux, macOS, và PowerShell trên Windows, nhập vào lệnh sau:

```console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

Với dòng lệnh Windows (CMD), nhập vào:

```cmd
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

### Viết và chạy một chương trình Rust

Tiếp theo, tạo một file mã nguồn và đặt tên nó là *main.rs*. Các file Rust luôn 
kết thúc với đuôi *.rs*. Nếu bạn dùng nhiều hơn một từ trong tên file, hãy dùng 
ký tự gạch dưới _ để phân cách chúng. Ví dụ, dùng *hello_world.rs* thay vì 
*helloworld.rs*.

Giờ mở file *main.rs* bạn vừa tạo và nhập vào dòng code trong Listing 1-1.

<span class="filename">Filename: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<span class="caption">Listing 1-1: Một chương trình in ra dòng `Hello, world!`</span>

Lưu lại file và trở lại cửa sổ terminal. Trên Linux hoặc macOS, nhập vào các 
lệnh sau để dịch và chạy:

```console
$ rustc main.rs
$ ./main
Hello, world!
```

Trên Windows, gõ vào lệnh `.\main.exe` thay vì `./main`:

```powershell
> rustc main.rs
> .\main.exe
Hello, world!
```

Không phụ thuộc vào hệ điều hành nào, chuỗi `Hello, world!` sẽ được in ra màn
hình terminal. Nếu bạn không thấy dòng này, hãy tham khảo lại phần 
[“Troubleshooting”][troubleshooting]<!-- ignore --> trong phần cài đặt để xem
những cách để tìm kiếm hỗ trợ.

Nếu dòng `Hello, world!` được in ra, xin chúc mừng! Bạn đã chính thức viết một
chương trình Rust, và bạn đã bắt đầu con đường trở thành một lập trình viên Rust!

### Phân tích các thành phần trong một chương trình Rust

Hãy cùng xem lại một cách chi tiết những gì đã xảy ra trong chương trình 
“Hello, world!”. Đây là phần đầu tiên:

```rust
fn main() {

}
```

Các dòng này định nghĩa một hàm trong Rust. Hàm `main` là một hàm đặc biệt: nó 
là nơi chứa các câu lệnh đầu tiên nhất của chương trình. Dòng đầu tiên khai báo 
một hàm có tên `main` không có tham số và không trả về bất kỳ giá trị nào. Nếu 
có thêm tham số, chúng sẽ phải nằm bên trong cặp dấu ngoặc tròn `()`.

Tương tự, thân hàm sẽ nằm trong cặp dấu ngoặc nhọn `{}`. Rust yêu cầu chúng bao 
toàn bộ thân hàm. Tốt nhất là bạn đặt dấu ngoặc nhọn mở trên cùng dòng khai báo 
hàm với một khoảng trắng phân cách ở giữa.

Nếu bạn muốn áp dụng một định dạng code chuẩn trong tất cả các dự án Rust, bạn 
có thể dùng một công cụ định dạng có tên `rustfmt`. Nhóm Rust đã thêm công cụ này
vào gói cài đặt chuẩn của Rust, giống như `rustc`, do vậy nó chắc chắn cũng đã được
cài sẵn trong máy tính của bạn. Hãy xem thêm các tài liệu online nếu cần thêm
thông tin.

Bên trong hàm `main` là đoạn code sau:

```rust
    println!("Hello, world!");
```

Dòng này thực hiện toàn bộ công việc trong chương trình tí hon này: in một dòng 
văn bản ra màn hình. Có bốn chi tiết quan trọng cần lưu ý ở đây:

Đầu tiên, Rust canh dòng bằng bốn khoảng trắng, không phải dấu tab.

Thứ hai, `println!` gọi là một macro. Nếu là một hàm, nó sẽ được gọi với `println`
(không có dấu `!`). Chúng ta sẽ thảo luận chi tiết thêm về macro trong chương 19. 
Hiện tại, chúng ta chỉ cần biết là khi dùng `!`, chúng ta sẽ gọi đến một macro thay
vì một hàm bình thường, và các macro đó không hoàn toàn sử dụng cùng các quy tắc như 
các hàm.

Thứ ba, bạn thấy chuỗi `"Hello, world!"`. Chúng ta truyền chuỗi này như một tham 
số cho `println!`, và nó sẽ được in ra mạn hình.

Thứ tư, chúng ta kết thúc dòng với một dấu chấm phẩy (`;`), nó chỉ ra rằng câu lệnh
đã kết thúc và sẵn sàng cho lệnh kế tiếp. Hầu hết các lệnh Rust kết thúc bằng một 
dấu chấm phẩy.

### Dịch và chạy là các bước riêng biệt

Bạn vừa chạy chương trình mới tạo, giờ hãy cùng xem qua các bước trong toàn bộ 
quá trình.

Trước khi chạy một chương trình Rust, bạn phải dịch nó dùng trình dịch
Rust bằng cách gõ vào câu lệnh `rustc` và truyền cho nó tên file nguồn của bạn,
tương tự dưới đây:

```console
$ rustc main.rs
```

Nếu bạn đã có một nền tảng C hay C++, bạn sẽ thấy điều này tương tự như lệnh
`gcc` hay `clang`. Sau khi biên dịch thành công, Rust sẽ tạo ra một file thực thi.

Trên Linux, macOS, và PowerShell trên Windows, bạn có thể thấy file thực thi này 
bằng cách chạy lệnh `ls`. Trên Linux và macOS, bạn sẽ thấy 2 file. Với PowerShell 
trên Windows, bạn sẽ thấy 3 file giống như khi bạn liệt kê file dùng CMD.

```console
$ ls
main  main.rs
```

Với CMD trê Windows, Bạn có thể nhập vào:

```cmd
> dir /B %= tùy chọn /B chỉ ra bạn chỉ muốn hiển thị các file =%
main.exe
main.pdb
main.rs
```

This shows the source code file with the *.rs* extension, the executable file
(*main.exe* on Windows, but *main* on all other platforms), and, when using
Windows, a file containing debugging information with the *.pdb* extension.
From here, you run the *main* or *main.exe* file, like this:

```console
$ ./main # or .\main.exe on Windows
```

If *main.rs* was your “Hello, world!” program, this line would print `Hello,
world!` to your terminal.

If you’re more familiar with a dynamic language, such as Ruby, Python, or
JavaScript, you might not be used to compiling and running a program as
separate steps. Rust is an *ahead-of-time compiled* language, meaning you can
compile a program and give the executable to someone else, and they can run it
even without having Rust installed. If you give someone a *.rb*, *.py*, or
*.js* file, they need to have a Ruby, Python, or JavaScript implementation
installed (respectively). But in those languages, you only need one command to
compile and run your program. Everything is a trade-off in language design.

Just compiling with `rustc` is fine for simple programs, but as your project
grows, you’ll want to manage all the options and make it easy to share your
code. Next, we’ll introduce you to the Cargo tool, which will help you write
real-world Rust programs.

[troubleshooting]: ch01-01-installation.html#troubleshooting
