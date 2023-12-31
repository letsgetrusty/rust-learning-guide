# Concret Lifetimes

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/juIINGuZyBc?si=32FoZB4TiT2U-l1_&amp;start=15" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Lifetimes of owned values

```rust,editable,compile_fail
// Fix the code by shifting only one statement.

fn main() {
    let str1 = "🦀".to_string();
    let bytes = str1.into_bytes();
    println!("A crab: {str1}");
    println!("A crab represented in unicode: {bytes:?}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let str1 = "🦀".to_string();
    println!("A crab: {str1}");
    let bytes = str1.into_bytes();
    println!("A crab represented in unicode: {bytes:?}");
}
  ```
</details>

#### Dangling references

```rust,editable,compile_fail
// Fix the code by addressing the TODO.

fn main() {
    let num_ref;
    {
        // TODO: shift below statement to appropriate location
        let num = 23;
        num_ref = &num;
    }
    println!("Reference points to {}", num_ref);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let num_ref;
    let num = 23;
    {
        num_ref = &num;
    }
    println!("Reference points to {}", num_ref);
}
  ```
</details>

#### Non-lexical lifetimes


```rust,editable,compile_fail
// Fix the code by shifting only one statement.

fn main() {
    let mut my_str = "Old String".to_owned();
    let ref1 = &my_str;
    let ref2 = &mut my_str;
    ref2.replace_range(0..3, "New");
    println!("{ref1}");
    println!("{ref2}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let mut my_str = "Old String".to_owned();
    let ref1 = &my_str;
    println!("{ref1}");
    let ref2 = &mut my_str;
    ref2.replace_range(0..3, "New");
    println!("{ref2}");
}
  ```
</details>
