## Kiểu Slice

*Slice* cho phép bạn tham chiếu một chuỗi các phần tử liền kề trong một collection
thay vì toàn bộ collection. Một slide là một dạng reference, do đó, nó
không có ownership.

Đây là một vấn đề lập trình nhỏ: viết một hàm nhận vào một chuỗi
các từ được phân tách bằng khoảng trắng và trả về từ đầu tiên nó tìm thấy trong chuỗi đó.
Nếu hàm không tìm thấy khoảng trắng trong chuỗi, toàn bộ chuỗi phải là
một từ, vì vậy toàn bộ chuỗi sẽ được trả về.

Hãy tìm hiểu cách chúng ta viết khai báo hàm này mà không cần sử dụng
slice, để hiểu vấn đề mà slice sẽ giải quyết:

```rust,ignore
fn first_word(s: &String) -> ?
```

Hàm `first_word` có `&String` làm tham số. chúng tôi không muốn
ownership, vì vậy điều này là tốt. Nhưng chúng ta nên trả về những gì? Chúng ta 
không thực sự có một cách để nói về *phần* của một chuỗi. Tuy nhiên, chúng ta 
có thể trả về index của vị trí cuối từ, được biểu thị bằng khoảng trắng. Hãy 
thử điều đó, như trong Liệt kê 4-7.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:here}}
```

<span class="caption">Liệt kê 4-7: Hàm `first_word` trả về một
giá trị byte index vào tham số `String`</span>

Vì chúng ta cần đi qua từng phần tử `String` và kiểm tra xem
một giá trị có là khoảng trắng hay không, chúng ta sẽ chuyển 
đổi `String` của mình thành một mảng byte bằng cách sử dụng
phương thức `as_bytes`:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:as_bytes}}
```

Tiếp theo, chúng ta tạo một iterator trên mảng byte bằng phương thức `iter`:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:iter}}
```

Chúng ta sẽ thảo luận chi tiết hơn về các iterator trong [Chương 13][ch13]<!-- ignore -->.
Hiện tại, hãy biết rằng `iter` là một phương thức trả về từng phần tử trong một collection
và `enumerate` bao bọc kết quả của `iter` và trả về mỗi phần tử dưới dạng tuple. 
Phần tử đầu tiên của bộ dữ liệu được trả về từ
`enumerate` là chỉ mục và phần tử thứ hai là tham chiếu đến phần tử.
Điều này thuận tiện hơn một chút so với việc tự tính toán index.

Vì phương thức `enumerate` trả về một tuple, nên chúng ta có thể sử dụng các pattern (mẫu) để
hủy tuple đó. Chúng ta sẽ thảo luận nhiều hơn về các pattern trong [Chương
6][ch6]<!-- ignore -->. Trong vòng lặp `for`, chúng ta chỉ định một pattern có `i`
cho chỉ mục trong tuple và `&item` cho byte đơn trong tuple.
Bởi vì chúng tôi nhận được tham chiếu đến phần tử từ `.iter().enumerate()`, nên chúng ta sử dụng
`&` trong pattern.

Bên trong vòng lặp `for`, chúng ta tìm kiếm byte đại diện cho khoảng trắng bằng cách
sử dụng cú pháp ký tự byte. Nếu tìm thấy một khoảng trống, chúng ta sẽ trả lại vị trí.
Ngược lại, chúng tôi trả về độ dài của chuỗi bằng cách sử dụng `s.len()`:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:inside_for}}
```

Bây giờ chúng ta có một cách để tìm ra chỉ số kết thúc của từ đầu tiên trong
chuỗi, nhưng có một vấn đề. Chúng ta đang trả về một `usize`, nhưng nó
chỉ có ý nghĩa trong ngữ cảnh của `&String`. Nói cách khác,
bởi vì đó là một giá trị riêng biệt từ `String`, nên không có gì đảm bảo rằng nó
vẫn sẽ hợp lệ trong tương lai. Hãy xem xét chương trình trong Listing 4-8 
sử dụng hàm `first_word` từ Listing 4-7.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-08/src/main.rs:here}}
```

<span class="caption">Listing 4-8: Storing the result from calling the
`first_word` function and then changing the `String` contents</span>

This program compiles without any errors and would also do so if we used `word`
after calling `s.clear()`. Because `word` isn’t connected to the state of `s`
at all, `word` still contains the value `5`. We could use that value `5` with
the variable `s` to try to extract the first word out, but this would be a bug
because the contents of `s` have changed since we saved `5` in `word`.

Having to worry about the index in `word` getting out of sync with the data in
`s` is tedious and error prone! Managing these indices is even more brittle if
we write a `second_word` function. Its signature would have to look like this:

```rust,ignore
fn second_word(s: &String) -> (usize, usize) {
```

Now we’re tracking a starting *and* an ending index, and we have even more
values that were calculated from data in a particular state but aren’t tied to
that state at all. We have three unrelated variables floating around that
need to be kept in sync.

Luckily, Rust has a solution to this problem: string slices.


<span class="caption">Liệt kê 4-8: Lưu trữ kết quả từ việc gọi hàm
hàm `first_word` rồi thay đổi nội dung `String`</span>

Chương trình này biên dịch mà không có bất kỳ lỗi nào và cũng sẽ vậy nếu chúng ta sử dụng `word`
sau khi gọi `s.clear()`. Bởi vì `word` không được kết nối với trạng thái của `s`
hoàn toàn, `word` vẫn chứa giá trị `5`. Chúng ta có thể sử dụng giá trị `5` đó với
biến `s` để trích xuất từ đầu tiên, nhưng đây sẽ là một lỗi bởi vì nội dung 
của `s` đã thay đổi kể từ khi chúng ta lưu `5` trong `word`.

Việc phải lo lắng về index trong `word` không đồng bộ với dữ liệu trong
`s` thật chán và dễ bị lỗi! Việc quản lý các chỉ số này thậm chí còn khó khăn hơn nếu
chúng tôi viết một hàm `second_word`. Khai báo của nó sẽ phải trông như thế này:

```rust,ignore
fn second_word(s: &String) -> (sử dụng, sử dụng) {
```

Bây giờ chúng ta đang theo dõi chỉ mục bắt đầu *và* kết thúc, và chúng ta thậm chí còn có
các giá trị được tính toán từ dữ liệu ở một trạng thái cụ thể nhưng không bị ràng buộc với
trạng thái đó. Chúng ta có ba biến không liên quan trôi nổi xung quanh 
cần được giữ đồng bộ với nhau.

May mắn thay, Rust có một giải pháp cho vấn đề này: slide.

### String Slices

A *string slice* is a reference to part of a `String`, and it looks like this:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-17-slice/src/main.rs:here}}
```

Rather than a reference to the entire `String`, `hello` is a reference to a
portion of the `String`, specified in the extra `[0..5]` bit. We create slices
using a range within brackets by specifying `[starting_index..ending_index]`,
where `starting_index` is the first position in the slice and `ending_index` is
one more than the last position in the slice. Internally, the slice data
structure stores the starting position and the length of the slice, which
corresponds to `ending_index` minus `starting_index`. So in the case of `let
world = &s[6..11];`, `world` would be a slice that contains a pointer to the
byte at index 6 of `s` with a length value of 5.

Figure 4-6 shows this in a diagram.

<img alt="world containing a pointer to the byte at index 6 of String s and a length 5" src="img/trpl04-06.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-6: String slice referring to part of a
`String`</span>

With Rust’s `..` range syntax, if you want to start at index zero, you can drop
the value before the two periods. In other words, these are equal:

```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

By the same token, if your slice includes the last byte of the `String`, you
can drop the trailing number. That means these are equal:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..];
```

You can also drop both values to take a slice of the entire string. So these
are equal:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
```

> Note: String slice range indices must occur at valid UTF-8 character
> boundaries. If you attempt to create a string slice in the middle of a
> multibyte character, your program will exit with an error. For the purposes
> of introducing string slices, we are assuming ASCII only in this section; a
> more thorough discussion of UTF-8 handling is in the [“Storing UTF-8 Encoded
> Text with Strings”][strings]<!-- ignore --> section of Chapter 8.

With all this information in mind, let’s rewrite `first_word` to return a
slice. The type that signifies “string slice” is written as `&str`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-18-first-word-slice/src/main.rs:here}}
```

We get the index for the end of the word in the same way as we did in Listing
4-7, by looking for the first occurrence of a space. When we find a space, we
return a string slice using the start of the string and the index of the space
as the starting and ending indices.

Now when we call `first_word`, we get back a single value that is tied to the
underlying data. The value is made up of a reference to the starting point of
the slice and the number of elements in the slice.

Returning a slice would also work for a `second_word` function:

```rust,ignore
fn second_word(s: &String) -> &str {
```

We now have a straightforward API that’s much harder to mess up, because the
compiler will ensure the references into the `String` remain valid. Remember
the bug in the program in Listing 4-8, when we got the index to the end of the
first word but then cleared the string so our index was invalid? That code was
logically incorrect but didn’t show any immediate errors. The problems would
show up later if we kept trying to use the first word index with an emptied
string. Slices make this bug impossible and let us know we have a problem with
our code much sooner. Using the slice version of `first_word` will throw a
compile-time error:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/src/main.rs:here}}
```

Here’s the compiler error:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/output.txt}}
```

Recall from the borrowing rules that if we have an immutable reference to
something, we cannot also take a mutable reference. Because `clear` needs to
truncate the `String`, it needs to get a mutable reference. The `println!`
after the call to `clear` uses the reference in `word`, so the immutable
reference must still be active at that point. Rust disallows the mutable
reference in `clear` and the immutable reference in `word` from existing at the
same time, and compilation fails. Not only has Rust made our API easier to use,
but it has also eliminated an entire class of errors at compile time!

#### String Literals Are Slices

Recall that we talked about string literals being stored inside the binary. Now
that we know about slices, we can properly understand string literals:

```rust
let s = "Hello, world!";
```

The type of `s` here is `&str`: it’s a slice pointing to that specific point of
the binary. This is also why string literals are immutable; `&str` is an
immutable reference.

#### String Slices as Parameters

Knowing that you can take slices of literals and `String` values leads us to
one more improvement on `first_word`, and that’s its signature:

```rust,ignore
fn first_word(s: &String) -> &str {
```

A more experienced Rustacean would write the signature shown in Listing 4-9
instead because it allows us to use the same function on both `&String` values
and `&str` values.

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:here}}
```

<span class="caption">Listing 4-9: Improving the `first_word` function by using
a string slice for the type of the `s` parameter</span>

If we have a string slice, we can pass that directly. If we have a `String`, we
can pass a slice of the `String` or a reference to the `String`. This
flexibility takes advantage of *deref coercions*, a feature we will cover in
the [“Implicit Deref Coercions with Functions and
Methods”][deref-coercions]<!--ignore--> section of Chapter 15. Defining a
function to take a string slice instead of a reference to a `String` makes our
API more general and useful without losing any functionality:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:usage}}
```

### Other Slices

String slices, as you might imagine, are specific to strings. But there’s a
more general slice type, too. Consider this array:

```rust
let a = [1, 2, 3, 4, 5];
```

Just as we might want to refer to a part of a string, we might want to refer
to part of an array. We’d do so like this:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

This slice has the type `&[i32]`. It works the same way as string slices do, by
storing a reference to the first element and a length. You’ll use this kind of
slice for all sorts of other collections. We’ll discuss these collections in
detail when we talk about vectors in Chapter 8.

## Summary

The concepts of ownership, borrowing, and slices ensure memory safety in Rust
programs at compile time. The Rust language gives you control over your memory
usage in the same way as other systems programming languages, but having the
owner of data automatically clean up that data when the owner goes out of scope
means you don’t have to write and debug extra code to get this control.

Ownership affects how lots of other parts of Rust work, so we’ll talk about
these concepts further throughout the rest of the book. Let’s move on to
Chapter 5 and look at grouping pieces of data together in a `struct`.

[ch13]: ch13-02-iterators.html
[ch6]: ch06-02-match.html#patterns-that-bind-to-values
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[deref-coercions]: ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods
