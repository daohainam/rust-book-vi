<a id="accessing-values-in-a-hash-map"></a>
## Lưu trữ khóa với giá trị liên kết trong bản đồ băm (Hash Map)

Phần tập hợp cuối cùng trong bộ các tập hợp phổ biến là *bản đồ băm*. Kiểu `HashMap<K,V>` lưu trữ ánh xạ các khóa kiểu `K` tới các giá trị kiểu `V` bằng cách sử dụng *hàm băm*, xác định cách nó thiết đặt khóa và giá trị vào bộ nhớ. Nhiều ngôn ngữ lập trình hỗ trợ loại cấu trúc dữ liệu này, nhưng chúng thường được dùng với một cái tên khác, chẳng hạn như băm (hash), bản đồ (map), đối tượng (object), bảng băm (hash table), từ điển (dictionary) hoặc mảng kết hợp, chỉ nêu ra một vài cái tên ví dụ như vậy.

Bản đồ băm rất hữu ích khi bạn muốn tra cứu dữ liệu không phải bằng cách sử dụng chỉ mục index như bạn có thể làm với vector, mà bằng cách sử dụng một khóa có thể là bất kỳ loại nào. Ví dụ: trong một trò chơi, bạn có thể theo dõi điểm số của mỗi đội trong một bản đồ băm trong đó mỗi khóa là tên của đội và các giá trị là điểm số của mỗi đội. Tương ứng với tên một đội, bạn có thể lấy điểm của đội đó.

Chúng ta sẽ xem xét API cơ bản của bản đồ băm trong phần này, có nhiều tính năng bổ sung khác đang ẩn trong các hàm được định nghĩa trong `HashMap<K,V>` bởi thư viện chuẩn. Như mọi khi, hãy tham khảo tài liệu thư viện tiêu chuẩn để biết thêm thông tin.

### Tạo mới một bản đồ băm (hash map)

Một cách để tạo một bản đồ băm rỗng là sử dụng `new` và thêm các phần tử bằng `insert`. Trong mục 8-20, chúng ta đang theo dõi điểm số của hai đội có tên là *Blue* và *Yellow*. Đội Blue bắt đầu với 10 điểm và đội Yellow bắt đầu với 50 điểm.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-20/src/main.rs:here}}
```

<span class="caption">Mục 8-20: Tạo mới bản đồ băm và chèn vài cặp khóa-giá trị</span>

Lưu ý rằng trước tiên chúng ta cần `use` bản đồ băm `HashMap` từ bộ tập hợp của thư viện chuẩn. Trong số ba bộ sưu tập phổ biến của chúng ta, loại này ít được sử dụng nhất, vì vậy nó không được đưa vào như những tính năng tự động trong phạm vi của prelude (có thể hiểu là các thư viện đính sẵn khi compile, chỉ cần sử dụng mà không cần khai báo tường minh). Bản đồ băm cũng ít hỗ trợ hơn từ thư viện chuẩn; chẳng hạn như không có macro tích hợp sẵn để tạo chúng.

Cũng giống như vector, bản đồ băm lưu trữ dữ liệu của chúng trên heap. `HashMap` này có các khóa kiểu `String` và các giá trị kiểu `i32`. Giống như vector, bản đồ băm là đồng nhất: tất cả các khóa phải có cùng kiểu với nhau và tất cả các giá trị phải có cùng kiểu.

### Truy cập giá trị của bản đồ băm (hash map)

Chúng ta có thể lấy một giá trị từ bản đồ băm bằng cách cung cấp khóa của nó cho phương thức `get`, như được hiển thị trong mục 8-21.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-21/src/main.rs:here}}
```

<span class="caption">Mục 8-21: Truy cập điểm số của đội Blue lưu trong bản đồ băm</span>

Ở đây, `score` sẽ có giá trị được liên kết với đội Blue và kết quả sẽ là `10`. Phương thức `get` trả về một `Option<&V>`; nếu không có giá trị nào cho khóa đó trong bản đồ băm, `get` sẽ trả về `None`. Chương trình này sẽ cần phải xử lý `Option` bằng cách gọi `unsrap_or` để đặt `score` thành 0 nếu `scores` không có phần tử nào tương ứng với khóa.

Chúng ta có thể lặp qua từng cặp khóa/giá trị trong bản đồ băm theo cách tương tự như khi chúng ta làm với vector, bằng cách sử dụng vòng lặp `for`:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-03-iterate-over-hashmap/src/main.rs:here}}
```

Đoạn code này sẽ in ra từng cặp theo thứ tự tùy ý:

```text
Yellow: 50
Blue: 10
```

### Bản đồ băm và quyền sở hữu (ownership)

Đối với những kiểu dữ liệu triển khai từ đối tượng đặc tả `Copy`, như `i32`, các giá trị được sao chép vào bản đồ băm. Đối với các giá trị được sở hữu như `String`, các giá trị sẽ được di chuyển và bản đồ băm sẽ là chủ sở hữu của các giá trị đó, như được minh họa trong mục 8-22.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-22/src/main.rs:here}}
```

<span class="caption">Mục 8-22: Các khóa và giá trị thuộc sở hữu của
bản đồ băm sau khi chúng được thêm vào</span>

Chúng ta không thể sử dụng các biến `field_name` và `field_value` sau khi chúng được chuyển vào bản đồ băm với lệnh gọi `insert`.

Nếu chúng ta thêm tham chiếu (đến các giá trị) vào bản đồ băm, thì giá trị sẽ không được chuyển vào bản đồ băm. Các giá trị mà các tham chiếu trỏ đến phải hợp lệ ít nhất là trong thời gian bản đồ băm vẫn còn hợp lệ. Chúng ta sẽ nói thêm về những vấn đề này trong phần [“Xác thực tham chiếu với vòng đời (lifetime)”][validating-references-with-lifetimes]<!-- ignore --> ở Chương 10.

### Cập nhật bản đồ băm

Mặc dù số lượng cặp khóa-giá trị có thể tăng lên, nhưng mỗi khóa duy nhất chỉ có thể có một giá trị được liên kết với nó tại một thời điểm (điều này không áp dụng ngược lại: ví dụ: cả đội Blue và đội Yellow đều có thể có giá trị 10 được lưu trữ trong bản đồ băm `scores`).

Khi bạn muốn thay đổi dữ liệu trong bản đồ băm, bạn phải quyết định cách xử lý trường hợp khi một khóa đã được gán giá trị. Bạn có thể thay thế giá trị cũ bằng giá trị mới, có nghĩa là hoàn toàn bỏ qua giá trị cũ. Bạn có thể giữ giá trị cũ và bỏ qua giá trị mới, chỉ thêm giá trị mới nếu khóa *không* có giá trị. Hoặc bạn có thể kết hợp giá trị cũ và giá trị mới. Hãy xem cách thực hiện từng điều này!

#### Ghi đè một giá trị

Nếu chúng ta thêm một khóa-giá trị vào một bản đồ băm và sau đó lại chèn thêm cùng một khóa đó bằng một giá trị khác, thì giá trị được liên kết với khóa đó sẽ bị thay thế. Mặc dù mã trong mục 8-23 gọi `insert` hai lần, nhưng bản đồ băm sẽ chỉ chứa một cặp khóa-giá trị vì chúng ta đang chèn giá trị cho khóa của đội Blue cả hai lần.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-23/src/main.rs:here}}
```

<span class="caption">Mục 8-23: Thay thế giá trị lưu trữ bằng khóa cụ thể</span>

Đoạn mã này sẽ in `{"Blue": 25}`. Giá trị ban đầu của `10` đã bị ghi đè

<!-- Old headings. Do not remove or links may break. -->
<a id="only-inserting-a-value-if-the-key-has-no-value"></a>

#### Thêm khóa-giá trị nếu khóa chưa tồn tại

Thông thường, để kiểm tra xem một khóa cụ thể đã tồn tại trong bản đồ băm với một giá trị hay chưa, chúng ta sẽ thực hiện thao tác sau: nếu khóa tồn tại trong bản đồ băm, giá trị hiện có sẽ vẫn như cũ, nếu khóa không tồn tại, hãy chèn khóa và giá trị cho nó.

Bản đồ băm có một API đặc biệt cho điều này được gọi là `entry`, nó có tham số là khóa bạn muốn kiểm tra. Giá trị trả về của phương thức `entry` là một enum được gọi là `Entry` đại diện cho một giá trị có thể tồn tại hoặc không. Giả sử chúng ta muốn kiểm tra xem khóa của đội Yellow có giá trị liên kết với nó hay không. Nếu không, chúng ta muốn chèn giá trị 50 và làm điều tương tự với cho đội Blue. Sử dụng API `entry`, đoạn mã trông giống như mục 8-24.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-24/src/main.rs:here}}
```

<span class="caption">Mục 8-24: Dùng phương thức `entry` để chỉ thêm vào nếu key không tồn tại giá trị nào</span>

Phương thức `or_insert` trên `Entry` được xác định để trả về một tham chiếu mutable tham chiếu đến giá trị cho khóa `Entry` tương ứng nếu khóa đó tồn tại, và nếu không thì hãy thiết lập giá trị của tham số cho khóa này và trả về một tham chiếu mutable tham chiếu tới giá trị mới. Kỹ thuật này gọn hơn nhiều so với việc tự viết logic, và ngoài ra nó hoạt động độc đáo hơn với trình kiểm tra mượn tham chiếu.

Chạy đoạn mã trong mục 8-24 sẽ in ra `{" Yellow ": 50," Blue ": 10}`. Lệnh gọi `entry` đầu tiên sẽ chèn khóa cho đội Yellow với giá trị 50 vì đội Yellow chưa có giá trị. Lần gọi `entry` thứ hai sẽ không thay đổi bản đồ băm vì đội Blue đã có giá trị 10 sẵn.

#### Cập nhật giá trị dựa trên giá trị cũ

Một trường hợp sử dụng phổ biến khác cho bản đồ băm là tìm kiếm giá trị của khóa và sau đó cập nhật nó dựa trên giá trị cũ. Ví dụ: mục 8-25 là đoạn code xử lý đếm số lần xuất hiện của mỗi từ trong một văn bản. Chúng ta sử dụng bản đồ băm sử dụng mỗi từ ngữ làm khóa và tăng giá trị để theo dõi số lần xuất hiện của chúng. Nếu đây là lần đầu tiên chúng ta nhìn thấy một từ, trước tiên chúng ta sẽ thiết lập giá trị là 0.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-25/src/main.rs:here}}
```

<span class="caption">Mục 8-25: Đếm số lần xuất hiện của các từ ngữ bằng cách sử dụng bản đồ băm lưu trữ các từ và số đếm</span>

Đoạn mã này sẽ in ra `{"world": 2, "hello": 1, "wonderful": 1}`. Bạn có thể thấy các cặp khóa-giá trị giống nhau được in theo một thứ tự khác nhau: nhớ lại từ phần [“Truy cập giá trị của bản đồ băm”][access]<!-- ignore --> mà việc lặp qua bản đồ băm diễn ra theo thứ tự tùy ý.

Phương thức `split_whitespace` trả về một bộ lặp duyệt qua các đoạn nhỏ được phân tách bằng khoảng trắng của giá trị trong `text`. Phương thức `or_insert` trả về một tham chiếu mutable (`&mut V`) trỏ đến giá trị cho khóa được chỉ định. Ở đây chúng ta lưu trữ tham chiếu mutable đó trong biến `count`, vì vậy để gán cho giá trị đó, trước tiên chúng ta phải bỏ tham chiếu (deref) `count` bằng cách sử dụng dấu hoa thị (`*`). Tham chiếu mutable sẽ nằm ngoài phạm vi ở cuối vòng lặp `for`, vì vậy tất cả những thay đổi này đều an toàn và được các quy tắc mượn tham chiếu cho phép.

### Hàm băm

Theo mặc định, `HashMap` sử dụng một hàm băm được gọi là *SipHash* có thể cung cấp khả năng chống lại các cuộc tấn công Từ chối Dịch vụ (DoS) liên quan đến các bảng băm[^siphash]<!-- ignore -->. Đây không phải là thuật toán băm nhanh nhất hiện có, nhưng việc đánh đổi hiệu suất để mang lại bảo mật tốt hơn cũng là điều đáng giá. Nếu bạn kiểm tra các đoạn mã của mình và nhận thấy rằng hàm băm mặc định quá chậm so với mục đích của bạn, bạn có thể chuyển sang một hàm khác bằng cách chỉ định một bộ băm (hasher) khác. Một *hasher* là một kiểu triển khai của đối tượng đặc tả (trait) `BuildHasher`. Chúng ta sẽ nói về các trait và cách triển khai chúng trong Chương 10. Bạn không nhất thiết phải triển khai hàm băm của riêng mình từ đầu; [crates.io](https://crates.io/)<!-- ignore --> có các thư viện được chia sẻ bởi những người dùng Rust khác cung cấp nhiều bộ băm cho rất nhiều thuật toán băm thông dụng.

[^siphash]: [https://en.wikipedia.org/wiki/SipHash](https://en.wikipedia.org/wiki/SipHash)

## Tổng kết lại

Vector, chuỗi và bản đồ băm cung cấp một lượng lớn chức năng cần thiết trong các chương trình khi bạn cần lưu trữ, truy cập và sửa đổi dữ liệu. Dưới đây là một số bài tập bạn nên trang bị để giải quyết:

* Cho một danh sách các số nguyên, hãy sử dụng một vector và trả về giá trị trung vị (khi được sắp
  xếp, giá trị ở vị trí giữa) và mode của (giá trị xuất hiện thường xuyên nhất; bản đồ băm sẽ hữu ích trường hợp này) của danh sách.
* Chuyển đổi chuỗi sang pig latin. Phụ âm đầu tiên của mỗi từ được chuyển đến cuối từ và “ay”
  được thêm vào, vì vậy “first” trở thành “irst-fay”. Thay vào đó, những từ bắt đầu bằng một nguyên âm sẽ được thêm “hay” vào cuối (“apple” trở thành “apple-hay”). Hãy ghi nhớ các chi tiết về mã hóa UTF-8!
* Sử dụng bản đồ băm và vector, tạo giao diện văn bản để cho phép người dùng thêm tên nhân viên vào
  một phòng ban trong công ty. Ví dụ: “Thêm Sally vào Kỹ thuật” hoặc “Thêm Amir vào Bán hàng”. Sau đó, cho phép người dùng truy xuất danh sách tất cả những người trong một bộ phận hoặc tất cả những người trong công ty theo từng bộ phận, được sắp xếp theo thứ tự bảng chữ cái.

Tài liệu API thư viện tiêu chuẩn mô tả các phương thức mà vector, chuỗi và bản đồ băm có, sẽ hữu ích cho các bài tập này!

Chúng ta đang tiến đến các chương trình phức tạp hơn, mà trong đó các thao tác có thể bị lỗi, vì vậy, đây là thời điểm hoàn hảo để thảo luận về việc xử lý lỗi. Chúng ta sẽ làm điều đó tiếp theo!

[validating-references-with-lifetimes]:
ch10-03-lifetime-syntax.html#validating-references-with-lifetimes
[access]: #accessing-values-in-a-hash-map
