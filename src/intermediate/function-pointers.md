# Function Pointers

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/rnxutl7t1hI?si=iuy6CJKBsf3opZBU&amp;start=23" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### As parameters

```rust,editable,compile_fail
// Complete the function signature for `factorial`. It must not contain any generics/traits.

fn decrement(x: u32) -> u32 {
    x - 1
}

fn multiply(x: u32, y: u32) -> u32 {
    x * y
}

fn factorial(num, dec, mul) {
    let mut res = 1;
    let mut temp = num;
    while temp > 1 {
        res = mul(res, temp);
        temp = dec(temp);
    }
    res
}

fn main() {
    let num = 6;
    let fact = factorial(num, decrement, multiply);
    println!("{num}! = {fact}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn decrement(x: u32) -> u32 {
    x - 1
}

fn multiply(x: u32, y: u32) -> u32 {
    x * y
}

fn factorial(num: u32, dec: fn(u32)->u32, mul: fn(u32, u32)->u32) -> u32 {
    let mut res = 1;
    let mut temp = num;
    while temp > 1 {
        res = mul(res, temp);
        temp = dec(temp);
    }
    res
}

fn main() {
    let num = 6;
    let fact = factorial(num, decrement, multiply);
    println!("{num}! = {fact}");
}
  ```
</details>

#### Coercing from closures

```rust,editable,compile_fail
// Fix the code by addressing the TODO.

fn invoker<T>(logic: fn(T), arg: T) {
    logic(arg);
}

fn main() {
    // TODO: shift below declaration to somewhere else
    let greeting = String::from("Nice to meet you");
    let greet = |name| {
        println!("{greeting} {name}!");
    };
    invoker(greet, "Jenny");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn invoker<T>(logic: fn(T), arg: T) {
    logic(arg);
}

fn main() {
    let greet = |name| {
        let greeting = String::from("Nice to meet you");
        println!("{greeting} {name}!");
    };
    invoker(greet, "Jenny");
}
  ```
</details>
