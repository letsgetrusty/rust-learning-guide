# Tokio Tasks

> Note: No official YouTube video yet, but our <a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">Rust Developer Bootcamp</a> covers this topic!

<a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">
    <img src="https://d1aettbyeyfilo.cloudfront.net/letsgetrusty/31007320_1703634176QCJNo_video_yet.png" alt="No video yet..."/>
</a>

## Exercises

#### Awaiting tasks

```rust,editable,compile_fail
// Fix the code to make it compile.

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
    let ids = [1, 2, 4, 5, 9, 10];
    let mut handles = Vec::new();
    for id in ids {
        let handle = tokio::spawn(async move {
            let res = print_details(id).await;
            if let Err(e) = res {
                println!("{e}");
            }
        });
        handles.push(handle);
    }
    for handle in handles {
        handle.join().unwrap();
    }
}

async fn print_details(id: u32) -> Result<(), String> {
    let emp = read_details_from_db(id).await?;
    println!("Id: {}, Name: {}, Salary: {}", emp.id, emp.name, emp.salary);
    Ok(())
}

async fn read_details_from_db(id: u32) -> Result<Employee, String> {
    // dummy read from database
    sleep(Duration::from_millis(1000)).await;
    let database = [
        Employee::new(1, "Alice", 98000.0),
        Employee::new(2, "Bob", 95000.0),
        Employee::new(3, "Cindy", 95000.0),
        Employee::new(4, "Daniel", 88000.0),
    ];
    for emp in database {
        if id == emp.id {
            return Ok(emp);
        }
    }
    Err(format!("Employee record for id {} not present", id))
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
// Playground: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=270cc37d8222f963cedfb137f0472584

// Fix the code to make it compile.

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
    let ids = [1, 2, 4, 5, 9, 10];
    let mut handles = Vec::new();
    for id in ids {
        let handle = tokio::spawn(async move {
            let res = print_details(id).await;
            if let Err(e) = res {
                println!("{e}");
            }
        });
        handles.push(handle);
    }
    for handle in handles {
        handle.await.unwrap(); // use await here instead of join()
    }
}

async fn print_details(id: u32) -> Result<(), String> {
    let emp = read_details_from_db(id).await?;
    println!("Id: {}, Name: {}, Salary: {}", emp.id, emp.name, emp.salary);
    Ok(())
}

async fn read_details_from_db(id: u32) -> Result<Employee, String> {
    // dummy read from database
    sleep(Duration::from_millis(1000)).await;
    let database = [
        Employee::new(1, "Alice", 98000.0),
        Employee::new(2, "Bob", 95000.0),
        Employee::new(3, "Cindy", 95000.0),
        Employee::new(4, "Daniel", 88000.0),
    ];
    for emp in database {
        if id == emp.id {
            return Ok(emp);
        }
    }
    Err(format!("Employee record for id {} not present", id))
}
  ```
</details>
