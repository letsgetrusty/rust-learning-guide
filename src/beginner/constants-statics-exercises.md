# Constants & Statics

## Constants

<details>
  <summary>Description</summary>
  
  This is a helpful description. Read me to understand what to do!

</details>

```rust,editable,compile_fail
// Fix the code so it compiles!

const NUMBER = 3;

fn main() {
    println!("Number {}", NUMBER);
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
const NUMBER: i32 = 3;

fn main() {
    println!("Number {}", NUMBER);
}
  ```
</details>

## Statics

<details>
  <summary>Description</summary>
  
  This is a helpful description. Read me to understand what to do!

</details>

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


