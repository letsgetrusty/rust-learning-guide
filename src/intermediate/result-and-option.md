# Result and Option

> Note: No official YouTube video yet, but our <a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">Rust Developer Bootcamp</a> covers this topic!

<a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">
    <img src="https://d1aettbyeyfilo.cloudfront.net/letsgetrusty/31007320_1703634176QCJNo_video_yet.png" alt="No video yet..."/>
</a>

## Exercises

#### Option to Result

```rust,editable,compile_fail
// Fix the `fetch_last` function. Do not add any other statement.

fn fetch_last<T>(list: &mut Vec<T>) -> Option<T> {
    list.pop().ok_or()
}

fn main() {
    let mut my_nums = Vec::<i32>::new();
    match fetch_last(&mut my_nums) {
        Ok(ele) => println!("Last element: {ele}"),
        Err(error) => {
            println!("Error: {error}");
            assert_eq!(error, "Empty list".to_owned());
        }
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn fetch_last<T>(list: &mut Vec<T>) -> Result<T, String> {
    list.pop().ok_or("Empty list".to_owned())
}

fn main() {
    let mut my_nums = Vec::<i32>::new();
    match fetch_last(&mut my_nums) {
        Ok(ele) => println!("Last element: {ele}"),
        Err(error) => {
            println!("Error: {error}");
            assert_eq!(error, "Empty list".to_owned());
        }
    }
}
  ```
</details>

#### Result to Option

```rust,editable,compile_fail
// Use `ok` combinator to convert Result to Option.
// Do not add any statements anywhere.

fn add(num1: &str, num2: &str) -> Option<u8> {
    // TODO: only modify the 2 statements below
    let num1 = num1.parse::<u8>();
    let num2 = num2.parse::<u8>();
    num1.checked_add(num2)
}

fn main() {
    let (num1, num2) = ("4", "5");
    if let Some(sum) = add("4", "5") {
        println!("{num1} + {num2} = {sum}");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn add(num1: &str, num2: &str) -> Option<u8> {
    let num1 = num1.parse::<u8>().ok()?;
    let num2 = num2.parse::<u8>().ok()?;
    num1.checked_add(num2)
}

fn main() {
    let (num1, num2) = ("4", "5");
    if let Some(sum) = add("4", "5") {
        println!("{num1} + {num2} = {sum}");
    }
}
  ```
</details>
