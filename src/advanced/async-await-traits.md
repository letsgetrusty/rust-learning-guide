# async/.await

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/K8LNPYNvT-U?si=dOvFQfUFoGI0YJLJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/dOzrO40jgbU" target="_blank">Starter guide to writing async Rust</a>
- <a href="https://youtu.be/ThjvMReOXYM" target="_blank">Crust of Rust: async/await</a>

## Exercises

#### Awaiting

```rust,editable,compile_fail
// When executing the below code nothing is printed to console. Can you guess what is missing?

#[tokio::main]
async fn main() {
    my_function();
}

async fn my_function() {
    println!("My first asynchronous function in rust!");
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
#[tokio::main]
async fn main() {
    my_function().await;
}

async fn my_function() {
    println!("My first asynchronous function in rust!");
}
  ```
</details>

#### Creating executor

```rust,editable,compile_fail
// Fix the code to make it compile. You may only add code, not remove it.

use std::time::Duration;
use tokio::time::sleep;

struct Employee {
    id: u32,
    name: String,
    salary: f32,
}

impl Employee {
    fn new(id: u32, name: &str, salary: f32) -> Self {
        Self {
            id,
            name: name.to_string(),
            salary,
        }
    }
}

fn main() {
    let (id1, id2) = (2, 3);
    let emp1 = read_details_from_db(id1).await.unwrap();
    let emp2 = read_details_from_db(id2).await.unwrap();
    if emp1.salary > emp2.salary {
        println!(
            "{} earns ${} more than {}",
            emp1.name,
            emp1.salary - emp2.salary,
            emp2.name
        );
    } else if emp2.salary > emp1.salary {
        println!(
            "{} earns ${} more than {}",
            emp2.name,
            emp2.salary - emp1.salary,
            emp1.name
        );
    } else {
        println!("Both {} and {} earn same amount", emp1.name, emp2.name);
    }
}

async fn read_details_from_db(id: u32) -> Result<Employee, String> {
    // dummy read from database
    sleep(Duration::from_millis(1000)).await;
    let database = [
        Employee::new(1, "Alice", 98000.0),
        Employee::new(2, "Bob", 95000.0),
        Employee::new(3, "Cindy", 95000.0),
    ];
    for emp in database {
        if id == emp.id {
            return Ok(emp);
        }
    }
    Err("Employee record not present".into())
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::time::Duration;
use tokio::time::sleep;

struct Employee {
    id: u32,
    name: String,
    salary: f32,
}

impl Employee {
    fn new(id: u32, name: &str, salary: f32) -> Self {
        Self {
            id,
            name: name.to_string(),
            salary,
        }
    }
}

#[tokio::main]
async fn main() {
    let (id1, id2) = (2, 3);
    let emp1 = read_details_from_db(id1).await.unwrap();
    let emp2 = read_details_from_db(id2).await.unwrap();
    if emp1.salary > emp2.salary {
        println!(
            "{} earns ${} more than {}",
            emp1.name,
            emp1.salary - emp2.salary,
            emp2.name
        );
    } else if emp2.salary > emp1.salary {
        println!(
            "{} earns ${} more than {}",
            emp2.name,
            emp2.salary - emp1.salary,
            emp1.name
        );
    } else {
        println!("Both {} and {} earn same amount", emp1.name, emp2.name);
    }
}

async fn read_details_from_db(id: u32) -> Result<Employee, String> {
    // dummy read from database
    sleep(Duration::from_millis(1000)).await;
    let database = [
        Employee::new(1, "Alice", 98000.0),
        Employee::new(2, "Bob", 95000.0),
        Employee::new(3, "Cindy", 95000.0),
    ];
    for emp in database {
        if id == emp.id {
            return Ok(emp);
        }
    }
    Err("Employee record not present".into())
}
  ```
</details>
