# RefCell Smart Pointer

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/77aRH6YBKyY?si=26YzyT-fhfIhp-KT&amp;start=70" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Borrowing

```rust,editable,compile_fail
// Complete the following code.

use std::cell::RefCell;

fn main() {
    // storing value 5 on heap
    let ptr = RefCell::new(5);
    // get an immutable reference to the stored value
    let ref1 = ;
    println!("Stored value: {}", ref1);
    drop(ref1);
    // get a mutable reference to the stored value
    let mut ref2 = ;
    *ref2 = 6; // Note: we can mutate the value associated with ptr, even though it is not marked as mut
    println!("Stored value: {}", ref2);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::cell::RefCell;

fn main() {
    // storing value 5 on heap
    let ptr = RefCell::new(5);
    // get an immutable reference to the stored value
    let ref1 = ptr.borrow();
    println!("Stored value: {}", ref1);
    drop(ref1);
    // get a mutable reference to the stored value
    let mut ref2 = ptr.borrow_mut();
    *ref2 = 6; // Note: we can mutate the value associated with ptr, even though it is not marked as mut
    println!("Stored value: {}", ref2);
}
  ```
</details>

#### Interior mutability

<details>
  <summary>Description</summary>
  
  This is a helpful description. Read me to understand what to do!

</details>

```rust,editable,compile_fail
// Fix the print_details method. You can only modify the method body.

use std::cell::RefCell;

struct Student {
    name: String,
    marks: u8,
    grade: RefCell<char>,
}

impl Student {
    fn new(name: &str, marks: u8) -> Self {
        Student {
            name: name.to_owned(),
            marks,
            grade: RefCell::new('X'),
        }
    }

    fn print_details(&self) {
        let grade = match self.marks {
            0..=33 => 'C',
            34..=60 => 'B',
            _ => 'A',
        };
        self.grade = grade;
        println!(
            "name: {}, marks: {}, grade: {}",
            self.name,
            self.marks,
            self.grade.borrow()
        );
    }
}

fn main() {
    let student = Student::new("Harry", 70);
    student.print_details();
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::cell::RefCell;

struct Student {
    name: String,
    marks: u8,
    grade: RefCell<char>,
}

impl Student {
    fn new(name: &str, marks: u8) -> Self {
        Student {
            name: name.to_owned(),
            marks,
            grade: RefCell::new('X'),
        }
    }

    fn print_details(&self) {
        let grade = match self.marks {
            0..=33 => 'C',
            34..=60 => 'B',
            _ => 'A',
        };
        *self.grade.borrow_mut() = grade;
        println!(
            "name: {}, marks: {}, grade: {}",
            self.name,
            self.marks,
            self.grade.borrow()
        );
    }
}

fn main() {
    let student = Student::new("Harry", 70);
    student.print_details();
}
  ```
</details>
