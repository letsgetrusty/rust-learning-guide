# Data Types

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/2V0JaMVjzws?si=VMyGj59DPis4ak8X&amp;start=232" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Boolean

```rust,editable,compile_fail
// Something's missing. Fix the code so that it compiles.

fn main() {
    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    let // Finish the rest of this line
    if is_evening {
        println!("Good evening!");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    let is_evening = true; // can also assign false
    if is_evening {
        println!("Good evening!");
    }
}
  ```
</details>

#### Unsigned int

```rust,editable,compile_fail
// Do you really have that few friends?
// Assign the correct value to the variable.

fn main() {
    let number_of_friends: u32; // Don't change this line!
    number_of_friends = -1;
    println!("I have {} friends!", number_of_friends);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let number_of_friends: u32;
    number_of_friends = 10; // any integer from 0 to 4294967295 is valid
    println!("I have {} friends!", number_of_friends);
}

  ```
</details>

#### Signed int

```rust,editable,compile_fail
// Make this program compile by replacing the variable type.

fn main() {
    let number_of_stars: i32;
    // The Milky Way has more stars than can fit in a 32-bit integer type!
    number_of_stars = 400_000_000_000;
    println!("There are about {} stars in the Milky Way galaxy!", number_of_stars);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let number_of_stars: i64;
    number_of_stars = 400_000_000_000;
    println!("There are about {} stars in the Milky Way galaxy!", number_of_stars);
}
```
</details>

#### Floating point numbers

```rust,editable,compile_fail
// Assign the correct data types to the variables.

fn main() {
    let pi2: f32;
    pi2 = 3.14;
    println!("Pi is {}, correct to 2 decimal places.", pi2);

    // What if we want to be more precise with our floating-point numbers?

    let pi15: /* Give this variable a data type! */;
    pi15 = 3.141592653589793;
    println!("Pi is {}, correct to 15 decimal places.", pi15);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let pi2: f32;
    pi2 = 3.14;
    println!("Pi is {}, correct to 2 decimal places.", pi2);

    let pi15: f64;
    pi15 = 3.141592653589793;
    println!("Pi is {}, correct to 15 decimal places.", pi15);
}
```
</details>

#### char

```rust,editable,compile_fail
// Fill in the rest of the line that has code missing!

fn main() {
    let my_first_initial = 'C';

    if my_first_initial.is_alphabetic() {
        println!("Alphabetical!");
    } else if my_first_initial.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }

    let // Finish this line like the example! What's your favorite character?
    // Try a letter, try a number, try a special character, try a character
    // from a different language than your own, try an emoji!

    if your_character.is_alphabetic() {
        println!("Alphabetical!");
    } else if your_character.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let my_first_initial = 'C';

    if my_first_initial.is_alphabetic() {
        println!("Alphabetical!");
    } else if my_first_initial.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }

    let your_character = '😃';

    if your_character.is_alphabetic() {
        println!("Alphabetical!");
    } else if your_character.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
}
```
</details>

#### String types

```rust,editable,compile_fail
// Make this program compile without changing the variable type!

fn main() {
    let answer: String = /* Your favorite color here */;
    println!("My current favorite color is {}", answer);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let answer: String = "black".to_string();
    println!("My current favorite color is {}", answer);
}

```
</details>


#### Arrays

```rust,editable,compile_fail
// Create an array with at least 10 elements in it.

fn main() {
    let a = /* Your array here */;

    if a.len() >= 10 {
        println!("Wow, that's a big array!");
    } 
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let a = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1];
    
    if a.len() >= 10 {
        println!("Wow, that's a big array!");
    } 
}
```
</details>


#### Tuples

```rust,editable,compile_fail
// Destructure the `cat` tuple so that the println will work.

fn main() {
    let cat = ("Furry McFurson", 3.5);
    let /* Your pattern here */ = cat;

    println!("{} is {} years old.", name, age);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let cat = ("Furry McFurson", 3.5);
    let (name, age) = cat;

    println!("{} is {} years old.", name, age);
}
```
</details>


#### Type aliasing

```rust,editable,compile_fail
// Add a type alias for our dogs so we can store their names and ages.

fn main() {
    type /* Finish this line */

    let dog1: Dog = (String::from("Albert"), 3);
    println!("My dog {} is {} years old.", dog1.0, dog1.1);

    let dog2: Dog = (String::from("Barker"), 5);
    println!("My other dog {} is {} years old.", dog2.0, dog2.1);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    type Dog = (String, u8);

    let dog1: Dog = (String::from("Albert"), 3);
    println!("My dog {} is {} years old.", dog1.0, dog1.1);

    let dog2: Dog = (String::from("Barker"), 5);
    println!("My other dog {} is {} years old.", dog2.0, dog2.1);
}
```
</details>
