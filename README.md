## What is Rust?
Rust is a **systems programming language** focused on:
✅ **Memory safety** without garbage collection  
✅ **Concurrency** without data races  
✅ **Performance** comparable to C and C++  

Rust is ideal for:
- Low-level programming (OS development, embedded systems)
- WebAssembly
- Game development
- High-performance applications

---

## Installing Rust
Use `rustup` (Rust's official installer) to install Rust:

### For Windows
Download and install Rust from [https://rustup.rs](https://rustup.rs).  
OR  
Run in **PowerShell** (Admin mode):
```powershell
winget install --id Rustlang.Rustup -e
```

### For Linux/macOS
Open a terminal and run:
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
Restart the terminal and verify installation:
```sh
rustc --version
cargo --version
```

---

## Setting Up a Development Environment
You can use:
- **VS Code** (with the Rust extension)
- **CLion** (with Rust plugin)
- **Neovim** (with LSP & Treesitter)

### Creating a Rust Project
Rust projects are managed using `cargo`. To create a new project:
```sh
cargo new my_project
cd my_project
```
To compile and run the project:
```sh
cargo run
```

---

## Writing and Running a Simple Rust Program
Create a new Rust file `main.rs` and write:
```rust
fn main() {
    println!("Hello, Rust!");
}
```
Compile and run manually:
```sh
rustc main.rs
./main
```
Or use `cargo run` if inside a Cargo project.

---

# Basic Syntax & Data Types

## Variables & Mutability
Rust variables are immutable by default. Use `mut` for mutable variables.
```rust
fn main() {
    let x = 10;  // Immutable variable
    let mut y = 20;  // Mutable variable
    y += 5;
    println!("x: {}, y: {}", x, y);
}
```

## Data Types
Rust is **statically typed**, meaning every variable must have a type.

| Type       | Example      | Description                        |
|------------|-------------|------------------------------------|
| `i32`      | `let x: i32 = -10;` | Signed 32-bit integer |
| `u32`      | `let y: u32 = 100;` | Unsigned 32-bit integer |
| `f64`      | `let pi: f64 = 3.14;` | Floating-point number |
| `bool`     | `let is_rust_fun: bool = true;` | Boolean |
| `char`     | `let letter: char = 'R';` | Unicode character |
| `String`   | `let s: String = String::from("Rust");` | Heap-allocated string |
| `&str`     | `let s: &str = "Rust";` | String slice |

---

# Control Flow

## Conditionals (`if`, `else`, `match`)
```rust
fn main() {
    let number = 10;
    if number > 5 {
        println!("Greater than 5");
    } else {
        println!("Less than or equal to 5");
    }
}
```

### Pattern Matching with `match`
```rust
fn main() {
    let day = "Monday";
    match day {
        "Monday" => println!("Start of the week!"),
        "Friday" => println!("Weekend is coming!"),
        _ => println!("Just another day"),
    }
}
```

---

# Functions & Ownership

## Functions in Rust
```rust
fn add(a: i32, b: i32) -> i32 {
    a + b  // Implicit return
}
fn main() {
    let sum = add(4, 5);
    println!("Sum: {}", sum);
}
```

## Ownership & Borrowing
Rust enforces **ownership** to manage memory.
```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);
    // println!("{}", s); // ERROR: s was moved

    let x = 5;
    makes_copy(x);
    println!("x is still usable: {}", x);
}
fn takes_ownership(some_string: String) {
    println!("{}", some_string);
}
fn makes_copy(some_integer: i32) {
    println!("{}", some_integer);
}
```

To prevent moving, use **references (`&`)**.
```rust
fn main() {
    let s = String::from("hello");
    print_string(&s);
    println!("s is still usable: {}", s);
}
fn print_string(s: &String) {
    println!("{}", s);
}
```

---

# Structs & Enums

## Structs
```rust
struct Person {
    name: String,
    age: u32,
}
fn main() {
    let person = Person {
        name: String::from("Alice"),
        age: 30,
    };
    println!("Name: {}, Age: {}", person.name, person.age);
}
```

## Enums & Pattern Matching
```rust
enum Color {
    Red,
    Green,
    Blue,
}
fn main() {
    let color = Color::Red;
    match color {
        Color::Red => println!("It's Red!"),
        Color::Green => println!("It's Green!"),
        Color::Blue => println!("It's Blue!"),
    }
}
```

---

