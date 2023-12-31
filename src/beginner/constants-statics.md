# Constants & Statics

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/2V0JaMVjzws?si=3PFzQBx1nbULg5gc&amp;start=94" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://www.youtube.com/embed/LJ_tHdhkkng?si=722F6IQLCW5rOYEc&amp;start=388" target="_blank">Static variables in Rust</a>

## Exercises

#### Constants

```rust,editable,compile_fail
// Fix the code so it compiles!

const NUMBER = 3;

fn main() {
    println!("Number {}", NUMBER);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
const NUMBER: i32 = 3;

fn main() {
    println!("Number {}", NUMBER);
}
  ```
</details>

#### Statics

```rust,editable,compile_fail
// Fix the code so it compiles!

static LANGUAGE = "Go";

fn main() {
    // These two initializations perform a string copy from same memory location
    let lang1 = LANGUAGE;
    let mut lang2 = LANGUAGE;
    
    lang2 = "Rust";
    println!("I like {} more than {}!", lang2, lang1);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
static LANGUAGE: &str = "Go";

fn main() {
    // These two initializations perform a string copy from same memory location
    let lang1 = LANGUAGE;
    let mut lang2 = LANGUAGE;
    
    lang2 = "Rust";
    println!("I like {} more than {}!", lang2, lang1);
}

  ```
</details>