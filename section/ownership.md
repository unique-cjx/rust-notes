## Variables and Mutability

如果你想让这个变量的可变性暴露出来的时候，就在变量名前面明确地加个 `mut` 修饰符。
```rust
let mut x = 10u32;
```

固定尺寸类型的数据默认会在栈上；而其他复杂类型的数据默认会创建在堆上，成为堆的一个资源，然后在使用栈上的一个局部变量引用它。
```rust
fn main() {
    let s1 = String::from("I am a superman.");
    let s2 = s1;
    println!("{s1}"); // error: s1 was moved to s2, so s1 can no longer be used.
    println!("{s2}");
}
```
上面的案例会出现`borrow of moved value: s1`编译错误，Rust虽然也是把字符串的引用由 s1 拷贝到了 s2，但是只保留了最新的 s2 到字符串的指向，同时却把 s1 到字符串的指向给“抹去”了。s1 之后都处于一种“不可用”的状态，直到函数结束。

"consider cloning the value if the performance cost is acceptable." e.g. `s1.clone()`

## Ownership Rules

1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

### Transfer Ownership 
通过`->`将函数里的变量的所有权转移出来。
```rust
fn foo(s: String) -> String {
    println!("{s}");
    s
}
fn main() {
    let s1 = String::from("I am a superman.");
    let s1 = foo(s1);
    println!("{s1}");
}
```

### 拥有`Copy trait`数据的类型
- 所有整数类型: u32
- 布尔类型: bool
- 浮点类型: f32
- 字符类型: char
- 元组类型。元组里所有元素类型都实现了`Copy trait`
- 数组。数组中的元素类型实现了`Copy trait`
- 不可变的引用类型

**其他类型默认都是做移动所有权的操作。**

> 📢注意：所有权可以被转移，一旦所有权被转移，原来持有该资源的变量就失效了。变量的作用域是在最近的花括号位置内。
