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
# **Functions & Ownership in Rust**

Rust functions are a core part of the language, supporting **ownership, borrowing, and references**. They help organize code efficiently while maintaining Rust’s **memory safety guarantees**.

---

## **Function Definitions (`fn`)**

### **Basic Function Syntax**
A function in Rust is defined using the `fn` keyword.

```rust
fn greet() {
    println!("Hello, Rust!");
}

fn main() {
    greet();  // Calling the function
}
```

💡 **Key Points:**
- The function name follows **snake_case**.
- Function blocks are enclosed in `{}`.
- `()` is mandatory even if there are no parameters.

---

### **Function with Parameters**
```rust
fn add(a: i32, b: i32) {
    println!("Sum: {}", a + b);
}

fn main() {
    add(5, 10);  // Output: Sum: 15
}
```

💡 **Key Points:**
- Parameters are **type-specified** (`a: i32, b: i32`).
- Rust enforces **strict type checking**.

---

### **Return Types (`->`)**
Functions can return values using `->` followed by the return type.

```rust
fn square(x: i32) -> i32 {
    x * x  // Implicit return (no `;`)
}

fn main() {
    let result = square(4);
    println!("Square: {}", result);  // Output: Square: 16
}
```

💡 **Key Points:**
- The last expression in a function is **implicitly returned**.
- `return` keyword is optional unless an early return is needed.

---

### **Explicit Return Using `return`**
```rust
fn divide(a: i32, b: i32) -> f64 {
    if b == 0 {
        return 0.0;  // Early return
    }
    a as f64 / b as f64
}
```

---

# **Ownership, Borrowing, and References**
Rust **ensures memory safety** by enforcing **ownership rules** at compile-time.

## **Ownership**
**Rules of Ownership:**
1. Each value in Rust has a single **owner**.
2. When the owner **goes out of scope**, the value is **dropped**.
3. Values **move** instead of being copied by default.

### **Move Semantics**
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;  // s1 is MOVED to s2

    println!("{}", s2);  // ✅ Works
    // println!("{}", s1);  // ❌ ERROR: s1 is no longer valid
}
```
💡 **Explanation:**  
- `s1` **transfers ownership** to `s2`, so `s1` becomes **invalid**.

---

### **Clone to Keep Ownership**
To create a deep copy, use `.clone()`.
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();  // Deep copy

    println!("{}", s1);  // ✅ Works
    println!("{}", s2);
}
```

---

## **Borrowing (`&`)**
Borrowing allows multiple references **without transferring ownership**.

### **Immutable Borrowing**
```rust
fn print_length(s: &String) {
    println!("Length: {}", s.len());
}

fn main() {
    let my_string = String::from("Rust");
    print_length(&my_string);  // Borrowing
    println!("{}", my_string);  // ✅ Owner still valid
}
```
💡 **Rules of Borrowing:**
- **Immutable references (`&T`) allow multiple borrows.**
- Borrowed values **cannot be modified**.

---

### **Mutable Borrowing (`&mut`)**
Allows modification but only **one mutable reference at a time**.
```rust
fn add_exclamation(s: &mut String) {
    s.push_str("!");
}

fn main() {
    let mut my_string = String::from("Hello");
    add_exclamation(&mut my_string);
    println!("{}", my_string);  // Output: Hello!
}
```
💡 **Rules of Mutable Borrowing:**
- Only **one mutable reference** per variable.
- Prevents **data races**.

---

## **Stack vs. Heap Memory**
Rust manages memory using **stack and heap** efficiently.

| Stack | Heap |
|--------|------|
| Stores small, fixed-size data | Stores dynamically sized data |
| Fast access (LIFO) | Slower access |
| Function calls, primitives, references | Strings, Vectors, large objects |
| Automatically freed on function exit | Requires explicit management (`drop`) |

```rust
fn main() {
    let x = 10;  // Stored on Stack
    let s = String::from("hello");  // Stored on Heap
}
```
💡 **Rust automatically frees heap memory** when an owner goes out of scope.

---

# **Structs & Enums in Rust**
## **Structs**
A `struct` is a **custom data type** that groups related fields together.

### **Defining and Using Structs**
```rust
struct Person {
    name: String,
    age: u32,
}

fn main() {
    let user = Person {
        name: String::from("Alice"),
        age: 25,
    };

    println!("{} is {} years old.", user.name, user.age);
}
```
💡 **Key Points:**
- `struct` allows **grouping multiple fields**.
- Fields are accessed using `.` notation.

---

### **Struct Methods (`impl` Blocks)**
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect = Rectangle { width: 10, height: 5 };
    println!("Area: {}", rect.area());
}
```
💡 **Key Points:**
- `&self` allows borrowing (`Rectangle` remains valid).
- Methods are defined inside **`impl` blocks**.

---

## **Enums**
An `enum` allows defining **a type with multiple possible variants**.

### **Defining and Using Enums**
```rust
enum Color {
    Red,
    Green,
    Blue,
}

fn main() {
    let c = Color::Red;

    match c {
        Color::Red => println!("Red selected!"),
        Color::Green => println!("Green selected!"),
        Color::Blue => println!("Blue selected!"),
    }
}
```
💡 **Key Points:**
- `enum` represents **one of many possible values**.
- Works well with `match` for pattern matching.

---

### **Enums with Data**
Enums can **store values** within variants.

```rust
enum Message {
    Text(String),
    Quit,
    Move { x: i32, y: i32 },
}

fn main() {
    let msg = Message::Text(String::from("Hello"));

    match msg {
        Message::Text(text) => println!("Message: {}", text),
        Message::Quit => println!("Quitting..."),
        Message::Move { x, y } => println!("Moving to {}, {}", x, y),
    }
}
```
💡 **Use Cases:**
- `Quit`: No data needed.
- `Text(String)`: Stores a **String**.
- `Move { x, y }`: Stores **multiple values**.

---

### **Option Enum (`None`, `Some`)**
Rust’s `Option<T>` handles **nullable values safely**.

```rust
fn divide(numerator: i32, denominator: i32) -> Option<f64> {
    if denominator == 0 {
        None
    } else {
        Some(numerator as f64 / denominator as f64)
    }
}

fn main() {
    match divide(10, 2) {
        Some(result) => println!("Result: {}", result),
        None => println!("Cannot divide by zero"),
    }
}
```
💡 **`Option<T>` prevents null pointer bugs**.

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

# **Intermediate Rust: Memory & Ownership Model, Traits, and Generics**

Rust’s **memory safety model**, **ownership system**, **traits**, and **generics** are what make it a powerful and safe language. Let's dive deep into these core concepts.

---

# **Memory Safety & Ownership Deep Dive**

Rust **prevents memory errors** like null pointer dereferences, dangling pointers, and data races by enforcing strict **ownership rules**. Let's go deeper into **move semantics, borrowing, and lifetimes**.

---

## **Move Semantics (`=` and `.clone()`)**
Unlike languages like C++ where assignment copies values by default, **Rust enforces move semantics** for types stored on the heap.

### **How Move Semantics Work**
```rust
fn main() {
    let s1 = String::from("Rust");
    let s2 = s1; // Ownership moves from s1 to s2

    println!("{}", s2); // ✅ Works
    // println!("{}", s1); ❌ ERROR: s1 is no longer valid
}
```
💡 **Key Points:**
- `s1` **transfers ownership** to `s2`, making `s1` **invalid**.
- This prevents **double freeing memory** when `s1` and `s2` go out of scope.

---

### **Deep Copy Using `.clone()`**
If you want to **retain ownership** of `s1` while creating a new copy, use `.clone()`.

```rust
fn main() {
    let s1 = String::from("Rust");
    let s2 = s1.clone(); // Creates a deep copy

    println!("{}", s1); // ✅ Works
    println!("{}", s2);
}
```
💡 **Key Points:**
- `.clone()` allocates **new memory** instead of transferring ownership.
- **Use `.clone()` cautiously** since it has performance costs.

---

## **Borrowing (`&T`, `&mut T`) and Lifetimes (`'a`)**
Borrowing allows functions to **access** data without taking ownership.

### **Immutable Borrowing (`&T`)**
```rust
fn print_length(s: &String) {
    println!("Length: {}", s.len());
}

fn main() {
    let my_string = String::from("Rust");
    print_length(&my_string); // Borrowing
    println!("{}", my_string); // ✅ Owner still valid
}
```
💡 **Key Points:**
- Multiple **immutable references** can exist at the same time.
- Borrowed values **cannot be modified**.

---

### **Mutable Borrowing (`&mut T`)**
Only **one mutable borrow** is allowed at a time.

```rust
fn change(s: &mut String) {
    s.push_str(" is awesome!");
}

fn main() {
    let mut my_string = String::from("Rust");
    change(&mut my_string);
    println!("{}", my_string); // ✅ Works
}
```
💡 **Rules of Mutable Borrowing:**
- Only **one mutable reference** at a time.
- Prevents **data races**.

---

## **Lifetimes (`'a`)**
Lifetimes prevent **dangling references**.

### **Dangling Reference Example (Invalid Code)**
```rust
fn dangle() -> &String { // ❌ ERROR: Returns a reference to dropped data
    let s = String::from("Hello");
    &s // s goes out of scope here
}

fn main() {
    let ref_s = dangle();
    println!("{}", ref_s);
}
```
💡 **Problem:** `s` is dropped, so `&s` becomes invalid.

---

### **Correcting with Lifetimes**
```rust
fn safe_reference<'a>(s: &'a String) -> &'a String {
    s
}

fn main() {
    let s1 = String::from("Rust");
    let s2 = safe_reference(&s1);
    println!("{}", s2);
}
```
💡 **Key Points:**
- `'a` ensures the reference is valid **as long as `s1` is valid**.
- Lifetimes **prevent use-after-free errors**.

---

# **Traits & Generics**
## **Traits: Defining Shared Behavior**
Traits define **shared behavior** for multiple types.

### **Defining a Trait**
```rust
trait Speak {
    fn say_hello(&self);
}

struct Dog;
struct Cat;

impl Speak for Dog {
    fn say_hello(&self) {
        println!("Woof!");
    }
}

impl Speak for Cat {
    fn say_hello(&self) {
        println!("Meow!");
    }
}

fn main() {
    let d = Dog;
    let c = Cat;
    d.say_hello();
    c.say_hello();
}
```
💡 **Key Points:**
- **`trait Speak`** defines a behavior.
- `impl Speak for Dog` and `impl Speak for Cat` implement it for **different types**.

---

### **Trait Bounds (`T: Trait`)**
You can use **trait bounds** to specify that a type must implement a certain trait.

```rust
fn make_sound<T: Speak>(animal: T) {
    animal.say_hello();
}

fn main() {
    let d = Dog;
    make_sound(d); // ✅ Works because Dog implements Speak
}
```
💡 **Trait bounds** (`T: Speak`) ensure that **only types implementing `Speak`** can be passed.

---

## **Generics (`T`) and Type Parameters**
Generics allow defining **functions, structs, and enums** that work with **any type**.

### **Generic Functions**
```rust
fn print<T: std::fmt::Debug>(value: T) {
    println!("{:?}", value);
}

fn main() {
    print(42);
    print("Hello");
    print(vec![1, 2, 3]);
}
```
💡 **Key Points:**
- `T: Debug` ensures `T` implements `Debug` (for `println!`).

---

### **Generic Structs**
```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let int_point = Point { x: 10, y: 20 };
    let float_point = Point { x: 1.5, y: 2.5 };
}
```
💡 **Key Points:**
- `T` allows `Point` to store **any type** (`i32`, `f64`, etc.).
- Generics enable **code reuse**.

---

## **`impl Trait` and Associated Types**
### **Using `impl Trait` in Functions**
Instead of `T: Trait`, we can use `impl Trait`:
```rust
fn get_speaker() -> impl Speak {
    Dog
}

fn main() {
    let animal = get_speaker();
    animal.say_hello(); // Woof!
}
```
💡 **`impl Trait` is more concise** and useful when returning **a single concrete type**.

---

### **Associated Types in Traits**
Associated types help define **type placeholders** within traits.

```rust
trait Container {
    type Item;
    fn get(&self) -> Self::Item;
}

struct IntBox;

impl Container for IntBox {
    type Item = i32;
    fn get(&self) -> i32 {
        42
    }
}

fn main() {
    let b = IntBox;
    println!("{}", b.get()); // Output: 42
}
```
💡 **Key Points:**
- `type Item` defines a **placeholder type**.
- `Self::Item` allows **type inference**.

---

