## Các khối điều khiển

Khả năng chạy một số đoạn lệnh dựa trên một điều kiện nào đó, hoặc chạy lặp lại một
lệnh khi một điều kiện nào đó là đúng, là các khối điều khiển cơ bản có trong hầu 
hết các ngôn ngữ lập trình. Khối điều khiển phổ biến nhất cho phép bạn kiểm soát việc
thực thi các đoạn code trong Rust là các biểu thức `if` và các lệnh lặp.

### Biểu thức `if` 

Một biểu thức `if` cho phép bạn rẽ nhánh thực thi dựa trên các điều kiện nào đó.
Bạn cung cấp một điều kiện và phát biểu: "Nếu điều kiện này đúng, hãy chạy đoạn lệnh
này. Nếu không đạt, đừng chạy nó".

Tạo một dự án mới được gọi là *branches* trong thư mục *projects* để khảo sát biểu
thức `if`. Trong file *src/main.rs*, hãy nhập vào nội dung sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

Tất cả các phát biểu `if` sẽ bắt đầu bằng từ khóa `if`, theo sau bởi một điều kiện.
Trong trường hợp này, điều kiện là kiểm tra xem liệu biến `number` có giá trị nhỏ
hơn 5 hay không. Chúng ta đặt khối lệnh để thực thi nếu điều kiện là đúng ngay sau
dấu ngoặc đóng của điều kiện. Các khối lệnh kết hợp với `if` đôi khi được gọi là 
*arm*, giống như *arms* trong phát biểu `match` mà ta đã thảo luận trong phần 
[“Comparing the Guess to the Secret Number”][comparing-the-guess-to-the-secret-number]<!--
ignore --> section of Chapter 2.

Chúng ta cũng có thể tùy chọn thêm một phát biểu `else`, giống như chúng ta đã
làm ở đây, để cung cấp một đoạn code mà sẽ được thực thi nếu điều kiện trả về
giá trị false. Nếu bạn không cung cấp `else`, chương trình sẽ chỉ đơn giản bỏ qua
khối lệnh `if` và tiếp tục thực thi các lệnh tiếp sau đó.

Thử chạy đoạn lệnh này, bạn sẽ thấy kết xuất như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

Hãy thử thay đổi `number` sang một giá trị làm cho điều kiện trả về kết quả false
để xem điều gì xảy ra:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

Chạy lại chương trình, và xem kết xuất:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

Một điểm quan trọng cần lưu ý là điều kiện trong đoạn code này bắt buộc *phải* có 
kiểu `bool`. Nếu điều kiện này không phải là bool, bạn sẽ nhận một lỗi. Ví dụ, thử 
chạy đoạn lệnh sau:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

Trong trường hợp này biểu thức trong `if` trả về giá trị `3`, và Rust phát ra một
lỗi:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

Lỗi này chỉ ra Rust cần một `bool` nhưng lại nhận được một integer. Không như
các ngôn ngữ như Ruby hay JavaScript, Rust không tự động thử chuyển các giá trị
không phải boolean sang boolean. Nếu bạn muốn đoạn `if` chỉ chạy khi một số bằng
với `0`, bạn có thể viết lại như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

Chạy đoạn code này sẽ trả về kết xuất sau: `number was something other than zero`.

#### Xử lý nhiều điều kiện với `else if`

Bạn có thể dùng nhiều điều kiện khác nhau bằng cách kết hợp `if` với `else` trong
một biểu thức `else if`. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

Chương trình này có bốn nhánh có thể thực thi. Sau khi chạy bạn sẽ thấy kết xuất sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

Khi chạy chương trình này, nó sẽ kiểm tra lần lượt mỗi biểu thức `if` và thực
thi thân `if` đầu tiên mà biểu thức trả về `true`. Lưu ý rằng tuy 6 chia hết cho 2, 
chúng ta vẫn không thấy câu `number is divisible by 2` được in ra, cũng như không
thấy câu `number is not divisible by 4, 3, or 2` trong khối `else`. Đó là vì Rust 
chỉ thực thi đoạn lệnh trong thân `if` đầu tiên mà nó thấy trả về true, và một khi
tìm thấy, nó sẽ thậm chí không kiểm tra các biểu thức phía sau.

Sử dụng quá nhiều `else if` làm cho code của bạn lộn xộn, do vậy nếu bạn có nhiều 
hơn một, bạn có thể sẽ cần refactor code. Chương 6 mô tả một cấu trúc rẽ nhánh mạnh
mẽ trong Rust được gọi là `match` phù hợp với trường hợp này.

#### Sử dụng `if` bên trong phát biểu `let`

Vì `if` là một biểu thức, vậy nên ta có thể dùng nó bên phải của một phát biểu `let`
để gán giá trị trả về cho một biến, như trong Listing 3-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

<span class="caption">Listing 3-2: Gán kết quả của một biểu thức `if` vào một biến</span>

Biến `number` sẽ được gán một giá trị dựa trên kết quả của một biểu thức `if`. Chạy 
đoạn code sau để xem điều gì xảy ra:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

Hãy nhớ là các khối lệnh sẽ được định giá trị bằng với giá trị của biểu thức cuối
cùng bên trong nó, và bản thân các con số cũng là các biểu thức. Trong trường hợp này,
giá trị của toàn bộ biểu thức `if` phụ thuộc vào nhánh nào được thực thi. Có nghĩa là
các nhánh của `if` phải có cùng kiểu; trong Listing 3-2, kết quả của cả hai nhánh 
của `if` đều có kiểu số nguyên `i32`. Nếu các kiểu không khớp nhau, như trong ví dụ 
dưới đây, chúng ta sẽ nhận về một lỗi.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

Khi chúng ta thử dịch đoạn code này, chúng ta sẽ nhận về một lỗi. Các nhánh `if`
và `else` có các kiểu không tương thích, và Rust chỉ ra chính xác vị trí nơi phát 
sinh lỗi trong chương trình:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```
Biểu thức trong khối `if` trả về một giá trị integer, trong khi biểu thức trong khối `else` 
trả về một string. Điều này không thể hoạt động được vì các biến chỉ có thể có một kiểu
duy nhất, và Rust cần biết kiểu của biến `number` là gì ngay khi dịch. Việc biết 
kiểu của biến `number` cho phép trình dịch xác định tính hợp lệ về kiểu bất cứ khi nào
ta truy xuất đến nó. Rust sẽ không thể làm được điều này nếu nó chỉ có thể xác định kiểu
của `number` vào lúc chạy chương trình; trình dịch có lẽ sẽ phức tạp hơn nhiều cũng như
khó đảm bảo về đoạn code hơn nếu nó phải lưu giữ thông tin về tất cả các kiểu dữ liệu 
`giả tưởng` cho bất kỳ biến nào.
### Repetition with Loops

It’s often useful to execute a block of code more than once. For this task,
Rust provides several *loops*, which will run through the code inside the loop
body to the end and then start immediately back at the beginning. To
experiment with loops, let’s make a new project called *loops*.

Rust has three kinds of loops: `loop`, `while`, and `for`. Let’s try each one.

#### Repeating Code with `loop`

The `loop` keyword tells Rust to execute a block of code over and over again
forever or until you explicitly tell it to stop.

As an example, change the *src/main.rs* file in your *loops* directory to look
like this:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

When we run this program, we’ll see `again!` printed over and over continuously
until we stop the program manually. Most terminals support the keyboard shortcut
<span class="keystroke">ctrl-c</span> to interrupt a program that is stuck in
a continual loop. Give it a try:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

The symbol `^C` represents where you pressed <span class="keystroke">ctrl-c
</span>. You may or may not see the word `again!` printed after the `^C`,
depending on where the code was in the loop when it received the interrupt
signal.

Fortunately, Rust also provides a way to break out of a loop using code. You
can place the `break` keyword within the loop to tell the program when to stop
executing the loop. Recall that we did this in the guessing game in the
[“Quitting After a Correct Guess”][quitting-after-a-correct-guess]<!-- ignore
--> section of Chapter 2 to exit the program when the user won the game by
guessing the correct number.

We also used `continue` in the guessing game, which in a loop tells the program
to skip over any remaining code in this iteration of the loop and go to the
next iteration.

#### Returning Values from Loops

One of the uses of a `loop` is to retry an operation you know might fail, such
as checking whether a thread has completed its job. You might also need to pass
the result of that operation out of the loop to the rest of your code. To do
this, you can add the value you want returned after the `break` expression you
use to stop the loop; that value will be returned out of the loop so you can
use it, as shown here:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```

Before the loop, we declare a variable named `counter` and initialize it to
`0`. Then we declare a variable named `result` to hold the value returned from
the loop. On every iteration of the loop, we add `1` to the `counter` variable,
and then check whether the counter is equal to `10`. When it is, we use the
`break` keyword with the value `counter * 2`. After the loop, we use a
semicolon to end the statement that assigns the value to `result`. Finally, we
print the value in `result`, which in this case is 20.

#### Loop Labels to Disambiguate Between Multiple Loops

If you have loops within loops, `break` and `continue` apply to the innermost
loop at that point. You can optionally specify a *loop label* on a loop that we
can then use with `break` or `continue` to specify that those keywords apply to
the labeled loop instead of the innermost loop. Loop labels must begin with a
single quote. Here’s an example with two nested loops:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

The outer loop has the label `'counting_up`, and it will count up from 0 to 2.
The inner loop without a label counts down from 10 to 9. The first `break` that
doesn’t specify a label will exit the inner loop only. The `break
'counting_up;` statement will exit the outer loop. This code prints:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### Conditional Loops with `while`

A program will often need to evaluate a condition within a loop. While the
condition is true, the loop runs. When the condition ceases to be true, the
program calls `break`, stopping the loop. It’s possible to implement behavior
like this using a combination of `loop`, `if`, `else`, and `break`; you could
try that now in a program, if you’d like. However, this pattern is so common
that Rust has a built-in language construct for it, called a `while` loop. In
Listing 3-3, we use `while` to loop the program three times, counting down each
time, and then, after the loop, print a message and exit.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

<span class="caption">Listing 3-3: Using a `while` loop to run code while a
condition holds true</span>

This construct eliminates a lot of nesting that would be necessary if you used
`loop`, `if`, `else`, and `break`, and it’s clearer. While a condition holds
true, the code runs; otherwise, it exits the loop.

#### Looping Through a Collection with `for`

You can choose to use the `while` construct to loop over the elements of a
collection, such as an array. For example, the loop in Listing 3-4 prints each
element in the array `a`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

<span class="caption">Listing 3-4: Looping through each element of a collection
using a `while` loop</span>

Here, the code counts up through the elements in the array. It starts at index
`0`, and then loops until it reaches the final index in the array (that is,
when `index < 5` is no longer true). Running this code will print every element
in the array:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

All five array values appear in the terminal, as expected. Even though `index`
will reach a value of `5` at some point, the loop stops executing before trying
to fetch a sixth value from the array.

However, this approach is error prone; we could cause the program to panic if
the index value or test condition are incorrect. For example, if you changed
the definition of the `a` array to have four elements but forgot to update the
condition to `while index < 4`, the code would panic. It’s also slow, because
the compiler adds runtime code to perform the conditional check of whether the
index is within the bounds of the array on every iteration through the loop.

As a more concise alternative, you can use a `for` loop and execute some code
for each item in a collection. A `for` loop looks like the code in Listing 3-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

<span class="caption">Listing 3-5: Looping through each element of a collection
using a `for` loop</span>

When we run this code, we’ll see the same output as in Listing 3-4. More
importantly, we’ve now increased the safety of the code and eliminated the
chance of bugs that might result from going beyond the end of the array or not
going far enough and missing some items.

Using the `for` loop, you wouldn’t need to remember to change any other code if
you changed the number of values in the array, as you would with the method
used in Listing 3-4.

The safety and conciseness of `for` loops make them the most commonly used loop
construct in Rust. Even in situations in which you want to run some code a
certain number of times, as in the countdown example that used a `while` loop
in Listing 3-3, most Rustaceans would use a `for` loop. The way to do that
would be to use a `Range`, provided by the standard library, which generates
all numbers in sequence starting from one number and ending before another
number.

Here’s what the countdown would look like using a `for` loop and another method
we’ve not yet talked about, `rev`, to reverse the range:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

This code is a bit nicer, isn’t it?

## Summary

You made it! That was a sizable chapter: you learned about variables, scalar
and compound data types, functions, comments, `if` expressions, and loops!
To practice with the concepts discussed in this chapter, try building
programs to do the following:

* Convert temperatures between Fahrenheit and Celsius.
* Generate the nth Fibonacci number.
* Print the lyrics to the Christmas carol “The Twelve Days of Christmas,”
  taking advantage of the repetition in the song.

When you’re ready to move on, we’ll talk about a concept in Rust that *doesn’t*
commonly exist in other programming languages: ownership.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[quitting-after-a-correct-guess]:
ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
