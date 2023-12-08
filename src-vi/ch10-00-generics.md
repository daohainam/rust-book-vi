# Các Kiểu Generic, Traits, và Lifetimes

Mọi ngôn ngữ lập trình đều có các công cụ để xử lý một cách hiệu quả việc 
trùng lặp các khái niệm. Trong Rust, một trong những công cụ như vậy là *generics*: 
các đại diện trừu tượng cho các kiểu cụ thể hoặc các thuộc tính khác. Chúng ta 
có thể biểu diễn hành vi của generics hoặc cách chúng liên quan đến các generics 
khác mà không cần biết điều gì sẽ thay thế chúng khi biên dịch và chạy code.

Các hàm có thể chấp nhận tham số của một loại generic nào đó, thay vì một 
loại cụ thể như i32 hoặc String, giống như cách một hàm chấp nhận tham số 
với giá trị không xác định để chạy cùng code trên nhiều giá trị cụ thể khác nhau. 
Trên thực tế, chúng ta đã sử dụng generics trong Chương 6 với `Option<T>`, 
Chương 8 với `Vec<T>` và `HashMap<K, V>`, và Chương 9 với `Result<T, E>`. Trong 
chương này, bạn sẽ khám phá cách định nghĩa các kiểu, hàm, và phương thức của 
riêng bạn với generics!

Đầu tiên, chúng ta sẽ xem xét cách trích xuất một hàm để giảm sự trùng lặp code. 
Sau đó, chúng ta sẽ sử dụng cùng một kỹ thuật để tạo ra một hàm generic từ hai 
hàm khác nhau chỉ ở các kiểu tham số của chúng. Chúng ta cũng sẽ giải thích 
cách sử dụng generic types trong định nghĩa struct và enum.

Sau đó, bạn sẽ tìm hiểu cách sử dụng *traits* để định nghĩa hành vi một 
cách generic. Bạn có thể kết hợp *traits* với generic types để ràng buộc một kiểu 
generic chỉ chấp nhận những kiểu có một hành vi cụ thể, chứ không phải chỉ là bất kỳ kiểu nào.

Cuối cùng, chúng ta sẽ thảo luận về lifetimes: một dạng của generics cung 
cấp thông tin cho trình biên dịch về cách các tham chiếu liên quan đến nhau. 
Lifetimes cho phép chúng ta cung cấp đủ thông tin cho trình biên dịch về giá 
trị được mượn để đảm bảo rằng tham chiếu sẽ hợp lệ trong nhiều tình huống hơn 
so với việc không có sự giúp đỡ của chúng ta.

## Loại bỏ Sự Trùng Lặp bằng Cách Trích Xuất Một Hàm

Generics cho phép chúng ta thay thế các kiểu cụ thể bằng một địa chỉ giữ chỗ 
đại diện cho nhiều kiểu để loại bỏ sự trùng lặp code. Trước khi nhảy vào cú pháp 
generics, chúng ta hãy xem cách loại bỏ sự trùng lặp một cách không liên quan đến 
generic types bằng cách trích xuất một hàm thay thế các giá trị cụ thể bằng một 
địa chỉ giữ chỗ đại diện cho nhiều giá trị. Sau đó, chúng ta sẽ áp dụng cùng một 
kỹ thuật để trích xuất một hàm generic! Bằng cách xem xét cách nhận diện code trùng 
lặp có thể trích xuất thành một hàm, bạn sẽ bắt đầu nhận ra code trùng lặp có thể 
sử dụng generics.

Chúng ta bắt đầu với một chương trình ngắn trong Listing 10-1 để tìm số lớn 
nhất trong một danh sách.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-01/src/main.rs:here}}
```

<span class="caption">Listing 10-1: Tìm số lớn nhất trong một danh sách các số</span>

Chúng ta lưu trữ một danh sách các số nguyên trong biến `number_list` và đặt một tham 
chiếu đến số đầu tiên trong danh sách vào một biến có tên là `largest`. Sau đó, chúng 
ta lặp qua tất cả các số trong danh sách, và nếu số hiện tại lớn hơn số được lưu 
trữ trong `largest`, thay thế tham chiếu trong biến đó. Tuy nhiên, nếu số hiện tại 
nhỏ hơn hoặc bằng số lớn nhất đã thấy cho đến nay, biến không thay đổi và code di 
chuyển đến số tiếp theo trong danh sách. Sau khi xem xét tất cả các số trong danh sách, 
`largest` nên tham chiếu đến số lớn nhất, trong trường hợp này là 100.

Bây giờ, chúng ta đã được giao nhiệm vụ tìm số lớn nhất trong hai danh sách khác nhau 
của các số. Để làm điều này, chúng ta có thể chọn sao chép code trong Listing 10-1 
và sử dụng cùng một logic ở hai nơi khác nhau trong chương trình, như được thể 
hiện trong Listing 10-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-02/src/main.rs}}
```

<span class="caption">Listing 10-2: Code tìm số lớn nhất trong hai
danh sách số</span>

Mặc dù code này hoạt động, nhưng sao chép code là công việc nhàm chán và dễ gây lỗi. 
Chúng ta cũng phải nhớ cập nhật code ở nhiều nơi khi chúng ta muốn thay đổi nó.

Để loại bỏ sự trùng lặp này, chúng ta sẽ tạo ra một sự trừu tượng bằng
cách định nghĩa một hàm hoạt động trên bất kỳ danh sách số nguyên nào 
được truyền vào một tham số. Giải pháp này làm cho code của chúng ta rõ ràng 
hơn và cho phép chúng ta diễn đạt khái niệm tìm số lớn nhất trong một danh 
sách một cách trừu tượng.

Trong Listing 10-3, chúng ta trích xuất code tìm số lớn nhất vào một hàm có tên 
là `largest`. Sau đó, chúng ta gọi hàm để tìm số lớn nhất trong hai danh sách 
từ Listing 10-2. Chúng ta cũng có thể sử dụng hàm trên bất kỳ danh sách i32 
nào khác chúng ta có thể có trong tương lai.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-03/src/main.rs:here}}
```

<span class="caption">Listing 10-3: Abstracted trừu tượng để tìm số lớn nhất 
trong hai danh sách</span>

Hàm `largest` có một tham số có tên là list, biểu thị cho bất kỳ slice cụ thể nào 
của giá trị `i32` mà chúng ta có thể truyền vào hàm. Do đó, khi chúng ta gọi hàm, 
code chạy trên các giá trị cụ thể mà chúng ta truyền vào.

Tóm lại, dưới đây là các bước chúng ta đã thực hiện để thay đổi code từ Listing 10-2 
thành Listing 10-3:

1. Xác định code trùng lặp.
2. Trích xuất code trùng lặp vào phần thân của hàm và chỉ định các giá trị đầu vào 
và giá trị trả về của code đó trong chữ ký hàm.
3. Cập nhật hai trường hợp của code trùng lặp để gọi hàm thay vì.
Tiếp theo, chúng ta sẽ sử dụng những bước tương tự với generics để giảm sự trùng 
lặp code. Giống như cách phần thân hàm có thể hoạt động trên một list trừu tượng thay vì các 
giá trị cụ thể, generics cho phép code hoạt động trên các loại trừu tượng.

Ví dụ, giả sử chúng ta có hai hàm: một hàm tìm phần tử lớn nhất trong một slice 
giá trị `i32` và một hàm tìm phần tử lớn nhất trong một slice giá trị `char`. 
Làm thế nào để loại bỏ sự trùng lặp đó? Hãy cùng tìm hiểu nhé!

