# Dereferencing Raw Pointers

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/9E2v8pCUc48?si=7I-8tqRcdP8ymJlm&amp;start=153" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Immutable raw pointers

```rust,editable,compile_fail
// Fix the code to make it compile. You may not modify any statement.

fn main() {
    let num = 123;
    let ptr = &num as *const i32;
    println!("{} stored at {:p}", *ptr, ptr);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let num = 123;
    let ptr = &num as *const i32;
    unsafe {
        println!("{} stored at {:p}", *ptr, ptr);  
    }
}
  ```
</details>

#### Mutable raw pointers

```rust,editable,compile_fail
// Fix the code to make it compile.

fn main() {
    // print first 10 fibonacci numbers
    let (mut a, mut b) = (1, 0);
    let mut c = 0;
    let ptr_a = &mut a as *const i32;
    let ptr_b = &mut b as *const i32;
    let ptr_c = &mut c as *const i32;
    for _ in 0..10 {
        *ptr_c = *ptr_a + *ptr_b;
        println!("{}", *ptr_c);
        *ptr_a = *ptr_b;
        *ptr_b = *ptr_c;
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    // print first 10 fibonacci numbers
    let (mut a, mut b) = (1, 0);
    let mut c = 0;
    let ptr_a = &mut a as *mut i32;
    let ptr_b = &mut b as *mut i32;
    let ptr_c = &mut c as *mut i32;
    unsafe {
        for _ in 0..10 {
            *ptr_c = *ptr_a + *ptr_b;
            println!("{}", *ptr_c);
            *ptr_a = *ptr_b;
            *ptr_b = *ptr_c;
        }
    }
}
  ```
</details>

#### Multiple pointers

```rust,editable,compile_fail
// Fix the code to make it compile.

macro_rules! ptr {
    ($type:ty, $var:ident) => {
        &mut $var as *mut $type
    };
}

fn main() {
    let mut x = 20;
    let ptr1 = ptr!(i32, x);
    let ptr2 = ptr!(i32, x);
    println!("x: {x}");
    *ptr1 = *ptr1 * 2;
    *ptr2 = *ptr2 * 2;
    *ptr2 = *ptr1 * 2;
    println!("x * 8 = {x}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
macro_rules! ptr {
    ($type:ty, $var:ident) => {
        &mut $var as *mut $type
    };
}

fn main() {
    let mut x = 20;
    let ptr1 = ptr!(i32, x);
    let ptr2 = ptr!(i32, x);
    println!("x: {x}");
    unsafe {
        *ptr1 = *ptr1 * 2;
        *ptr2 = *ptr2 * 2;
        *ptr2 = *ptr1 * 2; 
    }
    println!("x * 8 = {x}");
}
  ```
</details>
