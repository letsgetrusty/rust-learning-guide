# Variables

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/2V0JaMVjzws?si=-KNBHl1ak5KfmMCy&amp;clip=UgkxjiDk9ulxk7ckRIzz95zAAqoI80sLPpn5&amp;clipt=EKSgAhiv3gU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/gmInx5c81sM" target="_blank">Shadowing in Rust</a>

## Exercises

#### Declaration

```rust,editable,compile_fail
// Fix the variable definition of 'x'

fn main() {
    x = 5;
    println!("x has the value {}", x);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let x = 5;
    println!("x has the value {}", x);
}
  ```
</details>


#### Declaration with reassignment

```rust,editable,compile_fail
// Fix the variable definition of 'x'

fn main() {
    let x = 3;
    println!("Number {}", x);
    x = 5; // don't change this line
    println!("Number {}", x);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let mut x = 3;
    println!("Number {}", x);
    x = 5; // don't change this line
    println!("Number {}", x);
}
  ```
</details>


#### Declaration with shadowing

```rust,editable,compile_fail
// Fix this code with shadowing

fn main() {
    let x = "three"; // don't change this line
    println!("Spell a Number : {}", x);
    x = 3; // don't rename this variable
    println!("Number plus two is : {}", x + 2);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let x = "three"; // don't change this line
    println!("Spell a Number : {}", x);
    let x = 3; // don't rename this variable
    println!("Number plus two is : {}", x + 2);
}
  ```
</details>