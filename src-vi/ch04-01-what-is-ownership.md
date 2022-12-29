<a id="the-stack-and-the-heap"></a>
## Ownership (tính sở hữu) là gì?

*Ownership* là một tập các quy tắc phối hợp với nhau, định hình cách Rust quản lý
bộ nhớ. Tất cả các chương trình đều phải quản lý cách chúng sử dụng bộ nhớ máy
tính khi chạy. Một số ngôn ngữ có bộ dọn rác chạy định kỳ để giải phóng các phần
bộ nhớ không còn được dùng tới; trong các ngôn ngữ khác, chương trình phải xin 
cấp phát hoặc giải phóng bộ nhớ một cách cụ thể. Rust dùng cách tiếp cận thứ ba: 
bộ nhớ được quản lý thông qua một hệ thống sở hữu với một tập các quy tắc sẽ được
kiểm tra bởi trình biên dịch. Nếu bất kỳ quy tắc nào bị vi phạm, chương trình sẽ
không được dịch. Không có bất kỳ tính năng nào của ownership làm chậm chương trình 
của bạn khi chạy.

Vì ownership là một khái niệm mới với nhiều lập trình viên, nó sẽ mất một thời 
gian để làm quen. Tin tốt là càng có nhiều kinh nghiệm với Rust và các quy tắc 
của hệ thống ownership, bạn càng thấy việc lập trình các code an toàn và hiệu quả
dễ dàng hơn. Vậy nên hãy cố lên!

Khi đã hiểu ownership, bạn sẽ có một nền tảng chắc chắn để hiểu các tính năng
vốn làm cho Rust trở nên độc nhất. Trong chương này bạn sẽ học về ownership
thông qua một số ví dụ tập trung vào một cấu trúc dữ liệu rất phổ biến: string.

> ### Stack và Heap
>
> Nhiều ngôn ngữ lập trình không yêu cầu bạn phải nghĩ về stack và heap thường
> xuyên. Nhưng với một ngôn ngữ lập trình hệ thống như Rust, việc một giá trị sẽ
> được lưu trên stack hay trên heap ảnh hưởng tới cách ngôn ngữ xử lý cũng như 
> lý do bạn quyết định. Các phần của ownership sẽ được mô tả trong mối quan hệ 
> với stack và heap ở phần sau của chương này, ở đây chỉ là một giải thích
> ngắn gọn để chuẩn bị.
>
> Cả hai stack và heap đều là các phần của bộ nhớ mà chương trình của bạn có thể
> sử dụng khi chạy, nhưng chúng được tổ chức theo những cách khác nhau. Stack lưu 
> trữ các giá trị theo thứ tự bạn đưa vào và lấy các giá trị ra theo thứ tự ngược
> lại. Ta thường gọi là *last in, first out*. Hãy tưởng tượng một chồng đĩa: khi 
> bạn thêm đĩa, bạn sẽ đặt chúng lên trên cùng, và khi cần lấy một cái đĩa, bạn 
> sẽ lấy cái trên nhất. Bạn sẽ không bỏ vào hoặc lấy ra các đĩa ở giữa hay bên 
> dưới cùng. Thêm dữ liệu vào được gọi là *đẩy (push)* và stack, và thao tác 
> lấy ra được gọi là *lấy ra khỏi stack (pop)*. Tất cả dữ liệu lưu trên stack 
> phải có một kiểu dữ liệu có kích thước cố định cho trước. Dữ liệu có kích thước 
> không thể biết trước khi biên dịch hoặc có thể thay đổi phải được lưu trên heap.
>
> Heap được tổ chức ít quy củ hơn: khi bạn đưa dữ liệu lên heap, bạn yêu cầu một
> không gian trống có kích thước cụ thể. Bộ phân phối bộ nhớ sẽ tìm một phần trống 
> trên heap đủ lớn để chứa, đánh dấu nó đã được dùng, và trả về một *con trỏ (pointer)*,
> chứa địa chỉ của vị trí phần bộ nhớ vừa được cấp phát. Quá trình này được gọi
> là *phân phối bộ nhớ trên heap* và đôi khi được gọi ngắn gọn là *phân phối bộ nhớ*
> (đẩy một giá trị vào stack không được coi là phân phối bộ nhớ). Vì con trỏ chứa 
> địa chỉ vùng nhớ có kích thước cố định và biết trước, nó có thể được lưu trong
> stack, nhưng khi bạn cần lấy dữ liệu thực sự, bạn sẽ cần đi theo địa chỉ chứa
> trong con trỏ. Hãy tưởng tượng khi đến một nhà hàng, bạn cho nhân viên biết
> số người trong nhóm, họ sẽ tìm một bàn trống đủ cho nhóm của bạn và dẫn bạn đến
> đó. Nếu một người trong nhóm đến muộn, họ có thể hỏi nơi bạn ngồi để tìm bạn.
>
> Đẩy một giá trị vào stack nhanh hơn phân phối trên heap vì trình quản lý không
> cần tìm một nơi để lưu dữ liệu; vị trí đó luôn nằm trên đỉnh của stack. Trong khi
> đó, phân phối bộ nhớ trên heap cần nhiều thao tác hơn vì trình quản lý đầu tiên
> phải tìm một không gian trống đủ lớn để chứa dữ liệu, sau đó làm các thao tác để 
> đánh dấu việc sử dụng không gian nhớ đó. 
> 
> Truy cập dữ liệu trên heap cũng chậm hơn trong stack vì bạn phải theo một con trỏ để
> đến đúng nơi. Các bộ xử lý hiện nay sẽ hoạt động nhanh hơn nếu chúng không phải 
> truy cập bộ nhớ nhiều. Tiếp tục với ví dụ ở trên, hãy tưởng tượng một người phục
> vụ ở nhà hàng phải nhận đặt món từ nhiều bàn khác nhau. Cách làm hiệu quả nhất là
> nhận tất cả yêu cầu đặt món từ một bàn trước khi di chuyển đến bàn tiếp theo. Nhận
> món từ bàn A, rồi sang bàn B, sau đó quay lại bàn A, rồi tiếp tục quay lại bàn B 
> có lẽ sẽ chậm hơn nhiều. Theo cùng cách, một bộ xử lý có thể hoàn thành công việc 
> tốt hơn nếu nó làm việc với các dữ liệu nằm gần nhau (giống như trong stack) hơn 
> là khi chúng nằm xa nhau (như với heap).
> 
> Khi code của bạn gọi một hàm, các giá trị truyền vào cho hàm đó (có thể bao gồm 
> cả các con trỏ lên heap) và các biến cục bộ của hàm đều được đẩy vào stack. Khi
> hàm kết thúc, các giá trị đó sẽ được lấy ra khỏi stack.
>
> Theo dõi phần code nào code đang dùng những phần nào của heap, tối thiểu hóa việc
> trùng lắp dữ liệu, và dọn dẹp những dữ liệu nào không được dùng tới trên heap sao
> cho chúng ta không cạn kiệt các tài nguyên bộ nhớ là những vấn đề mà ownership 
> nhắm đến. Một khi đã hiểu về ownership, bạn sẽ không cần nghĩ về stack và heap thường
> xuyên nữa, nhưng biết mục đích chính của ownership là để quản lý bộ nhớ heap có thể 
> giúp giải thích vì sao nó làm việc theo cách mà bạn sẽ thấy. 

### Các quy tắc của Ownership

Đầu tiên, hãy xem qua các quy tắc của ownership. Hãy ghi nhớ các quy tắc này khi
ta đi qua các ví đụ minh họa:

* Mỗi giá trị trong Rust có một chủ sở hữu (*owner*)
* Mỗi thời điểm chỉ có duy nhất một owner.
* Khi owner ra khỏi phạm vi (scope, tầm vực của biến) của nó, giá trị sẽ bị hủy.

### Tầm vực của biến

Giờ ta đã hoàn thành cú pháp cơ bản của Rust, chúng ta sẽ không cần thêm `fn main() {`
vào các ví dụ, do vậy nếu muốn chạy thử hãy tự thêm chúng vào trong hàm `main`. Khi viết 
các ví dụ theo cách ngắn gọn như vậy, chúng ta có thể tập trung vào chi tiết muốn
nói hơn là các đoạn code mẫu.

Ví dụ đầu tiên về ownership, chúng ta sẽ xem qua *tầm vực (scope)* của một số biến.
Một tầm vực là một đoạn trong một chương trình mà trong đó một thành phần nào đó
là hợp lệ. Hãy xem qua ví dụ sau:

```rust
let s = "hello";
```

Biến `s` tham chiếu đến một giá trị chuỗi, giá trị này được hard code vào trong 
phần text của chương trình. Các biến là hợp lệ tại thời điểm chúng được khai báo
cho đến hết *tầm vực* hiện tại. Listing 4-1 trình bày một chương trình với các 
ghi chú chỉ ra nơi biến `s` là hợp lệ.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

<span class="caption">Listing 4-1: Một biến và tầm vực (phạm vi) của nó</span>

Nói cách khác, có hai thời điểm quan trọng ở đây:

* Khi `s` đi vào trong tầm vực, nó trở nên hợp lệ.
* `s` sẽ vẫn hợp lệ cho đến khi nó đi ra khỏi *tầm vực*.

Tại điểm này, mối quan hệ giữa các tầm vực và khi các biến là hợp lệ hoàn toàn tương 
tự trong các ngôn ngữ lập trình khác. Giờ chúng ta sẽ áp dụng điều này với kiểu 
dữ liệu `String` để tìm hiểu sâu hơn.

### Kiểu `String`

Để minh họa các quy tắc của ownership, chúng ta sẽ cần một kiểu dữ liệu phức tạp
hơn những kiểu đã được nói đến trong phần [“Các kiểu dữ liệu”][data-types]<!-- ignore --> 
ở chương 3. Những kiểu được nói đến trong phần đó có kích thước cố định, có thể 
được lưu trong stack và dễ dàng lấy ra khi đi ra khỏi phạm vi của nó, cũng như 
có thể tạo một bản sao mới độc lập với bản gốc nếu một phần khác của chương trình 
muốn đọc giá trị của nó. Nhưng chúng ta muốn xem cách dữ liệu được lưu trên heap 
và khám phá cách Rust biết khi nào cần giải phóng dữ liệu đó, trong trường hợp
này `String` là một ví dụ tuyệt vời.

Chúng ta sẽ tập trung trên các phần của `String` có liên quan đến tính sở hữu (ownership).
Nhưng những phần này cũng có thể áp dụng lên các kiểu dữ liệu phức tạp khác,
bao gồm cả các kiểu trong thư viện chuẩn và các kiểu do bạn tự tạo. Chúng ta sẽ 
nói sâu hơn về `String` trong [Chương 8][ch8]<!-- ignore -->.

We’ve already seen string literals, where a string value is hardcoded into our
program. String literals are convenient, but they aren’t suitable for every
situation in which we may want to use text. One reason is that they’re
immutable. Another is that not every string value can be known when we write
our code: for example, what if we want to take user input and store it? For
these situations, Rust has a second string type, `String`. This type manages
data allocated on the heap and as such is able to store an amount of text that
is unknown to us at compile time. You can create a `String` from a string
literal using the `from` function, like so:

```rust
let s = String::from("hello");
```

The double colon `::` operator allows us to namespace this particular `from`
function under the `String` type rather than using some sort of name like
`string_from`. We’ll discuss this syntax more in the [“Method
Syntax”][method-syntax]<!-- ignore --> section of Chapter 5, and when we talk
about namespacing with modules in [“Paths for Referring to an Item in the
Module Tree”][paths-module-tree]<!-- ignore --> in Chapter 7.

This kind of string *can* be mutated:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

So, what’s the difference here? Why can `String` be mutated but literals
cannot? The difference is in how these two types deal with memory.

### Memory and Allocation

In the case of a string literal, we know the contents at compile time, so the
text is hardcoded directly into the final executable. This is why string
literals are fast and efficient. But these properties only come from the string
literal’s immutability. Unfortunately, we can’t put a blob of memory into the
binary for each piece of text whose size is unknown at compile time and whose
size might change while running the program.

With the `String` type, in order to support a mutable, growable piece of text,
we need to allocate an amount of memory on the heap, unknown at compile time,
to hold the contents. This means:

* The memory must be requested from the memory allocator at runtime.
* We need a way of returning this memory to the allocator when we’re done with
  our `String`.

That first part is done by us: when we call `String::from`, its implementation
requests the memory it needs. This is pretty much universal in programming
languages.

However, the second part is different. In languages with a *garbage collector
(GC)*, the GC keeps track of and cleans up memory that isn’t being used
anymore, and we don’t need to think about it. In most languages without a GC,
it’s our responsibility to identify when memory is no longer being used and to
call code to explicitly free it, just as we did to request it. Doing this
correctly has historically been a difficult programming problem. If we forget,
we’ll waste memory. If we do it too early, we’ll have an invalid variable. If
we do it twice, that’s a bug too. We need to pair exactly one `allocate` with
exactly one `free`.

Rust takes a different path: the memory is automatically returned once the
variable that owns it goes out of scope. Here’s a version of our scope example
from Listing 4-1 using a `String` instead of a string literal:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

There is a natural point at which we can return the memory our `String` needs
to the allocator: when `s` goes out of scope. When a variable goes out of
scope, Rust calls a special function for us. This function is called
[`drop`][drop]<!-- ignore -->, and it’s where the author of `String` can put
the code to return the memory. Rust calls `drop` automatically at the closing
curly bracket.

> Note: In C++, this pattern of deallocating resources at the end of an item’s
> lifetime is sometimes called *Resource Acquisition Is Initialization (RAII)*.
> The `drop` function in Rust will be familiar to you if you’ve used RAII
> patterns.

This pattern has a profound impact on the way Rust code is written. It may seem
simple right now, but the behavior of code can be unexpected in more
complicated situations when we want to have multiple variables use the data
we’ve allocated on the heap. Let’s explore some of those situations now.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Variables and Data Interacting with Move

Multiple variables can interact with the same data in different ways in Rust.
Let’s look at an example using an integer in Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Listing 4-2: Assigning the integer value of variable `x`
to `y`</span>

We can probably guess what this is doing: “bind the value `5` to `x`; then make
a copy of the value in `x` and bind it to `y`.” We now have two variables, `x`
and `y`, and both equal `5`. This is indeed what is happening, because integers
are simple values with a known, fixed size, and these two `5` values are pushed
onto the stack.

Now let’s look at the `String` version:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

This looks very similar, so we might assume that the way it works would be the
same: that is, the second line would make a copy of the value in `s1` and bind
it to `s2`. But this isn’t quite what happens.

Take a look at Figure 4-1 to see what is happening to `String` under the
covers. A `String` is made up of three parts, shown on the left: a pointer to
the memory that holds the contents of the string, a length, and a capacity.
This group of data is stored on the stack. On the right is the memory on the
heap that holds the contents.

<img alt="Two tables: the first table contains the representation of s1 on the
stack, consisting of its length (5), capacity (5), and a pointer to the first
value in the second table. The second table contains the representation of the
string data on the heap, byte by byte." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Representation in memory of a `String`
holding the value `"hello"` bound to `s1`</span>

The length is how much memory, in bytes, the contents of the `String` are
currently using. The capacity is the total amount of memory, in bytes, that the
`String` has received from the allocator. The difference between length and
capacity matters, but not in this context, so for now, it’s fine to ignore the
capacity.

When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the
pointer, the length, and the capacity that are on the stack. We do not copy the
data on the heap that the pointer refers to. In other words, the data
representation in memory looks like Figure 4-2.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap."
src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-2: Representation in memory of the variable `s2`
that has a copy of the pointer, length, and capacity of `s1`</span>

The representation does *not* look like Figure 4-3, which is what memory would
look like if Rust instead copied the heap data as well. If Rust did this, the
operation `s2 = s1` could be very expensive in terms of runtime performance if
the data on the heap were large.

<img alt="Four tables: two tables representing the stack data for s1 and s2,
and each points to its own copy of string data on the heap."
src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-3: Another possibility for what `s2 = s1` might
do if Rust copied the heap data as well</span>

Earlier, we said that when a variable goes out of scope, Rust automatically
calls the `drop` function and cleans up the heap memory for that variable. But
Figure 4-2 shows both data pointers pointing to the same location. This is a
problem: when `s2` and `s1` go out of scope, they will both try to free the
same memory. This is known as a *double free* error and is one of the memory
safety bugs we mentioned previously. Freeing memory twice can lead to memory
corruption, which can potentially lead to security vulnerabilities.

To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as
no longer valid. Therefore, Rust doesn’t need to free anything when `s1` goes
out of scope. Check out what happens when you try to use `s1` after `s2` is
created; it won’t work:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

You’ll get an error like this because Rust prevents you from using the
invalidated reference:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/output.txt}}
```

If you’ve heard the terms *shallow copy* and *deep copy* while working with
other languages, the concept of copying the pointer, length, and capacity
without copying the data probably sounds like making a shallow copy. But
because Rust also invalidates the first variable, instead of being called a
shallow copy, it’s known as a *move*. In this example, we would say that `s1`
was *moved* into `s2`. So, what actually happens is shown in Figure 4-4.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap.
Table s1 is grayed out be-cause s1 is no longer valid; only s2 can be used to
access the heap data." src="img/trpl04-04.svg" class="center" style="width:
50%;" />

<span class="caption">Figure 4-4: Representation in memory after `s1` has been
invalidated</span>

That solves our problem! With only `s2` valid, when it goes out of scope it
alone will free the memory, and we’re done.

In addition, there’s a design choice that’s implied by this: Rust will never
automatically create “deep” copies of your data. Therefore, any *automatic*
copying can be assumed to be inexpensive in terms of runtime performance.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-clone"></a>

#### Variables and Data Interacting with Clone

If we *do* want to deeply copy the heap data of the `String`, not just the
stack data, we can use a common method called `clone`. We’ll discuss method
syntax in Chapter 5, but because methods are a common feature in many
programming languages, you’ve probably seen them before.

Here’s an example of the `clone` method in action:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

This works just fine and explicitly produces the behavior shown in Figure 4-3,
where the heap data *does* get copied.

When you see a call to `clone`, you know that some arbitrary code is being
executed and that code may be expensive. It’s a visual indicator that something
different is going on.

#### Stack-Only Data: Copy

There’s another wrinkle we haven’t talked about yet. This code using
integers—part of which was shown in Listing 4-2—works and is valid:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

But this code seems to contradict what we just learned: we don’t have a call to
`clone`, but `x` is still valid and wasn’t moved into `y`.

The reason is that types such as integers that have a known size at compile
time are stored entirely on the stack, so copies of the actual values are quick
to make. That means there’s no reason we would want to prevent `x` from being
valid after we create the variable `y`. In other words, there’s no difference
between deep and shallow copying here, so calling `clone` wouldn’t do anything
different from the usual shallow copying, and we can leave it out.

Rust has a special annotation called the `Copy` trait that we can place on
types that are stored on the stack, as integers are (we’ll talk more about
traits in [Chapter 10][traits]<!-- ignore -->). If a type implements the `Copy`
trait, variables that use it do not move, but rather are trivially copied,
making them still valid after assignment to another variable.

Rust won’t let us annotate a type with `Copy` if the type, or any of its parts,
has implemented the `Drop` trait. If the type needs something special to happen
when the value goes out of scope and we add the `Copy` annotation to that type,
we’ll get a compile-time error. To learn about how to add the `Copy` annotation
to your type to implement the trait, see [“Derivable
Traits”][derivable-traits]<!-- ignore --> in Appendix C.

So, what types implement the `Copy` trait? You can check the documentation for
the given type to be sure, but as a general rule, any group of simple scalar
values can implement `Copy`, and nothing that requires allocation or is some
form of resource can implement `Copy`. Here are some of the types that
implement `Copy`:

* All the integer types, such as `u32`.
* The Boolean type, `bool`, with values `true` and `false`.
* All the floating-point types, such as `f64`.
* The character type, `char`.
* Tuples, if they only contain types that also implement `Copy`. For example,
  `(i32, i32)` implements `Copy`, but `(i32, String)` does not.

### Ownership and Functions

The mechanics of passing a value to a function are similar to those when
assigning a value to a variable. Passing a variable to a function will move or
copy, just as assignment does. Listing 4-3 has an example with some annotations
showing where variables go into and out of scope.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

<span class="caption">Listing 4-3: Functions with ownership and scope
annotated</span>

If we tried to use `s` after the call to `takes_ownership`, Rust would throw a
compile-time error. These static checks protect us from mistakes. Try adding
code to `main` that uses `s` and `x` to see where you can use them and where
the ownership rules prevent you from doing so.

### Return Values and Scope

Returning values can also transfer ownership. Listing 4-4 shows an example of a
function that returns some value, with similar annotations as those in Listing
4-3.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

<span class="caption">Listing 4-4: Transferring ownership of return
values</span>

The ownership of a variable follows the same pattern every time: assigning a
value to another variable moves it. When a variable that includes data on the
heap goes out of scope, the value will be cleaned up by `drop` unless ownership
of the data has been moved to another variable.

While this works, taking ownership and then returning ownership with every
function is a bit tedious. What if we want to let a function use a value but
not take ownership? It’s quite annoying that anything we pass in also needs to
be passed back if we want to use it again, in addition to any data resulting
from the body of the function that we might want to return as well.

Rust does let us return multiple values using a tuple, as shown in Listing 4-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Listing 4-5: Returning ownership of parameters</span>

But this is too much ceremony and a lot of work for a concept that should be
common. Luckily for us, Rust has a feature for using a value without
transferring ownership, called *references*.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
