# Programming a Guessing Game

```bash
cargo new guessing_game
```

cargo.toml

```toml
[package]
name = "guessing_game"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
rand = "0.8.4"
```

main.rs

```rust
use rand::Rng;
// std是标准库，无需在toml文件指定依赖
use std::cmp::Ordering;
use std::io;

/**
 * main函数，函数执行的起点
 */
fn main() {
    // println! 是一个宏，不是函数
    println!("Guess the number!");
    // 默认推断为i32，但是因为后面将guess手动标记为了u32，并且将它和secret_number进行了比较，
    // 所以Rust会将secret_number也推导为相同的u32类型。
    // 1..=100 是范围表达式
    let secret_number = rand::thread_rng().gen_range(1..=100);
    // 1..101 左闭右开
    // let secret_number = rand::thread_rng().gen_range(1..101);
    loop {
        println!("Please input your guess.");
        // new是String的关联函数，类似其他语言的静态方法。
        let mut guess = String::new();
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
        // let guess: i32 = guess.trim().parse().expect("Please input a number!");
        // trim是为了移除回车输入的\n
        // 这里的guess指定为i32和u32均可
        // match 类似其他语言的switch和case
        // parse 的结果是一个Result枚举类型，有Ok和Err两个值
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            // 解析失败让用户继续输入
            Err(_) => continue,
        };
        println!("You guessed: {guess}");
        // 数值类型可比较，自带cmp方法
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            // 必须覆盖所有分支
            Ordering::Equal => {
                println!("You win!");
                // 退出循环
                break;
            }
        }
    }
}
```

```bash
cargo build
```

```bash
cargo run
```
