# Giới thiệu

> Ghi chú: Phiên bản này cũng chính là phiên bản in
[The Rust Programming
> Language][nsprust] và ebook [No Starch
> Press][nsp] của sách.

[nsprust]: https://nostarch.com/rust
[nsp]: https://nostarch.com/

Chào mừng bạn đến với *Ngôn ngữ lập trình Rust*, một cuốn sách giới thiệu về Rust.
"Ngôn ngữ lập trình Rust" sẽ giúp bạn viết các phần mềm nhanh và tin cậy hơn.
Việc thiết kế ngôn ngữ lập trình luôn phải giải quyết bài toán xung đột giữa
kiểm soát ở cấp thấp và việc hỗ trợ con người ở bậc cao; Rust thách thức sự xung đột
này. Thông qua khả năng cân bằng giữa khả năng tiếp thu công nghệ mạnh mẽ và kinh nghiệm
phát triển tuyệt vời, Rust cung cấp cho bạn khả năng kiểm soát các chi tiết ở cấp độ 
thấp (chẳng hạn việc sử dụng bộ nhớ) mà vẫn tránh được những phiền toái vốn hay gặp phải
khi phải làm việc ở cấp độ này.

## Rust được dành cho ai

Rust khá lý tưởng cho nhiều người vì nhiều lý do khác nhau. Hãy cũng xem qua một 
vài trong số những nhóm lý do quan trọng nhất.

### Các nhóm phát triển

Rust đang dần chứng minh như một công cụ hữu hiệu cho việc cộng tác giữa những nhóm 
lớn nhà phát triển với các mức độ kiến thức về lập trình hệ thống khác nhau. Mã lệnh cấp thấp
thường dính phải các loại lỗi ẩn, vốn chỉ có thể tìm thấy thông qua việc kiểm thử 
hoặc review cẩn thận bởi các nhà phát triển nhiều kinh nghiệm. Trong Rust, trình biên
dịch đóng vai trò như người gác cổng bằng cách từ chối các loại lỗi như vậy, bao gồm
cả các lỗi liên quan đến việc xử lý đồng thời. Bằng cách kết hợp với trình dịch, nhóm
phát triển có thể dành thời gian tập trung cho logic chương trình hơn là tìm kiếm
các lỗi.

Rust cũng đồng thời cung cấp các công cụ hỗ trợ phát triển đến cho thế giới lập trình hệ thống:

* Cargo, công cụ quản lý thư viện và build, cho phép thêm, dịch, quản lý 
các thư viện phụ thuộc dễ dàng và đồng bộ xuyên suốt hệ sinh thái Rust.
* Rustfmt đảm bảo sự đồng nhất về phong cách viết code giữa các nhà phát triển.
* The Rust Language Server cung cấp sức mạnh cho các Integrated Development Environment (IDE) để 
hỗ trợ các tính năng như code completion và các thông báo lỗi tại chỗ.

Bằng cách sử dụng các công cụ ở trên cũng như một số công cụ khác trong hệ sinh thái
Rust, các nhà phát triển có thể viết một cách hiệu quả các mã lệnh cấp hệ thống.

### Sinh viên

Rust được dành cho sinh viên và bất kỳ ai yêu thích việc học các khái niệm 
hệ thống. Nhiều người thông qua sử dụng Rust đã học về những chủ đề như phát
triển hệ điều hành. Cộng đông Rust cũng rất sẵn sàng chào đón và hỗ trợ trả 
các câu hỏi của sinh viên. Thông qua những nỗ lực tương tự như cuốn sách này,
các nhóm Rust muốn làm cho việc hiểu các khái niệm về hệ thống dễ dàng hơn với 
nhiều người, đặc biệt những người mới làm quen với lập trình.

### Các công ty

Hàng trăm công ty, cả lớn và nhỏ, sử dụng Rust cho nhiều nhiệm vụ khác nhau trong 
hoạt động của họ. Những nhiệm vụ đó bao gồm các công cụ dòng lệnh, dịch vụ web, 
các công cụ DevOps, các thiết bị nhúng, các trình phân tích và mã hóa âm thanh 
hình ảnh, tiền mã hóa, tin sinh học, các máy tìm kiếm, các ứng dụng IoT, học máy, 
và thậm chí các phần chính của trình duyệt web FireFox.

### Các nhà phát triển mã nguồn mở

Rust is for people who want to build the Rust programming language, community,
developer tools, and libraries. We’d love to have you contribute to the Rust
language.


### People Who Value Speed and Stability

Rust is for people who crave speed and stability in a language. By speed, we
mean the speed of the programs that you can create with Rust and the speed at
which Rust lets you write them. The Rust compiler’s checks ensure stability
through feature additions and refactoring. This is in contrast to the brittle
legacy code in languages without these checks, which developers are often
afraid to modify. By striving for zero-cost abstractions, higher-level features
that compile to lower-level code as fast as code written manually, Rust
endeavors to make safe code be fast code as well.

The Rust language hopes to support many other users as well; those mentioned
here are merely some of the biggest stakeholders. Overall, Rust’s greatest
ambition is to eliminate the trade-offs that programmers have accepted for
decades by providing safety *and* productivity, speed *and* ergonomics. Give
Rust a try and see if its choices work for you.

## Who This Book Is For

This book assumes that you’ve written code in another programming language but
doesn’t make any assumptions about which one. We’ve tried to make the material
broadly accessible to those from a wide variety of programming backgrounds. We
don’t spend a lot of time talking about what programming *is* or how to think
about it. If you’re entirely new to programming, you would be better served by
reading a book that specifically provides an introduction to programming.

## How to Use This Book

In general, this book assumes that you’re reading it in sequence from front to
back. Later chapters build on concepts in earlier chapters, and earlier
chapters might not delve into details on a topic; we typically revisit the
topic in a later chapter.

You’ll find two kinds of chapters in this book: concept chapters and project
chapters. In concept chapters, you’ll learn about an aspect of Rust. In project
chapters, we’ll build small programs together, applying what you’ve learned so
far. Chapters 2, 12, and 20 are project chapters; the rest are concept chapters.

Chapter 1 explains how to install Rust, how to write a “Hello, world!” program,
and how to use Cargo, Rust’s package manager and build tool. Chapter 2 is a
hands-on introduction to the Rust language. Here we cover concepts at a high
level, and later chapters will provide additional detail. If you want to get
your hands dirty right away, Chapter 2 is the place for that. At first, you
might even want to skip Chapter 3, which covers Rust features similar to those
of other programming languages, and head straight to Chapter 4 to learn about
Rust’s ownership system. However, if you’re a particularly meticulous learner
who prefers to learn every detail before moving on to the next, you might want
to skip Chapter 2 and go straight to Chapter 3, returning to Chapter 2 when
you’d like to work on a project applying the details you’ve learned.

Chapter 5 discusses structs and methods, and Chapter 6 covers enums, `match`
expressions, and the `if let` control flow construct. You’ll use structs and
enums to make custom types in Rust.

In Chapter 7, you’ll learn about Rust’s module system and about privacy rules
for organizing your code and its public Application Programming Interface
(API). Chapter 8 discusses some common collection data structures that the
standard library provides, such as vectors, strings, and hash maps. Chapter 9
explores Rust’s error-handling philosophy and techniques.

Chapter 10 digs into generics, traits, and lifetimes, which give you the power
to define code that applies to multiple types. Chapter 11 is all about testing,
which even with Rust’s safety guarantees is necessary to ensure your program’s
logic is correct. In Chapter 12, we’ll build our own implementation of a subset
of functionality from the `grep` command line tool that searches for text
within files. For this, we’ll use many of the concepts we discussed in the
previous chapters.

Chapter 13 explores closures and iterators: features of Rust that come from
functional programming languages. In Chapter 14, we’ll examine Cargo in more
depth and talk about best practices for sharing your libraries with others.
Chapter 15 discusses smart pointers that the standard library provides and the
traits that enable their functionality.

In Chapter 16, we’ll walk through different models of concurrent programming
and talk about how Rust helps you to program in multiple threads fearlessly.
Chapter 17 looks at how Rust idioms compare to object-oriented programming
principles you might be familiar with.

Chapter 18 is a reference on patterns and pattern matching, which are powerful
ways of expressing ideas throughout Rust programs. Chapter 19 contains a
smorgasbord of advanced topics of interest, including unsafe Rust, macros, and
more about lifetimes, traits, types, functions, and closures.

In Chapter 20, we’ll complete a project in which we’ll implement a low-level
multithreaded web server!

Finally, some appendices contain useful information about the language in a
more reference-like format. Appendix A covers Rust’s keywords, Appendix B
covers Rust’s operators and symbols, Appendix C covers derivable traits
provided by the standard library, Appendix D covers some useful development
tools, and Appendix E explains Rust editions.

There is no wrong way to read this book: if you want to skip ahead, go for it!
You might have to jump back to earlier chapters if you experience any
confusion. But do whatever works for you.

<span id="ferris"></span>

An important part of the process of learning Rust is learning how to read the
error messages the compiler displays: these will guide you toward working code.
As such, we’ll provide many examples that don’t compile along with the error
message the compiler will show you in each situation. Know that if you enter
and run a random example, it may not compile! Make sure you read the
surrounding text to see whether the example you’re trying to run is meant to
error. Ferris will also help you distinguish code that isn’t meant to work:

| Ferris                                                                                                           | Meaning                                          |
|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| <img src="img/ferris/does_not_compile.svg" class="ferris-explain" alt="Ferris with a question mark"/>            | This code does not compile!                      |
| <img src="img/ferris/panics.svg" class="ferris-explain" alt="Ferris throwing up their hands"/>                   | This code panics!                                |
| <img src="img/ferris/not_desired_behavior.svg" class="ferris-explain" alt="Ferris with one claw up, shrugging"/> | This code does not produce the desired behavior. |

In most situations, we’ll lead you to the correct version of any code that
doesn’t compile.

## Source Code

The source files from which this book is generated can be found on
[GitHub][book].

[book]: https://github.com/rust-lang/book/tree/main/src
