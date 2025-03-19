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
# **Tuples & Arrays in Rust**  

Rust provides **tuples** and **arrays** for handling fixed-size collections. They differ in structure, memory allocation, and use cases.  

---

## **Tuples in Rust**  
**A tuple is a fixed-size collection of multiple values of different types.** It is useful for grouping related data together without defining a struct.  

### **Defining and Using Tuples**
```rust
fn main() {
    let person: (i32, f64, char) = (25, 5.6, 'A');

    // Access tuple elements by index
    println!("Age: {}", person.0);
    println!("Height: {}", person.1);
    println!("Initial: {}", person.2);
}
```
💡 **Key Points:**  
- Tuples have a **fixed size** and **heterogeneous** types.  
- Access elements using **dot notation** (`tuple.index`).  
- Useful for returning multiple values from functions.  

---

### **Tuple Destructuring**  
Rust allows destructuring tuples into variables.  
```rust
fn main() {
    let data = ("Alice", 30, 5.5);

    let (name, age, height) = data;  // Destructuring
    println!("Name: {}, Age: {}, Height: {}", name, age, height);
}
```

---

### **Returning Multiple Values from Functions with Tuples**  
```rust
fn get_details() -> (String, i32) {
    let name = String::from("Rustacean");
    let age = 3;
    (name, age)  // Returning a tuple
}

fn main() {
    let (name, age) = get_details();
    println!("Name: {}, Age: {}", name, age);
}
```

---

## **Arrays in Rust**  
**Arrays in Rust are fixed-size collections of elements of the same type.**  

### **Declaring and Using Arrays**
```rust
fn main() {
    let numbers: [i32; 5] = [1, 2, 3, 4, 5];

    println!("First element: {}", numbers[0]);
    println!("Last element: {}", numbers[4]);
}
```
💡 **Key Points:**  
- Arrays store multiple values of the **same type**.  
- The **size is fixed** at compile-time.  
- Access elements using **indexing (`array[index]`)**.  

---

### **Default Value Arrays**
```rust
fn main() {
    let zeros = [0; 5];  // Creates [0, 0, 0, 0, 0]
    println!("{:?}", zeros);
}
```

---

### **Iterating Over an Array**
```rust
fn main() {
    let numbers = [10, 20, 30, 40, 50];

    for num in numbers.iter() {
        println!("Number: {}", num);
    }
}
```

---

### **Out-of-Bounds Access**
Rust ensures **safe memory access**. Accessing out-of-range indices will cause a runtime panic.  
```rust
fn main() {
    let arr = [1, 2, 3, 4, 5];
    println!("{}", arr[10]); // ❌ ERROR: This will panic
}
```

---

## **Vec<T>: Growable Arrays**  
Unlike arrays, **Vec<T> (Vectors) are dynamically resizable.**  

### **Declaring and Using Vectors**
```rust
fn main() {
    let mut numbers = vec![1, 2, 3];

    numbers.push(4);  // Adding an element
    numbers.push(5);
    
    println!("{:?}", numbers);  // Output: [1, 2, 3, 4, 5]
}
```
💡 **Key Points:**  
- `Vec<T>` is a **growable collection** of elements of the same type.  
- **Use `.push()`** to add elements.  
- **Uses heap memory**, unlike arrays.  

---

### **Accessing Elements in a Vector**
```rust
fn main() {
    let numbers = vec![10, 20, 30];

    println!("First: {}", numbers[0]);
    println!("Second: {}", numbers.get(1).unwrap());
}
```
**Safe Access Using `.get()`**  
```rust
fn main() {
    let numbers = vec![10, 20, 30];

    match numbers.get(5) {
        Some(&value) => println!("Value: {}", value),
        None => println!("Out of bounds!"),
    }
}
```

---

### **Iterating Over a Vector**
```rust
fn main() {
    let numbers = vec![100, 200, 300];

    for num in &numbers {
        println!("{}", num);
    }
}
```

---

### **Removing Elements from a Vector**
```rust
fn main() {
    let mut nums = vec![1, 2, 3, 4, 5];

    nums.pop();  // Removes last element
    println!("{:?}", nums);

    nums.remove(1);  // Removes element at index 1
    println!("{:?}", nums);
}
```

---

# **Strings & Slices in Rust**  

## **String vs. &str**  
Rust has two string types:  
| Type    | Description | Example |
|---------|------------|---------|
| `String` | Heap-allocated, growable | `String::from("hello")` |
| `&str` | Immutable string slice | `"hello"` |

---

### **String Operations**
```rust
fn main() {
    let mut s = String::from("Hello");

    s.push('!');         // Adds a character
    s.push_str(" Rust"); // Adds a string
    println!("{}", s);   // Output: Hello! Rust
}
```

---

### **String Slices (`&str`)**  
A **string slice** is a reference to part of a `String`.  
```rust
fn main() {
    let s = String::from("Rust Programming");
    
    let slice = &s[0..4];  // "Rust"
    println!("{}", slice);
}
```
💡 **Key Points:**  
- Slices do **not own** data.  
- Useful for referencing substrings **without copying**.  

---

### **String Manipulation**
```rust
fn main() {
    let text = " Rust is awesome! ";

    println!("{}", text.trim());  // Removes whitespace
    println!("{:?}", text.split_whitespace().collect::<Vec<&str>>());
}
```

---

### **Iterating Over a String**
```rust
fn main() {
    let s = String::from("Rust");

    for c in s.chars() {
        println!("{}", c);
    }
}
```

---

## **Final Thoughts**
- **Use `&str` when possible** (more efficient).  
- **Use `String` when modification is needed.**  
- **Prefer slices to avoid unnecessary copies.**  

---

This is a deep dive into **Tuples, Arrays, Vectors, Strings, and Slices** in Rust! Let me know if you need more examples. 🚀
