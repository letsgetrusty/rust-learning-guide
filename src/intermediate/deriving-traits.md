# Deriving Traits

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/n3bPhdiJm9I?si=UZTgyEpL7NcKnx5a&amp;start=602" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/Nzclc6MswaI" target="_blank">5 traits your Rust types must implement</a>

## Exercises

#### Deriving on structs

```rust,editable,compile_fail
// Complete the code by deriving the required traits.

struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let my_point = Point { x: 20, y: 10 };
    let origin = Point::default();
    println!("Origin: {origin:?}");
    if my_point == origin {
        println!("Selected point is origin!");
    } else {
        println!("Selected point: {my_point:?}");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
#[derive(Default, Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let my_point = Point { x: 20, y: 10 };
    let origin = Point::default();
    println!("Origin: {origin:?}");
    if my_point == origin {
        println!("Selected point is origin!");
    } else {
        println!("Selected point: {my_point:?}");
    }
}
  ```
</details>

#### Deriving on enums


```rust,editable,compile_fail
// Only one trait needs to be derived. Can you figure out which?

enum Size {
    Small,
    Medium,
    Large,
}

fn main() {
    let my_size = Size::Small;
    if my_size == Size::Small {
        println!("I can fit in any size!");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
#[derive(PartialEq)]
enum Size {
    Small,
    Medium,
    Large,
}

fn main() {
    let my_size = Size::Small;
    if my_size == Size::Small {
        println!("I can fit in any size!");
    }
}
  ```
</details>
