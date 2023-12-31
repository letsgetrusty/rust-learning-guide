# Unit Tests

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/18-7NoNPO30?si=XFu_Tgw0wdxITGVa" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/-L4nKAlmH3M" target="_blank">Testing in Rust - Part 2</a>

## Exercises


#### Asserting 1

```rust,editable,compile_fail
// Make the test compile and pass successfully.

#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert() {
        assert!();
    }
}

```
<details>
  <summary>Solution</summary>
  
  ```rust
#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert() {
        assert!(1 == 1); // use any expression that evaluates to true
    }
}
  ```
</details>

#### Asserting 2

```rust,editable,compile_fail
// Make the test compile and pass successfully.

#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert_eq() {
        assert_eq!();
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert_eq() {
        assert_eq!(7+2, 9); // can use any two expressions that are equal
    }
}
  ```
</details>

#### Asserting 3

```rust,editable,compile_fail
// Make the tests compile and pass successfully.

pub fn is_even(num: i32) -> bool {
    num % 2 == 0
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_true_when_even() {
        assert!();
    }

    #[test]
    fn is_false_when_odd() {
        assert!();
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
pub fn is_even(num: i32) -> bool {
    num % 2 == 0
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_true_when_even() {
        assert!(is_even(10));
    }

    #[test]
    fn is_false_when_odd() {
        assert!(!is_even(9));
    }
}
  ```
</details>

#### Returning result

```rust,editable,compile_fail
// Complete the test function's signature.

fn calculate_sum(nums: &[i32]) -> Result<i32, String> {
    if nums.len() == 0 {
        return Err("Number list is empty".to_string());
    }
    let mut sum = 0;
    for num in nums {
        sum += num;
    }
    Ok(sum)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn calculates_sum_correctly() -> {
        let nums = [1, 2, 3, 4, 5];
        let sum = calculate_sum(&nums)?;
        assert_eq!(sum, 5 * (5 + 1) / 2);
        Ok(())
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
fn calculate_sum(nums: &[i32]) -> Result<i32, String> {
    if nums.len() == 0 {
        return Err("Number list is empty".to_string());
    }
    let mut sum = 0;
    for num in nums {
        sum += num;
    }
    Ok(sum)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn calculates_sum_correctly() -> Result<(), String> {
        let nums = [1, 2, 3, 4, 5];
        let sum = calculate_sum(&nums)?;
        assert_eq!(sum, 5 * (5 + 1) / 2);
        Ok(())
    }
}
  ```
</details>

#### Testing panics

```rust,editable,compile_fail
// Add a macro to make the test pass.

fn average(nums: &[i32]) -> i32 {
    if nums.len() == 0 {
        panic!("Empty number list");
    }
    let mut sum = 0;
    for num in nums {
        sum += num;
    }
    sum / nums.len() as i32
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_panics() {
        let nums = [];
        let _avg = average(&nums);
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
fn average(nums: &[i32]) -> i32 {
    if nums.len() == 0 {
        panic!("Empty number list");
    }
    let mut sum = 0;
    for num in nums {
        sum += num;
    }
    sum / nums.len() as i32
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn it_panics() {
        let nums = [];
        let _avg = average(&nums);
    }
}
  ```
</details>
