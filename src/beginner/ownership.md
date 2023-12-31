# Ownership

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/VFIOSWy93H0?si=FYS17f47UbfpZ0Hm&amp;start=19" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/ebnICYrM9zs" target="_blank">5. Rust ownership system</a>

## Exercises

#### Moving on assignment

```rust,editable,compile_fail
// Something's missing. Fix the code so that it compiles.

fn main() {
    let s1 = String::from("Rust");
    let mut s2 = s1;
    s2.push_str(" is an awesome language");
    println!("String:\"{s1}\" is a substring of \"{s2}\"");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
  fn main() {
    let s1 = String::from("Rust");
    let mut s2 = s1.clone();
    s2.push_str(" is an awesome language");
    println!("String:\"{s1}\" is a substring of \"{s2}\"");
  }
  ```
</details>

#### Moving on assignment 2

```rust,editable,compile_fail
// Fix the code so that it compiles. Modify only one statement.

fn main() {
    let mut my_str = String::from("Example");
    let mut temp;
    while my_str.len() > 0 {
        temp = my_str;
        println!("Length of temporary string is: {}", temp.len());
        my_str.pop();
    }
}
```

<details>
  <summary>Solution</summary>

<details>
  <summary>Description</summary>

The ownership of a value is assigned upon the assignment of the value itself. We need to be aware of the ownership of a value every time an assignment of a value occurs.

</details>

  ```rust
  fn main() {
    let mut my_str = String::from("Example");
    let mut temp;
    while my_str.len() > 0 {
        temp = my_str.clone();
        println!("Length of temporary string is: {}", temp.len());
        my_str.pop();
    }
  }
  ```
</details>

#### Moving into function

```rust,editable,compile_fail
// Fix the code so that it compiles.

fn main() {
    let my_string = String::from("I love rust bootcamp 💕");
    let occurence_count = count_occurences(my_string, 'o');
    println!("The number of times 'o' apprears in \"{my_string}\" = {occurence_count}");
}

// this function counts the number of times a letter appears in a text
fn count_occurences(text: String, letter: char) -> u32 {
    let mut res = 0;
    for ch in text.chars() {
        if ch == letter {
            res += 1;
        }
    }
    res
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
  fn main() {
    let my_string = String::from("I love rust bootcamp 💕");
    let occurence_count = count_occurences(&my_string, 'o');
    println!("The number of times 'o' apprears in \"{my_string}\" = {occurence_count}");
    }

    // this function counts the number of times a letter appears in a text
  fn count_occurences(text: &String, letter: char) -> u32 {
    let mut res = 0;
    for ch in text.chars() {
        if ch == letter {
            res += 1;
        }
    }
    res
  }
  ```
</details>

#### Moving out of function

```rust,editable,compile_fail
// Make the following code compile by modifying only one statement.

fn main() {
    let mut str1 = get_new_string();
    println!("Printing through str1: {}", str1);
    let mut str2 = str1;
    println!("Printing through str2: {}", str2);
    str1 = str2;
    println!("Again printing through str1: {}", str1);
    str2 = str1;
    println!("Again printing through str2: {}", str2);
    println!("Printing thourgh both: {}, {}", str1, str2);
}

fn get_new_string() -> String {
    let new_string = String::from("I will master rust 🦀 🦀");
    new_string
} // string ownership is transferred to the calling function
```
<details>
  <summary>Solution</summary>
  
  ```rust
  fn main() {
    let mut str1 = get_new_string();
    println!("Printing through str1: {}", str1);
    let mut str2 = str1;
    println!("Printing through str2: {}", str2);
    str1 = str2;
    println!("Again printing through str1: {}", str1);
    str2 = str1.clone();
    println!("Again printing through str2: {}", str2);
    println!("Printing thourgh both: {}, {}", str1, str2);
  }

  fn get_new_string() -> String {
    let new_string = String::from("I will master rust 🦀 🦀");
    new_string
  } // string ownership is transferred to the calling function
  ```
</details>