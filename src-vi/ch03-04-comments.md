## Ghi chú

Tất cả các lập trình viên khi viết code đều cố gắng viết sao cho dễ hiểu, nhưng 
đôi khi vẫn cần các đoạn giải thích thêm. Trong trường hợp đo, họ sẽ để lại các 
*ghi chú* trong mã nguồn, các ghi chú này sẽ bị bỏ qua bởi trình biên dịch nhưng 
sẽ hữu ích cho người đọc code.

Đây là một ghi chú đơn giản:

```rust
// hello, world
```

Trong Rust, các ghi chú bắt đầu bằng cặp dấu slash (`//`), và ký hiệu ghi chú đó 
sẽ có giá trị cho đến hết dòng. Với những ghi chú trên nhiều hàng, bạn sẽ cần 
bao gồm `//` trên từng dòng ghi chú.

```rust
// Ở đây chúng ta đang làm một số điều phức tạp, dài đủ để ta phải cần nhiều 
// dòng ghi chú để hoàn thành! Whew! Hi vọng là ghi chú này sẽ giải thích được 
// về những điều đang diễn ra.
```

Ghi chú cũng có thể được đặt ở cuối dòng code:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

Nhưng bạn sẽ thấy chúng được dùng ở dạng sau thường xuyên hơn, với ghi chú
trên một dòng riêng biệt phía trên đoạn code nó đang giải thích:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Rust cũng có một kiểu ghi chú khác, được gọi là các ghi chú tài liệu, chúng
ta sẽ thảo luận thêm trong phần “Publishing a Crate to Crates.io” ở chương 14.