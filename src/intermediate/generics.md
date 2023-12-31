# Generics

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/6rcTSxPJ6Bw?si=MHAApsNBgcLUUwWW&amp;start=13" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/JLfEiJhpTbE" target="_blank">Rust: Generics, Traits, Lifetimes
</a>

## Exercises

#### Defining generics types 1

```rust,editable,compile_fail
// Fix the code by annotating variable with the correct type.

fn main() {
    let mut shopping_list: Vec<?> = Vec::new();
    shopping_list.push("milk");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let mut shopping_list: Vec<&str> = Vec::new();
    shopping_list.push("milk");
}
  ```
</details>

#### Defining generics types 2


```rust,editable,compile_fail
// Define the generic struct Point.

fn main() {
    let p1 = Point { x: 20, y: 10 };
    let p2 = Point { x: 22.3, y: 3.14 };
    println!("Point1: ({}, {})", p1.x, p1.y);
    println!("Point2: ({}, {})", p2.x, p2.y);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let p1 = Point { x: 20, y: 10 };
    let p2 = Point { x: 22.3, y: 3.14 };
    println!("Point1: ({}, {})", p1.x, p1.y);
    println!("Point2: ({}, {})", p2.x, p2.y);
}
  ```
</details>

#### Defining generic types 3


```rust,editable,compile_fail
// Make the code compile by defining Operation enum with a single generic type.

fn main() {
    let _op1 = Operation::Add(15u8, 10u8);
    let _op2 = Operation::Mul(150, 23);
    let _op3 = Operation::Sub {
        left: 120,
        right: 50,
    };
    let _op4 = Operation::Div {
        dividend: 10.23,
        divisor: 2.43,
    };
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
enum Operation<T> {
    Add(T, T),
    Mul(T, T),
    Sub { left: T, right: T },
    Div { dividend: T, divisor: T }, 
}

fn main() {
    let _op1 = Operation::Add(15u8, 10u8);
    let _op2 = Operation::Mul(150, 23);
    let _op3 = Operation::Sub {
        left: 120,
        right: 50,
    };
    let _op4 = Operation::Div {
        dividend: 10.23,
        divisor: 2.43,
    };
}
  ```
</details>

#### Implementation block 1


```rust,editable,compile_fail
// Implement the add method for Pair<i32> type.

struct Pair<T>(T, T);

fn main() {
    let p1 = Pair(10, 23);
    let addition = p1.add();
    assert_eq!(addition, 33);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::ops::Add;

struct Pair<T>(T, T);
impl<T> Pair<T> 
where T: Add<Output = T> + Copy
{
    fn add(&self) -> T {
        self.0 + self.1
    }             
}

fn main() {
    let p1 = Pair(10, 23);
    let addition = p1.add();
    assert_eq!(addition, 33);
}
  ```
</details>

#### Implementation block 2


```rust,editable,compile_fail
// Rewrite Wrapper struct so that it supports wrapping ANY type.

struct Wrapper {
    value: u32,
}

impl Wrapper {
    pub fn new(value: u32) -> Self {
        Wrapper { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn store_u32_in_wrapper() {
        assert_eq!(Wrapper::new(42).value, 42);
    }

    #[test]
    fn store_str_in_wrapper() {
        assert_eq!(Wrapper::new("Foo").value, "Foo");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct Wrapper<T> {
    value: T,
}

impl<T> Wrapper<T> {
    pub fn new(value: T) -> Self {
        Wrapper { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn store_u32_in_wrapper() {
        assert_eq!(Wrapper::new(42).value, 42);
    }

    #[test]
    fn store_str_in_wrapper() {
        assert_eq!(Wrapper::new("Foo").value, "Foo");
    }
}
  ```
</details>

#### Generic functions


```rust,editable,compile_fail
// Fix the code so that it compiles.

fn take_and_give_ownership(input: T) -> {
    input
}

struct User {
    name: String,
    id: u32,
}

fn main() {
    let str1 = String::from("Ferris the 🦀!");
    let user1 = User {
        name: "Alice".to_string(),
        id: 199,
    };
    let _str2 = take_and_give_ownership(str1);
    let _user2 = take_and_give_ownership(user1);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn take_and_give_ownership<T>(input: T) -> T {
    input
}

struct User {
    name: String,
    id: u32,
}

fn main() {
    let str1 = String::from("Ferris the 🦀!");
    let user1 = User {
        name: "Alice".to_string(),
        id: 199,
    };
    let _str2 = take_and_give_ownership(str1);
    let _user2 = take_and_give_ownership(user1);
}
  ```
</details>
