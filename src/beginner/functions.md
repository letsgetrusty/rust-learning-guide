# Functions

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/2V0JaMVjzws?si=vyhHpm417BFIQGiz&amp;start=465" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Definition

```rust,editable,compile_fail
// Something's missing from the function definition. Fix it so it compiles!

fn main() {
    call_me(9395550113);
}

fn call_me(num:) {
    println!("Ring! Call number {}", num);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    call_me(9395550113);
}

fn call_me(num: i64) {
    println!("Ring! Call number {}", num);
}
  ```
</details>

#### Return types

```rust,editable,compile_fail
// This store is having a sale where if the price is an even number, you get
// 10 Rustbucks off, but if it's an odd number, it's 3 Rustbucks off.
// But it won't compile--fix it
// (Don't worry about the function bodies themselves, we're only interested
// in the signatures for now)

fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
}

fn sale_price(price: i32) -> {
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
}

fn is_even(num: i32) -> bool {
    num % 2 == 0
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
}

fn sale_price(price: i32) -> i32 {
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
}

fn is_even(num: i32) -> bool {
    num % 2 == 0
}
  ```
</details>

#### Return keyword

```rust,editable,compile_fail
// This function describes a morning in the life of a Rust programmer.
// She has a few morning rituals, but she skips all that if she wakes up late!
// Fix the morning_routine function to return early when the Rust programmer wakes up late.

fn main() {
    let i_woke_up_late = true;
    morning_routine(i_woke_up_late);
}

fn morning_routine(i_am_late: bool) {
    println!("This morning, I...");
    if i_am_late {
        go_to_work();
    }
    exercise();
    eat_breakfast();
    make_coffee();
    go_to_work();
}

fn exercise() {
    println!("I went to the gym.");
}

fn eat_breakfast() {
    println!("I had a healthy breakfast!");
}

fn make_coffee() {
    println!("I made myself coffee. Now that I'm ready..."); 
}

fn go_to_work() {
    println!("I went straight to work!");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let i_woke_up_late = true;
    morning_routine(i_woke_up_late);
}

fn morning_routine(i_am_late: bool) {
    println!("This morning, I...");
    if i_am_late {
        go_to_work();
        return;
    }
    exercise();
    eat_breakfast();
    make_coffee();
    go_to_work();
}

fn exercise() {
    println!("I went to the gym.");
}

fn eat_breakfast() {
    println!("I had a healthy breakfast!");
}

fn make_coffee() {
    println!("I made myself coffee. Now that I'm ready..."); 
}

fn go_to_work() {
    println!("I went straight to work!");
}

  ```
</details>
