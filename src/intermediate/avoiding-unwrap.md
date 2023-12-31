# Avoiding Unwrap

> Note: No official YouTube video yet, but our <a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">Rust Developer Bootcamp</a> covers this topic!

<a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">
    <img src="https://d1aettbyeyfilo.cloudfront.net/letsgetrusty/31007320_1703634176QCJNo_video_yet.png" alt="No video yet..."/>
</a>

## Exercises

#### Avoiding unwrap

```rust,editable,compile_fail
// Modify `get_last` to handle all cases and make the program execute successfully.

fn get_last(nums: &mut Vec<i32>) -> i32 {
    nums.pop().unwrap()
}

fn main() {
    let mut vec1 = vec![1, 2, 3];
    let mut vec2 = vec![];
    assert!(matches!(get_last(&mut vec1), Ok(3)));
    assert!(matches!(get_last(&mut vec2), Err("Empty vector")));
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn get_last(nums: &mut Vec<i32>) -> Result<i32, &str> {
    if nums.len() == 0 {
        return Err("Empty vector");
    }
    Ok(nums.pop().unwrap())
}

fn main() {
    let mut vec1 = vec![1, 2, 3];
    let mut vec2 = vec![];
    assert!(matches!(get_last(&mut vec1), Ok(3)));
    assert!(matches!(get_last(&mut vec2), Err("Empty vector")));
}
  ```
</details>
