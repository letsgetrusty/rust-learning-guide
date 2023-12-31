# Result

> Note: No official YouTube video yet, but our <a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">Rust Developer Bootcamp</a> covers this topic!

<a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">
    <img src="https://d1aettbyeyfilo.cloudfront.net/letsgetrusty/31007320_1703634176QCJNo_video_yet.png" alt="No video yet..."/>
</a>


#### Additional Resources
- <a href="https://youtu.be/s5S2Ed5T-dc" target="_blank">A Simpler Way to See Results</a>


## Exercises

#### Mathing result

```rust,editable,compile_fail
// Complete the match statement to check whether operation succeeded or not.

enum Operation {
    Add(i32, i32),
    Mul(i32, i32),
    Sub { first: i32, second: i32 },
    Div { divident: i32, divisor: i32 },
}

impl Operation {
    fn execute(self) -> Result<i32, String> {
        match self {
            Self::Add(a, b) => Ok(a + b),
            Self::Mul(a, b) => Ok(a * b),
            Self::Sub { first, second } => Ok(first - second),
            Self::Div { divident, divisor } => {
                if divisor == 0 {
                    Err(String::from("Can not divide by zero"))
                } else {
                    Ok(divident / divisor)
                }
            }
        }
    }
}

fn main() {
    let user_input = Operation::Div {
        divident: 20,
        divisor: 0,
    };
    match user_input.execute() {
         => println!("Result: {res}"),
         => println!("Error: {e}"),
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
enum Operation {
    Add(i32, i32),
    Mul(i32, i32),
    Sub { first: i32, second: i32 },
    Div { divident: i32, divisor: i32 },
}

impl Operation {
    fn execute(self) -> Result<i32, String> {
        match self {
            Self::Add(a, b) => Ok(a + b),
            Self::Mul(a, b) => Ok(a * b),
            Self::Sub { first, second } => Ok(first - second),
            Self::Div { divident, divisor } => {
                if divisor == 0 {
                    Err(String::from("Can not divide by zero"))
                } else {
                    Ok(divident / divisor)
                }
            }
        }
    }
}

fn main() {
    let user_input = Operation::Div {
        divident: 20,
        divisor: 0,
    };
    match user_input.execute() {
        Ok(res) => println!("Result: {res}"),
        Err(e) => println!("Error: {e}"),
    }
}
  ```
</details>

#### Returning result

```rust,editable,compile_fail
// Complete the function signature.

fn greet(name: &str) -> {
    if name.len() > 0 {
        println!("Hello {name}!");
        Ok(())
    } else {
        Err("Empty name provided".to_string())
    }
}

fn main() {
    let name = "Tom";
    if let Err(e) = greet(name) {
        println!("Error: {e}");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn greet(name: &str) -> Result<(), String> {
    if name.len() > 0 {
        println!("Hello {name}!");
        Ok(())
    } else {
        Err("Empty name provided".to_string())
    }
}

fn main() {
    let name = "Tom";
    if let Err(e) = greet(name) {
        println!("Error: {e}");
    }
}
  ```
</details>
