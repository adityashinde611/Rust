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
# **Rust Collections, Iterators, and Error Handling (Deep Dive)**  

Rust provides powerful **collections, iterators, and error handling mechanisms** that enable safe and efficient programming. Let's break them down **conceptually and practically** with deep insights.

---

# **1. Collections & Iterators**  

Rust has three primary **collection types**:  
- `Vec<T>` → Dynamic, resizable arrays.  
- `HashMap<K, V>` → Key-value storage, similar to dictionaries.  
- `HashSet<T>` → Unique, unordered collection of values.  

These collections work **efficiently** with **iterators**, which allow **lazy evaluation and functional-style transformations**.

---

## **1.1 Vec<T>: The Growable Array**  

A `Vec<T>` is a dynamically allocated array that grows automatically when needed. Unlike arrays (`[T; N]`), a `Vec<T>` can change its size at runtime.

### **Creating and Modifying a Vec**
```rust
fn main() {
    let mut numbers: Vec<i32> = vec![1, 2, 3];

    numbers.push(4); // Add element
    numbers.remove(1); // Remove second element

    println!("{:?}", numbers); // Output: [1, 3, 4]
}
```
💡 **Key Takeaways:**  
- `vec![]` is a macro that creates a `Vec<T>`.  
- `push()` adds elements, and `remove(index)` removes them.  
- **Indexing (`numbers[0]`) can panic if out of bounds!**  

---

### **Iterating Over a Vec**
```rust
fn main() {
    let numbers = vec![10, 20, 30];

    // Immutable reference iteration
    for num in &numbers {
        println!("{}", num);
    }

    // Mutable iteration
    let mut numbers = vec![1, 2, 3];
    for num in &mut numbers {
        *num *= 2;
    }
    println!("{:?}", numbers); // Output: [2, 4, 6]
}
```
💡 **Key Takeaways:**  
- `&numbers` → Iterates immutably.  
- `&mut numbers` → Iterates mutably, modifying elements in place.  

---

## **1.2 HashMap<K, V>: Key-Value Storage**  

A `HashMap<K, V>` stores data as key-value pairs, allowing fast lookups.  

### **Creating and Using a HashMap**
```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 85);

    if let Some(score) = scores.get("Alice") {
        println!("Alice's score: {}", score);
    }

    scores.entry("Charlie").or_insert(75); // Insert if missing
    println!("{:?}", scores);
}
```
💡 **Key Takeaways:**  
- `insert(key, value)` adds elements.  
- `get(&key)` returns an `Option<&V>`.  
- `entry().or_insert(value)` inserts only if the key **does not exist**.  

---

## **1.3 HashSet<T>: Unordered Unique Collection**  

A `HashSet<T>` stores only **unique elements** and is optimized for **fast lookups**.

```rust
use std::collections::HashSet;

fn main() {
    let mut set = HashSet::new();
    set.insert(10);
    set.insert(20);
    set.insert(10); // Duplicate is ignored

    println!("{:?}", set.contains(&10)); // true
    println!("{:?}", set); // Output: {10, 20}
}
```
💡 **Key Takeaways:**  
- **No duplicate values.**  
- **Lookup time is O(1)** (on average).  

---

## **1.4 Option<T> and Result<T, E> for Safer Handling**  

Rust doesn’t use `null`. Instead, it provides **`Option<T>`** and **`Result<T, E>`** for safer handling.

### **Option<T>: Handling Absence of a Value**
```rust
fn find_square(n: Option<i32>) -> Option<i32> {
    n.map(|num| num * num)
}

fn main() {
    let number = Some(5);
    let result = find_square(number);
    println!("{:?}", result); // Some(25)

    let none_value: Option<i32> = None;
    println!("{:?}", find_square(none_value)); // None
}
```
💡 **Key Takeaways:**  
- `Option<T>` is either `Some(value)` or `None`.  
- `map()` applies a function **only if Some(value) exists**.  

---

### **Result<T, E>: Handling Errors Properly**
```rust
fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err("Cannot divide by zero!".to_string())
    } else {
        Ok(a / b)
    }
}

fn main() {
    match divide(10, 2) {
        Ok(result) => println!("Result: {}", result),
        Err(error) => println!("Error: {}", error),
    }
}
```
💡 **Key Takeaways:**  
- `Result<T, E>` is either `Ok(value)` (success) or `Err(error)` (failure).  
- `match` is used to handle errors explicitly.  

---

# **2. Iterators & Custom Iterators**  

Rust’s **iterators** enable **lazy evaluation** and functional-style operations.

### **iter(), into_iter(), iter_mut()**
```rust
fn main() {
    let numbers = vec![1, 2, 3];

    // Immutable iteration
    for n in numbers.iter() {
        println!("{}", n);
    }

    // Consuming iteration (takes ownership)
    for n in numbers.into_iter() {
        println!("{}", n);
    }

    // Mutable iteration
    let mut numbers = vec![1, 2, 3];
    for n in numbers.iter_mut() {
        *n *= 2;
    }
    println!("{:?}", numbers); // Output: [2, 4, 6]
}
```
💡 **Key Takeaways:**  
- `iter()` → Immutable references (`&T`).  
- `into_iter()` → Consumes the collection (`T`).  
- `iter_mut()` → Mutable references (`&mut T`).  

---

## **Custom Iterators Using `impl Iterator`**
```rust
struct Counter {
    count: i32,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    type Item = i32;

    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}

fn main() {
    let mut counter = Counter::new();
    while let Some(num) = counter.next() {
        println!("{}", num);
    }
}
```
💡 **Key Takeaways:**  
- Implement the `Iterator` trait for custom iterators.  
- **`next()` defines how elements are retrieved lazily.**  

---

# **3. Error Handling in Rust**  

Rust provides **panic-free, predictable error handling**.

## **3.1 panic!(), unwrap(), expect()**
```rust
fn main() {
    // panic! Example
    // panic!("Something went wrong!"); // This will crash the program

    let value: Option<i32> = None;
    // println!("{}", value.unwrap()); // ❌ Panics if None

    println!("{}", value.unwrap_or(0)); // ✅ Default value
}
```
💡 **Avoid `unwrap()` unless you are sure the value exists!**  

---

## **3.2 Propagating Errors Using `?` Operator**
```rust
fn read_number() -> Result<i32, std::num::ParseIntError> {
    let number = "42".parse::<i32>()?; // Propagates error if parsing fails
    Ok(number)
}
```
💡 **`?` automatically propagates errors instead of using `match`.**  

---

## **3.3 Custom Error Types**
```rust
use std::fmt;

#[derive(Debug)]
struct MyError;

impl fmt::Display for MyError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Custom error occurred")
    }
}

fn main() {
    let result: Result<i32, MyError> = Err(MyError);
    println!("{:?}", result);
}
```
💡 **Implementing `Display` makes custom errors user-friendly.**  

---
# **Rust Concurrency & Smart Pointers (Deep Dive)**  

Rust provides **safe and efficient concurrency** using **threads, synchronization primitives, and message passing**. It also offers **smart pointers** that help with **memory management** and **interior mutability**. Let's break these concepts down **in-depth** with examples.  

---

# **1. Concurrency in Rust**  

Rust ensures **safe concurrent programming** by enforcing ownership and borrowing rules at **compile-time**.  

### **Concurrency Topics:**  
- **Threads (`std::thread::spawn`)** → Run tasks in parallel.  
- **Mutex (`std::sync::Mutex`)** → Shared mutable state across threads.  
- **Atomic Types (`AtomicUsize`)** → Low-level atomic operations.  
- **Channels (`std::sync::mpsc`)** → Thread communication via message passing.  

---

## **1.1 Threads (`std::thread::spawn`)**  

Threads allow you to run multiple tasks **simultaneously**. Rust uses **native OS threads**.  

### **Example: Creating Threads**  
```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..5 {
            println!("Thread: {}", i);
            thread::sleep(Duration::from_millis(500));
        }
    });

    for i in 1..3 {
        println!("Main: {}", i);
        thread::sleep(Duration::from_millis(500));
    }
}
```
**Output (order varies due to concurrency):**
```
Main: 1
Thread: 1
Main: 2
Thread: 2
Thread: 3
Thread: 4
```
💡 **Key Takeaways:**  
- `thread::spawn()` runs code **asynchronously**.  
- `thread::sleep()` simulates **workload**.  
- The **main thread may exit before spawned threads finish**.  

---

## **1.2 Mutex (`std::sync::Mutex`)**  

A **Mutex (mutual exclusion)** allows **safe** shared access to **mutable data** between threads.  

### **Example: Sharing Data with Mutex**  
```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..5 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Final counter value: {}", *counter.lock().unwrap());
}
```
💡 **Key Takeaways:**  
- **Mutex ensures only one thread can access data at a time.**  
- **Arc (Atomic Reference Counting) allows multiple threads to share ownership of the Mutex.**  
- `.lock().unwrap()` **locks** the data for exclusive use.  

---

## **1.3 Atomic Types (`std::sync::atomic::AtomicUsize`)**  

Rust provides **atomic types** for lock-free programming. These allow **fast, safe** concurrent access to a variable.  

### **Example: Atomic Counter**  
```rust
use std::sync::atomic::{AtomicUsize, Ordering};
use std::thread;

fn main() {
    let counter = AtomicUsize::new(0);
    let mut handles = vec![];

    for _ in 0..5 {
        let handle = thread::spawn({
            let counter = &counter;
            move || {
                counter.fetch_add(1, Ordering::SeqCst);
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Final counter value: {}", counter.load(Ordering::SeqCst));
}
```
💡 **Key Takeaways:**  
- `AtomicUsize` avoids using Mutex for simple **atomic operations**.  
- `fetch_add(1, Ordering::SeqCst)` **increments the value atomically**.  
- **Ordering ensures memory synchronization across threads.**  

---

## **1.4 Channels (`std::sync::mpsc`) for Message Passing**  

Rust supports **safe inter-thread communication** using **channels**.  

### **Example: Sending Messages Between Threads**  
```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let msg = String::from("Hello from thread!");
        tx.send(msg).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("Received: {}", received);
}
```
💡 **Key Takeaways:**  
- **mpsc (multi-producer, single-consumer)** → multiple threads can send messages to **one receiver**.  
- **`send()` moves ownership**, preventing data races.  
- **`recv()` blocks until a message arrives.**  

---

# **2. Smart Pointers & Interior Mutability**  

Rust provides **smart pointers** that add extra functionality to references, enabling **shared ownership, heap allocation, and interior mutability**.  

### **Types of Smart Pointers:**  
1. **Box<T>** → Heap allocation.  
2. **Rc<T> (Reference Counting)** → Shared ownership for single-threaded programs.  
3. **Arc<T> (Atomic Reference Counting)** → Shared ownership across threads.  
4. **RefCell<T>** → Mutable borrowing at runtime.  
5. **Weak<T>** → Preventing cyclic references in `Rc<T>`.  

---

## **2.1 Box<T>: Heap Allocation**  

A `Box<T>` **allocates memory on the heap** instead of the stack.  

### **Example: Using Box for Heap Allocation**  
```rust
fn main() {
    let b = Box::new(10);
    println!("Value: {}", b);
}
```
💡 **Key Takeaways:**  
- Used for **large data structures**.  
- Enables **recursive types** (like linked lists).  

---

## **2.2 Rc<T>: Shared Ownership**  

`Rc<T>` allows **multiple owners** for a value, but **only in a single-threaded context**.  

### **Example: Shared Ownership with Rc**  
```rust
use std::rc::Rc;

fn main() {
    let a = Rc::new(10);
    let b = Rc::clone(&a);
    println!("a = {}, b = {}", a, b);
    println!("Reference count: {}", Rc::strong_count(&a));
}
```
💡 **Key Takeaways:**  
- **Increases reference count** instead of copying.  
- **For multi-threading, use `Arc<T>`.**  

---

## **2.3 Arc<T>: Thread-Safe Shared Ownership**  

### **Example: Arc for Multi-threaded Ownership**  
```rust
use std::sync::Arc;
use std::thread;

fn main() {
    let counter = Arc::new(10);
    let counter_clone = Arc::clone(&counter);

    let handle = thread::spawn(move || {
        println!("Counter in thread: {}", counter_clone);
    });

    handle.join().unwrap();
    println!("Counter in main: {}", counter);
}
```
💡 **Key Takeaways:**  
- `Arc<T>` **works across threads**, unlike `Rc<T>`.  
- **Uses atomic operations** for thread safety.  

---

## **2.4 RefCell<T>: Interior Mutability**  

`RefCell<T>` **allows mutation even if the variable is immutable**, enforcing borrow rules **at runtime** instead of compile time.  

### **Example: Mutating Immutable Data**  
```rust
use std::cell::RefCell;

fn main() {
    let x = RefCell::new(10);
    *x.borrow_mut() += 5;
    println!("Updated value: {}", x.borrow());
}
```
💡 **Key Takeaways:**  
- Allows **mutable borrowing in immutable contexts**.  
- **Panics if multiple mutable borrows occur at runtime.**  

---

## **2.5 Weak<T>: Breaking Cyclic References in Rc<T>**  

`Rc<T>` can cause **memory leaks** if two objects reference each other. `Weak<T>` **prevents this by creating a non-owning reference**.  

### **Example: Avoiding Cyclic References**  
```rust
use std::rc::{Rc, Weak};

struct Node {
    parent: Option<Weak<Node>>,
}

fn main() {
    let child = Rc::new(Node { parent: None });
    let parent = Rc::new(Node { parent: Some(Rc::downgrade(&child)) });

    println!("Parent count: {}", Rc::strong_count(&parent));
}
```
💡 **Key Takeaways:**  
- `Weak<T>` prevents memory leaks caused by **cyclic references**.  

---


