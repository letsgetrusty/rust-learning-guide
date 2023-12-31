# Option

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/DSZqIJhkNCM?si=Ofl7m__k_-Gn_1vW&amp;start=256" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Matching option 1


```rust,editable,compile_fail
// Fix the code so that it compiles.

struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match y {
        Some(p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => println!("no match"),
    }
    y; // Fix without deleting this line.
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match &y {
        Some(p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => println!("no match"),
    }
    y; // Fix without deleting this line.
}
  ```
</details>

#### Matching option 2


```rust,editable,compile_fail
// Fix the code so that it compiles.

fn last_element(nums: &[i32]) -> Option<i32> {
    if nums.len() > 0 {
        Some(nums[nums.len() - 1])
    } else {
        None
    }
}

fn main() {
    let my_nums = [1, 1, 2, 3, 5, 8, 13];
    match last_element(&my_nums) {
        Some => println!("Last element: {ele}"),
        None => println!("Empty array"),
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn last_element(nums: &[i32]) -> Option<i32> {
    if nums.len() > 0 {
        Some(nums[nums.len() - 1])
    } else {
        None
    }
}

fn main() {
    let my_nums = [1, 1, 2, 3, 5, 8, 13];
    match last_element(&my_nums) {
        Some(ele) => println!("Last element: {ele}"),
        None => println!("Empty array"),
    }
}
  ```
</details>

#### If let

```rust,editable,compile_fail
// Fix the code so that it compiles.

struct User {
    id: u32,
    name: String,
}

fn get_user_name(id: u32) -> Option<String> {
    let database = [
        User {id: 1, name: String::from("Alice")},
        User {id: 2, name: String::from("Bob")},
        User {id: 3, name: String::from("Cindy")}
    ];
    for user in database {
        if user.id == id {
            return Some(user.name)
        }
    }
    None
}

fn main() {
    let user_id = 3;
    if Some(name) == get_user_name(user_id) {
        println!("User's name: {name}");
    }
}
```
<details>
  <summary>Solution</summary>
  
  ```rust
struct User {
    id: u32,
    name: String,
}

fn get_user_name(id: u32) -> Option<String> {
    let database = [
        User {id: 1, name: String::from("Alice")},
        User {id: 2, name: String::from("Bob")},
        User {id: 3, name: String::from("Cindy")}
    ];
    for user in database {
        if user.id == id {
            return Some(user.name)
        }
    }
    None
}

fn main() {
    let user_id = 3;
    if let Some(name) = get_user_name(user_id) {
        println!("User's name: {name}");
    }
}
  ```
</details>
