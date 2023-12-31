# Structs

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/n3bPhdiJm9I?si=ki2TQE0PTsMInCEH&amp;start=20" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Struct definition

```rust,editable,compile_fail
// Complete the code by addressing the TODO.

struct User {
    // TODO: Something goes here
}

fn main() {
    let user = User {
        name: String::from("Tom Riddle"),
        age: 17u8,
    };
    println!("User's name: {}", user.name);
    println!("User's age: {}", user.age);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct User {
    name: String,
    age: u8,
}

fn main() {
    let user = User {
        name: String::from("Tom Riddle"),
        age: 17u8,
    };
    println!("User's name: {}", user.name);
    println!("User's age: {}", user.age);
}
  ```
</details>

#### Mutating structs

```rust,editable,compile_fail
// Make the following code compile.

struct ShopItem {
    name: String,
    quantity: u32,
    in_stock: bool,
}

fn main() {
    let item = ShopItem {
        name: String::from("Socks"),
        quantity: 200,
        in_stock: true,
    };
    // 50 pairs of socks were sold
    item.quantity -= 50;
    if item.quantity == 0 {
        item.in_stock = false;
    }
    println!("{} is in stock: {}", item.name, item.in_stock);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct ShopItem {
    name: String,
    quantity: u32,
    in_stock: bool,
}

fn main() {
    let mut item = ShopItem {
        name: String::from("Socks"),
        quantity: 200,
        in_stock: true,
    };
    // 50 pairs of socks were sold
    item.quantity -= 50;
    if item.quantity == 0 {
        item.in_stock = false;
    }
    println!("{} is in stock: {}", item.name, item.in_stock);
}
  ```
</details>

#### Structs and Functions


```rust,editable,compile_fail
// Complete the function signatures and make the code compile.

struct ShopItem {
    name: String,
    quantity: u32,
}

fn main() {
    let item = create_item("Socks", 200);
    let in_stock = is_in_stock(&item);
    println!("{} is in stock: {in_stock}", item.name);
}

fn create_item(name: &str, quantity: u32) -> {
    ShopItem {
        name: name.to_string(),
        quantity, // notice how struct initializations can be shortened when variable and field have same name
    }
}

fn is_in_stock(item) -> bool {
    item.quantity > 0
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
struct ShopItem {
    name: String,
    quantity: u32,
}

fn main() {
    let item = create_item("Socks", 200);
    let in_stock = is_in_stock(&item);
    println!("{} is in stock: {in_stock}", item.name);
}

fn create_item(name: &str, quantity: u32) -> ShopItem {
    ShopItem {
        name: name.to_string(),
        quantity,
    }
}

fn is_in_stock(item: &ShopItem) -> bool {
    item.quantity > 0
}
  ```
</details>
