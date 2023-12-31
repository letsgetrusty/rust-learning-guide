# Lifetime Elision

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/juIINGuZyBc?si=bcFgFaK0egQUHRg7&amp;start=808" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Lifetime elision

```rust,editable,compile_fail
// The code below executes successfully. However, remove the lifetimes from wherever they can be inferred implicitly.

struct NameStack<'a> {
    names: Vec<&'a str>,
}

impl<'a> NameStack<'a> {
    fn new() -> Self {
        NameStack { names: Vec::new() }
    }
    fn add_name(&mut self, name: &'a str) {
        self.names.push(name);
    }
    fn remove_name_with_substr<'b>(&mut self, sub_str: &'b str) -> &'a str {
        for i in 0..self.names.len() {
            if self.names[i].contains(sub_str) {
                let removed = self.names.remove(i);
                return removed;
            }
        }
        panic!("Name with substring not found");
    }
}

fn main() {
    let mut my_names = NameStack::new();
    my_names.add_name("Alice");
    my_names.add_name("Bob");
    my_names.add_name("Cindy");
    my_names.add_name("Emily");
    let removed = my_names.remove_name_with_substr("ice");
    println!("Removed: {removed}");
    assert_eq!(my_names.names.len(), 3);
}

```

<details>
  <summary>Solution</summary>
  
  ```rust
struct NameStack<'a> {
    names: Vec<&'a str>,
}

impl<'a> NameStack<'a> {
    fn new() -> Self {
        NameStack { names: Vec::new() }
    }
    fn add_name(&mut self, name: &'a str) {
        self.names.push(name);
    }
    fn remove_name_with_substr(&mut self, sub_str: &str) -> &str {
        for i in 0..self.names.len() {
            if self.names[i].contains(sub_str) {
                let removed = self.names.remove(i);
                return removed;
            }
        }
        panic!("Name with substring not found");
    }
}

fn main() {
    let mut my_names = NameStack::new();
    my_names.add_name("Alice");
    my_names.add_name("Bob");
    my_names.add_name("Cindy");
    my_names.add_name("Emily");
    let removed = my_names.remove_name_with_substr("ice");
    println!("Removed: {removed}");
    assert_eq!(my_names.names.len(), 3);
}
  ```
</details>
