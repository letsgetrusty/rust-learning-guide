# Declarative Macros

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/KsJHlqULpO4?si=UvLU-nv87vStUZMX&amp;start=18" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/dHv8FhcAl5U" target="_blank">A Practical Introduction to Declarative Macros in Rust</a>

## Exercises

#### Invocation

```rust,editable,compile_fail
// Fix the code to make it compile.

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro();
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro!();
}
  ```
</details>

#### Scope 1

```rust,editable,compile_fail
// Everything seems correct, but the code does not compile. Maybe it has to do with the position of defining a macro.

fn main() {
    my_macro!();
}

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro!();
}
  ```
</details>

#### Scope 2

```rust,editable,compile_fail
// Fix the code by bringing `my_macros` in scope (You have to mark `macros` module with something).

mod macros {
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
#[macro_use]
mod macros {
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}
  ```
</details>

#### Multiple matchers 1

```rust,editable,compile_fail
// Fix the code to make it compile. You can not remove anything.

#[rustfmt::skip]
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    }
    ($val:expr) => {
        println!("Look at this other macro: {}", $val);
    }
}

fn main() {
    my_macro!();
    my_macro!(7777);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
#[rustfmt::skip]
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
    ($val:expr) => {
        println!("Look at this other macro: {}", $val);
    };
}

fn main() {
    my_macro!();
    my_macro!(7777);
}
  ```
</details>

#### Multiple matchers 2

```rust,editable,compile_fail
// We are trying to create a macro called `vec2`, which has the same functionality as the `vec` macro.
// Complete the transcriber for each matcher.

macro_rules! vec2 {
    () => {};
    ($($a:expr),+ $(,)?) => {{}};
    ($m:expr; $n:expr) => {{}};
}

#[cfg(test)]
mod tests {

    #[test]
    fn creates_empty_vector() {
        let first: Vec<i32> = vec![];
        let second: Vec<i32> = vec2![];
        assert_eq!(first, second);
    }

    #[test]
    fn creates_vector_from_list() {
        assert_eq!(vec![1, 2, 3,], vec2![1, 2, 3,]);
        assert_eq!(vec!['a', 'b', 'b', 'a'], vec2!['a', 'b', 'b', 'a']);
    }

    #[test]
    fn creates_vector_with_repeating_element() {
        assert_eq!(vec![5; 10], vec2![5;10]);
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
macro_rules! vec2 {
    () => {
        Vec::new()
    };
    ($($a:expr),+ $(,)?) => {
        {
            let mut res = Vec::new();
            $( res.push($a); )+
            res
        }
    };
    ($m:expr; $n:expr) => {
        {
            let mut res = Vec::new();
            for _ in 0..$n {
                res.push($m.clone());
            }
            res
        }
    };
}

#[cfg(test)]
mod tests {

    #[test]
    fn creates_empty_vector() {
        let first: Vec<i32> = vec![];
        let second: Vec<i32> = vec2![];
        assert_eq!(first, second);
    }

    #[test]
    fn creates_vector_from_list() {
        assert_eq!(vec![1, 2, 3,], vec2![1, 2, 3,]);
        assert_eq!(vec!['a', 'b', 'b', 'a'], vec2!['a', 'b', 'b', 'a']);
    }

    #[test]
    fn creates_vector_with_repeating_element() {
        assert_eq!(vec![5; 10], vec2![5;10]);
    }
}
  ```
</details>

#### Repetition

```rust,editable,compile_fail
// Complete the definition of `sum`.

macro_rules! sum {
    () => {
        {
            let mut sum = 0;
            $( sum += $a; )+
        }
    };
}

fn main() {
    assert_eq!(sum!(1, 2, 3), 6);
    assert_eq!(sum!(10u8, 20u8), 30);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
macro_rules! sum {
    ($($a:expr),+) => {
        {
            let mut sum = 0;
            $( sum += $a; )+
            sum
        }
    };
}

fn main() {
    assert_eq!(sum!(1, 2, 3), 6);
    assert_eq!(sum!(10u8, 20u8), 30);
}
  ```
</details>
