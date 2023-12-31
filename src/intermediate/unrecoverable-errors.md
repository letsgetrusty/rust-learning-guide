# Unrecoverable Errors

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/wM6o70NAWUI?si=p80xaiRvUY_dftEm&amp;start=25" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Panicking

```rust,editable,compile_fail
// Make the program error out with appropriate message where required.

fn div(a: i32, b: i32) -> i32 {
    if b == 0 {
        // TODO: panic with msg `divide by zero error`
    }
    a / b
}

fn main() {
    let _res = div(23, 0);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn div(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("divide by zero error");
    }
    a / b
}

fn main() {
    let _res = div(23, 0);
}
  ```
</details>
