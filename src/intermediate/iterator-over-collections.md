# Iterator Over Collections

> Note: No official YouTube video yet, but our <a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">Rust Developer Bootcamp</a> covers this topic!

<a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">
    <img src="https://d1aettbyeyfilo.cloudfront.net/letsgetrusty/31007320_1703634176QCJNo_video_yet.png" alt="No video yet..."/>
</a>

#### Additional Resources
- <a href="https://youtu.be/qY9j4dRaMjU" target="_blank">Make iterators 10X better with itertools</a>

## Exercises

#### Iterating immutably

```rust,editable,compile_fail
//  Make the code compile by filling in the `???`s

fn main() {
    let my_fav_fruits = vec!["banana", "custard apple", "avocado", "peach", "raspberry"];

    let mut my_iterable_fav_fruits = ???;   // TODO: Step 1

    assert_eq!(my_iterable_fav_fruits.next(), Some(&"banana"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 2
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"avocado"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 3
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"raspberry"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 4
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let my_fav_fruits = vec!["banana", "custard apple", "avocado", "peach", "raspberry"];

    let mut my_iterable_fav_fruits = my_fav_fruits.iter();

    assert_eq!(my_iterable_fav_fruits.next(), Some(&"banana"));
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"custard apple"));
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"avocado"));
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"peach"));
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"raspberry"));
    assert_eq!(my_iterable_fav_fruits.next(), None);
}
  ```
</details>

#### Iterating mutably

```rust,editable,compile_fail
// Make the code compile by only modifying the loop.

fn main() {
    let mut nums = [0, 1, 2, 3, 4];
    let odd_nums = [1, 3, 5, 7, 9];
    for num in nums.iter() {
        *num = 2 * *num + 1;
    }
    assert_eq!(nums, odd_nums)
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn main() {
    let mut nums = [0, 1, 2, 3, 4];
    let odd_nums = [1, 3, 5, 7, 9];
    for num in nums.iter_mut() {
        *num = 2 * *num + 1;
    }
    assert_eq!(nums, odd_nums)
}
  ```
</details>

#### Hashmaps

```rust,editable,compile_fail
// Fix the code to make it compile.

use std::collections::HashMap;

fn main() {
    // marks scored out of 50
    let mut marks = HashMap::from([("Harry", 40.0), ("Hermoine", 50.0), ("Ron", 35.5)]);
    // convert marks into percentage
    for (_, marks) in marks {
        *marks = (*marks * 100.0) / 50.0;
    }
    marks.for_each(|(student, marks)| println!("{student} scored {marks}%"));
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::collections::HashMap;

fn main() {
    // marks scored out of 50
    let mut marks = HashMap::from([("Harry", 40.0), ("Hermoine", 50.0), ("Ron", 35.5)]);
    // convert marks into percentage
    for (_, marks) in marks.iter_mut() {
        *marks = (*marks * 100.0) / 50.0;
    }
    marks.iter().for_each(|(student, marks)| println!("{student} scored {marks}%"));
}
  ```
</details>
