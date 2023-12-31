# Iterator Pattern

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/4GcKrj4By8k?si=c8Sar05Kr5L-zKSy&amp;start=33" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Iterator trait

```rust,editable,compile_fail
// Make the code compile by implementing the Iterator trait for `Queue`.

struct Queue {
    items: Vec<i32>,
}

impl Queue {
    fn new(items: Vec<i32>) -> Self {
        Self { items }
    }
}

impl Iterator for Queue {
    type Item;
    fn next(&mut self) -> Option<Self::Item> {}
}

fn main() {
    let mut queue = Queue::new(vec![3, 2, 1]);
    assert!(matches!(queue.next(), Some(3)));
    assert!(matches!(queue.next(), Some(2)));
    assert!(matches!(queue.next(), Some(1)));
    assert!(matches!(queue.next(), None));
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct Queue {
    items: Vec<i32>,
}

impl Queue {
    fn new(items: Vec<i32>) -> Self {
        Self { items }
    }
}

impl Iterator for Queue {
    type Item = i32;
    fn next(&mut self) -> Option<Self::Item> {
        if self.items.len() > 0 {
            Some(self.items.remove(0))
        } else {
            None
        }
    }
}

fn main() {
    let mut queue = Queue::new(vec![3, 2, 1]);
    assert!(matches!(queue.next(), Some(3)));
    assert!(matches!(queue.next(), Some(2)));
    assert!(matches!(queue.next(), Some(1)));
    assert!(matches!(queue.next(), None));
}
  ```
</details>

#### Into iterator 1


```rust,editable,compile_fail
// Provide the trait implementations and make the code execute successfully.

struct Employee {
    first_name: String,
    last_name: String,
    id: String,
}

struct EmployeeIter {
    state: Vec<String>,
}

impl Iterator for EmployeeIter {
    type Item;
    fn next(&mut self) -> Option<Self::Item> {}
}

impl IntoIterator for Employee {
    type Item;
    type IntoIter;
    fn into_iter(self) -> Self::IntoIter {}
}

fn main() {
    let employee = Employee {
        first_name: "Alice".to_owned(),
        last_name: "Smith".to_owned(),
        id: "ab123".to_owned(),
    };
    let mut emp_iter = employee.into_iter();
    println!("First name: {}", emp_iter.next().unwrap());
    println!("Last name: {}", emp_iter.next().unwrap());
    println!("ID: {}", emp_iter.next().unwrap());
    assert_eq!(emp_iter.next(), None);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct Employee {
    first_name: String,
    last_name: String,
    id: String,
}

struct EmployeeIter {
    state: Vec<String>,
}

impl Iterator for EmployeeIter {
    type Item = String;
    fn next(&mut self) -> Option<Self::Item> {
        self.state.pop()
    }
}

impl IntoIterator for Employee {
    type Item = String;
    type IntoIter = EmployeeIter;
    fn into_iter(self) -> Self::IntoIter {
        EmployeeIter {
            state: vec![
                self.id,
                self.last_name,
                self.first_name,
            ]
        }
    }
}

fn main() {
    let employee = Employee {
        first_name: "Alice".to_owned(),
        last_name: "Smith".to_owned(),
        id: "ab123".to_owned(),
    };
    let mut emp_iter = employee.into_iter();
    println!("First name: {}", emp_iter.next().unwrap());
    println!("Last name: {}", emp_iter.next().unwrap());
    println!("ID: {}", emp_iter.next().unwrap());
    assert_eq!(emp_iter.next(), None);
}
  ```
</details>

#### Into iterator 2

```rust,editable,compile_fail
// Fix the code by completing the into_iter() method.

struct Employee {
    first_name: String,
    last_name: String,
    id: String,
}

impl IntoIterator for Employee {
    type Item = String;
    type IntoIter = std::vec::IntoIter<String>;
    fn into_iter(self) -> Self::IntoIter {
        vec![
            format!("First name: {}", self.first_name),
            // do the same for last_name & id
        ]
    }
}

fn main() {
    let employee = Employee {
        first_name: "Alice".to_owned(),
        last_name: "Smith".to_owned(),
        id: "ab123".to_owned(),
    };
    println!("Employee Details:");
    for detail in employee {
        println!("{detail}");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct Employee {
    first_name: String,
    last_name: String,
    id: String,
}

impl IntoIterator for Employee {
    type Item = String;
    type IntoIter = std::vec::IntoIter<String>;
    fn into_iter(self) -> Self::IntoIter {
        vec![
            format!("First name: {}", self.first_name),
            format!("Last name: {}", self.last_name),
            format!("ID: {}", self.id),
        ].into_iter()
    }
}

fn main() {
    let employee = Employee {
        first_name: "Alice".to_owned(),
        last_name: "Smith".to_owned(),
        id: "ab123".to_owned(),
    };
    println!("Employee Details:");
    for detail in employee {
        println!("{detail}");
    }
}
  ```
</details>

#### Iterator methods

```rust,editable,compile_fail
// In this exercise, you'll learn some of the unique advantages that iterators
// can offer. Follow the steps to complete the exercise.

// Step 1.
// Complete the `capitalize_first` function.
// "hello" -> "Hello"
pub fn capitalize_first(input: &str) -> String {
    let mut c = input.chars();
    match c.next() {
        None => String::new(),
        Some(first) => ???,
    }
}

// Step 2.
// Apply the `capitalize_first` function to a slice of string slices.
// Return a vector of strings.
// ["hello", "world"] -> ["Hello", "World"]
pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    vec![]
}

// Step 3.
// Apply the `capitalize_first` function again to a slice of string slices.
// Return a single string.
// ["hello", " ", "world"] -> "Hello World"
pub fn capitalize_words_string(words: &[&str]) -> String {
    String::new()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        assert_eq!(capitalize_first("hello"), "Hello");
    }

    #[test]
    fn test_empty() {
        assert_eq!(capitalize_first(""), "");
    }

    #[test]
    fn test_iterate_string_vec() {
        let words = vec!["hello", "world"];
        assert_eq!(capitalize_words_vector(&words), ["Hello", "World"]);
    }

    #[test]
    fn test_iterate_into_string() {
        let words = vec!["hello", " ", "world"];
        assert_eq!(capitalize_words_string(&words), "Hello World");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
pub fn capitalize_first(input: &str) -> String {
    let mut c = input.chars();
    match c.next() {
        None => String::new(),
        Some(first) => format!("{}{}", first.to_uppercase(), c.as_str()),
    }
}

pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    words.iter().map(|w| capitalize_first(w)).collect()
}

pub fn capitalize_words_string(words: &[&str]) -> String {
    words.iter().map(|w| capitalize_first(w)).collect::<Vec<String>>().join("")
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        assert_eq!(capitalize_first("hello"), "Hello");
    }

    #[test]
    fn test_empty() {
        assert_eq!(capitalize_first(""), "");
    }

    #[test]
    fn test_iterate_string_vec() {
        let words = vec!["hello", "world"];
        assert_eq!(capitalize_words_vector(&words), ["Hello", "World"]);
    }

    #[test]
    fn test_iterate_into_string() {
        let words = vec!["hello", " ", "world"];
        assert_eq!(capitalize_words_string(&words), "Hello World");
    }
}
  ```
</details>
