# Implementing Unsafe Traits

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/9E2v8pCUc48?si=7I-8tqRcdP8ymJlm&amp;start=843" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Unsafe traits

```rust,editable,compile_fail
// Fix the code to make it compile. Do not modify trait definition.

unsafe trait Length {
    fn length(&self) -> usize;
}

impl Length for String {
    fn length(&self) -> usize {
        self.len()
    }
}

impl Length for i32 {
    fn length(&self) -> usize {
        match self {
            -9..=9 => 1,
            _ => 1 + (self / 10).length(),
        }
    }
}

fn main() {
    let my_str = "Unsafe Traits".to_owned();
    let my_num = 12323;
    println!("\"{my_str}\" takes {} bytes", my_str.length());
    println!("{my_num} has {} digits", my_num.length());
}

```

<details>
  <summary>Solution</summary>
  
  ```rust
unsafe trait Length {
    fn length(&self) -> usize;
}

unsafe impl Length for String {
    fn length(&self) -> usize {
        self.len()
    }
}

unsafe impl Length for i32 {
    fn length(&self) -> usize {
        match self {
            -9..=9 => 1,
            _ => 1 + (self / 10).length(),
        }
    }
}

fn main() {
    let my_str = "Unsafe Traits".to_owned();
    let my_num = 12323;
    println!("\"{my_str}\" takes {} bytes", my_str.length());
    println!("{my_num} has {} digits", my_num.length());
}
  ```
</details>
