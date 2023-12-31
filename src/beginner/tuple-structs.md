# Tuple Structs

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/n3bPhdiJm9I?si=CIUXxMZ9ybitHn3s&amp;start=360" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Definition

<details>
  <summary>Description</summary>
  
  This is a helpful description. Read me to understand what to do!

</details>

```rust,editable,compile_fail
// Complete the structure definition.

struct Point

impl Point {
    fn on_x_axis(&self) -> bool {
        self.1 == 0.0
    }
    fn on_y_axis(&self) -> bool {
        self.0 == 0.0
    }
}

fn main() {
    let point = Point(0.0, 0.0);
    if point.on_x_axis() && point.on_y_axis() {
        println!("Point is origin");
    }
}
```

<details>
  <summary>Hint 1</summary>

  This is a helpful hint! Read me to understand what to do!

</details>
<details>
  <summary>Hint 2</summary>

  This is a helpful hint! Read me to understand what to do!

</details>
<details>
  <summary>Hint 3</summary>

  This is a helpful hint! Read me to understand what to do!

</details>
<details>
  <summary>Solution</summary>
  
  ```rust
struct Point(f32, f32);

impl Point {
    fn on_x_axis(&self) -> bool {
        self.1 == 0.0
    }
    fn on_y_axis(&self) -> bool {
        self.0 == 0.0
    }
}

fn main() {
    let point = Point(0.0, 0.0);
    if point.on_x_axis() && point.on_y_axis() {
        println!("Point is origin");
    }
}
  ```
</details>
