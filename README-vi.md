# Ngôn ngữ lập trình Rust

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

Repository chứa mã nguồn cho cuốn sách "Ngôn ngữ lập trình Rust".

[The book is available in dead-tree form from No Starch Press][nostarch].

[nostarch]: https://nostarch.com/rust-programming-language-2nd-edition

Xin lưu ý sách có thể được phát hành theo các kênh phát hành Rust khác nhau, bao
gồm [stable], [beta], or [nightly]. Cũng xin lưu ý các vấn đề trong những phiên bản
trên cũng có thể đã được sửa trên repository, vì các bản phát hành thường được cập nhập
ít thường xuyên hơn.

[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

Đọc phần [releases] để tải về mã nguồn của tất cả các đoạn mã xuất hiện trong sách.

[releases]: https://github.com/rust-lang/book/releases

> ### Ghi chú sau dành riêng cho bản dịch tiếng Việt
> Bạn có thể đọc bản dịch tiếng Việt được build với mdbook tại địa chỉ:
> https://daohainam.github.io/rust-book/.
> Để tham gia dịch thuật, xin đọc https://github.com/rust-lang/book/issues/375#issuecomment-1295252256

## Yêu cầu

Để dịch sang định dạng sách online bạn sẽ cần [mdBook], tốt nhất là cùng phiên bản
mà rust-lang/rust dùng trong [tệp tin này][rust-mdbook]. Để tải về:

[mdBook]: https://github.com/rust-lang/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --version <version_num>
```

Trong sách này cũng chứa hai mdbook plugin như một phần của repository. Nếu bạn không cài
đặt chúng, bạn sẽ thấy một vài cảnh báo khi dịch và kết quả sẽ không chính xác, 
nhưng bạn _sẽ_ vẫn build được. Để dùng các plugin này, bạn hãy chạy lệnh sau:

```bash
$ cargo install --locked --path packages/mdbook-trpl
```
## Building

Để dịch thành sách online, gõ:

```bash
$ mdbook build
```

Kết quả sẽ được lưu lại trong thư mục con `book`. Bạn có thể mở ra đọc bằng trình
duyệt web của bạn.

_Firefox:_

```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_

```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

Để chạy phần kiểm tra:

```bash
$ cd packages/trpl
$ mdbook test --library-path packages/trpl/target/debug/deps
```

## Đóng góp

Chúng tôi trân trọng sự trợ giúp của các bạn! Xin đọc thêm [CONTRIBUTING.md][contrib]
để tìm hiểu về những cách đóng góp cho dự án.

[contrib]: https://github.com/rust-lang/book/blob/main/CONTRIBUTING.md

Vì bộ sách này đã được [in][nostarch], và cũng vì chúng tôi muốn giữ
cho phiên bản trực tuyến gần với phiên bản in nhất có thể, do vậy có thể mất nhiều thời
gian để chúng giải quyết các vấn đề hoặc pull request của bạn.

Cho đến nay, chúng tôi đang thực hiện một bản sửa đổi lớn hơn để khớp với
[Phiên bản Rust](https://doc.rust-lang.org/edition-guide/). Giữa những bản chỉnh sửa
lớn, chúng tôi sẽ chỉ đưa ra các bản sửa lỗi. Nếu vấn đề hoặc pull request của bạn
không nghiêm trọng, có thể bạn sẽ phải chờ đến lần chúng tôi làm việc trên một bản sửa
đổi lớn tiếp theo: dự kiến là theo tháng hoặc năm.
Rất cảm ơn vì sự kiên nhẫn của bạn!

### Dịch thuật

Chúng tôi rất trân trọng việc trợ giúp dịch bộ sách này sang các ngôn ngữ khác! Xin
xem nhãn [Translations] để tham gia vào những nỗ lực đang được thực hiện. Mở một issue
mới để bắt đầu một bản dịch mới cho một ngôn ngữ. Chúng tôi đang chờ [mdbook support]
để hỗ trợ đa ngôn ngữ trước khi merge, nhưng hiện tại các bạn hãy cứ thoải mái bắt đầu!

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang/mdBook/issues/5

## Kiểm tra chính tả

Để quét các file mã nguồn để dò lỗi chính tả, bạn có thể dùng `spellcheck.sh` có trong
thư mục `ci`. Nó cần một từ điển các từ hợp lệ, được chứa trong `ci/dictionary.txt`. Nếu
script trên sinh ra một lỗi (ví dụ như khi bạn dùng từ `BTreeMap` vối đối với script là
không hợp lệ), bạn cần thêm từ này vào `ci/dictionary.txt` (đưa vào đúng vị trí để giữ
đúng thứ tự sắp xếp sẵn có).
](https://github.com/rust-lang/book/issues/375#issuecomment-1295252256)
