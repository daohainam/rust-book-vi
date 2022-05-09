## Variables and Mutability

Như đã nhắc đến trong phần [“Storing Values with
Variables”][storing-values-with-variables]<!-- ignore -->, mặc nhiên các biến
là không thể thay đổi giá trị (immutable). Đây là một trong nhiều cách Rust mang đến để giúp viết 
ra những đoạn code an toàn và dễ dàng hoạt động song song. Tuy nhiên, bạn vẫn có các 
tùy chọn để cho phép thay đổi giá trị các biến. Hãy cùng chúng tôi xem qua như thế 
nào và vì sao Rust khuyến khích việc chặn thay đổi giá trị biến, và vì sao đôi khi 
chúng ta vẫn phải bỏ chặn.

Khi một biến là bất biến (immutable), một khi đã gán cho nó một giá trị, bạn sẽ không
thể thay đổi giá trị đó nữa. Để minh họa điều này, hãy cùng tạo ra một dự án mới
được gọi là *variables* trong thư mục *projects* bằng cách chạy câu lệnh `cargo new variables`.

Sau đó, trong thư mục *variables*, mở file *src/main.rs* và thay thế code của nó với
đoạn sau. Đoạn code này sẽ không thể dịch được ngay, trước tiên ta sẽ cần xem qua 
lỗi bất biến (immutability error).

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

Lưu lại và chạy chương trình dùng `cargo run`. Bạn sẽ phải thấy một thông báo lỗi, 
như được hiển thị dưới đây:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

Ví dụ này cho thấy cách trình biên dịch giúp bạn tìm lỗi trong chương trình.
Các lỗi biên dịch có thể làm nản lòng, nhưng chúng thực sự mang ý nghĩa là chương
trình của bạn không an toàn khi thực hiện cái mà bạn yêu cầu nó thực hiện.
Chúng *không* có nghĩa bạn không phải là một lập trình viên tốt! Những Rustaceans
nhiều kinh nghiệm vẫn gặp các lỗi biên dịch.

Thông báo lỗi chỉ ra điều gây ra lỗi là do bạn `` cannot assign twice to immutable variable `x` ``, 
bởi vì bạn đã thử gán giá trị cho biến immutable `x` thêm một lần nữa.

Việc nhận được các lỗi biên dịch khi ta cố gắng thay đổi giá trị của một biến được 
chỉ định là bất biến là rất quan trọng, nó là một trong những nguyên nhân hàng 
đầu dẫn đến bug. Nếu một phần trong chương trình cho là biến đó sẽ không thay đổi
nhưng ở một phần khác lại gán một giá trị mới cho nó, rất có thể phần đầu tiên sẽ
không còn hoạt động như nó được thiết kế. Nguyên nhân gây lỗi này đôi khi rất khó 
dò ra, đặc biệt nếu phần code thay đổi giá trị của biến chỉ *đôi khi* được thực hiện.
Trình dịch Rust bảo đảm khi bạn đã phát biểu rằng một biến sẽ không thể thay đổi, nó 
sẽ thực sự không thay đổi, và bạn không cần phải tự kiểm soát việc đó. Và như vậy 
code của bạn sẽ dễ lý giải hơn.

But mutability can be very useful, and can make code more convenient to write.
Variables are immutable only by default; as you did in Chapter 2, you can make
them mutable by adding `mut` in front of the variable name. Adding `mut` also
conveys intent to future readers of the code by indicating that other parts of
the code will be changing this variable’s value.

Lấy ví dụ, hãy thay đổi *src/main.rs* thành như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

Giờ chạy chương trình, bạn sẽ nhận được nội dung sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

Chúng ta được phép thay đổi giá trị gán cho `x` từ `5` thành `6` khi dùng `mut`.
Hơn hết, việc quyết định liệu có cho phép thay đổi hay không là do bạn và phụ thuộc 
vào việc bạn nghĩ cách nào là rõ ràng nhất trong một trường hợp cụ thể.

### Hằng

Like immutable variables, *constants* are values that are bound to a name and
are not allowed to change, but there are a few differences between constants
and variables.

First, you aren’t allowed to use `mut` with constants. Constants aren’t just
immutable by default—they’re always immutable. You declare constants using the
`const` keyword instead of the `let` keyword, and the type of the value *must*
be annotated. We’re about to cover types and type annotations in the next
section, [“Data Types,”][data-types]<!-- ignore --> so don’t worry about the
details right now. Just know that you must always annotate the type.

Constants can be declared in any scope, including the global scope, which makes
them useful for values that many parts of code need to know about.

The last difference is that constants may be set only to a constant expression,
not the result of a value that could only be computed at runtime.

Here’s an example of a constant declaration:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

The constant’s name is `THREE_HOURS_IN_SECONDS` and its value is set to the
result of multiplying 60 (the number of seconds in a minute) by 60 (the number
of minutes in an hour) by 3 (the number of hours we want to count in this
program). Rust’s naming convention for constants is to use all uppercase with
underscores between words. The compiler is able to evaluate a limited set of
operations at compile time, which lets us choose to write out this value in a
way that’s easier to understand and verify, rather than setting this constant
to the value 10,800. See the [Rust Reference’s section on constant
evaluation][const-eval] for more information on what operations can be used
when declaring constants.

Constants are valid for the entire time a program runs, within the scope they
were declared in. This property makes constants useful for values in your
application domain that multiple parts of the program might need to know about,
such as the maximum number of points any player of a game is allowed to earn or
the speed of light.

Naming hardcoded values used throughout your program as constants is useful in
conveying the meaning of that value to future maintainers of the code. It also
helps to have only one place in your code you would need to change if the
hardcoded value needed to be updated in the future.

### Shadowing

As you saw in the guessing game tutorial in [Chapter
2][comparing-the-guess-to-the-secret-number]<!-- ignore -->, you can declare a
new variable with the same name as a previous variable. Rustaceans say that the
first variable is *shadowed* by the second, which means that the second
variable is what the compiler will see when you use the name of the variable.
In effect, the second variable overshadows the first, taking any uses of the
variable name to itself until either it itself is shadowed or the scope ends.
We can shadow a variable by using the same variable’s name and repeating the
use of the `let` keyword as follows:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

This program first binds `x` to a value of `5`. Then it creates a new variable
`x` by repeating `let x =`, taking the original value and adding `1` so the
value of `x` is then `6`. Then, within an inner scope created with the curly
brackets, the third `let` statement also shadows `x` and creates a new
variable, multiplying the previous value by `2` to give `x` a value of `12`.
When that scope is over, the inner shadowing ends and `x` returns to being `6`.
When we run this program, it will output the following:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing is different from marking a variable as `mut`, because we’ll get a
compile-time error if we accidentally try to reassign to this variable without
using the `let` keyword. By using `let`, we can perform a few transformations
on a value but have the variable be immutable after those transformations have
been completed.

The other difference between `mut` and shadowing is that because we’re
effectively creating a new variable when we use the `let` keyword again, we can
change the type of the value but reuse the same name. For example, say our
program asks a user to show how many spaces they want between some text by
inputting space characters, and then we want to store that input as a number:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

The first `spaces` variable is a string type and the second `spaces` variable
is a number type. Shadowing thus spares us from having to come up with
different names, such as `spaces_str` and `spaces_num`; instead, we can reuse
the simpler `spaces` name. However, if we try to use `mut` for this, as shown
here, we’ll get a compile-time error:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

The error says we’re not allowed to mutate a variable’s type:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

Now that we’ve explored how variables work, let’s look at more data types they
can have.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html
