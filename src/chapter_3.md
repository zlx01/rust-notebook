## Variables/Mutability

- by default, variables are **immutable**
- make them mutable by adding `mut` in front of the variable name

## Constants

- not allowed to use `mut` with constants
- constants are always immutable
- the type of the constants _must_ be annotated
- constants should be set only to a constant expression, not the result of a value that could only be computed at runtime.

| immutable variable | mutable variable | constants |
| ------------------ | ---------------- | --------- |
| let                | let mut          | const     |

## Shadowing

- declare a new variable with the same name as a previous variable **in the same scope**
- we can change the type of the value but reuse the same name.

> Shadowing thus spares us from having to come up with different names, such as `spaces_str` and `spaces_num`

## Data Types

- Rust is a _statically typed_ language
- The compiler can usually infer what type we want to use based on the value and how we use it.
- Sometimes, we must add a type annotation
- integer types default to `i32`.
- float types default to `f64`.

| Scalar Types           | Compound Types |
| ---------------------- | -------------- |
| integers               | tuples         |
| floating-point numbers | arrays         |
| Booleans               |                |
| characters             |                |

### Integer Types

| **Length** | **Signed** | **Unsigned** |
| ---------- | ---------- | ------------ |
| 8-bit      | `i8`       | `u8`         |
| 16-bit     | `i16`      | `u16`        |
| **32-bit** | **`i32`**  | **`u32`**    |
| 64-bit     | `i64`      | `u64`        |
| 128-bit    | `i128`     | `u128`       |
| arch       | `isize`    | `usize`      |

| Number literals  | Example       |
| ---------------- | ------------- |
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

### Floating-Point Types

IEEE-754

| **Length** | **Signed** |
| ---------- | ---------- |
| 8-bit      | `f32`      |
| **16-bit** | `f64`      |

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

### Numeric Operations

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // 结果为 -1

    // remainder
    let remainder = 43 % 5;
}
```

### The Boolean Type

```rust
fn main() {
    let t = true;
    let f: bool = false; // with explicit type annotation
}
```

### The Character Type

- four bytes
- Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid `char` values in Rust.
- Unicode Scalar Value(from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF`)

```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```

### The Tuple Type

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
    let tup = (500, 6.4, 1);
    let (x, y, z) = tup

    let (mut x, y) = (1, 2);
    x += 2;

    let x: (i32, f64, u8) = (500, 6.4, 1);
    let five_hundred = x.0;
    let six_point_four = x.1;
    let one = x.2;
}
```

### The Array Type

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    let a: [i32; 5] = [1, 2, 3, 4, 5];
    let a = [3; 5]; // [3, 3, 3, 3, 3]

    let first = a[0];
    let second = a[1];
}
```

### Invalid Array Element Access

- resulted in a _runtime_ error at the point of using an invalid value in the indexing operation.

```rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!("The value of the element at index {index} is: {element}");
}
```

## Functions

- fn
- _snake_case_

```rust
fn main() {
    println!("Hello, world!");
    another_function(5);
    print_labeled_measurement(5, 'h');
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

## Comments

- `// line comments`

```rust
fn main() {
    // So we’re doing something complicated here, long enough that we need
    // multiple lines of comments to do it! Whew! Hopefully, this comment will
    // explain what’s going on.
    let lucky_number = 7; // I’m feeling lucky today
}
```

## Statements and Expressions

- Calling a function is an expression
- Calling a macro is an expression
- **A new scope block created with curly brackets is an expression**

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    // You can return early from a function by using the return keyword and specifying a value,
    // but most functions return the last expression implicitly.
    x + 1
}
```

## **Control Flow**

### **`if` Expressions**

- Rust will **not** automatically try to convert non-Boolean types to a Boolean

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
fn main() {
    let number = 3;
    // error! Rust will **not** automatically try to convert non-Boolean types to a Boolean
    if number {
        println!("number was three");
    }
    // right!
    if number != 0 {
        println!("number was something other than zero");
    }
}
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

### **Using `if` in a `let` Statement**

- like ternary operator

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

### **Repeating Code with `loop`**

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

### **Returning Values from Loops**

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

### **Loop Labels to Disambiguate Between Multiple Loops**

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

Java

```java
public class Main {
    public static void main(String[] args) {
        int count = 0;

        countingUp:
        while (true) {
            System.out.println("count = " + count);
            int remaining = 10;

            while (true) {
                System.out.println("remaining = " + remaining);
                if (remaining == 9) {
                    break;
                }
                if (count == 2) {
                    break countingUp;
                }
                remaining -= 1;
            }

            count += 1;
        }
        System.out.println("End count = " + count);
    }
}
```

C / C++ `goto`

```cpp
#include <stdio.h>

int main()
{
    int count = 0;

counting_up:
    while (1)
    {
        printf("count = %d\\n", count);
        int remaining = 10;

        while (1)
        {
            printf("remaining = %d\\n", remaining);
            if (remaining == 9)
            {
                break;
            }
            if (count == 2)
            {
                goto counting_up_end;
            }
            remaining -= 1;
        }

        count += 1;
    }

counting_up_end:
    printf("End count = %d\\n", count);

    return 0;
}
#include <iostream>

int main()
{
    int count = 0;

counting_up:
    while (true)
    {
        std::cout << "count = " << count << std::endl;
        int remaining = 10;

        while (true)
        {
            std::cout << "remaining = " << remaining << std::endl;
            if (remaining == 9)
            {
                break;
            }
            if (count == 2)
            {
                goto counting_up_end;
            }
            remaining -= 1;
        }

        count += 1;
    }

counting_up_end:
    std::cout << "End count = " << count << std::endl;

    return 0;
}
```

### **Conditional Loops with `while`**

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index += 1;
    }
}
```

### **Looping Through a Collection with `for`**

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```

## Matrix

### Integer Overflow

256 → u8

in debug mode: cause your program to _panic_ at runtime

in release mode: Rust performs _two’s complement wrapping_.

```rust
// 解决代码中的错误和 `panic`
fn main() {
   let v1 = 251_u8 + 8;
   let v2 = i8::checked_add(251, 8).unwrap();
   println!("{},{}",v1,v2);
}
```

### pattern matching and destructuring

```rust
let (x, y);
(x, ..) = (3, 4);
[.., y] = [1, 2];
```

### explicit type conversion

```rust
let x: i32 = 5;
let mut y: u32 = 5;

y = x as u32;
```

### float

```rust
let a: f64 = 0.1 + 0.2;
let b = 0.3;
let diff = a - b;
let tolerance = 10.0e-10;
assert!(diff.abs() < tolerance);
```

注意a一定要显式标注f64，否则会报错：can't call method `abs` on ambiguous numeric type `float`

### chat to u8

```rust
for c in 'a'..='z' {
		println!("{}", c as u8);
}
```

### never return function

```rust
fn never_return_fn() -> ! {
    unimplemented!()
}
fn never_return_fn() -> ! {
    panic!()
}
fn never_return_fn() -> ! {
    todo!();
}
fn never_return_fn() -> ! {
    loop {
        std::thread::sleep(std::time::Duration::from_secs(1))
    }
}
```
