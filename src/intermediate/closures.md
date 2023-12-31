# Closures

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/kZXJvLfjUS4?si=5aCXoONkVay2dqWH&amp;start=30" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Defining closures

```rust,editable,compile_fail
// Define the `double` closure & make the code execute successfully.

fn main() {
    let double = ;
    assert_eq!(double(5), 10);
    assert_eq!(double(-10), -20);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let double = |x| 2*x;
    assert_eq!(double(5), 10);
    assert_eq!(double(-10), -20);
}
  ```
</details>

#### Struct fields


```rust,editable,compile_fail
// Complete the struct definition & the implementation block.

struct BinaryOperation<T, U>
where
    T: Copy,
{
    operand1: T,
    operand2: T,
    operation: U,
}

impl<T, U> BinaryOperation<T, U>
where
    T: Copy,
{
    fn calculate(&self) -> T {}
}

fn main() {
    let multiply = BinaryOperation {
        operand1: 20,
        operand2: 6,
        operation: |a, b| a * b,
    };
    let divide = BinaryOperation {
        operand1: 22.0,
        operand2: 7.0,
        operation: |x, y| x / y,
    };
    println!(
        "{} x {} = {}",
        multiply.operand1,
        multiply.operand2,
        multiply.calculate()
    );
    println!(
        "{} / {} = {}",
        divide.operand1,
        divide.operand2,
        divide.calculate()
    );
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct BinaryOperation<T, U>
where
    T: Copy,
    U: Fn(T, T) -> T
{
    operand1: T,
    operand2: T,
    operation: U,
}

impl<T, U> BinaryOperation<T, U>
where
    T: Copy,
    U: Fn(T, T) -> T
{
    fn calculate(&self) -> T {
        (self.operation)(self.operand1, self.operand2)
    }
}

fn main() {
    let multiply = BinaryOperation {
        operand1: 20,
        operand2: 6,
        operation: |a, b| a * b,
    };
    let divide = BinaryOperation {
        operand1: 22.0,
        operand2: 7.0,
        operation: |x, y| x / y,
    };
    println!(
        "{} x {} = {}",
        multiply.operand1,
        multiply.operand2,
        multiply.calculate()
    );
    println!(
        "{} / {} = {}",
        divide.operand1,
        divide.operand2,
        divide.calculate()
    );
}
  ```
</details>

#### Mutably capturing environment


```rust,editable,compile_fail
// Something is wrong with the struct definition. Can you fix it?

struct Manipulator<T>
where
    T: Fn(),
{
    operation: T,
}

impl<T> Manipulator<T>
where
    T: Fn(),
{
    fn manipulate(&mut self) {
        (self.operation)()
    }
}
fn main() {
    let mut x = 0;
    let mut y = 100;
    let mut x_manipulator = Manipulator {
        operation: || x += 1,
    };
    let mut y_manipulator = Manipulator {
        operation: || y /= 5,
    };
    x_manipulator.manipulate();
    x_manipulator.manipulate();
    y_manipulator.manipulate();
    y_manipulator.manipulate();
    assert_eq!(x, 2);
    assert_eq!(y, 4);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct Manipulator<T>
where
    T: FnMut(),
{
    operation: T,
}

impl<T> Manipulator<T>
where
    T: FnMut(),
{
    fn manipulate(&mut self) {
        (self.operation)()
    }
}
fn main() {
    let mut x = 0;
    let mut y = 100;
    let mut x_manipulator = Manipulator {
        operation: || x += 1,
    };
    let mut y_manipulator = Manipulator {
        operation: || y /= 5,
    };
    x_manipulator.manipulate();
    x_manipulator.manipulate();
    y_manipulator.manipulate();
    y_manipulator.manipulate();
    assert_eq!(x, 2);
    assert_eq!(y, 4);
}
  ```
</details>

#### Moving into closures


```rust,editable,compile_fail
// Make the code compile by addressing the TODO.

fn main() {
    let my_str = "Hi there!".to_owned();
    let substr = "here";
    let check_substr = move |sub: &str| my_str.contains(sub);
    let res = check_substr(substr);
    // TODO: shift the statement below to somewhere else
    println!("String: {my_str}");
    println!("String contains {substr} : {res}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let my_str = "Hi there!".to_owned();
    let substr = "here";
    println!("String: {my_str}");
    let check_substr = move |sub: &str| my_str.contains(sub);
    let res = check_substr(substr);
    println!("String contains {substr} : {res}");
}
  ```
</details>

#### Passing to functions


```rust,editable,compile_fail
// Complete the function signature.

fn average<T, U>(nums: &[i32], add: T, div: U) -> f32
where
{
    let mut sum = 0;
    for num in nums {
        sum = add(sum, *num);
    }
    div(sum, nums.len() as i32)
}

fn main() {
    let add = |a, b| a + b;
    let div = |a, b| a as f32 / b as f32;
    let my_nums = [1, 2, 3, 4, 5];
    let res = average(&my_nums, add, div);
    println!("Average of {my_nums:?} = {res}");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn average<T, U>(nums: &[i32], add: T, div: U) -> f32
where T: Fn(i32, i32) -> i32,
      U: Fn(i32, i32) -> f32,
{
    let mut sum = 0;
    for num in nums {
        sum = add(sum, *num);
    }
    div(sum, nums.len() as i32)
}

fn main() {
    let add = |a, b| a + b;
    let div = |a, b| a as f32 / b as f32;
    let my_nums = [1, 2, 3, 4, 5];
    let res = average(&my_nums, add, div);
    println!("Average of {my_nums:?} = {res}");
}
  ```
</details>

#### Returning from functions 1


```rust,editable,compile_fail
// Fix the code to make it compile.

fn main() {
    let adder1 = get_adder(-2);
    let adder2 = get_adder(100);
    assert_eq!(adder1(20), 18);
    assert_eq!(adder2(10), 110);
}

fn get_adder(to_add: i32) -> impl Fn(i32) -> i32 {
    |x| x + to_add
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let adder1 = get_adder(-2);
    let adder2 = get_adder(100);
    assert_eq!(adder1(20), 18);
    assert_eq!(adder2(10), 110);
}

fn get_adder(to_add: i32) -> impl Fn(i32) -> i32 {
    move |x| x + to_add
}
  ```
</details>

#### Returning to functions 2


```rust,editable,compile_fail
// Fix the code to make it compile.

enum Operation {
    Add,
    Sub,
    Mul,
}

fn get_implementation(operation: Operation, operand2: i32) -> impl Fn(i32) -> i32 {
    match operation {
        Operation::Add => move |x| x + operand2,
        Operation::Sub => move |x| x - operand2,
        Operation::Mul => move |x| x * operand2,
    }
}

fn main() {
    const OPERAND2: i32 = 5;
    let adder = get_implementation(Operation::Add, OPERAND2);
    let multiplier = get_implementation(Operation::Mul, OPERAND2);
    assert_eq!(adder(10), 15);
    assert_eq!(multiplier(10), 50);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
enum Operation {
    Add,
    Sub,
    Mul,
}

fn get_implementation(operation: Operation, operand2: i32) -> Box<dyn Fn(i32) -> i32> {
    match operation {
        Operation::Add => Box::new(move |x| x + operand2),
        Operation::Sub => Box::new(move |x| x - operand2),
        Operation::Mul => Box::new(move |x| x * operand2),
    }
}

fn main() {
    const OPERAND2: i32 = 5;
    let adder = get_implementation(Operation::Add, OPERAND2);
    let multiplier = get_implementation(Operation::Mul, OPERAND2);
    assert_eq!(adder(10), 15);
    assert_eq!(multiplier(10), 50);
}
  ```
</details>
