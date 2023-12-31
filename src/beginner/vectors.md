# Vectors

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/Zs-pS-egQSs?si=lrwiLVc8RgI8mgX4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/GcsAQTMYR1M" target="_blank">Rust Programming Tutorial #24 - Vectors</a>

## Exercises

#### Pushing


```rust,editable,compile_fail
// Complete the function to make the program execute successfully.

fn append(nums, num) {
}

fn main() {
    let mut nums = vec![1, 2, 5, 6];
    append(&mut nums, 8);
    append(&mut nums, 3);
    assert_eq!(nums.len(), 6);
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
fn append(nums: &mut Vec<i32>, num: i32) {
    nums.push(num);
}

fn main() {
    let mut nums = vec![1, 2, 5, 6];
    append(&mut nums, 8);
    append(&mut nums, 3);
    assert_eq!(nums.len(), 6);
}
  ```
</details>

#### Removing

```rust,editable,compile_fail
// Complete the function to make the program execute successfully.

fn remove_if_odd(nums, index) {
    if index > nums.len() {
        println!("Index out of bounds");
        return;
    }
}

fn main() {
    let mut nums = vec![1, 2, 6, 9];
    let nums_ref = &mut nums;
    remove_if_odd(nums_ref, 0);
    remove_if_odd(nums_ref, 1);
    remove_if_odd(nums_ref, nums_ref.len() - 1);
    assert_eq!(nums.len(), 2);
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
fn remove_if_odd(nums: &mut Vec<i32>, index: usize) {
    if index > nums.len() {
        println!("Index out of bounds");
        return;
    }
    if nums[index] % 2 == 1 {
        nums.remove(index);
    }
}

fn main() {
    let mut nums = vec![1, 2, 6, 9];
    let nums_ref = &mut nums;
    remove_if_odd(nums_ref, 0);
    remove_if_odd(nums_ref, 1);
    remove_if_odd(nums_ref, nums_ref.len() - 1);
    assert_eq!(nums.len(), 2);
}
  ```
</details>

#### Fetching

```rust,editable,compile_fail
// Fix the code so that it compiles.

fn main() {
    let names = vec!["Alice", "Bob", "Cindy"];
    let index = 2;
    if names.get(index) {
        println!("{name} is present at index {index}");
    } else {
        println!("invalid index {index}");
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let names = vec!["Alice", "Bob", "Cindy"];
    let index = 2;
    if let Some(name) = names.get(index) {
        println!("{name} is present at index {index}");
    } else {
        println!("invalid index {index}");
    }
}
  ```
</details>

#### Iterating

```rust,editable,compile_fail
// Fix the code so that it compiles.

struct Student {
    name: String,
    marks: u8,
}

impl Student {
    fn new(name: &str, marks: u8) -> Self {
        Self {
            name: name.to_string(),
            marks,
        }
    }
}

fn main() {
    let students = vec![
        Student::new("Harry", 75),
        Student::new("Hermoine", 99),
        Student::new("Ron", 60),
    ];
    let mut grades = Vec::new();
    for student in students {
        if student.marks > 80 {
            grades.push('A');
        } else if student.marks > 60 {
            grades.push('B');
        } else {
            grades.push('C');
        }
    }
    for i in 0..grades.len() {
        println!("{} got {}!", students[i].name, grades[i]);
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
struct Student {
    name: String,
    marks: u8,
}

impl Student {
    fn new(name: &str, marks: u8) -> Self {
        Self {
            name: name.to_string(),
            marks,
        }
    }
}

fn main() {
    let students = vec![
        Student::new("Harry", 75),
        Student::new("Hermoine", 99),
        Student::new("Ron", 60),
    ];
    let mut grades = Vec::new();
    for student in students.iter() {
        if student.marks > 80 {
            grades.push('A');
        } else if student.marks > 60 {
            grades.push('B');
        } else {
            grades.push('C');
        }
    }
    for i in 0..grades.len() {
        println!("{} got {}!", students[i].name, grades[i]);
    }
}
  ```
</details>

#### Iterating & mutation

```rust,editable,compile_fail
// Fix the code so that it compiles.

struct Student {
    name: String,
    marks: u8,
    grade: char,
}

impl Student {
    fn new(name: &str, marks: u8) -> Self {
        Self {
            name: name.to_string(),
            marks,
            grade: 'X',
        }
    }
}

fn main() {
    let students = vec![
        Student::new("Harry", 75),
        Student::new("Hermoine", 99),
        Student::new("Ron", 60),
    ];
    for student in students {
        student.grade = if student.marks > 80 {
            'A'
        } else if student.marks > 60 {
            'B'
        } else {
            'C'
        };
    }
    for student in students {
        println!("{} got {}!", student.name, student.grade);
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
struct Student {
    name: String,
    marks: u8,
    grade: char,
}

impl Student {
    fn new(name: &str, marks: u8) -> Self {
        Self {
            name: name.to_string(),
            marks,
            grade: 'X',
        }
    }
}

fn main() {
    let mut students = vec![
        Student::new("Harry", 75),
        Student::new("Hermoine", 99),
        Student::new("Ron", 60),
    ];
    for student in students.iter_mut() {
        student.grade = if student.marks > 80 {
            'A'
        } else if student.marks > 60 {
            'B'
        } else {
            'C'
        };
    }
    for student in students {
        println!("{} got {}!", student.name, student.grade);
    }
}
  ```
</details>
