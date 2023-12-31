# Traits

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/T0Xfltu4h3A?si=Gik7TZgsm491312J&amp;start=10" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

#### Additional Resources
- <a href="https://youtu.be/m_phdVlkr6U" target="_blank">Class Based OOP vs Traits</a>


## Exercises

#### Implementing traits 1

```rust,editable,compile_fail
// Complete the code by addressing the TODO.

trait AppendBar {
    fn append_bar(self) -> Self;
}

impl AppendBar for String {
    // TODO: Implement `AppendBar` for type `String`.
}

fn main() {
    let s = String::from("Foo");
    let s = s.append_bar();
    println!("s: {}", s);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_foo_bar() {
        assert_eq!(String::from("Foo").append_bar(), String::from("FooBar"));
    }

    #[test]
    fn is_bar_bar() {
        assert_eq!(
            String::from("").append_bar().append_bar(),
            String::from("BarBar")
        );
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
trait AppendBar {
    fn append_bar(self) -> Self;
}

impl AppendBar for String {
    fn append_bar(self) -> Self {
        format!("{self}Bar")
    }
}

fn main() {
    let s = String::from("Foo");
    let s = s.append_bar();
    println!("s: {}", s);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_foo_bar() {
        assert_eq!(String::from("Foo").append_bar(), String::from("FooBar"));
    }

    #[test]
    fn is_bar_bar() {
        assert_eq!(
            String::from("").append_bar().append_bar(),
            String::from("BarBar")
        );
    }
}
  ```
</details>

#### Implementing traits 2


```rust,editable,compile_fail
// Complete the code by addressing the TODO.

trait AppendBar {
    fn append_bar(self) -> Self;
}

// TODO: Implement trait `AppendBar` for a vector of strings.

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_vec_pop_eq_bar() {
        let mut foo = vec![String::from("Foo")].append_bar();
        assert_eq!(foo.pop().unwrap(), String::from("Bar"));
        assert_eq!(foo.pop().unwrap(), String::from("Foo"));
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
trait AppendBar {
    fn append_bar(self) -> Self;
}

impl AppendBar for Vec<String> {
    fn append_bar(self) -> Self {
        let mut res = self;
        res.push(String::from("Bar"));
        res
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_vec_pop_eq_bar() {
        let mut foo = vec![String::from("Foo")].append_bar();
        assert_eq!(foo.pop().unwrap(), String::from("Bar"));
        assert_eq!(foo.pop().unwrap(), String::from("Foo"));
    }
}
  ```
</details>

#### Default implementations


```rust,editable,compile_fail
// Fix this code by updating the Licensed trait.

pub trait Licensed {
    fn licensing_info(&self) -> String;
}

struct SomeSoftware {
    version_number: i32,
}

struct OtherSoftware {
    version_number: String,
}

impl Licensed for SomeSoftware {} // Don't edit this line
impl Licensed for OtherSoftware {} // Don't edit this line

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_licensing_info_the_same() {
        let licensing_info = String::from("Some information");
        let some_software = SomeSoftware { version_number: 1 };
        let other_software = OtherSoftware {
            version_number: "v2.0.0".to_string(),
        };
        assert_eq!(some_software.licensing_info(), licensing_info);
        assert_eq!(other_software.licensing_info(), licensing_info);
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
pub trait Licensed {
    fn licensing_info(&self) -> String {
        format!("Some information")
    }
}

struct SomeSoftware {
    version_number: i32,
}

struct OtherSoftware {
    version_number: String,
}

impl Licensed for SomeSoftware {}
impl Licensed for OtherSoftware {}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_licensing_info_the_same() {
        let licensing_info = String::from("Some information");
        let some_software = SomeSoftware { version_number: 1 };
        let other_software = OtherSoftware {
            version_number: "v2.0.0".to_string(),
        };
        assert_eq!(some_software.licensing_info(), licensing_info);
        assert_eq!(other_software.licensing_info(), licensing_info);
    }
}
  ```
</details>

#### Overriding


```rust,editable,compile_fail
// Make the code execute successfully by correctly implementing Message trait for Cat type.

trait Message {
    fn message(&self) -> String {
        "Default Message!".to_string()
    }
}

struct Fish;
struct Cat;
impl Message for Fish {}
impl Message for Cat {}

fn main() {
    let fish = Fish;
    let cat = Cat;
    assert_eq!(String::from("Default Message!"), fish.message());
    assert_eq!(String::from("Meow 🐱"), cat.message());
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
trait Message {
    fn message(&self) -> String {
        "Default Message!".to_string()
    }
}

struct Fish;
struct Cat;
impl Message for Fish {}
impl Message for Cat {
    fn message(&self) -> String {
        "Meow 🐱".to_string()
    }
}

fn main() {
    let fish = Fish;
    let cat = Cat;
    assert_eq!(String::from("Default Message!"), fish.message());
    assert_eq!(String::from("Meow 🐱"), cat.message());
}
  ```
</details>
