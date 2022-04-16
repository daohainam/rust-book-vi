## Cài đặt

Bước đầu tiên là cài đặt Rust. Chúng ta sẽ tải về Rust thông qua `rustup`, một công cụ
dòng lệnh quản lý phiên bản và các công cụ của Rust. Bạn sẽ cần một đường Internet 
để download.

> Ghi chú: nếu vì lý do gì đó bạn không muốn dùng `rustup`, xin đọc phần
> [Other Rust Installation Methods page][otherinstall] for more options.

[otherinstall]: https://forge.rust-lang.org/infra/other-installation-methods.html

Các bước kế tiếp cài đặt phiên bản ổn định mới nhất của trình dịch Rust. Các yêu cầu về tính
ổn định của Rust đảm bảo rằng tất cả các ví dụ có thể dịch được trong sách vẫn có thể tiếp tục 
dịch được với các phiên bản Rust mới hơn. Các thông báo có thể khác nhau một chút, vì Rust 
thường xuyên cải thiện các thông báo lỗi hoặc các cảnh báo. Nói cách khác, bất kỳ phiên bản 
ổn định mới hơn nào của Rust cũng đều hoạt động với nội dung cuốn sách này.

> ### Quy ước về dòng lệnh
>
> Trong chương này cũng như trong các phần còn lại của sách, chúng tôi 
> sẽ trình bày một số câu lệnh giống như khi chạy trong cửa sổ terminal.
> Các dòng bạn cần nhập vào terminal sẽ bắt đầu với `$`. Tuy nhiên bạn 
> không cần nhập ký tự `$`; nó chỉ để biểu thị vị trí bắt đầu của các câu lệnh.
> Các dòng không bắt đầu với `$` thường là kết quả hiển thị của câu lệnh trước đó.
> Thêm nữa, các ví dụ với PowerShell sẽ dùng `>` thay vì `$`.

### Cài đặt `rustup` trên Linux hoặc macOS

Nếu bạn đang dùng Linux hay macOS, hãy mở một cửa sổ terminal và gõ vào lệnh sau:

```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Câu lệnh này sẽ tải về một đoạn script và bắt đầu quá trình cài đặt công cụ
`rustup`, tiếp sau đó sẽ cài đặt phiên bản ổn định mới nhất của Rust. Bạn có thể
được nhắc nhập vào mật khẩu. Nếu quá trình cài đặt thành công, dòng sau đây sẽ xuất hiện:

```text
Rust is installed now. Great!
```

Bạn cũng sẽ cần một trình liên kết (linker), thứ mà Rust sẽ dùng để kết nối các 
kết quả dịch riêng biệt thành một file duy nhất. Và thường nó sẽ được cài đặt sẵn. 
Tuy nhiên nếu bạn gặp phải các lỗi liên kết, bạn cần cài một trình biên dịch C, và 
nó thường sẽ bao gồm cả một trình liên kết. Một trình dịch C cũng cũng cần thiết vì 
các gói Rust dựa trên mã C và do đó sẽ cần một trình dịch C.

Trên MacOS, bạn có thể cài đặt một trình dịch C bằng cách chạy:

```console
$ xcode-select --install
```

Với  người dùng Linux nói chung nên cài GCC hoặc Clang, tùy thuộc vào tài liệu của
bộ phân phối Linux mà họ sử dụng. Ví dụ, nếu bạn dùng Ubuntu, bạn có thể cài đặt gói 
`build-essential`.

### Cài đặt `rustup` trên Windows

Trên Windows, truy cập vào [https://www.rust-lang.org/tools/install][install] và 
theo các hướng dẫn để cài đặt Rust. Trong lúc cài đặt bạn sẽ nhận được một thông 
báo nói về việc bạn sẽ cần thêm các công cụ build cho Visual Studio 2013 hoặc mới
hơn. Cách dễ nhất để lấy về các công cụ này là cài đặt [Build Tools for Visual Studio 2019][visualstudio].
Khi được hỏi phần nào cần được cài đặt, nhớ hãy chọn phần “C++ build tools”
và Windows 10 SDK and the English language pack.

[install]: https://www.rust-lang.org/tools/install
[visualstudio]: https://visualstudio.microsoft.com/visual-cpp-build-tools/

Các câu lệnh trong phần còn lại của cuốn sách này sẽ làm việc với cả hai *cmd.exe* 
và PowerShell. Nếu có gì khác nhau, chúng tôi sẽ chỉ ra bạn cần sử dụng cái nào.

### Cập nhật và gỡ bỏ

Sau khi đã cài đặt xong Rust thông qua `rustup`, việc cập nhật phiên bản mới nhất
khá đơn giản. Từ dòng lệnh, chạy câu lệnh cập nhật sau:

```console
$ rustup update
```

Để gỡ bỏ Rust và `rustup`, chạy câu lệnh sau:

```console
$ rustup self uninstall
```

### Xử lý trục trặc

Để kiểm tra xem bạn đã cài đặt Rust đúng chưa, mở một cửa sổ lệnh mới và gõ vào
dòng sau:

```console
$ rustc --version
```

Bạn sẽ thấy số phiên bản, chuỗi hash và ngày commit của phiên bản ổn định mới nhất
theo định dạng sau:

```text
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

Nếu bạn thấy thông tin này, bạn đã cài đặt Rust thành công! Nếu không thấy và bạn 
đang sử dụng Windows, hãy kiểm tra xe Rust có trong biến môi trường PATH không.
Nếu tất cả đều đúng nhưng Rust vẫn không chạy, bạn có thể tìm sự giúp đỡ từ một 
số nơi. Cách dễ nhất là kênh #beginners trên [the official Rust Discord][discord]. 
Ở đó bạn có thể chat với những Rustacean (nickname chúng tôi tự đặt cho bản thân) khác.
Những tài nguyên tuyệt vời khác bao gồm [the Users forum][users] và [Stack Overflow][stackoverflow].

[discord]: https://discord.gg/rust-lang
[users]: https://users.rust-lang.org/
[stackoverflow]: https://stackoverflow.com/questions/tagged/rust

### Tài liệu trên máy

Bản cài đặt của Rust cũng bao gồm một bản sao của tài liệu này, dó đó bạn có thể 
đọc offline. Chạy `rustup doc` để mở tài liệu này bằng trình duyệt của bạn.

Bất kỳ lúc nào nếu bạn không biết một kiểu dữ liệu hay một hàm trong thư viện chuẩn
phải được dùng thế nào, hay tra tài liệu application programming interface (API)!
