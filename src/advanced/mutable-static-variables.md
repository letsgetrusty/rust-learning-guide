# Mutable Static Variables

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/9E2v8pCUc48?si=7I-8tqRcdP8ymJlm&amp;start=730" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Mutating

```rust,editable,compile_fail
// Fix the code to make it compile.

static COUNTER: u32 = 0;

// return fibonacci number corresponding to specified position
fn fib(num: u32) -> u32 {
    unsafe {
        COUNTER += 1;
    }
    if num <= 1 {
        num
    } else {
        fib(num - 1) + fib(num - 2)
    }
}

fn main() {
    let num = fib(5);
    println!("Fibonacci number at position 5: {num}");
    println!("Number of function calls made to calculate: {COUNTER}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
static mut COUNTER: u32 = 0;

// return fibonacci number corresponding to specified position
fn fib(num: u32) -> u32 {
    unsafe {
        COUNTER += 1;
    }
    if num <= 1 {
        num
    } else {
        fib(num - 1) + fib(num - 2)
    }
}

fn main() {
    let num = fib(5);
    println!("Fibonacci number at position 5: {num}");
    unsafe {
        println!("Number of function calls made to calculate: {COUNTER}");
    }
}
  ```
</details>
