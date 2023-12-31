# Calling Unsafe Functions

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/9E2v8pCUc48?si=7I-8tqRcdP8ymJlm&amp;start=346" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Unsafe functions

```rust,editable,compile_fail
// Fix the code to make it compile. You can only add and not remove anything from the code.

fn increment(a: *mut i32) {
    *a += 1;
}

fn get_val(a: *const i32) -> i32 {
    *a
}

fn main() {
    let mut x = 0;
    let ptr1 = &mut x as *mut i32;
    unsafe {
        increment(ptr1);
        increment(ptr1);
        increment(ptr1);
        assert_eq!(get_val(ptr1), 3);
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
unsafe fn increment(a: *mut i32) {
    *a += 1;
}

unsafe fn get_val(a: *const i32) -> i32 {
    *a
}

fn main() {
    let mut x = 0;
    let ptr1 = &mut x as *mut i32;
    unsafe {
        increment(ptr1);
        increment(ptr1);
        increment(ptr1);
        assert_eq!(get_val(ptr1), 3);
    }
}
  ```
</details>

#### Unsafe methods

```rust,editable,compile_fail
// Something is missing from the method signatures. Complete them wherever required.

use std::ops::{Add, Mul, Sub};

struct VarManipulator<T>(*mut T)
where
    T: Copy + Add<Output = T> + Mul<Output = T> + Sub<Output = T>;

impl<T> VarManipulator<T>
where
    T: Copy + Add<Output = T> + Mul<Output = T> + Sub<Output = T>,
{
    fn new(ptr: *mut T) -> Self {
        Self(ptr)
    }
    fn add(&self, operand2: T) {
        *self.0 = *self.0 + operand2;
    }
    fn mul(&self, operand2: T) {
        *self.0 = *self.0 * operand2;
    }
    fn sub(&self, operand2: T) {
        *self.0 = *self.0 - operand2;
    }
    fn get_val(&self) -> T {
        *self.0
    }
}

fn main() {
    let mut x = 20;
    let manipulator = VarManipulator::new(&mut x);
    unsafe {
        manipulator.sub(10);
        manipulator.mul(9);
        manipulator.add(10);
        assert_eq!(manipulator.get_val(), 100);
        assert_eq!(x, manipulator.get_val());
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::ops::{Add, Mul, Sub};

struct VarManipulator<T>(*mut T)
where
    T: Copy + Add<Output = T> + Mul<Output = T> + Sub<Output = T>;

impl<T> VarManipulator<T>
where
    T: Copy + Add<Output = T> + Mul<Output = T> + Sub<Output = T>,
{
    fn new(ptr: *mut T) -> Self {
        Self(ptr)
    }
    unsafe fn add(&self, operand2: T) {
        *self.0 = *self.0 + operand2;
    }
    unsafe fn mul(&self, operand2: T) {
        *self.0 = *self.0 * operand2;
    }
    unsafe fn sub(&self, operand2: T) {
        *self.0 = *self.0 - operand2;
    }
    unsafe fn get_val(&self) -> T {
        *self.0
    }
}

fn main() {
    let mut x = 20;
    let manipulator = VarManipulator::new(&mut x);
    unsafe {
        manipulator.sub(10);
        manipulator.mul(9);
        manipulator.add(10);
        assert_eq!(manipulator.get_val(), 100);
        assert_eq!(x, manipulator.get_val());
    }
}
  ```
</details>
