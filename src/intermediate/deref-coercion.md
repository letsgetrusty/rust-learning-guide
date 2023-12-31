# Deref Coercion

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/dYEC6NElVOg?si=IhHelWRpm2k6AITo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Deref coercion with Box

```rust,editable,compile_fail
// Complete the code by addressing the TODOs.

use std::rc::Rc;

struct Employee {
    name: String,
    id: u32,
}

impl Employee {
    fn new(name: &str, id: u32) -> Self {
        Employee {
            name: name.to_string(),
            id,
        }
    }
    fn print_details(&self) {
        println!("Name: {}, ID: {}", self.name, self.id);
    }
}

fn main() {
    let emp1 = Box::new(Employee::new("Alice", 1234));
    // TODO: call print_details on emp1
    let emp2 = Box::new(emp1);
    // TODO: call print_details on emp2
    let emp3 = Rc::new(emp2);
    // TODO: call print_details on emp3
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::rc::Rc;

struct Employee {
    name: String,
    id: u32,
}

impl Employee {
    fn new(name: &str, id: u32) -> Self {
        Employee {
            name: name.to_string(),
            id,
        }
    }
    fn print_details(&self) {
        println!("Name: {}, ID: {}", self.name, self.id);
    }
}

fn main() {
    let emp1 = Box::new(Employee::new("Alice", 1234));
    emp1.print_details();
    let emp2 = Box::new(emp1);
    emp2.print_details();
    let emp3 = Rc::new(emp2);
    emp3.print_details();
}
  ```
</details>

#### Deref & deref mut

```rust,editable,compile_fail
// Make the code compile by implementing Deref & DerefMut for Wrapper.

use std::ops::{Deref, DerefMut};

struct Wrapper<T>(T);

impl<T> Deref for Wrapper<T> {
    type Target;
    fn deref(&self) -> &Self::Target {}
}

impl<T> DerefMut for Wrapper<T> {
    fn deref_mut(&mut self) -> &mut Self::Target {}
}

fn main() {
    let mut my_str = Wrapper(String::from("Ferris"));
    my_str.push_str(" the crab!!");
    my_str.pop();
    assert!(are_equal(&my_str, "Ferris the crab!"));
}

fn are_equal(a: &str, b: &str) -> bool {
    a == b
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::ops::{Deref, DerefMut};

struct Wrapper<T>(T);

impl<T> Deref for Wrapper<T> {
    type Target = T;
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

impl<T> DerefMut for Wrapper<T> {
    fn deref_mut(&mut self) -> &mut Self::Target {
        &mut self.0
    }
}

fn main() {
    let mut my_str = Wrapper(String::from("Ferris"));
    my_str.push_str(" the crab!!");
    my_str.pop();
    assert!(are_equal(&my_str, "Ferris the crab!"));
}

fn are_equal(a: &str, b: &str) -> bool {
    a == b
}
  ```
</details>

#### Shared ownership with mutability

```rust,editable,compile_fail
// Complete the update_value function.

use core::fmt::Debug;
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let owner1 = Rc::new(RefCell::new("Harry"));
    print_value(&owner1);
    let owner2 = Rc::clone(&owner1);
    update_value(&owner2, "Ron");
    print_value(&owner1);
    print_value(&owner2);
}

fn update_value<T>(owner: &Rc<RefCell<T>>, value: T) {}

fn print_value<T: Debug>(owner: &Rc<RefCell<T>>) {
    println!("{:?}", owner.borrow());
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use core::fmt::Debug;
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let owner1 = Rc::new(RefCell::new("Harry"));
    print_value(&owner1);
    let owner2 = Rc::clone(&owner1);
    update_value(&owner2, "Ron");
    print_value(&owner1);
    print_value(&owner2);
}

fn update_value<T>(owner: &Rc<RefCell<T>>, value: T) {
    *owner.borrow_mut() = value;
}

fn print_value<T: Debug>(owner: &Rc<RefCell<T>>) {
    println!("{:?}", owner.borrow());
}
  ```
</details>
